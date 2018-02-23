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

### Required Properties

* `version`: The version of the app that this manifest installs.
* `url`: The URL or URLs of files to download. If there's more than one URL, you can use a JSON * array, e.g. `"url": [ "http://example.org/program.zip", "http://example.org/dependencies.zip" ]`. URLs can be HTTP, HTTPS or FTP.

To change the filename of the downloaded URL, you can append a URL fragment (starting with `#`) to URLs. For example,

`"http://example.org/program.exe"` -> `"http://example.org/program.exe#/dl.7z"`

Note the fragment must start with `#/` for this to work.

In the above example, Scoop will download `program.exe` but save it as `dl.7z`, which will then be extracted automatically with 7-Zip. This technique is commonly used in Scoop manifests to bypass executable installers which might have undesirable side-effects like registry changes, files placed outside the install directory, or an admin elevation prompt.

### Optional Properties

* `architecture`: If the app has 32- and 64-bit versions, architecture can be used to wrap the differences ([example](https://github.com/lukesampson/scoop/blob/master/bucket/7zip.json)]).
    * `32bit|64bit`: contains architecture-specific instructions (`bin`, `checkver`, `extract_dir`, `hash`, `installer`,  `pre_install`, `post_install`, `shortcuts`, `uninstaller`, `url`, and `msi` [`msi` is deprecated]).
* [`autoupdate`](App-Manifest-Autoupdate#add-autoupdate-to-a-manifest): Definition of how the manifest can be updated automatically.
* `bin`: A string or array of strings of programs (executables or scripts) to make available on the user's path.
    * You can also create an alias shim which uses a different name to the real executable and (optionally) passes arguments to the executable. Instead of just using a string for the executable, use e.g: `[ "program.exe", "alias", "--arguments" ]`. See [busybox](https://github.com/lukesampson/scoop/blob/master/bucket/busybox.json) for an example.
    * However if you declare just one shim like this, you must ensure it's enclosed in an outer array, e.g: 
      `"bin": [ [ "program.exe", "alias" ] ]`. Otherwise it will be read as separate shims.
* [`checkver`](App-Manifest-Autoupdate#add-checkver-to-a-manifest): App maintainers and developers can use the [bin/checkver](https://github.com/lukesampson/scoop/blob/master/bin/checkver.ps1) tool to check for updated versions of apps. The `checkver` property in a manifest is a regular expression that can be used to match the current stable version of an app from the app's homepage. For an example, see the [go](https://github.com/lukesampson/scoop/blob/master/bucket/go.json) manifest. If the homepage doesn't have a reliable indication of the current version, you can also specify a different URL to check—for an example see the [ruby](https://github.com/lukesampson/scoop/blob/master/bucket/ruby.json) manifest.
* `depends`: Runtime dependencies for the app which will be installed automatically. See also `suggest` (below) for an alternative to `depends`.
* `env_add_path`: Add this directory to the user's path (or system path if `--global` is used). The directory is relative to the install directory and must be inside the install directory.
* `env_set`: Sets one or more environment variables for the user (or system if `--global` is used) ([example](https://github.com/lukesampson/scoop/blob/master/bucket/go.json)).
* `extract_dir`: If `url` points to a compressed file (.zip, .7z, .tar, .gz, .lzma, and .lzh are supported), Scoop will extract just the directory specified from it.
* `hash`: A string or array of strings with a file hash for each URL in `url`. Hashes are SHA256 by default, but you can use SHA512, SHA1 or MD5 by prefixing the hash string with 'sha512:', 'sha1:' or 'md5:'.
* `homepage`: The home page for the program.
* `installer`|`uninstaller`: Instructions for running a non-MSI installer.
    * `file`: The installer executable file. For `installer` this defaults to the last URL downloaded. Must be specified for `uninstaller`.
    * `script`: A string of commands to be executed as an installer/uninstaller instead of `file`.
    * `args`: An array of arguments to pass to the installer. Optional.
    * `keep`: `"true"` if the installer should be kept after running (for future uninstallation, as an example). If omitted or set to any other value, the installer will be deleted after running. See [`extras/oraclejdk`](https://github.com/lukesampson/scoop-extras/blob/master/oraclejdk.json) for an example. This option will be ignored when used in an `uninstaller` directive.
* `license`: The software license for the program. For well-known licenses, this will be a string like "MIT" or "GPL2". For custom licenses, this should be the URL of the license.
* `notes`: A string with a message to be displayed after installing the app.
* `persist` A string or array of strings of directories and files to persist inside the data directory for the app. [Persistent data](Persistent-data)
* `pre_install` | `post_install` : A string or array of strings of the commands to be executed before or after an application is installed. (Available variables: `$dir`, `$persist_dir`, `$version` many more (_check the `lib/install` script_))
* `psmodule`: Install as a PowerShell module in `~/scoop/modules`.
    * `name` (required for `psmodule`): the name of the module, which should match at least one file in the extracted directory for PowerShell to recognize this as a module.
* `shortcuts`: Specifies the shortcut values to make available in the startmenu. See [sourcetree](https://github.com/lukesampson/scoop-extras/blob/master/sourcetree.json) for an example. The array has to contain a executable/label pair. The third and fourth element are optional.
  1. Path to target file [required]
  2. Name of the shortcut (supports subdirectories: `<AppsSubDir>\\<AppShortcut>` [e.g. sysinternals](https://github.com/lukesampson/scoop-extras/blob/master/sysinternals.json)) [required]
  3. Start parameters [optional]
  4. Path to icon file [optional]
* `suggest`: Display a message suggesting optional apps that provide complementary features. See [ant](https://github.com/lukesampson/scoop/blob/master/bucket/ant.json) for an example. 
    * `["Feature Name"] = [ "app1", "app2"... ]`<br>e.g. `"JDK": [ "extras/oraclejdk", "openjdk" ]`<br>
If any of the apps suggested for the feature are already installed, the feature will be treated as 'fulfilled' and the user won't see any suggestions.

### Undocumented Properties

* `_comment`
* `cookie`
* `description`
* `extract_to`
* `innosetup`

### Deprecated Properties

* `msi` *(deprecated)*: Settings for running an MSI installer<br>
**This property is deprecated and support will be removed in a future version of Scoop.** *The new method is to treat .msi files just like a .zip and extract the files from it without running the full install. You can use the new method simply by not including this `msi` property in your manifest.*
    * `code` *required*: the product code GUID for the MSI installer
    * `silent`: should normally be `true` to try to install without popups and UAC prompts