pacredir
========

**pacredir - redirect pacman requests, assisted by avahi service discovery**

By default every [Arch Linux](https://www.archlinux.org/) installation
downloads its package files from online mirrors, transferring all the
bits via WAN connection.

But often other Arch systems may be around that already have the files
available on local storage - just a fast LAN connection away. This is
where `pacredir` can help. It uses [Avahi](http://avahi.org/) to find
other instances and get the files there if available.

Requirements
------------

To compile and run `pacredir` you need:

* [systemd](https://www.github.com/systemd/systemd)
* [avahi](https://avahi.org/)
* [libmicrohttpd](https://www.gnu.org/software/libmicrohttpd/)
* [curl](https://curl.haxx.se/)
* [iniparser](https://github.com/ndevilla/iniparser)
* [darkhttpd](https://unix4lyfe.org/darkhttpd/)
* [markdown](https://daringfireball.net/projects/markdown/) (HTML documentation)

`Arch Linux` installs development files for the packages by default, so
no additional development packages are required.

Build and install
-----------------

Building and installing is very easy. Just run:

> make

followed by:

> make install

This will place an executable at `/usr/bin/pacredir`,
documentation can be found in `/usr/share/doc/pacredir/`.
Additionally systemd service files are installed to
`/usr/lib/systemd/system/`.

Usage
-----

Enable systemd services `pacserve` and `pacredir`, open TCP
port `7078` and add the following line to your repository
definitions in `pacman.conf`:

> Include = /etc/pacman.d/pacredir

To get a better idea what happens in the background have a look at
[the request flow chart](FLOW.md).

### Databases from cache server

By default databases are not fetched from cache servers. To make that
happen just add the server twice, additionally as regular server. This
is prepared in `/etc/pacman.d/pacredir`, just uncomment the corresponding
line.

Do not worry if `pacman` reports the following after the change:

> error: failed retrieving file 'core.db' from 127.0.0.1:7077 : The
> requested URL returned error: 404 Not Found

This is ok, it just tells `pacman` that `pacredir` could not find a file
and downloading it from an official server is required.

Please note that `pacredir` redirects to the most recent database file
found on the local network if it is not too old (currently 24 hours). To
make sure you really do have the latest files run `pacman -Syu` *twice*.

Security
--------

There is no security within this project, information and file content
is transferred unencrypted and unverified. Anybody is free to serve
broken and/or malicious files to you, but this is by design. So make
sure `pacman` is configured to check signatures! It will then detect if
anything goes wrong.

License and warranty
--------------------

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
[GNU General Public License](COPYING.md) for more details.

### Upstream

URL:
[GitHub.com](https://github.com/eworm-de/pacredir#pacredir)

Mirror:
[eworm.de](https://git.eworm.de/cgit.cgi/pacredir/about/)
[GitLab.com](https://gitlab.com/eworm-de/pacredir#pacredir)

---
[⬆️ Go back to top](#top)
