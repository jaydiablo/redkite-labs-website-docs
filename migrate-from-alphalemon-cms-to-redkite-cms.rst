Migrationn from AlphaLemon CMS to RedKite CMS
=============================================

This document explains in detail how to migrate an existing Symfony2 application powered
by AlphaLemon CMS to RedKite CMS.

Composer
--------

Replace the apps repository url in your **composer.json**:

.. code:: text

    "repositories": [
        {
            "type": "composer",
            "url": "http://apps.alphalemon.com/"
        }
    ],

with these ones:

.. code:: text

    "repositories": [
        {
            "type": "composer",
            "url": "http://apps.redkite-labs.com/"
        }
    ],

then migrate the old AlphaLemon CMS requirements:

.. code:: text

    "alphalemon/alphalemon-cms-bundle": "dev-master",
    "alphalemon/alphalemon-cms-installer-bundle": "dev-master",
    "alphalemon/app-social-block-bundle": "dev-master", 
    "alphalemon/app-tinymce-block-bundle" : "dev-master",          
    "alphalemon/bootbusiness-theme-bundle": "dev-master"

with the new ones:

.. code:: text

    "redkite-cms/redkite-cms-bundle": "dev-master",
    "redkite-cms/installer-bundle": "dev-master",
    "redkite-labs/bootbusiness-theme-bundle": "dev-master",
    "redkite-cms/redkite-cms-base-blocks": "dev-master",
    "redkite-cms/tinymce-block-bundle": "dev-master",
    "redkite-cms/social-block-bundle": "dev-master"

Update your project:

.. code:: text

    php composer.phar update

When composer finishes its job, you could get this error: 

.. code:: text

    Error: Class 'AlphaLemon\CmsInstallerBundle\AlphaLemonCmsInstallerBundle' not found

This happens when you left the AlphaLemonCmsInstallerBundle instantation in your AppKernel.


The AppKernel.php
-----------------
Open the **AppKernel** and remove the **AlphaLemon\CmsInstallerBundle\AlphaLemonCmsInstallerBundle**
instantation or replace it as follows:

.. code:: php

    // app/AppKernel.php

    new RedKiteCms\InstallerBundle\RedKiteCmsInstallerBundle(),


.. note::

    That bundle is not required when the CMS is installed, so, the best choice is to remove it
    from the **AppKernel**.

Replace the following bundle instantation:

.. code:: php

    new AlphaLemon\BootstrapBundle\AlphaLemonBootstrapBundle(),


with the new bundle namespace:

.. code:: php

    new RedKiteLabs\BootstrapBundle\RedKiteLabsBootstrapBundle(), 


then replace the BundlesAutoloader:


.. code:: php

    $bootstrapper = new \AlphaLemon\BootstrapBundle\Core\Autoloader\BundlesAutoloader(__DIR__, $this->getEnvironment(), $bundles);


with the new namespace:


.. code:: php

    $bootstrapper = new \RedKiteLabs\BootstrapBundle\Core\Autoloader\BundlesAutoloader(__DIR__, $this->getEnvironment(), $bundles);


Configuration files
-------------------

Rename the cms configuration files in your app/config folder:

.. code:: text

    config_alcms.yml -> config_rkcms.yml
    config_alcms_dev.yml -> config_rkcms_dev.yml
    config_alcms_test.yml -> config_rkcms_test.yml


and the routing files:


.. code:: text

    routing_alcms.yml -> routing_rkcms.yml
    routing_alcms_dev.yml -> routing_rkcms_dev.yml
    routing_alcms_test.yml -> routing_rkcms_test.yml


Open the config_rkcms.yml file and replace the following code:

.. code:: text

    imports:
        - { resource: parameters.yml }
        - { resource: "@AlphaLemonCmsBundle/Resources/config/config_alcms.yml" }
        - { resource: "@AlphaLemonCmsBundle/Resources/config/security.yml" }

    [...]

    alpha_lemon_theme_engine:
        deploy_bundle: [YOUR DEPLOY BUNDLE]

with this one:

.. code:: text

    imports:
        - { resource: parameters.yml }
        - { resource: "@RedKiteCmsBundle/Resources/config/config_rkcms.yml" }
        - { resource: "@RedKiteCmsBundle/Resources/config/security.yml" }

    [...]

    red_kite_labs_theme_engine:
        deploy_bundle: [YOUR DEPLOY BUNDLE]


Add the website_url

.. code:: text

    red_kite_cms:
        website_url: [YOUR WEB SITE URL ACCORDING THIS FORMAT: http://redkite-labs.com/]

Don't forget the slash at the end. This setting is used to generate the sitemap for your website.

Open the config_rkcms_dev.yml file and replace the following code:

.. code:: text

    imports:
        - { resource: config_alcms.yml }
        - { resource: "@AlphaLemonCmsBundle/Resources/config/config_alcms_dev.yml" }

with this one:

.. code:: text

    imports:
        - { resource: config_rkcms.yml }
        - { resource: "@RedKiteCmsBundle/Resources/config/config_rkcms_dev.yml" }


Open the routig_rkcms.yml file and replace the following code:

.. code:: text

    _alcms:
        resource: "@AlphaLemonCmsBundle/Resources/config/routing_alcms.yml"

with this one:

.. code:: text

    _rkcms:
        resource: "@RedKiteCmsBundle/Resources/config/routing_rkcms.yml"


Open the routig_rkcms_dev.yml file and replace the following code:

.. code:: text

    _alcms:
        resource: "@AlphaLemonCmsBundle/Resources/config/routing_alcms_dev.yml"

    _alcms_dev:
        resource: routing_alcms.yml

with this one:

.. code:: text

    _rkcms:
        resource: "@RedKiteCmsBundle/Resources/config/routing_rkcms_dev.yml"

    _rkcms_dev:
        resource: routing_rkcms.yml


The web folder
--------------
In you application's web folder rename the following front controllers as follows:

.. code:: text

    alcms.php -> rkcms.php
    alcms_dev.php -> rkcms_dev.php

then open the rkcms.php file and replace the following code:

.. code:: php

    $kernel = new AppKernel('alcms', false);

with this one:

.. code:: php

    $kernel = new AppKernel('rkcms', false);

open the rkcms_dev.php file and replace the following code:

.. code:: php

    $kernel = new AppKernel('alcms_dev', false);

with this one:

.. code:: php

    $kernel = new AppKernel('rkcms_dev', false);

Open the **web/components** and rename **alphalemoncms** to **redkitecms**.


The deploy bundle controller
----------------------------

Open the controller which renders your website inside your deploy bundle, in a standard
installation this file is the **Acme\WebSiteBundle\Controller\WebSiteController.php** 
and replace the following **use** statement:

.. code:: php

    use AlphaLemon\ThemeEngineBundle\Core\Rendering\Controller\FrontendController;

with this one:

.. code:: php

    use RedKiteLabs\ThemeEngineBundle\Core\Rendering\Controller\FrontendController;


Run the commands
----------------

The hard job has been made, so run the following commands from the console to complete
the migration:

Clear the cache for all the environments:

.. code:: text

    php app/console ca:c
    php app/console ca:c --env=rkcms_dev
    php app/console propel:model:build --env=rkcms
    php app/console assets:install web --symlink --env=rkcms
    php app/console assetic:dump --env=rkcms
