====================================================
API Token Security — Often Overlooked Best Practices
====================================================

A leaked token is a nightmare scenario. If your app is compromised and the leaked token has ``payment-enabled`` access, corporate funds can be drained in seconds.

Principle of Least-Privilege
============================
Always isolate tokens based on their required function:

* **Read-only Tokens:** Use these for dashboards, reporting, and data reconciliation. If compromised, an attacker can only view transaction history, not move money.
* **Payment-enabled Tokens:** Store these *strictly* in an isolated, secure backend environment (e.g., AWS KMS, HashiCorp Vault). Only inject them into the specific worker services that process payment queues.

Zero-Downtime Token Rotation
============================
1. Generate a new token (Token B) in the Mercury dashboard.
2. Update the environment variables on your servers from Token A to Token B.
3. Deploy / Restart your application.
4. Wait 24 hours to ensure all background jobs and queues using Token A have completed.
5. Safely delete Token A from the Mercury dashboard.

Legal & Compliance Risks
========================
If a token is leaked and fraud occurs, banks (including Mercury/Evolve Bank & Trust) will often refuse to reimburse lost funds if developer negligence is proven. You could also face compliance penalties and unilateral account freezes.

See Also
--------
* :doc:`oauth2-multi-tenant-saas`
* :doc:`mercury-sandbox-testing`
