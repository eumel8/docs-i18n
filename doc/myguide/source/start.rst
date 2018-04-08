===============
Getting Started
===============

Common workflow of documentation is:

.. py:function:: Write -> Build -> Publish

Our workflow will be:

.. py:function:: Write -> Build -> Translate -> Publish

Text Editor
===========

There ara variuous tools for different platforms with RST support

* `Online <http://rst.ninjs.org/>`__
* `vim extension <https://www.vim.org/scripts/script.php?script_id=973>`__
* `Notepad++ <https://notepad-plus-plus.org/>`__
* `Visual Studio Code <https://code.visualstudio.com/>`__

Build Tools
===========

It's all described in :ref:`sphinx`. Sphinx package includes all tools
to check and build the documents.

Translation Platform
====================

In the chapter :ref:`zanata` it's described, how Zanata can help us
to translate text strings in differenz languages.

Publishing
==========

Compiled HTML pages can served on our own webserver (see :ref:`lxdcontainer`)
Or we use the pretty online service :ref:`Readthedocs`)

.. _lxdcontainer:

LXD Container
=============

To build our own sphinx build and translation platform, we simple use
`LXD Container <https://www.ubuntu.com/containers/lxd>`__.
If your LXD environment is initialized, just do it::

    lxc launch ubuntu:16.04 zanata
    lxc exec zanata -- bash

Of course, any other Ubuntu system can be used.
