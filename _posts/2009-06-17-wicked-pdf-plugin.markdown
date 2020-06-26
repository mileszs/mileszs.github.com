---
layout:     post
title:      "Wicked PDF Plugin"
alias: /blog/2009/06/17/wicked-pdf-plugin.html
date: 2009-06-17 19:33:11
---
A bit of background, in the hopes that it will help you save some time:

At **The Job**, I _need_ PDF generation.  We want to generate various documents in PDF form that are professional looking without requiring our users print a particular web page to some PDF printer.  I'd prefer it be really easy for me to write, of course, but if it's something I can do once and never again, I can probably live with a convoluted DSL of some sort.  So, once upon a Google, my search began.

... and it was frustrating.

## Princely

I started with [ Princely ](http://github.com/mbleigh/princely/tree/master), a Rails plugin that uses the [ PrinceXML library ](http://www.princexml.com/) to generate PDFs from Rails views.  It's super-simple.  It's _almost_ everything I wanted in a PDF generation plugin.  It allows you to do all your layout and prettifying with CSS and HTML, which is sweet because -- damn -- I already know HTML and CSS.  Hell yes.

Except that it watermarks all your PDFs with a blue square containing a white 'P'.  You can get rid of it if you've got $3800.  Verdict?  **Not worth it**.

## Prawn

I moved to playing with [ Prawn ](http://github.com/sandal/prawn/tree/master), which is essentially a Ruby-ish DSL for laying out and generating PDFs.  It was a nightmare.  [ Check out this gist ](http://gist.github.com/125953) to get an idea of what I mean.  It can't be printed here.  It's too big and cumbersome.  It'll break my blog. ... Here's a taste:


```ruby
  def info_table
    tags :blue => { :color => 'blue' },
         :green => { :color => 'green' }
 
    bounding_box [235, 700], :width => 300 do
      font "Times-Roman"
      fill_color('999999')
      text "TERM SHEET", :size => 24, :align => :right
      fill_color('000000')
      font "Helvetica"
      text "<blue>Keep IT Safe,</blue> <green>Keep IT Simple</green>", :size => 12, :align => :right
    end
    bounding_box [260, 650] do
      table [[@ticket, @sales_rep, @doc_no, @project, @date]],
        :headers => ['TICKET', 'SALES REP', 'DOC. NO.', 'PROJECT NO.', 'DATE'],
        :header_color => '000000',
        :header_text_color => 'ffffff',
        :align_headers => :center,
        :vertical_padding => 2,
        :horizontal_padding => 2,
        :font_size => 8,
        :width => 272,
        :border_width => 0.2,
        :align => { 0 => :center, 1 => :center, 2 => :center, 3 => :center, 4 => :center, 5 => :center, 6 => :left }
    end
  end
```

I appreciate what the Prawn library can do.  I appreciate all the hard work that has been put into it.  It's just not for me.  *So not worth it*.

## Wicked PDF

So, with a little help from [ Elijah Miller ](http://jqr.github.com/) and the [ Indy.rb ](http://indyrb.org) IRC channel (#indyrb on Freenode), the shell utility [ wkhtmltopdf ](http://code.google.com/p/wkhtmltopdf/) landed in my lap.  It uses the WebKit engine via QT 4.4 to render HTML and CSS and convert it to PDF.  It's free.  It's exactly what I needed.  Based upon wkhtmltopdf, the [ Wicked PDF plugin ](http://github.com/mileszs/wicked_pdf/tree/master) was born to make PDF generation in your Rails app easier and cheaper than it was previously.

First of all, it's _wicked_-easy to get started.  After you've visited [ wkhtmltopdf ](http://code.google.com/p/wkhtmltopdf/) and installed wkhtmltopdf, simply:

```ruby
script/plugin install git://github.com/mileszs/wicked_pdf.git
```

In your controller:

```ruby
class ThingsController < ApplicationController
  def show
    respond_to do |format|
      format.html
      format.pdf do
        render :pdf => "file_name",
               :template => "things/show.pdf.erb",
               :layout => "pdf.html"
      end
    end
  end
end
```

You don't have to specify a template, as it will choose the template for the current controller and action by default.  Also, by default, layout is set to false, so you may want to keep that around.

I hope this helps others who have travelled the long, lonely, crufty, potentially expensive route of finding a sufficiently powerful and simple PDF generator.  If you find it lacking, or if you find a bug, please report it to [ Wicked PDF's Issues page ](http://github.com/mileszs/wicked_pdf/issues) so I can make it do right.

**Look:**

![wicked witch](/images/wicked_witch_speaks.jpg)

You _know_ the Wicked Witch likes it.
