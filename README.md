
# TETRIX-OS-JDH

This code was initially created by jdh, and serves as a great source of information on building legacy BIOS x86 code.
Original videos documenting it creation still available at https://www.youtube.com/watch?v=FaILnmUYS_U ("I made an entire OS that only runs Tetris")
- [original videos]( https://www.youtube.com/watch?v=FaILnmUYS_U)

That some corporate overlords can make locating material this rich in educational value even marginally more difficult is absurd

# TETRIX-OS: An operating system that only plays Tetrix.

![screenshot](images/0.png)

[Video with an explanation of the development process.](https://www.youtube.com/watch?v=FaILnmUYS_U)

#### Features:
- It's Tetris.
- 32-bit (x86)
- Fully custom bootloader
- Soundblaster 16 driver
- Custom music track runner
- Fully hardcoded tetrix theme
- Double-buffered 60 FPS graphics at 320x200 pixels with custom 8-bit RGB palette

#### Resources Used
- [osdev.org wiki](https://wiki.osdev.org/Main_Page)
- [Sortix](https://sortix.org)
- [ToaruOS](https://toaruos.org)
- [James Molloy's Kernel Development Tutorials](http://www.jamesmolloy.co.uk/tutorial_html/)

### Building & Running
~~**NOTE**: This has *only* been tested in an emulator. Real hardware might not like it.~~

EDIT: this is not true anymore! [@parkerlreed has run this on a Thinkpad T510](https://github.com/jdah/tetrix-os/issues/5#issuecomment-824507979).

#### Mac OS
For the cross-compiler: `$ brew tap nativeos/i386-elf-toolchain && brew install i386-elf-binutils i386-elf-gcc`
```
$ make iso
$ qemu-system-i386 -drive format=raw,file=boot.iso -d cpu_reset -monitor stdio -device sb16 -audiodev coreaudio,id=coreaudio,out.frequency=48000,out.channels=2,out.format=s32
```

#### Unix-like
You should not need a cross-compiler in *most* cases as the `gcc` shipped in most linux distros will support `i386` targets.

[If this isn't the case for you, read here about getting a cross-compiler.](https://wiki.osdev.org/GCC_Cross-Compiler)

To run:
```
$ make iso
$ qemu-system-i386 -drive format=raw,file=boot.iso -d cpu_reset -monitor stdio -device sb16 -audiodev pulseaudio,id=pulseaudio,out.frequency=48000,out.channels=2,out.format=s32
```

If you have sound device issues, try building without the `#define ENABLE_MUSIC` in `main.c` and running with `$ qemu-system-i386 -drive format=raw,file=boot.iso`.

If you're having issues with no image showing up/QEMU freezing, this is a known bug with QEMU SB16 emulation under GTK. [Please read what @takaswie has written in #2 for a workaround](https://github.com/jdah/tetrix-os/issues/2#issuecomment-824773889).

#### Windows
Absolutely no idea. Maybe try WSL.

#### Real hardware
You probably know what you're doing if you're going to try this. Just burn `boot.iso` onto some bootable media and give it a go. If things break, try disabling all of the music (remove `#define ENABLE_MUSIC` in `main.c`) since you *probably* don't have something with a SB16 in it.
