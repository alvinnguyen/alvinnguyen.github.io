---
layout: master
category: magento
title: Magento & Composer with restricted deployment
---
<strong>Requirement</strong>: Magento, Composer, <a href="https://github.com/magento-hackathon/magento-composer-installer">Magento Composer Installer</a>

#### What's the right way to develop PHP module, Magento one in specific, with Composer and deployment?

1. Develop the module as standard
1. Move the files into your package
1. Create composer to contain the module information and file mapping
1. Push the module into your GIT repo
1. Run composer on your production to pull the module and symlink it
1. No files should be committed to your Magento repo except for composer.json &amp; composer.lock

Well, I currently have a situation where nothing much can be done with the deployment process. Current deployment process is pulling file from a GIT, that's it; so running `composer update` on production environment is a no-go.

***

#### So how to make Composer and Magento play nice together when you cannot change the deployment process?

Given this situation, I have to come up with my own workflow. This might not be perfect, so ideas are welcome.

We will keep the same 1 to 4 steps. However, I'm skeptical to committing the whole vendor folder as some of them are not contributing that much to the whole Magento system (except for my own module). With that in mind, I excluded these by putting them into .gitignore

    vendor/composer
    vendor/bin
    vendor/eloquent
    vendor/icecave
    vendor/justinrainbow
    vendor/magento-hackathon
    vendor/autoload.php

So if you do a GIT status check on your Magento repo, you shouldn't see any modified file except for maybe composer.json and composer.lock.

Now, to deploy your newly developed module into this Magento, run `composer update --prefer-dist`

This is to make sure composer will pull the repo without .git so the whole module will belong to your Magento repo.

Upon running this, GIT status should show your module in vendor and the symlinks in their corresponding position.

***

#### Cool, the module is installed in Magento, now how do I go about upgrading/modifying it?

1. Work on your files in Magento repo as usual
1. Do <strong>NOT</strong> commit your changes to Magento repo yet
1. After finish updating your module, copy your whole vendor/your_namespace/your_module in your Magento repo back to your module repo
1. Commit &amp; push from your module repo
1. Do a `composer update --prefer-dist` in your Magento workspace to pull your updated module
1. Now you can commit your changes to your Magento repo
1. Proceed with your normal deployment process