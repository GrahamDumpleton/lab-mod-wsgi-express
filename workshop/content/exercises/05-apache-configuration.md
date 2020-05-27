Each time you run the ``mod_wsgi-express start-server`` command it will generate an Apache configuration on the fly based on the command line options given for that specific invocation.

This generated configuration will by default usually be placed into a sub directory of the system ``/tmp`` or ``/var/tmp`` directory. The exact location depends on the operating system being used.

To see the contents of the Apache configuration generated when you ran ``mod_wsgi-express start-server``, run:

```execute
cat /tmp/mod_wsgi-localhost:8000:1001/httpd.conf
```

The output is long and detailed and you are not expected to understand it, and that is the aim. Rather than you needing to work out how to configure Apache, ``mod_wsgi-express`` will do it for you, applying an opionated configuration which uses all the techniques learned over the years to setup a configuration which gets close to ensuring Apache runs at its best for Python WGSI applications.

Although the Apache configuration is generated, it doesn't mean you will not need to tune the configuration based on expected server load and application characteristics. In order to tune the configuration though, rather than needing to understand the arcane Apache syntax, all you need to do is use command line arguments to ``mod_wsgi-express start-server`` and it will make the adjustments.

As noted, the generated configuation is an opionated configuration. This means it makes choices as to what is the best configuration for running Apache with a Python WSGI application.

Some of the key choices it makes and enforces through the configuration are as follows.

* The "event" multi processing module (MPM) for Apache is used when available. If it isn't available then the "worker" MPM is used. The "prefork" MPM is only used when there is no other choice. The "prefork" MPM is the worst option for running Python WSGI applications, yet is usually the default in most Apache configurations as the multi threaded "event" and "worker" MPMs are not recommended for PHP web applications.

* Daemon mode of mod_wsgi is used. This means that the Python WSGI application runs in its own separate set of processes distinct from the Apache child worker processes. Further, initialisation of the Python interpreter within the Apache child worker processes is turned off to reduce resource usage. Daemon mode of mod_wsgi allows much greater control over the application lifecycle of the Python WSGI application and leads to a more resilient deployment in the face of application issues than when embedded mode of mod_wsgi is used.

* The Apache MPM server settings for the Apache child processes are configured to reduce the amount of process restarting when the Apache server becomes momentarily idle and traffic starts to flow once again.

* The Apache child worker stack memory and buffer space is configured to reduce the blow out in process memory that can often occur with a default Apache configuration. Additional buffer space will still be able to be allocated when serving up a large response, but any excess memory will be reclaimed for reuse by the process as a whole, rather than then being constantly held in reserve against the request handlers thread specific memory pool where it was used.

* A smaller pool of threads is by default used for each daemon mode process running the Python WSGI application than is usually the default for mod_wsgi. This is because the original high default doesn't perform well when running Python WSGI applications which lean towards being CPU bound. The new default can be overridden where you have a primarily I/O bound web application, but in general you are better off scaling by using more processes, rather than threads. This is due to how Python implements threads and potential contention issues with the Python global interpreter lock.

* The daemon mode processes where the Python WSGI application runs are configured with request timeouts and other timeouts which detect liveness and trigger automatic process restarts when lock ups occur, ensuring that the server can recover and not lock up completely and require a full server restart.