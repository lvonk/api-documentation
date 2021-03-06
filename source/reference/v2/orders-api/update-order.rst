Update order
===================
.. api-name:: Orders API
   :version: 2

.. warning::
   This API is currently in private beta. If you are interested in participating, please contact your account manager at
   Mollie.

.. endpoint::
   :method: PATCH
   :url: https://api.mollie.com/v2/orders/*id*

.. authentication::
   :api_keys: true
   :oauth: true

This endpoint can be used to update the billing and/or shipping address of an order.

When updating an order that uses a *pay after delivery* method such as *Klarna Pay later*, it can be
the case that Klarna does not accept the requested changes. In that case the order is still valid,
though the requested changes have not been applied to the order.

Parameters
----------
Replace ``id`` in the endpoint URL by the order's ID, for example ``ord_8wmqcHMN4U``.

Please note that even though both parameters are optional, at least one of them needs to be provided
in the request.

.. list-table::
   :widths: auto

   * - ``billingAddress``

       .. type:: address object
          :required: false

     - The billing person and address for the order. See :ref:`order-address-details` for the exact
       fields needed.

   * - ``shippingAddress``

       .. type:: address object
          :required: false

     - The shipping address for the order. See :ref:`order-address-details` for the exact fields
       needed.

Mollie Connect/OAuth parameters
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
If you're creating an app with :doc:`Mollie Connect/OAuth </oauth/overview>`, the ``testmode`` parameter is also available.

.. list-table::
   :widths: auto

   * - ``testmode``

       .. type:: boolean
          :required: false

     - Set this to ``true`` to update a test mode order.

Response
--------
``200`` ``application/hal+json; charset=utf-8``

An order object is returned, as described in
:doc:`Get order </reference/v2/orders-api/get-order>`.

Example
-------

Request (curl)
^^^^^^^^^^^^^^
.. code-block:: bash
   :linenos:

   curl -X PATCH https://api.mollie.com/v2/orders/ord_kEn1PlbGa \
       -H "Authorization: Bearer test_dHar4XY7LxsDOtmnkVtjNVWXLSlXsM" \
       -d '{
           "billingAddress": {
               "streetAndNumber": "Keizersgracht 313",
               "city": "Amsterdam",
               "region": "Noord-Holland",
               "postalCode": "1234AB",
               "country": "NL",
               "title": "Dhr",
               "givenName": "Piet",
               "familyName": "Mondriaan",
               "email": "piet@mondriaan.com",
               "phone": "+31208202070"
           }
       }'

Request (PHP)
^^^^^^^^^^^^^
.. code-block:: php
   :linenos:

   <?php
   $mollie = new \Mollie\Api\MollieApiClient();
   $mollie->setApiKey("test_dHar4XY7LxsDOtmnkVtjNVWXLSlXsM");

   $order = $mollie->orders->get("ord_kEn1PlbGa");
   $order->billingAddress->streetAndNumber = "Keizersgracht 313";
   $order->billingAddress->city = "Amsterdam";
   $order->billingAddress->region = "Noord-Holland";
   $order->billingAddress->postalCode = "1234AB";
   $order->billingAddress->country = "NL";
   $order->billingAddress->title = "Dhr";
   $order->billingAddress->givenName = "Piet";
   $order->billingAddress->familyName = "Mondriaan";
   $order->billingAddress->email = "piet@mondriaan.com";
   $order->billingAddress->phone = "+31208202070";
   $order->update();

Response
^^^^^^^^
.. code-block:: http
   :linenos:

   HTTP/1.1 200 OK
   Content-Type: application/hal+json; charset=utf-8

   {
        "resource": "order",
        "id": "ord_kEn1PlbGa",
        "profileId": "pfl_URR55HPMGx",
        "method": "ideal",
        "amount": {
            "value": "1027.99",
            "currency": "EUR"
        },
        "amountCaptured": {
            "value": "0.00",
            "currency": "EUR"
        },
        "amountRefunded": {
            "value": "0.00",
            "currency": "EUR"
        },
        "status": "created",
        "isCancelable": true,
        "metadata": null,
        "createdAt": "2018-08-02T09:29:56+00:00",
        "expiresAt": "2018-08-30T09:29:56+00:00",
        "mode": "live",
        "locale": "nl_NL",
        "orderNumber": "18475",
        "billingAddress": {
            "streetAndNumber": "Keizersgracht 313",
            "city": "Amsterdam",
            "region": "Noord-Holland",
            "postalCode": "1234AB",
            "country": "NL",
            "title": "Dhr",
            "givenName": "Piet",
            "familyName": "Mondriaan",
            "email": "piet@mondriaan.com",
            "phone": "+31208202070"
        },
        "shippingAddress": {
            "streetAndNumber": "Keizersgracht 313",
            "postalCode": "1016 EE",
            "city": "Amsterdam",
            "country": "nl",
            "givenName": "Luke",
            "familyName": "Skywalker",
            "email": "luke@skywalker.com"
        },
        "lines": [
            {
                "resource": "orderline",
                "id": "odl_dgtxyl",
                "orderId": "ord_pbjz8x",
                "name": "LEGO 42083 Bugatti Chiron",
                "productUrl": "https://shop.lego.com/nl-NL/Bugatti-Chiron-42083",
                "imageUrl": "https://sh-s7-live-s.legocdn.com/is/image//LEGO/42083_alt1?$main$",
                "sku": "5702016116977",
                "type": "physical",
                "status": "created",
                "isCancelable": true,
                "quantity": 2,
                "quantityShipped": 0,
                "amountShipped": {
                    "value": "0.00",
                    "currency": "EUR"
                },
                "quantityRefunded": 0,
                "amountRefunded": {
                    "value": "0.00",
                    "currency": "EUR"
                },
                "quantityCanceled": 0,
                "amountCanceled": {
                    "value": "0.00",
                    "currency": "EUR"
                },
                "unitPrice": {
                    "value": "399.00",
                    "currency": "EUR"
                },
                "vatRate": "21.00",
                "vatAmount": {
                    "value": "121.14",
                    "currency": "EUR"
                },
                "discountAmount": {
                    "value": "100.00",
                    "currency": "EUR"
                },
                "totalAmount": {
                    "value": "698.00",
                    "currency": "EUR"
                },
                "createdAt": "2018-08-02T09:29:56+00:00"
            },
            {
                "resource": "orderline",
                "id": "odl_jp31jz",
                "orderId": "ord_pbjz8x",
                "name": "LEGO 42056 Porsche 911 GT3 RS",
                "productUrl": "https://shop.lego.com/nl-NL/Porsche-911-GT3-RS-42056",
                "imageUrl": "https://sh-s7-live-s.legocdn.com/is/image/LEGO/42056?$PDPDefault$",
                "sku": "5702015594028",
                "type": "physical",
                "status": "created",
                "isCancelable": true,
                "quantity": 1,
                "quantityShipped": 0,
                "amountShipped": {
                    "value": "0.00",
                    "currency": "EUR"
                },
                "quantityRefunded": 0,
                "amountRefunded": {
                    "value": "0.00",
                    "currency": "EUR"
                },
                "quantityCanceled": 0,
                "amountCanceled": {
                    "value": "0.00",
                    "currency": "EUR"
                },
                "unitPrice": {
                    "value": "329.99",
                    "currency": "EUR"
                },
                "vatRate": "21.00",
                "vatAmount": {
                    "value": "57.27",
                    "currency": "EUR"
                },
                "totalAmount": {
                    "value": "329.99",
                    "currency": "EUR"
                },
                "createdAt": "2018-08-02T09:29:56+00:00"
            }
        ],
        "_links": {
            "self": {
                "href": "https://api.mollie.com/v2/orders/ord_pbjz8x",
                "type": "application/hal+json"
            },
            "checkout": {
                "href": "https://www.mollie.com/payscreen/order/checkout/pbjz8x",
                "type": "text/html"
            },
            "documentation": {
                "href": "https://docs.mollie.com/reference/v2/orders-api/get-order",
                "type": "text/html"
            }
        }
   }
