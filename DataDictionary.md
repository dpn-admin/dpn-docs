Data Objects

|Name|Type|Description|
|----|----|------------|
|dpn_token|apiKey|DPN Authorization Token|
|Error|object|String of the encountered errors, possibly json formatted.|
|Timestamps|object|`created_at` - DPN-formatted date-time when this record was first created.<br> `updated_at` - DPN-formatted date-time when this record was last updated<br> ISO 8601 compliant <br><http://www.iso.org/iso/home/standards/iso8601.htm>|
|created_at|timestamp|DPN-formatted date-time when this record was first created. _(read only)_|
|updated_at|timestamp|DPN-formatted date-time when this record was last updated|
|ResultList|object|count - Count of total results<br>next - URL of next page, or null.<br> previous - URL of previous page, or null. <br>Note that null (correct) is not \"null\" (incorrect)|
|BagReference|string - UUID|bag - UUIDv4 of the associated bag|
|uuid|string|Unique UUIDv4 identifier for this bag. _(read only)_|
|local_id|string|Primary local identifier|
|size|integer64|Size of the bag in bytes. _(read only)_|
|first_version_uuid|string|UUID of the first version of this bag. _(read only)_|
|ingest_node|string|Namespace of the node that originally ingest this bag. _(read only)_|
|admin_node|string|Namespace of the node that has administrative rights to this bag|
|member|string|UUIDv4 of the member who \"owns\" this bag|
|version|integer|This bag's version number, beginning with 1. _(read only)_|
|bag_type|string|Single character specifying the type of this bag. <br> 'D'->Data, 'I'->Interpretive, 'R'->Rights|
|interpretive|array|Empty array or array of uuids of this bag's interpretive bags|
|rights|array|Empty array or array of uuids of this bag's rights bags|
|replicating_nodes|array|Empty array or array of namespaces of this bag's replicating nodes|
|total_size|integer32|Total size of bags from query|
|count|integer32|Count of total result set|
|next|string|URL of next page, or null. Note that null (correct) is not \"null\" (incorrect)|
|previous|string|URL of previous page, or null. Note that null (correct) is not \"null\" (incorrect)|
|algorithm|string|Algorithm used to calculate the value. _(read only)_|
|fixity_check_id|string|UUIDv4 that uniquely identifies this fixity check. _(read only)_|
|ingest_id|string|UUIDv4 that uniquely identifies this record. _(read only)_|
|ingested|boolean|Whether the ingested criteria was met or lost _(read only)_|
|replicating_nodes|array|The node namespaces of nodes that were storing the bag at the time this record was generated. Note that this list should include the state of the replicating nodes **after** the change. _(read only)_|
member_id, member|string|UUIDv4 that identifies this member. _(read only)_|
|name|string|The name the member goes by|
|namespace|string|Lowercase identifier to unambiguously reference this node|
|api_root|string|null or the root location of this node's server. This field MUST NOT be changed during normal operations; it should only change out of band|
|ssh_pubkey|string|null or the ssh public key of this node|
|replicate_from|array|Array of namespaces of the nodes that this node will replicate from|
|repliciate_to|array|Array of namespaces of the nodes that this node will replicate to|
|restore_from|array|Array of namespaces of the nodes that this node will restore from|
|restore_to|array|Array of namespaces of the nodes that this node will restore to|
|protocols|string|Array of currently supported transfer protocols|
|fixity_algorithms|string|Array of currently supported fixity algorithm|
|storage|string|Storage profile for this node|
|region|string|Region where bags are stored|
|type|string|Type of storage used|
|replication_id|string|UUIDv4 that uniquely identifies this replication transfer  request. _(read only)_|
from_node|string|Namespace of the node sending the bag. This is the node that generated this request. _(read only)_|
|to_node|string|Namespace of the node receiving the bag. Must not be a replicating_node for the bag being transferred|
|fixity_algorithm|string|Name of the fixity algorithm expected for the receipt. _(read only)_|
|fixity_nonce|string|null or the nonce to be used for verification. _(read only)_|
|fixity_value|string|null or the string of the fixity value calculated by the to_node after transferring the bag to its staging area.  _(Once set to a value, this field is read only.)_|
|protocol|string|Name of the transfer protocol. _(read only)_|
|link|string|URL of the bag to be transferred. _(read only)_|
|store_requested|boolean|Set by the from_node after it receives a correct fixity_value to instruct the to_node to complete the replication.  _(Once set to true, this field is read only.)_|
|stored|boolean|Set by the to_node to indicate the bag has been transferred into its storage repository from the staging area.  The to_node promises to fulfill replicating node duties by setting this field.  Must not be set before store_requested. _(Once set to true, this field is read only.)_|
cancelled|boolean|Set by either node to immediately end the transaction. _(Once set to true, the entire record is read-only)_|
|cancel_reason|string|Null or the reason for cancelling.  The cancel_reason must not be treated as an actionable field; it is only for debugging and analytics. Can only be set at the same time as cancelled.<br>enum": ["reject","bag_invalid","fixity_reject","other" ]|
|restore_id|string|UUIDv4 that uniquely identifies this restore transfer request. _(read only)_|
|accepted|boolean|Set by the from_node to indicate that it has accepted the request. _(Once set to true, this field is read only.)_|
|finished|boolean|Set by the to_node to indicate the transfer completed successfully.  _(Once set to true, the entire record is read-only.)_|
|local_id|string|Primary local identifier|
|size|integer64|Size of the bag in bytes|
|version|integer|This bag's version number, beginning with 1. _(read only)_|
|responses|string|200: success<br>201: record created<br>400: Bad Resource or Illegal Change<br>401: Authentication Failed<br>403: Authenticated, Not Authorized<br>404: Not found<br>409: Duplicate|
|`bag_uuid_path`|string|UUIDv4 of the target bag|