# gather-libs
Gather a binary and all its linked libraries into a self-executing tarball

# Dependencies on source system and target system
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
$ cd $HOME
$ gather-libs nano
```
The above step produces `$HOME/nano` directory. Copy that directory to the target system.

On target system:
```
$ cd nano
$ ./filter-libs # this step sorts the libraries so that only those missing on target system will be used.
$ ./pack

That's it!

more details coming

# Notes
Coming soon

