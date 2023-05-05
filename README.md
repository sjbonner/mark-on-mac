# mark-on-mac

This repository allows you to install Program MARK on Mac OS using homebrew. The advantage of using homebrew over installing the download directly is that it will handle the problem of locating the correct Fortran libraries. Program MARK requires GCC, and you need to tell mark to look for those libraries instead of using MacOS's stock clang libraries. I've created a script to do this and packaged it in a homebrew Formula along with the Program Mark binary so that it is easy to install.

Note that I have only tested this steps on an Intel based Mac. I do not have access to a computer with either the M1 or M2 Apple Silicon chips. If you do then please let me know what happens!

## Installation

Run the following commands within a terminal to install the package (known as a bottle in homebrew speak). You can skip steps 1 and/or 2 if you already have Xcode and/or homebrew installed. 

1) Install Xcode
You can either install install Xcode from the App Store or run
```
sudo xcode-select --install
```
in the terminal.

1) Install homebrew:
```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
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

## Updating
Another advantage of installing Program MARK through homebrew is that it is simple to update. You can simply run the following two commands:
```
brew update
brew upgrade
```
This will update both GCC and mark-on-mac, if new versions are available, and make sure that the binary has access to the correct libraries.

## Troubleshooting

If you encounter any problems then please submit an issue using the link above. 
