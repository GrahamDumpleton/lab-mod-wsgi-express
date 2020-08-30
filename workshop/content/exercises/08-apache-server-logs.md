Apache generates two primary log files. These are the access log and the error log.

The access log is where details of HTTP requests received by the Apache server are logged.

The error log is where the Apache server and any Apache server modules which are loaded log details of what they are doing.

It is also the error log where details of any Python exceptions raised from a WSGI application will be logged.

Do note though that many Python web frameworks will intercept exceptions and log details in their own way. To have those details appear in the Apache error log may require those web frameworks to be configured to log them to the standard error output (console) so they can be captured.

When you run ``mod_wsgi-express`` only the error log is enabled by default. This is because with the high throughput that web servers often receive, enabling the access log can actually result in a decrease in performance. Thus unless you have a specific need to audit all requests, it can be better to leave the access log disabled. If you need to track users, you might instead consider using Google analytics or other application performance monitoring services.

