Goals
-----

-   Specify process and definitions of DPN fixity


SLA language
------------
   
-   Node will perform bit auditing and Fixity Checks on each copy of Depositors’ Content no less than once every twenty-four (24) months, and will use reasonable efforts to replace corruption, errors and data loss of any of the Content by requesting an uncorrupted copy from another Replication Node in the Network.


Assertions
----------

-   The original conceptual design is
    -  Each Node will have implemented a fixity check workflow for their repository. 
    -  DPN places a floor requirement of once every 24 months on each object. 
    -  It is assumed that the Node repository has a robust fixity workflow in place.

-   Fixity type at initial release is SHA256
    -  No other fixity algorithm has been implemented in DPN-Server releases v1.x or v2.x

-   Fixity checks will
    -  Retrieve a copy of the stored bag and validate to verify all files are present and their checksums match the manifest;
    -  Calculate the checksum again on the tag manifest and send that back to the DPN registry via the fixity_check API endpoint. 

-   The tag manifest checksum is the only value that is validated against the registry.
