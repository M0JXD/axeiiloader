# Axe-FX II Loader

A simple Preset/IR loader for the Axe-FX II on Linux and FreeBSD. It offers both a CLI and a GUI.
You do not need the CLI installed to use the GUI version, they are completely separate and standalone.

## GUI

The GUI should hopefully be quite obvious to use.

![screenshot](assets/screenshot.png)

The GUI is made using GTK3, which should integrate well with most desktop environments.
I'm waiting to see what the likes of Cinnamon and XFCE do before porting it to GTK4.

## CLI

Without arguments, the CLI attempts to receive from the first preset location.
You must specify a file with `-i` to change to send mode.
If you'd prefer to set your own name for received files than the one provided by the tool, you can do so with `-o`.
You can get this help text by passing `-h`:

```
===== AXE-FX II LOADER =====
=== Axe-FX II Loader Help Text ===
The default mode is receive mode, which will save obtained files into the current directory.
If the -d option is not given it will try to autodetect from up to the first five available devices.
The unit type (-t option) does not autodetect, and transfers may fail if it's incorrect.

=== Options ===
-d <device>      The device string as appropriate for the backend, e.g. "hw:2,0" on ALSA and "/dev/umidi0.0" on OSS.
-i <file name>   Set send mode with given input file (whether it's a preset or IR will be autodetected).
-o <file name>   Override the provided file name for receive mode.
-m               Set to get IRs in receive mode (ignored for send mode).
-p <integer>     Set Preset or IR location, defaults to 0. Ignored when sending presets as they're loaded to the edit buffer.
-t <o/x/p>       Set connected unit as Original/MKII (o), XL (x) or XL Plus (p). Defaults to Original/MKII.
-h               Show this help text.
=== END OF HELP ===
```

### Examples

- Send a preset to the edit buffer, specifying an ALSA device string: `axeiiloader -d "hw:2,0" -i mypreset.syx`

- Get a preset from location 100: `axeiiloader -p 100`

- Send an IR to Cabinet location 15: `axeiiloader -i myir.syx -p 15`

- Get an IR from Cabinet location 34: `axeiiloader -m -p 34`

- Note: Scratchpads are locations after the last memory slot, e.g. to send to scratchpad 2 on an OG/MKII: `axeiiloader -i myir.syx -p 102`
    - And likewise on an XL:  `axeiiloader -i myir.syx -p 1026 -t x`

## Building

First make sure all dependencies are installed on your system:
- ALSA on Linux (on Debian based this can be got with `sudo apt install libasound2-dev`)
- OSS on FreeBSD (comes with base)
- GTK3 if building the UI version

Then just issue `make`. The Makefile works with both GNU and BSD versions of Make, and it will create binaries in *build*.
By default it will build both the CLI and GUI programs, although you can specify either `cli` or `gui` targets.

## License

This project is dual licensed under the *BSD 2 Clause* and *GPLv2*.
Some of the ALSA backend code used on Linux originates and derives from `amidi`, which is *GPLv2*.
If you use the ALSA backend you must distribute under the *GPLv2*. With the OSS backend, the `amidi` derived code is not used and you may use/distribute under either license.
From my understanding I believe this does not violate the *GPLv2*, but if it does please make an issue and I will promptly fix.

## Notes

- This was developed on Linux Mint Cinnamon 22 and FreeBSD 15 with an Axe-FX II MkII. It has also been used on Bazzite with an XL+.
- A GCC bug with -02 optimisations messes up the sysex checksum calculation, so the base is built with -01.
- When detecting MIDI devices in both the CLI and GUI versions, it will stop looking/counting after five devices.
    I can improve this to check all devices but will require reworking some aspects. Please open an issue if you need this fixed, otherwise I'm happy to leave as is.
- I previously made a [GUI version of this utility in Flutter](https://github.com/M0JXD/axeiiloader_flutter), but for reasons detailed there this now replaces it.
- This repo was renamed from axeii_loader_native to axeiiloader on 14/02/26 to better reflect the binary names and the fact it no longer uses the "native wrapping" library, IUP.

## Credits

Thanks to:

- Geert Bevin's very handy ReceiveMIDI tool - https://github.com/gbevin/ReceiveMIDI
- The Wine contributors, which has let me run Fractal-Bot and Axe-Edit fairly well.
- The contributors to the Axe-FX Wiki's
- @Wepeell for taking the time to provide me with the information to support XL(+) units.
