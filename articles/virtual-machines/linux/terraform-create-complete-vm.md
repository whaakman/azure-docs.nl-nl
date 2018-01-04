---
title: Terraform gebruiken voor het maken van een volledige Linux VM in Azure | Microsoft Docs
description: Informatie over het gebruik van Terraform maken en beheren van een omgeving met volledige Linux virtuele machines in Azure
services: virtual-machines-linux
documentationcenter: virtual-machines
author: echuvyrov
manager: timlt
editor: na
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/14/2017
ms.author: echuvyrov
ms.openlocfilehash: 13390c2db203332433e7e3c39c8d9ed5f688448c
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2018
---
# <a name="create-a-complete-linux-virtual-machine-infrastructure-in-azure-with-terraform"></a>Maken van een volledige infrastructuur voor Linux-virtuele machine in Azure met Terraform

Terraform kunt u definiëren en volledige infrastructuur implementaties maken in Azure. U bouwen Terraform sjablonen in een leesbare indeling maken en configureren van Azure-resources in een consistente, reproduceerbare manier. In dit artikel leest u hoe een volledige Linux-omgeving en de ondersteunende resources met Terraform maken. U kunt ook meer te weten hoe [installeren en configureren van Terraform](terraform-install-configure.md).


## <a name="create-azure-connection-and-resource-group"></a>Azure-verbinding en resourcegroep maken

We doorlopen die elke sectie van een sjabloon Terraform. U ziet ook de volledige versie van de [Terraform sjabloon](#complete-terraform-script) die u kunt kopiëren en plakken.

De `provider` gedeelte wordt uitgelegd Terraform gebruik van een Azure-provider. Ophalen van waarden voor *subscription_id*, *client_id*, *client_secret*, en *tenant_id*, Zie [installeren en Configureer Terraform](terraform-install-configure.md). 

> [!TIP]
> Als u omgevingsvariabelen voor de waarden maken of gebruikmaakt van de [ervaring Azure Cloud Shell Bash](/azure/cloud-shell/overview) , moet u niet de variabelendeclaraties opnemen in deze sectie.

```tf
provider "azurerm" {
    subscription_id = "xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    client_id       = "xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    client_secret   = "xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    tenant_id       = "xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
}
```

De volgende sectie maakt u een resourcegroep met de naam `myResourceGroup` in de `eastus` locatie:

```tf
resource "azurerm_resource_group" "myterraformgroup" {
    name     = "myResourceGroup"
    location = "East US"

    tags {
        environment = "Terraform Demo"
    }
}
```

In extra secties vindt u verwijzen naar de resourcegroep met *${azurerm_resource_group.myterraformgroup.name}*.

## <a name="create-virtual-network"></a>Virtueel netwerk maken
De volgende sectie maakt u een virtueel netwerk met de naam *myVnet* in de *10.0.0.0/16* adresruimte:

```tf
resource "azurerm_virtual_network" "myterraformnetwork" {
    name                = "myVnet"
    address_space       = ["10.0.0.0/16"]
    location            = "East US"
    resource_group_name = "${azurerm_resource_group.myterraformgroup.name}"

    tags {
        environment = "Terraform Demo"
    }
}
```
: De volgende sectie maakt u een subnet met de naam *mySubnet* in de *myVnet* virtueel netwerk

```tf
resource "azurerm_subnet" "myterraformsubnet" {
    name                 = "mySubnet"
    resource_group_name  = "${azurerm_resource_group.myterraformgroup.name}"
    virtual_network_name = "${azurerm_virtual_network.myterraformnetwork.name}"
    address_prefix       = "10.0.2.0/24"
}
```


## <a name="create-public-ip-address"></a>Openbare IP-adres maken
Toegang krijgen tot bronnen via het Internet, maken en een openbare IP-adres toewijzen aan uw virtuele machine. De volgende sectie maakt u een openbaar IP-adres met de naam *myPublicIP*:

```tf
resource "azurerm_public_ip" "myterraformpublicip" {
    name                         = "myPublicIP"
    location                     = "East US"
    resource_group_name          = "${azurerm_resource_group.myterraformgroup.name}"
    public_ip_address_allocation = "dynamic"

    tags {
        environment = "Terraform Demo"
    }
}
```


## <a name="create-network-security-group"></a>Netwerkbeveiligingsgroep maken
Netwerkbeveiligingsgroepen regelen de stroom van netwerkverkeer binnen en buiten uw virtuele machine. De volgende sectie maakt u een netwerkbeveiligingsgroep met de naam *myNetworkSecurityGroup* en definieert u een regel voor SSH-verkeer op TCP-poort 22:

```tf
resource "azurerm_network_security_group" "temyterraformpublicipnsg" {
    name                = "myNetworkSecurityGroup"
    location            = "East US"
    resource_group_name = "${azurerm_resource_group.myterraformgroup.name}"
    ;
    security_rule {
        name                       = "SSH"
        priority                   = 1001
        direction                  = "Inbound"
        access                     = "Allow"
        protocol                   = "Tcp"
        source_port_range          = "*"
        destination_port_range     = "22"
        source_address_prefix      = "*"
        destination_address_prefix = "*"
    }

    tags {
        environment = "Terraform Demo"
    }
}
```


## <a name="create-virtual-network-interface-card"></a>Maken van de virtuele netwerkinterfacekaart
Een virtuele netwerkinterfacekaart (NIC) verbindt uw virtuele machine met een opgegeven virtuele netwerk, het openbare IP-adres en de netwerkbeveiligingsgroep. De volgende sectie in een sjabloon Terraform maakt u een virtuele NIC met de naam *myNIC* verbonden met de virtuele netwerken bronnen die u hebt gemaakt:

```tf
resource "azurerm_network_interface" "myterraformnic" {
    name                = "myNIC"
    location            = "East US"
    resource_group_name = "${azurerm_resource_group.myterraformgroup.name}"

    ip_configuration {
        name                          = "myNicConfiguration"
        subnet_id                     = "${azurerm_subnet.myterraformsubnet.id}"
        private_ip_address_allocation = "dynamic"
        public_ip_address_id          = "${azurerm_public_ip.myterraformpublicip.id}"
    }

    tags {
        environment = "Terraform Demo"
    }
}
```


## <a name="create-storage-account-for-diagnostics"></a>Opslagaccount voor diagnostische gegevens maken
Voor het opslaan van diagnostische gegevens over opstarten voor een virtuele machine, moet u een opslagaccount. Deze diagnostische gegevens voor opstarten kunt u problemen kunt oplossen en de status bewaken van uw virtuele machine. Het opslagaccount dat u maakt is alleen voor het opslaan van de boot diagnostics-gegevens. Omdat elke storage-account een unieke naam hebben moet, genereert de volgende sectie willekeurige tekst:

```tf
resource "random_id" "randomId" {
    keepers = {
        # Generate a new ID only when a new resource group is defined
        resource_group = "${azurerm_resource_group.myterraformgroup.name}"
    }
    
    byte_length = 8
}
```

U kunt nu een opslagaccount maken. De volgende sectie maakt een opslagaccount met de naam op basis van de gegenereerde in de vorige stap willekeurige tekst:

```tf
resource "azurerm_storage_account" "mystorageaccount" {
    name                = "diag${random_id.randomId.hex}"
    resource_group_name = "${azurerm_resource_group.myterraformgroup.name}"
    location            = "East US"
    account_replication_type = "LRS"
    account_tier = "Standard"

    tags {
        environment = "Terraform Demo"
    }
}
```


## <a name="create-virtual-machine"></a>Virtuele machine maken

De laatste stap is het maken van een virtuele machine en gebruik van alle bronnen die zijn gemaakt. De volgende sectie maakt u een virtuele machine met de naam *myVM* en koppelt u de virtuele NIC met de naam *myNIC*. De meest recente *Ubuntu 16.04-LTS* installatiekopie wordt gebruikt en een gebruiker met de naam *azureuser* wordt gemaakt met wachtwoordverificatie uitgeschakeld.

 SSH-sleutel gegevens vindt u in de *ssh_keys* sectie. Geef een geldige openbare SSH-sleutel in de *key_data* veld.

```tf
resource "azurerm_virtual_machine" "myterraformvm" {
    name                  = "myVM"
    location              = "East US"
    resource_group_name   = "${azurerm_resource_group.myterraformgroup.name}"
    network_interface_ids = ["${azurerm_network_interface.myterraformnic.id}"]
    vm_size               = "Standard_DS1_v2"

    storage_os_disk {
        name              = "myOsDisk"
        caching           = "ReadWrite"
        create_option     = "FromImage"
        managed_disk_type = "Premium_LRS"
    }

    storage_image_reference {
        publisher = "Canonical"
        offer     = "UbuntuServer"
        sku       = "16.04.0-LTS"
        version   = "latest"
    }

    os_profile {
        computer_name  = "myvm"
        admin_username = "azureuser"
    }

    os_profile_linux_config {
        disable_password_authentication = true
        ssh_keys {
            path     = "/home/azureuser/.ssh/authorized_keys"
            key_data = "ssh-rsa AAAAB3Nz{snip}hwhqT9h"
        }
    }

    boot_diagnostics {
        enabled     = "true"
        storage_uri = "${azurerm_storage_account.mystorageaccount.primary_blob_endpoint}"
    }

    tags {
        environment = "Terraform Demo"
    }
}
```

## <a name="complete-terraform-script"></a>Volledige Terraform script

Als u wilt alle deze secties samen te voegen en Terraform in actie, maakt u een bestand met de naam *terraform_azure.tf* en plak de volgende inhoud:

```tf
variable "resourcename" {
  default = "myResourceGroup"
}

# Configure the Microsoft Azure Provider
provider "azurerm" {
    subscription_id = "xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    client_id       = "xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    client_secret   = "xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
    tenant_id       = "xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
}

# Create a resource group if it doesn’t exist
resource "azurerm_resource_group" "myterraformgroup" {
    name     = "myResourceGroup"
    location = "East US"

    tags {
        environment = "Terraform Demo"
    }
}

# Create virtual network
resource "azurerm_virtual_network" "myterraformnetwork" {
    name                = "myVnet"
    address_space       = ["10.0.0.0/16"]
    location            = "East US"
    resource_group_name = "${azurerm_resource_group.myterraformgroup.name}"

    tags {
        environment = "Terraform Demo"
    }
}

# Create subnet
resource "azurerm_subnet" "myterraformsubnet" {
    name                 = "mySubnet"
    resource_group_name  = "${azurerm_resource_group.myterraformgroup.name}"
    virtual_network_name = "${azurerm_virtual_network.myterraformnetwork.name}"
    address_prefix       = "10.0.1.0/24"
}

# Create public IPs
resource "azurerm_public_ip" "myterraformpublicip" {
    name                         = "myPublicIP"
    location                     = "East US"
    resource_group_name          = "${azurerm_resource_group.myterraformgroup.name}"
    public_ip_address_allocation = "dynamic"

    tags {
        environment = "Terraform Demo"
    }
}

# Create Network Security Group and rule
resource "azurerm_network_security_group" "myterraformnsg" {
    name                = "myNetworkSecurityGroup"
    location            = "East US"
    resource_group_name = "${azurerm_resource_group.myterraformgroup.name}"
    
    security_rule {
        name                       = "SSH"
        priority                   = 1001
        direction                  = "Inbound"
        access                     = "Allow"
        protocol                   = "Tcp"
        source_port_range          = "*"
        destination_port_range     = "22"
        source_address_prefix      = "*"
        destination_address_prefix = "*"
    }

    tags {
        environment = "Terraform Demo"
    }
}

# Create network interface
resource "azurerm_network_interface" "myterraformnic" {
    name                      = "myNIC"
    location                  = "East US"
    resource_group_name       = "${azurerm_resource_group.myterraformgroup.name}"
    network_security_group_id = "${azurerm_network_security_group.myterraformnsg.id}"

    ip_configuration {
        name                          = "myNicConfiguration"
        subnet_id                     = "${azurerm_subnet.myterraformsubnet.id}"
        private_ip_address_allocation = "dynamic"
        public_ip_address_id          = "${azurerm_public_ip.myterraformpublicip.id}"
    }

    tags {
        environment = "Terraform Demo"
    }
}

# Generate random text for a unique storage account name
resource "random_id" "randomId" {
    keepers = {
        # Generate a new ID only when a new resource group is defined
        resource_group = "${azurerm_resource_group.myterraformgroup.name}"
    }
    
    byte_length = 8
}

# Create storage account for boot diagnostics
resource "azurerm_storage_account" "mystorageaccount" {
    name                        = "diag${random_id.randomId.hex}"
    resource_group_name         = "${azurerm_resource_group.myterraformgroup.name}"
    location                    = "East US"
    account_tier                = "Standard"
    account_replication_type    = "LRS"

    tags {
        environment = "Terraform Demo"
    }
}

# Create virtual machine
resource "azurerm_virtual_machine" "myterraformvm" {
    name                  = "myVM"
    location              = "East US"
    resource_group_name   = "${azurerm_resource_group.myterraformgroup.name}"
    network_interface_ids = ["${azurerm_network_interface.myterraformnic.id}"]
    vm_size               = "Standard_DS1_v2"

    storage_os_disk {
        name              = "myOsDisk"
        caching           = "ReadWrite"
        create_option     = "FromImage"
        managed_disk_type = "Premium_LRS"
    }

    storage_image_reference {
        publisher = "Canonical"
        offer     = "UbuntuServer"
        sku       = "16.04.0-LTS"
        version   = "latest"
    }

    os_profile {
        computer_name  = "myvm"
        admin_username = "azureuser"
    }

    os_profile_linux_config {
        disable_password_authentication = true
        ssh_keys {
            path     = "/home/azureuser/.ssh/authorized_keys"
            key_data = "ssh-rsa AAAAB3Nz{snip}hwhqT9h"
        }
    }

    boot_diagnostics {
        enabled = "true"
        storage_uri = "${azurerm_storage_account.mystorageaccount.primary_blob_endpoint}"
    }

    tags {
        environment = "Terraform Demo"
    }
}
```


## <a name="build-and-deploy-the-infrastructure"></a>Bouwen en implementeren van de infrastructuur
Bij uw Terraform sjabloon hebt gemaakt, wordt de eerste stap Terraform initialiseren. Deze stap zorgt u ervoor dat Terraform alle vereisten voor het bouwen van uw sjabloon in Azure.

```bash
terraform init
```

De volgende stap is dat Terraform controleren en valideren van de sjabloon. Deze stap vergelijkt de gewenste bronnen voor informatie over de opgeslagen door Terraform en vervolgens voert de geplande uitvoering. Resources zijn *niet* gemaakt in Azure.

```bash
terraform plan
```

Nadat u de vorige opdracht uitvoert, ziet u ongeveer het volgende scherm:

```bash
Refreshing Terraform state in-memory prior to plan...
The refreshed state will be used to calculate this plan, but will not be
persisted to local or remote state storage.


...

Note: You didn’t specify an “-out” parameter to save this plan, so when
“apply” is called, Terraform can’t guarantee this is what will execute.
  + azurerm_resource_group.myterraform
      <snip>
  + azurerm_virtual_network.myterraformnetwork
      <snip>
  + azurerm_network_interface.myterraformnic
      <snip>
  + azurerm_network_security_group.myterraformnsg
      <snip>
  + azurerm_public_ip.myterraformpublicip
      <snip>
  + azurerm_subnet.myterraformsubnet
      <snip>
  + azurerm_virtual_machine.myterraformvm
      <snip>
Plan: 7 to add, 0 to change, 0 to destroy.
```

Als u klaar voor het bouwen van de infrastructuur van Azure terwijl alles er goed uitziet, kunt u de sjabloon in Terraform toepassen:

```bash
terraform apply
```

Zodra Terraform is voltooid, wordt uw VM-infrastructuur is gereed. Het openbare IP-adres van uw virtuele machine met [az vm weergeven](/cli/azure/vm#show):

```azurecli
az vm show --resource-group myResourceGroup --name myVM -d --query [publicIps] --o tsv
```

U kunt vervolgens SSH met uw virtuele machine:

```bash
ssh azureuser@<publicIps>
```

## <a name="next-steps"></a>Volgende stappen
U kunt de basisinfrastructuur in Azure hebt gemaakt met behulp van Terraform. Voor complexere scenario's, inclusief voorbeelden die gebruik load balancers en de virtuele machine worden geschaald sets, Zie talrijke [Terraform voorbeelden voor Azure](https://github.com/hashicorp/terraform/tree/master/examples). Zie voor een bijgewerkte lijst met ondersteunde providers voor Azure de [Terraform documentatie](https://www.terraform.io/docs/providers/azurerm/index.html).
