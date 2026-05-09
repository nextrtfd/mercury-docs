=================================
OAuth2 Flow for Multi-Tenant SaaS
=================================

Building a SaaS product (like accounting software) that connects to multiple clients' Mercury accounts requires implementing OAuth2.

Token Management per Tenant
===========================
1. **Authorization Code Grant:** Redirect the user to the Mercury login portal.
2. **Secure Token Storage:** Once you receive the ``access_token`` and ``refresh_token``, store them using asymmetric encryption in your database, mapped to the specific ``tenant_id``.

.. code-block:: sql

   -- Example Schema
   CREATE TABLE mercury_connections (
       tenant_id UUID PRIMARY KEY,
       mercury_account_id VARCHAR(255),
       encrypted_access_token TEXT,
       encrypted_refresh_token TEXT,
       expires_at TIMESTAMP
   );

Refresh Tokens & Edge Cases
===========================
* **Auto-Refresh:** Implement a cron job or middleware that monitors ``expires_at``. If the token expires in < 1 hour, trigger a token refresh automatically.
* **User Deactivation:** If a user revokes access via their Mercury dashboard, your API requests will return a ``401 Unauthorized`` error. Your system must detect this, mark the connection as ``disconnected`` in your database, and trigger an email prompting the user to re-authorize.

See Also
--------
* :doc:`api-token-security`
* :doc:`automating-invoice-payments`
