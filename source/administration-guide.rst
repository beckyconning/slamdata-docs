.. figure:: images/white-logo.png
   :alt: SlamData Logo


Administrator's Guide
=====================

This Administrator's Guide can assist with installing and configuring
SlamData.

For information on how to use SlamData from a developer's perspective
see the `SlamData Developer's Guide <developers-guide.html>`__.

For information on how to use SlamData from a user's perspective
see the `SlamData User's Guide <users-guide.html>`__.

.. note:: **SlamData Advanced Features**

  Throughout this guide there are references to functionality available
  only in SlamData Advanced Edition.  Sections that apply only to SlamData
  Advanced Edition will be called out with the Murray (MRA)
  icon. |Murray-Small|


Section 1 - Installation
------------------------


1.1 Minimum System Requirements
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

  * Minimum memory
      * 2 GB memory
      * An additional 25 MB is required for each active user
  * Disk
      * 300 MB for a basic installation
      * Additional space varies based upon Workspace size, cached queries, and so on
  * Java
      * Java 1.8 or newer
      * Windows and Mac OS versions of SlamData with installers include Java
      * Linux requires a separate Java installation
  * Browsers
      * The most compatible browsers with SlamData are always the most recent versions of Google Chrome and Mozilla Firefox
  * Target data sources (for analytics)
      * Apache Spark 2.1 or newer
      * Couchbase x.x or newer
      * MarkLogic x.x or newer
      * MongoDB 2.6 or newer

**SlamData Community Edition** uses a file called ``quasar-config.json`` to
store server configuration data.

**SlamData Analyst Edition** and **SlamData Advanced Edition** store configuration
data in a metastore database. This should not be confused with target data
sources. The options for storing server configuration data are as follows:

  * Metastore data sources
      * Java H2 (included with SlamData)
      * PostgreSQL 9.x (must be installed and configured separately)


1.2 Obtaining SlamData
~~~~~~~~~~~~~~~~~~~~~~


1.2.1 Downloading the SlamData Installer
''''''''''''''''''''''''''''''''''''''''

A fully automated installer package can be obtained directly from the
SlamData website `here <http://slamdata.com/get-slamdata/>`__.


1.2.2 Building SlamData from Source
'''''''''''''''''''''''''''''''''''

1.2.2.1 Build Preparation
@@@@@@@@@@@@@@@@@@@@@@@@@

Before building SlamData, some required software must be installed.

1. Install
   `Node.js <https://docs.npmjs.com/getting-started/installing-node>`__
   version ~4.2, which includes the ``npm`` package manager

2. Install Bower

.. code-block:: shell

    npm install bower -g

3. Install Gulp

.. code-block:: shell

    npm install -g gulp

   

1.2.2.2 Build Process
@@@@@@@@@@@@@@@@@@@@@

.. note::

  If you wish to have the SlamData build process automatically
  download the required Quasar backend engine you will need to have
  a shell environment variable ``GITHUB_AUTH_TOKEN`` populated with
  the appropriate `authorization token <https://github.com/settings/tokens/>`__.

1. Obtain the latest SlamData code

.. code-block:: shell

    git clone https://github.com/slamdata/slamdata.git
    cd slamdata

2. Fetch the dependencies

Ensure that you are in slamdata directory.

.. code-block:: shell

    bower install
    npm install

3. Build the code

.. code-block:: shell

    npm i && bower i && gulp make && gulp bundle && gulp less


After this task completes, the ``public`` directory will contain the complete
SlamData front-end application.


1.3 Starting SlamData
~~~~~~~~~~~~~~~~~~~~~

SlamData is comprised of a front-end interface and a back-end analytics
engine. Starting the SlamData application will start both.


1.3.1 Starting SlamData from Source
'''''''''''''''''''''''''''''''''''

If SlamData is installed from source, the launch process is the same
on all operating systems.  After successfully building SlamData:

1. Change directory to the directory created by ``git clone``.
2. Start SlamData:  ``java -jar ./jars/quasar.jar --content-path public``.

A message similar to the following should be displayed:

::

    Server started listening on port 20223
    Press Enter to stop.


1.3.2 Starting SlamData from the Installer Package
''''''''''''''''''''''''''''''''''''''''''''''''''

**Mac OS**

1. Open the Applications folder.
2. Double-click on the SlamData icon.

A new browser window or tab will open displaying the SlamData interface.
The SlamData icon will appear in the Mac OS dock. As with other dock
applications the SlamData icon may be right-clicked and the application
terminated.

**Microsoft Windows**

1. Open the Start menu.
2. Click on the newly installed SlamData icon or use the search bar
   and type ``slamdata`` and press return to launch it. Select
   appropriate network security settings if prompted.

**Linux**

1. Change directory to the location of the SlamData executable: ``cd SlamData<version>``.
2. Execute the SlamData executable: ``./SlamData``.

Some Linux systems may not launch a browser automatically. If this is
the case, open a browser and point it to the following URL: http://localhost:20223/slamdata


1.3.3 Starting SlamData Advanced from the Command Line
''''''''''''''''''''''''''''''''''''''''''''''''''''''

SlamData Advanced Edition requires license key information before
launching.  This information is passed into the JVM at startup.
An example of how this can be done is shown below.  Note the use of
escaping the quote characters with ``\"``.

.. code-block:: bash

    _JAVA_OPTIONS="-Xms1G -Xmx4G"

    export SD_OPTS="\
    -Dlicense_key=ABCDE-12345-ABCDE-12345-ABCDE \
    -Dlicense_email=myemail@example.com \
    -Dlicense_full_name=\"My Name\" \
    -Dlicense_registered_to=\"Name Registered To\" \
    -Dlicense_company=\"My Company Name\" \
    -Dlicense_street=\"123 Anywhere Street, Suite A1\" \
    -Dlicense_tel_number=3035551212 \
    -Dlicense_fax_number=NA \
    -Dlicense_city=Boulder \
    -Dlicense_zip=80302 \
    -Dlicense_country=US"

    export _JAVA_OPTIONS="$_JAVA_OPTIONS $SD_OPTS"

    java -jar quasar.jar --content-path public


Section 2 - Connecting to a Data Source
---------------------------------------

Connecting to a data source is the first step to analyzing data.


2.1 Data Sources
~~~~~~~~~~~~~~~~

Supported data sources are listed in the following sections.  As new
target data sources are released, they will be listed below.


2.1.1 MongoDB
'''''''''''''

To connect to MongoDB click on the Mount |Mount-Icon| icon in the upper
right.

A mount dialog will be presented, as shown below.

.. figure:: images/SD4/screenshots/mount-dialog-start.png
   :alt: SlamData Mount Dialog

Enter a name for the data source mount. This name is used in the SlamData
User Interface (UI) as well as SQL² query paths.

.. hint:: **Mount Name**

  Use a name that makes sense for the environment. For example,
  if a data source were hosted on Amazon AWS/EC2 it might be named
  ``aws`` or ``aws-1``.

Select **MongoDB** as the mount type. Once the mount type has been selected,
additional fields will appear in the dialog. The following form shows
example values for the remaining fields.

.. figure:: images/SD4/screenshots/mount-mongodb.png
   :alt: SlamData MongoDB Dialog

.. note::

  When using MongoDB, the database field value should be the
  database the username and password will authenticate against. This value
  will depend on which database the user was created in. For example,
  it could be ``admin``, the name of the user or something completely different.

Click the **Mount** button to mount the database in SlamData.


2.2 Several Mounts
~~~~~~~~~~~~~~~~~~

After mounting several data sources, the SlamData UI might look like the
following image. In this image, there are two separate mounts named
``aws`` and ``macbook``, the latter representing a
locally mounted data source.

.. figure:: images/SD4/screenshots/mount-all-mounts.png
   :alt: SlamData Multiple Mounts


2.3 Mount Options
~~~~~~~~~~~~~~~~~

The mount dialog will display the appropriate fields based upon the mount
type selected. For each data source that SlamData supports, a section
below describes the options available.

2.3.1 MongoDB
'''''''''''''

The MongoDB values listed in the Connection Options on the MongoDB
web site are supported. As of MongoDB 2.6 these options are as follows.

+------------------+---------+--------------------------------------------------------------------+
| Options          | Example | Description                                                        |
+==================+=========+====================================================================+
| ssl              | true    | Enable SSL encryption.                                             |
+------------------+---------+--------------------------------------------------------------------+
| connectTimeoutMS | 15000   | The time in milliseconds to attempt a connection before timing out.|
+------------------+---------+--------------------------------------------------------------------+
| socketTimeoutMS  | 10000   | The time in milliseconds to attempt a send or receive on a socket  |
|                  |         | before the attempt times out.                                      |
+------------------+---------+--------------------------------------------------------------------+


2.4 SQL² View
~~~~~~~~~~~~~

SQL² Views are covered in detail in the `SlamData Developer's Guide <developers-guide.html>`__.


2.5 Enabling SSL
~~~~~~~~~~~~~~~~

If you have difficulty following the steps below, you may also view the `SSL
tutorial video <https://www.youtube.com/watch?v=FWdAMyZnOMM>`__.

If a data source connection supports SSL encryption, that is to say
encryption between a client and server such as SlamData and the
data source, additional configuration will be required.

The backend engine of SlamData is written in
`Scala <http://www.scala-lang.org/>`__ and executes within a Java
Virtual Machine (JVM). To enable SSL encryption, several options must be
passed to the JVM when running SlamData. SlamData simplifies this by
allowing these options to be listed in a text file that the SlamData
launcher will reference when executed. The file location for each
operating system is shown in the following table.

+-------------------------+------------------------------------------------------------------+
| Operating System        | File Location                                                    |
+=========================+==================================================================+
| Mac OS                  | /Applications/SlamData <version>.app/Contents/vmoptions.txt      |
+-------------------------+------------------------------------------------------------------+
| Microsoft Windows       | C:\\Programs Files (x86)\\slamdata <version>\\SlamData.vmoptions |
+-------------------------+------------------------------------------------------------------+
| Linux (various vendors) | $HOME/slamdata<version>/SlamData.vmoptions                       |
+-------------------------+------------------------------------------------------------------+

There are two important options that must be passed to the JVM at
startup to enable SSL. These options are shown in the table below
and point the JVM to a Java Key Store (JKS).

+----------------------------------+---------------------------------------------------------+
| JVM Option                       | Purpose                                                 |
+==================================+=========================================================+
| javax.net.ssl.trustStore         | The location of the encrypted trust store file.         |
+----------------------------------+---------------------------------------------------------+
| javax.net.ssl.trustStorePassword | The password required to decrypt the trust store file.  |
+----------------------------------+---------------------------------------------------------+

Example values for these two options could be as shown in the code below.

::

    -Djavax.net.ssl.trustStore=/users/me/ssl/truststore.jks
    -Djavax.net.ssl.trustStorePassword=mySecretPassword

This guide does not provide exhaustive steps to create a Java Key Store
in every scenario, but the following simple example should be helpful.

Let's consider a data source hosted with a service provider. That service provider
makes a signed (or self-signed) certificate available so that the data source
can connect securely using SSL. Using the JKS configuration described above, the
``your_provider.crt`` text file could be created as follows.

1. Import the certificate into the Java trust store, as follows.

::

    keytool -import -alias "your_providers_name" -file your_provider.crt \
    -keystore /users/me/ssl/truststore.jks -noprompt -storepass mySecretPassword

2. Ensure that the appropriate changes have been made to the JVM options file referenced above.

3. Restart SlamData so it reloads the JVM options file and picks up the new certificate in the JKS.

4. Mount the data source with SSL as shown in the following image. This example uses MongoDB.

.. figure:: images/SD4/screenshots/mount-ssl.png
   :alt: SlamData SSL Mounts



Section 3 - Configuring SlamData
--------------------------------


3.1 Community Edition Configuration File
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The SlamData configuration file allows an administrator to change
settings, such as the port number SlamData listens on, the mounts
available, and so on. The location of the configuration file depends upon
the operating system being used, as shown in the table below.

+-------------------------+-------------------------------------------------------------+
| Operating System        | File Location                                               |
+=========================+=============================================================+
| Mac OS                  | $HOME/Library/Application Support/quasar/quasar-config.json |
+-------------------------+-------------------------------------------------------------+
| Microsoft Windows       | %HOMEDIR%\\AppData\\Local\\quasar\\quasar-config.json       |
+-------------------------+-------------------------------------------------------------+
| Linux (various vendors) | $HOME/.config/quasar/quasar-config.json                     |
+-------------------------+-------------------------------------------------------------+

An example configuration file for **SlamData Community Edition** is shown below.


::

    {
      "server": {
        "port": 8080,
        "ssl": {
          "enabled": true,
          "port": 9090,
          "cert": "<base64 encoded pkcs12 cert file>"
        }
      },

      "mountings": {
        "/aws/": {
          "mongodb": {
            "connectionUri": "mongodb://myUser:myPass@aws-box.example.com:27017/admin"
          }
        },
        "/macbook/": {
          "mongodb": {
            "connectionUri": "mongodb://localhost:27017"
          }
        }
      },
    }


3.2 Advanced Edition Configuration File
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**SlamData Advanced Edition**
has additional configuration parameters to setup security, including the
``authentication``, ``auditing`` and ``metastore`` directives.

.. attention:: **SlamData Advanced Features**

  The configuration file listed below is applicable only
  to SlamData Advanced Edition and contains parameters and
  values that are valid only in that version.
  
  |Murray-Small|


An example configuration file for **SlamData Advanced Edition** might appear
as follows.

::

    {
      "server": {
        "port": 8080,
        "ssl": {
          "enabled": true,
          "port": 9090,
          "cert": "<base64 encoded pkcs12 cert file>"
        }
      },

      "authentication": {
        "openid_providers": [
          {
            "issuer": "https://accounts.google.com",
            "client_id": "123...googleusercontent.com",
            "display_name": "Google",
          },
          {
            "issuer": "https://accounts.google.com",
            "client_id": "456...789.apps.googleusercontent.com",
            "display_name": "OAuth 2.0 Playground"
          }
        ]
      },
      {
        "display_name": "Our Company OP",
        "client_id": "123455976",
        "openid_configuration": {
          "issuer": "https://op.ourcompany.com",
          "authorization_endpoint": "https://op.ourcompany.com/authorize",
          "token_endpoint": "https://op.ourcompany.com/token",
          "userinfo_endpoint": "https://op.ourcompany.com/userinfo",
          "jwks": [
            {
              "kty": "RSA",
              "kid": "1234",
              "alg": "RS256",
              "use": "sig",
              "n": "2354098udw...2957835lkj"
            },
            {
              "kty": "RSA",
              "kid": "5678",
              "alg": "RS256",
              "use": "sig",
              "n": "skljhdfiugy...39587dlkjsd"
            }
          ]
        }
      },

      "auditing": {
        "log_file": "/aws/logdb/slamdata-logs"
      },

      "metastore": {
        "database": "<h2 config | postgresql config>"
      }
    }


Section 4 - SlamData User Security
----------------------------------

**SlamData Advanced Edition** provides additional features not available in other editions,
such as user authorization, authentication, and auditing.

.. attention:: **SlamData Advanced Features**

  SlamData User Security is available only with SlamData Advanced Edition.

  |Murray-Small|


4.1 Security Overview
~~~~~~~~~~~~~~~~~~~~~

**SlamData Advanced Edition** controls user security through the use of
tokens, permissions, groups, actions and types. Each of these is defined in the table below.

+------------+----------------------------------------------------------------------------------+
|            | Description                                                                      |
+============+==================================================================================+
| Token      | Allows specific actions regardless of implicitly-assigned or explicitly-assigned |
|            | permissions.                                                                     |
+------------+----------------------------------------------------------------------------------+
| Permission | Contains actions, users and groups.                                              |
+------------+----------------------------------------------------------------------------------+
| Group      | Contains users and other groups.                                                 |
+------------+----------------------------------------------------------------------------------+
| Action     | Distinct operation(s) that can be performed on a resource based upon its type.   |
+------------+----------------------------------------------------------------------------------+
| Type       | `Structural`, `Content`, or `Mount`.                                             |
+------------+----------------------------------------------------------------------------------+


4.1.1 Users
'''''''''''

Users are technically not objects stored in the SlamData metadata repository.
Since SlamData relies on OAuth to authenticate users, it trusts the OpenID
Provider to authenticate a user and state if the user is currently logged-in.

Once logged-in, a user may perform actions depending upon the configuration of groups and
permissions.  Users are not created in the metadata store, but references
to them are listed within Groups and Permissions.  So while technically a user does
not have an object in the metadata store, logically a user can be thought of as
an object with privileges provided by Groups, Permissions, and possibly
Tokens (when supplied with a request).


4.1.2 Groups
''''''''''''

Groups contain users and other groups which are in the path (subgroups).

|SD-Group-Example|

Since permissions may contain a group, and groups may contain users, then a user
within a group inherits the permissions assigned to that group.

In the example above, both users ``John`` and ``Jack`` would inherit all of the
permissions that contain the ``/engineering`` group.  Those permissions would
also apply to the subgroups for ``John`` and ``Jack``.

The users ``Sayid``, ``Kate``, and ``Sawyer`` would inherit all of the permissions
that contain the ``/engineering/frontend`` group, but would not inherit the
permissions "above" from ``/engineering``.


4.1.3 Permissions
'''''''''''''''''

|SD-Permission-Example-1|

In the example above, permission 150 contains several actions and the user ``John``.  This
allows John to perform all actions listed, which includes any operation under the ``/John`` path.

|SD-Permission-Example-2|

In the example above, both the user ``Damon`` and any other user within the ``/support``
group may read data from the ``/customers`` path, but may not create, modify
or delete anything.


4.1.4 Tokens
''''''''''''

If a token is passed in a request to SlamData, and the token is valid, the request
will proceed based upon the permissions assigned to that token.

In other words, if a user is trying to read from the ``/data`` mount, but does not
have permissions through direct assignment or through group assignment, if the appropriate
token with those permissions is passed into the same request, it will succeed.

In the following example, if a request included the token ``A1``, then any operation performed
within ``/priv`` would succeed, despite the permissions the user actually had.

|SD-Token-Example|



4.2 Initializing the SlamData Metastore
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

**SlamData Advanced Edition** uses a metastore for user security.  Before **SlamData
Advanced Edition** can be started, the metadata store must be initialized and
initial administrator users defined.  The administrator users are added
to a group having complete and unrestricted access to the system allowing them
to provision additional groups and roles as needed.

To initialize the metadata store, run the ``bootstrap`` command and provide
the name of the administrator group and e-mail addresses of initial members,
as shown in the following example.

::

    java -jar quasar.jar bootstrap --admin-group <name> --admin-users user1@example.com[,user2@example.com,...]


4.3 Authentication
~~~~~~~~~~~~~~~~~~

**SlamData Advanced Edition** adds support for authenticated requests via the
`OpenID Connect <http://openid.net/connect/>`__ protocol. A request to any
SlamData or **SlamData Advanced Edition** API may be authenticated. If no
credentials are included in a request, it is considered unauthenticated
(or "anonymous") and may fail if the system is not configured to allow
anonymous access for the given request.


4.3.1 Making an Authenticated Request
'''''''''''''''''''''''''''''''''''''

To make an authenticated request, clients first need to ensure their
OpenID Provider (OP) has been configured in **SlamData Advanced Edition** along
with the "Client Identifier" (CID) issued to the client by the OP, this
allows the **SlamData Advanced Edition** administrator to specify which clients
are permitted to access **SlamData Advanced Edition**. If an ID Token is received
from a known provider but with an unknown CID, it will be rejected outright.

Next, the client should obtain the list of known providers from the
``/security/oidc/providers`` endpoint (see details on this endpoint below)
and authenticate the user against one of them, obtaining an
`ID Token <http://openid.net/specs/openid-connect-core-1_0.html#IDToken/>`__
The ID Token **MUST** be requested using at least the openid and email scopes and
their claims must be included in the ID Token.

Once in possession of a valid ID Token, the client includes it, verbatim,
in the request to **SlamData Advanced Edition** via the ``Authorization`` header
as a
`bearer token <http://self-issued.info/docs/draft-ietf-oauth-v2-bearer.html>`__
using the ``Bearer`` scheme.

If a request includes valid authentication and the identified subject is not
permitted to perform the requested action per the authorization policy,
a ``403 Forbidden`` response will be returned. If, however, a request which
does not include any authentication information is denied due to the
authorization policy a ``401 Unauthorized`` response will be returned to
indicate that repeating the request with authentication may allow it to
succeed.


4.3.1.1 Authentication and Performance
''''''''''''''''''''''''''''''''''''''

**SlamData Advanced Edition** requests require authentication before performing
most actions.  When an OIDC Provider (OP) is configured with minimal
information, and the Discovery process is used, each action will make
a discovery request as well.  This can result in a noticeable degradation
in performance.

To avoid this, the OP can be configured with all attributes normally
provided by the OIDC Discovery process within the configuration process
itself.  See the "Our Company OP" example in Section 3.2.


4.4 Authorization
~~~~~~~~~~~~~~~~~

**SlamData Advanced Edition** adds support for authorization of service requests.
Permissions for a request are derived from the union of permission tokens
provided in the `X-Extra-Permissions` header and those configured for the
authenticated user and anonymous user. Permissions are defined as an
operation, its type, and a filesystem resource path. A permission token
grants a set of permissions.

The available operations and types are as follows.

**Type**: Content, Structural, Mount

**Operation**: Add, Read, Delete, Modify

+--------+----------------------+-------------------------+----------------------+
|        | Content              | Structural              | Mount                |
+========+======================+=========================+======================+
| Add    | append to file       | create resource         | create mount         |
+--------+----------------------+-------------------------+----------------------+
| Read   | read file contents   | list directory          | retrieve mount info  |
+--------+----------------------+-------------------------+----------------------+
| Delete | delete file contents | delete resource         | remove mount         |
+--------+----------------------+-------------------------+----------------------+
| Modify | modify file contents | rename or move resource | Not Available        |
+--------+----------------------+-------------------------+----------------------+

A permission on a parent resource is sufficient to authorize an action on a
resource granted the nature and type of the operation are the same.

A ``403 Forbidden`` is returned by the server when a request does not have
sufficient permissions to perform the associated actions.

The ``X-Extra-Permissions`` header is formatted as follows.

``X-Extra-Permissions: [token1],[token2]``


4.5 Auditing
~~~~~~~~~~~~

.. attention:: **File System Definition**

  The SlamData product sometimes refers to virtual database paths
  as file systems and tables or collections as file names.  In the
  Auditing section below, the **log file** path should be a
  path to the collection or table you wish to save to.  This does
  not equate to an operating system file name or directory path.

When a log file is specified in the configuration file, all filesystem
operations will be logged to that file. **SlamData Advanced Edition** logs the
operations as data in the filesystem where the path is located. This
means that it is then possible to use **SlamData Advanced Edition** to
analyze the log data.


Section 5 - Security APIs
-------------------------

**SlamData Advanced Edition** provides additional APIs to control user access.

Actions and permissions are central concepts to the security api. An action
is any operation a subject can perform on a given resource in the system.
A permission represents the capability of a subject (group, user, token)
in the system to perform a given action. All permissions have a lineage
which represents by which authority a permission was granted to a subject.
Any subject in the system has the authority to grant a new permission which
is a subset of one of their own permissions. This new permission is said to
have been derived from the relevant permission(s) of the grantor and
that/those relevant permission(s) are said to be the parent(s) of that
permission.

Permissions can be revoked. If a permission is revoked, that permission as
well as all permissions derived from it become invalid and can no longer be
used to perform operations in the system. It is possible however for one of
those derived permissions to have been derived from more than one permission,
i.e. another permission than the one being revoked. In such a case, that
permission will not become invalid. It will only become invalid once all
its parents have been revoked. The permission being revoked however, will
be revoked, no matter how many sources of authority it possess.

Actions and permissions are found throughout the following api endpoints
and are represented as follows in JSON.

**Action**

.. code:: json

    {
      "operation": "ADD|READ|MODIFY|DELETE",
      "resource": "<filesystem_path>|<group_path>",
      "accessType": "Structural|Content|Mount",
    }

**Permission**

.. code:: json

    {
      "id": "<permission_id>",
      "action": {
        "operation": "ADD|READ|MODIFY|DELETE",
        "resource": "<filesystem_path>|<group_path>",
        "accessType": "Structural|Content|Mount",
      },
      "grantedTo": "<user_id>|<group_path>|<token_id>",
      "grantedBy": ["<user_id>", "<group_path>", "<token_id>", "..."]
    }

* **<filesystem_path>** is a path in the quasar virtual filesystem such as
  ``data:/foo/bar`` for a file and ``data:/foo/bar/`` for a directory

* **<group_path>** is a path uniquely identifying a group and its location
  in the group hierarchy such as ``group:/engineering/backend``

* **<grantedBy>** The sources of authority by which this permission was
  granted. In reality, the sources are the parent permissions; here we are
  simply surfacing the subjects which possess the permissions by which this
  permission was granted.

* **<user_id>** is an email prefixed with the "user" string such as
  ``user:bob@example.com``

* **<token_id>** is a string identifier prefixed by the "token" string such
  as ``token:786549382``

.. note::

  The Mount value of accessType is only valid if the resource is a
  filesystem path. It is not a valid value for a group resource.

In the following API endpoints descriptions, "your permissions" refers to
the set of permissions associated with the HTTP request. In the case of an
authenticated user, this means all permissions directly associated with that
user as well as all groups that user is a explicitly or implicitly a part
of. Additionally, any permission associated with tokens present in the request
headers are added to the permissions associated with the request.

Whenever no return body is specified, a response with a ``2XX`` status can be
expected along with an empty body.

In any of the following endpoints, if the request does not "carry" sufficient
permissions to satisfy the requirements of the particular endpoint, the server
will return a ``403 Forbidden`` with an explanation of which permissions were
missing in order to perform the operation. Certain endpoints will always
succeed, but the results will be filtered based on what the user is
permitted to see. In such a case, the endpoint will document how to determine
what a user can and cannot see.


5.1 - Group Endpoint
~~~~~~~~~~~~~~~~~~~~

**GET /security/group/<path>**

* Retrieves information about this group. The result of the query will depend
  upon your permissions according to the rules described below.

* If you have READ content group permission on this group, then your view is
  unrestricted. (all fields are present).

* If you have READ structural group permission on this group, then you can
  know of the existence of this group and all of its sub-groups. (``subGroups``
  field is present in response).

* If you have ANY OTHER group permission on this group, you can know of the
  existence of this group, but nothing else. (response is empty).

* If you have READ content group permission on one of this group's sub-groups,
  then you can see that subgroup as well as any of its own subgroups. You can
  see all members of that group and sub-groups. (``allMembers`` and ``subGroups``
  fields are present in response).

* If you have READ structural group permission on one of this group's sub-groups,
  then you can see that subgroup as well as any of its own sub-groups. You
  cannot see any of the members of those groups however. (``subGroups`` field is
  present in response).

* If you have ANY OTHER group permission on one of this group's sub-groups,
  then you can see that subgroup.

These rules are cumulative, so if more than one rule applies, you will see the
combined result. If none of the rules apply, the query will result in a
``403 Forbidden``. If certain fields do not apply to your view of this group,
they will be omitted in order to clearly convey that they are not necessarily
empty, you just don't have permission to see anything related to that field.

* ``<path>`` is the path of the group in the group hierarchy

.. note::

  All users are members of the root group ("/") regardless of whether
  they are a member of any other group. Permissions associated with the root
  group represent the capabilities of any agent in the system.

Response:

The response body will vary depending on the rules outlined above. If you
have some relevant permission as outlined above and the group does
not exist, the response will be a ``404 Not Found``.

.. code:: json

    {
      "members": ["<user_email>", "..."],
      "allMembers": ["<user_email>", "..."],
      "subGroups": ["<group_path>", "..."],
    }

* ``members`` All users are explicitly a member of this group.

* ``allMembers`` All users are explicitly and implicitly a member of this group.
  Implicit members of a group refer to the users that are explicit members
  of any of the sub-groups of this group.

* ``subGroups`` All descendants of this group in the group hierarchy.

Example:

Given the following groups exist in the system:

/corporate -> "Alice" /corporate/engineering -> "Bob" /corporate/engineering/software -> /corporate/engineering/software/scala -> "Marcy" /corporate/engineering/hardware -> ("Tom", "Beth")

``GET /security/group/corporate/engineering`` will return the following:

::

  {
      "members": ["bob@example.com"],
      "allMembers": [ "bob@example.com",
          "marcy@example.com",
          "tom@example.com",
          "beth@example.com"
      ],
      "subGroups": [ "/corporate/engineering/software",
          "/corporate/engineering/software/scala",
          "/corporate/engineering/hardware"
      ]
  }

**POST /security/group/<path>**

Creates a new empty group. If any of the parent groups do not exist yet, they
will be created.

*Requires ADD or MODIFY structural group permission.*

Response:

If you have adequate permissions and the group already exists, will return
a ``400 Bad Request``.

**PATCH /security/group/<path>**

Add or remove users of a group.

*Requires ADD content group permission to add users. Requires DELETE
content group permission to remove users. Alternatively, the MODIFY
content group permission is sufficient to add and/or remove users.*

Request:

::

  {
    "addUsers": ["<user_email>"],
    "removeUsers": ["<user_email>"]
  }

Response:

If you have adequate permissions, but the group does not exist, the
response will be a ``404 Not Found``. If a user found in the removeUsers
field was not actually a member of the group, the request will
succeed nevertheless and simply ignore that user.

**DELETE /security/group/<path>**

Delete this group and all of its sub-groups. All permissions associated
with this group and subgroups as well as shared by this group and subgroups
will immediately become invalid.

*Requires DELETE or MODIFY structural group permission.*

Response:

If you have adequate permissions, but the group does not exist, the
response will be a ``404 Not Found``


5.2 - Authority Endpoint
~~~~~~~~~~~~~~~~~~~~~~~~

**GET /security/authority**

Returns all permissions granted to you.

Response:

::

  [<permission>]


5.3 - Permission Endpoint
~~~~~~~~~~~~~~~~~~~~~~~~~

**GET /security/permission[?transitive]**

Returns all permissions granted by you. If the ``transitive`` query param
is supplied, will also return all permissions which were derived from your own.

We may add query parameters in the future in order to filter the result set.

Response:

::

  [<permission>]


**GET /security/permission/<permission_id>**

Retrieve a permission by its unique identifier. You may only retrieve
information about permissions shared with you or by you.

If the permission does not exist or you do not have adequate permission
to see it, the response will be a ``404 Not Found``.

Response:

::

  <permission>


**GET /security/permission/<permission_id>/children[?transitive]**

Retrieve all permissions that were directly derived from this permission.
If the ``transitive`` query param is supplied, will also include permissions
which were indirectly derived. You may only retrieve information about
permissions shared with you or by you.

If the permission does not exist or you do not have adequate permission
to see it, the response will be a ``404 Not Found``.

Response:

::

  [<permission>]


**POST /security/permission**

Grant new permissions to a given set of users and/or groups.

Request:

::

  {
    "subjects" : ["<user_id>", "<group_id>", "..."],
    "actions": []
  }


* **user_id** is a email prefixed with the "user" string such as ``user:bob@example.com``
  representing the users to whom you wish to grant permissions. Users do not
  need to exist in the system at the time the permission is granted. When a
  user first logs into the system, they will be able to perform any action
  associated with permissions granted to their email.

* **group_id** a path prefixed with the "group" string such as
  ``group:/engineering/backend``. Groups DO need to exist in the system prior to
  granting them a permission. Providing a group path that points to a group
  that does not yet exist in the system will result in a ``400 Bad Request`` and
  no new permissions will have been granted to users or groups.

* **actions** The actions that the new permissions will allow the subjects
  to perform. All actions must be the same or a subset of actions found in
  your permissions. If that is not the case a ``400 Bad Request`` with an appropriate
  message will be returned and no new permissions will have been granted to users
  or groups.

Although all fields accept arrays, a permission is only ever granted to ONE
subject to perform ONE action. Thus, many permissions will be created and
returned by this endpoint.

Response:

::

  [<permission>]


**DELETE /security/permission/**

Revoke a permission. In order to revoke a permission, you must have a
permission which is a source of authority for the permission you wish
to revoke.

Refer to the top-level api description for explanation on the process of revoking.

.. note::

  Revoking a permission does not guarantee that the subject associated
  with that permission no longer has the capability to perform that action as
  another subject in the system may have also granted a permission with the
  capability to perform the same action. Unless you possess the root authority
  (e.g. if you are a member of the "admin" group created when the metastore
  was initialized), it is impossible for you to know for sure whether or not
  a subject still has the ability to perform the action.

If the permission does not exist or you do not have adequate permission to
see it, the response will be a ``404 Not Found``. If you attempt to revoke
one of your own permissions, the response will be a ``400 Bad Request``.


5.4 - Token Endpoint
~~~~~~~~~~~~~~~~~~~~

The following is the json representation of a token.

::

  {
    "id": "<token_id>",
    "secret": "<token_hash>",
    "name": "<name>",
    "grantedBy": ["<token_id>", "<user_id>", "<group_id>", "..."],
    "actions": [{
      "operation": "ADD|READ|MODIFY|DELETE",
      "resource": "<filesystem_path>|<group_path>",
      "accessType": "Structural|Content|Mount",
    }]
  }

* **secret** is a cryptographically secure string whose possession
  allows you to perform the action associated with the token.

* **name** an optional field that may or may not have been provided
  upon creation of the token.

* is a string identifier prefixed by the "token:" string

* an email address prefixed with the "user:" string

* a group path prefixed with the "group:" string

.. note::

  Once again, the ``Mount`` value for ``accessType`` is only valid
  for a filesystem path.


**GET /security/token**

List tokens that you have created. Does not list tokens that were created by
others based on your authority.

The json representation of the tokens does not contain the ``secret`` field
for this endpoint in order to reduce the chance of the secret leaking. The
secret can be retrieved by using the ``id`` endpoint.

Response:

::

  [<token>]


**GET /security/token/<id>**

Retrieve token for a given id.

You may only retrieve information about a token that you created. If the token
does not exist or was not created by you, the response will be a ``404 Not Found``.

Response:

::

  <token>


**POST /security/token**

Create a new token granting the capability to perform the given actions. All
actions must be a subset of your own capabilities. If the later condition is not
satisfied, a ``400 Bad Request`` will be returned.

Request:

::

  {
    "name": "",
    "actions": []
  }

* **name** is an optional field

Response:

::

  <token>


**DELETE /security/token/<id>**

Delete a token. In order to delete a token, you must have a permission which
is a source of authority of the token. If the token does not exist or was
not created by you, a ``404 Not Found`` will be returned.


**GET /security/oidc/providers**

This endpoint allows clients to obtain the list of configured OpenID Providers
(OPs). Responses will be a JSON array of configurations similar to the
following.

Response:

::

  [
    {
      "display_name": "Google",
      "client_id": "sdf9......dflkj",
      "openid_configuration": {
        "issuer": "https://accounts.google.com",
        "authorization_endpoint": "https://accounts.google.com/o/oauth2/v2/auth",
        "token_endpoint": "https://www.googleapis.com/oauth2/v4/token",
        "userinfo_endpoint": "https://www.googleapis.com/oauth2/v3/userinfo",
        "jwks": [
          {
            "kty": "RSA",
            "alg": "RS256",
            "use": "sig",
            "kid": "1195d......6abd",
            "n": "qy5D0......tJRJY02Qt0UKzJ2OquiPw",
            "e": "AQAB"
          },
          {
            "kty": "RSA",
            "alg": "RS256",
            "use": "sig",
            "kid": "b0a61.....9ba8575712",
            "n": "rvhjUe0..........n2IRNM8S8iJ36w",
            "e": "AQAB"
          }
        ]
      }
    },
    {
      "display_name": "Our Company OP",
      "client_id": "123455976",
      "openid_configuration": {
        "issuer": "https://op.ourcompany.com",
        "authorization_endpoint": "https://op.ourcompany.com/authorize",
        "token_endpoint": "https://op.ourcompany.com/token",
        "userinfo_endpoint": "https://op.ourcompany.com/userinfo",
        "jwks": [
          {
            "kty": "RSA",
            "kid": "1234",
            "alg": "RS256",
            "use": "sig",
            "n": "2354098udw...2957835lkj"
          },
          {
            "kty": "RSA",
            "kid": "5678",
            "alg": "RS256",
            "use": "sig",
            "n": "skljhdfiugy...39587dlkjsd"
          }
        ]
      }
    }
  ]





.. |Mount-Icon| image:: images/SD4/icon-mount.png

.. |Murray| image:: images/SD4/murray.png

.. |Murray-Small| image:: images/SD4/murray-small.png

.. |SD-Group-Example| image:: images/SD4/screenshots/sd-group-example.png

.. |SD-Token-Example| image:: images/SD4/screenshots/sd-token-example.png

.. |SD-Permission-Example-1| image:: images/SD4/screenshots/sd-permission-example-1.png

.. |SD-Permission-Example-2| image:: images/SD4/screenshots/sd-permission-example-2.png
