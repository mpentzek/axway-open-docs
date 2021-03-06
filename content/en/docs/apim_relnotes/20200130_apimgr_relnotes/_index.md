{
"title": "API Gateway and API Manager 7.7.20200130 Release Notes",
"linkTitle": "API Gateway and API Manager 7.7.20200130",
"no_list": "true",
"weight": "20",
"date": "2019-09-20",
"description": "Learn about the new features and enhancements in this release of API Gateway and API Manager."
}

## Summary

API Gateway is available as a software installation or a virtualized deployment in Docker containers. API Manager is a licensed product running on top of API Gateway, and has the same deployment options as API Gateway.

The software installation is available on Linux. For more details on supported platforms for software installation, see [System requirements](/docs/apim_installation/apigtw_install/system_requirements/).

Docker deployment is supported on Linux. For a summary of the system requirements for a Docker deployment, see [Set up Docker environment](/docs/apim_installation/apigw_containers/docker_scripts_prereqs/).

## New features and enhancements

The following new features and enhancements are available in this release.

<!-- Add the new features here -->

### Swagger 2.0 enhancements

API Manager imports, retains, and exports all Swagger v2.0 fields, except for vendor extensions.

### Open API Specification (OAS) 3.0 enhancements

* API Manager imports, retains, and exports all Open API Specification (OAS) v3.0 fields, except for vendor extensions, callbacks, links, and examples.
* Parameter content types are supported in OAS3.

### Try it improvements

In API Manager, the  **Try it** and **Try method** capabilities support the rendering of `enum`, which allows you to send multipart forms.

* When trying the method of an API, you can select files as part of the request.
* The parameters object types are autogenerated in the UI, and nested schemes and arrays are rendered fully.
* The default for parameters are fully supported.
* The `allOf` and `anyOf` in the request bodies are also supported.

### Usage tracking improvements

Health checks are no longer counted in usage tracking.

Two new environment variables have been introduced:

* EMT_HEALTHCHECK_PORT (allowable range 1025 to 65535 inclusive)
* EMT_HEALTHCHECK_PATH (a path that begins with `/` character and includes 0 or more characters)

Both of these environment variables are optional and configurable with defaults of 8080 and `/healthcheck`. The endpoint configured here is not billed as part of usage tracking.

### Back-end API improvements

The API Manager UI supports OAS3 `response.content.schemes`.

* The OAS3 multiple back-ends are rendered on the screen, which allows you to select the required URL.
* The UI has been extended to include all response codes available in OAS3.
* Multipart request bodies are rendered in the back-end UI.
* The UI allows you to define `allOf` response types for Swagger 2.
* The `DataTypes` in API Manager have been changed to align with the OAS3 data types.
* You have the option to modify all back-end APIs without cloning.

### API documentation enhancements

All API Manager APIs are represented as both Swagger 2 and OAS3 (previously these APIs were only available in Swagger 2 format). The OAS3 representation of the APIs provides additional information and are better aligned to the <https://editor.swagger.io/> standard.

### Traffic monitor externalization showcase

An example dashboard for Elasticsearch leverages existing capabilities to output traffic monitor data using the open logging functionality and showcases this capability.

Easy to integrate with and to set up, [ELK](https://www.elastic.co/what-is/elk-stack) enables you to extend your analytics needs using easy customization options.

The showcase provides a HTML version of the Traffic Monitor dashboard to visualize data, while demonstrating how you can leverage ELK to increase performance and storage capacity.

{{< alert title="Note" color="primary" >}}
This showcase is not for use in a production environment. It is designed to run locally alongside a running API Gateway for demo purposes. Auto scaling in a multi-node environment requires custom configuration of Elasticsearch indexes.
{{< /alert >}}

### Multi organization beta

A new beta version (1.4) version of the following APIs are shipped with this release.

* The `user` API facilitates the GET, POST, UPDATE, and DELETE of additional organizations and roles.
* The `currentuser` API is used by API Manager and returns the organizations and role information as part of a validation check.
* The `apirepo` API encapsulates all of the actions that can be performed to manage a back-end API in API Manager. You can pass in an `organizationId={uuid}` to filter by organization, or specify no `{uuid}` to return all of the back-end APIs for all organizations that the user is a member of.
* The `discovery` API manages all APIs in the API Catalog and all virtualized front-end APIs. This API also provides the ability to return all APIs associated with a user, or to filter by an `organizationId={uuid}`.

These APIs manage two new variables, the `orgs2Role` and `orgs2Name` maps. The user is still assigned to a primary organization as in earlier versions, however, the new variables store additional organizations and the user's role within each organization.

The beta 1.4 version of the APIs are generated in OAS3 format and published on the [swagger-ui page](http://apidocs.axway.com/swagger-ui/index.html).

To enable the 1.4 beta APIs in Policy Studio, browse to the `API Portal v1.4` Servlet and set the `com.axway.portal.servlet.disabled` flag to false.

{{< alert title="Note" color="primary" >}}
Do not enable this flag on a production environment. Use these APIs only in test environments. Feedback on the implementation is welcome.
{{< /alert >}}

## Important changes

It is important, especially when upgrading from an earlier version, to be aware of the following changes in the behavior or operation of the product in this release.

<!-- Use this section to describe any changes in the behavior of the product (as a result of features or fixes), for example, new Java system properties in the jvm.xml file. This section could also be used for any important information that doesn't fit elsewhere. -->

### Increased validation of WSDLs

In this release the xerces library has been updated to `xerces 2.12.0`. This library enforces stricter rules when validating malformed schemas. This means that some WSDLs that were previously imported successfully by API Manager might not import successfully in this version.

To suppress schema validation errors and relax the stricter validation of XML files, set the flag `-DwsdlImport.suppressSchemaValidationErrors` to `true` in the `policystudio.ini` file. The default value is `false`.

### Filebeat v6.2.2

Filebeat has been updated to use v6.2.2. When installing Filebeat, follow the [official Filebeat documentation](https://www.elastic.co/guide/en/beats/filebeat/6.6/index.html).

### Increased validation of `/users` endpoint

In earlier versions of the product the `/users` API returns a list of all the users in an organization. This endpoint was used to allow a user share an application with other users in their organization. This was identified as a security risk,  as only `OrgAdmin` and `APIAdmin` roles should have access to a list of users, and not `User` roles. For this reason we have removed the ability for `Users` to view all other user names in the organization. This change might break some use cases for API Manager and API Portal.

To reduce the impact of this change, you can relax this restriction using a configuration flag. Set the flag in the `jvm.xml` file (it does not exist by default) under `groups/group-x/instance-y/conf`.

```xml
<ConfigurationFragment>
    <VMArg name="-DAPIGW_TOGGLE_FEATURE_GET_ALL_USERS=true" />
</ConfigurationFragment>
```

## Limitations of this release

This release has the following limitations.

<!-- Add any limitations here -->

## Deprecated features

<!-- Add features that are deprecated here -->

As part of our software development life cycle we constantly review our API Management offering.

The following capabilities have been deprecated:

* API Gateway already supports the industry standard Internet Content Adaption Protocol (ICAP), so from the November 2020 release we will remove the existing embedded Anti-Virus scanners:
    * McAfee
    * Sophos
    * Clam AV

    Content scanning is still supported using the ICAP filter, which provides out-of-the-box integration with ICAP-capable servers provided by Symantec, McAfee, OPSWAT and others, promoting ease of deployment and operational control.

## Removed features

<!-- Add features that are removed here -->

To stay current and align our offerings with customer demand and best practices, Axway might discontinue support for some capabilities.

As part of this review, the following capabilities have been removed:

* API Tester - For testing APIs, it is recommended to use alternative tools, such as Postman, SoapUI, or API Fortress.
* RAML support - RESTful API Modeling Language (RAML) support has been removed in favour of widely-adopted standards like Swagger and OpenAPI 3.
* The functionality to export back-end APIs converts all API formats to Swagger 1. With the introduction of OAS3, API Manager uses the `io.swagger.parser.v3.swagger-parser-v3:2.0.16` and `io.swagger.swagger-parser:1.0.48` libraries during the import process. This means that the export of back-end APIs is not supported for OAS3 or WSDL APIs, as this functionality relied on custom code in the old parser that is no longer available.
* Documentation is no longer provided in PDF format. You can continue to save individual topics or entire guides in PDF format using the **Save as PDF** icon on the [Axway documentation portal](https://docs.axway.com/).

## Fixed issues

<!-- Fixed issues are maintained in another topic -->

See [Fixed issues](/docs/apim_relnotes/20200130_apimgr_relnotes/fixed_issues/) for a complete list.

## Known issues

The following are known issues for this release.

<!-- Add the known issues here -->

## Update a classic (non-container) deployment

These instructions apply to API Gateway and API Manager classic deployments only. For container deployments, see [Apply a patch or service pack](/docs/apim_installation/apigw_containers/container_patch_sp/).

### Prerequisites

This service pack has the following prerequisites in addition to the [System requirements](/docs/apim_installation/apigtw_install/system_requirements/).

1. Shut down any Node Manager or API Gateway instances on your existing installation.
2. Back up your existing installation. For details on backing up, see [API Gateway backup and disaster recovery](/docs/apim_administration/apigtw_admin/manage_operations/#api-gateway-backup-and-disaster-recovery).

    Ensure that you back up any customized files. You should merge updated files instead of copying them back directly to avoid any regex matching issues. For example, the following directories might contain customized files:

    ```
    webapps/apiportal/vordel/apiportal
    webapps/emc/vordel/manager/app
    webapps/emc
    system/conf/apiportal/email
    system/conf
    samples/scripts/
    tools/filebeat-VERSION-PLATFORM
    ```

3. Remove old third-party libraries by deleting the following directories:

    ```
    INSTALL_DIR/apigateway/system/lib/modules
    INSTALL_DIR/analytics/system/lib/modules
    ```

4. Remove old JRE versions by deleting the following directories:

    ```
    INSTALL_DIR/apigateway/platform/jre
    ```

5. If you have an existing Apache Cassandra installation, ensure that you back up your data (Cassandra and `kpsadmin`), and that the `JAVA_HOME` variable is set correctly in `cassandra.in.sh` and `cassandra.in.bat`.
6. On Linux, remove existing capabilities on product binaries (which might prevent overwriting files):

    ```
    setcap -r INSTALL_DIR/apigateway/platform/bin/vshell
    ```

### FIPS mode only

If FIPS mode is enabled, you must also perform the following steps to install the service pack:

1. Run `togglefips --disable` to turn FIPS mode off.
2. Start the Node Manager to move the JARs.
3. Stop the Node Manager.
4. Install the API Gateway service pack.
5. Start the Node Manager.
6. Stop the Node Manager.
7. Run `togglefips --enable` to turn FIPS on again.
8. Start the Node Manager.

### Installation

This section describes how to install the service pack on existing 7.7 installations of API Gateway or API Manager.

* If you have installed an existing version of API Manager, installing the API Gateway server service pack automatically also installs the updates and fixes for API Manager.
* If you have installed a licensed version of API Gateway or API Manager 7.7, you do not require a new license to install service packs.

#### Install the API Gateway server service pack

To install the service pack on your existing API Gateway 7.7 server installation, perform the following steps:

1. Ensure that your existing API Gateway instance and Node Manager have been stopped.
2. Remove any previous patches from your `INSTALL_DIR/ext/lib` and `INSTALL_DIR/META-INF` directories (or the `ext/lib` directory in an API Gateway instance). These patches have already been included in this service pack. You do not need to copy patches from a previous version.
3. Verify the owners of API Gateway binaries before extracting the service pack.

    ```
    ls -l INSTALL_DIR/apigateway/posix/bin
    ```

4. Using the same user who owns the API Gateway binaries, unzip and extract API Gateway 7.7 SP2 server over the `apigateway` directory in your existing installation directory . For example:

    ```
    tar -xzvf APIGateway_7.7_SP2_Core_linux-x86-64_BNYYYYMMDDn.tar.gz -C /opt/Axway-7.7/apigateway/
    ```

5. Change to the `apigateway` directory in your installation.

    ```
    cd INSTALL_DIR/apigateway
    ```

6. Run the post-install script, and ensure that the correct permissions are set:

    ```
    apigw_sp_post_install.sh
    ```

#### Install the Policy Studio service pack

To install the service pack on your existing Policy Studio installation, perform the following steps:

1. Shut down Policy Studio.
2. Back up your existing `INSTALL_DIR/policystudio` directory.
3. Remove old JRE versions by deleting the following directories:

    ```
    INSTALL_DIR/policystudio/jre
    ```

4. Unzip and extract API Gateway 7.7 SP2 Policy Studio over the `policystudio` directory in your existing API Gateway 7.7 installation directory. For example:

    ```
    tar -xzvf APIGateway_7.7_SP2_PolicyStudio_linux-x86-64_BNYYYYMMDDn.tar.gz -C /opt/Axway-7.7/policystudio/
    ```

5. Start Policy Studio with `policystudio -clean`

#### Install the Configuration Studio service pack

To install the service pack on your existing Configuration Studio installation, perform the following steps:

1. Shut down Configuration Studio.
2. Back up your existing `INSTALL_DIR/configurationstudio` directory.
3. Remove old JRE versions by deleting the following directories:

    ```
    INSTALL_DIR/configurationstudio/jre
    ```

4. Unzip and extract API Gateway 7.7 SP2 Configuration Studio over the `configurationstudio` directory in your existing API Gateway 7.7 installation directory. For example:

    ```
    tar -xzvf APIGateway_7.7_SP2_ConfigurationStudio_linux-x86-64_BNYYYYMMDDn.tar.gz -C /opt/Axway-7.7/configurationstudio/
    ```

5. Start Configuration Studio with `configurationstudio  -clean`

#### Install the API Gateway Analytics service pack

To install the service pack on your existing API Gateway Analytics 7.7 installation, perform the following steps:

1. Ensure that your existing API Gateway Analytics instance and Node Manager have been stopped.
2. Verify the owners of API Gateway binaries before extracting the service pack.

    ```
    ls -l INSTALL_DIR/analytics/posix/bin
    ```

3. Using the same user who owns the API Gateway Analytics binaries, unzip and extract API Gateway 7.7 SP2 Analytics over the `analytics` directory in your existing API Gateway 7.7 installation directory. For example:

    ```
    tar -xzvf APIGateway_7.7_SP2_Analytics_linux-x86-64_BNYYYYMMDDn.tar.gz -C /opt/Axway-7.7/analytics/
    ```

4. Change to the `analytics` directory in your installation:

    ```
    cd INSTALL_DIR/analytics
    ```

5. Run the post-install script for API Gateway Analytics.

    ```
    apigw_analytics_sp_post_install.sh
    ```

You must also install a service pack for your existing API Gateway 7.7 server.

### After installation

The following steps apply after installing the service pack.

#### API Gateway

To allow an unprivileged user to run the API Gateway on a Linux system, perform the following steps:

1. Add the following line to the `INSTALL_DIR/system/conf/jvm.xml` file:

    ```
    <VMArg name="-Djava.library.path=$VDISTDIR/$DISTRIBUTION/jre/lib/amd64/server:$VDISTDIR/$DISTRIBUTION/jre/lib/amd64:$VDISTDIR/$DISTRIBUTION/lib/engines:$VDISTDIR/ext/$DISTRIBUTION/lib:$VDISTDIR/ext/lib:$VDISTDIR/$DISTRIBUTION/jre/lib:system/lib:$VDISTDIR/$DISTRIBUTION/lib"/>
    ```

2. Run the command `setcap 'cap_net_bind_service=+ep cap_sys_rawio=+ep' INSTALL_DIR/platform/bin/vshell` to allow the API Gateway to listen on privileged ports.

##### JRE properties

The JRE included in API Gateway disables undesirable cipher suites when using SSL/TLS by default. Users using RSA Access Manager (formerly known as RSA ClearTrust) with API Gateway might experience SSL/TLS handshake issues where no common cipher suites can be found. In this case, you should reconfigure SSL/TLS of the RSA Access Manager to support stronger cipher suites.

Alternatively, to re-enable the anonymous cipher suites in JRE for successful SSL/TLS connections with the RSA Access Manager, remove `anon`  from the  `jdk.tls.disabledAlgorithms` Java security property in the  `INSTALL_DIR/Linux.x86_64/jre/lib/security/java.security` file.

The JRE included in API Gateway enables endpoint identification algorithms for LDAPS (secure LDAP over TLS) by default to  improve the robustness of the connections. This might cause API Gateway LDAP filters to fail to connect to an LDAPS server. To disable endpoint identification add the  `<VMArg  name="-Dcom.sun.jndi.ldap.object.disableEndpointIdentification=true"/>`  line to the  `INSTALL_DIR/system/conf/jvm.xml` file.

#### API Manager

When API Manager is installed, you must run the `update-apimanager` script after the API Gateway post-install script to ensure that all paths are up-to-date.

{{< alert title="Caution" color="warning" >}}
Before executing the `update-apimanager` script:

* Apply the service pack to all API Gateways.
* Ensure that all Node Managers and API Gateway instances are running.

{{< /alert >}}

This script updates the active deployment in the API Manager group. After running the script, you must recreate the API Manager project (common project, containing Server Settings) from the deployment, so that you will not need to revert the changes the next time you perform a project deployment.

As an alternative to recreating the API Manager project, you can deploy only your common project to a development server and run the `update-apimanager` script against it, and then create a new common project from this API Gateway instance. Finally, you must deploy your updated policies to your API Manager group.

You can run this command once at the API Gateway group level, instead of on every API Gateway instance, for example:

```
/opt/Axway-7.7/apigateway/posix/bin/update-apimanager --username=admin --password=MY_PASSWORD --group=API_MGR_GROUP
```

If the API Gateway group is protected by a passphrase, you must append the command with `--passphrase=API_MGR_GROUP_PASSPHRASE`.

The following command shows an example of running the `update-apimanager` script when the Client Application Registry is installed:

```
/opt/Axway-7.7/apigateway/posix/bin/update-apimanager --username=admin --password=MY_PASSWORD --group=API_MGR_GROUP   --productname=clientappreg
```

If the API Gateway group is protected by a passphrase, you must append the command with `--passphrase=API_MGR_GROUP_PASSPHRASE`.

## Update a container deployment

If a `fed` file is provided as part of building the API Manager container, you must follow these steps to update the `fed` with the Service Pack API Manager configuration:

1. Install the Service Pack on a installation of the API Gateway.
2. Run the following command:

    ```
    /opt/Axway-7.7/apigateway/posix/bin/update-apimanager --fed <path to old file>.fed --oa <path to update file>.fed
    ```

You do not need to run any API Manager instances.

The `fed` now contains the Service Pack updates for the API Manager configuration and can be used to build containers.

## Documentation

You can find the latest information and up-to-date user guides at the Axway Documentation portal at <https://docs.axway.com>.

This section describes documentation enhancements and related documentation.

### Documentation enhancements

<!-- Add a summary of doc changes or enhancements here-->

The latest version of API Gateway, API Manager, and API Portal documentation has been migrated to Markdown format and is available in a [public GitHub repository](https://github.com/Axway/axway-open-docs) to prepare for future collaboration using an open source model. As part of this migration, the documentation has been restructured to help users navigate the content and find the information they are looking for more easily.

Documentation change history is now stored in GitHub. To see details of changes on any page, click the link in the last modified section at the bottom of the page.

### Related documentation

To find all available documents for this product version:

1. Go to <https://docs.axway.com/bundle>.
2. In the left pane Filters list, select your product or product version.

Customers with active support contracts need to log in to access restricted content.

The following reference documents are also available:

* [Supported Platforms](https://docs.axway.com/bundle/Axway_Products_SupportedPlatforms_allOS_en)
    * Lists the different operating systems, databases, browsers, and thick client platforms supported by each Axway product.
* [Interoperability Matrix](https://docs.axway.com/bundle/Axway_Products_InteroperabilityMatrix_allOS_en)
    * Provides product version and interoperability information for Axway products.

## Support services

The Axway Global Support team provides worldwide 24 x 7 support for customers with active support agreements.

Email <mailto:support@axway.com> or visit Axway Support at <https://support.axway.com>.

See [Get help with API Gateway](/docs/apim_administration/apigtw_admin/trblshoot_get_help/) for the information that you should be prepared to provide when you contact Axway Support.
