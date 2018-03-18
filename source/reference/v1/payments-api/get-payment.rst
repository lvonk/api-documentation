.. _v1/payments-get:

Payments API v1: Get payment
============================
.. warning:: This is the documentation of the v1 API. The documentation for retrieving payments in the new v2 API can be
             found :ref:`here <v2/payments-get>`. For more information on the v2 API, refer to our
             :ref:`v2 migration guide <migrate-to-v2>`.

``GET`` ``https://api.mollie.com/v1/payments/*id*``

Authentication: :ref:`API keys <guides/authentication>`. :ref:`OAuth access tokens <oauth/overview>`

Retrieve a single payment object by its payment token.

.. note:: We call your webhook when the :ref:`payment status changes <guides/payment-status-changes>`, so there's no
          need to poll this endpoint for status changes.

Parameters
----------
Replace ``id`` in the endpoint URL by the payment's ID, for example ``tr_7UhSN1zuXS``.

Mollie Connect/OAuth parameters
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
If you are creating an app with Mollie Connect (OAuth), the ``testmode`` parameter is available. You must pass this as a
parameter in the query string if you want to retrieve a payment that was created in test mode.

.. list-table::
   :header-rows: 0
   :widths: auto

   * - | ``testmode``
       | boolean
     - Set this to ``true`` to get a payment made in test mode. If you omit this parameter, you can only retrieve live
       mode payments.

Includes
^^^^^^^^
This endpoint allows you to include additional information by appending the following values via the ``include``
querystring parameter.

* ``settlement`` Include the settlement this payment belongs to, when available.
* ``details.qrCode`` Include a :ref:`QR code <guides/qr-codes>` object. Only available for iDEAL, Bitcoin, Bancontact
  and bank transfer payments.

Response
--------
``200`` ``application/json; charset=utf-8``

.. list-table::
   :header-rows: 0
   :widths: auto

   * - | ``resource``
       | string
     - Indicates the response contains a payment object. Will always contain ``payment`` for this endpoint.

   * - | ``id``
       | string
     - The identifier uniquely referring to this payment. Mollie assigns this identifier at payment creation time. For
       example ``tr_7UhSN1zuXS``. Its ID will always be used by Mollie to refer to a certain payment.

   * - | ``mode``
       | string
     - The mode used to create this payment. Mode determines whether a payment is *real* (live mode) or a *test*
       payment.

       Possible values: ``live`` ``test``

   * - | ``createdDatetime``
       | datetime
     - The payment's date and time of creation, in `ISO 8601 <https://en.wikipedia.org/wiki/ISO_8601>`_ format.

   * - | ``status``
       | string
     - The payment's status. Please refer to the documentation regarding statuses for more info about which statuses
       occur at what point.

   * - | ``canBeCancelled``
       | boolean
     - Optional – Whether or not the payment can be cancelled.

   * - | ``paidDatetime``
       | datetime
     - Optional – The date and time the payment became paid, in `ISO 8601 <https://en.wikipedia.org/wiki/ISO_8601>`_
       format. This parameter is omitted if the payment isn't completed (yet).

   * - | ``cancelledDatetime``
       | datetime
     - Optional – The date and time the payment was cancelled, in `ISO 8601 <https://en.wikipedia.org/wiki/ISO_8601>`_
       format. This parameter is omitted if the payment isn't cancelled (yet).

   * - | ``expiredDatetime``
       | datetime
     - Optional – The date and time the payment was expired, in `ISO 8601 <https://en.wikipedia.org/wiki/ISO_8601>`_
       format. This parameter is omitted if the payment did not expire (yet).

   * - | ``expiryPeriod``
       | duration
     - Optional – The time until the payment will expire in
       `ISO 8601 duration <https://en.wikipedia.org/wiki/ISO_8601#Durations>`_ format.

   * - | ``failedDatetime``
       | datetime
     - Optional – The date and time the payment failed, in `ISO 8601 <https://en.wikipedia.org/wiki/ISO_8601>`_ format.
       This parameter is omitted if the payment did not fail (yet).

   * - | ``amount``
       | decimal
     - The amount in EURO.

   * - | ``amountRefunded``
       | decimal
     - Optional - The total amount that is already refunded. Only available when refunds are available for this payment.
       For some payment methods, this amount may be higher than the payment amount, for example to allow reimbursement
       of the costs for a return shipment to the customer.

   * - | ``amountRemaining``
       | decimal
     - Optional - The remaining amount that can be refunded. Only available when refunds are available for this payment.

   * - | ``description``
       | string
     - A short description of the payment. The description is visible in the Dashboard and will be shown on the
       customer's bank or card statement when possible.

   * - | ``method``
       | string
     - The payment method used for this payment, either forced on creation by specifying the ``method`` parameter, or 
       chosen by the customer on our payment method selection screen.

       If the payment is only partially paid with a gift card, the method remains ``giftcard``.

       Possible values: ``banktransfer`` ``belfius`` ``bitcoin`` ``creditcard`` ``directdebit`` ``giftcard`` ``ideal``
       ``inghomepay`` ``kbc`` ``mistercash`` ``paypal`` ``paysafecard`` ``sofort``

   * - | ``metadata``
       | object
     - The optional metadata you provided upon payment creation. Metadata can be used to link an order to a payment.

   * - | ``locale``
       | string
     - Optional – The customer's locale, either forced on creation by specifying the ``locale`` parameter, or detected
       by us during checkout. Will be a full locale, for example ``nl_NL``.

   * - | ``countryCode``
       | string
     - Optional – The customer's `ISO 3166-1 alpha-2 <https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2>`_ country code,
       detected by us during checkout. For example: ``BE``.

   * - | ``profileId``
       | string
     - The identifier referring to the profile this payment was created on. For example, ``pfl_QkEhN94Ba``.

   * - | ``settlementId``
       | string
     - Optional – The identifier referring to the settlement this payment was settled with. For example,
       ``stl_BkEjN2eBb``.

   * - | ``customerId``
       | string
     - Optional - If a customer was specified upon payment creation, the customer's token will be available here as
       well. For example, ``cst_XPn78q9CfT``.

   * - | ``recurringType``
       | string
     - Optional - This field indicates the position of the payment in a recurring stream. Refer to the
       :ref:`recurring payments guide <guides/recurring>` for more information.

       Possible values: ``null`` ``first`` ``recurring``

   * - | ``mandateId``
       | string
     - Optional - If the payment is a recurring payment, this field will hold the ID of the mandate used to authorize
       the recurring payment.

   * - | ``subscriptionId``
       | string
     - Optional – When implementing the Subscriptions API, any recurring charges resulting from the subscription will
       hold the ID of the subscription that triggered the payment.

   * - | ``issuer``
       | string
     - Optional - Only available for payment methods that use an issuer, e.g. iDEAL, KBC/CBC payment button and gift
       cards. Holds the ID of the issuer that was used during the payment.

   * - | ``failureReason``
       | string
     - Optional - Only available for failed Bancontact and credit card payments. Contains a failure reason code.

       Possible values: ``invalid_card_number`` ``invalid_cvv`` ``invalid_card_holder_name`` ``card_expired``
       ``invalid_card_type`` ``refused_by_issuer`` ``insufficient_funds`` ``inactive_card``

   * - | ``applicationFee``
       | object
     - Optional – The application fee, if the payment was created with one.

       .. list-table::
          :header-rows: 0
          :widths: auto

          * - | ``amount``
              | decimal
            - The application fee amount in EUR as specified during payment creation.

          * - | ``description``
              | string
            - The description of the application fee as specified during payment creation.

   * - | ``links``
       | object
     - An object with several URLs important to the payment process.

       .. list-table::
          :header-rows: 0
          :widths: auto

          * - | ``paymentUrl``
              | string
            - Optional - The URL your customer should visit to make the payment. This is where you should redirect the
              consumer to. Make sure you redirect using the HTTP ``GET`` method.

              Note the URL will not be present for recurring payments.

          * - | ``webhookUrl``
              | string
            - The URL Mollie will call as soon an important status change takes place.

          * - | ``redirectUrl``
              | string
            - The URL the customer will be redirected to after completing or cancelling the payment process.

              Note the URL will not be present for recurring payments.

          * - | ``settlement``
              | string
            - The API resource URL of the settlement this payment belongs to.

          * - | ``refunds``
              | string
            - The API resource URL of the refunds that belong to this payment.

          * - | ``chargebacks``
              | string
            - The API resource URL of the chargebacks that belong to this payment.

Payment method specific parameters
----------------------------------
If you specify the ``method`` parameter, optional parameters may be available for the payment method. If no method is
specified, you can still send the optional parameters and we will apply them when the consumer selects the relevant
payment method.

Bancontact
^^^^^^^^^^
.. list-table::
   :header-rows: 0
   :widths: auto

   * - | ``details``
       | object
     - Optional – An object with payment details.

       .. list-table::
          :header-rows: 0
          :widths: auto

          * - | ``cardNumber``
              | string
            - Only available if the payment is completed - The last four digits of the card number.

          * - | ``cardFingerprint``
              | string
            - Only available if the payment is completed - Unique alphanumeric representation of card, usable for
              identifying returning customers.

          * - | ``qrCode``
              | object
            - Only available if requested during payment creation - The QR code that can be scanned by the mobile
              Bancontact application. This enables the desktop to mobile feature.

Bank transfer
^^^^^^^^^^^^^
.. list-table::
   :header-rows: 0
   :widths: auto

   * - | ``details``
       | object
     - Optional – An object with payment details.

       .. list-table::
          :header-rows: 0
          :widths: auto

          * - | ``bankName``
              | string
            - The name of the bank the consumer should wire the amount to.

          * - | ``bankAccount``
              | string
            - The IBAN the consumer should wire the amount to.

          * - | ``bankBic``
              | string
            - The BIC of the bank the consumer should wire the amount to.

          * - | ``transferReference``
              | string
            - The reference the consumer should use when wiring the amount. Note you should not apply any formatting
              here; show it to the consumer as-is.

          * - | ``consumerName``
              | string
            - Only available if the payment has been completed – The consumer's name.

          * - | ``consumerAccount``
              | string
            - Only available if the payment has been completed – The consumer's bank account. This may be an IBAN, or it
              may be a domestic account number.

          * - | ``consumerBic``
              | string
            - Only available if the payment has been completed – The consumer's bank's BIC / SWIFT code.

          * - | ``billingEmail``
              | string
            - Only available if filled out in the API or by the consumer – The email address which the consumer asked
              the payment instructions to be sent to.

Belfius Pay Button
^^^^^^^^^^^^^^^^^^
.. list-table::
   :header-rows: 0
   :widths: auto

   * - | ``details``
       | object
     - Optional – An object with payment details.

       .. list-table::
          :header-rows: 0
          :widths: auto

          * - | ``consumerName``
              | string
            - Only available one banking day after the payment has been completed – The consumer's name.

          * - | ``consumerAccount``
              | string
            - Only available one banking day after the payment has been completed – The consumer's bank account. This
              may be an IBAN, or it may be a domestic account number.

          * - | ``consumerBic``
              | string
            - Only available one banking day after the payment has been completed – ``GKCCBEBB``.

Bitcoin
^^^^^^^
.. list-table::
   :header-rows: 0
   :widths: auto

   * - | ``details``
       | object
     - Optional – An object with payment details.

       .. list-table::
          :header-rows: 0
          :widths: auto

          * - | ``bitcoinAddress``
              | string
            - Only available if the payment has been completed – The bitcoin address the bitcoins were transferred to.

          * - | ``bitcoinAmount``
              | object
            - The amount transferred in BTC.

          * - | ``bitcoinUri``
              | string
            - Optional - An URI that is understood by Bitcoin wallet clients and will cause such clients to prepare the
              transaction. Follows the
              `BIP 21 URI scheme <https://github.com/bitcoin/bips/blob/master/bip-0021.mediawiki>`_.

          * - | ``qrCode``
              | object
            - Only available if requested during payment creation - The QR code that can be scanned by Bitcoin wallet
              clients and will cause such clients to prepare the transaction.

Credit card
^^^^^^^^^^^
.. list-table::
   :header-rows: 0
   :widths: auto

   * - | ``details``
       | object
     - Optional – An object with payment details.

       .. list-table::
          :header-rows: 0
          :widths: auto

          * - | ``cardHolder``
              | string
            - Only available if the payment has been completed - The card holder's name.

          * - | ``cardNumber``
              | string
            - Only available if the payment has been completed - The last four digits of the card number.

          * - | ``cardFingerprint``
              | string
            - Only available if the payment has been completed - Unique alphanumeric representation of card, usable for
              identifying returning customers.

          * - | ``cardAudience``
              | string
            - Only available if the payment has been completed and if the data is available - The card's target
              audience.

              Possible values: ``consumer`` ``business`` ``null``

          * - | ``cardLabel``
              | string
            - Only available if the payment has been completed - The card's label. Note that not all labels can be
              processed through Mollie.

              Possible values: ``American Express`` ``Carta Si`` ``Carte Bleue`` ``Dankort`` ``Diners Club``
              ``Discover`` ``JCB Laser`` ``Maestro`` ``Mastercard`` ``Unionpay`` ``Visa`` ``null``

          * - | ``cardCountryCode``
              | string
            - Only available if the payment has been completed - The
              `ISO 3166-1 alpha-2 <https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2>`_ country code of the country the
              card was issued in. For example: ``BE``.

          * - | ``cardSecurity``
              | string
            - Only available if the payment has been completed – The type of security used during payment processing.

              Possible values: ``normal`` ``3dsecure``

          * - | ``feeRegion``
              | string
            - Only available if the payment has been completed – The fee region for the payment: ``intra-eu`` for
              consumer cards from the EU, and ``other`` for all other cards.

              Possible values: ``intra-eu`` ``other``

Gift cards
^^^^^^^^^^
.. list-table::
   :header-rows: 0
   :widths: auto

   * - | ``details``
       | object
     - Optional – An object with payment details.

       .. list-table::
          :header-rows: 0
          :widths: auto

          * - | ``voucherNumber``
              | string
            - The voucher number, with the last four digits masked. When multiple gift cards are used, this is the first
              voucher number. Example: ``606436353088147****``.

          * - | ``giftcards``
              | array
            - A list of details of all giftcards that are used for this payment. Each object will contain the following
              properties.

              .. list-table::
                 :header-rows: 0
                 :widths: auto

                 * - | ``issuer``
                     | string
                   - The ID of the gift card brand that was used during the payment.

                 * - | ``amount``
                     | decimal
                   - The amount in EURO that was paid with this gift card.

                 * - | ``voucherNumber``
                     | string
                   - The voucher number, with the last four digits masked. Example: ``606436353088147****``

          * - | ``remainderAmount``
              | decimal
            - Only available if another payment method was used to pay the remainder amount – The amount in EURO that
              was paid with another payment method for the remainder amount.

          * - | ``remainderMethod``
              | string
            - Only available if another payment method was used to pay the remainder amount – The payment method that
              was used to pay the remainder amount.

iDEAL
^^^^^
.. list-table::
   :header-rows: 0
   :widths: auto

   * - | ``details``
       | object
     - Optional – An object with payment details.

       .. list-table::
          :header-rows: 0
          :widths: auto

          * - | ``consumerName``
              | string
            - Only available if the payment has been completed – The consumer's name.

          * - | ``consumerAccount``
              | string
            - Only available if the payment has been completed – The consumer's IBAN.

          * - | ``consumerBic``
              | string
            - Only available if the payment has been completed – The consumer's bank's BIC.

ING Home'Pay
^^^^^^^^^^^^
.. list-table::
   :header-rows: 0
   :widths: auto

   * - | ``details``
       | object
     - Optional – An object with payment details.

       .. list-table::
          :header-rows: 0
          :widths: auto

          * - | ``consumerName``
              | string
            - Only available one banking day after the payment has been completed – The consumer's name.

          * - | ``consumerAccount``
              | string
            - Only available one banking day after the payment has been completed – The consumer's IBAN.

          * - | ``consumerBic``
              | string
            - Only available one banking day after the payment has been completed – ``BBRUBEBB``.

KBC/CBC Payment Button
^^^^^^^^^^^^^^^^^^^^^^
.. list-table::
   :header-rows: 0
   :widths: auto

   * - | ``details``
       | object
     - Optional – An object with payment details.

       .. list-table::
          :header-rows: 0
          :widths: auto

          * - | ``consumerName``
              | string
            - Only available one banking day after the payment has been completed – The consumer's name.

          * - | ``consumerAccount``
              | string
            - Only available one banking day after the payment has been completed – The consumer's IBAN.

          * - | ``consumerBic``
              | string
            - Only available one banking day after the payment has been completed – The consumer's bank's BIC.

PayPal
^^^^^^
.. list-table::
   :header-rows: 0
   :widths: auto

   * - | ``details``
       | object
     - An object with payment details.

       .. list-table::
          :header-rows: 0
          :widths: auto

          * - | ``consumerName``
              | string
            - Only available if the payment has been completed – The consumer's first and last name.

          * - | ``consumerAccount``
              | string
            - Only available if the payment has been completed – The consumer's email address.

          * - | ``paypalReference``
              | string
            - PayPal's reference for the transaction, for instance ``9AL35361CF606152E``.

paysafecard
^^^^^^^^^^^
.. list-table::
   :header-rows: 0
   :widths: auto

   * - | ``details``
       | object
     - An object with payment details.

       .. list-table::
          :header-rows: 0
          :widths: auto

          * - | ``consumerName``
              | string
            - The consumer identification supplied when the payment was created.

SEPA Direct Debit
^^^^^^^^^^^^^^^^^
.. list-table::
   :header-rows: 0
   :widths: auto

   * - | ``details``
       | object
     - An object with payment details.

       .. list-table::
          :header-rows: 0
          :widths: auto

          * - | ``transferReference``
              | string
            - Transfer reference used by Mollie to identify this payment.

          * - | ``creditorIdentifier``
              | string
            - The creditor identifier indicates who is authorized to execute the payment. In this case, it is a
              reference to Mollie.

          * - | ``consumerName``
              | string
            - Optional – The consumer's name.

          * - | ``consumerAccount``
              | string
            - Optional – The consumer's IBAN.

          * - | ``consumerBic``
              | string
            - Optional – The consumer's bank's BIC.

          * - | ``dueDate``
              | date
            - Estimated date the payment is debited from the consumer's bank account, in ``YYYY-MM-DD`` format.

          * - | ``signatureDate``
              | date
            - Only available if the payment has been verified – Date the payment has been signed by the consumer, in
              ``YYYY-MM-DD`` format.

          * - | ``bankReasonCode``
              | string
            - Only available if the payment has failed – The official reason why this payment has failed. A detailed
              description of each reason is available on the website of the European Payments Council.

          * - | ``bankReason``
              | string
            - Only available if the payment has failed – A textual desciption of the failure reason.

          * - | ``endToEndIdentifier``
              | string
            - Only available for batch transactions – The original end-to-end identifier that you've specified in your
              batch.

          * - | ``mandateReference``
              | string
            - Only available for batch transactions – The original mandate reference that you've specified in your
              batch.

          * - | ``batchReference``
              | string
            - Only available for batch transactions – The original batch reference that you've specified in your batch.

          * - | ``fileReference``
              | string
            - Only available for batch transactions – The original file reference that you've specified in your batch.

SOFORT Banking
^^^^^^^^^^^^^^
.. list-table::
   :header-rows: 0
   :widths: auto

   * - | ``details``
       | object
     - An object with payment details.

       .. list-table::
          :header-rows: 0
          :widths: auto

          * - | ``consumerName``
              | string
            - Only available if the payment has been completed – The consumer's name.

          * - | ``consumerAccount``
              | string
            - Only available if the payment has been completed – The consumer's IBAN.

          * - | ``consumerBic``
              | string
            - Only available if the payment has been completed – The consumer's bank's BIC.

QR codes (optional)
-------------------
A QR code object with payment method specific values is available for certain payment methods if you pass the include
``details.qrCode`` to the resource endpoint.

The ``qrCode`` key in the ``details`` object will then become available. The key will contain this object:

.. list-table::
   :header-rows: 0
   :widths: auto

   * - | ``height``
       | integer
     - Height of the image in pixels.

   * - | ``width``
       | integer
     - Width of the image in pixels.

   * - | ``src``
       | string
     - The URI you can use to display the QR code. Note that we can send both data URIs as well as links to HTTPS
       images. You should support both.

For an implemention guide, see our :ref:`QR codes guide <guides/qr-codes>`.

Example
-------

Request
^^^^^^^
.. code-block:: bash

   curl -X GET https://api.mollie.com/v1/payments/tr_WDqYK6vllg \
       -H "Authorization: Bearer test_dHar4XY7LxsDOtmnkVtjNVWXLSlXsM"

Response
^^^^^^^^
.. code-block:: http

   HTTP/1.1 200 OK
   Content-Type: application/json; charset=utf-8

   {
       "resource": "payment",
       "id": "tr_WDqYK6vllg",
       "mode": "test",
       "createdDatetime": "2018-03-16T14:30:07.0Z",
       "status": "paid",
       "paidDatetime": "2018-03-16T14:34:50.0Z",
       "amount": "35.07",
       "description": "Order 33",
       "method": "ideal",
       "metadata": {
           "order_id": "33"
       },
       "details": {
           "consumerName": "Hr E G H K\u00fcppers en\/of MW M.J. K\u00fcppers-Veeneman",
           "consumerAccount": "NL53INGB0618365937",
           "consumerBic": "INGBNL2A"
       },
       "locale": "nl",
       "profileId": "pfl_QkEhN94Ba",
       "links": {
           "webhookUrl": "https://webshop.example.org/payments/webhook",
           "redirectUrl": "https://webshop.example.org/order/33/"
       }
   }