# How is Scoop different to [Chocolatey](http://chocolatey.org)?

* **Installs to ~/appdata/local/ by default** You can set up your own programs and not worry that they'll interfere with other users' programs (or theirs with yours, perhaps more importantly). You can optionally choose to install programs system-wide if you have admin rights.
* **No UAC popups, doesn't require admin rights*.** Since programs are installed just for your user account, you won't be interrupted by UAC popups. InnoSetup installers are the exception when no other install method is available.
* **Doesn't pollute your path.** Where possible, Scoop puts your program shims in a single directory and just adds that to your path
* **Doesn't use NuGet.** NuGet is a great solution to the problem of managing software library dependencies. Scoop avoids this problem altogether: each program you install is isolated and independent.
* **Simpler than packaging** Scoop isn't a package manager, rather it reads plain JSON manifests that describe how to install a program and it's dependencies.
* **Simpler app repo.** Scoop just uses git for it's app repo. You can create your own repo, or even just a single file that describes an app to install.
* **Can't install a specific version of a program.** Scoop doesn't allow installing every version release of a program, just the latest stable version. There are some exceptions, e.g. Python 2.7 and Ruby 1.9 which are commonly required—these can be installed from `python27` and `ruby19`.
* **Focuses on developer tools.** While it would be easy to install Skype with Scoop, this will probably never be in Scoop's main bucket (app repository). Scoop focuses on open-source, command-line developer tools.