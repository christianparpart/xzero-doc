# Director Plugin

The director plugin is the load balancer and reverse proxy for x0.

Main features include:

- Supporting different backend protocols (HTTP and FastCGI).
- Supporting different transport protocols (TCP over IPv4 and IPv6, and UNIX Domain Sockets).
- Advanced health monitoring.
- JSON API for retrieving state, stats, and reconfiguring clusters at runtime (including adding/updating/removing backends).
- Client side routing support.
- Sticky offline mode.
- Road Warrior Reverse Proxying (basic proxying to a single w/o load balancing and health checks).

### Setup API

#### director.create(name, backend1\_name => url [, backend2\_name => url, ...])

Statically spawns a load balancer by given unique `name`. This one cannot be configured at runtime.

#### director.load(name1 => directoryPath1, ...)

Spawns a load balancer with given identifier `name1` and its configuration stored ad `directoryPath1`.
Multiple load balancers can be passed to one `directory.load()` invokation, as well as
this function can be invoked just multiple times.

A load balancer identifier must be unique and the backend path should also be unique.

### Handler API

#### director.api()

A request handler, to provide the client with a JSON API to inspect the current load balancer states, their statistics, as well as allowing you to actually
reconfigure specific backends.

Statically spawned clusters may only enable/disable backends, but dynamically spawned clusters are capable of creating new backends, reconfiguring them, and
having them removed again.  Dynamic cluster configuration is also kept on local storage and is kept in sync with the live configuration's changes.


#### director.pass(identifier)

Passes the request to the director by given name.  This is usually what you are to use in your configuration file.

    handler main {
        director.pass 'app-cluster'
    }

#### director.pass(identifier, backend)

Passes the request to a specific backend on the given director.  Use this to allow clients to decide (for example via cookie or request-header) what backend is
to server that request, which should help you testing code on a single backend before rolling out the backend's code change on the full cluster.

If the specified backend is not available, a 503 (Service Unavailable) response will be generated and an appropriate error message will be logged.

Please note, that explicitely specifying a backend does ignore any configured capacity limits.

    handler main {
        director.pass 'app-cluster', req.cookie('X-Backend')
    }

#### director.fcgi('address' => IP, 'port' => port)

Directly passes the request to a *FastCGI* backend server on given `address:port` without any load-balancing or queueing features.

    handler main {
        director.fcgi 'address' => 127.0.0.1, 'port' => 9000
    }

#### director.http('address' => IP, 'port' => port)

Directly passes the request to a *HTTP* backend server on given `address:port` without any load-balancing or queueing features.

    handler main {
        director.http 'address' => 127.0.0.1, 'port' => 3000
    }

