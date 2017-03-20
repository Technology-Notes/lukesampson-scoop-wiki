If you want to customize the settings of your PHP Installation you should never edit the `php.ini` file inside the PHP directory. This file will not survive updates.

Always create you own custom configuration files inside the configuration scan dir.
The directory is located at `~/scoop/apps/php/conf`. You can create as many `.ini` files as you like.

## Examples

Some basic settings like the timezone and limits (`custom.ini`)

``` ini
date.timezone = Europe/Berlin
max_execution_time = 60
memory_limit = 256M
post_max_size = 128M
upload_max_filesize = 128M
```

Enabling debugging (`debug.ini`)

``` ini
display_errors = On
display_startup_errors = On
error_reporting = E_ALL
html_errors = Off
```

Enabling php modules, those are the most commonly needed modules. Take a look inside the `php.ini` what is available (`extensions.ini`)

``` ini
extension=php_curl.dll
extension=php_fileinfo.dll
extension=php_gd2.dll
extension=php_gettext.dll
extension=php_intl.dll
extension=php_ldap.dll
extension=php_mbstring.dll
extension=php_exif.dll      ; Must be after mbstring as it depends on it
extension=php_mysqli.dll
extension=php_openssl.dll
extension=php_pdo_mysql.dll

extension=php_sqlite3.dll
extension=php_tidy.dll
```

_Tip: You can git to store you configurations_