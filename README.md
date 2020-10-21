# gather-libs
Gather a binary and all the libraries it needs into an executable bundle

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

# Basic usage (using `nano` as an example)
**On source system:**
```
$ gather-libs nano
```
The above step produces `nano` directory in your current directory. Copy the `nano` directory to the target system.

**On target system:**
```
$ cd nano
$ ./filter-libs
$ ./pack
```

Now you have `nano.run` (feel free to rename it to just `nano` or whatever you want), which is an executable shell script and a tarball "magically" rolled into a single file.

At this point you no longer need the `nano` directory on the target system (everything you need is in `nano.run`), so you can go ahead and delete the directory.

Running `nano.run` causes the enclosed tarball to be extracted to a temporary directory in `/tmp`, then the enclosed binary runs with the bundled libraries. When the binary is done running, the temporary directory in `/tmp` is automatically deleted.

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
- `foo.run` works if target system uses same glibc or newer compared to source system (run `ldd --version` to check glibc version). If target system's glibc version is older than source system's, then you should use [make-portable](https://github.com/bdantas/make-portable) instead of `gather-libs`.
- `gather-libs` is simple and works great, but only for applications that require just its binary and shared libraries. That being said, a **lot** of applications fall into this category (e.g., curl, feh, lftp, mplayer, mpv, mupdf, nano, to name just a few).

# Other notes
- The basic idea behind the `pack` and `unpack` scripts comes from this [Linux Journal article](https://www.linuxjournal.com/node/1005818).
- I borrowed some general concepts from the AppImage project, of which I'm a huge fan. However, there are innumerable differences between my scrappy `.run` file and the venerable AppImage format, of course. At the highest level: An AppImage (type 2) is an ELF executable concatenated to a `squashfs` archive, whereas my `.run` is a shell script concatenated to a `tar` archive. Whereas my `.run` file _extracts_ its payload at runtime, an AppImage _mounts_ its payload at runtime. My `.run` file can extract itself and recreate itself without any special tools; an AppImage can extract itself but requires an external tool to be put back together again.
