# Rapid Build tool

The tools are needed to setup a "rapid" server which is used to distribute
files in the Spring RTS universe.

See http://springrts.com/wiki/Rapid for more info.

# compile

## prequesites
you need to install ocaml, on ubuntu for example type:

sudo apt-get install ocaml-nox ocaml-findlib libextlib-ocaml-dev \
    libzip-ocaml-dev libxml-light-ocaml-dev libpcre-ocaml-dev libaprutil1-dev \
    libsvn-dev liblua5.1-0-dev


## compile

make


# usage

for automatic usage, setup svnsync to mirror a svn repo with a cronjob.

./build-ca <svn url>" <path to read get logs from> <modinfo lua or tdf> <path to the file store> <revision> <rapid tag>

example:

./build-ca "file://$REPO" trunk/mods trunk/mods/zk/modinfo.lua /home/packages/packages $REVISION zk


streamer is used to "stream" the file to http-clients
