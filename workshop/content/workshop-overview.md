The Apache/mod_wsgi project is now over 13 years old. Long gone are the days when it was viewed as being the new cool thing to use. These days people seeking a hosting mechanism for Python WSGI applications tend to gravitate to other solutions, although they aren't much younger in years.

That mod_wsgi is associated with the Apache project doesn't particularly help as Apache is seen as being old and stale. Truth is that the Apache httpd server has never stopped being improved on and is quite a lot better now than it was around the time when mod_wsgi was started.

Even though the Apache httpd server itself has an even longer history going back 25 years, it is still a workhorse of the Internet and provides a rock solid platform for hosting web sites. It can still hold its own against competing solutions and for hosting Python WSGI applications using mod_wsgi, is a proven reliable solution.

Where mod_wsgi has suffered though is the reputation that Apache is hard to configure. For running mod_wsgi at least, this problem was solved 5 years ago with the release of ``mod_wsgi-express``. This way of deploying Apache/mod_wsgi dramatically simplified the installation and configuration of Apache/mod_wsgi for hosting Python WSGI applications by doing all the hard work of configuring Apache for you.

In this workshop you will learn how to install and use ``mod_wsgi-express``, as well as learn how to customize the configuration.
