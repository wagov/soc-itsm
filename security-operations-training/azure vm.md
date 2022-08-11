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

    :::image type="content" source="media/quick-create-portal/instance-details.png" alt-text="Screenshot of the Instance details section where you provide a name for the virtual machine and select its region, image, and size.":::

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


### WAF Deployment
----

TODO

