---
layout: api-command
language: Ruby
permalink: api/ruby/connect/
command: connect
related_commands:
    use: use/
    repl: repl/
    close: close/
---


# Command syntax #

{% apibody %}
r.connect(opts={}) &rarr; connection
{% endapibody %}

# Description #

<img src="/assets/images/docs/api_illustrations/connect_ruby.png" class="api_command_illustration" />

Create a new connection to the database server.  Accepts the following
options:

- `host`: the host to connect to (default `localhost`).
- `port`: the port to connect on (default `28015`).
- `db`: the default database (default `test`).
- `auth_key`: the authentication key (default none).
- `timeout`: timeout period in seconds for the connection to be opened (default `20`).
- `ssl`: a hash of options to support SSL connections (default `nil`). Currently, there is only one option available, and if the `ssl` option is specified, this key is required:
    - `ca_certs`: a path to the SSL CA certificate.

If the connection cannot be established, a `ReqlDriverError` exception will be thrown.

{% infobox %}
Using SSL with RethinkDB requires proxy software on the server, such as [Nginx][], [HAProxy][] or an SSL tunnel. RethinkDB will encrypt traffic and verify the CA certification to prevent [man-in-the-middle][mitm] attacks. Consult your proxy's documentation for more details.

[Nginx]: http://nginx.org/
[HAProxy]: http://www.haproxy.org/
[mitm]: http://en.wikipedia.org/wiki/Man-in-the-middle_attack
{% endinfobox %}

The authentication key can be set from the RethinkDB command line tool. Once set, client connections must provide the key as an option to `run` in order to make the connection. For more information, read "Using the RethinkDB authentication system" in the documentation on [securing your cluster](http://rethinkdb.com/docs/security/).

__Example:__ Open a connection using the default host and port, specifying the default database.

```rb
conn = r.connect(:db => 'marvel')
```

__Example:__ Open a new connection to the database.

```rb
conn = r.connect(:host => 'localhost',
                 :port => 28015,
                 :db => 'heroes',
                 :auth_key => 'hunter2')
```

__Example:__ Open a new connection to the database using an SSL proxy.

```rb
conn = r.connect(:host => 'localhost',
                 :port => 28015,
                 :auth_key => 'hunter2',
                 :ssl => {
                    :ca_certs => '/path/to/ca.crt'
                 })
```

__Example:__ Open a connection and immediately pass it to a Ruby block. Using this style, the connection will be automatically closed when execution reaches the end of the block.

```rb
r.connect(:db => 'marvel') { |conn|
    r.table('superheroes').run(conn)
}
```
