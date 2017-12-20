omxplayer-frontend
==================

This is a web frontend for the Raspberry Pi omxplayer media player.
It is licensed under GNU GPL v3 or later and available at 
https://github.com/mmitch/omxplayer-frontend

omxplayer-frontend was originally written by TallOak and Krageon.

This fork is for «Malina» educational kit http://amperka.ru/product/malina.

PREREQUISITES
-------------

Obviously, you'll need omxplayer.  If it is not already installed on
your system, it should be available via
```
  % apt-get install omxplayer
```

The preinstalled version on the Raspian images from late 2012 was too
old (some commandline parameters have changed), but
```
  % apt-get update; apt-get upgrade
```

did the trick.  If all else fails, get omxplayer from here:
* omxplayer source:   https://github.com/popcornmix/omxplayer
* omxplayer builds:   http://omxplayer.sconde.net/


You'll also need python and the Python package installer.  Both should
be installed by 
```
   % apt-get install python-pip
```

After that, install the Python Flask:
```
   % pip install flask
```


INSTALLATION
------------

Put omxplayer-frontend wherever you like.  In this example, we'll put
it under `$HOME/git/omxplayer-frontend`.  The easiest way to download
is to clone the repository:
```
   $ cd ~/git
   $ git clone git://github.com/mmitch/omxplayer-frontend.git
```
This will create the `omxplayer-frontend` subdirectory. 
Next, either create a subdirectory named `media` in the
`omxplayer-frontend` directory and copy your media files there or
(this is more flexible and recommended) create a link named media to
your existing files:
```
   $ cd ~/git/omxplayer-frontend
   $ ln -s /path/where/your/files/are media
```
Now you are ready to go and can start the omxplayer daemon:
```
   $ cd ~/git/omxplayer-frontend
   $ ./omx_flask.py
```
`omx_flask.py` will then be running on port 8080 and can be reached via
http://your.ip:8080/

`omx_flask.py` itself should run perfectly fine with normal user
permissions.  If you can't play videos, check if you can run `omxplayer`
as a normal user - if this works only as root, either change the
permissions on the video and audio devices or run `omx_flask.py` as
root.  Running as root is generally not a very good idea, but on a
small system for video-only use it might be acceptable.

Root permissions will also be necessary for shutting down your system
via `omx_flask.py`, see SHUTDOWN below.


PROPER INSTALLATION
-------------------

### AUTOSTART ###

To automatically start `omx_flask.py` on boot, you could write
initscripts or add `omx_flask.py` to `/etc/inittab`, but the easiest way
is a crontab entry.  Open your crontab via `crontab -e` and add a line
like this:
```bash
   @reboot cd git/omxplayer-frontend; ./omx_flask.py > /tmp/omx_flask.log
```
If you want to run `omx_flask.py` as root, add this to root's crontab.
In that case, the logfile should be at `/var/log/omx_flask.log`

### PORT CHANGE ###

By default, `omx_flask.py` will run on port 8080. If you want another
port (say, the default HTTP port 80 when you have no other webserver
running, so you don't have to enter the `:8080` in the URL), add the
port number as a parameter to `omx_flask.py`
```
   $ cd ~/git/omxplayer-frontend
   $ ./omx_flask.py 80
```
Of course, this can also be done in a crontab.

UPDATES
-------

Let git update the repository:
```
   $ cd ~/git/omxplayer-frontend
   $ git update
```
If it finds updates and downloads them, you shold restart the running
`omx_flask.py`.  The most simple way to do this is a reboot if
`omx_flask.py` is started automatically.  Ugly but working :)

