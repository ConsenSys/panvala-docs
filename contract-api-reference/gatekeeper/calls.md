---
description: free
---

# Calls

## Get Current Batch Number

```javascript
function currentBatchNumber() public view returns(uint);
```

**Return value**: the number of the current batch, specified by `batchNumber`.

## Check if a Voter Committed in a Ballot

```javascript
function didCommit(uint ballotID, address voter) public view returns(bool);
```

**Return value**: a boolean value indicating if the voter has committed for a given ballot.

## Get Commit Hash

```javascript
function getCommitHash(uint ballotID, address voter) public view returns(bytes32);
```

Gets the commit hash for a given voter and ballot.

* reverts if voter has not committed yet.

**Return value**: the hash of the committed vote.

