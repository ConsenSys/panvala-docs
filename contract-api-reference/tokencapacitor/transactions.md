---
description: moo la la la la la
---

# Transactions

## Create a Proposal

```javascript
function createProposal(address to, uint tokens, bytes metadataHash) public;
```

Creates a proposal to send the given amount of tokens to the given address, with details about the request in the `metadataHash`.

Emits the following event:

```javascript
event ProposalCreated(uint proposalID, address proposer, address to, uint tokens, bytes metadataHash);
```

**Return value**: the identifier of the request being made, specified by `requestID`.

