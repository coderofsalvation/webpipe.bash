Webpipe.bash 
============

Webpipes empower your bash-environment with remote executed applications, aka Bash in the Cloud.

### Why

Write less html-interfaces, write more glue.
Mashup your webpipes, mashup your api.

<center><img alt="" src="http://media1.giphy.com/media/MVlRUmPRsAnRe/200.gif"/></center>

### Installation

Its very simple, first get webpipe.bash

    wget https://raw.github.com/coderofsalvation/webpipe.bash/master/webpipe

Now the only thing you need to do is index some online webpipes:

    source webpipe
    webpipe::index http://neon-semiotics-490.appspot.com

Done! Now you can just use a webpipe in your terminal as if it were a local unix command.


### Example of a webpipe

Show usage:

    $ xpath
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

### How it works

<img alt="" src="https://raw.github.com/coderofsalvation/webpipe.bash/master/webpipe.png"/>

### Indexing a single webpipe

You can also import a batch of webpipes all at once like this:

    source webpipe
    webpipe::set webpipe http://localhost/mypipe

Voila..now mypipe is indexed..

`HINT: you can put the above in your ~/.bashrc`

### Show webpipes

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

# Compatibility with WebPipe Specification 0.2

The webpipes above are more or less compatible.
One exception is that requests (with content-type 'plain/text',  used in the example above) uses single-input single-output.
This is to easify unix commandline development, in contrast to the multi-jsoninput multi-jsonoutput requirement of the <a target="_blank" href="http://www.webpipes.org/" ></a>webpipe 0.2</a>.
`(Jeff: if you read this, please contact me)`
However, the multi-jsoninput multi-jsonoutput requirement *should* apply when you call it with content-type 'application/json'.

Some examples:

    $ curl -X POST    -H 'Content-Type: text/plain' http://localhost/foobar
    # should return the processed value ("some value for bar" e.g.)

    $ curl -X POST    -H 'Content-Type: application/json' -d '{"inputs":[{"css":".foo { font-weight:bold; }"}]}' http://localhost/foobar
    # should return: {"outputs": [{"bar": "some value for bar"}]}
    
    $ curl -X OPTIONS -H 'Content-Type: text/plain' http://localhost/foobar
    # should return usage text like 'Usage: foo <dir> <file>' e.g. (see top of this page)

    $ curl -X OPTIONS -H 'Content-Type: application/json' http://localhost/foobar
    # should return a json schema like so:
    # {
    #     name: String
    #     url: String
    #     description: String
    #     inputs: {
    #         name: String
    #         type: String (Array, String, Number, Boolean)
    #         description: String
    #         optional?: Boolean
    #         default?: Any
    #     }
    #     outputs: {
    #         name: String
    #         type: String (Array, String, Number, Boolean)
    #         description: String
    #     }
    # }

    $ curl -X TRACE http://localhost/foobar 
    # debugs / runs unit test(s) e.g.

However, this multi-in multi-out-assumption of the specs doesnt really fit my needs, therefore for practical reasons contenttypes 'text/plain' and 'text/html' default to single-textinput single-textoutput..the default behaviour of any webserver.

### Further Thoughts

* webpipe is easy to scale
* install once, usable anywhere
* centralize certain functionality on certain server
* prevent a developer becoming 'the guy' within a team
* teams dont need graphical user interfaces
* teams need something pipable
* prevents a large codebase 
* spread domainknowledge across webpipes instead of bundling in 1 codebase
* webpipes need tests
* webpipes need to notify a central place in case of problems .e.g
