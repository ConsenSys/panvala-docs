---
description: 'Regulator, mount up!'
---

# Gatekeeper

[Source](https://github.com/ConsenSys/panvala/blob/develop/governance-contracts/contracts/Gatekeeper.sol).

The Gatekeeper controls access to the [TokenCapacitor](../tokencapacitor/) \(and other resources\) through slate governance. Any attempt to send funds from the TokenCapacitor to a beneficiary must begin with a request for permission made to the Gatekeeper.

The Gatekeeper keeps track of the requests that have been made, and can be queried at any time for the status of a given request. Since the system is controlled by slate governance, a request is considered granted only if has been included in an accepted slate \(either through being unchallenged, or by winning a vote\). Once the request has been granted, the action can be executed, for example, tokens can be transferred from the TokenCapacitor to the specified beneficiary in the specified amount.

When a recommender puts together a grant proposal slate, they choose the proposals they think should be included, and make a request for permission for each of the proposals to the TokenCapacitor. This gives them a list of request IDs that they include when they call [`Gatekeeper.recommendSlate`](transactions.md#recommend-a-slate). They need to include the category that they are submitting their slate for, since there can be multiple categories of slates.

Once a recommender has called `Gatekeeper.recommendSlate`, their slate gets added to a pool for the current batch, for the appropriate category. In that case, it’s in the `Unstaked` state. People have the chance to stake on the slate through the staking period. Once it’s staked, it is added to a list of staked slates for that category. If no other slates in the category are staked by the end of the staking period, the staked slate is automatically accepted, and all its associated requests will have status granted.

