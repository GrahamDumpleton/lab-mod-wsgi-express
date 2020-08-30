Prior to the arrival of ``mod_wsgi-express`` there were two different ways of installing mod_wsgi.

The first method was to use the mod_wsgi package provided by the operating system distribution. This was a binary package pre-compiled for the specific version of Apache and Python provided by the operating system.

Because it was a pre-compiled binary, it couldn't be used if you wanted to use a different version of Python than the system Python installation that mod_wsgi was compiled for. This was the case because mod_wsgi is C code and links directly to the Python library for the version of Python it was compiled against. It isn't possible to force the use of a different Python version by pointing mod_wsgi at a Python virtual environment created using a different Python version or installation.

Another issue with the system provided mod_wsgi package was that it was often a quite old version and not the latest version, even when using the latest version of the operating system distribution and not just an LTS (long-term support) version.

The alternative to using the mod_wsgi package provided by the operating system was to compile mod_wsgi from its original source code yourself.

This entailed downloading the source package for mod_wsgi, unpacking it, running a ``configure`` script, then running ``make`` and ``make install``.

By compiling from source code yourself, you could specify an alternate Python version be used, and even a different Apache installation if necessary.

Very important when building from source code was to uninstall any system package for mod_wsgi first, otherwise they would conflict. In uninstalling the system package for mod_wsgi, it did mean though having to configure Apache from scratch to know about mod_wsgi.

Either way, you still had to then manually configure Apache to know about the Python WSGI application you wanted to host. It was this step which tripped many people up when they were new to Apache. More often than not the resulting configuration was sub optimal and wouldn't work well for hosting a Python application as Apache is usually setup to work best for PHP applications, which have totally different runtime requirements.

This is where ``mod_wsgi-express`` came in. At least for the case of where you were only interested in hosting a Python WSGI application, and didn't at the same time need to host other web applications with Apache such as a PHP application, it would generate all the Apache configuration for you. This Apache configuration was setup out of the box to provide a good starting experience for a Python WSGI application, whereas the default Apache configuration doesn't.

One of the additional benefits of ``mod_wsgi-express`` was that it could be run from the command line and didn't have to be run as a system service. This meant it could be used during development as well as in production.
