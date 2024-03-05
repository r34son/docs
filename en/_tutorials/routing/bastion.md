# Creating a bastion host

If you have ever had an interest in early modern fortifications, the word _bastion_ should sound familiar to you. A bastion is a structure projecting outward from the outer wall of a fortification. Just like early modern fortresses, computer networks require multi-layer protection against external attacks. Such network bastions are called bastion hosts, and they form part of a network perimeter.

A bastion host is a virtual machine with a public IP address assigned to it to enable SSH access. With a bastion host configured, you get sort of a [jump server](https://en.wikipedia.org/wiki/Jump_server) allowing you to establish secure connections to virtual machines that have no public IP addresses. In this guide, you will learn how to deploy a bastion host and secure your access to remote virtual machines residing inside your virtual private cloud (VPC).

A bastion host will help you make your VPC servers less vulnerable. Administration of specific servers will be carried out within a proxy connection via a bastion host over SSH.

To create a bastion host:

1. [Prepare your cloud](#before-you-begin).
1. [Create an SSH key pair](#create-ssh-keys).
1. [Create networks](#create-networks).
1. [Create security groups](#create-sg).
1. [Reserve a static public IP address](#get-static-ip).
1. [Create a virtual machine for your bastion host](#create-bastion-instance).
1. [Test your bastion host](#test-bastion).
1. [Add a virtual server to your bastion host's internal segment](#add-virtual-server).
1. [Connect to the VM you created](#connect-to-instance).

If you no longer need the resources you created, [delete them](#clear-out).


![](../../_assets/tutorials/bastion-yc.svg)



## Getting started {#before-you-begin}

{% include [before-you-begin](../../_tutorials/_tutorials_includes/before-you-begin.md) %}

### Required paid resources {#paid-resources}

The infrastructure support cost includes:

* Fee for disks and continuously running VMs (see [{{ compute-full-name }} pricing](../../compute/pricing.md)).
* Fee for using an external IP address (see [{{ vpc-full-name }} pricing](../../vpc/pricing.md#prices-public-ip)).

## Create an SSH key pair {#create-ssh-keys}

{% include [vm-ssh-rsa-key](../../_includes/vm-ssh-rsa-key.md) %}

{% note warning %}

Save the private key in a secure location, as you will not be able to connect to the VM without it.

{% endnote %}

## Create an external network and an internal one {#create-networks}

### Create an external network and subnet {#create-external-network}

{% list tabs group=instructions %}

- Management console {#console}

   1. In the [management console]({{ link-console-main }}), go to the folder where you need to create a cloud network.
   1. In the list of services, select **{{ ui-key.yacloud.iam.folder.dashboard.label_vpc }}**.
   1. In the top-right corner, click **{{ ui-key.yacloud.vpc.networks.button_create }}**.
   1. Enter the network name: `external-bastion-network`.
   1. Click **{{ ui-key.yacloud.vpc.networks.button_create }}**.
   1. Create a subnet:

      1. At the top right, click **{{ ui-key.yacloud.vpc.network.overview.button_create_subnetwork }}**.
      1. Specify the subnet parameters:

         1. **{{ ui-key.yacloud.vpc.subnetworks.create.field_name }}**: `bastion-external-segment`
         1. **{{ ui-key.yacloud.vpc.subnetworks.create.field_zone }}**: `{{ region-id }}-b`
         1. **{{ ui-key.yacloud.vpc.subnetworks.create.field_ip }}**: `172.16.17.0/28`

      1. Click **{{ ui-key.yacloud.vpc.subnetworks.create.button_create }}**.

{% endlist %}

### Create an internal network and subnet {#create-internal-network}

{% list tabs group=instructions %}

- Management console {#console}

   1. In the list of services, select **{{ ui-key.yacloud.iam.folder.dashboard.label_vpc }}**.
   1. In the top-right corner, click **{{ ui-key.yacloud.vpc.networks.button_create }}**.
   1. Enter the network name: `internal-bastion-network`.
   1. Click **{{ ui-key.yacloud.vpc.networks.button_create }}**.
   1. Create a subnet:

      1. At the top right, click **{{ ui-key.yacloud.vpc.network.overview.button_create_subnetwork }}**.
      1. Specify the subnet parameters:

         1. **{{ ui-key.yacloud.vpc.subnetworks.create.field_name }}**: `bastion-internal-segment`
         1. **{{ ui-key.yacloud.vpc.subnetworks.create.field_zone }}**: `{{ region-id }}-b`
         1. **{{ ui-key.yacloud.vpc.subnetworks.create.field_ip }}**: `172.16.16.0/24`

      1. Click **{{ ui-key.yacloud.vpc.subnetworks.create.button_create }}**.

{% endlist %}

## Create security groups {#create-sg}

### Create a security group for your bastion host {#create-internet-sg}

Create a [security group](../../vpc/concepts/security-groups.md) and configure the rules for the bastion host's inbound traffic for it to be accessible from the internet.

{% list tabs group=instructions %}

- Management console {#console}

   1. In the list of services, select **{{ ui-key.yacloud.iam.folder.dashboard.label_vpc }}** and the `external-bastion-network` network.
   1. In the left-hand panel, select ![image](../../_assets/vpc/security-group.svg) **{{ ui-key.yacloud.vpc.switch_security-groups }}**.
   1. Click **{{ ui-key.yacloud.vpc.network.security-groups.button_create }}**.
   1. Enter the security group name: `secure-bastion-sg`.
   1. Under **{{ ui-key.yacloud.vpc.network.security-groups.forms.label_section-rules }}**, go to the **{{ ui-key.yacloud.vpc.network.security-groups.label_ingress }}** tab and click **{{ ui-key.yacloud.vpc.network.security-groups.button_add-rule }}**.
   1. Specify the rule settings:

      * **{{ ui-key.yacloud.vpc.network.security-groups.forms.field_sg-rule-port-range }}**: `22`
      * **{{ ui-key.yacloud.vpc.network.security-groups.forms.field_sg-rule-protocol }}**: `TCP`
      * **{{ ui-key.yacloud.vpc.network.security-groups.forms.field_sg-rule-source }}**: `{{ ui-key.yacloud.vpc.network.security-groups.forms.value_sg-rule-destination-cidr }}`
      * **{{ ui-key.yacloud.vpc.network.security-groups.forms.field_sg-rule-cidr-blocks }}**: `0.0.0.0/0`

   1. Click **{{ ui-key.yacloud.common.save }}** in the rule creation window and then in the security group creation window.

{% endlist %}

### Create a security group for internal hosts {#create-internal-hosts-sg}

Create a security group and set up rules for incoming traffic from the bastion host to internal hosts:

{% list tabs group=instructions %}

- Management console {#console}

   1. In the list of services, select **{{ ui-key.yacloud.iam.folder.dashboard.label_vpc }}** and the `internal-bastion-network` network.
   1. In the left-hand panel, select ![image](../../_assets/vpc/security-group.svg) **{{ ui-key.yacloud.vpc.switch_security-groups }}**.
   1. Click **{{ ui-key.yacloud.vpc.network.security-groups.button_create }}**.
   1. Enter the security group name: `internal-bastion-sg`.
   1. Under **{{ ui-key.yacloud.vpc.network.security-groups.forms.label_section-rules }}**, go to the **{{ ui-key.yacloud.vpc.network.security-groups.label_ingress }}** tab and click **{{ ui-key.yacloud.vpc.network.security-groups.button_add-rule }}**.
   1. Specify the rule settings:

      * **{{ ui-key.yacloud.vpc.network.security-groups.forms.field_sg-rule-port-range }}**: `22`
      * **{{ ui-key.yacloud.vpc.network.security-groups.forms.field_sg-rule-protocol }}**: `TCP`
      * **{{ ui-key.yacloud.vpc.network.security-groups.forms.field_sg-rule-source }}**: `{{ ui-key.yacloud.vpc.network.security-groups.forms.value_sg-rule-destination-cidr }}`
      * **{{ ui-key.yacloud.vpc.network.security-groups.forms.field_sg-rule-cidr-blocks }}**: `172.16.16.254/32`

   1. Click **{{ ui-key.yacloud.common.save }}** in the rule creation window.
   1. Go to the **{{ ui-key.yacloud.vpc.network.security-groups.label_egress }}** tab and click **{{ ui-key.yacloud.vpc.network.security-groups.button_add-rule }}**.
   1. Specify the rule settings:

      * **{{ ui-key.yacloud.vpc.network.security-groups.forms.field_sg-rule-port-range }}**: `22`
      * **{{ ui-key.yacloud.vpc.network.security-groups.forms.field_sg-rule-protocol }}**: `TCP`
      * **{{ ui-key.yacloud.vpc.network.security-groups.forms.field_sg-rule-destination }}**: `{{ ui-key.yacloud.vpc.network.security-groups.forms.value_sg-rule-destination-sg }}`
      * **{{ ui-key.yacloud.vpc.network.security-groups.forms.field_sg-rule-sg-type }}**: `{{ ui-key.yacloud.vpc.network.security-groups.forms.value_sg-rule-sg-type-self }}`

   1. Click **{{ ui-key.yacloud.common.save }}** in the rule creation window and then in the security group creation window.

{% endlist %}

## Reserve a static public IP address {#get-static-ip}

The bastion host will need a static [public IP address](../../vpc/concepts/address.md#public-addresses) to run.

{% list tabs group=instructions %}

- Management console {#console}

   1. In the [management console]({{ link-console-main }}), go to the page of the folder where you want to reserve an IP address.
   1. In the list of services, select **{{ vpc-name }}**.
   1. In the left-hand panel, select ![image](../../_assets/vpc/ip-addresses.svg) **{{ ui-key.yacloud.vpc.switch_addresses }}**.
   1. Click **{{ ui-key.yacloud.vpc.addresses.button_create }}**.
   1. In the window that opens, select the `{{ region-id }}-b` [availability zone](../../overview/concepts/geo-scope.md).
   1. Click **{{ ui-key.yacloud.vpc.addresses.popup-create_button_create }}**.

{% endlist %}

## Create a VM for the bastion host {#create-bastion-instance}

After you created the subnet and security group, proceed to create a virtual server for the bastion host:

{% list tabs group=instructions %}

- Management console {#console}

   1. In the [management console]({{ link-console-main }}), select the folder to create the virtual machine in.
   1. In the list of services, select **{{ ui-key.yacloud.iam.folder.dashboard.label_compute }}**.
   1. At the top right, click **{{ ui-key.yacloud.compute.instances.button_create }}**.
   1. Enter the VM name: `bastion-host`.
   1. Select the `{{ region-id }}-b` availability zone.
   1. Under **{{ ui-key.yacloud.compute.instances.create.section_image }}**, go to the **{{ ui-key.yacloud.compute.instances.create.image_value_marketplace }}** tab and select the **NAT instance powered by Ubuntu 22.04 LTS** product.
   1. Under **{{ ui-key.yacloud.compute.instances.create.section_network }}**, configure the first network interface:

      * **{{ ui-key.yacloud.compute.groups.create.label_instance-net }}**: `bastion-external-segment`.
      * **{{ ui-key.yacloud.component.compute.network-select.field_external }}**: Select from the list the [IP address](#get-static-ip) reserved earlier.
      * **{{ ui-key.yacloud.component.compute.network-select.field_internal-ipv4 }}**: `{{ ui-key.yacloud.component.compute.network-select.switch_auto }}`.
      * **{{ ui-key.yacloud.component.compute.network-select.field_security-groups }}**: `secure-bastion-sg`.

   1. Click **{{ ui-key.yacloud.compute.instances.create.label_add-network-interface }}** and configure the second network interface:

      * **{{ ui-key.yacloud.compute.groups.create.label_instance-net }}**: `bastion-internal-segment`
      * **{{ ui-key.yacloud.component.compute.network-select.field_external }}**: `{{ ui-key.yacloud.component.compute.network-select.switch_none }}`
      * **{{ ui-key.yacloud.component.compute.network-select.field_internal-ipv4 }}**: `172.16.16.254`
      * **{{ ui-key.yacloud.component.compute.network-select.field_security-groups }}**: `internal-bastion-sg`

      {% note info %}

      Make sure the first interface on the new VM belongs to an external segment, since the default gateway is automatically specified on this interface.

      {% endnote %}

      Specify a public IP address for the external segment only. For the internal segment, specify an internal static IP address.

   1. Under **{{ ui-key.yacloud.compute.instances.create.section_access }}**, enter `bastion` for the username in the **{{ ui-key.yacloud.compute.instances.create.field_user }}** field.
   1. In the **{{ ui-key.yacloud.compute.instances.create.field_key }}** field, paste the contents of the [public key](../../compute/operations/vm-connect/ssh.md#creating-ssh-keys) file.
   1. Click **{{ ui-key.yacloud.compute.instances.create.button_create }}**.

   As soon as the server VM starts and gets the **Running** status, you will see the public IP address assigned to it in the **{{ ui-key.yacloud.compute.group.overview.label_instance-address }}** field.

{% endlist %}

## Test the bastion host {#test-bastion}

After you start your bastion host, try to connect to it via the SSH client:

```bash
ssh -i ~/.ssh/<private_key> bastion@<public_IP_address_of_the_bastion_host>
```

## Add a virtual server to the internal segment of the bastion host {#add-virtual-server}

To administer your servers, add a network interface to the internal network segment of the bastion host: `bastion-internal-segment`.

If you already have a virtual machine, add a new network interface to it. If not, create a new machine to test your bastion host configuration:

{% list tabs group=instructions %}

- Management console {#console}

   1. In the [management console]({{ link-console-main }}), select the folder to create the virtual machine in.
   1. In the list of services, select **{{ ui-key.yacloud.iam.folder.dashboard.label_compute }}**.
   1. At the top right, click **{{ ui-key.yacloud.compute.instances.button_create }}**.
   1. Enter the VM name: `test-vm`.
   1. Select the `{{ region-id }}-b` availability zone.
   1. Under **{{ ui-key.yacloud.compute.instances.create.section_image }}**, select an operating system.
   1. Under **{{ ui-key.yacloud.compute.instances.create.section_network }}**, configure a network interface:

      * **{{ ui-key.yacloud.compute.groups.create.label_instance-net }}**: `bastion-internal-segment`
      * **{{ ui-key.yacloud.component.compute.network-select.field_external }}**: `{{ ui-key.yacloud.component.compute.network-select.switch_none }}`
      * **{{ ui-key.yacloud.component.compute.network-select.field_internal-ipv4 }}**: `{{ ui-key.yacloud.component.compute.network-select.switch_auto }}`
      * **{{ ui-key.yacloud.component.compute.network-select.field_security-groups }}**: `internal-bastion-sg`

   1. Under **{{ ui-key.yacloud.compute.instances.create.section_access }}**, enter `test` for the username in the **{{ ui-key.yacloud.compute.instances.create.field_user }}** field.
   1. In the **{{ ui-key.yacloud.compute.instances.create.field_key }}** field, paste the contents of the [public key](../../compute/operations/vm-connect/ssh.md#creating-ssh-keys) file.
   1. Click **{{ ui-key.yacloud.compute.instances.create.button_create }}**.

{% endlist %}

## Connect to the created VM {#connect-to-instance}

If connecting to the VM over SSH via a private IP address, you will use your bastion host as a jump host.

To simplify and configure SSH access, add the `-J` (ProxyJump) parameter to the SSH command:

```bash
ssh -i ~/.ssh/id_rsa -J bastion@<public_IP_address_of_the_bastion_host> test@<private_IP_address_of_the_virtual_server>
```

The SSH client will automatically connect to the internal server.

You can use the `-J` flag in OpenSSH version 7.3 or higher. In earlier versions, `-J` is not available. In this case, the easiest and most secure way is to use standard I/O redirection (the `-W` flag) for connection forwarding through the bastion host, e.g.:

```bash
ssh -i ~/.ssh/id_rsa -o ProxyCommand="ssh -W %h:%p bastion@<public_IP_address_of_the_bastion_host>" test@<private_IP_address_of_the_virtual_server>
```

## Additional connection options {#more-options}

### Using an SSH agent for connections via the bastion host {#using-ssh-agent}

By default, server access is only set up for authentication using a public SSH key. We do not recommend storing keys directly on your bastion hosts, especially without a passphrase. Use an SSH agent instead. In this case, private SSH keys will only be stored on your computer and you will be able to safely use them for authentication on the next server.

To add a key to an authentication agent, use the `ssh-add` command. If the key is stored in the `~/.ssh/id_rsa` file, it is added automatically. You can also set a specific key to use by running the command below:

```bash
ssh-add <key_file_path>
```

macOS users can set up the `~/.ssh/config` file on their own. In this case, the keys can be uploaded to the agent with this command:

```bash
AddKeysToAgent yes
```

The following command used to connect to the bastion host allows you to perform agent forwarding and log in to the next server by providing the credentials from your local machine:

```bash
ssh -A bastion@<public_IP_address_of_the_bastion_host>
```

Windows users can use Pageant and upload their private key file to it. Next, to ensure agent forwarding, open the [PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/) configuration window and select **Connection** → **SSH** → **Authentication**.

### Access to services through SSH tunnels {#ssh-tunneling}

Sometimes, SSH access alone is not enough to complete your task. If that is the case, use SSH tunnels to easily connect to web applications and other services used to process inbound connections.

The main types of SSH tunnels are local, remote, and dynamic:

* A **local** tunnel is an open port in a local loopback interface, which connects to the `IP:port` address on your SSH server.

   For example, you can connect local port 8080 to the `web_server_IP_address:80` address accessible from your bastion host and then open `http://localhost:8080` in your browser:

   ```bash
   ssh bastion@<public_IP_address_of_the_bastion_host> -L 8080:<web_server_IP_address>:80
   ```

* A **remote** tunnel works in the direction opposite to that of the local tunnel by opening a local port for connection to a remote server.
* A **dynamic** tunnel provides a SOCKS proxy on a local port with connections established from a remote host. For example, you can set up a dynamic tunnel on port 1080 and then specify it as a SOCKS proxy in your browser. As a result, you will be able to connect to any resources that are accessible from your bastion host and reside in a private subnet.

   ```bash
   ssh bastion@<public_IP_address_of_the_bastion_host> -D 1080
   ```

These methods rely on simple replacement that often requires a VPN connection as well as a combination with ProxyJump or ProxyCommand connections.

Windows users can set up tunnels using PuTTY by selecting **Connection** → **SSH** → **Tunnels**.

For easy connections to Remote Desktop Services (RDS), i.e., Windows hosts running in a cloud, you can use port redirection (especially the local one) by establishing a tunnel connection to port 3389 and then connecting to the `localhost` via an RDS client. If the RDS client is already awaiting connection on the local machine, you can choose a different port as shown in the example below:

```bash
ssh bastion@<public_IP_address_of_the_bastion_host> -L 3390:<IP_address_of_the_Windows_host>:3389
```

### Transferring files {#file-transfers}

For the Linux clients and servers, you can configure [SCP](https://en.wikipedia.org/wiki/Secure_copy_protocol) for secure file transfer through the bastion host to internal hosts and back. Do this using the same ProxyCommand and ProxyJump options specified in the SSH command line. For example:

```bash
scp -o "ProxyJump bastion@<public_IP_address_of_the_bastion_host>" <file_name> bastion@<private_IP_address_of_the_virtual_server>:<file_path>
```

If you are a Windows client user, one of the most popular SCP applications for Windows is [WinSCP](https://winscp.net). To transfer files via your bastion host to a remote Linux machine:

1. Create a session to connect to a private host IP without a password. Set up an SSH key on the Linux machine.
1. In the left-hand navigation menu, click **Advanced** and select **Tunnel**.
1. Enter the IP address and username for your bastion host. In the **Private key file** field, select and set the private key file to use for authentication on your bastion host.
1. In the left-hand navigation menu, select **Authentication** under **SSH**.
1. Make sure to select **Allow agent forwarding**.
1. Choose the private key to use for authentication on a private host.

This configuration enables a direct file transfer between your Windows machine and Linux private host. The bastion host will secure connections between them.

If using Windows hosts residing behind a Linux bastion, you can transfer files using [RDP](https://en.wikipedia.org/wiki/Remote_Desktop_Protocol) via a tunnel. This method ensures an efficient and secure file transfer.

## How to delete the resources you created {#clear-out}

To stop paying for the resources you created:

* [Delete](../../compute/operations/vm-control/vm-delete.md) the VM.
* [Delete](../../vpc/operations/address-delete.md) the static public IP address.
