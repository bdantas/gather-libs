# gather-libs
Gather a binary and all the libraries it needs into a self-extracting, self-executing tarball

# Dependencies
- **/bin/sh**
- **tar**

# Installation on source system
```
$ cd /tmp
$ wget https://github.com/bdantas/gather-libs/archive/master.zip
$ unzip master.zip
$ cd gather-libs-master
$ chmod -R a+x *
$ sudo cp ./gather-libs /usr/local/bin/
$ cp -r ./pack_template $HOME/.pack_template
```

# Basic usage example
**On source system:**
```
$ gather-libs nano
```
The above step produces `nano` directory in your current directory. Copy the `nano` directory to the target system.

**On target system:**
```
$ cd nano
$ ./filter-libs # this step sorts the libraries so that only those missing on target system are copied to ./libs for active use. all libraries are saved in ./libs-all.tgz in case you need to run ./filter-libs on some other target system at a later date.
$ ./pack
```

Now you have `nano.run` (feel free to rename it to just `nano` or whatever you want), which is an executable shell script and a tarball "magically" rolled into a single file.

At this point you no longer need the `nano` directory on the target system (everything you need is in `nano.run`), so you can go ahead and delete the directory.

Running `nano.run` causes the invisible tarball to extract itself to a temporary directory in `/tmp`, then the enclosed binary runs with the bundled libraries. When the binary is done running, the temporary directory in `/tmp` is deleted so that only `nano.run` remains :)

# Advanced usage
If at any time you need to tweak `nano.run`, just do this:
```
$ ./nano.run unpack
$ cd nano
-> make your tweaks
$ ./pack
```

If sometime in the future you want to use `nano.run` on a different target system, just copy `nano.run` to the new target system then do this:
```
$ ./nano.run unpack
$ cd nano
$ ./filter-libs
$ ./pack
```

# Important notes
- `gather-libs` works if target system uses same glibc or newer compared to source system (run `ldd --version` to check glibc version). If target system's glibc version is older than source system's, then you should use my `make-portable` script instead.
- `gather-libs` is simple and works great, but only for applications that require just its binary and shared libraries. That being said, a **lot** of applications fall into this category (e.g., dzen2, lsdvd, mksquashfs, mplayer, mpv, mupdf, nano, scrot, sshpass, xdotool, to name just a few).

# Not-so-important notes
- The `pack` and `unpack` scripts are based on this [great Linux Journal article](https://www.linuxjournal.com/node/1005818), which took me years to discover
- I borrowed some very general concepts ideas from the AppImage project, of which I'm a huge fan. There are innumerable differences between my scrappy `.run` file and the venerable AppImage format, of course. Even at the highest level: An AppImage (type 2) is an ELF executable concatenated to a `squashfs` archive, whereas my `.run` is a humble shell script concatenated to a `tar` archive
- The `pack` and `unpack` scripts can, of course, be put to uses other than bundling binaries and libraries. For other uses you probably won't need the `gather-libs` and `filter-libs` scripts. 
