:orphan:

Messaging
=========

The Pulp application uses an AMQP Message Broker for communication with its distributed
components. The Pulp Agent, running on consumers, is one such component.  Pulp supports
the Qpid_ and RabbitMQ_ brokers.

.. _Qpid: http://qpid.apache.org/
.. _RabbitMQ: http://www.rabbitmq.com/


Pulp Configuration
==================

The *messaging* section in Pulp configuration files is used configure messaging. On the server,
this configuration is specified in `/etc/pulp/server.conf`. And, on the consumer, this configuration
is specified in `/etc/pulp/consumer/consumer.conf`.
The **url** property is used to specify the location of the broker. The **transport** property
specifies which transport package (driver) should be used to communicate with the selected broker.

The URL has the format of: `<protocol>://<host>:<port>` where protocol may be one of: (tcp|ssl).

Supported transports:

- **qpid** - AMQP 0-10, tested with:

  - QPid *(all versions)*

- **amqplib** - AMQP 0-8, tested with:

  - RabbitMQ *(3.1.5)*

The following properties are used to specify the (optional) SSL configuration:

- **cacert** - The path to the file containing a CA certificate.
- **clientcert** - The path to the client certificate. Must contain both the private key and
  client certificate.

Qpid Example:

::

   [messaging]
   url: tcp://localhost:5672
   transport: qpid
   cacert: <path to a CA certificate>
   clientcert: <path to a CA certificate>

RabbitMQ Example:

::

   [messaging]
   url: amqp://localhost:5672
   transport: amqplib
   cacert: <path to a CA certificate>
   clientcert: <path to a CA certificate>

Qpid
====

Installation
------------

1. Install the following packages:

- **qpid-cpp-server** - The Qpid message broker.
- **qpid-cpp-server-ssl** - The (optional) SSL support for the Qpid broker.

::

 # yum install qpid-cpp-server
 # yum install qpid-cpp-server-ssl


2. Configure the Qpid broker in /etc/qpidd.conf and either add or change the auth setting
   to be off by having ``auth=no`` on its own line.  The server can be *optionally* configured
   so that it will connect to the broker using SSL by following the steps defined in the
   :ref:`Qpid SSL Configuration Guide <qpid-ssl-configuration>`.  By default, the server
   will connect using a plain TCP connection.

3. Start the `qpidd` service.

::

 # service qpidd start

4. Configure the `qpidd` service to start on boot.

::

 # chkconfig qpidd on


RabbitMQ
========

Installation
------------

1. Install the following packages:

- **rabbitmq-server** - The RabbitMQ message broker.

2. Start the `rabbitmq-server` service.

::

 # service rabbitmq-server start

3. Configure the `rabbitmq-server` service to start on boot.

::

 # chkconfig rabbitmq-server on



