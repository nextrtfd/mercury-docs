=========================================
ACH vs Wire Transfer — When to Use Which?
=========================================

For non-US developers, the US domestic transfer system can be confusing. From an API perspective, there are fundamental differences between ACH (Automated Clearing House) and Wire transfers.

Technical & Operational Comparison
==================================

.. list-table::
   :widths: 20 40 40
   :header-rows: 1

   * - Feature
     - ACH Transfer
     - Wire Transfer
   * - **Settlement Time**
     - 1-3 business days (Same-Day ACH is available but depends on cut-offs).
     - Real-time (usually settles within minutes or hours on the same day).
   * - **Reversibility**
     - **Reversible** within 60 days (e.g., for fraud or incorrect amounts).
     - **Final and non-reversible** (except for extremely rare bank technical errors).
   * - **Limits & Fees**
     - Usually free. Ideal for mass payouts and payroll.
     - Carries a fee (around $15-$30). Ideal for large or urgent transactions.
   * - **Cut-off Time**
     - Batch dependent (often around 1 PM EST).
     - Typically before 3 PM EST for same-day processing.

API Payload Comparison
======================

When making a request to ``POST /api/v1/account/{account_id}/transactions``, the main difference is the ``paymentMethod`` parameter.

**ACH Payload Example:**

.. code-block:: json

   {
     "recipientId": "rec_123456789",
     "amount": 500.00,
     "paymentMethod": "ach",
     "idempotencyKey": "ach-inv-001"
   }

**Wire Payload Example:**

.. code-block:: json

   {
     "recipientId": "rec_987654321",
     "amount": 50000.00,
     "paymentMethod": "wire",
     "idempotencyKey": "wire-inv-001"
   }

.. tip::
   Do not use Wire for micro-transactions, as the fees will eat into your margins. Always use an ``idempotencyKey`` to prevent duplicate transactions during network timeouts.

See Also
--------
* :doc:`mercury-sandbox-testing`
* :doc:`automating-invoice-payments`
