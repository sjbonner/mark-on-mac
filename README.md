# mark-on-mac

This repository allows you to install Program MARK on Mac OS using homebrew. The advantage of using homebrew over installing the download directly is that it will handle the problem of locating the correct Fortran libraries. Program MARK requires GCC, and you need to tell mark to look for those libraries instead of using MacOS's stock clang libraries. I've created a script to do this and packaged it in a homebrew Formula along with the Program Mark binary so that it is easy to install.

## Installation (Apple Silicon based Macs)

Good news! As of October 2026 I am able to compile MARK directly for Apple Silicon based Macs (MX chips) using Github actiions. This means that it is no longer necessary to install the Intel version of homebrew and rosetta to access the executable for Intel based Macs. In fact, homebrew has also been discontinued for Intel based Macs. The following should work if you are using a new Mac with an Apple Silicon chip. If you are still running an Intel based Mac (kudos for keeping old hardware out of the landfill), then please the the instructions below.

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
This should tell you where the binary has been installed (e.g., `/opt/homebrew/bin/mark`). You can also run
```
mark
```
which should return 
```
No input file was specified, so MARK job is done.
MARK Files:
  i=input_file_name
  o=output_file_name
  r=residuals_file_name
  v=variance-covariance_file_name
MARK Parameters:
  nocolor    - do not use color on output screen
  noecho     - do not echo commands on output screen
  batch      - no output printed to output screen (i.e., batch mode)
  dynamic    - dynamic thread allocation used, otherwise static allocation used
  threads=x  - specify the number of threads to be used
                0 means use all available
               -1 means use all available minus 1
                otherwise use the number specified as x
  linex=x    - print x lines per page, default is 50
                0 means do not print page headers
STOP No input file
```
Running
```
mark -v
```
will provide information about the version you have installed starting with something like this
```
This version was compiled by GCC version 16.2.0 on Oct  5 2026 at 23:07:03
 using the options: 
  -cpp -iprefix /opt/homebrew/Cellar/gcc/16.2.0/bin/../lib/gcc/current/gcc/
  aarch64-apple-darwin25/16/ -D__DYNAMIC__ -D accelerate -fPIC
  -mmacosx-version-min=26.0.0 -mcpu=apple-m1 -mlittle-endian -mabi=lp64 -O3
  -std=f2023 -fimplicit-none -ffpe-summary=invalid,zero,overflow,underflow
  -fno-unsafe-math-optimizations -fopenmp-simd -fstack-arrays -flto=auto
  -fall-intrinsics -fopenmp

  Resolved path: /opt/homebrew/bin/mark
```

## Installation (Intel based Macs)

If you are still running an Intel based Mac, then I commend you for keeping older hardware running and not buying into the need to upgrade to the latest and greatest that tech companies try to sell us.

Unfortunately, homebrew is no longer supported on Intel based Macs. However, Macports provides an alternative. Please follow the instructions at [https://www.macports.org/] to install MacPorts and the latest version of GCC. The current port as of October 2026 is gcc15 which install version 15.2.0.

After that, you can manually download the `mark64intel` binary above. You will need to rename the binary to `mark`, move it to a location in your path, and make sure that it is executable (`chmod a+x mark`).

## Troubleshooting

If you encounter any problems then please submit an issue using the link above. 
