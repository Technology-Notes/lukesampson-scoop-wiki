If you want to customize the settings of your PHP Installation you should never edit the `php.ini` file inside the PHP directory. This file will not survive updates.

Always create you own custom configuration files inside the configuration scan dir.
The directory is located at `~/scoop/persist/php/cli/conf.d`. You can create as many `.ini` files as you like.

## Examples

### Custom settings
Some basic settings like the timezone and limits (`custom.ini`)

```ini
date.timezone = Europe/Berlin
max_execution_time = 60
memory_limit = 256M
post_max_size = 128M
upload_max_filesize = 128M
```

### Debugging
Enabling debugging (`debug.ini`)

```ini
display_errors = On
display_startup_errors = On
error_reporting = E_ALL
html_errors = Off
```
#### Using Xdebug
Install Xdebug via `scoop bucket add extras` and `scoop install php-xdebug`. (`xdebug.ini`)
```ini
zend_extension=C:\Users\<user>\scoop\apps\php-xdebug\current\php_xdebug.dll
[xdebug]
xdebug.remote_enable=on
xdebug.remote_autostart=on
xdebug.remote_connect_back=on
```

### Enabling PHP modules
Take a look inside the `php.ini` to know what is available (`extensions.ini`)

```ini
extension_dir=ext

extension=bz2
extension=curl
extension=fileinfo
extension=gd2
;extension=gettext
;extension=gmp
extension=intl
;extension=imap
;extension=interbase
;extension=ldap
extension=mbstring
extension=exif      ; Must be after mbstring as it depends on it
;extension=mysqli
;extension=oci8_12c  ; Use with Oracle Database 12c Instant Client
;extension=odbc
extension=openssl
;extension=pdo_firebird
extension=pdo_mysql
;extension=pdo_oci
;extension=pdo_odbc
extension=pdo_pgsql
extension=pdo_sqlite
extension=pgsql
;extension=shmop

; The MIBS data available in the PHP distribution must be installed.
; See http://www.php.net/manual/en/snmp.installation.php
;extension=snmp

;extension=soap
;extension=sockets
;extension=sodium
extension=sqlite3
;extension=tidy
;extension=xmlrpc
;extension=xsl
```


### Setup Curl and Openssl
Install `cacert.pem` via `scoop install cacert`.

Enable `extension=curl` and `extension=openssl` in `extensions.ini`

Configure curl and openssl to use the `cacert.pem` (`cacert.ini`)
```ini
[curl]
curl.cainfo="C:\Users\<user>\scoop\apps\cacert\current\cacert.pem"
[openssl]
openssl.cafile="C:\Users\<user>\scoop\apps\cacert\current\cacert.pem"
openssl.capath="C:\Users\<user>\scoop\apps\cacert\current\cacert.pem"
```


_Tip: You can use git to store your configurations_