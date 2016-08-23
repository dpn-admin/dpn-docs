This is a document going over client changes in order to be compatible with the api-v2 spec.

#### Model Updates

In api-v2 we have 3 models that have been modified, 1 that has been renamed, and 2 that are new. 
We'll cover the modified models first, then the new.

```
Discarded fields are prepended with a '-'
Added fields are prepended with a '+'

Naming is as followings:
field_name: Type
```

##### Bags

```
# Removed Fields
-fixities: Map[String, String]
```

##### Member

```
# Replaced Fields
-uuid: UUID
+member_id: UUID
```

##### Replication

```
# Replaced Fields
-uuid: UUID
+bag: UUID

# Removed Fields
-fixity_accept: Boolean
-bag_valid: Boolean
-status: char

# New Fields
+store_requested: Boolean  
+stored: Boolean  
+cancelled: Boolean  
+cancel_reason: String
```

##### Restore

```
# Replacements
-uuid: UUID 
+bag: UUID

# Removed Fields
-status: char

# New Fields
+accepted: Boolean
+finished: Boolean
+cancelled: Boolean
+cancelReason: String
```

##### Message Digest

This takes the place of the old 'fixities' model in the bag data structure. The relationship
between bags and the message digests that have is the same, with a bit more information on
the MessageDigest structure.

```
# Replacements
-digest: String  
+value: String  

# New Fields
+bag: String  
+node: String  
```

##### Fixity Check

A new model representing a fixity check operation done on a bag by a node

```
# New Fields
+bag: UUID
+node: String  
+fixityCheckId: UUID   
+success: Boolean  
+fixityAt: DateTime  
+createdAt: DateTime
```

##### Ingest

A new model representing the moment in time a bag is 'ingested' in DPN
(i.e. all replication requirements are met)
```
#New Fields
+bag: UUID  
+ingestId: UUID  
+ingested: Boolean  
+createdAt: DateTime  
+replicatingNodes: List[String]  
```

#### Endpoint Updates

Most of the endpoint updates are simply to facilitate crud operations on the new models,
so there isn't any in depth discussion except where needed.

* GET /api-v2/bag

The response object now includes the summation of the size of the bags queried, and looked like:
```
{
  "count": 2,
  "next": "http://localhost:8000/api_v1/bag/?page=3&page_size=2",
  "previous": "http://localhost:8000/api_v1/bag/?page=1&page_size=2",
  "total_size": 2526492640,
  "results": [
    ...
  ]
```

* GET /bag/{uuid}/digest
* POST /bag/{uuid}/digest
* GET /bag/{uuid}/digest/algorithm
* GET /api-v2/ingest
* POST /api-v2/ingest
* GET /api-v2/fixity_check
* POST /api-v2/fixity_check
* GET /api-v2/digest

#### Replication Updates

##### Chronopolis POV
*Note this has not gone under integration tests, just unit tests for the  preliminary changes I've made to my own flow* 

The changes to the replication model have had significant impacts on how the replication
flow works in the client. By removing the `bag_valid` and `status` fields, information was
lost which was used in order to determine which step to run. In order to gain some of this information
back, I created a simple data structure to assist in replications:
```
replicationId: String
pushed: Boolean
received: Boolean
extracted: Boolean
validated: Boolean
```

Depending on your needs these can likely be changed, but for the most part this gets
everything which we used to have. I tacked on a sqlite db and persist the objects in
order to recover in case I need to restart my client.

With these changes, all I needed to do was update where I queried the registry. Instead
of using `status` as a query parameter, I now use `store_requested`.
 
When `store_requested=false`, I know I have not yet done any operations prior to
calculating the fixity.
* rsync; when complete set `received=true` 
* fixity; calculate fixity on the tagmanifest and update the replication

When `store_requested=true`, I know I have all operations after the fixity is computed.
* untar; set `extracted=true` when finished
* bag validation; if not valid we cancel, else set `validated=true`
* push to repository; when this reports completion, set `pushed=true`. Could be renamed to
stored to fit more with the replication model.
* finally set `stored=true` on the replication object and update the remote registry

The only difference operationally is that we now actively cancel replications if they fail
on the client side (either fixity or validation). Before I left this to the server by setting
`fixity_valid=false` or `bag_valid=false`.
