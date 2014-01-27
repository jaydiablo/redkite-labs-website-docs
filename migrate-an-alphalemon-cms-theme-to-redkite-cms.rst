Migrate an AlphaLemon CMS Theme to a RedKite CMS Theme
======================================================
This document explains in detail how to migrate an existing AlphaLemon CMS Theme to
a RedKite CMS Theme.

The AlphaLemon folder
---------------------

RedKite CMS Themes live under the **RedKiteCms**, so rename the **AlphaLemon** to 
**RedKiteCms**

The DependencyInjection Extension
---------------------------------

Open the theme **DependencyInjection Extension** file and replace the following **use**
statement

.. code:: php

    use AlphaLemon\ThemeEngineBundle\Core\Rendering\DependencyInjection\BaseExtension;


to this one:


.. code:: php

    use RedKiteLabs\ThemeEngineBundle\Core\Rendering\DependencyInjection\BaseExtension;

then replace the **configureTheme** method as follows:


.. code:: php

    public function configureTheme()
    {
        return array();
    }

The theme service file
----------------------
Open your theme service file, saved under the **[ThemeBundle]/Resources/config** folder
and replace the following code:


.. code:: text 

    alpha_lemon_theme_engine.themes.theme

with this one:

.. code:: text

    red_kite_labs_theme_engine.themes.theme

Now you have to replace all the AlphaLemon entries in each file. The best and easy way
to do that is to use the **"replace in files"** function of your editor. So look for
**namespace AlphaLemon\** and replace it with **namespace RedKiteCms\** 

Run the commands
----------------

The hard job has been made, so run the following commands from the console to complete
the migration:

Clear the cache for all the environments:

.. code:: text

    php app/console ca:c
    php app/console ca:c --env=rkcms_dev
    php app/console --env=rkcms redkitecms:generate:templates [YOUR THEME BUNDLE]

Multi languages websites
------------------------

The block type, which handles the languages menu for multilanguages websites, has called
with another name, so the database m.ust be updated. Ru n the following command from the
console:

.. code:: text

    redkitecms:update-to-red-kite-cms

This step can safety be omitted if your website has only a language.
