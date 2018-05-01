.. _v2/settlements-get-chargebacks:

Settlements API v2: Get settlement chargebacks
==============================================
``GET`` ``https://api.mollie.com/v2/settlements/*settlementId*/chargebacks``

Authentication: :ref:`OAuth access tokens <oauth/overview>`

Retrieve all chargebacks included in a settlement.

Parameters
----------
Replace ``settlementId`` in the endpoint URL by the settlement's ID, for example ``stl_jDk30akdN``.

This endpoint is an alias of the :ref:`List chargebacks <v2/chargebacks-list>` endpoint. All parameters for that
endpoint can be used here as well.

Response
--------
``200`` ``application/hal+json; charset=utf-8``

This endpoint is an alias of the :ref:`List chargebacks <v2/chargebacks-list>` endpoint. The response is therefore the
exact same.

Example
-------

Request
^^^^^^^
.. code-block:: bash

   curl -X GET https://api.mollie.com/v2/settlements/stl_jDk30akdN/chargebacks \
       -H "Authorization: Bearer access_Wwvu7egPcJLLJ9Kb7J632x8wJ2zMeJ"

Response
^^^^^^^^
.. code-block:: http

   HTTP/1.1 200 OK
   Content-Type: application/hal+json; charset=utf-8

   {
       "count": 3,
       "_embedded": {
           "chargebacks": [
               {
                   "resource": "chargeback",
                   "id": "chb_n9z0tp",
                   "amount": {
                       "currency": "USD",
                       "value": "43.38"
                   },
                   "settlementAmount": {
                       "currency": "EUR",
                       "value": "35.07"
                   },
                   "createdAt": "2018-03-14T17:00:52.0Z",
                   "reversedAt": null
                   "paymentId": "tr_WDqYK6vllg",
                   "settlementId": "stl_jDk30akdN",
                   "_links": {
                       "self": {
                           "href": "https://api.mollie.com/v2/payments/tr_WDqYK6vllg/chargebacks/chb_n9z0tp",
                           "type": "application/hal+json"
                       },
                       "payment": {
                           "href": "https://api.mollie.com/v2/payments/tr_WDqYK6vllg",
                           "type": "application/hal+json"
                       },
                       "documentation": {
                           "href": "https://www.mollie.com/en/docs/reference/chargebacks/get",
                           "type": "text/html"
                       }
                   }
               }
               { },
               { }
           ]
       },
       "_links": {
           "self": {
               "href": "https://api.mollie.com/v2/settlements/stl_jDk30akdN/chargebacks",
               "type": "application/hal+json"
           },
           "documentation": {
               "href": "https://www.mollie.com/en/docs/reference/chargebacks/list",
               "type": "text/html"
           }
       }
   }