# :fontawesome-brands-magento: Magento Introduction for New Technicians

This document introduces the Magento CMS with a focus on Magento 2, the latest version of the platform. We will be mentioning some differences with Magento 1 for better understanding, but won’t go into too much detail.


## Basic Magento 1 Information

Before diving into Magento 2, let’s cover some information regarding Magento 1 seeing as many legacy systems are still active (even though they shouldn’t be).

**Key Features**

* Version

    Magento 1 has reached its end of life (EOL) in 2020 and is no longer supported.
    This means that the Magento team no longer provides updates, patches, or official support for this version. (please update to Magento 2)

* Structure

    The primary directory structure can be considered cluttered in comparison to its later version. These include: app, lib, media, skin, and js.

* Configuration

    Core configuration settings, such as database connection details, cache settings and other neat details are located in app/etc/local.xml.

* No Composer

    Package management is manual and clients cannot utilize Composer for dependencies.
    Unlucky for them.


**Now, let’s get to the good part: Magento 2! From here on out, we’ll be only be talking about it.**

## Useful Magento 2 Commands to Remember

!!! warning "Important Notice"

    These commands must be executed as the actual user within the website’s document root and will not work when run as a support agent.

### Cache management

```console
php bin/magento cache:clean
``` 

- Cleans the cache storage.

```console
php bin/magento cache:flush
``` 

- Flushes the cache storage, clearing all cache types. 

The first command deletes only the expired cache entries while preserving the other cache items, whereas the second one clears the entire cache storage, including external cache (like Redis or Varnish), regardless of expiration.

```console
php bin/magento cache:status
```

- Used to check the status of various cache types in the application. 
When you run this command, it will output a list of cache types along with their current state (enabled 1 or disabled 0).

*Sample:*
```console
Current status:
                        config: 1
                        layout: 1
                    block_html: 1
                   collections: 1
                    reflection: 1
                        db_ddl: 1
               compiled_config: 1
             webhooks_response: 1
                           eav: 1
         customer_notification: 1
 graphql_query_resolver_result: 1
            config_integration: 1
        config_integration_api: 1
                  admin_ui_sdk: 1
                     full_page: 1
                   target_rule: 1
             config_webservice: 1
                     translate: 1
```

```console
php bin/magento cache:enable <cache_type>
php bin/magento cache:disable <cache_type>
```

- The first command enables a specific cache type in Magento 2, while the second one disables it.
If a cache type is off when it should be on, it can cause slow page loads or other issues.

### Compilation and Deployment

```console
php bin/magento setup:di:compile
```

- Generates/compiles the necessary dependency injection (DI) classes (code) and configures the application. (Pretty important stuff)

```console
php bin/magento setup:static-content:deploy
```

- Deploys static files like CSS and JavaScript for the front end.
(Probably the best thing in existence alongside cache cleaning)

### Index Management

```console
php bin/magento indexer:status
```

- Checks the status of each indexer to determine whether it's valid and ready for use or if it needs reindexing.
If an indexer is invalid, any changes made in the backend won’t be visible on the frontend.

```console
php bin/magento indexer:reindex
```

- Rebuilds indexes in Magento to help the store quickly find and display the latest information.
For example, when product prices are updated or new products are added to the catalog.

### Developer Commands

```console
php bin/magento deploy:mode:set <mode>
```

- Sets the site's deployment mode, where <mode> can be default, developer, or production.

The developer mode gives more verbose error messages (troubleshooting purposes), while production should be used for live environments seeing as it optimizes performance. *Default is like the middle child, no one even knows it exists.*


