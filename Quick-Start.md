### Prerequisites
Before you start, make sure you have Powershell 3 installed. 

### Installing Scoop
From the command prompt run:

    @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex (new-object net.webclient).downloadstring('https://get.scoop.sh')"

Assuming you didn't see any error messages, Scoop is now ready to run.

### Using Scoop
Although Scoop is written in Powershell, it's interface is closer to Git and Mercurial than it is to most Powershell programs.

To get an overview of Scoop's interface, run:

    scoop help

You'll see a list of commands with a brief summary of what each command does. For more detailed information on a command, run `scoop help <command>`, e.g. `scoop help install` (try it!).

Now that you have a rough idea of how scoop commands work, let's try installing something.

    scoop install curl

You'll probably see a warning about a missing hash, but you should see a final message that cURL was installed successfully. Try running it:

    curl -L https://get.scoop.sh

You should see some HTML, probably with a 'document moved' message. Note that, like when you installed Scoop, you didn't need to restart your console for the program to work. Also, if you've installed cURL manually before you might have noticed that you didn't get an error about SSL—Scoop downloaded a certificate bundle for you.

##### Finding apps
Let's say you want to install the `ssh` command but you're not sure where to find it. Try running:
    
    scoop search ssh

You'll should see a result for 'openssh'. This is an easy case because the name of the app contains 'ssh'.

You can also find apps by the name of the commands they install. For example,

    scoop search hg

This shows you that the 'mercurial' app includes 'hg.exe'.