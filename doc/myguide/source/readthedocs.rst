.. _readthedocs:

Readthedocs
===========

Introduction
------------

`Readthedocs <https://readthedocs.org/>`__ is an online service to host
technical documentation, especially in Sphinx format.
If your documentation code lives on `GitHub <https://github.com/eumel8/docs-i18n/tree/master/doc/myguide/source>`__
a `webhook <https://developer.github.com/webhooks/>`__ can configured
to trigger Readthedocs to fetch and build Sphinx documentation.

Workflow
--------

.. py:function:: Git Commit -> Webhook -> Sphinx Build -> Publish

Use cases
---------

Readthedocs provides different output formats like HTML, ePub, and PDF
in different languages and different versions (branches)

You can configure your own sub-domain like `http://docs-i18n.readthedocs.io/ <http://docs-i18n.readthedocs.io/>`__
or use your own name space and configure aliases or forwards.

Example
-------

`Ansible OTC <http://ansible-otc.readthedocs.io>`__
