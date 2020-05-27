When the WSGI application was provided for ``mod_wsgi-express start-server`` to run, it was supplied as a WSGI script file. This is the ``wsgi.py`` file.

In other words, a path to the file containing the WSGI application entry point was provided. This is different to how a WSGI application is usually supplied to other WSGI servers. In other WSGI servers you would instead supply the name of a Python module to import which contains the WSGI application, rather than an explicit path to a file containing it.

When using ``mod_wsgi-express`` it defaults to expecting a path to a script file as Apache generally works by mapping URL paths to files in the file system, with access controls in part being based around file system directories. Thus when configuring Apache manually to host a Python WSGI application using mod_wsgi, a path to a WSGI script file is required.

This requirement carries across to ``mod_wsgi-express`` as the default behaviour, but it can be overridden.

To start ``mod_wsgi-express``, but specify the WSGI application by using the name of the Python module which contains it, use the ``--application-type`` option and specify ``module`` instead of the default of ``script``. For our case where the WSGI script file is called ``wsgi.py``, the module name is ``wsgi``.

To test this, in the first terminal run:

```execute-1
mod_wsgi-express start-server --application-type=module wsgi
```

To test that the WSGI application still works, in the second terminal run:

```execute-2
curl http://localhost:8000
```

Shutdown the server by entering ``ctrl-c`` into the first terminal.

```execute-1
<ctrl-c>
```

By default mod_wsgi expects that the name of the WSGI application callable which provides the entry point for the WSGI application, is called ``application``.

Not all web frameworks use the convention of the WSGI application callable being called ``application`` and some instead use ``app``.

Rather than rename the WSGI application callable in the source code, you can use the ``--callable-object`` option to specify the alternate name.

To test this with the existing name of ``application`` used in our application, in the first terminal run:

```execute-1
mod_wsgi-express start-server --application-type=module --callable-object=application --entry-point=wsgi
```

To test that the WSGI application still works, in the second terminal run:

```execute-2
curl http://localhost:8000
```

Shutdown the server by entering ``ctrl-c`` into the first terminal.

```execute-1
<ctrl-c>
```

In this case we also introduced another option ``--entry-point``. This can be used explicitly to name the Python module or WSGI script file, rather than using a positional argument with no option.

To see a full list of all the command line options that ``mod_wsgi-express start-server`` accepts, supply the ``--help`` option.

```execute
mod_wsgi-express start-server --help
```

We will be going through more of the most commonly used options in this workshop, so no need to go through and read the help strings of all the options at this point.