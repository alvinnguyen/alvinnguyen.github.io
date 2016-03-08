---
layout: master
category: magento
title: Flush Magento Enterprise cache programmatically
---

A quick snippet to flush entire Magento cache including full page cache.

This code flushes all the cache but full page cache.

``` php
<?php
    Mage::dispatchEvent('adminhtml_cache_flush_all');
    Mage::app()->getCacheInstance()->flush();
?>
```

The following line of code should do the job nicely, I found this by checking Magento observer method on catalog rule apply after.

``` php
<?php
    Enterprise_PageCache_Model_Cache::getCacheInstance()->clean(Enterprise_PageCache_Model_Processor::CACHE_TAG);
?>
```