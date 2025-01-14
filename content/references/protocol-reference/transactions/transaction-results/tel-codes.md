---
html: tel-codes.html
parent: transaction-results.html
blurb: tel codes indicate an error in the local server processing the transaction.
labels:
  - Transaction Sending
---
# tel Codes

These codes indicate an error in the local server processing the transaction; it is possible that another server with a different configuration or load level could process the transaction successfully. They have numerical values in the range -399 to -300. The exact code for any given error is subject to change, so don't rely on it.

**Caution:** Transactions with `tel` codes are not applied to ledgers and cannot cause any changes to the XRP Ledger state. However, these transactions may be automatically cached and retried later. Transactions that provisionally failed may still succeed or fail with a different code after being reapplied. For more information, see [Finality of Results](finality-of-results.html) and [Reliable Transaction Submission](reliable-transaction-submission.html).

| Code            | Explanation |
|:----------------|:------------|
| `telBAD_DOMAIN` | The transaction specified a domain value (for example, the `Domain` field of an [AccountSet transaction][]) that cannot be used, probably because it is too long to store in the ledger. |
| `telBAD_PATH_COUNT` | The transaction contains too many paths for the local server to process. |
| `telBAD_PUBLIC_KEY` | The transaction specified a public key value (for example, as the `MessageKey` field of an [AccountSet transaction][]) that cannot be used, probably because it is not the right length. |
| `telCAN_NOT_QUEUE` | The transaction did not meet the [open ledger cost](transaction-cost.html), but this server did not queue this transaction because it did not meet the [queuing restrictions](transaction-queue.html#queuing-restrictions). For example, a transaction returns this code when the sender already has 10 other transactions in the queue. You can try again later or sign and submit a replacement transaction with a higher transaction cost in the `Fee` field. |
| `telCAN_NOT_QUEUE_BALANCE` | The transaction did not meet the [open ledger cost](transaction-cost.html) and also was not added to the transaction queue because the sum of potential XRP costs of already-queued transactions is greater than the expected balance of the account. You can try again later, or try submitting to a different server. |
| `telCAN_NOT_QUEUE_BLOCKS` | The transaction did not meet the [open ledger cost](transaction-cost.html) and also was not added to the transaction queue. This transaction could not replace an existing transaction in the queue because it would block already-queued transactions from the same sender. (For details, see [Queuing Restrictions](transaction-queue.html#queuing-restrictions).) You can try again later, or try submitting to a different server. |
| `telCAN_NOT_QUEUE_BLOCKED` | The transaction did not meet the [open ledger cost](transaction-cost.html) and also was not added to the transaction queue because a transaction queued ahead of it from the same sender blocks it. (For details, see [Queuing Restrictions](transaction-queue.html#queuing-restrictions).) You can try again later, or try submitting to a different server. |
| `telCAN_NOT_QUEUE_FEE` | The transaction did not meet the [open ledger cost](transaction-cost.html) and also was not added to the transaction queue. This code occurs when a transaction with the same sender and sequence number already exists in the queue and the new one does not pay a large enough transaction cost to replace the existing transaction. To replace a transaction in the queue, the new transaction must have a `Fee` value that is at least 25% more, as measured in [fee levels](transaction-cost.html#fee-levels). You can increase the `Fee` and try again, send this with a higher `Sequence` number so it doesn't replace an existing transaction, or try sending to another server. |
| `telCAN_NOT_QUEUE_FULL` | The transaction did not meet the [open ledger cost](transaction-cost.html) and the server did not queue this transaction because this server's transaction queue is full. You could increase the `Fee` and try again, try again later, or try submitting to a different server. The new transaction must have a higher transaction cost, as measured in [fee levels](transaction-cost.html#fee-levels), than the transaction in the queue with the smallest transaction cost. |
| `telFAILED_PROCESSING` | An unspecified error occurred when processing the transaction. |
| `telINSUF_FEE_P` | The `Fee` from the transaction is not high enough to meet the server's current [transaction cost](transaction-cost.html) requirement, which is derived from its load level and network-level requirements. If the individual server is too busy to process your transaction right now, it may cache the transaction and automatically retry later. |
| `telLOCAL_ERROR` | Unspecified local error. The transaction may be able to succeed if you submit it to a different server. |
| `telNETWORK_ID_MAKES_TX_NON_CANONICAL` | The transaction specifies the [`NetworkID` field](transaction-common-fields.html#networkid-field), but the current network rules require that the `NetworkID` field be omitted. (Mainnet and other networks with a chain ID of 1024 or less do not use this field.) If the transaction was intended for a network that does not use `NetworkID`, remove the field and try again. If the transaction was intended for a different network, submit it to a server that is connected to the correct network. [New in: rippled 1.11.0][] |
| `telNO_DST`_`PARTIAL` | The transaction is an XRP payment that would fund a new account, but the [`tfPartialPayment` flag](partial-payments.html) was enabled. This is disallowed. |
| `telREQUIRES_NETWORK_ID` | The transaction does not specify a [`NetworkID` field](transaction-common-fields.html#networkid-field), but the current network requires one. If the transaction was intended for a network that requires `NetworkID`, add the field and try again. If the transaction was intended for a different network, submit it to a server that is connected to the correct network. [New in: rippled 1.11.0][] |
| `telWRONG_NETWORK` | The transaction specifies the wrong [`NetworkID` value](transaction-common-fields.html#networkid-field) for the current network. Either specify the correct the `NetworkID` value for the intended network, or submit the transaction to a server that is connected to the correct network. [New in: rippled 1.11.0][] |

<!--{# common link defs #}-->
{% include '_snippets/rippled-api-links.md' %}
{% include '_snippets/tx-type-links.md' %}
{% include '_snippets/rippled_versions.md' %}
