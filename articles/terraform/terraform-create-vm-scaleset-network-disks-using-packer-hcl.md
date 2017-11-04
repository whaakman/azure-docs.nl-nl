---
title: Terraform gebruik maken van een virtuele machine van Azure schaal van een aangepaste installatiekopie verpakker instellen
description: Terraform configureren en een virtuele machine van Azure-schaalset van een aangepaste installatiekopie gegenereerd door verpakker (voltooid met een virtueel netwerk en beheerde gekoppelde schijven) versie gebruiken.
keywords: "terraform, devops, ingesteld, virtuele machine, netwerk, opslag, modules, aangepaste installatiekopieën, verpakker schalen"
author: VaijanathB
ms.author: tarcher
ms.date: 10/29/2017
ms.topic: article
ms.openlocfilehash: 284eae93de36986e41ba80f98f86495d8f34f57b
ms.sourcegitcommit: 43c3d0d61c008195a0177ec56bf0795dc103b8fa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/01/2017
---
# <a name="use-terraform-to-create-an-azure-virtual-machine-scale-set-from-a-packer-custom-image"></a>Terraform gebruik maken van een virtuele machine van Azure schaal van een aangepaste installatiekopie verpakker instellen

In deze zelfstudie gebruikt u [Terraform](https://www.terraform.io/) maakt en implementeert een [Azure virtuele-machineschaalset](/azure/virtual-machine-scale-sets/virtual-machine-scale-sets-overview) gemaakt met een aangepaste installatiekopie gemaakt met [verpakker](https://www.packer.io/intro/index.html) met beheerde-schijven met behulp van de [HashiCorp configuratie taal](https://www.terraform.io/docs/configuration/syntax.html) (lijst met compatibele hardware).  

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Instellen van uw implementatie Terraform
> * Gebruik van variabelen en uitvoer voor de implementatie van Terraform 
> * Maken en implementeren van een netwerkinfrastructuur
> * De installatiekopie van een aangepaste virtuele machine met behulp van verpakker maken
> * Maken en implementeren van een virtuele-machineschaalset ingesteld met de aangepaste installatiekopie
> * Maken en implementeren van een jumpbox 

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="before-you-begin"></a>Voordat u begint
> * [Terraform installeren en configureren van toegang tot Azure](https://docs.microsoft.com/azure/virtual-machines/linux/terraform-install-configure)
> * [Maken van een SSH-sleutelpaar](https://docs.microsoft.com/azure/virtual-machines/linux/mac-create-ssh-keys) als u er nog geen hebt
> * [Installeer verpakker](https://www.packer.io/docs/install/index.html) als u nog niet geïnstalleerd op uw lokale machine verpakker hebt


## <a name="create-the-file-structure"></a>De bestandsstructuur maken

Maak drie nieuwe bestanden in een lege map met de volgende namen:

- ```variables.tf```Dit bestand bevat de waarden van variabelen die in de sjabloon.
- ```output.tf```Dit bestand beschrijft de instellingen die na de implementatie weergeven.
- ```vmss.tf```Dit bestand bevat de code van de infrastructuur die u implementeert.

##  <a name="create-the-variables"></a>De variabelen maken 

In deze stap definieert u de variabelen die de resources die zijn gemaakt door Terraform aanpassen.

Bewerk de `variables.tf` bestand, Kopieer de volgende code en sla de wijzigingen.

```tf 
variable "location" {
  description = "The location where resources are created"
  default     = "East US"
}

variable "resource_group_name" {
  description = "The name of the resource group in which the resources are created"
  default     = ""
}

```

> [!NOTE]
> De standaardwaarde van de variabele resource_group_name is niet in, het definiëren van uw eigen waarde.

Sla het bestand op.

Wanneer u de sjabloon Terraform implementeert, wilt u de FQDN-naam die wordt gebruikt voor toegang tot de toepassing. Gebruik de ```output``` brontype van Terraform en haal de ```fqdn``` eigenschap van de resource. 

Bewerk de `output.tf` bestand en kopieer de volgende code om de volledig gekwalificeerde domeinnaam voor de virtuele machines weer te geven. 

```hcl 
output "vmss_public_ip" {
    value = "${azurerm_public_ip.vmss.fqdn}"
}
```

## <a name="define-the-network-infrastructure-in-a-template"></a>Definieer de netwerkinfrastructuur in een sjabloon 

In deze stap maakt maken u de volgende netwerkinfrastructuur in een nieuwe Azure-resourcegroep: 
  - Een VNET met de adresruimte van 10.0.0.0/16 
  - Een subnet met de adresruimte van 10.0.2.0/24
  - Twee openbare IP-adressen. Gebruikt door de virtuele machine scale set load balancer. de andere verbinding maken met de SSH-jumpbox

U moet ook een resourcegroep waarin alle resources worden gemaakt. 

Bewerken en kopieer de volgende code in de ```vmss.tf``` bestand: 

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
> Het is raadzaam om de labels van de resources in Azure vergemakkelijken de identificatie in de toekomst wordt geïmplementeerd.

## <a name="create-the-network-infrastructure"></a>Maken van de netwerkinfrastructuur

Initialiseren van de omgeving Terraform door met de volgende opdracht in de map waar u hebt gemaakt de `.tf` bestanden:

```bash
terraform init 
```
 
De invoegtoepassingen van de provider downloaden uit het register Terraform in de ```.terraform``` map in de map waar u de opdracht hebt uitgevoerd.

Voer de volgende opdracht om de infrastructuur in Azure te implementeren.

```bash
terraform apply
```

Controleer of de volledig gekwalificeerde domeinnaam van het openbare IP-adres overeenkomt met uw configuratie.

![Virtuele-machineschaalset Terraform volledig gekwalificeerde domeinnaam voor openbare IP-adres](./media/terraform-create-vm-scaleset-network-disks-using-packer-hcl/tf-create-vmss-step4-fqdn.png)

De resourcegroep bevat de volgende bronnen:

![Virtuele-machineschaalset Terraform netwerkbronnen](./media/terraform-create-vm-scaleset-network-disks-using-packer-hcl/tf-create-vmss-step4-rg.png)


## <a name="create-an-azure-image-using-packer"></a>Een Azure-installatiekopie via verpakker maken
Maken van een aangepaste Linux-installatiekopie met de stappen die worden beschreven in de zelfstudie [verpakker gebruiken om u te maken van installatiekopieën van Linux virtuele machines in Azure](https://docs.microsoft.com/azure/virtual-machines/linux/build-image-with-packer).
 
Volg de zelfstudie voor het maken van een virtuele Ubuntu-installatiekopie gemaakt met NGINX geïnstalleerd.

![Zodra u de installatiekopie van het verpakker maakt, hebt u een installatiekopie](./media/terraform-create-vm-scaleset-network-disks-using-packer-hcl/packerimagecreated.png)

> [!NOTE]
> Een opdracht wordt uitgevoerd voor deze zelfstudie, in de afbeelding verpakker naar nginx installeert. U kunt ook uw eigen script uitvoeren tijdens het maken van.

## <a name="edit-the-infrastructure-to-add-the-virtual-machine-scale-set"></a>De infrastructuur voor het toevoegen van de virtuele-machineschaalset bewerken

In deze stap maakt u de volgende bronnen op het netwerk die eerder was geïmplementeerd:
- Azure load balancer dient voor de toepassing en koppelt u dit aan het openbare IP-adres dat is geïmplementeerd in stap 4
- Een Azure load balancer en regels voor het leveren van de toepassing en deze te koppelen aan het openbare IP-adres eerder hebt geconfigureerd.
- Azure back-end-adresgroep en deze toewijzen aan de load balancer 
- Een health testpoort gebruikt door de toepassing en geconfigureerd op de load balancer 
- Een virtuele-machineschaalset zitten achter de load balancer, uitgevoerd op het vnet eerder hebt geïmplementeerd
- [Nginx](http://nginx.org/) op de knooppunten van de virtuele-machineschaalset geïnstalleerd vanuit de aangepaste installatiekopie


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

data "azurerm_resource_group" "image" {
  name = "myResourceGroup"
}

data "azurerm_image" "image" {
  name                = "myPackerImage"
  resource_group_name = "${data.azurerm_resource_group.image.name}"
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
    id="${data.azurerm_image.image.id}"
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

De uitvoer van de opdracht ziet er in de volgende afbeelding:

![Terraform toevoegen aan virtuele-machineschaalset planning instellen](./media/terraform-create-vm-scaleset-network-disks-using-packer-hcl/tf-create-vmss-step6-plan.png)

De aanvullende resources in Azure implementeren: 

```bash
terraform apply 
```

De inhoud van de resourcegroep, ziet de volgende afbeelding:

![Terraform virtuele-machineschaalset resourcegroep](./media/terraform-create-vm-scaleset-network-disks-using-packer-hcl/tf-create-vmss-step6-apply.png)

Open een browser en maak verbinding met de volledig gekwalificeerde domeinnaam die is geretourneerd door de opdracht. 


## <a name="add-a-jumpbox-to-the-existing-network"></a>Een jumpbox toevoegen aan de bestaande netwerk 

Deze optionele stap kunt SSH toegang tot de exemplaren van de virtuele-machineschaalset ingesteld met behulp van een jumpbox.

De volgende bronnen toevoegen aan uw bestaande implementatie:
- Een netwerkinterface die is verbonden met hetzelfde subnet bevinden dan de virtuele-machineschaalset
- Een virtuele machine met deze netwerkinterface

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

Bewerken `outputs.tf` om toe te voegen met de volgende code die de hostnaam van de jumpbox wordt weergegeven wanneer de implementatie is voltooid:

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

Zodra de implementatie is voltooid, ziet de inhoud van de resourcegroep uit de volgende afbeelding:

![Terraform virtuele-machineschaalset resourcegroep](./media/terraform-create-vm-scaleset-network-disks-using-packer-hcl/tf-create-create-vmss-step8.png)

> [!NOTE]
> Meld u aan met een wachtwoord op de jumpbox is uitgeschakeld en de virtuele-machineschaalset die u hebt geïmplementeerd. Aanmelden met SSH voor toegang tot de virtuele machines.

## <a name="clean-up-the-environment"></a>Opschonen van de omgeving

De volgende opdrachten verwijderen de resources in deze zelfstudie hebt gemaakt:

```bash
terraform destroy
```

Type `yes` wanneer u wordt gevraagd om te bevestigen voor het verwijderen van de resources. De vernietiging kan enkele minuten duren.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie maakt u een virtuele-machineschaalset en geïmplementeerd een jumpbox op Azure met behulp van Terraform. U hebt geleerd hoe u:

> [!div class="checklist"]
> * Implementatie van Terraform initialiseren
> * Gebruik van variabelen en uitvoer voor de implementatie van Terraform 
> * Maken en implementeren van een netwerkinfrastructuur
> * De installatiekopie van een aangepaste virtuele machine met behulp van verpakker maken
> * Maken en implementeren van een virtuele-machineschaalset ingesteld met de aangepaste installatiekopie
> * Maken en implementeren van een jumpbox 