_Work in progress_

Here you will find a in depth explanation how the `autoupdate` part of a app manifest works

## Using autoupdate
In the output of `checkver` you can see if an outdated app has autoupdate available, if so you can run following command to automatically update the manifest

    .\bin\checkver.ps1 <app> -u

It is recommended to verify that the updated manifest still works by installing the app with following command

    scoop install bucket\<app>.json


## Add autoupdate to a manifest
For the autoupdate feature to work it needs a `checkver` definition to find the latest version number.

*Some manifests already using the `autoupdate` feature*
 - [NodeJS](https://github.com/lukesampson/scoop/blob/master/bucket/nodejs.json)
 - [PHP](https://github.com/lukesampson/scoop/blob/master/bucket/php.json)

### Reference

_TODO_

### Limitations
There are some complex manifests which reach the limits of the current autoupdate implementation (_The list of affected manifests is incomplete_)

 - The binaries specified in the `bin` option change with the version number ([pngcrush](https://github.com/lukesampson/scoop/blob/master/bucket/pngcrush.json), …)
 - There are multiple `url` needed to be updated ([python-exp](https://github.com/lukesampson/scoop/blob/master/bucket/python-exp.json))
 - The `pre_install` or `post_install` commands contain references to the version ([python](https://github.com/lukesampson/scoop/blob/master/bucket/python.json), [python-exp](https://github.com/lukesampson/scoop/blob/master/bucket/python-exp.json))
 - The `env_set` is version depend ([sbcl](https://github.com/lukesampson/scoop/blob/master/bucket/sbcl.json))
