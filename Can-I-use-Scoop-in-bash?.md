## Short answer: yes!

Add `alias scoop="powershell -noprofile -ex unrestricted scoop.ps1"` to your `~/.bashrc` (or `~/.profile` if you're using `busybox`). Restart bash or run `source ~/.bashrc` and Scoop will now work.

## Long answer

When you're in bash: if you use Scoop, Powershell needs to be started up every time you call the alias. You'll notice that it can take a while. So the question is, should you use Scoop in bash?