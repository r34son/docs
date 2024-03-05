# Viewing operations with the service's resources

All actions with {{ vpc-name }} resources are logged as a list of operations. Each operation is assigned a unique ID.

## Get a list of operations {#get-operations}

{% list tabs group=instructions %}

- Management console {#console}

   To see all operations with the service's resources, in the left-hand panel, select ![image](../../_assets/operations.svg) **{{ ui-key.yacloud.common.operations-key-value }}**. In the list that opens, you will also see operations with deleted resources.

   You can get a list of operations for a specific resource. The steps below describe how you can do this for a cloud network. The same steps apply to other service resources.

   1. In the [management console]({{ link-console-main }}), go to the folder where the cloud network is located.
   1. In the list of services, select **{{ ui-key.yacloud.iam.folder.dashboard.label_vpc }}**.
   1. Select the network you need.
   1. Go to the ![image](../../_assets/operations.svg) **{{ ui-key.yacloud.common.operations-key-value }}** panel.

      You will see a list of cloud network operations.

- CLI {#cli}

   {% include [cli-install](../../_includes/cli-install.md) %}

   {% include [default-catalogue](../../_includes/default-catalogue.md) %}

   To get a list of operations for a {{ vpc-name }} resource, run this command:

   ```bash
   yc vpc <resource_type> list-operations <resource_ID_or_name>
   ```

   **Example**

   Get a list of operations for a cloud network:

   ```bash
   yc vpc network list-operations enpgl5o8te3k********
   ```

   Result:

   ```text
   +----------------------+---------------------+----------------------+---------------------+--------+----------------+
   |          ID          |     CREATED AT      |      CREATED BY      |     MODIFIED AT     | STATUS |  DESCRIPTION   |
   +----------------------+---------------------+----------------------+---------------------+--------+----------------+
   | enp75021agjg******** | 2024-02-01 10:16:51 | ajego134p5h1******** | 2024-02-01 10:16:53 | DONE   | Create network |
   +----------------------+---------------------+----------------------+---------------------+--------+----------------+
   ```

   By default, information about operations is provided as text. To get more detailed information, specify the `yaml` or `json` output data format using the `--format` flag:

   ```bash
   yc vpc network list-operations enpgl5o8te3k******** --format yaml
   ```

   Result:

   ```yaml
   - id: enp75021agjg********
     description: Create network
     created_at: "2024-02-01T10:16:51.955935138Z"
     created_by: ajego134p5h1********
     modified_at: "2024-02-01T10:16:53.389542083Z"
     done: true
     metadata:
       '@type': type.googleapis.com/yandex.cloud.vpc.v1.CreateNetworkMetadata
       network_id: enpgl5o8te3k********
     response:
       '@type': type.googleapis.com/google.protobuf.Empty
       value: {}
   ```

- API {#api}

   To get a list of operations, use the `listOperations` REST API method for the appropriate resource or the `<service>/ListOperations` gRPC API call.

   For example, to obtain a list of operations for a cloud network, use either the [listOperations](../api-ref/Network/listOperations.md) REST API method for the [Network](../api-ref/Network/index.md) resource or the [NetworkService/ListOperations](../api-ref/grpc/network_service.md#ListOperations) gRPC API call.

{% endlist %}

## Get detailed information about an operation {#get-operations-info}

1. [Get a list of operations](#get-operations) for the resource.
1. Copy the ID of the operation.
1. Get detailed information about the operation:

   {% list tabs group=instructions %}

   - CLI {#cli}

      {% include [cli-install](../../_includes/cli-install.md) %}

      {% include [default-catalogue](../../_includes/default-catalogue.md) %}

      Run this command:

      ```bash
      yc operation get <operation_ID>
      ```

      Result:

      ```yaml
      id: enp75021agjg********
      description: Create network
      created_at: "2024-02-01T10:16:51.955Z"
      created_by: ajego134p5h1********
      modified_at: "2024-02-01T10:16:53.389Z"
      done: true
      metadata:
        '@type': type.googleapis.com/yandex.cloud.vpc.v1.CreateNetworkMetadata
        network_id: enpgl5o8te3k********
      response:
        '@type': type.googleapis.com/yandex.cloud.vpc.v1.Network
        id: enpgl5o8te3kke6q3psa
        folder_id: b1gmit33ngp3********
        created_at: "2024-02-01T10:16:51Z"
        name: test-network
        default_security_group_id: enp0catll8gm********
      ```

   - API {#api}

      Use the [get](../api-ref/Operation/get.md) REST API method for the [Operation](../api-ref/Operation/index.md) resource or the [OperationService/Get](../api-ref/grpc/operation_service.md#Get) gRPC API call.

   {% endlist %}

#### See also {#see-also}

* [{#T}](../../api-design-guide/concepts/about-async.md)
