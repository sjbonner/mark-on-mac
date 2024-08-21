## How to Update the Mark-on-Mac Homebrew Tap

I created the tap following the instructions at https://betterprogramming.pub/a-step-by-step-guide-to-create-homebrew-taps-from-github-repos-f33d3755ba74. 

The tap relies on two github repositories

1. sjbonner/Mark-on-Mac (https://github.com/sjbonner/mark-on-mac)
    This is the repository where the binary of Mark actually resides. This simply contains the binary and a bash script that sets the appropriate path for GCC. 
	
2. sjbonner/homebrew-tap (https://github.com/sjbonner/homebrew-tap)
   This repository houses my personal homebrew-tap. This provides the ruby script that homebrew uses to install mark-on-mac.
   
## Updating Mark-on-Mac

1. Clone or update the local version of sjbonner/Mark-on-Mac. 

2. Download the zip archive containing the Mark binary from http://www.phidot.org/software/mark/rmark/linux/. Unzip it and replace copy the most recent version (the binary often contains different versions) to the binary mark.64.OSX in sjbonner/Mark-on-Mac. Commit and push the change.

3. Create a new release in https://github.com/sjbonner/mark-on-mac . Note: I am naming releases as vX.Y.Z where X and Y are the Mark major and minor versions and Z is my version number (in case I release multiple versions of the tap for the same version of Mark). E.g., v10.1.1 is my first release of Mark 10.1.

4. In sjbonner/Mark-on-Mac, run
```
brew create https://github.com/sjbonner/mark-on-mac/archive/refs/tags/vX.Y.Z.tar.gz
```
replacing X, Y, and Z with the approprite version numbers. When prompted provide the formula name MarkOnMac. Note the name of the ruby file which is created. This will be something like `/usr/local/Homebrew/Library/Taps/homebrew/homebrew-core/Formula/markonmac.rb`. I will refer to this as the new ruby script. 

5. Clone or update the local version of sjbonner/homebrew-tap.

6. Copy the `url` and `sha256` from the new ruby script into sjbonner/homebrew-tap/mark-on-mac.rb. 

7. Commit and push the changes.

## Testing

1. Remove the new ruby script.

2. Run
```
brew update
brew upgrade
```

3. Check that the links `/usr/local/bin/mark` and `/usr/local/bin/mark.64.OSX` have been updated to point to the new versions.

4. Run 
```
mark
```



