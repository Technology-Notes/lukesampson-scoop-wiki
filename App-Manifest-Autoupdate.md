Auto Update is a tool for package maintainers. It automatically checks for new versions of an app and updates the manifest accordingly. It helps to eliminate much of the tedium of updating manifests, as well as reducing the risk of human error while doing so.

Here you will find an in-depth explanation of how the `autoupdate` part of an app manifest works.

# Using autoupdate

Use `checkver` to query the current version of either a specific app or all apps of a bucket.
Open a powershell/cmd, then `cd` into the buckets repository directory and run the following commands.

To query the current version of a specific app the bucket, run:

    cd <bucket repository>
    .\bin\checkver.ps1 <app>

To query the current version of all apps of the main bucket, run:

    .\bin\checkver.ps1 *

In the output of `checkver`, you can see if an outdated app has autoupdate available. If so, you can run the following command to automatically update the respective app's manifest (using `*` will update all apps)

    .\bin\checkver.ps1 <app> -u

To use a bucket other than the main bucket, specify its directory as the second argument to `checkver`

    .\bin\checkver.ps1 <app> <bucket_dir> -u

It is recommended to verify that the updated manifest still works by installing the app with the following command

    scoop install bucket\<app>.json

# Add `checkver` to a manifest

Simplest solution is to use an regex and it will match it to the source of `homepage`. Example: [7zip](https://github.com/lukesampson/scoop/blob/master/bucket/7zip.json)
- `homepage` Page where the version can be found
- `checkver` Regex for finding the version
```
"homepage": "http://www.7-zip.org/",
"checkver": "Download 7-zip ([^\\ ]+)",
```

Use another url if the `homepage` doesn't contain the version. Example: [gradle](https://github.com/lukesampson/scoop/blob/master/bucket/gradle.json)
- `homepage` will be ignored
- `checkver.url` Page where the version can be found
- `checkver.re` Regex for finding the version
```
"homepage": "https://gradle.org",
"checkver": {
    "url": "https://gradle.org/install",
    "re": "The current Gradle release is version ([\\d.]+)"
},
```

Use a JSON endpoint with rudimentary JSON path expressions to retrieve the version. Example: [nuget](https://github.com/lukesampson/scoop/blob/master/bucket/nuget.json)
- `checkver.url` JSON endpoint where the version can be found
- `checkver.jp` JSON path expression for finding the version ([JSONPath Expression Tester](https://jsonpath.curiousconcept.com/))
```
"checkver": {
    "url": "https://dist.nuget.org/index.json",
    "jp": "$.artifacts[0].versions[0].version"
},
```

Use latest app release on Github by setting `checkver` to `github` and the `homepage` to the repository URL. This will try to match the tag with `\/releases\/tag\/(?:v)?([\d.]+)`. *The repository maintainer has to use Github's release feature for this to work. Pre-releases will be ignored!* Example: [nvm](https://github.com/lukesampson/scoop/blob/master/bucket/nvm.json)
```
"homepage": "https://github.com/coreybutler/nvm-windows",
"checkver": "github",
```

Or use different urls for the homepage and repository. Example: [cmder](https://github.com/lukesampson/scoop/blob/master/bucket/cmder.json)
```
"homepage": "http://cmder.net",
"checkver": {
    "github": "https://github.com/cmderdev/cmder"
},
```

Use capture groups for complex versions and use the results in the [`autoupdate` property](#add-autoupdate-to-a-manifest).
This example will provide `$version` and `$matchShort` as variables. Example: [git](https://github.com/lukesampson/scoop/blob/master/bucket/git.json)
```
"checkver": {
    "url": "https://github.com/git-for-windows/git/releases/latest",
    "re": "v(?<version>[\\d\\w.]+)/PortableGit-(?<short>[\\d.]+).*\\.exe"
},
```
- `checkver`: Regex for finding the version on the `homepage`
  - `url`: Page where the version can be found
  - `re`: Regex for finding the version
  - `github`: Url to the apps Github repository
  - `jp`: JSON path expression for finding the version ([JSONPath Expression Tester](https://jsonpath.curiousconcept.com/))
  - `reverse: true`: match the last occurrence found (default is to match the first occurrence). Example: [x264](https://github.com/lukesampson/scoop/blob/master/bucket/x264.json#L26)
  - `replace`: replace the matched value with a calculated value.Example: [sysinternals](https://github.com/lukesampson/scoop-extras/blob/master/sysinternals.json#L9)

# Add `autoupdate` to a manifest
For the autoupdate feature to work it needs a [`checkver` property](#add-checkver-to-a-manifest) to find the latest version number.

Some example manifests using the `autoupdate` feature:
[NodeJS](https://github.com/lukesampson/scoop/blob/master/bucket/nodejs.json),
[PHP](https://github.com/lukesampson/scoop/blob/master/bucket/php.json),
[nginx](https://github.com/lukesampson/scoop/blob/master/bucket/nginx.json),
[imagemagick](https://github.com/lukesampson/scoop/blob/master/bucket/imagemagick.json)

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
   - The `$version` (e.g. `3.7.1.2`) is splitted on each `.` and is assigned to:
     - `$majorVersion`: `3`
     - `$minorVersion`: `7`
     - `$patchVersion`: `1`
     - `$buildVersion`: `2`
   - `$preReleaseVersion`: Everything after the first `-`, e.g. `3.7.1-rc.1` would result in `rc.1`
   - Each capturing group in the [`checkver` property](#add-checkver-to-a-manifest) adds a `$matchX` variable (named groups are allowed). Matching `v3.7.1/3.7` with [`v(?<version>[\d.]+)\/(?<short>[\d.]+)`](https://regex101.com/r/M7RP3p/1) would result in:
      - `$match1` or `$matchVersion`: `3.7.1`
      - `$match2` or `$matchShort`: `3.7`
 - `extract_dir`: Option to update `extract_dir` (Variables: see above)
 - `note`: Optional message to be displayed when the autoupdate command is run
 - `hash`: Set this [property](#add-hash-to-autoupdate) for obtaining hash values without download the actual files.

Some examples with autoupdate and multiple `$match` variables:
* [haxe](https://github.com/lukesampson/scoop/blob/master/bucket/haxe.json)
* [openjdk](https://github.com/lukesampson/scoop/blob/master/bucket/openjdk.json)

# Add `hash` to `autoupdate`
There are several options to obtain the hash of the new file. If the app provider publishes hash values it is possible to extract these from their website or hashfile. If nothing is defined or something goes wrong while downloading/extracting the hash values the files will be downloaded and hashed locally.

Use the same URL as the file and append `.sha256` to it. Example: [openjdk](https://github.com/lukesampson/scoop/blob/master/bucket/openjdk.json)
```
"hash": {
    "mode": "extract",
    "url": "$url.sha256"
}
```

Use a different URL to checksums file (can contain multiple hashes and files). Example: [nodejs](https://github.com/lukesampson/scoop/blob/master/bucket/nodejs.json)
```
"hash": {
    "mode": "extract",
    "url": "https://nodejs.org/dist/v$version/SHASUMS256.txt.asc"
}
```

Use a different regex to extract the hash.
Example: [apache](https://github.com/lukesampson/scoop/blob/master/bucket/apache.json)
```
"hash": {
    "mode": "extract",
    "url": "$url.txt",
    "find": "SHA256-Checksum for: (?:$basename):\\s+([a-fA-F0-9]{64})"
}
```

Use a JSON endpoint with rudimentary JSON path expressions to retrieve the hash. Example: [openssl](https://github.com/lukesampson/scoop/blob/master/bucket/openssl.json)
```
"hash": {
    "mode": "json",
    "jp": "$.files.$basename.sha256",
    "url": "https://slproweb.com/download/win32_openssl_hashes.json"
}
```

All the options can be set globally for all architectures or for each architecture separately

 - `mode`:
   - `extract`: (default) download from `url` and find with the regex
   - `rdf`: extract from a RDF file. Example: [imagemagick](https://github.com/lukesampson/scoop/blob/master/bucket/imagemagick.json)
   - `json`: extract from a JSON file. Example: [openssl](https://github.com/lukesampson/scoop/blob/master/bucket/openssl.json)
   - `download`: (fallback) downloads the file and hash it locally
 - `url`: URL template for downloading RDF/JSON files or extracting hashes. It supports the following variables:
   - All variables used for [`autoupdate` URLs](#add-autoupdate-to-a-manifest)
   - `$url`: autoupdate URL without fragments (`#/dl.7z`) [e.g. `http://example.com/path/file.exe`]
   - `$baseurl`: autoupdate URL without filename and fragments (`#/dl.7z`) [e.g. `http://example.com/path`]
 - `find`: A regex to extract the hash from the source. [Defaults to: `^([a-fA-F0-9]+)$` and `([a-fA-F0-9]+)\s+\*?(?:$basename)`]
   - `$basename`: filename from autoupdate URL (ignores fragments `#/dl.7z`)
 - `jp`: For JSON files: A JSON path to extract the hash from the source (Variables: `$basename`)
 - `type`: Deprecated, hash type is determined automatically

# Limitations
There are some complex manifests which reach the limits of the current autoupdate implementation (_The list of affected manifests is incomplete_)

 - The binaries specified in the `bin` option change with the version number. Example: [pngcrush](https://github.com/lukesampson/scoop/blob/master/bucket/pngcrush.json), …
 - There are multiple `url` needed to be updated. Example: [python-exp](https://github.com/lukesampson/scoop/blob/master/bucket/python-exp.json)
 - The `env_set` is version dependent. Example: [sbcl](https://github.com/lukesampson/scoop/blob/master/bucket/sbcl.json)


<!—- ~~The `pre_install` or `post_install` commands contain references to the version ([python](https://github.com/lukesampson/scoop/blob/master/bucket/python.json), [python-exp](https://github.com/lukesampson/scoop/blob/master/bucket/python-exp.json))~~ There is a variable `$version` around for that -->

# Testing and running autoupdate
If you want to confirm an autoupdate works (e.g. after adding it to an existing manifest or creating a new one) change the `version` field to a lower or different version and then run or use the `-f` parameter

    cd <bucket repository>
    .\bin\checkver.ps1 <app> -u

Check if the `url`, `extract_dir` and `hash` properties have the correct values. Try to install/uninstall the app and submit your changes.