LXD Container
=============

To build our own translation platform, we simple use `LXD Container <https://www.ubuntu.com/containers/lxd>`__
. If your LXD environment is initialized, just do it::

    lxc launch ubuntu:16.04 zanata
    lxc exec zanata -- bash

Of course, any other Ubuntu system can be used.
