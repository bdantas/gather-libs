# gather-libs
Gather a binary and all its linked libraries into a self-executing tarball

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
$ ./filter-libs # this step sorts the libraries so that only those missing on target system are copied to ./libs for active use on the current target system. all libraries are stored in ./libs-all.tgz in case you need to run ./filter-libs on some other target system at a later date.
$ ./pack
```

Now you have `nano.run` (feel free to rename it to just `nano` or whatever you want), which is an executable shell script attached to a tarball.  

At this point you no longer need the `nano` directory, because everything you need is in `nano.run`.

Running `nano.run` causes the tarball to extract itself to a temporary directory in `/tmp` and then the enclosed binary runs with the bundled libraries. When the binary is done running, the temporary directory in /tmp is deleted and only nano.run remains on your system.

# Notes
Coming soon

