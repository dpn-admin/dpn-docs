Attendees
---------

-   Dan Galewsky
-   James Simon
-   Bryan Hockey
-   Michael Ritter
-   Sebastian Korner
-   Scott Turnbull (note taker)

What follows is a summary of the tech team discussion around fixity
checking requirements within DPN and what reporting requirements might
be required as a result.

Goals
-----

-   (James) Wants conversation based on Fixity Issues.
-   (Bryan/Scott)  Establish frequency requirements of Audit.
-   (Dan)  Wants to establish business goals.\
    -   Bit Level Integrity
    -   Fixity checking
-   (Sebastian)  Understanding of what factors are we going to take
    into consideration.

Discussion
----------

-   Take On Demand Fixity checking off the table. (James)\
    -   Scott Second this.
    -   Bryan/James Off the table as a primary means of fixity but would
        be special case driven.
-   Question about what to confirm.
-   Glacier represents a problem in that it limits what information you
    can get back from the service.  Also the service is opaque.
-   We want to be able meet a minimum frequency of fixity, 20 months is
    acceptable for long term fixity in DPN\
    -   Is 24 a possibility?
    -   Need this cost to be manageable
    -   Given glacier policy can pull out 5% of content a month so this
        should allow for a fixity check every 20 months.
-   Should we wait until it's in our preservation space before sending a
    registry update?\
    -   It's up to the first node when to send an ack of receipt.
    -   Can a node wait a long time to allow for the content to be
        preserved and at rest before.  The implication is that this
        could really increase the response time on content
        copy functions.
    -   As long as it all happens within the TTL it shouldn't be a
        problem and should be left to the discretion of the nodes.
-   What do we actually report back for fixity to the other nodes in the
    federation.\
    -   Can we send a nonce to include in fixity checks.
    -   This would require keeping the nonce in the registry and wait
        until this has been sent out.
    -   Conversation about nonces that went back too and forth too much
        for me to keep track of.
    -   First node is the authoritative source for information on
        it's content.
    -   How to we sync up our fixity cycles?
    -   Question of how to report back to nodes, what is the
        requirement? \
        -   How much does the method we're requiring limit how a node
            can store a bag?
-   Concern about non-replication first nodes.\
    -   Our current assertions imply heavy coordination of reporting by
        the first node, but how does this jive with non-replication
        first nodes.
    -   After succession events how does this get coordinated.

Assertions
----------

-   Scheduled based fixity instead of fixity on demand for regular
    fixity.\
    -   Schedule and criteria meets an agreed-ed on SLA.
-   On demand fixity should be an option in special cases.\
    -   Should be a charge-back to the originator of the request.
-   Long timeline for regular fixity is acceptable 20+ month should be
    fine (\~24 months)\
    -   Policy should be flexible enough to allow for a broad range of
        storage options.  (Support Heterogeneity)
-   Question on Nonces or Process audit for confirmation of content
    fixity over time?\
    -   Unresolved this time.  
-   Need to resolve model and algorithm to use for fixity.
    -   What exactly are we checking,
    -   What reporting requirements are there.

 
