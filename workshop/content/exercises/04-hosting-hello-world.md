If mod_wsgi had been installed in the traditional manner using a system package for mod_wsgi, the first step you would have needed to perform to host your WSGI application would have been to configure the system Apache to know about that application. Using ``mod_wsgi-express`` that isn't needed.

To illustrate how you can host a WSGI application with ``mod_wsgi-express``, change to the ``~/exercises/hello-world`` directory by running:

```execute
cd ~/exercises/hello-world
```

To see what is contained in this directory run:

```execute
tree `pwd`
```

You will see that all the directory contains is a ``wsgi.py`` file.

To view the contents of the ``wsgi.py`` file run:

```execute
cat wsgi.py
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

```execute-1
mod_wsgi-express start-server wsgi.py
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

To test the WSGI application is working, in the second terminal run:

```execute-2
curl http://localhost:8000
```

You should see the output:

```
Hello World!
```

Shutdown the server by entering ``ctrl-c``.

```execute-1
<ctrl-c>
```