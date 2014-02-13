Webpipe.bash 
============

Webpipes empower your bash-environment with remote executed applications, aka Bash in the Cloud.

<img alt="" src="https://raw.github.com/coderofsalvation/webpipe.bash/master/webpipe.png"/>

### Update: use bashlive

<a target="_blank" href="http://www.bashlive.com"></a>bashlive</a> integrates this project, including other powerfull features.
You are adviced to use <a target="_blank" href="http://www.bashlive.com"></a>bashlive</a> to enjoy community webpipes.

### Example of a webpipe

Getting info:

    $ json_print_r --help

        Usage: echo <jsontext> | json_print_r [options]

        prettyprints a json string using the php print_r() function 

        Options:
             --foo=n             number of foos

        Examples:
          echo '{"foo":"bar"}' | json_print_r 
          echo '{"foo":"bar"}' | json_print_r --foo=2

Execute it:

    $ echo '{"foo":["bar","flop"]}' | json_print_r

        stdClass Object
        (
            [foo] => Array
                (
                    [0] => bar
                    [1] => flop
                )

        )

Would you believe me this just happend in the cloud using curl?
Well it did..

### Installation

Its very simple, first get webpipe.bash

    wget https://raw.github.com/coderofsalvation/webpipe.bash/master/webpipe

Now the only thing you need to do is index some online webpipes:

    source webpipe
    webpipe::set webpipe http://neon-semiotics-490.appspot.com/json_print_r

Done! Now you can just use 'json_print_r --help' in your terminal as if it were a local binary.

### Indexing lists of commands

You can also import a batch of webpipes all at once like this:

    source webpipe
    webpipe::index http://neon-semiotics-490.appspot.com

Voila..now json_print_r will also be indexed.

`HINT: you can put the above in your ~/.bashrc`

### How can I build my own webpipes in the cloud?

Simple, a webpipe is just a weburl which listens to a POST-request (for data) or OPTIONS-request (for displaying help).

For php there's this skeleton [repository](https://github.com/coderofsalvation/webpipe.bash.php)

### Projects using webpipe.bash 

* bashlive.com ( supernormal livecoding using bash )

### Security

Sourcing output of remote procedures is not adviced.
However, in case your webpipes needs authorisation (.htaccess .htpasswd e.g.) run, decorate the curl calls with this:

    webpipe::set curloptions http://foo.com "-user foo:bar"

By doing so, the webpipe (called by curl) will pass authorisation info.
