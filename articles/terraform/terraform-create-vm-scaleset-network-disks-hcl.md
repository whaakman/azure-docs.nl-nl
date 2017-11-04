---
title: Gebruik van Terraform en maak een Azure VM-scale ingesteld met de lijst met compatibele hardware.
description: Gebruik Terraform configureren en versie van de schaal van een virtuele machine van Azure instellen op voltooid met een virtueel netwerk en beheerd gekoppelde schijven.
keywords: terraform, devops, schalen ingesteld, virtuele machine, netwerk, opslag, modules
ms.service: virtual-machines-linux
author: dcaro
ms.author: dcaro
ms.date: 10/04/2017
ms.topic: article
ms.openlocfilehash: 7a4e21d547b3d2b2399f9f68b1babd9f82a421b7
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2017
---
# <a name="use-terraform-to-plan-and-create-a-networked-azure-vm-scale-set-with-managed-storage"></a>Terraform gebruiken om te plannen en maken van een netwerk Azure VM schaal beheerde opslaggroep in te stellen

In dit artikel leert u [Terraform](https://www.terraform.io/) maakt en implementeert een [scaleset van de virtuele machine van Azure](/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-overview) met beheerde schijven met behulp van de [Hashicorp configuratie taal](https://www.terraform.io/docs/configuration/syntax.html) (lijst met compatibele hardware).  

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Instellen van uw implementatie Terraform
> * Gebruik van variabelen en uitvoer voor de implementatie van Terraform 
> * Maken en implementeren van de netwerkinfrastructuur
> * Maken en implementeren van een virtuele-machineschaalset en koppelt u dit aan het netwerk
> * Maken en implementeren van een jumpbox verbinding maken met de virtuele machines via SSH

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="before-you-begin"></a>Voordat u begint

- [Terraform installeren en configureren van toegang tot Azure](/azure/virtual-machines/linux/terraform-install-configure)
- [Maken van een SSH-sleutelpaar](/azure/virtual-machines/linux/mac-create-ssh-keys) als u er nog geen hebt.

## <a name="create-the-file-structure"></a>De bestandsstructuur maken

Maak drie nieuwe bestanden in een lege map met de volgende namen:

- `variables.tf`Dit bestand bevat de waarden van variabelen die in de sjabloon.
- `output.tf`Dit bestand beschrijft de instellingen die worden weergegeven na de implementatie.
- `vmss.tf`Deze code voor de virtuele-machineschaalset instellen infrastructuur.

## <a name="create-the-variables-and-output-definitions"></a>Maak de variabelen en definities uitvoer

In deze stap definieert u de variabelen die de resources die zijn gemaakt door Terraform aanpassen.

Bewerk de `variables.tf` bestand, Kopieer de volgende code en sla de wijzigingen.

```tf 
variable "location" {
  description = "The location where resources will be created"
  default     = "West US"
}

variable "resource_group_name" {
  description = "The name of the resource group in which the resources will be created"
  default     = "myResourceGroup"
}
```

Bewerk de `output.tf` -bestand en kopieer de volgende code om de volledig gekwalificeerde domeinnaam voor de virtuele machines weer te geven. 

```hcl 
output "vmss_public_ip" {
    value = "${azurerm_public_ip.vmss.fqdn}"
}
```

## <a name="define-the-network-infrastructure-in-a-template"></a>Definieer de netwerkinfrastructuur in een sjabloon

In deze stap maakt maken u de volgende netwerkinfrastructuur in een nieuwe Azure-resourcegroep: 

  - Een VNET met de adresruimte van 10.0.0.0/16 
  - Een subnet met de adresruimte van 10.0.2.0/24
  - Twee openbare IP-adressen. Een door de virtuele machine scale set load balancer, de andere verbinding maken met de SSH-jumpbox gebruikt.


Bewerken en kopieer de volgende code in de `vmss.tf` bestand: 

```tf
resource "azurerm_resource_group" "vmss" {
  name     = "${var.resource_group_name}"
  location = "${var.location}"

  tags {
    environment = "codelab"
  }
}

resource "azurerm_virtual_network" "vmss" {
  name                = "vmss-vnet"
  address_space       = ["10.0.0.0/16"]
  location            = "${var.location}"
  resource_group_name = "${azurerm_resource_group.vmss.name}"

  tags {
    environment = "codelab"
  }
}

resource "azurerm_subnet" "vmss" {
  name                 = "vmss-subnet"
  resource_group_name  = "${azurerm_resource_group.vmss.name}"
  virtual_network_name = "${azurerm_virtual_network.vmss.name}"
  address_prefix       = "10.0.2.0/24"
}

resource "azurerm_public_ip" "vmss" {
  name                         = "vmss-public-ip"
  location                     = "${var.location}"
  resource_group_name          = "${azurerm_resource_group.vmss.name}"
  public_ip_address_allocation = "static"
  domain_name_label            = "${azurerm_resource_group.vmss.name}"

  tags {
    environment = "codelab"
  }
}

``` 

> [!NOTE]
> Er is een goed idee om de resources die worden geïmplementeerd in Azure vergemakkelijken de identificatie in de toekomst taggen.

## <a name="create-the-network-infrastructure"></a>Maken van de netwerkinfrastructuur

Initialiseren van de omgeving Terraform door met de volgende opdracht in de map waar u hebt gemaakt de `.tf` bestanden:

```bash
terraform init 
```

Voer de volgende opdracht om de infrastructuur in Azure te implementeren.

```bash
terraform apply
```

Controleer of de FQDN-naam van het openbare IP-adres overeenkomt met de configuratie: ![VMSS terraform FQDN voor openbare IP-adres](./media/tf-create-vmss-step4-fqdn.png)


De resourcegroep moet de volgende bronnen: ![VMSS terraform netwerkbronnen](./media/tf-create-vmss-step4-rg.png)


## <a name="edit-the-infrastructure-to-add-the-virtual-machine-scale-set"></a>De infrastructuur voor het toevoegen van de virtuele-machineschaalset bewerken

In deze stap kunt u de volgende bronnen toevoegen aan de sjabloon:

- Een Azure load balancer en regels voor het leveren van de toepassing en deze te koppelen aan het openbare IP-adres eerder hebt geconfigureerd.
- Azure back-end-adresgroep en deze toewijzen aan de Load Balancer 
- Een health testpoort gebruikt door de toepassing en geconfigureerd op de Load Balancer 
- Een virtuele-machineschaalset zitten achter de load balancer, uitgevoerd op het vnet eerder hebt geïmplementeerd
- [Nginx](http://nginx.org/) op de knooppunten van de schaal van de virtuele machine met behulp van een extensie voor aangepaste scripts.

Voeg de volgende code toe aan het einde van de `vmss.tf` bestand.

```tf
resource "azurerm_lb" "vmss" {
  name                = "vmss-lb"
  location            = "${var.location}"
  resource_group_name = "${azurerm_resource_group.vmss.name}"

  frontend_ip_configuration {
    name                 = "PublicIPAddress"
    public_ip_address_id = "${azurerm_public_ip.vmss.id}"
  }

  tags {
    environment = "codelab"
  }
}

resource "azurerm_lb_backend_address_pool" "bpepool" {
  resource_group_name = "${azurerm_resource_group.vmss.name}"
  loadbalancer_id     = "${azurerm_lb.vmss.id}"
  name                = "BackEndAddressPool"
}

resource "azurerm_lb_probe" "vmss" {
  resource_group_name = "${azurerm_resource_group.vmss.name}"
  loadbalancer_id     = "${azurerm_lb.vmss.id}"
  name                = "ssh-running-probe"
  port                = "${var.application_port}"
}

resource "azurerm_lb_rule" "lbnatrule" {
    resource_group_name            = "${azurerm_resource_group.vmss.name}"
    loadbalancer_id                = "${azurerm_lb.vmss.id}"
    name                           = "http"
    protocol                       = "Tcp"
    frontend_port                  = "${var.application_port}"
    backend_port                   = "${var.application_port}"
    backend_address_pool_id        = "${azurerm_lb_backend_address_pool.bpepool.id}"
    frontend_ip_configuration_name = "PublicIPAddress"
    probe_id                       = "${azurerm_lb_probe.vmss.id}"
}

resource "azurerm_virtual_machine_scale_set" "vmss" {
  name                = "vmscaleset"
  location            = "${var.location}"
  resource_group_name = "${azurerm_resource_group.vmss.name}"
  upgrade_policy_mode = "Manual"

  sku {
    name     = "Standard_DS1_v2"
    tier     = "Standard"
    capacity = 2
  }

  storage_profile_image_reference {
    publisher = "Canonical"
    offer     = "UbuntuServer"
    sku       = "16.04-LTS"
    version   = "latest"
  }

  storage_profile_os_disk {
    name              = ""
    caching           = "ReadWrite"
    create_option     = "FromImage"
    managed_disk_type = "Standard_LRS"
  }

  storage_profile_data_disk {
    lun          = 0
    caching        = "ReadWrite"
    create_option  = "Empty"
    disk_size_gb   = 10
  }

  os_profile {
    computer_name_prefix = "vmlab"
    admin_username       = "azureuser"
    admin_password       = "Passwword1234"
  }

  os_profile_linux_config {
    disable_password_authentication = true

    ssh_keys {
      path     = "/home/azureuser/.ssh/authorized_keys"
      key_data = "${file("~/.ssh/id_rsa.pub")}"
    }
  }

  network_profile {
    name    = "terraformnetworkprofile"
    primary = true

    ip_configuration {
      name                                   = "IPConfiguration"
      subnet_id                              = "${azurerm_subnet.vmss.id}"
      load_balancer_backend_address_pool_ids = ["${azurerm_lb_backend_address_pool.bpepool.id}"]
    }
  }

  extension { 
    name = "vmssextension"
    publisher = "Microsoft.OSTCExtensions"
    type = "CustomScriptForLinux"
    type_handler_version = "1.2"
    settings = <<SETTINGS
    {
        "commandToExecute": "sudo apt-get -y install nginx"
    }
    SETTINGS
  }

  tags {
    environment = "codelab"
  }
}
```

De implementatie aanpassen door de volgende code toe te voegen `variables.tf`:

```tf 
variable "application_port" {
    description = "The port that you want to expose to the external load balancer"
    default     = 80
}

variable "admin_password" {
    description = "Default password for admin"
    default = "Passwwoord11223344"
}
``` 


## <a name="deploy-the-virtual-machine-scale-set-in-azure"></a>De virtuele-machineschaalset instellen in Azure implementeren

Voer de volgende opdracht voor het visualiseren van de virtuele machine scale set implementatie:

```bash
terraform plan
```

De uitvoer van de opdracht ziet er als volgt.
![Terraform vmss abonnement toevoegen](./media/tf-create-vmss-step6-plan884d3aefd9708a711bc09a66e85eb149c23a3ccff959655ec00418168b2bd481.png)

Vervolgens implementeert u de aanvullende resources in Azure: 

```bash
terraform apply 
```

De inhoud van de resourcegroep moet eruitzien als:

![Terraform vm scaleset resourcegroep](./media/tf-create-vmss-step6-apply.png)

Open een browser en maak verbinding met de FQDN-naam die is geretourneerd door de opdracht. 

## <a name="add-an-ssh-jumpbox-to-the-existing-network"></a>Een SSH-jumpbox toevoegen aan de bestaande netwerk 

In deze stap configureert u de volgende bronnen:
- Een netwerkinterface is verbonden met hetzelfde subnet als de virtuele-machineschaalset.
- Een virtuele machine verbonden met deze netwerkinterface. Deze 'jumpbox' is extern toegankelijk. Eenmaal zijn verbonden, kunt u SSH met een van de virtuele machines in de schaalset.



Voeg de volgende code toe aan het einde van de `vmss.tf` bestand:

```hcl 
resource "azurerm_public_ip" "jumpbox" {
  name                         = "jumpbox-public-ip"
  location                     = "${var.location}"
  resource_group_name          = "${azurerm_resource_group.vmss.name}"
  public_ip_address_allocation = "static"
  domain_name_label            = "${azurerm_resource_group.vmss.name}-ssh"

  tags {
    environment = "codelab"
  }
}

resource "azurerm_network_interface" "jumpbox" {
  name                = "jumpbox-nic"
  location            = "${var.location}"
  resource_group_name = "${azurerm_resource_group.vmss.name}"

  ip_configuration {
    name                          = "IPConfiguration"
    subnet_id                     = "${azurerm_subnet.vmss.id}"
    private_ip_address_allocation = "dynamic"
    public_ip_address_id          = "${azurerm_public_ip.jumpbox.id}"
  }

  tags {
    environment = "codelab"
  }
}

resource "azurerm_virtual_machine" "jumpbox" {
  name                  = "jumpbox"
  location              = "${var.location}"
  resource_group_name   = "${azurerm_resource_group.vmss.name}"
  network_interface_ids = ["${azurerm_network_interface.jumpbox.id}"]
  vm_size               = "Standard_DS1_v2"

  storage_image_reference {
    publisher = "Canonical"
    offer     = "UbuntuServer"
    sku       = "16.04-LTS"
    version   = "latest"
  }

  storage_os_disk {
    name              = "jumpbox-osdisk"
    caching           = "ReadWrite"
    create_option     = "FromImage"
    managed_disk_type = "Standard_LRS"
  }

  os_profile {
    computer_name  = "jumpbox"
    admin_username = "azureuser"
    admin_password = "Password1234!"
  }

  os_profile_linux_config {
    disable_password_authentication = true

    ssh_keys {
      path     = "/home/azureuser/.ssh/authorized_keys"
      key_data = "${file("~/.ssh/id_rsa.pub")}"
    }
  }

  tags {
    environment = "codelab"
  }
}
```

Bewerken `outputs.tf` en voeg de volgende code om weer te geven van de hostnaam van de jumpbox wanneer de implementatie voltooid is:

```
output "jumpbox_public_ip" {
    value = "${azurerm_public_ip.jumpbox.fqdn}"
}
```

## <a name="deploy-the-jumpbox"></a>De jumpbox implementeren

De jumpbox implementeren.

```bash
terraform apply 
```

Zodra de implementatie is voltooid, wordt de inhoud van de resourcegroep ziet:

![Terraform vm scaleset resourcegroep](./media/tf-create-create-vmss-step8.png)

> [!NOTE]
> Meld u aan met een wachtwoord op de jumpbox is uitgeschakeld en de virtuele-machineschaalset die u hebt geïmplementeerd. Aanmelden met SSH voor toegang tot de virtuele machines.

## <a name="clean-up-the-environment"></a>Opschonen van de omgeving

De volgende opdrachten verwijderen de resources in deze zelfstudie hebt gemaakt:

```bash
terraform destroy
```

Type `yes` wanneer u wordt gevraagd om te bevestigen voor het verwijderen van de resources. Het proces vernietiging duurt enkele minuten duren.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie maakt u een virtuele-machineschaalset ingesteld op Azure met behulp van Terraform geïmplementeerd. U hebt geleerd hoe u:

> [!div class="checklist"]
> * Implementatie van Terraform initialiseren
> * Gebruik van variabelen en uitvoer voor de implementatie van Terraform 
> * Maken en implementeren van een netwerkinfrastructuur
> * Maken en implementeren van een virtuele-machineschaalset en koppelt u dit aan een bestaande omgeving
> * Maken en implementeren van een jumpbox verbinding maken met de virtuele machines via SSH 
