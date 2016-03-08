---
layout: master
category: magento
title: Magento logged-in customer detection
---

This is a very common task for most of Magento plugin

The script to check if customer is logged in then get their information is as below

``` php
<?php
    if (Mage::getSingleton('customer/session')->isLoggedIn()) {
        $customer = Mage::getSingleton('customer/session');
        $customerData = Mage::getModel('customer/customer')->load($customer->getId())->getData();
        print_r($customerData);
    }
?>
```

The $customerData array should look something like this, which is quite easy to manipulate

``` php
<?php
    Array
    (
        [entity_id] => 1
        [entity_type_id] => 1
        [attribute_set_id] => 0
        [website_id] => 1
        [email] => john.doe@example.com
        [group_id] => 1
        [increment_id] => 000000001
        [store_id] => 1
        [created_at] => 2007-08-30 23:23:13
        [updated_at] => 2008-08-08 12:28:24
        [is_active] => 1
        [firstname] => John
        [lastname] => Doe
        [password_hash] => 204948a4020ed1d0e4238db2277d5:eg
        [prefix] =>
        [middlename] =>
        [suffix] =>
        [taxvat] =>
        [default_billing] => 274
        [default_shipping] => 274
    )
    ?>
```