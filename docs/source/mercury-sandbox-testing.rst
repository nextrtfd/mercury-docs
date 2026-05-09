=======================================
Mercury Sandbox — Testing the Right Way
=======================================

Sandbox documentation often focuses heavily on the "happy path." Here is how to test failure scenarios and edge cases properly.

Simulating Failed Transactions / NSF (Non-Sufficient Funds)
===========================================================
In the Sandbox environment, you can trigger specific errors by using predefined "magic numbers" for the transfer amount or by using specific testing headers. Always verify that your system correctly processes the ``transaction.failed`` webhook.

Generating Test Recipients & Mock Webhooks
==========================================
1. **Test Recipient:** Use a valid test routing number (e.g., ``111000025`` for sandbox) combined with a random account number.
2. **Mock Webhook:** Use services like Webhook.site or Ngrok to catch webhooks locally. Register this URL in your Mercury Sandbox Dashboard.

Sandbox vs Production Behavior
==============================
* **Settlement Speed:** In the Sandbox, ACH transactions often switch to ``settled`` almost immediately to facilitate testing. In *Production*, the ``settled`` webhook will take 2-3 days to arrive. Your architecture must be fully asynchronous.
* **Compliance Checks:** Production environments have strict Anti-Money Laundering (AML) filters. Transactions to certain entities might get stuck in a ``pending_review`` state. Ensure your system gracefully handles this intermediate state.

See Also
--------
* :doc:`ach-vs-wire-transfer`
* :doc:`api-token-security`
