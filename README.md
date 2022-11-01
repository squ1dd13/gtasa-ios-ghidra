# GTA SA iOS Ghidra Project

This repository contains a Ghidra project with all of my work on reverse-engineering the 64-bit iOS
version of GTA: San Andreas. The analysis data for the 32-bit Android version of the game is also
included, as it forms a useful reference for the iOS code.

## Why?

My CLEO iOS ([repo](https://github.com/squ1dd13/CLEO-iOS)) project hooks into specific functions
and uses specific parts of game memory. This analysis was required to find the addresses of these
functions and regions. I'm open-sourcing this work as it is, to my knowledge, the most
comprehensive RE work that has been undertaken on the iOS version of the game, and I want this to
be available to anyone who is interested.

## Using this repo

If you want to do your own analysis, simply clone this repository and open the Ghidra project. If
you fork this repository and want to push to your fork, you'll need to set up [Git
LFS](https://git-lfs.github.com/), because GitHub will otherwise reject any files that are over
100MB (which is quite a few in this repository).

If you want to hook functions or mess about with game memory, please note that you can't just use
the raw memory addresses you find in the analysis. iOS offsets the memory addresses by a different
random value every time the game is launched. This value can be found by calling
`_dyld_get_image_vmaddr_slide(int)` (see the [man
page](https://developer.apple.com/library/archive/documentation/System/Conceptual/ManPages_iPhoneOS/man3/dyld.3.html)).
The image index passed to this function will be `0` if the symbol you are hooking is in the game
image.

While the version of the game analysed here is the arm64 version, the memory addresses also apply
to arm64e builds of the game.

## How to use the Android program

Analysis in this project is only focused on the iOS program. However, the Android program is useful
because it has a lot of information that is relevant to the iOS version too. When trying to
understand some code in the iOS program, it can be useful to open the Android program and find the
equivalent code, as it may reveal some symbol names for free.

There is no simple way to transpose all of the symbol names from the Android program to the iOS
program, because the Android game is 32-bit and the iOS game is 64-bit, so the same function in
each program has very different machine code.

## What has been done?

The work done here is actually quite minimal. Only a small percentage of the symbols found in other
versions of the game have been mapped to memory addresses. Everything that is essential to CLEO iOS
has been mapped, plus a few other obvious matches.

## Contributing

Contributions are welcome. You don't need to be an experienced reverse-engineer to be able to match
symbols from the Android game to the iOS game by inspecting them. If you match a function, please
add the full signature. If you match a variable, please ensure that you are using the correct type,
or don't add a type at all (leave it as `undefinedX`).

## Useful resources

Please ignore any structure padding information that you find in other projects, because all other
projecs are 32-bit, which makes a huge difference for structure padding. Just figure out the
structure layout and determine the padding values manually.

[gta-reversed](https://github.com/codenulls/gta-reversed) has full C++ code for some functions,
which can be very useful as a reference. Be aware, however, that the code in that repo is for the
Windows version of the game, so will not (and does not) always match the iOS game.

[plugin-sdk](https://github.com/DK22Pac/plugin-sdk) is useful for finding function signatures and
structure definitions.