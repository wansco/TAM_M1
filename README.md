# TAM - Total Asset Monitor

Run TAM on your apple silicon mac (or Intel Macs... and Linux too)

**Disclaimer #1**: This is not an officially sanctioned project. Basically, if something goes wrong, complain to me... if it works, let the folks at Echometer know.

**Disclaimer #2**: This involves command-line instructions and has the potential to completely destroy your OS and/or data. If you are not comfortable in a terminal, turn back now.

**Disclaimer #3**: Connecting to the Echometer sensors is not supported. This is only for opening/viewing data that was collected through the officially supported windows program.

# The problem
TAM is a windows program and is heavily tied to system specific libraries and components. Compiling TAM to run natively on a non-intel, non-windows system would require significant work and is not on the roadmap. There are many interesting things on that roadmap, and you should talk to Echometer at their booth at one of the many tradeshows, or attend one of their classes.
https://echometer.com/Training

## Options:

A virtual machine such as VirtualBox doesnt yet fully support emulating x86 processors on the M1. Likewise, commercial software such as Parallels supports windows binaries, but only those compiled for ARM processors.

<sub>Note: Apparently Parallels just released an x86 version for the ARM Macs, but I've heard its painfully slow. The solution below runs TAM basically as fast as you'd expect (at least on the M2... I havent tried an M1).</sub>

<sub>Note: I havent tried it, but this is a commercial version of the instructions below https://www.codeweavers.com/crossover</sub>


# The solution

Wine is a utility to natively run windows binaries on non-windows systems.
Rosetta is a macos utility to run x86 binaries on the M1.
By combining the 2, we can run x86 windows binaries on the ARM-based M1/M2/M3/M4... but it isn't always easy. (Actually it isn't nearly as hard as it sounds)

Note: this also works on Intel macs, but it is advisable to use a VM like Virtualbox or Parallels running the intel version of Windows.

This also works on Linux, but the instructions below don't apply. If you're running Linux, just install wine and winbind and then skip to the last step.


## Steps

### Install Homebrew:

https://brew.sh/

```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```


### Install wine:

https://wiki.winehq.org/MacOS

```
brew tap homebrew/cask-versions
brew install --cask --no-quarantine wine-stable
```

### Enable Rosetta (ARM Macs)

At the end of the wine install above, the terminal should print the following message:
<pre>
wine-stable is built for Intel macOS and so requires Rosetta 2 to be installed.
You can install Rosetta 2 with:
  softwareupdate --install-rosetta --agree-to-license
Note that it is very difficult to remove Rosetta 2 once it is installed.
</pre>
See Disclaimer #2 above. If you are ok with this, run the following command:
```
softwareupdate --install-rosetta --agree-to-license
```



### Run TAM:

TAM has a mode where it can be run entirely from a folder. This was originally done so they could "distribute" TAM on those little blue Echometer USB drives. The INI and wells data folder live entirely within this "DistributedTAM" folder, so you should be able to just extract the following zip somewhere and just double-click the "TAM Application.exe" file.

[TAM 1.9.26 release zip](https://github.com/wansco/TAM_M1/releases/download/TAM_1.9.26/TAM_1.9.26.zip)

Note: this zip file doesn't include any of the example wells. You'll have to import those separately.
<br /><br />


At this point you should be able to just double-click the "TAM Application.exe" file from that unzipped folder.




# Troubleshooting

What could go wrong? Probably a lot. Here's some things to try:

Is wine installed and functional? Try running notepad.

```
wine ~/.wine/drive_c/windows/notepad.exe
```

Don't submit crash reports because that'll just confuse things. Remember, this is unsupported. Contact me if something goes wrong.
