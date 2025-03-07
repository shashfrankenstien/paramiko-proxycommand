========
Paramiko
========

Note
----

This version has been modified to use ProxyCommand option specified in :code:`~/.ssh/config`
file. This change also impacts :code:`pysftp` downstream

Example :code:`~/.ssh/config` entry (windows)::

    host *
        ProxyCommand C:\Program Files (x86)\Nmap\ncat.exe --proxy <proxy_host>:<proxy_port> %h %p

To install this version::

    pip install -U git+https://github.com/shashfrankenstien/paramiko-proxycommand.git


----


.. Continuous integration and code coverage badges

.. image:: https://travis-ci.org/paramiko/paramiko.svg?branch=master
    :target: https://travis-ci.org/paramiko/paramiko
.. image:: https://codecov.io/gh/paramiko/paramiko/branch/master/graph/badge.svg
    :target: https://codecov.io/gh/paramiko/paramiko

:Paramiko:    Python SSH module
:Copyright:   Copyright (c) 2009  Robey Pointer <robeypointer@gmail.com>
:Copyright:   Copyright (c) 2020  Jeff Forcier <jeff@bitprophet.org>
:License:     `LGPL <https://www.gnu.org/copyleft/lesser.html>`_
:Homepage:    http://www.paramiko.org/
:API docs:    http://docs.paramiko.org
:Development: https://github.com/paramiko/paramiko


What
----

"Paramiko" is a combination of the Esperanto words for "paranoid" and
"friend".  It's a module for Python 2.7/3.4+ that implements the SSH2 protocol
for secure (encrypted and authenticated) connections to remote machines. Unlike
SSL (aka TLS), SSH2 protocol does not require hierarchical certificates signed
by a powerful central authority.  You may know SSH2 as the protocol that
replaced Telnet and rsh for secure access to remote shells, but the protocol
also includes the ability to open arbitrary channels to remote services across
the encrypted tunnel (this is how SFTP works, for example).

It is written entirely in Python (though it depends on third-party C wrappers
for low level crypto; these are often available precompiled) and is released
under the GNU Lesser General Public License (`LGPL
<https://www.gnu.org/copyleft/lesser.html>`_).

The package and its API is fairly well documented in the ``docs`` folder that
should have come with this repository.


Installation
------------

For most users, the recommended method to install is via pip::

    pip install paramiko

For more detailed instructions, see the `Installing
<http://www.paramiko.org/installing.html>`_ page on the main Paramiko website.


Portability Issues
------------------

Paramiko primarily supports POSIX platforms with standard OpenSSH
implementations, and is most frequently tested on Linux and OS X.  Windows is
supported as well, though it may not be as straightforward.

Bugs & Support
--------------

:Bug Reports:  `Github <https://github.com/paramiko/paramiko/issues/>`_
:Mailing List: ``paramiko@librelist.com`` (see the `LibreList website
               <http://librelist.com/>`_ for usage details).
:IRC:          ``#paramiko`` on Freenode


Kerberos Support
----------------

Paramiko ships with optional Kerberos/GSSAPI support; for info on the extra
dependencies for this, see the `GSS-API section
<http://www.paramiko.org/installing.html#gssapi>`_
on the main Paramiko website.


Demo
----

Several demo scripts come with Paramiko to demonstrate how to use it.
Probably the simplest demo is this::

    import base64
    import paramiko
    key = paramiko.RSAKey(data=base64.b64decode(b'AAA...'))
    client = paramiko.SSHClient()
    client.get_host_keys().add('ssh.example.com', 'ssh-rsa', key)
    client.connect('ssh.example.com', username='strongbad', password='thecheat')
    stdin, stdout, stderr = client.exec_command('ls')
    for line in stdout:
        print('... ' + line.strip('\n'))
    client.close()

This prints out the results of executing ``ls`` on a remote server. The host
key ``b'AAA...'`` should of course be replaced by the actual base64 encoding of the
host key.  If you skip host key verification, the connection is not secure!

The following example scripts (in demos/) get progressively more detailed:

:demo_simple.py:
    Calls invoke_shell() and emulates a terminal/TTY through which you can
    execute commands interactively on a remote server.  Think of it as a
    poor man's SSH command-line client.

:demo.py:
    Same as demo_simple.py, but allows you to authenticate using a private
    key, attempts to use an SSH agent if present, and uses the long form of
    some of the API calls.

:forward.py:
    Command-line script to set up port-forwarding across an SSH transport.

:demo_sftp.py:
    Opens an SFTP session and does a few simple file operations.

:demo_server.py:
    An SSH server that listens on port 2200 and accepts a login for
    'robey' (password 'foo'), and pretends to be a BBS.  Meant to be a
    very simple demo of writing an SSH server.

:demo_keygen.py:
    A key generator similar to OpenSSH ``ssh-keygen(1)`` program with
    Paramiko keys generation and progress functions.

Use
---

The demo scripts are probably the best example of how to use this package.
Also a lot of documentation is generated by Sphinx autodoc, in the
doc/ folder.

There are also unit tests here::

    $ pip install -r dev-requirements.txt
    $ pytest

Which will verify that most of the core components are working correctly.

To test Kerberos/GSSAPI, you need a Kerberos environment. On UNIX you can
use the package k5test to setup a Kerberos environment on the fly::

    $ pip install -r dev-requirements.txt
    $ pip install k5test gssapi pyasn1
    $ pytest
