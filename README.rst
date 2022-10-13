.. image:: https://img.shields.io/readthedocs/circlink/latest
   :alt: Read the Docs (version)

.. image:: https://img.shields.io/github/workflow/status/tekktrik/circlink/Build%20CI/main
   :alt: Build CI status (main branch)

.. image:: https://img.shields.io/github/v/release/tekktrik/circlink
   :alt: GitHub release (latest SemVer)

.. image:: https://img.shields.io/github/license/tekktrik/circlink
   :alt: License

CircLink
========

Write code locally and have it automatically pushed to your CircuitPython device

Installation
------------

You install CircLink via pip:

.. code-block:: shell

    pip install circlink

Starting a Link
---------------

You can start a link using the ``start`` command, and then the local "read" path and
"write" path on the device.  So to link a file named ``file1.txt`` to a folder named
``cool_files`` on your board, you would use:

.. code-block:: shell

    circlink start file1.txt cool_files

This will also create ``cool_files/`` (and any other parent folders) on your device
if they do not already exist.  If you wanted to write it to the root folder of
the CircuitPython device, use ``.`` for the write path.

You can also use glob patterns for files using ``*``:

.. code-block:: shell

    circlink start *.txt .

If you want to use the glob pattern recursively, you can add the ``--recursive``
flag.

Note that if you're using bash, you'll need to escape the asterisk:

.. code-block:: bash

    circlink start '*'.txt .

Once a link is started all relevant files are pushed to the board, and any
changes in the specified file(s) (including new or deleted files matching a glob
pattern if used) are pushed to the CircuitPython device.  Additionally, the
command line will print out the link ID for the link created.

Other options for starting a link are as follows:

- ``--name NAME`` gives the link a name attribute of ``NAME``, which may be
  useful in remember what the file or glob pattern represents.
- ``--path`` specifies that the write path is based on the current working
  directory in the command line.  This can be useful if for some reason
  ``circlink`` isn't detecting the CircuitPython device.
- ``--wipe-dest`` forces a recursive wipe of the write path directory before
  starting the link.
- ``--skip-presave`` skips the initial save of all linked files when starting
  a link.  This can be useful if you want to start a link, but only want files
  that change since that time to be pushed.

Listing Link Details
--------------------

To list details about a link, you can use the ``link`` command along with the
link ID.  So to list information about the link with ID 1, you would use:

.. code-block:: shell

    circlink list 1

This will list information about links such as IDs, names, whether they are
active, the read and write paths, and even the process ID numbers corresponding
to the links.  Instead of the list ID, you can also use ``all`` or ``last`` to list
information about all the links or just the last one created, respectively.

Stopping a Link
---------------

To stop a link, use the ``stop`` command along with the Link ID:

.. code-block:: shell

    circlink stop 1

You can also use ``all`` and ``last`` to stop all links or just the last one
created, respectively.

Restarting a Link
-----------------

To restart a link, use the ``restart`` command along with the link ID:

.. code-block:: shell

    circlink restart 1

This will start a new link (assuming it was stopped) with the same
settings as before (except for the ``--wipe-dest`` and ``--skip-presave``
settings that were originally used, which are now at they're default).
Note that this means the link will change link IDs.  This command will
also clear the old link from the link history.

If you want to keep the ``--wipe-dest`` and ``--skip-presave`` flags, you'll
need to start a new link using the ``start`` command.

Clearing the Link History
-------------------------

To clear a link from the history, you can use the ``clear`` command with the
link ID:

.. code-block:: shell

    circlink clear 1

Note that this will only work on links that are not actively running.  But as
they say in Yiddish, "Mann Tracht, Un Gott Lacht", and sometimes a link truly
has stopped but wasn't recorded as such.  If you ever need to clear the link
history manually of a link that still shows up, you can use the ``--force`` flag:

.. code-block:: shell

    circlink clear 1 --force

If you If the link is still running, you'll get some nasty error text though.
You can also use ``all`` and ``last`` instead of the link ID to clear all links
or just the last one created, respectively.

License
=======

This library is licensed under an MIT license, so feel free to do with it what
you want, and contributions are always welcome!
