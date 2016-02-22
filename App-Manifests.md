An app manifest is a JSON file that describes how to install a program.

##### A simple example:
```json
{
    "version": "1.0",
    "url": "https://github.com/lukesampson/cowsay-psh/archive/master.zip",
    "extract_dir": "cowsay-psh-master",
    "bin": "cowsay.ps1"
}
```

When this manifest is run with `scoop install` it will download the zip file specified by `url`, extract the "cowsay-psh-master" directory from the zip file, and then make the `cowsay.ps1` script available on your path.

For more examples, see the app manifests in the [main Scoop bucket](https://github.com/lukesampson/scoop/tree/master/bucket).

### Reference

* `version` **required**: The version of the app that this manifest installs.
* `url` **required**: The URL or URLs of files to download. If there's more than one URL, you can use a JSON * array, e.g. `"url": [ "http://example.org/program.zip", "http://example.org/dependencies.zip" ]`. URLs can be HTTP, HTTPS or FTP.
* `architecture`: If the app has 32- and 64-bit versions, architecture can be used to wrap the differences ([example](https://github.com/lukesampson/scoop/blob/master/bucket/7zip.json)]).
    * `32bit|64bit`: contains architecture-specific instructions (`url`, `hash`, `msi`, and `installer`).
* `bin`: A string or array of strings of programs (executables or scripts) to make available on the user's path.
    * you can also create an alias shim which uses a different name to the real executable and (optionally) passes arguments to the executable. Instead of just using a string for the executable, use e.g: `[ "program.exe", "alias", "--arguments" ]`. See [busybox](https://github.com/lukesampson/scoop/blob/master/bucket/busybox.json) for an example.
* `shortcuts`: Specifies the shortcut values to made available in a users startmenu. The array specifies an executable/Label pair. See [sourcetree](https://github.com/lukesampson/scoop-extras/blob/master/sourcetree.json) for an example.
* `depends`: Runtime dependencies for the app.
* `env_add_path`: Add this directory to the user's path (or system path if `--global` is used). The directory is relative to the install directory, and must be inside the install directory.
* `env_set`: Sets one or more environment variables for the user (or system if `--global` is used) ([example](https://github.com/lukesampson/scoop/blob/master/bucket/go.json)).
* `extract_dir`: If `url` points to a compressed file (.zip, .7z, .tar, .gz, .lzma are supported), Scoop will extract just the directory specified from it.
* `hash`: A string or array of strings with a file hash for each URL in `url`. Hashes are SHA256 by default, but you can use SHA1 or MD5 by prefixing the hash string with 'sha1:' or 'md5:'.
* `homepage`: The home page for the program.
* `installer`|`uninstaller`: Instructions for running a non-MSI installer.
    * `file`: The installer executable file. For installer, this defaults to the last URL downloaded. Must be specified for `uninstaller`.
    * `args`: An array of arguments to pass to the installer.
* `pre_install` | `post_install` : A string or array of strings of the commands to executed before or after an application is installed. 
* `license`: The software license for the program. For well-known licenses, this will be a string like "MIT" or "GPL2". For custom licenses, this should be the URL of the license.
* `msi` *(deprecated)*: Settings for running an MSI installer<br>
**This property is deprecated and support will be removed in a future version of Scoop.** *The new method is to treat .msi files just like a .zip and extract the files from it without running the full install. You can use the new method simply by not including this `msi` property in your manifest.*
    * `code` *required*: the product code GUID for the MSI installer
    * `silent`: should normally be `true` to try to install without popups and UAC prompts
* `notes`: A string with a message to be displayed after installing the app.
* `checkver`: App maintainers and developers can use the [bin/checkver](https://github.com/lukesampson/scoop/blob/master/bin/checkver.ps1) tool to check for updated versions of apps. The **optional** `checkver` property in a manifest is a regular expression that can be used to match the current stable version of an app from the app's homepage. For an example, see the [go](https://github.com/lukesampson/scoop/blob/master/bucket/go.json) manifest. If the homepage doesn't have a reliable indication of the current version, you can also specify a different URL to check—for an example see the [ruby](https://github.com/lukesampson/scoop/blob/master/bucket/ruby.json) manifest.
 
