# TAM - Total Asset Monitor

Run TAM on your apple silicon mac

**Disclaimer #1**: This is not an officially sanctioned project. Basically, if something goes wrong, complain to me... if it works, let the folks at Echometer know.

**Disclaimer #2**: This involves command-line instructions and has the potential to completely destroy your OS and/or data. If you are not comfortable in a terminal, turn back now.

**Disclaimer #3**: Connecting to the Echometer sensors is not supported. This is only for opening/viewing data that was collected through the officially supported windows program.

# The problem
TAM is a windows program and is heavily tied to system specific libraries and components. Compiling TAM to run natively on a non-intel, non-windows system would require significant work and is not on the roadmap. There are many interesting things on that roadmap, and you should talk to Echometer at their booth at one of the many tradeshows, or attend one of their classes.
https://echometer.com/Training

## Options:

A virtual machine such as VirtualBox doesnt yet fully support emulating x86 processors on the M1. Likewise, commercial software such as Parallels supports windows binaries, but only those compiled for ARM processors.

# The solution

Wine is a utility to natively run windows binaries on non-windows systems.
Rosetta is a macos utility to run x86 binaries on the M1.
By combining the 2, we can run x86 windows binaries on the ARM-based M1/M2/M3... but it isnt always easy.

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

### Install TAM:

This is where it gets dicey. The TAM installer fails, but since we arent concerned about the data acquisition sensor connectivity part (see Disclaimer #3 above), we can install TAM by simply copying the TAM folder from a Windows machine into the wine folder.

If you have access to a windows machine and can transfer files, copy the entire TAM folder to your mac (you can omit the `Documents` folder). The location of the TAM folder on your mac doesn't matter, but if you want to keep it windows-y, you can put it in the program files folder (NOTE: the wine folder is hidden, so you might have trouble finding this later)

```
open ~/.wine/drive_c/Program\ Files\ \(x86\)/
```
Or if you dont like all the scary backslashes:
```
open ".wine/drive_c/Program Files (x86)/"
```

Again, you dont have to put the TAM folder here since wine associates the exe files.

One last thing, we need to make the folder for the INI. I'm not 100% sure why it fails to generate this on startup, but its simple enough:

```
mkdir ~/.wine/drive_c/users/`echo $USER`/AppData/Roaming/Echometer/TAM
```

At this point you should be able to just doubleclick the "TAM Application.exe" file.




# Troubleshooting

What could go wrong? Probably a lot. Heres some things to try:

Is wine installed and functional? Try running notepad

```
wine ~/.wine/drive_c/windows/notepad.exe
```

