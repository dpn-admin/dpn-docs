Created by Bryan Hockey, last modified on Oct 01, 2013

Note: Bryan favors A.2.

###### Goals, Requirements, Assumptions

1.  On-demand scans are not part of normal auditing operations.
2.  Do not rely on trust+process audit.
3.  Minimize amount of state managed.
4.  Each replicating node must check the fixity of any given bag within
    24 months.
5.  Proposal A.1 and A.2 rely on there being some minimum time between
    **nonced** fixity checks, although nodes are always free to check
    their content against the registry.

 

#### Proposal A.1 (Centralized)

1.  First node completes successful transfer with replicating node.
2.  First node calculates j nonces and the associated fixities on
    the bag.
3.  Within 24 months, replicating node broadcasts a request for a nonce
    from the first node.
4.  First node sends them a nonce (no fixity).
5.  Replicating node calculates fixities on the bag using nonce.
6.  Replicating node sends fixities to the first node.
7.  First node examines fixities and:\
    1.  Detects a match. Then:\
        1.  First node acks the verification request. (This step not
            strictly necessary.)
        2.  First node broadcasts a registry update changing the
            last\_fixity\_date

    2.  Detects a mismatch.  Then:\
        1.  First node naks verification request.
        2.  Replicating node:\
            1.  Agrees its content is bad, then:\
                1.  Replicating node issues request for content.

            2.  States its content is correct, then:\
                1.  Confer with the federation for a quorum, update
                    content accordingly. (a separate flow)

###### Notes (A.1)

-   Nonces should never be re-used.
-   last\_fixity\_date is only updated by the first node.
-   Assumes that first nodes are active.
-   No additional state management required at replicating nodes.
-   The first node should calculate j = f(t,m,k) nonces+fixities. See
    math section below.
-   Nonces and their associated fixities are **not** held in the
    registry.  They are private at each node.
-   There is no requirement for how many nonces+fixities must be
    pre-calculated.  Instead, the first node makes the following
    guarantee:\
    -   **"I will be able to give you a nonce and check your results
        when you ask for it within your 24 month window."**
-   Problem: If the first node does not respond in step 4, these
    operations are blocked.\
    \

#### Proposal A.2 (Distributed)

1.  First node completes successful transfer with replicating nodes.
2.  Replicating nodes (which may include the first node) calculate
    nonces and the associated fixities on the bag.
3.  Within 24 months, replicating node \#1 broadcasts a request for
    a nonce.
4.  The other replicating nodes 2,3,4 respond with ack or nak.
5.  Replicating node \#1 chooses from the acks, e.g. chooses node \#2
    and requests a nonce.
6.  Replicating node \#2 responds with nonce.
7.  Replicating node \#1 calculates fixities on the bag using nonce.
8.  Replicating node \#1 sends fixities to node \#2.
9.  Node \#2 examines fixities and:\
    1.  Detects a match. Then:\
        1.  Node \#2 acks the verification request. (This step not
            strictly necessary.)
        2.  Node \#2 broadcasts a registry update changing the
            last\_fixity\_date

    2.  Detects a mismatch.  Then:\
        1.  Node \#2 naks verification request.
        2.  Replicating node (node \#1):\
            1.  Agrees its content is bad, then:\
                1.  Node \#1 issues request for content.

            2.  States its content is correct, then:\
                1.  Confer with the federation for a quorum, update
                    content accordingly. (a separate flow)

 

###### Notes (A.2)

-   Nonces should never be re-used.
-   last\_fixity\_date is updated only by replicating nodes (which may
    include the first node)
-   Does NOT assume first node is active or even online.
-   Requires all replicating nodes to manage nonces for each bag,
    instead of just the first node.
-   Selection in step 5 should likely be at random.
-   Each replicating node should calculate j = f(t,m,k-1)
    nonces+fixities.  See math section below.
-   Nonces and their associated fixities are **not** held in the
    registry.  They are private at each node.
-   There is no requirement for how many nonces+fixities must be
    pre-calculated.  Instead, each replicating node makes the following
    guarantee:\
    -   **"I will be able to give you a nonce and check your results
        when you ask for it within your 24 month window."**

 

#### Math!

<div>

j = total number of nonces pre-calculated

</div>

<div>

t = minimum time until we're out of nonces

</div>

<div>

k = number of replicating nodes

</div>

<div>

m = minimum time between nonced fixity requests per node

</div>

<div>

j = f(t,m,k) = round\_up\[ t / m \] \* k\
\

</div>

<div>

So for a goal of 20 years, 3 replicating nodes, and assuming the minimum
is 18 months:

</div>

<div>

j = round\_up\[ t / m \] \* k

</div>

<div>

j = round\_up\[240/18\] \* 3

</div>

<div>

j = 14 \* 3

</div>

<div>

j = 42 //This is the MAX number of nonces required given the minimums.

</div>

<div>

         // "For a period of 20 years with three replicating nodes and
fixity checks occurring at most every 18 months, a **total** of 42
nonces+fixities would be required."

</div>

<div>

A.1: The first node should calculate j = f(t,m,k) nonces+fixities. 

</div>

<div>

A.2: Each replicating node should calculate j = f(t,m,k-1)
nonces+fixities.

</div>

<div>

**Note:** j is a function of t.  If your node will be "visiting" a piece
of content every year or so, j is reduced significantly. 

</div>

<div>

E.g. two years:

</div>

<div>

j = f( 24, 18, 3)

</div>

<div>

j = round\_up( 24 / 18 ) \* 3

</div>

<div>

j = 2 \* 3

</div>

<div>

j = 6

</div>

 

 

 

 
