Here you will find a in depth explanation how the `autoupdate` part of a app manifest works

## Using autoupdate
In the output of `checkver` you can see if an outdated app has autoupdate available, if so you can run following command to automatically update the manifest

    .\bin\checkver.ps1 <app> -u

It is recommended to verify that the updated manifest still works by installing the app with following command

    scoop install bucket\<app>.json


## Add autoupdate to a manifest
For the autoupdate feature to work it needs a `checkver` definition to find the latest version number.

Some manifests already using the `autoupdate` feature:
[NodeJS](https://github.com/lukesampson/scoop/blob/master/bucket/nodejs.json),
[PHP](https://github.com/lukesampson/scoop/blob/master/bucket/php.json),
[nginx](https://github.com/lukesampson/scoop/blob/master/bucket/nginx.json),
[imagemagick](https://github.com/lukesampson/scoop/blob/master/bucket/imagemagick.json)

### Reference

```
    "autoupdate": {
        "note": "Thanks for using autoupdate, please test your updates!",
        "architecture": {
            "64bit": {
                "url": "https://example.org/dl/example-v$version-x64.msi"
            },
            "32bit": {
                "url": "https://example.org/dl/example-v$version-x86.msi"
            }
        },
        "hash": {
            "mode": "extract",
            "url": "https://example.org/download",
            "find": "([a-z0-9]{64})"
        }
    }
```
```
    "autoupdate": {
        "url": "https://example.org/dl/example-$version.zip"
    }
```

All the options can be set globally for all architectures or for each architecture separately 

 - `url`: an url template for generating the new url. It supports the following variables:
   - `$version`: `3.7.1`
   - `$underscoreVersion`: `3_7_1`
   - `$cleanVersion`: `371`
   - `$majorVersion`: `3`
   - `$minorVersion`: `7`
   - `$patchVersion`: `1`
   - `$preReleaseVersion`: Everything after the first `-`, e.g. `3.7.1-rc.1` would result in `rc.1`
 - `extract_dir`: Option to update `extract_dir` option (Variables: `$version`)
 - `note`: Optional message to be displayed when the autoupdate command is run

#### Hashing `hash`
There are several options to obtain the hash of the new file, if nothing is defined the files will be downloaded and hashed locally.

 - `mode`: Can be `extract` (download from `url` and find with the regex), `rdf` to extract from a RDF file ([imagemagick](https://github.com/lukesampson/scoop/blob/master/bucket/imagemagick.json)) or `download` (default) for downloading the file and hash it locally
 - `url`: Url of the page to download or the RDF file (Variables: `$version`, `$url`)
 - `find`: A regex to extract the hash from the source (Variables: `$basename`)
 - `type`: The type of the hash (`sha1`, `sha256` (default), `sha512`)

### Limitations
There are some complex manifests which reach the limits of the current autoupdate implementation (_The list of affected manifests is incomplete_)

 - The binaries specified in the `bin` option change with the version number ([pngcrush](https://github.com/lukesampson/scoop/blob/master/bucket/pngcrush.json), …)
 - There are multiple `url` needed to be updated ([python-exp](https://github.com/lukesampson/scoop/blob/master/bucket/python-exp.json))
 - ~~The `pre_install` or `post_install` commands contain references to the version ([python](https://github.com/lukesampson/scoop/blob/master/bucket/python.json), [python-exp](https://github.com/lukesampson/scoop/blob/master/bucket/python-exp.json))~~ There is a variable `$version` around for that
 - The `env_set` is version depend ([sbcl](https://github.com/lukesampson/scoop/blob/master/bucket/sbcl.json))
