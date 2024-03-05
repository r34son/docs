---
editable: false
sourcePath: en/_cli-ref/cli-ref/managed-services/organization-manager/federation/saml/delete-user-accounts.md
---

# yc organization-manager federation saml delete-user-accounts

Delete user accounts from the specified federation

#### Command Usage

Syntax: 

`yc organization-manager federation saml delete-user-accounts <FEDERATION-NAME>|<FEDERATION-ID> [Flags...] [Global Flags...]`

#### Flags

| Flag | Description |
|----|----|
|`--id`|<b>`string`</b><br/>federation id.|
|`--name`|<b>`string`</b><br/>federation name.|
|`--organization-id`|<b>`string`</b><br/>Set the ID of the organization to use.|
|`--async`|Display information about the operation in progress, without waiting for the operation to complete.|
|`--subject-ids`|<b>`value[,value]`</b><br/>Unique subject identifiers to delete from the federation.|

#### Global Flags

| Flag | Description |
|----|----|
|`--profile`|<b>`string`</b><br/>Set the custom configuration file.|
|`--debug`|Debug logging.|
|`--debug-grpc`|Debug gRPC logging. Very verbose, used for debugging connection problems.|
|`--no-user-output`|Disable printing user intended output to stderr.|
|`--retry`|<b>`int`</b><br/>Enable gRPC retries. By default, retries are enabled with maximum 5 attempts.<br/>Pass 0 to disable retries. Pass any negative value for infinite retries.<br/>Even infinite retries are capped with 2 minutes timeout.|
|`--cloud-id`|<b>`string`</b><br/>Set the ID of the cloud to use.|
|`--folder-id`|<b>`string`</b><br/>Set the ID of the folder to use.|
|`--folder-name`|<b>`string`</b><br/>Set the name of the folder to use (will be resolved to id).|
|`--endpoint`|<b>`string`</b><br/>Set the Cloud API endpoint (host:port).|
|`--token`|<b>`string`</b><br/>Set the OAuth token to use.|
|`--impersonate-service-account-id`|<b>`string`</b><br/>Set the ID of the service account to impersonate.|
|`--no-browser`|Disable opening browser for authentication.|
|`--format`|<b>`string`</b><br/>Set the output format: text (default), yaml, json, json-rest.|
|`-h`,`--help`|Display help for the command.|
