About
=====

Umberum Engine is a system event handling engine.

At the moment it supports receiving RELP syslog events and storing them in
MongoDB.

Installation
============

Prerequisites
-------------

* erlang R15B
  - kernel, stdlib, sasl

Mongodb support:

* emongo 0.0.6: 
  - http://bitbucket.org/japerk/emongo/
* mongodb 1.2.2

Compilation
-----------

Build the source code by running::

  make

To clean::

  make clean

Execution
---------

Run the application in the background::

  ./bin/umberum start

Stop the backgrounded application with::

  ./bin/umberum stop

Administration
--------------

You can run the application in the foreground with an active Erlang tty session.
This is perfect for debugging and development.::

  ./bin/umberum startfg

From here you can do things like start the appmon process visualiser/manager::

  (ologd@localhost)1> appmon:start().

To exit tty just type Ctrl-C then hit 'q'.

Alternatively you can run the toolbar application while the logger is in the
background::

  ./bin/umberum toolbar

Or you can run webtools::

  ./bin/umberum webtools

Which can normally be accessed via HTTP on port 8888.

Usage
-----

At this point the application is very much in heavy development and will 
be in various stages of 'working' depending on what revision you check out.

For now, I have a RELP listener working. If you have rsyslog, modify your 
configuration to include something like the following::

  $ModLoad omrelp
  *.*     :omrelp:127.0.0.1:2222;RSYSLOG_ForwardFormat

This will forward your logs to the listener on port 2222 in RELP format. If you
have mongodb running, check the 'test' database and you should see results 
appear.

Also tail /tmp/log to see the log file output. Again - this is all very basic 
for now - see RELEASE.rst list for things I want to work on. Most of the obvious
stuff is already there.

Sample RELP Transmission
------------------------

RELP + RFC3169, using RFC3339 time (with 6 digit precision). To make this work,
telnet into port 2222 and enter the 3 commands:

0 open 0
1 syslog 90 <0>1937-01-01T12:00:27.871234+01:00 scapegoat.dmz.example.org sched[0]: That's All Folks!
2 close 0
