The reason installing mod_wsgi from source code was originally done the way it was, using a ``Makefile`` and the ``make`` program, was because that is how Apache modules were traditionally built. Compilation of Apache modules even required a special compilation script called ``apxs`` to be used instead of the C compiler. This script would execute the C compiler for you, automatically supplying all the required options to tell the compiler where the Apache header files were located and what Apache libraries to link to.

When developing with Python and you want to use a Python package from someone else, you would usually use the ``pip`` command to install it from the Python package index.

What ``mod_wsgi-express`` allows you to do is use the Python workflow of using ``pip`` to install packages, to install mod_wsgi.

Although you can now use ``pip`` to install mod_wsgi, the setup file of the Python package for mod_wsgi still needs to be able to work out where Apache is installed. As a consequence, you still need to have that ``apxs`` script available, but it will be executed under the covers to query from it the information it requires.

On Linux distributions, Apache is usually divided into multiple system packages. The primary packages are the Apache runtime package and the developer package.

The runtime package for Apache contains just the binaries for Apache and all the Apache modules, along with the configuration. This is all that is required to run Apache.

If however you want to build additional Apache modules, which is what we are doing when we install mod_wsgi, you also need the Apache developer package. It is the developer package which contains the ``apxs`` script.

What the names of the runtime and developer packages for Apache are called differ between Linux distributions, so you will need to look up documentation for your Linux distribution to work out what they are.

In this workshop environment we already have both the Apache runtime and developer packages installed, so if you run:

```execute
which apxs
```

you should see:

```
/usr/bin/apxs
```

As mentioned, when installing mod_wsgi using ``pip`` it will execute the ``apxs`` script to query details about the Apache installation. For example, the location of the header files would be determined by running:

```execute
apxs -q INCLUDEDIR
```

When installing mod_wsgi using ``pip``, the ``apxs`` script will not though actually be used to trigger the compilation of the source code for mod_wsgi. Instead, all the required compiler flags will be determined through queries run using ``apxs``, but ``pip`` will actually trigger the compilation of the source code and the creation of the compiled Apache module.

Using ``pip`` to trigger the compilation works because Apache modules and Python extension modules are both shared objects. The only difference is how the entrypoint for an Apache module and Python extension module are setup.

Note that on macOS, Apple removed the ``apxs`` utility and no longer supplies it with the Xcode developer package. It is therefore not possible to build third party Apache modules on macOS in the traditional way. When using ``pip`` to install mod_wsgi, tricks are used to determine details about where the system Apache is installed to workaround the absence of ``apxs``. It is therefore still possible to install mod_wsgi on macOS using ``pip`` where as the traditional way of building it direct from source code no longer works.


