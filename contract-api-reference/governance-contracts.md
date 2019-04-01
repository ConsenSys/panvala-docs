---
description: 'Regulators, mount up.'
---

# Gatekeeper

[Source](https://github.com/ConsenSys/panvala/blob/develop/governance-contracts/contracts/Gatekeeper.sol)

The Gatekeeper controls access to the TokenCapacitor \(and other resources\) through slate governance. Any attempt to send funds from the TokenCapacitor to a beneficiary must begin with a request for permission made to the Gatekeeper.

The Gatekeeper keeps track of the requests that have been made, and can be queried at any time for the status of a given request. Since the system is controlled by slate governance, a request is considered granted only if has been included in an accepted slate \(either through being unchallenged, or by winning a vote\). Once the request has been granted, the action can be executed, for example, tokens can be transferred from the TokenCapacitor to the specified beneficiary in the specified amount.

## Get Current Batch Number

```text
function currentBatchNumber() public view returns(uint);
```

**Return value**: the number of the current batch.

## Request Permission

```
function requestPermission(bytes category, bytes metadataHash) public returns(uint);
```

Request permission to perform the action described in the metadataHash**.**

Emits the following event:

```text
event PermissionRequested(uint requestID, bytes metadataHash);
```

**Return value**: the identifier of the request being made, specified by `requestID`.

