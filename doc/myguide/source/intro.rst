=====
Intro
=====

Here is a solution described as :ref:`Sphinx <sphinx:examples>` how to
reach a multi-language documentation service.

Why Sphinx?
===========
Sphinx is used to document :ref:`Python <python:functions>`.
Documentation strings from Python modules are fetched and Sphinx will
generate documentation from that.

Output
======
For the output there are different :ref:`Builders <sphinx:builders>`.
You can choose between single- or multi-page HTML, TXT, PDF, or ePub.
There are also other output formats like JSON, XML, or man-pages.
With `hieroglyph <http://docs.hieroglyph.io>`__ you can also generate
slides.
The builder will also make syntax- and link checks for the documents.

Input
=====
The documents are written in :ref:`restructuredtext <sphinx:rst-primer>`

The syntax is pretty easy, i.e. :ref:`print tables <sphinx:rst-tables>`.
Or print code:

.. code-block:: bash

    # python -m sphinx.ext.intersphinx 'http://www.sphinx-doc.org/en/master/objects.inv

Or  :download:`download the document <intro.rst>`.

:ref:`Markdown <sphinx:markdown>` is also supported as source.

Templating
==========
Other things in Sphinx are :ref:`templating <sphinx:templating>`.
Default template engine is `Jinja <http://jinja.pocoo.org/>`__. You can
use variables in loops to work with data from json files or others.

Themes
======
Last but not least to highlight are :ref:`Themes <sphinx:builtin-themes>`.
There are some themes included in Sphinx and the standard theme has
multiple options to configure. But you can also build yuur own theme,
or  `look what Python Packages are available for Sphinx Theme <https://pypi.python.org/pypi?%3Aaction=search&term=Sphinx+Theme&submit=search>`__

Search Engine
=============
:ref:`A search engine <sphinx:search>` is included.

Not enough? 
===========
And mark a timestamp without and script language

|today|

And who is using Sphinx?
========================

* `Ansible Doc <http://docs.ansible.com/>`__
* `OpenStack Doc <https://docs.openstack.org>`__

others:

.. raw:: html

    <pre>

.. raw:: html
   :url: https://raw.githubusercontent.com/sphinx-doc/sphinx/e84ba569a200043b4c13c09d5b21d6e478bfcc47/EXAMPLES

.. raw:: html

    </pre>
   
