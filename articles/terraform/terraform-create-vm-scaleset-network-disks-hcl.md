---
title: Gebruik Terraform om te maken van de schaal van een virtuele machine van Azure instellen
description: Zelfstudie over het gebruik van Terraform configureren en de versie van de schaal van een virtuele Azure-machine instellen op voltooid met een virtueel netwerk en beheerd gekoppelde schijven
keywords: terraform, devops, virtuele machines, Azure, schaal instellen, netwerk, opslag, modules
author: tomarcher
manager: jeconnoc
ms.author: tarcher
ms.date: 06/04/2018
ms.topic: article
ms.openlocfilehash: 5922bad24c50a9d315aae42ce11a33801b9dbcaf
ms.sourcegitcommit: f606248b31182cc559b21e79778c9397127e54df
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/12/2018
ms.locfileid: "38971830"
---
# <a name="use-terraform-to-create-an-azure-virtual-machine-scale-set"></a>Gebruik Terraform om te maken van de schaal van een virtuele machine van Azure instellen

[Azure virtuele-machineschaalsets](/azure/virtual-machine-scale-sets) kunt u maken en beheren van een groep identieke, laden met gelijke taakverdeling virtuele machines waarbij het aantal exemplaren van virtuele machines kan automatisch vergroten of afnemen in reactie op aanvraag of volgens een ingesteld schema. 

In deze zelfstudie leert u hoe u [Azure Cloud Shell](/azure/cloud-shell/overview) naar de volgende taken uitvoeren:

> [!div class="checklist"]
> * Instellen van een implementatie met Terraform
> * Gebruik van variabelen en uitvoer voor de implementatie van Terraform 
> * Maken en implementeren van de netwerkinfrastructuur
> * Maken en implementeren van een virtuele-machineschaalset en deze koppelen aan het netwerk
> * Maken en implementeren van een jumpbox verbinding maken met de virtuele machines via SSH

> [!NOTE]
> De meest recente versie van de Terraform-configuratiebestanden gebruikt in dit artikel zijn in de [geweldige Terraform-bibliotheek op Github](https://github.com/Azure/awesome-terraform/tree/master/codelab-vmss).

## <a name="prerequisites"></a>Vereisten

- **Azure-abonnement**: als u nog geen abonnement op Azure hebt, maakt u een [gratis Azure-account](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) aan voordat u begint.

- **Terraform installeren**: Volg de aanwijzingen in het artikel [Terraform en toegang tot Azure configureren](/azure/virtual-machines/linux/terraform-install-configure)

- **Een SSH-sleutelpaar maken**: als u een SSH-sleutelpaar, volg de instructies in het artikel nog niet hebt [maken en gebruiken van de openbare en persoonlijke sleutelpaar voor een SSH voor virtuele Linux-machines in Azure](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/mac-create-ssh-keys).

## <a name="create-the-directory-structure"></a>De mapstructuur maken

1. Blader naar de [Azure-portal](http://portal.azure.com).

1. Open [Azure Cloudshell](/azure/cloud-shell/overview). Als u een omgeving niet eerder hebt geselecteerd, selecteert u **Bash** als uw omgeving.

    ![Cloud Shell-prompt](./media/terraform-create-vm-scaleset-network-disks-hcl/azure-portal-cloud-shell-button-min.png)

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

## <a name="create-the-variables-definitions-file"></a>De variabelen voor de definities van bestand maken
In deze sectie definieert u de variabelen die de resources die zijn gemaakt door Terraform aanpassen.

In de Azure Cloud Shell, moet u de volgende stappen uitvoeren:

1. Maak een bestand met de naam `variables.tf`.

    ```bash
    vi variables.tf
    ```

1. Voer de invoegmodus door het selecteren van de sleutel.

1. Plak de volgende code in de editor:

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

1. Afsluiten invoegen modus door het selecteren van de Esc-toets.

1. Sla het bestand op en sluit de editor vi met de volgende opdracht:

    ```bash
    :wq
    ```

## <a name="create-the-output-definitions-file"></a>Het uitvoerbestand definities maken
In deze sectie maakt u het bestand met een beschrijving van de uitvoer na de implementatie.

In de Azure Cloud Shell, moet u de volgende stappen uitvoeren:

1. Maak een bestand met de naam `output.tf`.

    ```bash
    vi output.tf
    ```

1. Voer de invoegmodus door het selecteren van de sleutel.

1. Plak de volgende code in de editor om de volledig gekwalificeerde domeinnaam (FQDN) voor de virtuele machines zichtbaar te maken. :

  ```JSON
    output "vmss_public_ip" {
        value = "${azurerm_public_ip.vmss.fqdn}"
    }
  ```

1. Afsluiten invoegen modus door het selecteren van de Esc-toets.

1. Sla het bestand op en sluit de editor vi met de volgende opdracht:

    ```bash
    :wq
    ```

## <a name="define-the-network-infrastructure-in-a-template"></a>De netwerkinfrastructuur definiëren in een sjabloon
In deze sectie maakt maken u de volgende netwerkinfrastructuur in een nieuwe Azure-resourcegroep: 

  - Een virtueel netwerk (VNET) met de adresruimte van 10.0.0.0/16 
  - Een subnet met de adresruimte van 10.0.2.0/24
  - Twee openbare IP-adressen. Een door de virtual machine scale set load balancer, de andere gebruikt voor verbinding met de jumpbox SSH gebruikt.

In de Azure Cloud Shell, moet u de volgende stappen uitvoeren:

1. Maak een bestand met de naam `vmss.tf` instellen om te beschrijven van de virtuele-machineschaalset infrastructuur.

    ```bash
    vi vmss.tf
    ```

1. Voer de invoegmodus door het selecteren van de sleutel.

1. Plak de volgende code aan het einde van het bestand om de volledig gekwalificeerde domeinnaam (FQDN) voor de virtuele machines zichtbaar te maken. 

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

1. Afsluiten invoegen modus door het selecteren van de Esc-toets.

1. Sla het bestand op en sluit de editor vi met de volgende opdracht:

  ```bash
  :wq
  ```

## <a name="provision-the-network-infrastructure"></a>De netwerkinfrastructuur inrichten
Met behulp van de Azure Cloud Shell uit de map waar u de configuratiebestanden (.tf) hebt gemaakt, voer de volgende stappen uit:

1. Terraform initialiseren.

  ```bash
  terraform init 
  ```

1. Voer de volgende opdracht om de gedefinieerde infrastructuur in Azure te implementeren.

  ```bash
  terraform apply
  ```

  Terraform vraagt u om een waarde van "locatie" Als de **locatie** variabele is gedefinieerd in `variables.tf`, maar deze nooit ingesteld. U kunt elke geldige locatie, bijvoorbeeld VS-West' gevolgd door het selecteren van Enter invoeren. (Gebruik een waarde tussen haakjes met spaties).

1. De uitvoer van Terraform worden afgedrukt zoals gedefinieerd in de `output.tf` bestand. Zoals weergegeven in de volgende schermafbeelding, de FQDN-naam heeft de vorm &lt;id >.&lt; locatie >. cloudapp.azure.com. De id-waarde is een berekende waarde en de locatie is de waarde die u opgeeft bij het uitvoeren van Terraform.

  ![Virtuele-machineschaalset FQDN-naam op voor openbare IP-adres](./media/terraform-create-vm-scaleset-network-disks-hcl/fqdn.png)

1. Selecteer in het menu van Azure portal **resourcegroepen** in het hoofdmenu.

1. Op de **resourcegroepen** tabblad **myResourceGroup** om de resources die zijn gemaakt door Terraform weer te geven.
  ![Virtuele-machineschaalset netwerkbronnen](./media/terraform-create-vm-scaleset-network-disks-hcl/resource-group-resources.png)

## <a name="add-a-virtual-machine-scale-set"></a>Toevoegen van een virtuele-machineschaalset

In deze sectie leert u hoe u de volgende bronnen toevoegen aan de sjabloon:

- Een Azure load balancer en regels voor het leveren van de toepassing en deze koppelen aan het openbare IP-adres geconfigureerd eerder in dit artikel
- Een Azure-back-end-adresgroep en wijs deze toe aan de load balancer 
- Een health testpoort die wordt gebruikt door de toepassing en geconfigureerd op de load balancer 
- Een virtuele-machineschaalset zitten achter de load balancer die wordt uitgevoerd op de VNET die eerder in dit artikel is geïmplementeerd
- [Nginx](http://nginx.org/) op de knooppunten van de virtuele machine schaal met [cloud-init](http://cloudinit.readthedocs.io/en/latest/).

In Cloud Shell, voert u de volgende stappen uit:

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

1. Afsluiten invoegen modus door het selecteren van de Esc-toets.

1. Sla het bestand op en sluit de editor vi met de volgende opdracht:

    ```bash
    :wq
    ```

1. Maak een bestand met de naam `web.conf` om te fungeren als de cloud-init-configuratie voor de virtuele machines die deel van de schaalset uitmaken. 

    ```bash
    vi web.conf
    ```

1. Voer de invoegmodus door het selecteren van de sleutel.

1. Plak de volgende code in de editor:

  ```JSON
  #cloud-config
  packages:
    - nginx
  ```

1. Afsluiten invoegen modus door het selecteren van de Esc-toets.

1. Sla het bestand op en sluit de editor vi met de volgende opdracht:

    ```bash
    :wq
    ```

1. Open de `variables.tf` configuratiebestand.

  ```bash
  vi variables.tf
  ```

1. Ga naar het einde van het bestand en voer modus toevoegen door het selecteren van de A-toets.

1. De implementatie aanpassen door de volgende code aan het einde van het bestand te plakken:

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

1. Afsluiten invoegen modus door het selecteren van de Esc-toets.

1. Sla het bestand op en sluit de editor vi met de volgende opdracht:

    ```bash
    :wq
    ```

1. Maak een plan Terraform voor het visualiseren van de implementatie van virtuele machines scale set. (U moet een wachtwoord van uw keuze, evenals de locatie voor uw resources opgeven.)

  ```bash
  terraform plan
  ```

  De uitvoer van de opdracht is vergelijkbaar met de volgende schermafbeelding:

  ![Uitvoer van de virtuele-machineschaalset maken](./media/terraform-create-vm-scaleset-network-disks-hcl/add-mvss-plan.png)

1. De nieuwe resources in Azure implementeren.

  ```bash
  terraform apply 
  ```

  De uitvoer van de opdracht is vergelijkbaar met de volgende schermafbeelding:

  ![Terraform machineschaalset virtuele-resourcegroep](./media/terraform-create-vm-scaleset-network-disks-hcl/resource-group-contents.png)

1. Open een browser en maak verbinding met de FQDN-naam die is geretourneerd door de opdracht. 

  ![Resultaten van bladeren naar de FQDN-naam](./media/terraform-create-vm-scaleset-network-disks-hcl/browser-fqdn.png)

## <a name="add-an-ssh-jumpbox"></a>Een SSH-jumpbox toevoegen
Een SSH *jumpbox* is een enkele server die u "gaan" via toegang tot andere servers in het netwerk. In deze stap configureert u de volgende bronnen:

- Een netwerkinterface (of een jumpbox) verbonden met hetzelfde subnet als de virtuele-machineschaalset.

- Een virtuele machine aan deze netwerkinterface worden gekoppeld. Deze 'jumpbox' is extern toegankelijk. Eenmaal verbinding hebben, kunt u SSH op elk van de virtuele machines in de schaalset.

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

1. Plak de volgende code aan het einde van het bestand om weer te geven van de hostnaam van de jumpbox wanneer de implementatie voltooid is:

  ```
  output "jumpbox_public_ip" {
      value = "${azurerm_public_ip.jumpbox.fqdn}"
  }
  ```

1. Afsluiten invoegen modus door het selecteren van de Esc-toets.

1. Sla het bestand op en sluit de editor vi met de volgende opdracht:

    ```bash
    :wq
    ```

1. Implementeer de jumpbox.

  ```bash
  terraform apply 
  ```

Als de implementatie is voltooid, de inhoud van de resourcegroep lijkt op die wordt weergegeven in de volgende schermafbeelding:

![Terraform machineschaalset virtuele-resourcegroep](./media/terraform-create-vm-scaleset-network-disks-hcl/resource-group-contents-final.png)

> [!NOTE]
> De mogelijkheid om aan te melden met een wachtwoord is uitgeschakeld op de jumpbox en de virtuele-machineschaalset die u hebt geïmplementeerd. Aanmelden met SSH voor toegang tot de virtuele machines.

## <a name="environment-cleanup"></a>Opschonen van de omgeving 

De Terraform om resources te verwijderen die zijn gemaakt in deze zelfstudie, voer de volgende opdracht in Cloud Shell:

```bash
terraform destroy
```

De vernietiging-proces kan enkele minuten duren.

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd hoe u een schaalset voor virtuele Azure-machine maken met Terraform. Hier volgen enkele aanvullende resources waarmee u meer informatie over Terraform op Azure: 

 [Terraform-Hub in Microsoft.com](https://docs.microsoft.com/azure/terraform/)  
 [Documentatie voor Terraform Azure-provider](http://aka.ms/terraform)  
 [Bron van Terraform Azure-provider](http://aka.ms/tfgit)  
 [Terraform Azure-modules](http://aka.ms/tfmodules)