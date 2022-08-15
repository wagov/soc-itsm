# Azure VM (Virtual Machines) Basic Training 

## Documentation

[Microsoft Documents](https://docs.microsoft.com/en-us/azure/virtual-machines/) - Main Documentation for learning more about Azure VMs

## Exercises

A program of excersise to assist with learning and training with Azure VMs

## 1. Basic Deployment
---

### **Prerequisite**

 - SSH Client installed - [windows](https://docs.microsoft.com/en-us/windows/terminal/tutorials/ssh) - [linux](https://phoenixnap.com/kb/ssh-to-connect-to-remote-server-linux-or-windows)
 - Firewall Rule allowance for SSH Protocol (port 22)


### **Portal Deployment**

 - [Microsoft Portal Exercise](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/quick-create-portal) - Default exercise for deploying VMs via Portal


## Sign in to Azure

Sign in to the [Azure portal](https://portal.azure.com).

## Create virtual machine

1. Enter *virtual machines* in the search.
1. Under **Services**, select **Virtual machines**.
1. In the **Virtual machines** page, select **Create** and then **Virtual machine**.  The **Create a virtual machine** page opens.

1. In the **Basics** tab, under **Project details**, make sure the correct subscription is selected and then choose to **Create new** resource group. Enter *myResourceGroup* for the name.*. 

	Image TODO


1. Under **Instance details**, enter *myVM* for the **Virtual machine name**, and choose *Ubuntu 20.04 LTS - Gen2* for your **Image**. Leave the other defaults. The default size and pricing is only shown as an example. Size availability and pricing are dependent on your region and subscription.

    Image TODO

    > [!NOTE]
    > Some users will now see the option to create VMs in multiple zones. To learn more about this new capability, see [Create virtual machines in an availability zone](../create-portal-availability-zone.md).


1. Under **Administrator account**, select **SSH public key**.

1. In **Username** enter *azureuser*.

1. For **SSH public key source**, leave the default of **Generate new key pair**, and then enter *myKey* for the **Key pair name**.

    Image TODO

1. Under **Inbound port rules** > **Public inbound ports**, choose **Allow selected ports** and then select **SSH (22)** and **HTTP (80)** from the drop-down. 

	Image TODO

1. Leave the remaining defaults and then select the **Review + create** button at the bottom of the page.

1. On the **Create a virtual machine** page, you can see the details about the VM you are about to create. When you are ready, select **Create**.

1. When the **Generate new key pair** window opens, select **Download private key and create resource**. Your key file will be download as **myKey.pem**. Make sure you know where the `.pem` file was downloaded; you will need the path to it in the next step.

1. When the deployment is finished, select **Go to resource**.

1. On the page for your new VM, select the public IP address and copy it to your clipboard.


	Image TODO


## Connect to virtual machine

Create an SSH connection with the VM.

1. If you are on a Mac or Linux machine, open a Bash prompt and set read-only permission on the .pem file using `chmod 400 ~/Downloads/myKey.pem`. If you are on a Windows machine, open a PowerShell prompt. 

1. At your prompt, open an SSH connection to your virtual machine. Replace the IP address with the one from your VM, and replace the path to the `.pem` with the path to where the key file was downloaded.

```console
ssh -i ~/Downloads/myKey.pem azureuser@<Your Public IP Address>
```

> [!TIP]
> The SSH key you created can be used the next time your create a VM in Azure. Just select the **Use a key stored in Azure** for **SSH public key source** the next time you create a VM. You already have the private key on your computer, so you won't need to download anything.

## Install Docker

Docker is an open source containerization platform. It enables developers to package applications into containers—standardized executable components combining application source code with the operating system (OS) libraries and dependencies required to run that code in any environment. Containers simplify delivery of distributed applications, and have become increasingly popular as organizations shift to cloud-native development and hybrid multicloud environments.

Further information reagrding [docker](https://docs.docker.com/get-started/overview/)

```console
sudo apt-get -y update
curl -fsSL https://get.docker.com -o get-docker.sh
sh get-docker.sh
```
## Install Web Application

Caddy simplifies your infrastructure. It takes care of TLS certificate renewals, OCSP stapling, static file serving, reverse proxying, Kubernetes ingress, and more.

Further information reagrding [caddy](https://caddyserver.com/docs/)

```console
echo "hello world - My name is <first name>" > index.html
docker run -d -p 80:80 \
    -v $PWD/index.html:/usr/share/caddy/index.html \
    -v caddy_data:/data \
    caddy
```

## 2. Security Hardening
---

## 2.1 Basic

<<<<<<< HEAD
## WAF Deployment
=======
Basic security hardening is accepatable for testing but is not recommended alone for extensive development and production workloads

## Network Security Group

### Whitelist/Allow IP

## 2.1 Moderate

## WAF

### Create an application gateway

1. Select **Create a resource** on the left menu of the Azure portal. The **New** window appears.

2. Select **Networking** and then select **Application Gateway** in the **Featured** list.

### Basics tab

1. On the **Basics** tab, enter these values for the following application gateway settings:

   - **Resource group**: Select **myResourceGroupAG** for the resource group. If it doesn't exist, select **Create new** to create it.
   - **Application gateway name**: Enter *myAppGateway* for the name of the application gateway.
   - **Tier**: select **WAF V2**.
   - **WAF Policy**: Select **Create new**, type a name for the new policy, and then select **OK**.
     This creates a basic WAF policy with a managed Core Rule Set (CRS).

     :::image type="content" source="../media/application-gateway-web-application-firewall-portal/application-gateway-create-basics.png" alt-text="Screenshot of Create new application gateway: Basics tab." lightbox="../media/application-gateway-web-application-firewall-portal/application-gateway-create-basics.png":::

2.  For Azure to communicate between the resources that you create, it needs a virtual network. You can either create a new virtual network or use an existing one. In this example, you'll create a new virtual network at the same time that you create the application gateway. Application Gateway instances are created in separate subnets. You create two subnets in this example: one for the application gateway, and another for the backend servers.

    Under **Configure virtual network**,  select **Create new** to create a new virtual network. In the **Create virtual network** window that opens, enter the following values to create the virtual network and two subnets:

    - **Name**: Enter *myVNet* for the name of the virtual network.

    - **Subnet name** (Application Gateway subnet): The **Subnets** grid will show a subnet named *Default*. Change the name of this subnet to *myAGSubnet*.<br>The application gateway subnet can contain only application gateways. No other resources are allowed.

    - **Subnet name** (backend server subnet): In the second row of the **Subnets** grid, enter *myBackendSubnet* in the **Subnet name** column.

    - **Address range** (backend server subnet): In the second row of the **Subnets** Grid, enter an address range that doesn't overlap with the address range of *myAGSubnet*. For example, if the address range of *myAGSubnet* is 10.21.0.0/24, enter *10.21.1.0/24* for the address range of *myBackendSubnet*.

    Select **OK** to close the **Create virtual network** window and save the virtual network settings.

    Image TODO
    
3. On the **Basics** tab, accept the default values for the other settings and then select **Next: Frontends**.

## Firewall

Deploy the firewall into the VNet.

1. On the Azure portal menu or from the **Home** page, select **Create a resource**.
2. Type **firewall** in the search box and press **Enter**.
3. Select **Firewall** and then select **Create**.
4. On the **Create a Firewall** page, use the following table to configure the firewall:

   |Setting  |Value  |
   |---------|---------|
   |Subscription     |\<your subscription\>|
   |Resource group     |**Test-FW-RG** |
   |Name     |**Test-FW01**|
   |Region     |Select the same location that you used previously|
   |Firewall tier|**Standard**|
   |Firewall management|**Use Firewall rules (classic) to manage this firewall**|
   |Choose a virtual network     |**Use existing**: **Test-FW-VN**|
   |Public IP address     |**Add new**<br>**Name**:  **fw-pip**|

5. Accept the other default values, then select **Review + create**.
6. Review the summary, and then select **Create** to create the firewall.

   This will take a few minutes to deploy.
7. After deployment completes, go to the **Test-FW-RG** resource group, and select the **Test-FW01** firewall.
8. Note the firewall private and public IP addresses. You'll use these addresses later.

## NSG Logging

## 2.2 Advanced

### Network Watcher

## DDOS Protection

### Create a DDoS protection plan

1. Select **Create a resource** in the upper left corner of the Azure portal.
1. Search the term *DDoS*. When **DDoS protection plan** appears in the search results, select it.
1. Select **Create**.
1. Enter or select the following values.

    |Setting        |Value                                              |
    |---------      |---------                                          |
    |Subscription   | Select your subscription.                         |
    |Resource group | Enter exisitng **MyResourceGroup**.|
    |Name           | Enter **MyDdosProtectionPlan**.                     |
    |Region         | Enter **Australia East**.                                  |

1. Select **Review + create** then **Create**

### Enable DDoS protection for an existing virtual network

1. Create a DDoS protection plan by completing the steps in [Create a DDoS protection plan](#create-a-ddos-protection-plan), if you don't have an existing DDoS protection plan.
1. Enter the name of the virtual network that you want to enable DDoS Protection Standard for in the **Search resources, services, and docs box** at the top of the Azure portal. When the name of the virtual network appears in the search results, select it.
1. Select **DDoS protection**, under **Settings**.
1. Select **Enable**. Under **DDoS protection plan**, select an existing DDoS protection plan, or the plan you created in step 1, and then click **Save**. The plan you select can be in the same, or different subscription than the virtual network, but both subscriptions must be associated to the same Azure Active Directory tenant. 

### Just-In-Time Access Control (JIT)

For further information on [Just In Time Access](https://docs.microsoft.com/en-us/azure/defender-for-cloud/just-in-time-access-overview?tabs=defender-for-container-arch-aks)

### Enable JIT on your VMs from Azure virtual machines

You can enable JIT on a VM from the Azure virtual machines pages of the Azure portal.

![Configuring JIT VM access in Azure virtual machines.](./media/just-in-time-access-usage/jit-config-virtual-machines.gif)

> [!TIP]
> If a VM already has just-in-time enabled, when you go to its configuration page you'll see that just-in-time is enabled and you can use the link to open the just-in-time VM access page in Defender for Cloud, and view and change the settings.

1. From the [Azure portal](https://portal.azure.com), search for and select **Virtual machines**. 

1. Select the virtual machine you want to protect with JIT.

1. In the menu, select **Configuration**.

1. Under **Just-in-time access**, select **Enable just-in-time**. 

    This enables just-in-time access for the VM using the following default settings:

    - Windows machines:
        - RDP port 3389
        - Three hours of maximum allowed access
        - Allowed source IP addresses is set to Any
    - Linux machines:
        - SSH port 22
        - Three hours of maximum allowed access
        - Allowed source IP addresses is set to Any

1. To edit any of these values, or add more ports to your JIT configuration, use Microsoft Defender for Cloud's just-in-time page:

    1. From Defender for Cloud's menu, select **Just-in-time VM access**.

    1. From the **Configured** tab, right-click on the VM to which you want to add a port, and select edit. 

        Image TODO

    1. Under **JIT VM access configuration**, you can either edit the existing settings of an already protected port or add a new custom port.

    1. When you've finished editing the ports, select **Save**.

>>>>>>> 16afc1e4ce5ed0bd8507b3ca8a254127179a2af1
----

## WAF Deployment

