---
title: Een volledige Linux-VM in Azure maken met behulp van Terraform | Microsoft Docs
description: Informatie over het gebruik van Terraform maken en beheren van een volledige omgeving van Linux virtuele machine in Azure
services: virtual-machines-linux
documentationcenter: virtual-machines
author: echuvyrov
manager: jeconnoc
editor: na
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/14/2017
ms.author: echuvyrov
ms.openlocfilehash: cf0fad78613d063a0f1270597cf67eadd996124a
ms.sourcegitcommit: b7e5bbbabc21df9fe93b4c18cc825920a0ab6fab
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/27/2018
ms.locfileid: "47406904"
---
# <a name="create-a-complete-linux-virtual-machine-infrastructure-in-azure-with-terraform"></a>Een volledige Linux-VM-infrastructuur in Azure maken met Terraform

Terraform kunt u definiëren en implementaties van een volledige maken in Azure. U bouwen Terraform-sjablonen in een leesbare indeling die maken en configureren van Azure-resources in een consistente, reproduceerbare manier. Dit artikel ziet u hoe u een volledige Linux-omgeving en de ondersteunende resources met Terraform maakt. U leert ook hoe u [installeren en configureren van Terraform](terraform-install-configure.md).


## <a name="create-azure-connection-and-resource-group"></a>Maak verbinding met het Azure- en resourcegroep

We gaan via elke sectie van een sjabloon Terraform. U ziet ook de volledige versie van de [Terraform sjabloon](#complete-terraform-script) die u kunt kopiëren en plakken.

De `provider` gedeelte wordt uitgelegd Terraform om een Azure-provider te gebruiken. Om op te halen van de waarden voor *abonnements-id*, *client_id*, *client_secret*, en *tenant_id*, Zie [installeren en Terraform configureren](terraform-install-configure.md). 

> [!TIP]
> Als u omgevingsvariabelen voor de waarden te maken of gebruikmaakt van de [Azure Cloud Shell-Bash-ervaring](/azure/cloud-shell/overview) , moet u niet de variabelendeclaraties opnemen in deze sectie.

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
    location = "eastus"

    tags {
        environment = "Terraform Demo"
    }
}
```

In extra onderdelen, verwijst u naar de resourcegroep met *${azurerm_resource_group.myterraformgroup.name}*.

## <a name="create-virtual-network"></a>Virtueel netwerk maken
De volgende sectie maakt u een virtueel netwerk met de naam *myVnet* in de *10.0.0.0/16* adresruimte:

```tf
resource "azurerm_virtual_network" "myterraformnetwork" {
    name                = "myVnet"
    address_space       = ["10.0.0.0/16"]
    location            = "eastus"
    resource_group_name = "${azurerm_resource_group.myterraformgroup.name}"

    tags {
        environment = "Terraform Demo"
    }
}
```

De volgende sectie maakt u een subnet met de naam *mySubnet* in de *myVnet* virtueel netwerk:

```tf
resource "azurerm_subnet" "myterraformsubnet" {
    name                 = "mySubnet"
    resource_group_name  = "${azurerm_resource_group.myterraformgroup.name}"
    virtual_network_name = "${azurerm_virtual_network.myterraformnetwork.name}"
    address_prefix       = "10.0.2.0/24"
}
```


## <a name="create-public-ip-address"></a>Openbare IP-adres maken
Voor toegang tot resources via het Internet, maken en een openbaar IP-adres toewijzen aan uw virtuele machine. De volgende sectie maakt u een openbaar IP-adres met de naam *myPublicIP*:

```tf
resource "azurerm_public_ip" "myterraformpublicip" {
    name                         = "myPublicIP"
    location                     = "eastus"
    resource_group_name          = "${azurerm_resource_group.myterraformgroup.name}"
    public_ip_address_allocation = "dynamic"

    tags {
        environment = "Terraform Demo"
    }
}
```


## <a name="create-network-security-group"></a>Netwerkbeveiligingsgroep maken
Netwerkbeveiligingsgroepen beheren van de gegevensoverdracht van netwerkverkeer naar en uit uw virtuele machine. De volgende sectie maakt u een netwerkbeveiligingsgroep met de naam *myNetworkSecurityGroup* en definieert u een regel voor SSH-verkeer op TCP-poort 22:

```tf
resource "azurerm_network_security_group" "myterraformnsg" {
    name                = "myNetworkSecurityGroup"
    location            = "eastus"
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
```


## <a name="create-virtual-network-interface-card"></a>Maken van virtuele netwerkinterfacekaart
Een virtuele netwerkinterfacekaart (NIC) verbindt de virtuele machine met een bepaald virtueel netwerk, het openbare IP-adres en de netwerkbeveiligingsgroep. De volgende sectie in een Terraform-sjabloon maakt u een virtuele NIC met de naam *myNIC* verbonden met de virtuele netwerken bronnen die u hebt gemaakt:

```tf
resource "azurerm_network_interface" "myterraformnic" {
    name                = "myNIC"
    location            = "eastus"
    resource_group_name = "${azurerm_resource_group.myterraformgroup.name}"
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
```


## <a name="create-storage-account-for-diagnostics"></a>Opslagaccount voor diagnostische gegevens maken
Voor het opslaan van diagnostische gegevens over opstarten voor een virtuele machine, moet u een opslagaccount. De diagnostische gegevens over deze opstarten kunt u problemen kunt oplossen en de status van uw virtuele machine controleren. Het opslagaccount dat u maakt, is alleen voor het opslaan van de gegevens van de diagnostische gegevens over opstarten. Als elk opslagaccount een unieke naam hebben moet, genereert de volgende sectie willekeurige tekst:

```tf
resource "random_id" "randomId" {
    keepers = {
        # Generate a new ID only when a new resource group is defined
        resource_group = "${azurerm_resource_group.myterraformgroup.name}"
    }
    
    byte_length = 8
}
```

U kunt nu een storage-account maken. De volgende sectie maakt u een storage-account met de naam op basis van de willekeurige tekst die in de vorige stap is gegenereerd:

```tf
resource "azurerm_storage_account" "mystorageaccount" {
    name                = "diag${random_id.randomId.hex}"
    resource_group_name = "${azurerm_resource_group.myterraformgroup.name}"
    location            = "eastus"
    account_replication_type = "LRS"
    account_tier = "Standard"

    tags {
        environment = "Terraform Demo"
    }
}
```


## <a name="create-virtual-machine"></a>Virtuele machine maken

De laatste stap is het maken van een virtuele machine en gebruiken van alle resources die zijn gemaakt. De volgende sectie maakt u een virtuele machine met de naam *myVM* en koppelt u de virtuele NIC met de naam *myNIC*. De meest recente *Ubuntu 16.04-LTS* installatiekopie wordt gebruikt en een gebruiker met de naam *azureuser* wordt gemaakt met wachtwoordverificatie uitgeschakeld.

 SSH-sleutel gegevens is opgegeven in de *ssh_keys* sectie. Geef een geldige openbare SSH-sleutel in de *key_data* veld.

```tf
resource "azurerm_virtual_machine" "myterraformvm" {
    name                  = "myVM"
    location              = "eastus"
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

## <a name="complete-terraform-script"></a>Volledige Terraform-script

Als u wilt samenbrengen alle deze secties en Terraform in actie zien, maakt u een bestand met de naam *terraform_azure.tf* en plak de volgende inhoud:

```tf
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
    location = "eastus"

    tags {
        environment = "Terraform Demo"
    }
}

# Create virtual network
resource "azurerm_virtual_network" "myterraformnetwork" {
    name                = "myVnet"
    address_space       = ["10.0.0.0/16"]
    location            = "eastus"
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
    location                     = "eastus"
    resource_group_name          = "${azurerm_resource_group.myterraformgroup.name}"
    public_ip_address_allocation = "dynamic"

    tags {
        environment = "Terraform Demo"
    }
}

# Create Network Security Group and rule
resource "azurerm_network_security_group" "myterraformnsg" {
    name                = "myNetworkSecurityGroup"
    location            = "eastus"
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
    location                  = "eastus"
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
    location                    = "eastus"
    account_tier                = "Standard"
    account_replication_type    = "LRS"

    tags {
        environment = "Terraform Demo"
    }
}

# Create virtual machine
resource "azurerm_virtual_machine" "myterraformvm" {
    name                  = "myVM"
    location              = "eastus"
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
Met de Terraform-sjabloon is gemaakt, wordt de eerste stap is om te initialiseren van Terraform. Deze stap zorgt u ervoor dat Terraform alle vereisten voor het bouwen van uw sjabloon in Azure.

```bash
terraform init
```

De volgende stap is dat Terraform controleren en valideren van de sjabloon. Deze stap vergelijkt de aangevraagde bronnen om de informatie over de status opgeslagen met Terraform en vervolgens de geplande uitvoering weergeeft. Resources zijn *niet* gemaakt in Azure.

```bash
terraform plan
```

Nadat u de vorige opdracht hebt uitgevoerd, ziet u ongeveer het volgende scherm:

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

Als alles er goed uitziet en u gereed voor het bouwen van de infrastructuur in Azure bent, moet u de sjabloon in Terraform toepassen:

```bash
terraform apply
```

Nadat Terraform is voltooid, wordt uw VM-infrastructuur gereed is. Het openbare IP-adres van uw virtuele machine met [az vm show](/cli/azure/vm#az_vm_show):

```azurecli
az vm show --resource-group myResourceGroup --name myVM -d --query [publicIps] --o tsv
```

U kunt vervolgens SSH met uw virtuele machine:

```bash
ssh azureuser@<publicIps>
```

## <a name="next-steps"></a>Volgende stappen
U kunt de basisinfrastructuur in Azure hebt gemaakt met behulp van Terraform. Voor complexere scenario's, inclusief voorbeelden die kunnen worden geschaald gebruik load balancers en virtuele machine sets, Zie talrijke [Terraform-voorbeelden voor Azure](https://github.com/hashicorp/terraform/tree/master/examples). Zie voor een bijgewerkte lijst van ondersteunde Azure-providers, de [Terraform documentatie](https://www.terraform.io/docs/providers/azurerm/index.html).
