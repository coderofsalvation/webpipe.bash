Webpipe.bash 
============

Webpipes empower your bash-environment with remote executed applications, aka Bash in the Cloud.

<center><img alt="" src="http://media1.giphy.com/media/MVlRUmPRsAnRe/200.gif"/></center>

### Why

Write less html-interfaces, write more glue, mashup your webpipes, control your api.

<img alt="" src="https://raw.github.com/coderofsalvation/webpipe.bash/master/webpipe.png"/>

### Example of a webpipe

Show usage:

    Usage: echo <xmltext> | xpath <xpath> [options]
    
    prints values from xml based on given xpath (can be commaseperated)
    
    Options:
         --dumppath          dump xpath instead of values 
         --dumppathvalues    dump xpath with values 
         --manual            show examples/syntax
         --html              input is html instead of xml
         --source            dont strip tags 
    
    Examples:
      echo '<xml><foo><bar>123</bar></foo></xml>' | xpath //foo/bar
      echo '<xml><foo><bar>123</bar></foo></xml>' | xpath //* --dumppath
      echo '<xml><foo><bar>123</bar></foo></xml>' | xpath //* --dumppathvalues

Execute it:

    $ cat foo.xml | xpath //order/customer/name
    John Doe

Would you believe this just happened in the cloud?
Well it did..

### Installation

Its very simple, first get webpipe.bash

    wget https://raw.github.com/coderofsalvation/webpipe.bash/master/webpipe

Now the only thing you need to do is index some online webpipes:

    source webpipe
    webpipe::index http://neon-semiotics-490.appspot.com

Done! Now you can just use 'xpath --help' in your terminal as if it were a local binary.

### Indexing a single webpipe

You can also import a batch of webpipes all at once like this:

    source webpipe
    webpipe::set webpipe http://localhost/mypipe

Voila..now mypipe is indexed..

`HINT: you can put the above in your ~/.bashrc`

Finally, check which webpipes are indexed:

    $ webpipe::list
    http://localhost/mypipe
    http://neon-semiotics-490.appspot.com/xpath
    http://neon-semiotics-490.appspot.com/cssselect
    http://neon-semiotics-490.appspot.com/csv2json
    http://neon-semiotics-490.appspot.com/json2csv
    http://neon-semiotics-490.appspot.com/json_print_r
    http://neon-semiotics-490.appspot.com/jsonpath
    http://neon-semiotics-490.appspot.com/markdown
    http://neon-semiotics-490.appspot.com/striphtml
    http://neon-semiotics-490.appspot.com/url2html
    http://neon-semiotics-490.appspot.com/xml2json
    http://foo.bar.com/mycommand
    http://foo.bar.com/sendmail
    http://foo.bar.com/notifypentagon
    http://foo.bar.com/makecoffee
    http://foo.bar.com/notifypentagon

### How can I build my own webpipes in the cloud?

Simple, a webpipe is just a weburl which listens to a POST-request (for data) or OPTIONS-request (for displaying help).

For php there's this skeleton [repository](https://github.com/coderofsalvation/webpipe.bash.php)

### Projects using webpipe.bash 

* bashlive.com ( supernatural livecoding using bash )

### Security

Sourcing output of remote procedures from untrusted sources is never adviced.
However, in case your webpipes needs authorisation (.htaccess .htpasswd e.g.) run, decorate the curl calls with this:

    webpipe::set curloptions http://foo.com "-user foo:bar"

By doing so, the webpipe (called by curl) will pass authorisation info.
