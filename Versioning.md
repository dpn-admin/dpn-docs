This version was agreed to on May 1, 2013.

Assumptions
-----------

1.  DPN manages and tracks "bags of files" which are mostly opaque to
    DPN Replicating nodes except for "top level" metadata which is
    readable by all nodes
2.  There is a one-to-one relationship between bags and registry entries
3.  Deletion of existing bags or removal or modification of any files
    contained in an already existing bag will not be supported by what
    is being termed "DPN Versioning"\
    1.  Versioning content will require the creation of a new bag, and
        new registry entry.

4.  The process by which the latest (or any) version of an object can be
    reconstituted from its previous versions will be detailed in the
    object's associated Brightening information
5.  DPN First Nodes will have a versioning implementation that will
    support the representation described here

Goals
-----

1.  The DPN versioning design should be ~~simple~~ transparent for DPN Replicating
    nodes and should put the responsibility for versioning content on
    the First Nodes
2.  The DPN versioning design should support differential versioning (at
    the granularity of whole files) so that First Nodes have the
    flexibility to manage the versions the way they want\
    1.  Everything from changing/adding/removing one file to updating
        every file, from one version to the next

3.  Bag versions should be able to be used to reconstitute the state of
    a digital object at any particular moment in time to support
    succession and restoration goals.

Basic Design
------------

1.  DPN registry entries will maintain a doubly linked list of versions\
    1.  A registry entry will contain a reference to a previous version
        (if any) and a reference to a subsequent version (if any)

2.  Bags will contain a reference to the previous version of the bag
    (if any) in the top level metadata of the bag
3.  Bags will also contain a reference to first version (if bag is
    subsequent version)
4.  Bag references will be the unique ID of a bag. 
5.  Brightening information for a given bag will detail the process for
    reconstituting any given version of that bag. 

Basic Scenario
--------------

1.  A First Node submits an initial version of a bag of files (aka, the
    root bag)
2.  To version content (add, modify, remove files), a NEW bag is created
    that references previous versions (see basic design)
    1.  No changes are made to existing bags

3.  The current contents of the bag (or any version of it) can be
    recreated by using the description/information held in the
    Brightening Object. 

 

------------------------------------------------------------------------

 

Implications and Usage Notes for each node
------------------------------------------

 

#### APTrust

-   This approach seems fine for us but it should be noted we are only
    just starting to design our content models and our versioning policy
    is under development.
-   APTrust currently has no content but expects to begin accepting
    content by the end of the year.
-   APTrust preserves snapshots of client repositories and does not
    override versioning policies from the original partner.
-   We gather metadata about the contents of Fedora objects being
    preserved such that we should be able to produce differential bags
    for DPN.
-   Digital Objects that come from DSpace institutions come in a zip
    format already so any updated item from a DSpace partner will result
    in an entire replacement of an AIP in any updated DPN bag.

#### Chronopolis

The versioning proposal outlined above is satisfactory to Chronopolis.
In particular we DO NOT change bag content in any way once it has been
ingested.  We are in general agreement with HathiTrust that there might
be times that replacing content is appropriate. We have seen requests
like this in the past for purely economic reasons. However, it's not
clear if we support "replaceable flag" solution. Given the description
below, it sounds like most of the Hathi collection would be tagged as
"replaceable." 

FYI for about the last year now in Chronopolis we have been appending a
timestamp in the form of YEAR-MO-DA to the bag names given to us by data
depositors.  That is at ingest the bag name is modified so that the bag
name used throughout our process becomes depositor-bagname\_YEAR-MO-DA. 
This gives us some low level versioning capability.

#### HathiTrust

The versioning proposal outlined above is satisfactory to HT. The
content replacement conversation has been moved, reflecting the
consensus that it is a separate issue.

#### SDR

SDR supports differential versioning and does not support replacement as
a policy, although it has happened in the past, going forward this
proposal supports our needs. 

#### UTDR

No objections to the versioning proposal, UTDR supports it.  We also
support replacement/deletion in general and would be fine making it a
separate discussion.

 
#### [Don Sutton asks]

Bag naming question - Data depositor submits bag named
collectionA. Then has an updated version and submits as
collectionA Do we rename
somehow?

Is the Brightening Object part of Bagit
profile? A file at root level? Format?


#### [James Simon]

Don,

The "Data depositor" will work with a First node, and it will be the
First node that puts content into DPN. Therefore using our DPN bagit
spec means that there will be a name that is local to the First node and
a DPN UUID, both in the bag. So in this case, if the depositor is a
First node, then the local name will be "collectionA" and the First node
will mint a DPN UUID. A new version will require the creation of a new
bag with a new UUID and local ID. The new version bag will have a
pointer to the previous version. No renaming done.

A Brightening Object is a DPN object with its own local name and DPN
UUID (it can also be versioned). It will follow the same format that
other DPN bags follow. Hopefully the Brightening bag, under the /data
directory, is easy to understand since it holds the key to brightening
for more complex non-administrative bags.


#### [Andrew Woods]

Addressing Don's question about two bags having the same name:
"collectionA". The short answer is that the top-level bag name will
always be a DPN UUID, not a depositor-defined name or otherwise.
Subsequent versions of a bag will be their own first-class object and
will have their own UUID, and therefore, the bag names will also be
different across versions.
