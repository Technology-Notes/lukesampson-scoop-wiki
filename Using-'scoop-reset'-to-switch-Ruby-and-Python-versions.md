If you need to run multiple versions of Ruby or Python on the same computer, you can use `scoop reset` to switch between versions. If you're familiar with rbenv or RVM this is roughly similar, although with `scoop reset` you always need to switch version manually.

### Example
```powershell
scoop install ruby ruby19
ruby --version # -> ruby 1.9.3p448 (2013-06-27) [i386-mingw32]

# switch to ruby 2.0
scoop reset ruby
ruby --version # -> ruby 2.0.0p247 (2013-06-27) [x64-mingw32]

# switch back to 1.9.3
scoop reset ruby19
ruby --version # -> ruby 1.9.3p448 (2013-06-27) [i386-mingw32]
```

`scoop reset` re-installs shims for the app and updates environment variables according to the app manifest. You can switch between Ruby versions as many times as you like—both versions remain installed but one takes priority.

## Python
The same thing works for Python

```powershell
scoop install python27 python
python --version # -> Python 3.3.2

scoop reset python27
python --version # -> Python 2.7.5
```
