---
title: Unsupported/Problematic Modules and Plugins
description: An up-to-date list of Drupal modules and WordPress plugins Pantheon does not support.
keywords: modules, plugins, unsupported, drupal, wordpress
---
This article lists modules and plugins that do not work with or are currently unsupported on the Pantheon platform.
We do not prevent you from installing and using these plugins/modules; however, they will not work as expected and we cannot provide troubleshooting support.

##Drupal Modules

- APC

- Adaptive Image Styles

- Apache Solr Search
**Issue** If Acquia Solr modules are present in the site codebase (even if disabled) and Pantheon Apache Solr is enabled, the site will be unable to connect to Solr server.  
**Solution** Delete the Acquia Solr modules from the codebase and then disable and re-enable the Pantheon Apache Solr module.

- Background Process  
**Issue**: The module allows for Drupal to perform "parallel" (asynchronous non-blocking mode) requests. However, there are a number of limitations working in a distributed environment and function correctly on the platform.

- Backup & Migrate

- Boost

- Cache Expiration

- Global Redirect  
 **Issue**: Too many redirects error when site is in maintenance mode.  
 **Solution**: Ensure that the "Frontpage Redirect Handler" is not ticked in the Global Redirect administration page.

- HTTPRL  
**Issue**: This module can severely impact performance. This may be the result of module code or its configuration on the platform that results in the spikes.

- IMCE

- Mobile Tools

- Mollom  
 **Issue**: Cookies break Varnish.

- Pathologic  
 **Issue**: The path of the base URL is changed and cached by the module itself.  
 **Solution**: The [documentation on Drupal.org](https://drupal.org/node/257026) for the module mentions the issues and the remedy, which is a cache clear operation. If you are unable to exclude cached data from your dumps or avoid migrating cache data, you should clear your site’s cache after importing the data.

- Registry Rebuild  
This is built into the Pantheon platform.

- Schema  
**Issue**: The module doesn't work with the MySQL TIMESTAMP column type in our heartbeat table, which is part of how we maintain status around whether or not a site and its database is active. This is a [known bug](https://drupal.org/node/468644) in the schema module.  
**Solution**: Set a variable to suppress the error, [shown here](http://drupalcode.org/project/schema.git/blob/08b02458694d186f8ab3bd0b24fbc738f9271108:/schema.module#l372). Setting the variable `schema_suppress_type_warnings` to **true** will do it. You can achieve that by adding the following line to `settings.php`:  
   ```
   $conf[‘schema_suppress_type_warnings’] = TRUE;
   ```

- Varnish

### Using the `/tmp` Directory
**Issue**:
The modules listed below are not supported due to the use of the `/tmp` directory. With multiple application servers, as exists on Live environments, Drupal assumes the `/tmp` directory will be on the same application container. However, as we run a distributed application container matrix, the `/tmp` directory is not shared. For more details on Pantheon's distributed infrastructure, see [All About Application Containers](/docs/articles/sites/all-about-application-containers).

<div class="alert alert-danger" role="alert">
<h4>Warning</h4>
Using sites/default/files/tmp as a work around for these issues will produce unpredictable side effects and is not supported.</div>


- Media: Browser Plus

- Media: Filesystem

- Plupload

- Taxonomy CSV  

- Views data export

- Webform export<br>
 **Solution**: Use [drush](http://www.drush.org/en/master/), as this uses a single application container to process the export. The relevant Drush command is `webform-export` (alias wfx).


##WordPress Plugins

- WP Super Cache

- W3 Total Cache

- batcache

- Timthumb


## Dynamic Outbound IPs
Due to the cloud-based infrastructure of the Pantheon platform, outbound requests are served by dynamic IP addresses. If your site relies on a static IP address for outgoing requests, the recommended solution is the [Pantheon Enterprise Gateway](/docs/articles/sites/code/pantheon-enterprise-gateway). This is the only way to guarantee compatibility with extensions or services that require a known outgoing IP. Otherwise, you will need to find an alternative to accomplish the request.

For more information, see [Dynamic Outgoing IP Addresses](/docs/articles/sites/code/dynamic-outgoing-ip-addresses).
