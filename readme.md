# Grunt Email Design Workflow

Designing and testing emails is a pain. HTML tables, inline CSS, various devices and clients to test, and varying support for the latest web standards.

This grunt task helps simplify things at the design stage.

1. Compiles your SCSS to CSS

2. Builds your email templates

3. Inlines your CSS

4. Uploads any images to a CDN (optional)

5. Sends you a test email to your inbox

## Requirements

* Node.js - [Install Node.js](https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager)
* Grunt-cli and Grunt (`npm install grunt-cli -g`)
* Ruby - [Install ruby with RVM](https://rvm.io/rvm/install)
* Premailer (`gem install premailer hpricot nokogiri`) - Inlines the CSS
* [Mailgun](http://www.mailgun.com) (optional) - Sends the email
* [Litmus](https://litmus.com) (optional) - Tests the email across all clients/browsers/devices
* [Rackspace Cloud](http://www.rackspace.com/cloud/files/) (optional) - Uses Cloud Files as a CDN

## Getting started

If you haven't used [Grunt](http://gruntjs.com/) before check out Chris Coyier's post on [getting started with Grunt](http://24ways.org/2013/grunt-is-not-weird-and-hard/).

Clone this repo, cd to the directory, run `npm install` to install the necessary packages.

```
git clone https://github.com/leemunroe/grunt-email-design.git
cd grunt-email-design
npm install
grunt
```

## How it works

<img src="http://i.imgur.com/yrHpTdr.jpg" width="500">

### CSS

This project uses [SCSS](http://sass-lang.com/). You don't need to touch the .css files, these are compiled automatically.

For changes to CSS, modify the .scss files.

Media queries and responsive styles are in a separate style sheet so that they don't get inlined. Note that only a few clients support media queries e.g. iOS Mail app.

### Email templates and content

Handlebars is used for templating.

`/layouts` contains the standard header/footer HTML markup. You most likely will only need one layout template, but you can have as many as you like.

`/emails` is where your email content will go. To start you off I've included example transactional emails based on my [simple HTML email template](https://github.com/leemunroe/html-email-template).

### Generate your email templates

In terminal, run `grunt`. This will:

* Compile your SCSS to CSS
* Generate your email layout and content
* Inline your CSS

See the output HTML in the `dist` folder. Open them and preview it the browser.

Alternatively run `grunt watch`. This will check for any changes you make to your .scss and .hbs templates, then automatically run the tasks. Saves you having to run grunt every time.

### Send the email to yourself

* Sign up for a [Mailgun](http://www.mailgun.com) account (it's free)
* Open up `Gruntfile.js`
* Replace 'MAILGUN_KEY' with your actual Mailgun API key
* Change the sender and recipient to your own email address (or whoever you want to send it to)

Run `grunt send --template=transaction.html`. This will email out the template you specify.

Change 'transaction.html' to the name of the email template you want to send.

### How to test with Litmus

If you have a [Litmus](http://www.litmus.com) account and want to test the email in multiple clients/devices, create a new test in Litmus, copy the email address they tell you to send the email to, open up `Gruntfile.js` and paste it where the recipient goes. Then run `grunt send --template=TEMPLATE_NAME.html` to send the email to Litmus.

<img src="http://f.cl.ly/items/1T003x0t1m2k0a2O302c/Image%202014-06-10%20at%2010.11.01%20AM.png" width=-"500">


### CDN and working with image assets

If your email contains images you'll want to serve them from a CDN. This Gruntfile has support for Rackspace Cloud Files ([pricing](http://www.rackspace.com/cloud/files/pricing/)).

<img src="http://i.imgur.com/oO5gfkZ.jpg" width="500">

* Sign up for a Rackspace Cloud account (use the [Developer Discount](http://developer.rackspace.com/devtrial/) for $300 credit)
* Create a new Cloud Files container
* Open up `Gruntfile.js`
* Change 'cloudfiles' settings to your settings (you can find your Rackspace API key under your account settings)
* Make any other config changes as per [grunt-cloudfiles](https://github.com/rtgibbons/grunt-cloudfiles) instructions

Run `grunt cdnify` to run the default tasks as well as upload any images to your CDN.

Run `grunt cdnify send --template=branded.html` to send the email to yourself with the 'CDNified' images.

### Sample email templates

I've added two templates here to help you get started.

* [Simple transactional email template](http://leemunroe.github.io/grunt-email-design/dist/transaction.html)
* [Branded email via CDN](http://leemunroe.github.io/grunt-email-design/dist/branded.html)

For more transactional email templates check out [Mailgun's collection of templates](http://github.com/mailgun/transactional-email-templates).
