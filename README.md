# Server Side Analytics for Magento 2

This extension aims to solve the problem of discrepancies between Magento revenue reports and the revenue reports in Google Analytics.

That problem arises due to the fact that a certain number of people close the browser window before returning to Magento's success page. Since Google Analytics is Javascript based, and thus client based, the GA Purchase Event will not be fired and the order will not be registered in Analytics.

Another reason why this problem arises is that people decide to pay at a later point in time through a different platform (like the PSP's), using a link in an email for example.

## Installation

```bash
composer require elgentos/serversideanalytics2
bin/magento setup:upgrade
```

### Sample query for GraphQL to set the GA data

```
mutation AddGaUserId($cartId: String!, $gaUserId: String, $gaSessionId: String) {
                            AddGaUserId(input: {
                                    cartId: $cartId
                                    gaUserId: $gaUserId
                                    gaSessionId: $gaSessionId
                                }
                            ),
                            {
                               cartId
                               maskedId
                            }
                    }
```

## Further info
- Compatible with GA4 Measurement Protocol;
- When using YireoGTM, you can either disable the purchase event on the success page, or simply not send it to GA with GTM. Or just keep it, since Google will fix double transactionId's
- Debugging is enabled when Magento is in developer mode. See `var/log/system.log` for the log;
- Exceptions will be logged to `var/log/exceptions.log`;
- The products in the payload are retrieve on invoice-basis, not on order-basis;
- An event has been added for you to add or overwrite custom fields to products in the purchase event; `elgentos_serversideanalytics_product_item_transport_object`;
- An event has been added for you to add or overwrite custom fields to transaction data in the purchase event; `elgentos_serversideanalytics_transaction_data_transport_object`;
- An event has been added for you to add or overwrite fields to tracking data in the purchase event; `elgentos_serversideanalytics_tracking_data_transport_object`;
- Testing can be done by dispatching `test_event_for_serversideanalytics` with a `$payment` (`\Magento\Sales\Order\Payment`) object in the payload;
- magerun2 dev:events:fire --eventName sales_order_payment_pay --parameters "payment::\Magento\Sales\Model\Order\Payment:X;invoice::\Magento\Sales\Model\Order\Invoice:X";
