To install Apache with PHP support, run:

    scoop install php apache

The order of 'php' and 'apache' above is important. The Apache setup detects PHP and configures a handler for it if PHP is already installed.

**To start Apache on the command line**, run:

    httpd

Apache will continue running until you press `Ctrl-C` to terminate it.

If you open http://localhost in your browser, you should see a page saying that &ldquo;It works!&rdquo;.

## The document root directory
Scoop configures Apache to serve web pages from the htdocs directory inside the Scoop install directory.

You can get to this directory by running:
    
    pushd "$(scoop which httpd | split-path)\..\htdocs"

If you would like to serve documents from somewhere else, you need to change the DocumentRoot inside the conf/httpd.conf file. You can find httpd.conf at

    "$(scoop which httpd | split-path)\..\conf\httpd.conf"

**To install Apache as a service**, run:

    sudo httpd -k install -n apache
    sudo net start apache

If you don't have `sudo`, you can install it with `scoop install sudo`.

To uninstall the Apache service

    sudo net stop apache
    sudo httpd -k uninstall -n apache

For more information, see [Using Apache HTTP Server on Windows](http://httpd.apache.org/docs/current/platform/windows.html).

