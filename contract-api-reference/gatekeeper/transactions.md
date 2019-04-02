---
description: state-changing
---

# Transactions

## Request Permission

```javascript
function requestPermission(bytes category, bytes metadataHash) public returns(uint);
```

Requests permission to perform the action described in the metadataHash**.**

* starts a poll.
* called for each proposal that is associated with the slate.

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

Creates a new slate with the associated requestIds and metadata hash \(stored in IPFS\).

Emits the following event:

```javascript
event SlateCreated(uint slateID, address indexed recommender, bytes metadataHash);
```

**Return value**: the identifier of the created slate, specified by `slateID`.

## Deposit Vote Tokens

```javascript
function depositVoteTokens(uint numTokens) public returns(bool);
```

Deposits tokens into the Gatekeeper to use in voting, specified as `voteTokenBalance[msg.sender]`.

* assumes that `msg.sender` has approved the Gatekeeper to spend on their behalf.

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

## Stake Tokens

```javascript
function stakeTokens(uint slateID) public payable returns(bool);
```

## Accept a Slate

```javascript
function acceptSlate(uint batchNumber, uint contest, uint slateID) public;
```

## Reveal a Ballot

```javascript
function revealBallot(uint batchNumber) public;
```

## Start Runoff

```javascript
function startRunoff(uint batchNumber, uint contest) public;
```

## Finalize Voting Results

```javascript
function finalizeResult(uint batchNumber, uint contest) public;
```

## Create a new Category

```javascript
function createCategory(address to, uint tokens, bytes metadataHash) public;
```



