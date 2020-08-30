If mod_wsgi had been installed in the traditional manner using a system package for mod_wsgi, the first step you would have needed to perform to host your WSGI application would have been to configure the system Apache to know about that application. Using ``mod_wsgi-express`` that isn't needed.

To illustrate how you can host a WSGI application with ``mod_wsgi-express``, change to the ``~/exercises/hello-world`` directory by running:

```terminal:execute
command: cd ~/exercises/hello-world
```

To see what is contained in this directory run:

```terminal:execute
command: tree `pwd`
```

You will see that all the directory contains is a ``wsgi.py`` file.

To view the contents of the ``wsgi.py`` file run:

```terminal:execute
command: cat wsgi.py
```

It should contain:

```
def application(environ, start_response):
    status = '200 OK'
    output = b'Hello World!'

    response_headers = [('Content-type', 'text/plain'),
                        ('Content-Length', str(len(output)))]
    start_response(status, response_headers)

    return [output]
```

To start up Apache/mod_wsgi and host this WSGI application using ``mod_wsgi-express``, from the first terminal run:

```terminal:execute
command: mod_wsgi-express start-server wsgi.py
```

This should result in the output:

```
Server URL         : http://localhost:8000/
Server Root        : /tmp/mod_wsgi-localhost:8000:1001
Server Conf        : /tmp/mod_wsgi-localhost:8000:1001/httpd.conf
Error Log File     : /tmp/mod_wsgi-localhost:8000:1001/error_log (warn)
Request Capacity   : 5 (1 process * 5 threads)
Request Timeout    : 60 (seconds)
Startup Timeout    : 15 (seconds)
Queue Backlog      : 100 (connections)
Queue Timeout      : 45 (seconds)
Server Capacity    : 20 (event/worker), 20 (prefork)
Server Backlog     : 500 (connections)
Locale Setting     : en_US.UTF-8
```

This lists various details about the Apache/mod_wsgi server instance, including the local URL for accessing it.

To view what processes are running, in the second terminal run:

```terminal:execute
command: ps f
session: 2
```

The output from this should include:

```
  PID TTY      STAT   TIME COMMAND
  100 pts/0    Ss     0:00 /bin/bash -il
  200 pts/0    S+     0:00  \_ httpd (mod_wsgi-express)   -f /tmp/mod_wsgi-localhost:8000:1001/httpd.conf ...
  201 pts/0    Sl+    0:00      \_ (wsgi:localhost:8000:1001) -f /tmp/mod_wsgi-localhost:8000:1001/httpd.conf ...
  202 pts/0    Sl+    0:00      \_ httpd (mod_wsgi-express)   -f /tmp/mod_wsgi-localhost:8000:1001/httpd.conf ...
```

Right now the deployment of Apache/mod_wsgi consists of three processes.

The first process is the Apache parent process. It's main job is to manage the child processes and ensure they are restarted if they stop.

The second process is the mod_wsgi daemon process. This is the one labelled ``(wsgi:localhost:8000:1001)``. It is in this process that the WSGI application runs.

The third process is the Apache child worker process. It is what accepts any inbound HTTP requests. In the case of the HTTP request needing to be handled by the WSGI application, this request will be proxied to the WSGI application running in the mod_wsgi daemon process. The Apache child worker processes are also where HTTP requests for static resources are handled.

The number of Apache child worker processes and mod_wsgi daemon process can differ based on the options given to the ``mod_wsgi-express start-server`` command, with the number of Apache child worker processes also scaling up as necessary when the server is under load.

To test the WSGI application is working, in the second terminal run:

```terminal:execute
command: curl http://localhost:8000
session: 2
```

You should see the output:

```
Hello World!
```

Shutdown the server by entering ``ctrl+c`` into the first terminal.

```terminal:interrupt
```
