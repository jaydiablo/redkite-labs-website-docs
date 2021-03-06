Add the Business Slider App-Block to AlphaLemon CMS
===================================================

The Business Slider App-Block is a javascript slider to rotate a series of images.

Installation
------------

You can add the **Business Slider App-Block** to the application managed by AlphaLemon 
CMS, adding it to the **composer.json** of your Symfony2 application:

.. code-block:: text

    {
        "name": "alphalemon/alphalemon-cms-sandbox",
        [...]
        "require": {
            [...]        
            "alphalemon/app-business-slider-bundle": "dev-master",        
        }
    }

Be sure that there is declared the reference to **http://apps.alphalemon.com** repository,
under the **repositories** option:

.. code-block:: text

    "repositories": [

        [...]

        {
            "type": "composer",
            "url": "http://apps.alphalemon.com/"
        }
    ],

then run the composer's update command:

.. code-block:: text

    php composer.phar update alphalemon/app-business-slider-bundle

To have the new block available in the AlphaLemon CMS' add blocks menu, you have to 
clear the cache as follows:

.. code-block:: text

    app/console cache:clear --env=alcms


.. note::

    This App-Block is already required by the BusinessWebSiteTheme which is the default 
    theme distributed with AlphaLemon CMS