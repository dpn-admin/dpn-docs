This document is a draft working document and should be considered in
process until the Threat Model strike team asks for specific feedback.

**Contributors:**  Scott Turnbull, Tom Cramer, Christopher Jordan, James Simon

Introduction
============

This document attempts to define a Threat model for DPN, along with
Threats to the DPN preservation model and security threats to the
application(s), messaging infrastructure and systems of DPN.  As a
reminder, DPN represents a system for long term preservation that uses a
federated infrastructure for the storage of digital objects, and a
messaging infrastructure that supports the functions of DPN via messages
and message control flows. DPN is comprised of Nodes and message brokers
connected via a robust Internet 2 infrastructure. There is an attempt to
identify effective mitigation strategies for the identified risks. This
model combines [Requirements for Digital Preservation
Systems](http://arxiv.org/pdf/cs.DL/0509018.pdf) approach with
application decomposition and dataflow diagram analysis from the
[Simplified Implementation of the Microsoft
SDL](http://www.microsoft.com/en-us/download/details.aspx?id=12379).

High Level Assumptions - DPN as an architecture and a system of long term preservation
--------------------------------------------------------------------------------------

1.  Nodes will get compromised
2.  Brokers will be compromised
3.  Networks will be compromised
4.  Any potential attacker will have lots of attack vectors
5.  We trust, but verify all transactions from nodes.  Especially with
    regards to updates, or recovery operations.

#### Assumptions regarding Node level Security {#assumptions-regarding-node-level-security .p1}

DPN is a federated network of secure and trustworthy institutions that
have secure digital repositories. As such, we assume that each node has
institutional safeguards that are well documented. DPN should assume
(with verification) that each node follows the current best practice
regarding:

-   Network Communications
-   Firewalls and Zone level security\
    -   screening IP traffic
    -   Denial of Service attacks
    -   eavesdropping (of institution network)
    -   Point to point routing, for example IPtables
    -   intrusion detection
    -   etc.
-   Use of VPN's for access by users (in general environment)
-   Password level security for users, e.g. SAML,
    Shibboleth, Kerberos, X509 Cert., etc.
-   Secured transport layers to services, including SSL, SSH, etc.
-   Security that supports the institutional use of and compliance of
    HIPPA data, FERPA data, and other restricted data. 

Security Objectives
-------------------

The primary goal of security for DPN is to protect DPN content from
being compromised.  To accomplish this, DPN must protect systems and
services and security that enforces:

-   Confidentiality
-   <span
    style="font-size: 10.0pt;line-height: 13.0pt;">Integrity
-   <span
    style="font-size: 10.0pt;line-height: 13.0pt;">Availability
-   <span style="font-size: 10.0pt;line-height: 13.0pt;">Non-repudiation
    of DPN transactions.

Federated Network Overview
--------------------------

DPN is a federated network of independent institutions, in addition,
each institution has its own implementation of DPN protocol and
services. Thus DPN is a loosely coupled environment that uses messaging
to affect change and facilitate content replication between nodes within
the federation.  

Of specific interest with regards to threats to the federation, the
following are of special interest:

-   Software Failure - Messaging broker and its environment.
-   Communication Errors - Node to node communication,
    messaging failures/attacks.
-   Failure of Network Services - specific to the operation of
    the DPN infrastructure.
-   Software Obsolescence - Implementation of DPN functions,
    broker services.
-   Operator Error - As DPN uses a messaging environment, it is
    important that DPN secure messages and protocols.
-   External Attack - We must assume that Nodes/networks will be
    compromised and that DPN has some mechanism to detect and protect
    against these attacks. 
-   Internal Attack - As independent entity's each institution must
    protect itself from an internal attack, It is assumed that each node
    will follow best practices.
-   Economic Failure , and Organizational Failure - The reason for DPN
    is to protect and preserve in the event of these two possibilities
    for any individual node. 

### Roles

Roles represent general system privilege roles anticipated in the
application architecture, they should help identify risks and inform
security best practices.  These should be referenced where necessary in
Threat Scenarios below.  In addition to a general list of privileges
attributed to the role list any important explicitly denied privileges
for that role.

**Role:** Message Queue Agent

Used for connecting to message brokers in the AMQP DPN federated network
to consume and produce control flow messages.

-   **Privileges:**  publish, consume, get, declare exchange/queue, bind
    on federated message queue.
-   **Denied Privileges:**  unbind, delete exchange/queue, any manage or
    web UI management privileges.

**Role:** *DPN Package Replication\
*

*Responsible for movement of packages between DPN replicating nodes.\
*

-   **Privileges:**  *basic list of privileges*
-   **Denied Privileges:**  *important explicit denied privileges.*

### Use Scenarios

Message queue

Federated Network Decomposition
-------------------------------

### Protocols

-   AMQP
-   ZIP or TAR:  Used as part of the BagIt Specification.
-   Rsync and/or HTTPS:  Used for File transfers.

#### Software

-   RabbitMQ is used at each node as a message broker.
-   RabbitMQ Federation Plug-in use instead of broker clustering.
-   Other specific software is specific to each node's implementation.

#### Application Development Environment(s)

Currently the five nodes of DPN develop software independently, using:

-   Ruby 1.9
-   Java?
-   PHP?
-   Python 2.7

Each of these represent the potential for obsolescence and possibly
incompatibilities between systems.  

### Federation

### Data Flows

Known connection and communication scenarios.

-   Broker to Broker communications between nodes.
-   File transfer protocols between nodes.

Diagram of high level federated network dataflows to help engineers
understand the points of connections and trust boundaries between
members of the Federation..

### Trust Boundaries

-   Each individual node AMPQ broker.
-   Each individual node File Transfer port.

Assumptions
-----------

-   Each node will be responsible for the security of its selected
    technologies though our threat model assumes that any node may be
    compromised (see High Level Assumptions above).
-   Each node is encouraged to develop a Threat Model of their own.

Threat Scenarios
----------------

Media Failure,
Hardware Failure, Software Failure, Communication Errors, Failure of
Network Services, Media & Hardware Obsolescence, Software Obsolescence,
Operator Error, Natural Disaster, External Attack, Internal Attack,
Economic Failure, Organizational Failure,

Note the general
bullet structure below is as follows:

Overall Category
of Threat

Threat
scenario

-   Mitigation
    for threat

### Security Threats

Message Injection
-

A malicious user
compromises the Federated Messaging Network and injects a malicious
script into the payload of a message that causes the receiving node to
execute it. 

-   Messages need
    to be validated for format to ensure there’s not wrapped code
    contained inside.

-   Message
    validation should include features to prevent
    injection attacks.

-   Message
    processing should be sandboxed from the rest of the
    nodes features.

-   Implement
    effective network controls (need better clarity)

A malicious user
injects a malformed message into the system that causes the receiving
node to crash or otherwise fail in an unpredictable way.

-   Messages need
    to be validated for format to ensure there’s not wrapped code
    contained inside.

-   Message
    validation should include features to prevent
    injection attacks.

-   Message
    processing should be sandboxed from the rest of the
    nodes features.

-   Application
    updates should be effectively managed to limit known
    security vulnerabilities.

-   The
    configuration should limit buffer overrun vulnerabilities and
    frame overruns.

-   The
    configuration should provide flood control mechanisms to mitigate
    denial of service attacks intentional or otherwise.

A node is
compromised and starts injecting valid messages at a high rate, without
responding. 

-   Implement
    flood control detection mechanisms on the broker to prevent flooding
    of the MQ.

-   Implement
    rate limits in the Federation to allow for proper alerts and
    isolation of problem nodes or brokers.

A malicious user
injects a valid message into the Federation that causes an action in the
receiving node that is not appropriate for the transaction.

-   Messages need
    to be validated for format to ensure there’s not wrapped code
    contained inside.

-   ?? Something
    about end to end verification between nodes.

-   ?? Something
    about ensuring proper sequencing of messages that is only
    understandable by participating nodes.

A malicious user
intercepts messages during transactions and gains security information
or hijacks the control flow operation.

-   Ensure
    implementations don’t include any security information in
    the message.

-   Ensure
    transactions and security information is context specific between
    nodes for that transaction to prevent reuse.

Broker Compromise
- 

A malicious user
compromises the Message Broker at a particular Node itself and modifies
the message broker to gain access to the node.

-   Brokers
    should be sandboxed from other components in the DPN node.

-   Ensure
    components of the DPN architecture have isolated permissions so
    something executed as the message broker user can’t effect items in
    other parts of the architecture (like file management).

Broker
eavesdropping - 

-   An attacker
    gains access to a broker through a compromised node and listens to
    ALL traffic. 

    -   Each
        broker should include signed messages or connections to ensure
        end to end integrity of delivery.\
        
-   ??

Node Compromise
-

A malicious user
compromises the hosting system of a node and alters or acts on the node
content to damage it or inject further security vulnerabilities into the
Federation.

-   Nodes should
    confirm checksums on digital objects before operating on them to
    ensure they have not been modified.

Network
Interception - 

Malicious users
have modified bags in transit to inject malicious code.

-   Nodes should
    not act or operate on bags they have received before confirming the
    integrity of the object with the originating node.

Registry
Compromise -

-   A malicious
    user compromises the registry of a node and alters data in such a
    way to compromise management of content in the
    preservation Federation.

Protocol
Vulnerabilities -

An attacker
compromises the protocol and causes incorrect/malicious information to
be injected into the registry, or cause improper bags to be
copied.

A known protocol
vulnerability is discovered in the community and published.

-   Node
    engineers should periodically review protocol notifications to
    identify when a vulnerability has been discovered and apply
    appropriate patches as necessary.

<!-- -->

-   Network
    eavesdropping -
    -   A site
        does not have sufficient security protections and foreign
        entities can monitor traffic

Internal
Attack

A malicious
superuser at a node acts to destroy content or data in the node.

-   User
    permissions for replicating nodes should be kept granular enough
    that malicious individuals can be locked out of a system quickly to
    prevent further damage or compromise to the federation.

-   Copies of the
    registry should be current across all nodes so they can be restored
    and multiple copies of bags across replicating nodes should be
    available to restore content at the compromised node once the action
    has been detected.

A malicious
administrator or superuser opens a backdoor into the system so it can be
compromised or accessed insecurely in secret.

-   ??
    NOTE: 
     Probably need some feedback by someone with more system admin
    skills here.  I would think something in the neighborhood of low
    level logging, auditing and port scanning would be what we
    could target.

### Operational Threats

Control Flow
Failure\


Failure of Bag
Copy Services results in insufficient redundancy of preservation storage
for individual bags.

-   Periodic
    audit of bags throughout the DPN network should confirm registry
    data about those items and raise alerts if inadequate redundancy
    is discovered.
-   Control flow
    specification should include fallback mechanisms to deal with
    content if an inadequate number of nodes meet the replication
    criteria.\
    

An improperly
formed message is produced by a member of the Federation that causes a
the receiving node to to crash, throw an error or produce a critical
failure in the workflow or system.

-   Nodes should
    carefully validate message formats and fail gracefully if they
    receive an improperly formatted message.

-   Information
    about improperly formatted messages should be logged and alerts
    raised quickly so the originating nodes can be notified.

-   The
    Federation of nodes need an effective emergency response policy to
    respond effectively to critical operational errors.

File Corruption
-

A bag is
corrupted in transfer and results in an unrestorable file or a
differential between preserved files.

-   Originating
    nodes must always confirm the integrity of bags after they are
    delivered and before they are processed by the receiving node to
    affirm they have not been corrupted.

Fixity audits are
insufficient infrequency to uncover bit rot at individual nodes to
ensure redundant copies at other nodes also have integrity.

-   ??
    NOTE: 
     Likely the mitigation here needs to be some minimum acceptable
    audit frequency of items at each node based on average bitrot rates.
     Anyone have more information on this?

Format
Obsolescence

Serialization of
digital objects in a bag depend on 3rd party software for restoration
that may expire or no longer be available in the future.

-   First Nodes
    should manage preserved content, conducting format migrations as
    needed to maintain the viability of files as they are preserved.
      ??
    NOTE: 
     Check in on this.. is this in the purview of DPN?  Is format
    migration a part of DPN overall?  Seems like a First Node thing but
    what about the scenario I’m hearing about “pass through” items
    directly to DPN or the idea I’m hearing of “contributing nodes” that
    might only push content into DPN but not manage it?

-   Media formats
    used for digital objects are no longer viable or available rendering
    content unusable on modern systems.

Archival format
used to create a preservation bag is no longer supported and contents
are unable to be access or acted upon for further preservation
action.

-   DPN policy
    should exist for the annual review of Protocols and Standards used
    specifically for DPN to identify format or
    protocol obsolescence.

Network
Failure

-   Significant
    disruption of internet communications result in nodes being unable
    to communicate, rendering them unable to confirm integrity of
    content and registries between nodes.

    -   First
        nodes should be able to continue their own functions during
        network distributions and have the ability to gracefully clear
        backlogs of DPN replication once network services
        are restored.
    -   A
        contingency plan for long term distributions should be created
        that considers the physical transfer of vital information when
        needed to re-establish. \
        
-   Significant
    disruption of internet communications result in nodes being unable
    to access their data or content stored in cloud based providers or
    through cloud services.  

    -   The Core
        DPN architecture between different nodes should remain diverse
        enough not to depend on a single cloud infrastructure or all use
        cloud based approach.  If Nodes reach this state the governing
        board should recruit replicating node partners to as to
        sufficiently increase the diversity of architecture.

Hardware
Failure

The physical
storage devices used for preserve content suffer a catastrophic failure,
resulting in the loss or corruption of content.

-   Individual
    nodes should have an adequate emergency response policy for hardware
    failures to ensure the quick restoration.

Authorization
Failure

An unauthorized
user or attacker gains access to a node and compromises shared secret
information that would authorize it for action at other nodes in the
Federation.  (i.e. they get a hold of ssh keys to other nodes and
such)

-   Key
    authorization to other nodes should be based on public
    keys only.

-   Effectively
    secured certificates should exist for all keys used for
    secure connections.

-   The system
    architecture should have a means for 3rd party independent key
    verification for secure keys in the Federation.

Workflow
Failure

-   Registry
    Updates issued from a node do not accurately reflect activities that
    took place on other participating nodes resulting in out of sync or
    incorrect registry information.
    -   Any data
        about content at a specific node should be verified by that
        node.\
        

Operational
Security

-   Conflicts or
    misalignment between node service levels render them unable to
    effectively respond to security or operational threats.

    -   Have a
        clear service level agreement between nodes in the federation
        that identifies how responses should be conducted for DPN
        related concerns.

-   Environments
    are not properly updated with security patches as flaws are exposed,
    allowing the system to be compromised through
    published vulnerabilities.
    -   Each node
        should have an active update and patching policy which should be
        reviewed by the DPN Federation to confirm security practices in
        a security audit.\
        

Obsolescence of
Environment

-   Language
    versions, security practices, framework versions and operating
    systems are updated regularly or are obsolete.\
    

Operational
Sustainability

-   A node is
    unable to maintain the staffing and skills needed for proper
    security, reliable operations and maintenance of services as a
    DPN node.
    -   Each node
        should maintain proper documentation and a training plan for new
        staff to enable them to properly operation that nodes
        DPN architecture.
    -   DPN
        should retain copies of important operational documentation and
        periodically review ongoing staffing commitments from each node
        involved.\
        

Governance
Failure ?? Probably need feedback on this from other groups.\


-   A node's
    governing structure, for causes political or otherwise, is altered
    in such a way as to be directly opposed to DPN's purposes. There is
    no reason to expect it to return to a normal status.
-   The
    collaborative relationship between replicating nodes breaks down to
    the point where communications are not proactive or in good-faith
    enough to identify and address operational or security concerns.\
    

Economic Failure
?? Probably need feedback on this form for other groups.\


-   Economic
    pressures leave nodes unable to accept more content or manage the
    content they have leaving an inadequate number of minimal
    replicating nodes. \
    

Group Sign Off and Review {#group-sign-off-and-review style="text-decoration: none;"}
---------------------------------------------------------------------------------------

Indicate your
name and check the box if you've nothing more to add to this and
approve.

2 complete 


6 complete James Simon

4 incomplete  (I
have reviewed, but see comment below)\


 

####Tom Cramer

This format for a threat model is new to me, and one I'm interested to
see how it pans out.

There is also some literature and a good number of extant examples for
digital preservation threat models, that deal with the threats to
digital information over time. See...

-   <http://www.dlib.org/dlib/september12/vermaaten/09vermaaten.html>\
    (check out the bibliography and appendix, in particular)
-   <http://arxiv.org/pdf/cs.DL/0509018.pdf>

From the latter:

**Media Failure** All storage media must be expected to degrade with
time, causing irrecoverable bit errors, and to be subject to sudden
catastrophic irrecoverable loss of bulk data such as disk crashes \[67\]
or loss of oﬀ-line media \[49\].

**Hardware Failure** All hardware components must be expected to suﬀer
transient recoverable failures, such as power loss, and catastrophic
irrecoverable failures, such as burnt-out power supplies.

**Software Failure** All software components must be expected to suﬀer
from bugs that pose a risk to the stored data.

**Communication Errors** Systems cannot assume that the network
transfers they use to ingest or disseminate content will either succeed
or fail within a speciﬁed time period, or will actually deliver the
content unaltered. A recent study “suggests that between one (data)
packet in every 16 million packets and one packet in 10 billion packets
will have an undetected checksum error” \[66\].

**Failure of Network Services** Systems must anticipate that the
external network services they use, including resolvers such as those
for domain names \[38\] and persistent URLs \[43\], will suﬀer both
transient and irrecoverable failures both of the network services and of
individual entries in them. As examples, domain names will vanish3 or be
reassigned if the registrant fails to pay the registrar, and a
persistent URL will fail to resolve if the resolver service fails to
preserve its data with as much care as the digital preservation service.

**Media & Hardware Obsolescence** All media and hardware components will
eventually fail. Before that, they may become obsolete in the sense of
no longer being capable of communicating with other system components or
being replaced when they do fail. This problem is particularly acute for
removable media, which have a long history of remaining theoretically
readable if only a suitable reader could be found \[27\].

**Software Obsolescence** Similarly, software components will become
obsolete. This will often be manifested as format obsolescence when,
although the bits in which some data was encoded remain accessible, the
information can no longer be decoded from the storage format into a
legible form.

**Operator Error** Operator actions must be expected to include both
recoverable and irrecoverable errors. This applies not merely to the
digital preservation application itself, but also to the operating
system on which it is running, the other applications sharing the same
environment, the hardware underlying them, and the network through which
they communicate.

**Natural Disaster** Natural disasters, such as ﬂood \[37\], ﬁre and
earthquake must be anticipated. They will typically be manifested by
other types of threat, such as media, hardware and infrastructure
failures.

**External Attack** Paper libraries and archives are subject to
malicious attack \[70\]; there is no reason to expect their digital
equivalents to be exempt. Worse, all systems connected to public
networks are vulnerable to viruses and worms. Digital preservation
systems must either defend against the inevitable attacks, or be be
completely isolated from external networks.\
\
**Internal Attack** Much abuse of computer systems involves insiders,
those who have or used to have authorized access to the system \[26\].
Even if a digital preservation system is completely isolated from
external networks, it must anticipate insider abuse.\
\
**Economic Failure** Information in digital form is much more vulnerable
to interruptions in the money supply than information on paper. There
are ongoing costs for power, cooling, bandwidth, system administration,
domain registration, and so on. Budgets for digital preservation must be
expected to vary up and down, possibly even to zero, over time.

**Organizational Failure** The system view of digital preservation must
include not merely the technology but the organization in which it is
embedded. These organizations may die out, perhaps through bankruptcy,
or change missions. This may deprive the digital preservation technology
of the support it needs to survive. System planning must envisage the
possibility of the asset represented by the preserved content being
transferred to a successor organization, or otherwise being properly
disposed of.

To me DPN seems like it must focus on

-   external attack
-   internal attack
-   operator error
-   economic failure
-   organizational failure
-   network / communication errors

And we are relying on First Nodes and Successor Nodes to deal with

-   format / software obsolescence

And all nodes individually combat

-   media failure
-   hardware failure
-   software failure
-   media & software obsolescence
-   &lt;internal&gt; communication errors



#### [Scott Turnbull]


Thanks for posting that, it's very helpful but I'm not familiar with it
so I'll look it over. The only two forms I was familiar with were the
Microsoft SDL Model
(<http://www.microsoft.com/security/sdl/adopt/threatmodeling.aspx>) that
I thought James asked us to use today or the OWASP model I was
interested in trying
(<https://www.owasp.org/index.php/Threat_Risk_Modeling>). Neither is a
great fit in a distributed system like ours but

The model you linked is more useful now but give me a day to mix in some
elements from the models I'm more use to. A dataflow diagram with access
boundaries may be helpful as well as a few of the other methods. In
particular STRIDE
(<http://msdn.microsoft.com/en-us/library/ee823878(v=cs.20).aspx>) might
be useful for classification and DREAD
(<http://en.wikipedia.org/wiki/DREAD:_Risk_assessment_model>) useful for
assessment.

Threat modeling as I'm familiar with it is a pretty extensive thing
though so I'm having trouble figuring how to get to the right size for
us.

#### [Sebastien Korner]


This is a great start on an important and **massive** topic. I couldn't
check a box saying I had nothing more to add, but on the other hand if I
had a week I'm sure I, and most others, could spend that week working on
this. I'm not sure what the middle ground is.

Chris, do you feel this document has enough detail now to have a
productive conversation with, for example, your local security folks who
have offered their assistance to DPN?

A possibility for ensuring the document stays current is to require the
sub-teams that take on responsibility for designing components of DPN
consider it part of their charge to update the threat model page with
entries relevant to what they are designing. This also ensures that the
DPN folks most intimately aware of the design vulnerabilities of each
component are channeling that knowledge here.
