*Note: This is only a proposal and hasn't been implemented yet.*

### What are travelling directories?
When Scoop updates an app, it creates a separate directory for the new version. Travelling directories can be used to tell Scoop that certain directories should travel from the old version to the new version. This is intended to solve the problem of packages for an app not working after an update, because they were left behind in the old version directory.

### How do they work?
The contents of travelling directories are moved from the old to the new version. It's fairly similar to just moving the files in Windows Explorer, but with an important exception: *if a file or directory at the top level of the travelling directory already exists at the new location, it isn't moved.*

### Example: Ruby gems
In `ruby.json`, the following `travel_dirs` property is added:

```json
{
...
    "travel_dirs": [ "bin", "lib/ruby/gems/2.0.0/gems" ]
...
}
```

The `bin` directory contains .bat files that should be executable anywhere and this directory will be added to the user's path by Scoop. The `...\gems` directory contains the actual Ruby gems that were installed using the previous Ruby version.

### Example: NodeJS
In `nodejs.json`, the following `travel_dirs` property is added:

```json
{
...
    "travel_dirs": [ "./", "node_modules" ]
...
}
```

The `./` directory (the base directory for NodeJS) contains executable .bat/.cmd files. These executables are sometimes installed by NPM (the package manager for NPM) when packages are installed, so they need to travel to the new version when NodeJS is updated. The `node_modules` directory contains the actual packages so it needs to travel too.
