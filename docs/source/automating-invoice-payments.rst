============================================
Automating Invoice Payments with Mercury API
============================================

This is the most common use case: An invoice is approved, and the system automatically pays it.

End-to-End Workflow
===================
1. **Receipt & Validation:** An invoice is uploaded, and the finance team approves it in your system (or a tool like Airtable).
2. **Trigger ACH Payment:** Your backend calls the Mercury API with a unique ``idempotencyKey`` (e.g., the invoice number: ``INV-2023-089``).
3. **Webhook Confirmation:** Mercury sends a ``transaction.created`` webhook. Once the funds clear days later, Mercury sends ``transaction.settled``. Your system then updates the invoice status to ``PAID``.

Integrating a "Simple Ledger" (Airtable / Notion)
=================================================
If you aren't using a complex database yet, Airtable can serve as a lightweight ledger:

1. Create an ``Invoices`` table in Airtable with status fields: ``Pending``, ``Processing``, and ``Paid``.
2. Use Zapier, Make.com, or a custom Node.js/Python script to poll the Airtable data periodically.
3. When a status changes to ``Processing``, the script executes the Mercury transfer and writes the returned ``Transaction ID`` back to the Airtable record.

Handling Idempotency (Preventing Double Payments)
=================================================
This is **mandatory**. Always include the ``idempotencyKey`` in your header or JSON payload.

.. code-block:: bash

   curl -X POST https://api.mercury.com/api/v1/account/.../transactions \
     -H "Authorization: Bearer $MERCURY_TOKEN" \
     -H "Idempotency-Key: invoice-payment-4492-attempt-1" \
     -d '{"amount": 1000, "paymentMethod": "ach", "recipientId": "..."}'

If a request fails mid-flight due to a network drop, you can safely retry the exact same ``curl`` request. Mercury will recognize it as the same transaction and guarantee that the account is not debited twice.

See Also
--------
* :doc:`ach-vs-wire-transfer`
* :doc:`mercury-sandbox-testing`
