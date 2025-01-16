# TAM - Total Asset Monitor

Run TAM on your Apple silicon Mac (or Intel Macs... and Linux too)

**Disclaimer #1**: This is not an officially sanctioned project. Basically, if something goes wrong, complain to me... if it works, let the folks at Echometer know.

**Disclaimer #2**: This involves command-line instructions and has the potential to completely destroy your OS and/or data. If you are not comfortable in a terminal, turn back now. (It's really just copying and pasting 4 lines... but this is your official cautionary warning)

**Disclaimer #3**: Live data acqusition is not supported. The method below doesnt easily support USB devices. This is only for opening/viewing data that was collected through the officially supported windows program.

# The problem
TAM is a windows program and is heavily tied to system specific libraries and components. Compiling TAM to run natively on a non-Intel, non-Windows system would require significant work and is not currently on the roadmap. There are many interesting things on that roadmap, and you should talk to Echometer at their booth at one of the many tradeshows, or attend one of their classes.
https://echometer.com/Training

## Options:

A virtual machine such as VirtualBox doesnt yet fully support emulating x86 processors on the M1. Likewise, commercial software such as Parallels supports windows binaries, but only those compiled for ARM processors.

<sub>Note: Apparently Parallels just released an x86 version for the ARM Macs, but I've heard its painfully slow. The solution below runs TAM basically as fast as you'd expect (at least on the M2... I havent tried an M1).</sub>

<sub>Note: I havent tried it, but this is a commercial version of the instructions below https://www.codeweavers.com/crossover</sub>


# The solution

[Wine](https://www.winehq.org/) is a utility to natively run windows binaries on non-windows systems.
Rosetta is a Mcos utility to run x86 binaries on the M1.
By combining the 2, we can run x86 Windows binaries natively on the ARM-based M1/M2/M3/M4... but it isn't always easy. (Actually it isn't nearly as hard as it sounds)

Note: this also works on Intel Macs, but it is advisable to use a VM like Virtualbox or Parallels running the Intel version of Windows.

This also works on Linux, but the instructions below don't apply. If you're running Linux, just install wine and winbind and then skip to the last step <sub>If you're running Linux, you should know how to do that... right?.</sub>


## Steps

Open the Mac terminal. Get redy to copy and paste a few commands...

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

### Enable Rosetta (ARM Macs only)

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

TAM has a mode where it can be run entirely isolated from a folder. This was originally done so they could "distribute" TAM on those little blue Echometer USB drives. The INI and wells data folder live within this "DistributedTAM" folder, so you should be able to just extract the following zip somewhere and just double-click the "TAM Application.exe" file.

[TAM 1.9.26 release zip](https://github.com/wansco/TAM_M1/releases/download/TAM_1.9.26/TAM_1.9.26.zip)

Note: this zip file doesn't include any of the example wells. You'll have to import those separately. Let me know if you need those.
<br /><br />


At this point you should be able to just double-click the "TAM Application.exe" file from that unzipped folder.


### Automated TAM Download

Or, if you're already in the terminal, you could copy/paste the following:

```
curl -LO https://github.com/wansco/TAM_M1/releases/latest/download/TAM.zip
unzip TAM.zip
rm TAM.zip
```
The above downloads and extracts the zip file. Open that folder and double-click "TAM Application.exe"

And if you want to see the example wells, do the following:

```
curl -LO https://github.com/wansco/TAM_M1/raw/refs/heads/main/Wells.zip
mkdir -p TAM/Echometer/TAM
unzip ./Wells.zip -d ./TAM/Echometer/TAM/
rm Wells.zip
```
The above commands downloads the example Wells and extracts them to the right place in the TAM folder you created above. Fire up TAM (or restart TAM if it was still running) and you should be able to "Pick Well" in the top left.



# Troubleshooting

What could go wrong? Probably a lot. Here's some things to try:

Is wine installed and functional? Try running notepad.

```
wine ~/.wine/drive_c/windows/notepad.exe
```


The wine installer via Macos & Homebrew configures EXE files to run by just doubleclicking. On Linux you need to do a few extra steps to get the doubleclick. Alternatively you can try this from wherever you extracted the TAM folder:

```
wine "TAM Application.exe"
```

Don't submit crash reports because that'll just confuse things. Remember, this is unsupported. Contact me if something goes wrong.
