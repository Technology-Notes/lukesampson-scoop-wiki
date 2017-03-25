## Coming soon!
Check [#1372](/lukesampson/scoop/issues/1372)

## Data directory
If you need to store data which should persist between updates you should use `~/scoop/data/<app>/`.
Inside the manifest the path to the data directory is available in the `$data_dir` variable.

The [PHP](/lukesampson/scoop/blob/master/bucket/php.json) package uses it for the configuration files.

## App manifest
Directories and files can be added to the `persist` definition inside the app manifest.
Persist data is linked from the installed application directory to the data directory with directory conjunctions or hard links.

During the installation any persistent data is copied into the data directory and linked to.

### Definition
The `persist` definition can be a string if only one item is needed or an array for multiple items.

Optionally a item can have a different name inside the data directory
``` json
{
    "persist": [
        "keeps_its_name",
        ["original_name", "new_name_inside_the_data_dir"]
    ]
}
```
### Examples
- [MySQL](/lukesampson/scoop/blob/master/bucket/mysql.json)
- [MariaDB](/lukesampson/scoop/blob/master/bucket/mariadb.json)
- [NGINX](/lukesampson/scoop/blob/master/bucket/ngnix.json)
- [node.js](/lukesampson/scoop/blob/master/bucket/nodejs.json)