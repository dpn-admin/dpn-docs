Overview
========

Each Node will keep information regarding digital objects held by the
Digital Preservation Network in the "Registry". As part of DPN's mandate
is to separate implementation from specification, each node will
independently create their own set of services to support Registry
operations.  Each node will implement a registry independently, as such
it is only necessary for use to define which fields will be share across
the federation of nodes.  Registry entries for all objects will be held
by all nodes, thus the level of protection for object description is
very high. 

The registry, conceptually, is an implementation-agnostic way for Nodes
in the DPN Federation to keep metadata about DPN objects synchronized. 
The overall aim of the registry is to keep an easily accessible set of
metadata to allow for the effective management of DPN objects across the
Federation.  Discovery and rich metadata are out of scope for the DPN
registry at this time and proper management of objects across the
federation is the primary objective of the registry. 

The primary means of exchange for registry data between Nodes is via
messaging.

Registry Fields Design
======================

Registry Fields dpn\_object\_id: UUID local\_id: Bla-repository-specific
first\_node\_name: source repository replicating\_node\_names: \[hathi,
chron, sdr\] version\_number: number previous\_version\_object\_id: UUID
to previous version forward\_version\_object\_id: UUID to next version
or nil first\_version\_object\_id: UUID fixity\_algorithm: sha256
fixity\_value: some sha256 value last\_fixity\_date: ISO 8601
creation\_date: ISO 8601 last\_modified\_date: ISO 8601   bag\_size:
number of bytes of the serialized bag brightening\_object\_id: \[UUID1
... UUIDn\] rights\_object\_id: reference to DPN and repository
agreements \[UUID1 ... UUIDn\] object\_type: "data" | "rights" |
"brightening"

 

Versioning
==========

DPN Versioning
