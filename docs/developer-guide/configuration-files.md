# Configuring MapStore

MapStore (and every application developed with MapStore) allows customization through configuration.
To understand how to configure MapStore you have to know that the back-end and the front-end of MapStore have two different configuration systems.

This separation allows to:

* Make mapstore configuration system live also as a front-end only framework
* Keep the power of customization provided by spring on the back-end

## Back-end Configuration Files

They are `.properties` files or `.xml` files, and they allow to configure the various parts of the back-end.
They are located in `java/web/src/main/resources` and they will be copied in  `MapStore.war` under the directory `/WEB-INF/classes`.

* `proxy.properties`: configuration for the internal proxy (for cross-origin requests). More information [here](https://github.com/geosolutions-it/http-proxy/wiki/Configuring-Http-Proxy).
* `geostore-datasource-ovr.properties`: provides settings for the database.
* `log4j.properties`: configuration for back-end logging
* `sample-categories.xml`: initial set of categories for back-end resources (MAP, DASHBOARD, GEOSTORY...)
* `mapstore.properties`: allow specific overrides to front-end files, See [externalization system](externalized-configuration.md#externalized-configuration) for more details

Except for `mapstore.properties` and `ldap.properties`, all these files are simply overrides of original configuration files coming from the included sub-applications part of the back-end. In `WEB-INF/classes` you will find also some other useful files coming from the original application:

### Back-end security configuration files

Back-end security can be configured to use different authentication strategies. Maven profiles can be used to switch between these different strategies.

Depending on the chosen profile a different file will be copied from the `product/config` folder to  override `WEB-INF/classes/geostore-spring-security.xml` in the final package. In particular:

* **default**: `db\geostore-spring-security-db.xml` (geostore database)
* **ldap**: `ldap\geostore-spring-security-ldap.xml` (LDAP source)

Specific configuration files are available to configure connection details for the chosen profile.

For example, if using LDAP, look at [LDAP integration](integrations/users/ldap.md#ldap-integration-with-mapstore).

## Front-end Configurations Files

They are JSON files that will be loaded via HTTP from the client, keeping most of the framework working also in an html-only context (when used with different back-ends or no-backend). These JSON files are located in `web/client/configs` directory and they will be copied in the `configs` of the war file.

Several configuration files (at development and / or run time) are available to configure all the different aspects of an application.

* `localConfig.json`: Dedicated to the application configuration. Defines all general settings of the front-end part, with all the plugins for all the pages. See [Application Configuration](local-config.md#application-configuration) for more information.
* `new.json` Can be customized to set-up the initial new map, setting the backgrounds, initial position .. See [Maps configuration](maps-configuration.md#map-configuration) for more information.
* `pluginsConfig.json`: Allows to configure the context editor plugins list. See [Context Editor Configuration](context-editor-config.md#configuration-of-application-context-manager) for more information.

## Externalize Configurations

Typically configuration customization should stay outside the effective application installation directory to simplify future updates. Updates in fact are usually replacement of the old application file package with the newer one. Changes applied directly inside the application package may be so removed on every update. For this reason MapStore provides a externalization system for both the configuration systems. See [Externalize Configuration](externalized-configuration.md#externalized-configuration) section to learn how to do this.
