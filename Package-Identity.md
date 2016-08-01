Specification
=============

### Node specific Unique Identifier and Standard RFC 4122 Universal Unique Identifier

Option 3 entails creating an object identifier by using two fields in
the registry:

Field 1: standard [RFC
4122](http://tools.ietf.org/html/rfc4122){.external-link} v4 UUID.

Field 2: a Node generated UUID. 

These two fields support an identifier mechanism that is both world UUID
immutable/opaque and mappable to Node specific identifier mechanisms.
This supports a completely transparent Node succession event with
identifiers that will not change, and identifiers that may need new
versions. This also preserves the mapping of DPN UUID to node specific
UUID as part of the distributed registry. 

**Field 1:** [RFC
4122](http://tools.ietf.org/html/rfc4122){.external-link} v4 UUID.

<http://www.ietf.org/rfc/rfc4122.txt>

RFC 4122 defines a Uniform Resource Name namespace for UUIDs
(Universally Unique IDentifier), also known as GUIDs (Globally Unique
IDentifier).  A UUID is 128 bits long, and requires no central
registration process. Generating RFC 4122 v4 compliant UUID's is
relatively easy and is supported in multiple languages via standard
libraries.

For DPN we will use a human readable form using octets.

    hexOctet = hexDigit hexDigit
          hexDigit =
                "0" / "1" / "2" / "3" / "4" / "5" / "6" / "7" / "8" / "9" /
                "a" / "b" / "c" / "d" / "e" / "f" /
                "A" / "B" / "C" / "D" / "E" / "F"

    A pure Identifier looks like this:

    f81d4fae-7dec-11d0-a765-00a0c91e6bf6

We may consider using a prefix to indicate that UUIDs are resources.
 For example: ***urn:uuid:***

This UUID is completely opaque and world usable, i.e. should not have
any collisions with other RFC 4122 v4 generated UUID's. This has an
advantage for DPN as DPN UUID's are expected to last at least 50-100
years (or at least till there is a technology refresh). These UUIDs
also survive Node succession events and can be used for discovery
and administration functions  across all Nodes and Registries.
Additionally, the standard format is easily parsed and is small in size,
within DPN and without. As new Nodes are added, standard approaches can
be used within local registries to manipulate and utilize these UUIDs.
Their is a very low likelihood of UUID collision. 

**Field 2:** Node UUID.

Each Node may use its own internal UUID in Field 2. This has several
advantages for the Node as it may be used in the same manner following
the Nodes current practices. We expect the format of this UUID to be
different across the Nodes within the DPN consortium, and that after a
succession event the UUID may or may not change, or be versioned. The
primary advantage is for the convenience of the First Node and will
reduce its administrative burden with regards to the archiving of
content as the Node UUID will support the Nodes current model for naming
and allow for better preservation functions. 

Registry entries map via a standards based UUID to the Node/domain
specific UUID and each site can use which ever UUID best suits their
needs internally, or within the DPN consortium.

 