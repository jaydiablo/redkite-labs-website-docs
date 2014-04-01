Migrate an AlphaLemon CMS Block to a RedKite CMS Block
======================================================
This document explains in detail how to migrate an existing AlphaLemon CMS Block to
a RedKite CMS Block.

The AlphaLemon namespace
------------------------
Many services of your block, refer to many AlphaLemon objects: you must change this
reference.

Search the **AlphaLemon\\AlphaLemonCmsBundle** reference and replace it with **RedKiteLabs\\RedKiteCmsBundle**.

.. note:

    Doing this by hand is quite a nightmare, so you should use an IDE which has a function
    to let you search and replace in files.
    

The AlphaLemon folder
---------------------

A RedKite CMS Block lives under the **RedKiteCms** folder, so rename the **AlphaLemon** folder to 
**RedKiteCms**.

Due to this change you must replace all the **AlphaLemon** entries in your bundle,
so search **AlphaLemon** and replace it with **RedKiteCms**.

The app_block.xml configuration file
------------------------------------

Open the **Resources/config/app_block.xml** and replace the **alphalemon_cms.blocks_factory.block**
tag entry with **red_kite_cms.blocks_factory.block** for the service that defines the block.

If your service requires the **alpha_lemon_cms.events_handler** as first argument,
replace it with the **red_kite_cms.events_handler** service.

The configuration files
-----------------------
Inside the **Resources/config** there are three configuration files:

- config_alcms
- config_alcms_dev
- config_alcms_test

rename them as

- config_rkcms
- config_rkcms_dev
- config_rkcms_test

Open the **config_rkcms_dev** and the **config_rkcms_test** and in both files, replace
the reference to **config_alcms** with **config_rkcms**.

Run the commands
----------------

The hard job has been made, so run the following commands from the console to complete
the migration:

.. code:: text

    php app/rkconsole ca:c --env=rkcms_dev
    php app/rkconsole assets:install web --env=rkcms
    php app/rkconsole assetic:dump --env=rkcms