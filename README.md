# gather-libs
Gather a binary and all the libraries it needs into a self-extracting, self-executing tarball

# Dependencies
- /bin/sh
- tar

# Installation on source system
```
$ cd /tmp
$ wget https://github.com/bdantas/gather-libs/archive/master.zip
$ unzip master.zip
$ sudo cp ./gather-libs-master/gather-libs /usr/local/bin/
$ cp -r ./gather-libs/pack_template $HOME/.pack_template
```

# Usage example:
On source system:
```
$ gather-libs nano
```
The above step produces `nano` directory in your current directory. Copy the `nano` directory to the target system.

On target system:
```
$ cd nano
$ ./filter-libs # this step sorts the libraries so that only those missing on target system are copied to ./libs for active use. all libraries are saved in ./libs-all.tgz in case you need to run ./filter-libs on some other target system at a later date.
$ ./pack
```

Now you have `nano.run` (feel free to rename it to just `nano` or whatever you want), which is an executable shell script attached to a tarball.  

At this point you no longer need the `nano` directory, because everything you need is in `nano.run`.

Running `nano.run` causes the tarball to extract itself to a temporary directory in `/tmp` and then the enclosed binary runs with the bundled libraries. When the binary is done running, the temporary directory in /tmp is deleted and only `nano.run` remains on your system.  

If at any time you need to tweak `nano.run`, just do this:
```
$ ./nano.run unpack
-> make your tweaks
$ ./pack
```

If sometime in the future you want to use `nano.run` on a different target system, just copy `nano.run` to the target system then do this:
```
$ ./nano.run unpack
$ ./filter-libs
$ ./pack
```

# Notes
- This approach works if target system uses same glibc or newer compared to source system (run `ldd --version` to check glibc version). If target system's glibc version is older than source system's, then you should use my `make-portable` script instead.
- This approach works great but only for applications that work with just its binary and shared libraries. That being said, a lot of applications fall into this category (to name a few: dzen2, lsdvd, mksquashfs, mplayer, mpv, mupdf, scrot, sshpass, xdotool). 

