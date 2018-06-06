---
title: Gebruik Terraform voor het maken van een virtuele machine van Azure schaal instellen
description: Zelfstudie over het gebruik van Terraform configureren en de versie van de schaal van een virtuele machine van Azure instellen op voltooid met een virtueel netwerk en beheerd gekoppelde schijven
keywords: terraform, devops, virtuele machines, Azure, schalen set-, netwerk-, opslag-, modules
author: tomarcher
manager: jeconnoc
ms.author: tarcher
ms.date: 06/04/2018
ms.topic: article
ms.openlocfilehash: b7cd9ad90198ead7c68d838547232429dbd1289f
ms.sourcegitcommit: 4f9fa86166b50e86cf089f31d85e16155b60559f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34757317"
---
# <a name="use-terraform-to-create-an-azure-virtual-machine-scale"></a>Terraform gebruiken voor het maken van de schaal van een virtuele machine van Azure

[Virtuele machine van Azure-schaalsets](/azure/virtual-machine-scale-sets) kunt u maken en beheren van een groep identieke virtuele machines waarbij het aantal exemplaren van virtuele machines kan automatisch vergroot of verklein in reactie op aanvraag of een ingesteld schema laden. 

In deze zelfstudie leert u het gebruik van [Azure Cloud Shell](/azure/cloud-shell/overview) de volgende taken uitvoeren:

> [!div class="checklist"]
> * De implementatie van een Terraform instellen
> * Gebruik van variabelen en uitvoer voor de implementatie van Terraform 
> * Maken en implementeren van de netwerkinfrastructuur
> * Maken en implementeren van een virtuele-machineschaalset en koppelt u dit aan het netwerk
> * Maken en implementeren van een jumpbox verbinding maken met de virtuele machines via SSH

> [!NOTE]
> De meest recente versie van de Terraform configuratiebestanden die in dit artikel zijn in de [geweldig Terraform opslagplaats op Github](https://github.com/Azure/awesome-terraform/tree/master/codelab-vmss).

## <a name="prerequisites"></a>Vereisten

- **Azure-abonnement**: als u nog geen abonnement op Azure hebt, maakt u een [gratis Azure-account](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) aan voordat u begint.

- **Installeer Terraform**: Volg de instructies in het artikel [Terraform en toegang tot Azure configureren](/azure/virtual-machines/linux/terraform-install-configure)

- **Maken van een SSH-sleutelpaar**: als u een SSH-sleutelpaar, volg de instructies in het artikel nog niet hebt [maken en de openbare en persoonlijke sleutelpaar voor een SSH gebruiken voor virtuele Linux-machines in Azure](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/mac-create-ssh-keys).

## <a name="create-the-directory-structure"></a>Maak de mapstructuur

1. Blader naar de [Azure-portal](http://portal.azure.com).

1. Open [Azure-Cloud-Shell](/azure/cloud-shell/overview). Als u een omgeving eerder hebt geselecteerd, selecteert u **Bash** als uw omgeving.

    ![Shell-prompt voor cloud](./media/terraform-create-vm-scaleset-network-disks-hcl/azure-portal-cloud-shell-button-min.png)

1. Wijzig de mappen op de `clouddrive` directory.

    ```bash
    cd clouddrive
    ```

1. Maak een map met de naam `vmss`.

    ```bash
    mkdir vmss
    ```

1. Wijzig de mappen in de nieuwe map:

    ```bash
    cd vmss
    ```

## <a name="create-the-variables-definitions-file"></a>De variabelen definities-bestand maken
In deze sectie definieert u de variabelen die de resources die zijn gemaakt door Terraform aanpassen.

In de Azure-Cloud-Shell, moet u de volgende stappen uitvoeren:

1. Maak een bestand met de naam `variables.tf`.

    ```bash
    vi variables.tf
    ```

1. Voer de invoegmodus door het selecteren van de sleutel.

1. De volgende code in de editor plakken:

  ```JSON
  variable "location" {
    description = "The location where resources will be created"
  }

  variable "tags" {
    description = "A map of the tags to use for the resources that are deployed"
    type        = "map"

    default = {
      environment = "codelab"
    }
  }

  variable "resource_group_name" {
    description = "The name of the resource group in which the resources will be created"
    default     = "myResourceGroup"
  }
  ```

1. Exit invoegmodus door het selecteren van de Esc-toets.

1. Sla het bestand op en sluit de editor vi af met de volgende opdracht:

    ```bash
    :wq
    ```

## <a name="create-the-output-definitions-file"></a>Het uitvoerbestand definities maken
In deze sectie maakt u het bestand met een beschrijving van de uitvoer na de implementatie.

In de Azure-Cloud-Shell, moet u de volgende stappen uitvoeren:

1. Maak een bestand met de naam `output.tf`.

    ```bash
    vi output.tf
    ```

1. Voer de invoegmodus door het selecteren van de sleutel.

1. Plak de volgende code in de editor om de volledig gekwalificeerde domeinnaam (FQDN) voor de virtuele machines weer te geven. :

  ```JSON
    output "vmss_public_ip" {
        value = "${azurerm_public_ip.vmss.fqdn}"
    }
  ```

1. Exit invoegmodus door het selecteren van de Esc-toets.

1. Sla het bestand op en sluit de editor vi af met de volgende opdracht:

    ```bash
    :wq
    ```

## <a name="define-the-network-infrastructure-in-a-template"></a>Definieer de netwerkinfrastructuur in een sjabloon
In deze sectie kunt u de volgende netwerkinfrastructuur in een nieuwe Azure-resourcegroep maken: 

  - Een virtueel netwerk (VNET) met de adresruimte van 10.0.0.0/16 
  - Een subnet met de adresruimte van 10.0.2.0/24
  - Twee openbare IP-adressen. Een door de virtuele machine scale set load balancer, de andere verbinding maken met de SSH-jumpbox gebruikt.

In de Azure-Cloud-Shell, moet u de volgende stappen uitvoeren:

1. Maak een bestand met de naam `vmss.tf` ingesteld om te beschrijven van de virtuele-machineschaalset infrastructuur.

    ```bash
    vi vmss.tf
    ```

1. Voer de invoegmodus door het selecteren van de sleutel.

1. Plak de volgende code aan het einde van het bestand om de volledig gekwalificeerde domeinnaam (FQDN) voor de virtuele machines weer te geven. 

  ```JSON
  resource "azurerm_resource_group" "vmss" {
    name     = "${var.resource_group_name}"
    location = "${var.location}"
    tags     = "${var.tags}"
  }

  resource "random_string" "fqdn" {
    length  = 6
    special = false
    upper   = false
    number  = false
  }

  resource "azurerm_virtual_network" "vmss" {
    name                = "vmss-vnet"
    address_space       = ["10.0.0.0/16"]
    location            = "${var.location}"
    resource_group_name = "${azurerm_resource_group.vmss.name}"
    tags                = "${var.tags}"
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
    domain_name_label            = "${random_string.fqdn.result}"
    tags                         = "${var.tags}"
  }
  ```

1. Exit invoegmodus door het selecteren van de Esc-toets.

1. Sla het bestand op en sluit de editor vi af met de volgende opdracht:

  ```bash
  :wq
  ```

## <a name="provision-the-network-infrastructure"></a>Inrichten van de netwerkinfrastructuur
Met behulp van de Azure-Cloud-Shell uit de map waar u de configuratiebestanden (.tf) hebt gemaakt in de volgende stappen uitvoeren:

1. Terraform initialiseren.

  ```bash
  terraform init 
  ```

1. Voer de volgende opdracht om de gedefinieerde infrastructuur in Azure te implementeren.

  ```bash
  terraform apply
  ```

  Terraform vraagt u om een waarde 'locatie' als de **locatie** variabele is gedefinieerd in `variables.tf`, maar deze nooit ingesteld. U kunt geen enkele geldige locatie - zoals 'West ons' gevolgd door het selecteren van Enter invoeren. (Een waarde tussen haakjes gebruiken met spaties.)

1. De uitvoer van Terraform worden afgedrukt zoals gedefinieerd in de `output.tf` bestand. Zoals u in de volgende schermafbeelding, de FQDN-naam heeft de vorm &lt;id >.&lt; locatie >. cloudapp.azure.com. De id-waarde is een berekende waarde en de locatie is de waarde die u bij het uitvoeren van Terraform opgeven.

  ![Virtuele-machineschaalset instellen FQDN-naam voor het openbare IP-adres](./media/terraform-create-vm-scaleset-network-disks-hcl/fqdn.png)

1. Selecteer in het menu van Azure portal, **resourcegroepen** vanuit het hoofdmenu.

1. Op de **resourcegroepen** tabblad **myResourceGroup** om de resources die zijn gemaakt door Terraform weer te geven.
  ![Virtuele-machineschaalset netwerkbronnen](./media/terraform-create-vm-scaleset-network-disks-hcl/resource-group-resources.png)

## <a name="add-a-virtual-machine-scale-set"></a>Toevoegen van een virtuele-machineschaalset

In deze sectie leert u hoe de volgende bronnen toevoegen aan de sjabloon:

- Een Azure load balancer en regels voor het leveren van de toepassing en deze te koppelen aan het openbare IP-adres geconfigureerd eerder in dit artikel
- Een Azure back-end-adresgroep en deze toewijzen aan de load balancer 
- Een health testpoort gebruikt door de toepassing en geconfigureerd op de load balancer 
- Een virtuele-machineschaalset ingesteld gestaan achter de load balancer die wordt uitgevoerd op de VNET die eerder in dit artikel wordt geïmplementeerd
- [Nginx](http://nginx.org/) op de knooppunten van de virtuele machine schaal met [cloud init](http://cloudinit.readthedocs.io/en/latest/).

In de Cloud-Shell, moet u de volgende stappen uitvoeren:

1. Open de `vmss.tf` configuratiebestand.

  ```bash
  vi vmss.tf
  ```

1. Ga naar het einde van het bestand en voer modus toevoegen door het selecteren van de A-toets.

1. Plak de volgende code aan het einde van het bestand:

  ```JSON
  resource "azurerm_lb" "vmss" {
    name                = "vmss-lb"
    location            = "${var.location}"
    resource_group_name = "${azurerm_resource_group.vmss.name}"

    frontend_ip_configuration {
      name                 = "PublicIPAddress"
      public_ip_address_id = "${azurerm_public_ip.vmss.id}"
    }

    tags = "${var.tags}"
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
      name              = "osdisk"
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
      admin_username       = "${var.admin_user}"
      admin_password       = "${var.admin_password}"
      custom_data          = "${file("web.conf")}"
    }

    os_profile_linux_config {
      disable_password_authentication = false
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

    tags = "${var.tags}"
}
  ```

1. Exit invoegmodus door het selecteren van de Esc-toets.

1. Sla het bestand op en sluit de editor vi af met de volgende opdracht:

    ```bash
    :wq
    ```

1. Maak een bestand met de naam `web.conf` moeten fungeren als de cloud init-configuratie voor de virtuele machines die deel van de scale-set uitmaken. 

    ```bash
    vi web.conf
    ```

1. Voer de invoegmodus door het selecteren van de sleutel.

1. De volgende code in de editor plakken:

  ```JSON
  #cloud-config
  packages:
    - nginx
  ```

1. Exit invoegmodus door het selecteren van de Esc-toets.

1. Sla het bestand op en sluit de editor vi af met de volgende opdracht:

    ```bash
    :wq
    ```

1. Open de `variables.tf` configuratiebestand.

  ```bash
  vi variables.tf
  ```

1. Ga naar het einde van het bestand en voer modus toevoegen door het selecteren van de A-toets.

1. De implementatie aanpassen door de volgende code aan het einde van het bestand plakken:

  ```JSON
  variable "application_port" {
      description = "The port that you want to expose to the external load balancer"
      default     = 80
  }

  variable "admin_user" {
      description = "User name to use as the admin account on the VMs that will be part of the VM Scale Set"
      default     = "azureuser"
  }

  variable "admin_password" {
      description = "Default password for admin account"
  }
  ``` 

1. Exit invoegmodus door het selecteren van de Esc-toets.

1. Sla het bestand op en sluit de editor vi af met de volgende opdracht:

    ```bash
    :wq
    ```

1. Een Terraform opstellen voor het visualiseren van de virtuele machine scale set-implementatie. (U moet een wachtwoord van uw keuze, evenals de locatie voor uw resources opgeven.)

  ```bash
  terraform plan
  ```

  De uitvoer van de opdracht moet er ongeveer als de volgende schermafbeelding:

  ![De uitvoer van de virtuele-machineschaalset maken](./media/terraform-create-vm-scaleset-network-disks-hcl/add-mvss-plan.png)

1. Implementeer de nieuwe resources in Azure.

  ```bash
  terraform apply 
  ```

  De uitvoer van de opdracht moet er ongeveer als de volgende schermafbeelding:

  ![Terraform virtuele-machineschaalset resourcegroep](./media/terraform-create-vm-scaleset-network-disks-hcl/resource-group-contents.png)

1. Open een browser en maak verbinding met de FQDN-naam die is geretourneerd door de opdracht. 

  ![Resultaten van het bladeren naar de FQDN-naam](./media/terraform-create-vm-scaleset-network-disks-hcl/browser-fqdn.png)

## <a name="add-an-ssh-jumpbox"></a>Een SSH-jumpbox toevoegen
Een SSH *jumpbox* is een enkele server die u 'springen' via om andere servers in het netwerk toegang te krijgen. In deze stap configureert u de volgende bronnen:

- Een netwerkinterface (of jumpbox) gekoppeld aan hetzelfde subnet als de virtuele-machineschaalset.

- Een virtuele machine verbonden met deze netwerkinterface. Deze 'jumpbox' is extern toegankelijk. Eenmaal zijn verbonden, kunt u SSH met een van de virtuele machines in de schaalset.

1. Open de `vmss.tf` configuratiebestand.

  ```bash
  vi vmss.tf
  ```

1. Ga naar het einde van het bestand en voer modus toevoegen door het selecteren van de A-toets.

1. Plak de volgende code aan het einde van het bestand:

  ```JSON
  resource "azurerm_public_ip" "jumpbox" {
    name                         = "jumpbox-public-ip"
    location                     = "${var.location}"
    resource_group_name          = "${azurerm_resource_group.vmss.name}"
    public_ip_address_allocation = "static"
    domain_name_label            = "${random_string.fqdn.result}-ssh"
    tags                         = "${var.tags}"
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

    tags = "${var.tags}"
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
      admin_username = "${var.admin_user}"
      admin_password = "${var.admin_password}"
    }

    os_profile_linux_config {
      disable_password_authentication = false
    }

    tags = "${var.tags}"
  }
  ```

1. Open de `output.tf` configuratiebestand.

  ```bash
  vi output.tf
  ```

1. Ga naar het einde van het bestand en voer modus toevoegen door het selecteren van de A-toets.

1. Plak de volgende code aan het einde van het bestand naar de hostnaam van de jumpbox weergegeven wanneer de implementatie voltooid is:

  ```
  output "jumpbox_public_ip" {
      value = "${azurerm_public_ip.jumpbox.fqdn}"
  }
  ```

1. Exit invoegmodus door het selecteren van de Esc-toets.

1. Sla het bestand op en sluit de editor vi af met de volgende opdracht:

    ```bash
    :wq
    ```

1. De jumpbox implementeren.

  ```bash
  terraform apply 
  ```

Zodra de implementatie is voltooid, de inhoud van de resourcegroep lijkt op die wordt weergegeven in de volgende schermafbeelding:

![Terraform virtuele-machineschaalset resourcegroep](./media/terraform-create-vm-scaleset-network-disks-hcl/resource-group-contents-final.png)

> [!NOTE]
> De mogelijkheid aanmelden met een wachtwoord is uitgeschakeld op de jumpbox en de virtuele-machineschaalset die u hebt geïmplementeerd. Aanmelden met SSH voor toegang tot de virtuele machine.

## <a name="environment-cleanup"></a>Opruimen van de omgeving 

Als u wilt de Terraform resources verwijderen die zijn gemaakt in deze zelfstudie, voer de volgende opdracht in de Cloud Shell:

```bash
terraform destroy
```

Het proces vernietiging kan enkele minuten duren.

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe Terraform gebruiken voor het maken van een virtuele machine van Azure-schaalset. Hier volgen enkele aanvullende resources waarmee u kunt meer informatie over Terraform op Azure: 

 [Terraform-Hub in Microsoft.com](https://docs.microsoft.com/azure/terraform/)  
 [Terraform Azure provider-documentatie](http://aka.ms/terraform)  
 [Terraform Azure provider bron](http://aka.ms/tfgit)  
 [Terraform Azure-modules](http://aka.ms/tfmodules)