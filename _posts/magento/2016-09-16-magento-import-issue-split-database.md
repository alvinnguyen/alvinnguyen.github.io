---
layout: master
category: magento
title: Magento import issue with split read/write database
---

I ran across this issue on Magento EE 1.13.0 on a split read/write database setup.

* The import via script (using Magento core functions) occasionally succeeded but failed most of the time.
* The dataflow import in Magento back-end seems to work all the time.

I've traced the failing point to `\Mage_ImportExport_Model_Import::importSource`

In particular `\Mage_ImportExport_Model_Resource_Import_Data::getEntityTypeCode` was throwing an exception at line 119 
since the query not returning expected result. 

However, the exception doesn’t seem to be caught by the parent function to log into exception log.

In both `\Mage_ImportExport_Model_Resource_Import_Data::getBehavior` 
and `\Mage_ImportExport_Model_Resource_Import_Data::getEntityTypeCode`, 
they are using read adapter to get the import information got written into the read database mili-seconds before.

Since the read database replica latency might be greater than the time consumed by the speed of the script, 
the information the script is looking for might not be available in the read databse.

This leads to the script dying. The solution is as simple as overriding those 2 functions and 
use write adapter to read for those information instead.