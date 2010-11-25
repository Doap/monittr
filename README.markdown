# Monittr : A Web Interface for Monit Statistics #

_Monittr_ provides a _Ruby_ interface for displaying [_Monit_](http://mmonit.com/) statistics in [_Sinatra_](http://www.sinatrarb.com/) based web applications.

It loads information from the [web server embedded in _Monit_](http://mmonit.com/monit/documentation/monit.html#monit_httpd) and makes it accessible as Ruby objects.

It also displays the information in HTML with the provided helpers. You can use the default template, or provide your own. The default template is located in `lib/monittr/sinatra/template.erb`.

## Installation ##

    $ gem install monittr


## Usage ##

You can try the interface in a IRB console:

    $ irb -Ilib -rubygems -rmonittr

You have to pass one or more URLs to a running Monit web server.

In case you don't have a running Monit handy, fake its output:

    require 'fakeweb'
    FakeWeb.register_uri(:get, 'http://localhost:2812/_status?format=xml', :body => File.read('test/fixtures/status.xml') ); nil

Now, retrieve information from the cluster:

    cluster = Monittr::Cluster.new 'http://localhost:2812/_status?format=xml'
    cluster.servers.size

    server = cluster.servers.first
    server.system.status
    server.system.load

    server.filesystems.first.name
    server.filesystems.first.percent

    server.processes.first.name
    server.processes.first.cpu
    server.processes.first.memory

    ...

You can also check out the HTML output by running the example application:

    $ ruby examples/application.rb
    $ open http://localhost:4567/

You should see the information about two faked Monit instances in your browser.

Provide the URLs to _Monit_ instances by setting the appropriate option:

    set :monit_urls,  ['http://localhost:2812/_status?format=xml', 'http://localhost:2812/_status?format=xml']


## Customization ##

It's easy to customize the HTML output by setting the appropriate options in your _Sinatra_ application.


set :template,   Proc.new { File.join(root, 'template.erb') }
set :stylesheet, '/path/to/my/stylesheet'

Please see the example application for more.

## Other Information ##

See the [_monit_](https://github.com/k33l0r/monit) gem for another Ruby interface to _Monit_.

-----

[Karel Minarik](http://karmi.cz)