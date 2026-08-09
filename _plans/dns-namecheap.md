# DNS runbook — mileszs.com at Namecheap

Written 2026-08-08, during an outage. Not published (Jekyll ignores `_`-prefixed
directories).

## What happened

Nameserver delegation for `mileszs.com` had already moved from Dyn to Namecheap
(`dns1`/`dns2.registrar-servers.com`), but the Namecheap zone was never
populated with the web records. It contained only Namecheap's own defaults:
email-forwarding MX plus a matching SPF.

Result: `www.mileszs.com` returned **NXDOMAIN** and the site was offline. The
old records survived only in the abandoned Dyn zone, which is still queryable
but no longer authoritative — nothing on the internet consults it.

GitHub Pages was healthy throughout (`status: built`, Let's Encrypt certificate
valid to 2026-09-24). This was purely DNS.

## The old Dyn zone, captured before it disappears

The only surviving record of the pre-migration configuration:

| Type | Host | Value |
|---|---|---|
| A | `@` | `130.61.69.238` — Dyn's HTTP redirect service. **Dead. Do not recreate.** |
| CNAME | `www` | `mileszs.github.io.` |
| A | `music` | `204.62.14.137` |
| MX | `@` | Google Workspace, 7 records (below) |
| TXT / DMARC / DKIM / CAA | — | none existed |

## Target zone

Namecheap → Domain List → **Manage** → **Advanced DNS**.

### 1. Website — fixes the outage

| Type | Host | Value | TTL |
|---|---|---|---|
| CNAME | `www` | `mileszs.github.io.` | Automatic |
| A | `@` | `185.199.108.153` | Automatic |
| A | `@` | `185.199.109.153` | Automatic |
| A | `@` | `185.199.110.153` | Automatic |
| A | `@` | `185.199.111.153` | Automatic |
| AAAA | `@` | `2606:50c0:8000::153` | Automatic |
| AAAA | `@` | `2606:50c0:8001::153` | Automatic |
| AAAA | `@` | `2606:50c0:8002::153` | Automatic |
| AAAA | `@` | `2606:50c0:8003::153` | Automatic |

The `www` CNAME alone restores the site. The apex A/AAAA records are what make
`https://mileszs.com` work — it never did before, even on Dyn, because Dyn's
redirect service had no TLS. With these, GitHub serves the apex with a real
certificate and redirects to `www` itself, using the `CNAME` file in the repo.

**Delete Namecheap's default parking / URL Redirect record on `@` first**, or
the A records will not save.

### 2. Email — Google Workspace

**Before touching MX:** in the domain's **Mail Settings**, switch from *Email
Forwarding* to **Custom MX**. If you skip this, Namecheap re-adds its
`eforward*.registrar-servers.com` records and silently overrides yours.

| Type | Host | Priority | Value |
|---|---|---|---|
| MX | `@` | 1 | `aspmx.l.google.com.` |
| MX | `@` | 5 | `alt1.aspmx.l.google.com.` |
| MX | `@` | 5 | `alt2.aspmx.l.google.com.` |
| MX | `@` | 10 | `aspmx2.googlemail.com.` |
| MX | `@` | 10 | `aspmx3.googlemail.com.` |
| MX | `@` | 10 | `aspmx4.googlemail.com.` |
| MX | `@` | 10 | `aspmx5.googlemail.com.` |

This restores exactly what Dyn served. Google also supports a modern
single-record setup — one MX, priority 1, `smtp.google.com.` — which is
equivalent and easier to maintain. Either is fine; the 7-record set is a
known-good restore of prior state.

### 3. SPF — must be replaced, not kept

The zone currently carries Namecheap's forwarding SPF:

```
v=spf1 include:spf.efwd.registrar-servers.com ~all
```

That authorizes Namecheap's forwarders and **not** Google, so mail sent from
Gmail soft-fails SPF. Replace with:

| Type | Host | Value |
|---|---|---|
| TXT | `@` | `v=spf1 include:_spf.google.com ~all` |

Exactly one SPF record per domain — edit the existing one, do not add a second.

### 4. DMARC — new

| Type | Host | Value |
|---|---|---|
| TXT | `_dmarc` | `v=DMARC1; p=none; rua=mailto:miles@mileszs.com` |

`p=none` is monitoring-only: it changes nothing about delivery, it just asks
receivers to report who is sending as your domain. Tighten to `p=quarantine`
later once the reports look clean.

### 5. CAA — new

The GitHub Pages certificate is issued by **Let's Encrypt** (verified against
the live cert). CAA must permit them or renewal breaks.

| Type | Host | Flags | Tag | Value |
|---|---|---|---|---|
| CAA | `@` | 0 | `issue` | `letsencrypt.org` |
| CAA | `@` | 0 | `iodef` | `mailto:miles@mileszs.com` |

Caveat: CAA restricts *every* certificate for this domain. If you later add a
service that provisions its own certificate — Cloudflare, Fastly, a mail host —
issuance fails until its CA is added here. If that sounds like a trap, CAA is
the one hardening record that is safe to skip.

### 6. `music` subdomain

| Type | Host | Value |
|---|---|---|
| A | `music` | `204.62.14.137` |

Restored as-is from the Dyn zone. The IP has no reverse DNS and its purpose is
unrecorded.

### 7. GitHub domain verification

Prevents anyone else claiming `mileszs.com` on GitHub Pages if the DNS ever
dangles again — which is exactly what happened here.

1. GitHub → **Settings** → **Pages** → *Add a domain* → enter `mileszs.com`.
2. GitHub shows a TXT record: host `_github-pages-challenge-mileszs`, value a
   one-time token.
3. Add it at Namecheap, then click **Verify**.

The token is generated per-account and cannot be pre-computed.

## Verification

DNS first:

```bash
dig +short www.mileszs.com                    # -> mileszs.github.io. + 185.199.x.x
dig +short mileszs.com A                      # -> the four 185.199.x.x
dig +short mileszs.com MX                     # -> aspmx.l.google.com etc.
dig +short mileszs.com TXT                    # -> v=spf1 include:_spf.google.com ~all
dig +short _dmarc.mileszs.com TXT             # -> v=DMARC1; ...
```

Query `@dns1.registrar-servers.com` directly to bypass caching and see the zone
as published.

Then the site:

```bash
curl -sI https://www.mileszs.com | head -1    # -> HTTP/2 200
curl -sI https://mileszs.com | head -1        # -> 301 to www
```

Finally send yourself a message at `miles@mileszs.com` and confirm it arrives in
Google, not a forwarder.

## Gotchas, ranked by how much they hurt

1. **Mail Settings must be "Custom MX."** Left on Email Forwarding, Namecheap
   overwrites the Google MX records and mail quietly goes to the wrong place.
2. **Delete the default parking record on `@`** before adding apex A records.
3. **One SPF record only.** Two SPF TXT records is a permanent error state —
   receivers treat it as invalid rather than merging them.
4. **CAA blocks every CA you do not list.** See caveat above.
5. **The Dyn zone is not a backup.** It still answers today but is
   unauthoritative and will eventually be torn down. This file is the backup.

## Follow-ups not blocking the outage

- ~~DKIM is not configured.~~ **Wrong — DKIM is configured and valid.**
  `google._domainkey.mileszs.com` publishes a 2048-bit RSA key.

  The original claim came from a selector sweep run against
  `ns1199.dns.dyn.com` — the stale Dyn nameserver — after the delegation had
  already moved to Namecheap. The abandoned zone had no DKIM, so the sweep came
  back empty and the result was written up as "no selector found in either
  zone," which overstated a check that only ever touched one zone.

  **Lesson for any future DNS work here: query
  `dns1.registrar-servers.com`, or use a public resolver. The Dyn servers still
  answer and will confidently return stale data.**
- **Enforce HTTPS** in GitHub Pages settings is still off.
- **Resolved 2026-08-09: the apex is canonical, not `www`.** The GitHub Pages
  custom domain was set to `mileszs.com` during this migration, so live traffic
  goes `www` → 301 → apex. The repo's `CNAME` file and `_config.yml` `url:`
  were updated to match. All three must continue to name the same host or
  canonical tags advertise a URL that redirects.
