---
layout: master
category: magento
title: Magento layout handle
---

Upon developing new module with a front controller, Even with the help from <a href="http://magicento.com/">Magicento</a>, it could be tricky to get your layout handling right.

A small trick to get this over is to debug and show which layout handler is being loaded via the controller (although this means your controller configuration has to be working first!).

Just add a small line of debug in your controller and you shall see which layout handler is being called, hence why yours is not working.

``` php
<?php
    public function indexAction(){
        $this->loadLayout();
        $this->renderLayout();
        Zend_Debug::dump($this->getLayout()->getUpdate()->getHandles());
    }
?>
```