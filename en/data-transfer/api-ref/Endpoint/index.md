---
editable: false
sourcePath: en/_api-ref/datatransfer/v1/api-ref/Endpoint/index.md
---

# Data Transfer API, REST: Endpoint methods

## JSON Representation {#representation}
```json 
{
  "id": "string",
  "folderId": "string",
  "name": "string",
  "description": "string",
  "labels": "object",
  "settings": {

    // `settings` includes only one of the fields `mysqlSource`, `postgresSource`, `ydbSource`, `kafkaSource`, `mongoSource`, `clickhouseSource`, `mysqlTarget`, `postgresTarget`, `clickhouseTarget`, `ydbTarget`, `kafkaTarget`, `mongoTarget`
    "mysqlSource": {
      "connection": {

        // `settings.mysqlSource.connection` includes only one of the fields `mdbClusterId`, `onPremise`
        "mdbClusterId": "string",
        "onPremise": {
          "hosts": [
            "string"
          ],
          "port": "string",
          "tlsMode": {

            // `settings.mysqlSource.connection.onPremise.tlsMode` includes only one of the fields `disabled`, `enabled`
            "disabled": "object",
            "enabled": {
              "caCertificate": "string"
            },
            // end of the list of possible fields`settings.mysqlSource.connection.onPremise.tlsMode`

          },
          "subnetId": "string"
        },
        // end of the list of possible fields`settings.mysqlSource.connection`

      },
      "securityGroups": [
        "string"
      ],
      "database": "string",
      "serviceDatabase": "string",
      "user": "string",
      "password": {
        "raw": "string"
      },
      "includeTablesRegex": [
        "string"
      ],
      "excludeTablesRegex": [
        "string"
      ],
      "timezone": "string",
      "objectTransferSettings": {
        "view": "string",
        "routine": "string",
        "trigger": "string",
        "tables": "string"
      }
    },
    "postgresSource": {
      "connection": {

        // `settings.postgresSource.connection` includes only one of the fields `mdbClusterId`, `onPremise`
        "mdbClusterId": "string",
        "onPremise": {
          "hosts": [
            "string"
          ],
          "port": "string",
          "tlsMode": {

            // `settings.postgresSource.connection.onPremise.tlsMode` includes only one of the fields `disabled`, `enabled`
            "disabled": "object",
            "enabled": {
              "caCertificate": "string"
            },
            // end of the list of possible fields`settings.postgresSource.connection.onPremise.tlsMode`

          },
          "subnetId": "string"
        },
        // end of the list of possible fields`settings.postgresSource.connection`

      },
      "securityGroups": [
        "string"
      ],
      "database": "string",
      "user": "string",
      "password": {
        "raw": "string"
      },
      "includeTables": [
        "string"
      ],
      "excludeTables": [
        "string"
      ],
      "slotByteLagLimit": "string",
      "serviceSchema": "string",
      "objectTransferSettings": {
        "sequence": "string",
        "sequenceOwnedBy": "string",
        "sequenceSet": "string",
        "table": "string",
        "primaryKey": "string",
        "fkConstraint": "string",
        "defaultValues": "string",
        "constraint": "string",
        "index": "string",
        "view": "string",
        "materializedView": "string",
        "function": "string",
        "trigger": "string",
        "type": "string",
        "rule": "string",
        "collation": "string",
        "policy": "string",
        "cast": "string"
      }
    },
    "ydbSource": {
      "database": "string",
      "instance": "string",
      "serviceAccountId": "string",
      "paths": [
        "string"
      ],
      "subnetId": "string",
      "securityGroups": [
        "string"
      ],
      "saKeyContent": "string"
    },
    "kafkaSource": {
      "connection": {

        // `settings.kafkaSource.connection` includes only one of the fields `clusterId`, `onPremise`
        "clusterId": "string",
        "onPremise": {
          "brokerUrls": [
            "string"
          ],
          "tlsMode": {

            // `settings.kafkaSource.connection.onPremise.tlsMode` includes only one of the fields `disabled`, `enabled`
            "disabled": "object",
            "enabled": {
              "caCertificate": "string"
            },
            // end of the list of possible fields`settings.kafkaSource.connection.onPremise.tlsMode`

          },
          "subnetId": "string"
        },
        // end of the list of possible fields`settings.kafkaSource.connection`

      },
      "auth": {

        // `settings.kafkaSource.auth` includes only one of the fields `sasl`, `noAuth`
        "sasl": {
          "user": "string",
          "password": {
            "raw": "string"
          },
          "mechanism": "string"
        },
        "noAuth": {},
        // end of the list of possible fields`settings.kafkaSource.auth`

      },
      "securityGroups": [
        "string"
      ],
      "topicName": "string",
      "transformer": {
        "cloudFunction": "string",
        "serviceAccountId": "string",
        "numberOfRetries": "string",
        "bufferSize": "string",
        "bufferFlushInterval": "string",
        "invocationTimeout": "string"
      },
      "parser": {

        // `settings.kafkaSource.parser` includes only one of the fields `jsonParser`, `auditTrailsV1Parser`, `cloudLoggingParser`, `tskvParser`
        "jsonParser": {
          "dataSchema": {

            // `settings.kafkaSource.parser.jsonParser.dataSchema` includes only one of the fields `fields`, `jsonFields`
            "fields": {
              "fields": [
                {
                  "name": "string",
                  "type": "string",
                  "key": true,
                  "required": true,
                  "path": "string"
                }
              ]
            },
            "jsonFields": "string",
            // end of the list of possible fields`settings.kafkaSource.parser.jsonParser.dataSchema`

          },
          "nullKeysAllowed": true,
          "addRestColumn": true
        },
        "auditTrailsV1Parser": {},
        "cloudLoggingParser": {},
        "tskvParser": {
          "dataSchema": {

            // `settings.kafkaSource.parser.tskvParser.dataSchema` includes only one of the fields `fields`, `jsonFields`
            "fields": {
              "fields": [
                {
                  "name": "string",
                  "type": "string",
                  "key": true,
                  "required": true,
                  "path": "string"
                }
              ]
            },
            "jsonFields": "string",
            // end of the list of possible fields`settings.kafkaSource.parser.tskvParser.dataSchema`

          },
          "nullKeysAllowed": true,
          "addRestColumn": true
        },
        // end of the list of possible fields`settings.kafkaSource.parser`

      },
      "topicNames": [
        "string"
      ]
    },
    "mongoSource": {
      "connection": {
        "connectionOptions": {
          "user": "string",
          "password": {
            "raw": "string"
          },
          "authSource": "string",

          // `settings.mongoSource.connection.connectionOptions` includes only one of the fields `mdbClusterId`, `onPremise`
          "mdbClusterId": "string",
          "onPremise": {
            "hosts": [
              "string"
            ],
            "port": "string",
            "tlsMode": {

              // `settings.mongoSource.connection.connectionOptions.onPremise.tlsMode` includes only one of the fields `disabled`, `enabled`
              "disabled": "object",
              "enabled": {
                "caCertificate": "string"
              },
              // end of the list of possible fields`settings.mongoSource.connection.connectionOptions.onPremise.tlsMode`

            },
            "replicaSet": "string"
          },
          // end of the list of possible fields`settings.mongoSource.connection.connectionOptions`

        }
      },
      "subnetId": "string",
      "securityGroups": [
        "string"
      ],
      "collections": [
        {
          "databaseName": "string",
          "collectionName": "string"
        }
      ],
      "excludedCollections": [
        {
          "databaseName": "string",
          "collectionName": "string"
        }
      ],
      "secondaryPreferredMode": true
    },
    "clickhouseSource": {
      "connection": {
        "connectionOptions": {
          "database": "string",
          "user": "string",
          "password": {
            "raw": "string"
          },

          // `settings.clickhouseSource.connection.connectionOptions` includes only one of the fields `mdbClusterId`, `onPremise`
          "mdbClusterId": "string",
          "onPremise": {
            "shards": [
              {
                "name": "string",
                "hosts": [
                  "string"
                ]
              }
            ],
            "httpPort": "string",
            "nativePort": "string",
            "tlsMode": {

              // `settings.clickhouseSource.connection.connectionOptions.onPremise.tlsMode` includes only one of the fields `disabled`, `enabled`
              "disabled": "object",
              "enabled": {
                "caCertificate": "string"
              },
              // end of the list of possible fields`settings.clickhouseSource.connection.connectionOptions.onPremise.tlsMode`

            }
          },
          // end of the list of possible fields`settings.clickhouseSource.connection.connectionOptions`

        }
      },
      "subnetId": "string",
      "securityGroups": [
        "string"
      ],
      "includeTables": [
        "string"
      ],
      "excludeTables": [
        "string"
      ]
    },
    "mysqlTarget": {
      "connection": {

        // `settings.mysqlTarget.connection` includes only one of the fields `mdbClusterId`, `onPremise`
        "mdbClusterId": "string",
        "onPremise": {
          "hosts": [
            "string"
          ],
          "port": "string",
          "tlsMode": {

            // `settings.mysqlTarget.connection.onPremise.tlsMode` includes only one of the fields `disabled`, `enabled`
            "disabled": "object",
            "enabled": {
              "caCertificate": "string"
            },
            // end of the list of possible fields`settings.mysqlTarget.connection.onPremise.tlsMode`

          },
          "subnetId": "string"
        },
        // end of the list of possible fields`settings.mysqlTarget.connection`

      },
      "securityGroups": [
        "string"
      ],
      "database": "string",
      "user": "string",
      "password": {
        "raw": "string"
      },
      "sqlMode": "string",
      "skipConstraintChecks": true,
      "timezone": "string",
      "cleanupPolicy": "string",
      "serviceDatabase": "string"
    },
    "postgresTarget": {
      "connection": {

        // `settings.postgresTarget.connection` includes only one of the fields `mdbClusterId`, `onPremise`
        "mdbClusterId": "string",
        "onPremise": {
          "hosts": [
            "string"
          ],
          "port": "string",
          "tlsMode": {

            // `settings.postgresTarget.connection.onPremise.tlsMode` includes only one of the fields `disabled`, `enabled`
            "disabled": "object",
            "enabled": {
              "caCertificate": "string"
            },
            // end of the list of possible fields`settings.postgresTarget.connection.onPremise.tlsMode`

          },
          "subnetId": "string"
        },
        // end of the list of possible fields`settings.postgresTarget.connection`

      },
      "securityGroups": [
        "string"
      ],
      "database": "string",
      "user": "string",
      "password": {
        "raw": "string"
      },
      "cleanupPolicy": "string"
    },
    "clickhouseTarget": {
      "connection": {
        "connectionOptions": {
          "database": "string",
          "user": "string",
          "password": {
            "raw": "string"
          },

          // `settings.clickhouseTarget.connection.connectionOptions` includes only one of the fields `mdbClusterId`, `onPremise`
          "mdbClusterId": "string",
          "onPremise": {
            "shards": [
              {
                "name": "string",
                "hosts": [
                  "string"
                ]
              }
            ],
            "httpPort": "string",
            "nativePort": "string",
            "tlsMode": {

              // `settings.clickhouseTarget.connection.connectionOptions.onPremise.tlsMode` includes only one of the fields `disabled`, `enabled`
              "disabled": "object",
              "enabled": {
                "caCertificate": "string"
              },
              // end of the list of possible fields`settings.clickhouseTarget.connection.connectionOptions.onPremise.tlsMode`

            }
          },
          // end of the list of possible fields`settings.clickhouseTarget.connection.connectionOptions`

        }
      },
      "subnetId": "string",
      "securityGroups": [
        "string"
      ],
      "clickhouseClusterName": "string",
      "altNames": [
        {
          "fromName": "string",
          "toName": "string"
        }
      ],
      "sharding": {

        // `settings.clickhouseTarget.sharding` includes only one of the fields `columnValueHash`, `customMapping`, `transferId`, `roundRobin`
        "columnValueHash": {
          "columnName": "string"
        },
        "customMapping": {
          "columnName": "string",
          "mapping": [
            {
              "columnValue": {
                "stringValue": "string"
              },
              "shardName": "string"
            }
          ]
        },
        "transferId": "object",
        "roundRobin": "object",
        // end of the list of possible fields`settings.clickhouseTarget.sharding`

      },
      "cleanupPolicy": "string"
    },
    "ydbTarget": {
      "database": "string",
      "instance": "string",
      "serviceAccountId": "string",
      "path": "string",
      "subnetId": "string",
      "securityGroups": [
        "string"
      ],
      "saKeyContent": "string",
      "cleanupPolicy": "string",
      "isTableColumnOriented": true
    },
    "kafkaTarget": {
      "connection": {

        // `settings.kafkaTarget.connection` includes only one of the fields `clusterId`, `onPremise`
        "clusterId": "string",
        "onPremise": {
          "brokerUrls": [
            "string"
          ],
          "tlsMode": {

            // `settings.kafkaTarget.connection.onPremise.tlsMode` includes only one of the fields `disabled`, `enabled`
            "disabled": "object",
            "enabled": {
              "caCertificate": "string"
            },
            // end of the list of possible fields`settings.kafkaTarget.connection.onPremise.tlsMode`

          },
          "subnetId": "string"
        },
        // end of the list of possible fields`settings.kafkaTarget.connection`

      },
      "auth": {

        // `settings.kafkaTarget.auth` includes only one of the fields `sasl`, `noAuth`
        "sasl": {
          "user": "string",
          "password": {
            "raw": "string"
          },
          "mechanism": "string"
        },
        "noAuth": {},
        // end of the list of possible fields`settings.kafkaTarget.auth`

      },
      "securityGroups": [
        "string"
      ],
      "topicSettings": {

        // `settings.kafkaTarget.topicSettings` includes only one of the fields `topic`, `topicPrefix`
        "topic": {
          "topicName": "string",
          "saveTxOrder": true
        },
        "topicPrefix": "string",
        // end of the list of possible fields`settings.kafkaTarget.topicSettings`

      },
      "serializer": {

        // `settings.kafkaTarget.serializer` includes only one of the fields `serializerAuto`, `serializerJson`, `serializerDebezium`
        "serializerAuto": {},
        "serializerJson": {},
        "serializerDebezium": {
          "serializerParameters": [
            {
              "key": "string",
              "value": "string"
            }
          ]
        },
        // end of the list of possible fields`settings.kafkaTarget.serializer`

      }
    },
    "mongoTarget": {
      "connection": {
        "connectionOptions": {
          "user": "string",
          "password": {
            "raw": "string"
          },
          "authSource": "string",

          // `settings.mongoTarget.connection.connectionOptions` includes only one of the fields `mdbClusterId`, `onPremise`
          "mdbClusterId": "string",
          "onPremise": {
            "hosts": [
              "string"
            ],
            "port": "string",
            "tlsMode": {

              // `settings.mongoTarget.connection.connectionOptions.onPremise.tlsMode` includes only one of the fields `disabled`, `enabled`
              "disabled": "object",
              "enabled": {
                "caCertificate": "string"
              },
              // end of the list of possible fields`settings.mongoTarget.connection.connectionOptions.onPremise.tlsMode`

            },
            "replicaSet": "string"
          },
          // end of the list of possible fields`settings.mongoTarget.connection.connectionOptions`

        }
      },
      "subnetId": "string",
      "securityGroups": [
        "string"
      ],
      "database": "string",
      "cleanupPolicy": "string"
    },
    // end of the list of possible fields`settings`

  }
}
```
 
Field | Description
--- | ---
id | **string**
folderId | **string**
name | **string**
description | **string**
labels | **object**
settings | **object**
settings.<br>mysqlSource | **object** <br>`settings` includes only one of the fields `mysqlSource`, `postgresSource`, `ydbSource`, `kafkaSource`, `mongoSource`, `clickhouseSource`, `mysqlTarget`, `postgresTarget`, `clickhouseTarget`, `ydbTarget`, `kafkaTarget`, `mongoTarget`<br>
settings.<br>mysqlSource.<br>connection | **object**<br><p>Database connection settings</p> 
settings.<br>mysqlSource.<br>connection.<br>mdbClusterId | **string** <br>`settings.mysqlSource.connection` includes only one of the fields `mdbClusterId`, `onPremise`<br><br><p>Managed Service for MySQL cluster ID</p> 
settings.<br>mysqlSource.<br>connection.<br>onPremise | **object** <br>`settings.mysqlSource.connection` includes only one of the fields `mdbClusterId`, `onPremise`<br>
settings.<br>mysqlSource.<br>connection.<br>onPremise.<br>hosts[] | **string**
settings.<br>mysqlSource.<br>connection.<br>onPremise.<br>port | **string** (int64)<br><p>Database port</p> 
settings.<br>mysqlSource.<br>connection.<br>onPremise.<br>tlsMode | **object**<br><p>TLS settings for server connection. Disabled by default.</p> 
settings.<br>mysqlSource.<br>connection.<br>onPremise.<br>tlsMode.<br>disabled | **object** <br>`settings.mysqlSource.connection.onPremise.tlsMode` includes only one of the fields `disabled`, `enabled`<br><br><p>Empty JSON object ``{}``.</p> 
settings.<br>mysqlSource.<br>connection.<br>onPremise.<br>tlsMode.<br>disabled.<br>disabled | **object** <br>`settings.mysqlSource.connection.onPremise.tlsMode` includes only one of the fields `disabled`, `enabled`<br><br><p>Empty JSON object ``{}``.</p> 
settings.<br>mysqlSource.<br>connection.<br>onPremise.<br>tlsMode.<br>enabled | **object** <br>`settings.mysqlSource.connection.onPremise.tlsMode` includes only one of the fields `disabled`, `enabled`<br>
settings.<br>mysqlSource.<br>connection.<br>onPremise.<br>tlsMode.<br>enabled.<br>caCertificate | **string**<br><p>CA certificate</p> <p>X.509 certificate of the certificate authority which issued the server's certificate, in PEM format. When CA certificate is specified TLS is used to connect to the server.</p> 
settings.<br>mysqlSource.<br>connection.<br>onPremise.<br>subnetId | **string**<br><p>Network interface for endpoint. If none will assume public ipv4</p> 
settings.<br>mysqlSource.<br>securityGroups[] | **string**<br><p>Security groups</p> 
settings.<br>mysqlSource.<br>database | **string**<br><p>Database name</p> <p>You can leave it empty, then it will be possible to transfer tables from several databases at the same time from this source.</p> 
settings.<br>mysqlSource.<br>serviceDatabase | **string**<br><p>Database for service tables</p> <p>Default: data source database. Here created technical tables (__tm_keeper, __tm_gtid_keeper).</p> 
settings.<br>mysqlSource.<br>user | **string**<br><p>User for database access.</p> 
settings.<br>mysqlSource.<br>password | **object**<br><p>Password for database access.</p> 
settings.<br>mysqlSource.<br>password.<br>raw | **string**<br><p>Raw secret value</p> 
settings.<br>mysqlSource.<br>includeTablesRegex[] | **string**
settings.<br>mysqlSource.<br>excludeTablesRegex[] | **string**
settings.<br>mysqlSource.<br>timezone | **string**<br><p>Database timezone</p> <p>Is used for parsing timestamps for saving source timezones. Accepts values from IANA timezone database. Default: local timezone.</p> 
settings.<br>mysqlSource.<br>objectTransferSettings | **object**<br><p>Schema migration</p> <p>Select database objects to be transferred during activation or deactivation.</p> 
settings.<br>mysqlSource.<br>objectTransferSettings.<br>view | **string**<br><p>Views</p> <p>CREATE VIEW ...</p> <ul> <li>BEFORE_DATA: Before data transfer</li> <li>AFTER_DATA: After data transfer</li> <li>NEVER: Don't copy</li> </ul> 
settings.<br>mysqlSource.<br>objectTransferSettings.<br>routine | **string**<br><p>Routines</p> <p>CREATE PROCEDURE ... ; CREATE FUNCTION ... ;</p> <ul> <li>BEFORE_DATA: Before data transfer</li> <li>AFTER_DATA: After data transfer</li> <li>NEVER: Don't copy</li> </ul> 
settings.<br>mysqlSource.<br>objectTransferSettings.<br>trigger | **string**<br><p>Triggers</p> <p>CREATE TRIGGER ...</p> <ul> <li>BEFORE_DATA: Before data transfer</li> <li>AFTER_DATA: After data transfer</li> <li>NEVER: Don't copy</li> </ul> 
settings.<br>mysqlSource.<br>objectTransferSettings.<br>tables | **string**<br><ul> <li>BEFORE_DATA: Before data transfer</li> <li>AFTER_DATA: After data transfer</li> <li>NEVER: Don't copy</li> </ul> 
settings.<br>postgresSource | **object** <br>`settings` includes only one of the fields `mysqlSource`, `postgresSource`, `ydbSource`, `kafkaSource`, `mongoSource`, `clickhouseSource`, `mysqlTarget`, `postgresTarget`, `clickhouseTarget`, `ydbTarget`, `kafkaTarget`, `mongoTarget`<br>
settings.<br>postgresSource.<br>connection | **object**<br><p>Database connection settings</p> 
settings.<br>postgresSource.<br>connection.<br>mdbClusterId | **string** <br>`settings.postgresSource.connection` includes only one of the fields `mdbClusterId`, `onPremise`<br><br><p>Managed Service for PostgreSQL cluster ID</p> 
settings.<br>postgresSource.<br>connection.<br>onPremise | **object** <br>`settings.postgresSource.connection` includes only one of the fields `mdbClusterId`, `onPremise`<br>
settings.<br>postgresSource.<br>connection.<br>onPremise.<br>hosts[] | **string**
settings.<br>postgresSource.<br>connection.<br>onPremise.<br>port | **string** (int64)<br><p>Will be used if the cluster ID is not specified.</p> 
settings.<br>postgresSource.<br>connection.<br>onPremise.<br>tlsMode | **object**<br><p>TLS settings for server connection. Disabled by default.</p> 
settings.<br>postgresSource.<br>connection.<br>onPremise.<br>tlsMode.<br>disabled | **object** <br>`settings.postgresSource.connection.onPremise.tlsMode` includes only one of the fields `disabled`, `enabled`<br><br><p>Empty JSON object ``{}``.</p> 
settings.<br>postgresSource.<br>connection.<br>onPremise.<br>tlsMode.<br>disabled.<br>disabled | **object** <br>`settings.postgresSource.connection.onPremise.tlsMode` includes only one of the fields `disabled`, `enabled`<br><br><p>Empty JSON object ``{}``.</p> 
settings.<br>postgresSource.<br>connection.<br>onPremise.<br>tlsMode.<br>enabled | **object** <br>`settings.postgresSource.connection.onPremise.tlsMode` includes only one of the fields `disabled`, `enabled`<br>
settings.<br>postgresSource.<br>connection.<br>onPremise.<br>tlsMode.<br>enabled.<br>caCertificate | **string**<br><p>CA certificate</p> <p>X.509 certificate of the certificate authority which issued the server's certificate, in PEM format. When CA certificate is specified TLS is used to connect to the server.</p> 
settings.<br>postgresSource.<br>connection.<br>onPremise.<br>subnetId | **string**<br><p>Network interface for endpoint. If none will assume public ipv4</p> 
settings.<br>postgresSource.<br>securityGroups[] | **string**<br><p>Security groups</p> 
settings.<br>postgresSource.<br>database | **string**<br><p>Database name</p> 
settings.<br>postgresSource.<br>user | **string**<br><p>User for database access.</p> 
settings.<br>postgresSource.<br>password | **object**<br><p>Password for database access.</p> 
settings.<br>postgresSource.<br>password.<br>raw | **string**<br><p>Raw secret value</p> 
settings.<br>postgresSource.<br>includeTables[] | **string**<br><p>Included tables</p> <p>If none or empty list is presented, all tables are replicated. Full table name with schema. Can contain schema_name.* patterns.</p> 
settings.<br>postgresSource.<br>excludeTables[] | **string**<br><p>Excluded tables</p> <p>If none or empty list is presented, all tables are replicated. Full table name with schema. Can contain schema_name.* patterns.</p> 
settings.<br>postgresSource.<br>slotByteLagLimit | **string** (int64)<br><p>Maximum lag of replication slot (in bytes); after exceeding this limit replication will be aborted.</p> 
settings.<br>postgresSource.<br>serviceSchema | **string**<br><p>Database schema for service tables (__consumer_keeper, __data_transfer_mole_finder). Default is public</p> 
settings.<br>postgresSource.<br>objectTransferSettings | **object**<br><p>Select database objects to be transferred during activation or deactivation.</p> 
settings.<br>postgresSource.<br>objectTransferSettings.<br>sequence | **string**<br><p>Sequences</p> <p>CREATE SEQUENCE ...</p> <ul> <li>BEFORE_DATA: Before data transfer</li> <li>AFTER_DATA: After data transfer</li> <li>NEVER: Don't copy</li> </ul> 
settings.<br>postgresSource.<br>objectTransferSettings.<br>sequenceOwnedBy | **string**<br><p>Owned sequences</p> <p>CREATE SEQUENCE ... OWNED BY ...</p> <ul> <li>BEFORE_DATA: Before data transfer</li> <li>AFTER_DATA: After data transfer</li> <li>NEVER: Don't copy</li> </ul> 
settings.<br>postgresSource.<br>objectTransferSettings.<br>sequenceSet | **string**<br><ul> <li>BEFORE_DATA: Before data transfer</li> <li>AFTER_DATA: After data transfer</li> <li>NEVER: Don't copy</li> </ul> 
settings.<br>postgresSource.<br>objectTransferSettings.<br>table | **string**<br><p>Tables</p> <p>CREATE TABLE ...</p> <ul> <li>BEFORE_DATA: Before data transfer</li> <li>AFTER_DATA: After data transfer</li> <li>NEVER: Don't copy</li> </ul> 
settings.<br>postgresSource.<br>objectTransferSettings.<br>primaryKey | **string**<br><p>Primary keys</p> <p>ALTER TABLE ... ADD PRIMARY KEY ...</p> <ul> <li>BEFORE_DATA: Before data transfer</li> <li>AFTER_DATA: After data transfer</li> <li>NEVER: Don't copy</li> </ul> 
settings.<br>postgresSource.<br>objectTransferSettings.<br>fkConstraint | **string**<br><p>Foreign keys</p> <p>ALTER TABLE ... ADD FOREIGN KEY ...</p> <ul> <li>BEFORE_DATA: Before data transfer</li> <li>AFTER_DATA: After data transfer</li> <li>NEVER: Don't copy</li> </ul> 
settings.<br>postgresSource.<br>objectTransferSettings.<br>defaultValues | **string**<br><p>Default values</p> <p>ALTER TABLE ... ALTER COLUMN ... SET DEFAULT ...</p> <ul> <li>BEFORE_DATA: Before data transfer</li> <li>AFTER_DATA: After data transfer</li> <li>NEVER: Don't copy</li> </ul> 
settings.<br>postgresSource.<br>objectTransferSettings.<br>constraint | **string**<br><p>Constraints</p> <p>ALTER TABLE ... ADD CONSTRAINT ...</p> <ul> <li>BEFORE_DATA: Before data transfer</li> <li>AFTER_DATA: After data transfer</li> <li>NEVER: Don't copy</li> </ul> 
settings.<br>postgresSource.<br>objectTransferSettings.<br>index | **string**<br><p>Indexes</p> <p>CREATE INDEX ...</p> <ul> <li>BEFORE_DATA: Before data transfer</li> <li>AFTER_DATA: After data transfer</li> <li>NEVER: Don't copy</li> </ul> 
settings.<br>postgresSource.<br>objectTransferSettings.<br>view | **string**<br><p>Views</p> <p>CREATE VIEW ...</p> <ul> <li>BEFORE_DATA: Before data transfer</li> <li>AFTER_DATA: After data transfer</li> <li>NEVER: Don't copy</li> </ul> 
settings.<br>postgresSource.<br>objectTransferSettings.<br>materializedView | **string**<br><p>Materialized views</p> <p>CREATE MATERIALIZED VIEW ...</p> <ul> <li>BEFORE_DATA: Before data transfer</li> <li>AFTER_DATA: After data transfer</li> <li>NEVER: Don't copy</li> </ul> 
settings.<br>postgresSource.<br>objectTransferSettings.<br>function | **string**<br><p>Functions</p> <p>CREATE FUNCTION ...</p> <ul> <li>BEFORE_DATA: Before data transfer</li> <li>AFTER_DATA: After data transfer</li> <li>NEVER: Don't copy</li> </ul> 
settings.<br>postgresSource.<br>objectTransferSettings.<br>trigger | **string**<br><p>Triggers</p> <p>CREATE TRIGGER ...</p> <ul> <li>BEFORE_DATA: Before data transfer</li> <li>AFTER_DATA: After data transfer</li> <li>NEVER: Don't copy</li> </ul> 
settings.<br>postgresSource.<br>objectTransferSettings.<br>type | **string**<br><p>Types</p> <p>CREATE TYPE ...</p> <ul> <li>BEFORE_DATA: Before data transfer</li> <li>AFTER_DATA: After data transfer</li> <li>NEVER: Don't copy</li> </ul> 
settings.<br>postgresSource.<br>objectTransferSettings.<br>rule | **string**<br><p>Rules</p> <p>CREATE RULE ...</p> <ul> <li>BEFORE_DATA: Before data transfer</li> <li>AFTER_DATA: After data transfer</li> <li>NEVER: Don't copy</li> </ul> 
settings.<br>postgresSource.<br>objectTransferSettings.<br>collation | **string**<br><p>Collations</p> <p>CREATE COLLATION ...</p> <ul> <li>BEFORE_DATA: Before data transfer</li> <li>AFTER_DATA: After data transfer</li> <li>NEVER: Don't copy</li> </ul> 
settings.<br>postgresSource.<br>objectTransferSettings.<br>policy | **string**<br><p>Policies</p> <p>CREATE POLICY ...</p> <ul> <li>BEFORE_DATA: Before data transfer</li> <li>AFTER_DATA: After data transfer</li> <li>NEVER: Don't copy</li> </ul> 
settings.<br>postgresSource.<br>objectTransferSettings.<br>cast | **string**<br><p>Casts</p> <p>CREATE CAST ...</p> <ul> <li>BEFORE_DATA: Before data transfer</li> <li>AFTER_DATA: After data transfer</li> <li>NEVER: Don't copy</li> </ul> 
settings.<br>ydbSource | **object** <br>`settings` includes only one of the fields `mysqlSource`, `postgresSource`, `ydbSource`, `kafkaSource`, `mongoSource`, `clickhouseSource`, `mysqlTarget`, `postgresTarget`, `clickhouseTarget`, `ydbTarget`, `kafkaTarget`, `mongoTarget`<br>
settings.<br>ydbSource.<br>database | **string**<br><p>Path in YDB where to store tables</p> 
settings.<br>ydbSource.<br>instance | **string**<br><p>Instance of YDB. example: ydb-ru-prestable.yandex.net:2135</p> 
settings.<br>ydbSource.<br>serviceAccountId | **string**
settings.<br>ydbSource.<br>paths[] | **string**
settings.<br>ydbSource.<br>subnetId | **string**<br><p>Network interface for endpoint. If none will assume public ipv4</p> 
settings.<br>ydbSource.<br>securityGroups[] | **string**<br><p>Security groups</p> 
settings.<br>ydbSource.<br>saKeyContent | **string**<br><p>Authorization Key</p> 
settings.<br>kafkaSource | **object** <br>`settings` includes only one of the fields `mysqlSource`, `postgresSource`, `ydbSource`, `kafkaSource`, `mongoSource`, `clickhouseSource`, `mysqlTarget`, `postgresTarget`, `clickhouseTarget`, `ydbTarget`, `kafkaTarget`, `mongoTarget`<br>
settings.<br>kafkaSource.<br>connection | **object**<br><p>Connection settings</p> 
settings.<br>kafkaSource.<br>connection.<br>clusterId | **string** <br>`settings.kafkaSource.connection` includes only one of the fields `clusterId`, `onPremise`<br><br><p>Managed Service for Kafka cluster ID</p> 
settings.<br>kafkaSource.<br>connection.<br>onPremise | **object** <br>`settings.kafkaSource.connection` includes only one of the fields `clusterId`, `onPremise`<br>
settings.<br>kafkaSource.<br>connection.<br>onPremise.<br>brokerUrls[] | **string**<br><p>Kafka broker URLs</p> 
settings.<br>kafkaSource.<br>connection.<br>onPremise.<br>tlsMode | **object**<br><p>TLS settings for broker connection. Disabled by default.</p> 
settings.<br>kafkaSource.<br>connection.<br>onPremise.<br>tlsMode.<br>disabled | **object** <br>`settings.kafkaSource.connection.onPremise.tlsMode` includes only one of the fields `disabled`, `enabled`<br><br><p>Empty JSON object ``{}``.</p> 
settings.<br>kafkaSource.<br>connection.<br>onPremise.<br>tlsMode.<br>disabled.<br>disabled | **object** <br>`settings.kafkaSource.connection.onPremise.tlsMode` includes only one of the fields `disabled`, `enabled`<br><br><p>Empty JSON object ``{}``.</p> 
settings.<br>kafkaSource.<br>connection.<br>onPremise.<br>tlsMode.<br>enabled | **object** <br>`settings.kafkaSource.connection.onPremise.tlsMode` includes only one of the fields `disabled`, `enabled`<br>
settings.<br>kafkaSource.<br>connection.<br>onPremise.<br>tlsMode.<br>enabled.<br>caCertificate | **string**<br><p>CA certificate</p> <p>X.509 certificate of the certificate authority which issued the server's certificate, in PEM format. When CA certificate is specified TLS is used to connect to the server.</p> 
settings.<br>kafkaSource.<br>connection.<br>onPremise.<br>subnetId | **string**<br><p>Network interface for endpoint. If none will assume public ipv4</p> 
settings.<br>kafkaSource.<br>auth | **object**<br><p>Authentication settings</p> 
settings.<br>kafkaSource.<br>auth.<br>sasl | **object** <br>`settings.kafkaSource.auth` includes only one of the fields `sasl`, `noAuth`<br>
settings.<br>kafkaSource.<br>auth.<br>sasl.<br>user | **string**<br><p>User name</p> 
settings.<br>kafkaSource.<br>auth.<br>sasl.<br>password | **object**<br><p>Password for user</p> 
settings.<br>kafkaSource.<br>auth.<br>sasl.<br>password.<br>raw | **string**<br><p>Raw secret value</p> 
settings.<br>kafkaSource.<br>auth.<br>sasl.<br>mechanism | **string**<br><p>SASL mechanism for authentication</p> 
settings.<br>kafkaSource.<br>auth.<br>noAuth | **object** <br>`settings.kafkaSource.auth` includes only one of the fields `sasl`, `noAuth`<br><br><p>No authentication</p> 
settings.<br>kafkaSource.<br>securityGroups[] | **string**<br><p>Security groups</p> 
settings.<br>kafkaSource.<br>topicName | **string**<br><p>Full source topic name Deprecated in favor of topic names</p> 
settings.<br>kafkaSource.<br>transformer | **object**<br><p>Data transformation rules</p> 
settings.<br>kafkaSource.<br>transformer.<br>cloudFunction | **string**<br><p>Cloud function</p> 
settings.<br>kafkaSource.<br>transformer.<br>serviceAccountId | **string**<br><p>Service account</p> 
settings.<br>kafkaSource.<br>transformer.<br>numberOfRetries | **string** (int64)<br><p>Number of retries</p> 
settings.<br>kafkaSource.<br>transformer.<br>bufferSize | **string**<br><p>Buffer size for function</p> 
settings.<br>kafkaSource.<br>transformer.<br>bufferFlushInterval | **string**<br><p>Flush interval</p> 
settings.<br>kafkaSource.<br>transformer.<br>invocationTimeout | **string**<br><p>Invocation timeout</p> 
settings.<br>kafkaSource.<br>parser | **object**<br><p>Data parsing rules</p> 
settings.<br>kafkaSource.<br>parser.<br>jsonParser | **object** <br>`settings.kafkaSource.parser` includes only one of the fields `jsonParser`, `auditTrailsV1Parser`, `cloudLoggingParser`, `tskvParser`<br>
settings.<br>kafkaSource.<br>parser.<br>jsonParser.<br>dataSchema | **object**
settings.<br>kafkaSource.<br>parser.<br>jsonParser.<br>dataSchema.<br>fields | **object** <br>`settings.kafkaSource.parser.jsonParser.dataSchema` includes only one of the fields `fields`, `jsonFields`<br>
settings.<br>kafkaSource.<br>parser.<br>jsonParser.<br>dataSchema.<br>fields.<br>fields[] | **object**<br><p>Column schema</p> 
settings.<br>kafkaSource.<br>parser.<br>jsonParser.<br>dataSchema.<br>fields.<br>fields[].<br>name | **string**
settings.<br>kafkaSource.<br>parser.<br>jsonParser.<br>dataSchema.<br>fields.<br>fields[].<br>type | **string**
settings.<br>kafkaSource.<br>parser.<br>jsonParser.<br>dataSchema.<br>fields.<br>fields[].<br>key | **boolean** (boolean)
settings.<br>kafkaSource.<br>parser.<br>jsonParser.<br>dataSchema.<br>fields.<br>fields[].<br>required | **boolean** (boolean)
settings.<br>kafkaSource.<br>parser.<br>jsonParser.<br>dataSchema.<br>fields.<br>fields[].<br>path | **string**
settings.<br>kafkaSource.<br>parser.<br>jsonParser.<br>dataSchema.<br>jsonFields | **string** <br>`settings.kafkaSource.parser.jsonParser.dataSchema` includes only one of the fields `fields`, `jsonFields`<br>
settings.<br>kafkaSource.<br>parser.<br>jsonParser.<br>nullKeysAllowed | **boolean** (boolean)<br><p>Allow null keys, if no - null keys will be putted to unparsed data</p> 
settings.<br>kafkaSource.<br>parser.<br>jsonParser.<br>addRestColumn | **boolean** (boolean)<br><p>Will add _rest column for all unknown fields</p> 
settings.<br>kafkaSource.<br>parser.<br>auditTrailsV1Parser | **object** <br>`settings.kafkaSource.parser` includes only one of the fields `jsonParser`, `auditTrailsV1Parser`, `cloudLoggingParser`, `tskvParser`<br>
settings.<br>kafkaSource.<br>parser.<br>cloudLoggingParser | **object** <br>`settings.kafkaSource.parser` includes only one of the fields `jsonParser`, `auditTrailsV1Parser`, `cloudLoggingParser`, `tskvParser`<br>
settings.<br>kafkaSource.<br>parser.<br>tskvParser | **object** <br>`settings.kafkaSource.parser` includes only one of the fields `jsonParser`, `auditTrailsV1Parser`, `cloudLoggingParser`, `tskvParser`<br>
settings.<br>kafkaSource.<br>parser.<br>tskvParser.<br>dataSchema | **object**
settings.<br>kafkaSource.<br>parser.<br>tskvParser.<br>dataSchema.<br>fields | **object** <br>`settings.kafkaSource.parser.tskvParser.dataSchema` includes only one of the fields `fields`, `jsonFields`<br>
settings.<br>kafkaSource.<br>parser.<br>tskvParser.<br>dataSchema.<br>fields.<br>fields[] | **object**<br><p>Column schema</p> 
settings.<br>kafkaSource.<br>parser.<br>tskvParser.<br>dataSchema.<br>fields.<br>fields[].<br>name | **string**
settings.<br>kafkaSource.<br>parser.<br>tskvParser.<br>dataSchema.<br>fields.<br>fields[].<br>type | **string**
settings.<br>kafkaSource.<br>parser.<br>tskvParser.<br>dataSchema.<br>fields.<br>fields[].<br>key | **boolean** (boolean)
settings.<br>kafkaSource.<br>parser.<br>tskvParser.<br>dataSchema.<br>fields.<br>fields[].<br>required | **boolean** (boolean)
settings.<br>kafkaSource.<br>parser.<br>tskvParser.<br>dataSchema.<br>fields.<br>fields[].<br>path | **string**
settings.<br>kafkaSource.<br>parser.<br>tskvParser.<br>dataSchema.<br>jsonFields | **string** <br>`settings.kafkaSource.parser.tskvParser.dataSchema` includes only one of the fields `fields`, `jsonFields`<br>
settings.<br>kafkaSource.<br>parser.<br>tskvParser.<br>nullKeysAllowed | **boolean** (boolean)<br><p>Allow null keys, if no - null keys will be putted to unparsed data</p> 
settings.<br>kafkaSource.<br>parser.<br>tskvParser.<br>addRestColumn | **boolean** (boolean)<br><p>Will add _rest column for all unknown fields</p> 
settings.<br>kafkaSource.<br>topicNames[] | **string**<br><p>List of topic names to read</p> 
settings.<br>mongoSource | **object** <br>`settings` includes only one of the fields `mysqlSource`, `postgresSource`, `ydbSource`, `kafkaSource`, `mongoSource`, `clickhouseSource`, `mysqlTarget`, `postgresTarget`, `clickhouseTarget`, `ydbTarget`, `kafkaTarget`, `mongoTarget`<br>
settings.<br>mongoSource.<br>connection | **object**
settings.<br>mongoSource.<br>connection.<br>connectionOptions | **object**
settings.<br>mongoSource.<br>connection.<br>connectionOptions.<br>user | **string**<br><p>User name</p> 
settings.<br>mongoSource.<br>connection.<br>connectionOptions.<br>password | **object**
settings.<br>mongoSource.<br>connection.<br>connectionOptions.<br>password.<br>raw | **string**<br><p>Raw secret value</p> 
settings.<br>mongoSource.<br>connection.<br>connectionOptions.<br>authSource | **string**<br><p>Database name associated with the credentials</p> 
settings.<br>mongoSource.<br>connection.<br>connectionOptions.<br>mdbClusterId | **string** <br>`settings.mongoSource.connection.connectionOptions` includes only one of the fields `mdbClusterId`, `onPremise`<br>
settings.<br>mongoSource.<br>connection.<br>connectionOptions.<br>onPremise | **object** <br>`settings.mongoSource.connection.connectionOptions` includes only one of the fields `mdbClusterId`, `onPremise`<br>
settings.<br>mongoSource.<br>connection.<br>connectionOptions.<br>onPremise.<br>hosts[] | **string**
settings.<br>mongoSource.<br>connection.<br>connectionOptions.<br>onPremise.<br>port | **string** (int64)
settings.<br>mongoSource.<br>connection.<br>connectionOptions.<br>onPremise.<br>tlsMode | **object**
settings.<br>mongoSource.<br>connection.<br>connectionOptions.<br>onPremise.<br>tlsMode.<br>disabled | **object** <br>`settings.mongoSource.connection.connectionOptions.onPremise.tlsMode` includes only one of the fields `disabled`, `enabled`<br><br><p>Empty JSON object ``{}``.</p> 
settings.<br>mongoSource.<br>connection.<br>connectionOptions.<br>onPremise.<br>tlsMode.<br>disabled.<br>disabled | **object** <br>`settings.mongoSource.connection.connectionOptions.onPremise.tlsMode` includes only one of the fields `disabled`, `enabled`<br><br><p>Empty JSON object ``{}``.</p> 
settings.<br>mongoSource.<br>connection.<br>connectionOptions.<br>onPremise.<br>tlsMode.<br>enabled | **object** <br>`settings.mongoSource.connection.connectionOptions.onPremise.tlsMode` includes only one of the fields `disabled`, `enabled`<br>
settings.<br>mongoSource.<br>connection.<br>connectionOptions.<br>onPremise.<br>tlsMode.<br>enabled.<br>caCertificate | **string**<br><p>CA certificate</p> <p>X.509 certificate of the certificate authority which issued the server's certificate, in PEM format. When CA certificate is specified TLS is used to connect to the server.</p> 
settings.<br>mongoSource.<br>connection.<br>connectionOptions.<br>onPremise.<br>replicaSet | **string**
settings.<br>mongoSource.<br>subnetId | **string**
settings.<br>mongoSource.<br>securityGroups[] | **string**<br><p>Security groups</p> 
settings.<br>mongoSource.<br>collections[] | **object**<br><p>List of collections for replication. Empty list implies replication of all tables on the deployment. Allowed to use * as collection name.</p> 
settings.<br>mongoSource.<br>collections[].<br>databaseName | **string**
settings.<br>mongoSource.<br>collections[].<br>collectionName | **string**
settings.<br>mongoSource.<br>excludedCollections[] | **object**<br><p>List of forbidden collections for replication. Allowed to use * as collection name for forbid all collections of concrete schema.</p> 
settings.<br>mongoSource.<br>excludedCollections[].<br>databaseName | **string**
settings.<br>mongoSource.<br>excludedCollections[].<br>collectionName | **string**
settings.<br>mongoSource.<br>secondaryPreferredMode | **boolean** (boolean)<br><p>Read mode for mongo client</p> 
settings.<br>clickhouseSource | **object** <br>`settings` includes only one of the fields `mysqlSource`, `postgresSource`, `ydbSource`, `kafkaSource`, `mongoSource`, `clickhouseSource`, `mysqlTarget`, `postgresTarget`, `clickhouseTarget`, `ydbTarget`, `kafkaTarget`, `mongoTarget`<br>
settings.<br>clickhouseSource.<br>connection | **object**
settings.<br>clickhouseSource.<br>connection.<br>connectionOptions | **object**
settings.<br>clickhouseSource.<br>connection.<br>connectionOptions.<br>database | **string**<br><p>Database</p> 
settings.<br>clickhouseSource.<br>connection.<br>connectionOptions.<br>user | **string**
settings.<br>clickhouseSource.<br>connection.<br>connectionOptions.<br>password | **object**<br>Password for user
settings.<br>clickhouseSource.<br>connection.<br>connectionOptions.<br>password.<br>raw | **string**<br><p>Raw secret value</p> 
settings.<br>clickhouseSource.<br>connection.<br>connectionOptions.<br>mdbClusterId | **string** <br>`settings.clickhouseSource.connection.connectionOptions` includes only one of the fields `mdbClusterId`, `onPremise`<br>
settings.<br>clickhouseSource.<br>connection.<br>connectionOptions.<br>onPremise | **object** <br>`settings.clickhouseSource.connection.connectionOptions` includes only one of the fields `mdbClusterId`, `onPremise`<br>
settings.<br>clickhouseSource.<br>connection.<br>connectionOptions.<br>onPremise.<br>shards[] | **object**
settings.<br>clickhouseSource.<br>connection.<br>connectionOptions.<br>onPremise.<br>shards[].<br>name | **string**
settings.<br>clickhouseSource.<br>connection.<br>connectionOptions.<br>onPremise.<br>shards[].<br>hosts[] | **string**
settings.<br>clickhouseSource.<br>connection.<br>connectionOptions.<br>onPremise.<br>httpPort | **string** (int64)
settings.<br>clickhouseSource.<br>connection.<br>connectionOptions.<br>onPremise.<br>nativePort | **string** (int64)
settings.<br>clickhouseSource.<br>connection.<br>connectionOptions.<br>onPremise.<br>tlsMode | **object**
settings.<br>clickhouseSource.<br>connection.<br>connectionOptions.<br>onPremise.<br>tlsMode.<br>disabled | **object** <br>`settings.clickhouseSource.connection.connectionOptions.onPremise.tlsMode` includes only one of the fields `disabled`, `enabled`<br><br><p>Empty JSON object ``{}``.</p> 
settings.<br>clickhouseSource.<br>connection.<br>connectionOptions.<br>onPremise.<br>tlsMode.<br>disabled.<br>disabled | **object** <br>`settings.clickhouseSource.connection.connectionOptions.onPremise.tlsMode` includes only one of the fields `disabled`, `enabled`<br><br><p>Empty JSON object ``{}``.</p> 
settings.<br>clickhouseSource.<br>connection.<br>connectionOptions.<br>onPremise.<br>tlsMode.<br>enabled | **object** <br>`settings.clickhouseSource.connection.connectionOptions.onPremise.tlsMode` includes only one of the fields `disabled`, `enabled`<br>
settings.<br>clickhouseSource.<br>connection.<br>connectionOptions.<br>onPremise.<br>tlsMode.<br>enabled.<br>caCertificate | **string**<br><p>CA certificate</p> <p>X.509 certificate of the certificate authority which issued the server's certificate, in PEM format. When CA certificate is specified TLS is used to connect to the server.</p> 
settings.<br>clickhouseSource.<br>subnetId | **string**
settings.<br>clickhouseSource.<br>securityGroups[] | **string**
settings.<br>clickhouseSource.<br>includeTables[] | **string**<br><p>While list of tables for replication. If none or empty list is presented - will replicate all tables. Can contain * patterns.</p> 
settings.<br>clickhouseSource.<br>excludeTables[] | **string**<br><p>Exclude list of tables for replication. If none or empty list is presented - will replicate all tables. Can contain * patterns.</p> 
settings.<br>mysqlTarget | **object** <br>`settings` includes only one of the fields `mysqlSource`, `postgresSource`, `ydbSource`, `kafkaSource`, `mongoSource`, `clickhouseSource`, `mysqlTarget`, `postgresTarget`, `clickhouseTarget`, `ydbTarget`, `kafkaTarget`, `mongoTarget`<br>
settings.<br>mysqlTarget.<br>connection | **object**<br><p>Database connection settings</p> 
settings.<br>mysqlTarget.<br>connection.<br>mdbClusterId | **string** <br>`settings.mysqlTarget.connection` includes only one of the fields `mdbClusterId`, `onPremise`<br><br><p>Managed Service for MySQL cluster ID</p> 
settings.<br>mysqlTarget.<br>connection.<br>onPremise | **object**<br>Connection options for on-premise MySQL <br>`settings.mysqlTarget.connection` includes only one of the fields `mdbClusterId`, `onPremise`<br>
settings.<br>mysqlTarget.<br>connection.<br>onPremise.<br>hosts[] | **string**
settings.<br>mysqlTarget.<br>connection.<br>onPremise.<br>port | **string** (int64)<br><p>Database port</p> 
settings.<br>mysqlTarget.<br>connection.<br>onPremise.<br>tlsMode | **object**<br><p>TLS settings for server connection. Disabled by default.</p> 
settings.<br>mysqlTarget.<br>connection.<br>onPremise.<br>tlsMode.<br>disabled | **object** <br>`settings.mysqlTarget.connection.onPremise.tlsMode` includes only one of the fields `disabled`, `enabled`<br><br><p>Empty JSON object ``{}``.</p> 
settings.<br>mysqlTarget.<br>connection.<br>onPremise.<br>tlsMode.<br>disabled.<br>disabled | **object** <br>`settings.mysqlTarget.connection.onPremise.tlsMode` includes only one of the fields `disabled`, `enabled`<br><br><p>Empty JSON object ``{}``.</p> 
settings.<br>mysqlTarget.<br>connection.<br>onPremise.<br>tlsMode.<br>enabled | **object** <br>`settings.mysqlTarget.connection.onPremise.tlsMode` includes only one of the fields `disabled`, `enabled`<br>
settings.<br>mysqlTarget.<br>connection.<br>onPremise.<br>tlsMode.<br>enabled.<br>caCertificate | **string**<br><p>CA certificate</p> <p>X.509 certificate of the certificate authority which issued the server's certificate, in PEM format. When CA certificate is specified TLS is used to connect to the server.</p> 
settings.<br>mysqlTarget.<br>connection.<br>onPremise.<br>subnetId | **string**<br><p>Network interface for endpoint. If none will assume public ipv4</p> 
settings.<br>mysqlTarget.<br>securityGroups[] | **string**<br><p>Security groups</p> 
settings.<br>mysqlTarget.<br>database | **string**<br><p>Database name</p> <p>Allowed to leave it empty, then the tables will be created in databases with the same names as on the source. If this field is empty, then you must fill below db schema for service table.</p> 
settings.<br>mysqlTarget.<br>user | **string**<br><p>User for database access.</p> 
settings.<br>mysqlTarget.<br>password | **object**<br><p>Password for database access.</p> 
settings.<br>mysqlTarget.<br>password.<br>raw | **string**<br><p>Raw secret value</p> 
settings.<br>mysqlTarget.<br>sqlMode | **string**<br><p>Default: NO_AUTO_VALUE_ON_ZERO,NO_DIR_IN_CREATE,NO_ENGINE_SUBSTITUTION.</p> 
settings.<br>mysqlTarget.<br>skipConstraintChecks | **boolean** (boolean)<br><p>Disable constraints checks</p> <p>Recommend to disable for increase replication speed, but if schema contain cascading operations we don't recommend to disable. This option set FOREIGN_KEY_CHECKS=0 and UNIQUE_CHECKS=0.</p> 
settings.<br>mysqlTarget.<br>timezone | **string**<br><p>Database timezone</p> <p>Is used for parsing timestamps for saving source timezones. Accepts values from IANA timezone database. Default: local timezone.</p> 
settings.<br>mysqlTarget.<br>cleanupPolicy | **string**<br><p>Cleanup policy</p> <p>Cleanup policy for activate, reactivate and reupload processes. Default is DISABLED.</p> <ul> <li>DISABLED: Don't cleanup</li> <li>DROP: Drop</li> <li>TRUNCATE: Truncate</li> </ul> 
settings.<br>mysqlTarget.<br>serviceDatabase | **string**<br><p>Database schema for service table</p> <p>Default: db name. Here created technical tables (__tm_keeper, __tm_gtid_keeper).</p> 
settings.<br>postgresTarget | **object** <br>`settings` includes only one of the fields `mysqlSource`, `postgresSource`, `ydbSource`, `kafkaSource`, `mongoSource`, `clickhouseSource`, `mysqlTarget`, `postgresTarget`, `clickhouseTarget`, `ydbTarget`, `kafkaTarget`, `mongoTarget`<br>
settings.<br>postgresTarget.<br>connection | **object**<br><p>Database connection settings</p> 
settings.<br>postgresTarget.<br>connection.<br>mdbClusterId | **string** <br>`settings.postgresTarget.connection` includes only one of the fields `mdbClusterId`, `onPremise`<br><br><p>Managed Service for PostgreSQL cluster ID</p> 
settings.<br>postgresTarget.<br>connection.<br>onPremise | **object**<br>Connection options for on-premise PostgreSQL <br>`settings.postgresTarget.connection` includes only one of the fields `mdbClusterId`, `onPremise`<br>
settings.<br>postgresTarget.<br>connection.<br>onPremise.<br>hosts[] | **string**
settings.<br>postgresTarget.<br>connection.<br>onPremise.<br>port | **string** (int64)<br><p>Will be used if the cluster ID is not specified.</p> 
settings.<br>postgresTarget.<br>connection.<br>onPremise.<br>tlsMode | **object**<br><p>TLS settings for server connection. Disabled by default.</p> 
settings.<br>postgresTarget.<br>connection.<br>onPremise.<br>tlsMode.<br>disabled | **object** <br>`settings.postgresTarget.connection.onPremise.tlsMode` includes only one of the fields `disabled`, `enabled`<br><br><p>Empty JSON object ``{}``.</p> 
settings.<br>postgresTarget.<br>connection.<br>onPremise.<br>tlsMode.<br>disabled.<br>disabled | **object** <br>`settings.postgresTarget.connection.onPremise.tlsMode` includes only one of the fields `disabled`, `enabled`<br><br><p>Empty JSON object ``{}``.</p> 
settings.<br>postgresTarget.<br>connection.<br>onPremise.<br>tlsMode.<br>enabled | **object** <br>`settings.postgresTarget.connection.onPremise.tlsMode` includes only one of the fields `disabled`, `enabled`<br>
settings.<br>postgresTarget.<br>connection.<br>onPremise.<br>tlsMode.<br>enabled.<br>caCertificate | **string**<br><p>CA certificate</p> <p>X.509 certificate of the certificate authority which issued the server's certificate, in PEM format. When CA certificate is specified TLS is used to connect to the server.</p> 
settings.<br>postgresTarget.<br>connection.<br>onPremise.<br>subnetId | **string**<br><p>Network interface for endpoint. If none will assume public ipv4</p> 
settings.<br>postgresTarget.<br>securityGroups[] | **string**<br><p>Security groups</p> 
settings.<br>postgresTarget.<br>database | **string**<br><p>Database name</p> 
settings.<br>postgresTarget.<br>user | **string**<br><p>User for database access.</p> 
settings.<br>postgresTarget.<br>password | **object**<br><p>Password for database access.</p> 
settings.<br>postgresTarget.<br>password.<br>raw | **string**<br><p>Raw secret value</p> 
settings.<br>postgresTarget.<br>cleanupPolicy | **string**<br><p>Cleanup policy for activate, reactivate and reupload processes. Default is truncate.</p> <ul> <li>DISABLED: Don't cleanup</li> <li>DROP: Drop</li> <li>TRUNCATE: Truncate</li> </ul> 
settings.<br>clickhouseTarget | **object** <br>`settings` includes only one of the fields `mysqlSource`, `postgresSource`, `ydbSource`, `kafkaSource`, `mongoSource`, `clickhouseSource`, `mysqlTarget`, `postgresTarget`, `clickhouseTarget`, `ydbTarget`, `kafkaTarget`, `mongoTarget`<br>
settings.<br>clickhouseTarget.<br>connection | **object**
settings.<br>clickhouseTarget.<br>connection.<br>connectionOptions | **object**
settings.<br>clickhouseTarget.<br>connection.<br>connectionOptions.<br>database | **string**<br><p>Database</p> 
settings.<br>clickhouseTarget.<br>connection.<br>connectionOptions.<br>user | **string**
settings.<br>clickhouseTarget.<br>connection.<br>connectionOptions.<br>password | **object**<br>Password for user
settings.<br>clickhouseTarget.<br>connection.<br>connectionOptions.<br>password.<br>raw | **string**<br><p>Raw secret value</p> 
settings.<br>clickhouseTarget.<br>connection.<br>connectionOptions.<br>mdbClusterId | **string** <br>`settings.clickhouseTarget.connection.connectionOptions` includes only one of the fields `mdbClusterId`, `onPremise`<br>
settings.<br>clickhouseTarget.<br>connection.<br>connectionOptions.<br>onPremise | **object** <br>`settings.clickhouseTarget.connection.connectionOptions` includes only one of the fields `mdbClusterId`, `onPremise`<br>
settings.<br>clickhouseTarget.<br>connection.<br>connectionOptions.<br>onPremise.<br>shards[] | **object**
settings.<br>clickhouseTarget.<br>connection.<br>connectionOptions.<br>onPremise.<br>shards[].<br>name | **string**
settings.<br>clickhouseTarget.<br>connection.<br>connectionOptions.<br>onPremise.<br>shards[].<br>hosts[] | **string**
settings.<br>clickhouseTarget.<br>connection.<br>connectionOptions.<br>onPremise.<br>httpPort | **string** (int64)
settings.<br>clickhouseTarget.<br>connection.<br>connectionOptions.<br>onPremise.<br>nativePort | **string** (int64)
settings.<br>clickhouseTarget.<br>connection.<br>connectionOptions.<br>onPremise.<br>tlsMode | **object**
settings.<br>clickhouseTarget.<br>connection.<br>connectionOptions.<br>onPremise.<br>tlsMode.<br>disabled | **object** <br>`settings.clickhouseTarget.connection.connectionOptions.onPremise.tlsMode` includes only one of the fields `disabled`, `enabled`<br><br><p>Empty JSON object ``{}``.</p> 
settings.<br>clickhouseTarget.<br>connection.<br>connectionOptions.<br>onPremise.<br>tlsMode.<br>disabled.<br>disabled | **object** <br>`settings.clickhouseTarget.connection.connectionOptions.onPremise.tlsMode` includes only one of the fields `disabled`, `enabled`<br><br><p>Empty JSON object ``{}``.</p> 
settings.<br>clickhouseTarget.<br>connection.<br>connectionOptions.<br>onPremise.<br>tlsMode.<br>enabled | **object** <br>`settings.clickhouseTarget.connection.connectionOptions.onPremise.tlsMode` includes only one of the fields `disabled`, `enabled`<br>
settings.<br>clickhouseTarget.<br>connection.<br>connectionOptions.<br>onPremise.<br>tlsMode.<br>enabled.<br>caCertificate | **string**<br><p>CA certificate</p> <p>X.509 certificate of the certificate authority which issued the server's certificate, in PEM format. When CA certificate is specified TLS is used to connect to the server.</p> 
settings.<br>clickhouseTarget.<br>subnetId | **string**
settings.<br>clickhouseTarget.<br>securityGroups[] | **string**
settings.<br>clickhouseTarget.<br>clickhouseClusterName | **string**
settings.<br>clickhouseTarget.<br>altNames[] | **object**<br><p>Alternative table names in target</p> 
settings.<br>clickhouseTarget.<br>altNames[].<br>fromName | **string**<br><p>Source table name</p> 
settings.<br>clickhouseTarget.<br>altNames[].<br>toName | **string**<br><p>Target table name</p> 
settings.<br>clickhouseTarget.<br>sharding | **object**
settings.<br>clickhouseTarget.<br>sharding.<br>columnValueHash | **object** <br>`settings.clickhouseTarget.sharding` includes only one of the fields `columnValueHash`, `customMapping`, `transferId`, `roundRobin`<br>
settings.<br>clickhouseTarget.<br>sharding.<br>columnValueHash.<br>columnName | **string**
settings.<br>clickhouseTarget.<br>sharding.<br>customMapping | **object** <br>`settings.clickhouseTarget.sharding` includes only one of the fields `columnValueHash`, `customMapping`, `transferId`, `roundRobin`<br>
settings.<br>clickhouseTarget.<br>sharding.<br>customMapping.<br>columnName | **string**
settings.<br>clickhouseTarget.<br>sharding.<br>customMapping.<br>mapping[] | **object**
settings.<br>clickhouseTarget.<br>sharding.<br>customMapping.<br>mapping[].<br>columnValue | **object**
settings.<br>clickhouseTarget.<br>sharding.<br>customMapping.<br>mapping[].<br>columnValue.<br>stringValue | **string**
settings.<br>clickhouseTarget.<br>sharding.<br>customMapping.<br>mapping[].<br>shardName | **string**
settings.<br>clickhouseTarget.<br>sharding.<br>transferId | **object** <br>`settings.clickhouseTarget.sharding` includes only one of the fields `columnValueHash`, `customMapping`, `transferId`, `roundRobin`<br><br><p>Empty JSON object ``{}``.</p> 
settings.<br>clickhouseTarget.<br>sharding.<br>transferId.<br>transferId | **object** <br>`settings.clickhouseTarget.sharding` includes only one of the fields `columnValueHash`, `customMapping`, `transferId`, `roundRobin`<br><br><p>Empty JSON object ``{}``.</p> 
settings.<br>clickhouseTarget.<br>sharding.<br>roundRobin | **object** <br>`settings.clickhouseTarget.sharding` includes only one of the fields `columnValueHash`, `customMapping`, `transferId`, `roundRobin`<br><br><p>Empty JSON object ``{}``.</p> 
settings.<br>clickhouseTarget.<br>sharding.<br>roundRobin.<br>roundRobin | **object** <br>`settings.clickhouseTarget.sharding` includes only one of the fields `columnValueHash`, `customMapping`, `transferId`, `roundRobin`<br><br><p>Empty JSON object ``{}``.</p> 
settings.<br>clickhouseTarget.<br>cleanupPolicy | **string**
settings.<br>ydbTarget | **object** <br>`settings` includes only one of the fields `mysqlSource`, `postgresSource`, `ydbSource`, `kafkaSource`, `mongoSource`, `clickhouseSource`, `mysqlTarget`, `postgresTarget`, `clickhouseTarget`, `ydbTarget`, `kafkaTarget`, `mongoTarget`<br>
settings.<br>ydbTarget.<br>database | **string**<br><p>Path in YDB where to store tables</p> 
settings.<br>ydbTarget.<br>instance | **string**<br><p>Instance of YDB. example: ydb-ru-prestable.yandex.net:2135</p> 
settings.<br>ydbTarget.<br>serviceAccountId | **string**
settings.<br>ydbTarget.<br>path | **string**<br><p>Path extension for database, each table will be layouted into this path</p> 
settings.<br>ydbTarget.<br>subnetId | **string**<br><p>Network interface for endpoint. If none will assume public ipv4</p> 
settings.<br>ydbTarget.<br>securityGroups[] | **string**<br><p>Security groups</p> 
settings.<br>ydbTarget.<br>saKeyContent | **string**<br><p>SA content</p> 
settings.<br>ydbTarget.<br>cleanupPolicy | **string**<br><p>Cleanup policy</p> 
settings.<br>ydbTarget.<br>isTableColumnOriented | **boolean** (boolean)<br><p>Should create column-oriented table (OLAP). By default it creates row-oriented (OLTP)</p> 
settings.<br>kafkaTarget | **object** <br>`settings` includes only one of the fields `mysqlSource`, `postgresSource`, `ydbSource`, `kafkaSource`, `mongoSource`, `clickhouseSource`, `mysqlTarget`, `postgresTarget`, `clickhouseTarget`, `ydbTarget`, `kafkaTarget`, `mongoTarget`<br>
settings.<br>kafkaTarget.<br>connection | **object**<br><p>Connection settings</p> 
settings.<br>kafkaTarget.<br>connection.<br>clusterId | **string** <br>`settings.kafkaTarget.connection` includes only one of the fields `clusterId`, `onPremise`<br><br><p>Managed Service for Kafka cluster ID</p> 
settings.<br>kafkaTarget.<br>connection.<br>onPremise | **object**<br>Connection options for on-premise Kafka <br>`settings.kafkaTarget.connection` includes only one of the fields `clusterId`, `onPremise`<br>
settings.<br>kafkaTarget.<br>connection.<br>onPremise.<br>brokerUrls[] | **string**<br><p>Kafka broker URLs</p> 
settings.<br>kafkaTarget.<br>connection.<br>onPremise.<br>tlsMode | **object**<br><p>TLS settings for broker connection. Disabled by default.</p> 
settings.<br>kafkaTarget.<br>connection.<br>onPremise.<br>tlsMode.<br>disabled | **object** <br>`settings.kafkaTarget.connection.onPremise.tlsMode` includes only one of the fields `disabled`, `enabled`<br><br><p>Empty JSON object ``{}``.</p> 
settings.<br>kafkaTarget.<br>connection.<br>onPremise.<br>tlsMode.<br>disabled.<br>disabled | **object** <br>`settings.kafkaTarget.connection.onPremise.tlsMode` includes only one of the fields `disabled`, `enabled`<br><br><p>Empty JSON object ``{}``.</p> 
settings.<br>kafkaTarget.<br>connection.<br>onPremise.<br>tlsMode.<br>enabled | **object** <br>`settings.kafkaTarget.connection.onPremise.tlsMode` includes only one of the fields `disabled`, `enabled`<br>
settings.<br>kafkaTarget.<br>connection.<br>onPremise.<br>tlsMode.<br>enabled.<br>caCertificate | **string**<br><p>CA certificate</p> <p>X.509 certificate of the certificate authority which issued the server's certificate, in PEM format. When CA certificate is specified TLS is used to connect to the server.</p> 
settings.<br>kafkaTarget.<br>connection.<br>onPremise.<br>subnetId | **string**<br><p>Network interface for endpoint. If none will assume public ipv4</p> 
settings.<br>kafkaTarget.<br>auth | **object**<br><p>Authentication settings</p> 
settings.<br>kafkaTarget.<br>auth.<br>sasl | **object**<br>Authentication with SASL <br>`settings.kafkaTarget.auth` includes only one of the fields `sasl`, `noAuth`<br>
settings.<br>kafkaTarget.<br>auth.<br>sasl.<br>user | **string**<br><p>User name</p> 
settings.<br>kafkaTarget.<br>auth.<br>sasl.<br>password | **object**<br><p>Password for user</p> 
settings.<br>kafkaTarget.<br>auth.<br>sasl.<br>password.<br>raw | **string**<br><p>Raw secret value</p> 
settings.<br>kafkaTarget.<br>auth.<br>sasl.<br>mechanism | **string**<br><p>SASL mechanism for authentication</p> 
settings.<br>kafkaTarget.<br>auth.<br>noAuth | **object**<br>No authentication <br>`settings.kafkaTarget.auth` includes only one of the fields `sasl`, `noAuth`<br>
settings.<br>kafkaTarget.<br>securityGroups[] | **string**<br><p>Security groups</p> 
settings.<br>kafkaTarget.<br>topicSettings | **object**<br><p>Target topic settings</p> 
settings.<br>kafkaTarget.<br>topicSettings.<br>topic | **object** <br>`settings.kafkaTarget.topicSettings` includes only one of the fields `topic`, `topicPrefix`<br>
settings.<br>kafkaTarget.<br>topicSettings.<br>topic.<br>topicName | **string**<br><p>Topic name</p> 
settings.<br>kafkaTarget.<br>topicSettings.<br>topic.<br>saveTxOrder | **boolean** (boolean)<br><p>Save transactions order Not to split events queue into separate per-table queues.</p> 
settings.<br>kafkaTarget.<br>topicSettings.<br>topicPrefix | **string** <br>`settings.kafkaTarget.topicSettings` includes only one of the fields `topic`, `topicPrefix`<br><br><p>Topic prefix</p> <p>Analogue of the Debezium setting database.server.name. Messages will be sent to topic with name &lt;topic_prefix&gt;.<schema>.&lt;table_name&gt;.</p> 
settings.<br>kafkaTarget.<br>serializer | **object**<br><p>Data serialization format settings</p> <p>Data serialization format</p> 
settings.<br>kafkaTarget.<br>serializer.<br>serializerAuto | **object** <br>`settings.kafkaTarget.serializer` includes only one of the fields `serializerAuto`, `serializerJson`, `serializerDebezium`<br>
settings.<br>kafkaTarget.<br>serializer.<br>serializerJson | **object** <br>`settings.kafkaTarget.serializer` includes only one of the fields `serializerAuto`, `serializerJson`, `serializerDebezium`<br>
settings.<br>kafkaTarget.<br>serializer.<br>serializerDebezium | **object** <br>`settings.kafkaTarget.serializer` includes only one of the fields `serializerAuto`, `serializerJson`, `serializerDebezium`<br>
settings.<br>kafkaTarget.<br>serializer.<br>serializerDebezium.<br>serializerParameters[] | **object**<br><p>Settings of sterilization parameters as key-value pairs</p> 
settings.<br>kafkaTarget.<br>serializer.<br>serializerDebezium.<br>serializerParameters[].<br>key | **string**<br><p>Name of the serializer parameter</p> 
settings.<br>kafkaTarget.<br>serializer.<br>serializerDebezium.<br>serializerParameters[].<br>value | **string**<br><p>Value of the serializer parameter</p> 
settings.<br>mongoTarget | **object** <br>`settings` includes only one of the fields `mysqlSource`, `postgresSource`, `ydbSource`, `kafkaSource`, `mongoSource`, `clickhouseSource`, `mysqlTarget`, `postgresTarget`, `clickhouseTarget`, `ydbTarget`, `kafkaTarget`, `mongoTarget`<br>
settings.<br>mongoTarget.<br>connection | **object**
settings.<br>mongoTarget.<br>connection.<br>connectionOptions | **object**
settings.<br>mongoTarget.<br>connection.<br>connectionOptions.<br>user | **string**<br><p>User name</p> 
settings.<br>mongoTarget.<br>connection.<br>connectionOptions.<br>password | **object**<br>Password for user
settings.<br>mongoTarget.<br>connection.<br>connectionOptions.<br>password.<br>raw | **string**<br><p>Raw secret value</p> 
settings.<br>mongoTarget.<br>connection.<br>connectionOptions.<br>authSource | **string**<br><p>Database name associated with the credentials</p> 
settings.<br>mongoTarget.<br>connection.<br>connectionOptions.<br>mdbClusterId | **string** <br>`settings.mongoTarget.connection.connectionOptions` includes only one of the fields `mdbClusterId`, `onPremise`<br>
settings.<br>mongoTarget.<br>connection.<br>connectionOptions.<br>onPremise | **object** <br>`settings.mongoTarget.connection.connectionOptions` includes only one of the fields `mdbClusterId`, `onPremise`<br>
settings.<br>mongoTarget.<br>connection.<br>connectionOptions.<br>onPremise.<br>hosts[] | **string**
settings.<br>mongoTarget.<br>connection.<br>connectionOptions.<br>onPremise.<br>port | **string** (int64)
settings.<br>mongoTarget.<br>connection.<br>connectionOptions.<br>onPremise.<br>tlsMode | **object**
settings.<br>mongoTarget.<br>connection.<br>connectionOptions.<br>onPremise.<br>tlsMode.<br>disabled | **object** <br>`settings.mongoTarget.connection.connectionOptions.onPremise.tlsMode` includes only one of the fields `disabled`, `enabled`<br><br><p>Empty JSON object ``{}``.</p> 
settings.<br>mongoTarget.<br>connection.<br>connectionOptions.<br>onPremise.<br>tlsMode.<br>disabled.<br>disabled | **object** <br>`settings.mongoTarget.connection.connectionOptions.onPremise.tlsMode` includes only one of the fields `disabled`, `enabled`<br><br><p>Empty JSON object ``{}``.</p> 
settings.<br>mongoTarget.<br>connection.<br>connectionOptions.<br>onPremise.<br>tlsMode.<br>enabled | **object** <br>`settings.mongoTarget.connection.connectionOptions.onPremise.tlsMode` includes only one of the fields `disabled`, `enabled`<br>
settings.<br>mongoTarget.<br>connection.<br>connectionOptions.<br>onPremise.<br>tlsMode.<br>enabled.<br>caCertificate | **string**<br><p>CA certificate</p> <p>X.509 certificate of the certificate authority which issued the server's certificate, in PEM format. When CA certificate is specified TLS is used to connect to the server.</p> 
settings.<br>mongoTarget.<br>connection.<br>connectionOptions.<br>onPremise.<br>replicaSet | **string**
settings.<br>mongoTarget.<br>subnetId | **string**
settings.<br>mongoTarget.<br>securityGroups[] | **string**<br><p>Security groups</p> 
settings.<br>mongoTarget.<br>database | **string**<br><p>Database name</p> 
settings.<br>mongoTarget.<br>cleanupPolicy | **string**<br><ul> <li>DISABLED: Don't cleanup</li> <li>DROP: Drop</li> <li>TRUNCATE: Truncate</li> </ul> 

## Methods {#methods}
Method | Description
--- | ---
[create](create.md) | 
[delete](delete.md) | 
[get](get.md) | 
[list](list.md) | 
[update](update.md) | 