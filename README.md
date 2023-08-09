# Rapid Build tool

The tools are needed to setup a "rapid" server which is used to distribute
files in the Spring RTS universe.

See http://springrts.com/wiki/Rapid for more info.


# blah

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

./build-ca <svn url>" <svnpath> <modinfopath> <packagespath> <revision> <rapidprefix>

svnpath:
	relative path in svn to watch for changes and log, for example: trunk/mods
	if a change is made in this path (and subpaths) a new package is created
modinfopath:
	relative path in svn to modinfo.lua, for example: trunk/mods/zk/modinfo.lua
packagespath:
	absolute path to the the file store, for example /home/packages/packages
revision:
	svn revision, usally $REVISION (as its run from svn hook)
rapidprefix:
	rapid prefix to use for the package, for example: zk

example:

./build-ca "file://$REPO" trunk/mods trunk/mods/zk/modinfo.lua /home/packages/packages $REVISION zk


streamer is used to "stream" the file to http-clients


# setup

to setup a mirror, you need to create a cron job which runs svn sync. first create an empty svn directory:

	svnadmin create svn/spring-features

##create svn hooks

vi svn/spring-features/hooks/post-commit
	#!/bin/sh
	REPO="$1"
	REVISION="$2"
	ROOT=/home/packages
	$ROOT/build_ca/build-ca "file://$REPO" trunk/mods trunk/spring-features.sdd/modinfo.lua $ROOT/packages $REVISION spring-features
	$ROOT/bin/log.py $REPO $REVISION <channel1> <channel2>
	exit 0

vi svn/spring-features/hooks/pre-revprop-change
	#!/bin/sh
	exit 0

initialize for syncing:

	svnsync init file:///home/packages/svn/spring-features/  http://spring-features.googlecode.com/svn/trunk/


and now do the sync:

	svnsync sync file:///home/packages/svn/spring-features

now add the command to a cron-job.

