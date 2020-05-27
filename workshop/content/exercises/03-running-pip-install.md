Before we attempt to install mod_wsgi using ``pip``, lets first create a Python virtual environment in which to install mod_wsgi and any other packages we require.

No matter how you are working with Python, it is always a recommended best practice to use a Python virtual environment. Installing extra Python packages direct into the main Python installation, or even into a per user ``site-packages`` directory using ``pip install --user``, can cause various [problems](http://blog.dscpl.com.au/2016/01/python-virtual-environments-and-docker.html). This applies even if using a ``Dockerfile`` to create a container image.

To create the Python virtual environment, run:

```execute
python3 -m venv $HOME/venv
```

To use this Python virtual environment, you then need to activate it. Do this for the top terminal window by running:

```execute-1
source $HOME/venv/bin/activate
```

and again for the bottom terminal window by running:

```execute-2
source $HOME/venv/bin/activate
```

Before we install mod_wsgi, first update the version of ``pip`` which is installed into the Python virtual environment. The initial version which is installed is quite often out of date, so it is always a good idea to update it to the latest version.

To update ``pip``, run:

```execute
pip3 install -U pip
```

You can now install mod_wsgi by running:

```execute
pip3 install mod_wsgi
```

This command will build and install the Apache module for mod_wsgi into the Python virtual environment. It will also install the ``mod_wsgi-express`` program from which the name used to refer to this package derives.

Note that at this point the system Apache installation hasn't been touched. In fact, if using ``mod_wsgi-express`` to run Apache with mod_wsgi, the system Apache doesn't need to be modified at all. When using ``mod_wsgi-express`` it will generate its own Apache configuration into a completely separate area and the system Apache configuration is left untouched.

If you didn't want to use ``mod_wsgi-express``, but just wanted the Apache module for mod_wsgi which was created by running ``pip install``, that can be done. For now we will ignore that use case, but will cover it later in the workshop.