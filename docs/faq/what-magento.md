# Magento 2 Theory

### What file contains Magento 2 database information (including username, database name, and password)?

??? abstract "Answer:" 

    app/etc/env.php
    
    This file holds configuration settings for the application, including database credentials and other environment variables, such as Redis for example. (You’ll get familiar with that later on 😼)

### What does a Magento 2 module do?

??? abstract "Answer:" 

    While WordPress uses plugins to add new features and extend functionalities, Magento utilizes modules for similar purposes.

### Which command would you use to install a specific Magento module?

??? abstract "Answer:" 

    That’s the neat part, you don’t.
    Magento is very sensitive to changes, and even a small adjustment can break the site.
    This is developer scope of work, so we can simply say, no you.

    Hypothetically, if the client provides written permission as well as a detailed explanation, and you consult a senior colleague to see if it's worth even risking/trying, the procedure might look something like this (basic example):

    ```markdown
    composer require <vendor/Module_Name>
    php bin/magento setup:upgrade
    ```

### In a Magento 2 site's database, which tables store settings related to the site's URLs?

??? abstract "Answer:" 

    The primary table that stores site URLs is core_config_data, where the keys web/unsecure/base_url and web/secure/base_url define the URLs.

### Which methods can be used to update Magento 2's core files to a new version? (select all that apply):

??? abstract "Answer:" 

    You can manually upload files or run a patch, but using Composer is the recommended method for maintaining dependencies and upgrades. <<< Developer issue, we don’t deal with that, at all.

### What is the octal (numerical) permissions value that a directory should have in a Magento installation?

??? abstract "Answer:" 

    Directories should generally have 755 permissions, and files should have 644. PHP files should preferably be set to 600 for security purposes.

### What is the purpose of the var directory in Magento 2?

??? abstract "Answer:" 

    The var directory is used for temporary files, including cache files, logs, and generated files.
    For Support purposes, it’s mostly used to review log data.

### What is Composer, and why is it essential for Magento 2?

??? abstract "Answer:" 

    Composer is a crucial dependency management tool for PHP and Magento 2 that simplifies module installation, updates, and version management of libraries.
