.. Rasmart API documentation documentation master file, created by
   sphinx-quickstart on Wed Feb 13 20:59:02 2019.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

RASMART API DOCUMENTATION
=====================================================

.. image:: _static/logo.png

.. toctree::
   :maxdepth: 3
   :caption: Contents:

Introduction
==================
API is an interface through which third-party applications can work with the blockchain information.

Architecture
------------
Running host software allows you to establish a TCP connection for two-way communication. On this connection, packets with requests and responses are sent to both sides. Packages are not related to each other in the "request - response" and are processed independently of each other.

Protocol
--------
The connection is initiated by the client. After the connection is established, the client sends its public key and waits for a similar packet from the server. After completion of the key exchange operation, the parties independently generate a common encryption key, which encrypts all further information exchange.

The keys of asymmetric encryption can be as permanent, there and session. During the exchange process, you can initiate a key change without reinstalling the connection.

The same command is used in the request and response block. A packet sent from the client to the server is a request, and from the server to the client is a response.

Format
-------
A block is a byte array with the following structure:

#. Block 
	#. Command (2 bytes) - block type identifier
	#. Data size (4 bytes) - block payload size in bytes
	#.  Data - byte array whose structure depends on the command handler.
#. ...
#. Zero block in which the command and data size are 0.

If a shipment package consists of one block, then a null block should follow it. If a packet consists of several blocks, the zero block must follow only the last block.

Commands
==================

SendInfo
--------------
If the server does not support the requested ``SendInfo`` protocol version, an error code is returned and the connection is terminated.

Request:

- ``version`` - version number
- ``key`` - client's public key

Response:

- ``error < code, message >`` - code and error message
- ``server_info`` - server public key

GetBalance
---------------

Request:

- ``currency`` - currency code

Response:

- ``balance < integral, fraction >`` - integer and fractional part of the balance

GetCounters
------------

Request:

- without parameters

Response:

- ``counters < blocks, transactions >`` - total number of blocks and transactions

GetLastHash
-------------

Request:

- without parameters

Response:

- ``hash`` - hash of the last recorded block

GetBlocks
----------

Request:

- ``offset`` - offset from the last recorded block in the blockchain
- ``limit`` - number of records

Response:

- ``blocks`` - hash list

GetBlockSize
------------

Request:

- ``hash`` - block hash

Response:

- ``block_size`` - number of transactions in a block

GetTransactions
-----------------

Request:

- ``hash`` - block hash
- ``offset`` - offset
- ``limit`` - quantity

Response:

- ``Transactions`` - transaction hash list

GetTransaction
----------------

Request:

- ``b_hash`` - block hash
- ``t_hash`` - transaction hash

Response:

- ``Transaction`` - completed transaction structure

SendTransaction
-----------------

Request:

- ``Transaction`` - completed and signed transaction

Response:

- not applicable



Indices and tables
==================
* :ref:`genindex`
* :ref:`modindex`
* :ref:`search`

.. image:: _static/1.png
