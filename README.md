# mark-on-mac

This repository allows you to install Program MARK on Mac OS using homebrew. The advantage of using homebrew over installing the download directly is that it will handle the problem of locating the correct Fortran libraries. Program MARK requires GCC, and you need to tell mark to look for those libraries instead of using MacOS's stock clang libraries. I've created a script to do this and packaged it in a homebrew Formula along with the Program Mark binary so that it is easy to install.

## Installation (Intel based Macs)

Run the following commands within a terminal to install the package (known as a bottle in homebrew speak). You can skip steps 1 and/or 2 if you already have Xcode and/or homebrew installed. 

1) Install Xcode

You can either install install Xcode from the App Store or run
```
sudo xcode-select --install
```
in the terminal.

1) Install homebrew:
```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

2) Install gcc:
```
brew install gcc
```

3) Install mark:
```
brew tap sjbonner/tap
brew install mark-on-mac
```



You can check that the installation was successful by running
```
which mark
```
This should tell you where the binary has been installed (e.g., `/usr/local/bin/mark`). You can also run
```
mark
```
which should return 
```
 No input file was specified, so MARK job is done.
STOP No input file
```

## Installation (Apple Silicon based Macs)

Many thanks to @waterfleas for providing these instructions.

While mark-on-mac installs on Apple silicon without error, the path that homebrew installs software on is different:
Homebrew on Apple silicon = /opt/homebrew
Homebrew on intel = /usr/local
[see debates on pros and cons of this on github](https://github.com/Homebrew/brew/issues/9177)
This causes an error on launch

```
mark
/opt/homebrew/bin/mark: line 2: /usr/local/bin/mark.64.OSX: No such file or directory
```

The workaround is simple enough - utilise rosetta to install an intel version. Much easier than rewriting paths in mark

Before you start - remove the mark-on-mac from your current brew setup as it will be the default and screw everything up...
```
brew uninstall mark-on-mac
```

Installing homebrew under rosetta
First, install rosetta - open a terminal and type
```
softwareupdate --install-rosetta
```
accept the agreement.

Install the intel version of homebrew
```
arch --x86_64 /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

Create an alias for easy use - I'm calling mine beer
```
echo "alias beer='arch --x86_64 /usr/local/Homebrew/bin/brew'" >> ~/.zsh_aliases
```

Either open a new terminal or source your aliases to enable
```
source ~/.zsh_aliases
```

From here the instructions are the same as the original:
```
beer install gcc
beer tap sjbonner/tap
beer install mark-on-mac
```

## Updating
Another advantage of installing Program MARK through homebrew is that it is simple to update. You can simply run the following two commands:
```
brew update
brew upgrade
```
Note that if you are using @waterfleas instructions for Apple Silicon then you will have to replace `brew` with `beer`:
```
beer update
beer upgrade
```
This will update both GCC and mark-on-mac, if new versions are available, and make sure that the binary has access to the correct libraries.

## Troubleshooting

If you encounter any problems then please submit an issue using the link above. 
