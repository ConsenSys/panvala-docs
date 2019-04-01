---
description: 'Regulators, mount up.'
---

# Gatekeeper

[Source](https://github.com/ConsenSys/panvala/blob/develop/governance-contracts/contracts/Gatekeeper.sol)

The Gatekeeper controls access to the TokenCapacitor \(and other resources\) through slate governance. Any attempt to send funds from the TokenCapacitor to a beneficiary must begin with a request for permission made to the Gatekeeper.

The Gatekeeper keeps track of the requests that have been made, and can be queried at any time for the status of a given request. Since the system is controlled by slate governance, a request is considered granted only if has been included in an accepted slate \(either through being unchallenged, or by winning a vote\). Once the request has been granted, the action can be executed, for example, tokens can be transferred from the TokenCapacitor to the specified beneficiary in the specified amount.

When a recommender puts together a grant proposal slate, they choose the proposals they think should be included, and make a request for permission for each of the proposals to the TokenCapacitor. This gives them a list of request IDs that they include when they call `Gatekeeper.recommendSlate()`. They need to include the category that they are submitting their slate for, since there can be multiple categories of slates.

Once a recommender has called `Gatekeeper.recommendSlate()`, their slate gets added to a pool for the current batch, for the appropriate category. In that case, it’s in the `Unstaked` state. People have the chance to stake on the slate through the staking period. Once it’s staked, it is added to a list of staked slates for that category. If no other slates in the category are staked by the end of the staking period, the staked slate is automatically accepted, and all its associated requests will have status granted.

## Get Current Batch Number

```javascript
function currentBatchNumber() public view returns(uint);
```

**Return value**: the number of the current batch, specified by `batchNumber`.

## Request Permission

```javascript
function requestPermission(bytes category, bytes metadataHash) public returns(uint);
```

Request permission to perform the action described in the metadataHash**.** Starts a poll. Called for each proposal that is associated with the slate.

Emits the following event:

```javascript
event PermissionRequested(uint requestID, bytes metadataHash);
```

**Return value**: the identifier of the request being made, specified by `requestID`.

## Recommend a Slate

```javascript
function recommendSlate(
    uint batchNumber,
    uint category,
    uint[] memory requestIDs,
    bytes memory metadataHash
) public returns (uint);
```

Creates a new slate with the associated requestIds and metadata hash.

Emits the following event:

```javascript
event SlateCreated(uint slateID, address indexed recommender, bytes metadataHash);
```

**Return value**: the identifier of the created slate, specified by `slateID`.

## Deposit Vote Tokens

```javascript
function depositVoteTokens(uint numTokens) public returns(bool);
```

Deposits tokens into the Gatekeeper to use in voting, specified as `voteTokenBalance[msg.sender]`. Assumes that `msg.sender` has approved the Gatekeeper to spend on their behalf.

Emits the following event:

```javascript
event SlateCreated(uint slateID, address indexed recommender, bytes metadataHash);
```

**Return value**: a boolean value indicating if the tokens were successfully transferred from the `msg.sender` to the Gatekeeper contract.

## Submit a Commitment for a Ballot

```javascript
function commitBallot(bytes32 commitHash, uint numTokens) public;
```

Submits a commitment for the current ballot.

* commit period must be active for the given epoch.
* if the voter doesn't have enough tokens for voting, deposit more and vote with a greater number of tokens.

Emits the following event:

```javascript
event BallotCommitted(uint ballotID, address indexed voter, uint numTokens, bytes32 commitHash);
```

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

