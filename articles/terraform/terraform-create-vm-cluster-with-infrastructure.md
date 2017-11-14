---
title: Maken van een VM-cluster met Terraform en de lijst met compatibele hardware
description: Gebruik Terraform en HashiCorp configuratie taal (lijst met compatibele hardware) te maken van een virtuele Linux-machine-cluster met een load balancer in Azure
keywords: terraform, devops, virtuele machine, netwerk, modules
author: tomarcher
manager: routlaw
ms.service: virtual-machines-linux
ms.custom: devops
ms.topic: article
ms.date: 11/13/2017
ms.author: tarcher
ms.openlocfilehash: 2435d694e6a1671a234d02f90860e5cafe98c2df
ms.sourcegitcommit: 659cc0ace5d3b996e7e8608cfa4991dcac3ea129
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/13/2017
---
# <a name="create-a-vm-cluster-with-terraform-and-hcl"></a>Maken van een VM-cluster met Terraform en de lijst met compatibele hardware

In deze zelfstudie wordt het maken van een kleine compute cluster met de [HashiCorp configuratie taal](https://www.terraform.io/docs/configuration/syntax.html) (lijst met compatibele hardware). De configuratie maakt een load balancer, twee Linux VM's in een [beschikbaarheidsset](/azure/virtual-machines/windows/manage-availability#configure-multiple-virtual-machines-in-an-availability-set-for-redundancy), en alle benodigde netwerkresources.

In deze zelfstudie hebt u:

> [!div class="checklist"]
> * Azure-verificatie instellen
> * Een configuratiebestand Terraform maken
> * Terraform initialiseren
> * Een uitvoeringsplan Terraform maken
> * Het uitvoeringsplan Terraform toepassen

## <a name="1-set-up-azure-authentication"></a>1. Azure-verificatie instellen

> [!NOTE]
> Als u [Terraform omgevingsvariabelen gebruiken](/azure/virtual-machines/linux/terraform-install-configure#set-environment-variables), of het uitvoeren van deze zelfstudie in de [Azure Cloud Shell](terraform-cloud-shell.md), kunt u deze sectie overslaan.

In deze sectie maakt u een Azure-service principal en genereren twee Terraform-configuratiebestanden met de referenties van de beveiligings-principal.

1. [Instellen van een Azure AD-service-principal](/azure/virtual-machines/linux/terraform-install-configure#set-up-terraform-access-to-azure) Terraform inrichten bronnen inschakelen in Azure. Tijdens het maken van de principal Noteer de waarden voor de abonnements-ID, de tenant, de appId en het wachtwoord.

2. Open een opdrachtprompt.

3. Maak een lege map waarin u uw Terraform-bestanden op te slaan.

4. Maak een nieuw bestand waarin de variabelen-declaraties. U kunt dit bestand een willekeurige naam met een `.tf` extensie.

5. Kopieer de volgende code in het bestand variabelen declareren:

  ```tf
  variable subscription_id {}
  variable tenant_id {}
  variable client_id {}
  variable client_secret {}
  
  provider "azurerm" {
      subscription_id = "${var.subscription_id}"
      tenant_id = "${var.tenant_id}"
      client_id = "${var.client_id}"
      client_secret = "${var.client_secret}"
  }
  ```

6. Maak een nieuw bestand met de waarden voor uw Terraform-variabelen. Is het gebruikelijk dat als naam voor uw variabelebestand Terraform `terraform.tfvars` die Terraform automatisch een bestand met de naam geladen `terraform.tfvars` (of een patroon van de volgende `*.auto.tfvars`) indien aanwezig in de huidige map. 

7. Kopieer de volgende code in uw bestand variabelen. Vervang de tijdelijke aanduidingen als volgt: voor `subscription_id`, gebruikt u de Azure-abonnement-ID die u hebt opgegeven bij het uitvoeren van `az account set`. Voor `tenant_id`, gebruiken de `tenant` geretourneerde waarde van `az ad sp create-for-rbac`. Voor `client_id`, gebruiken de `appId` geretourneerde waarde van `az ad sp create-for-rbac`. Voor `client_secret`, gebruiken de `password` geretourneerde waarde van `az ad sp create-for-rbac`.

  ```tf
  subscription_id = "<azure-subscription-id>"
  tenant_id = "<tenant-returned-from-creating-a-service-principal>"
  client_id = "<appId-returned-from-creating-a-service-principal>"
  client_secret = "<password-returned-from-creating-a-service-principal>"
  ```

## <a name="2-create-a-terraform-configuration-file"></a>2. Een configuratiebestand Terraform maken

In deze sectie maakt u een bestand met de definities van de bron voor uw infrastructuur.

1. Maak een nieuw bestand met de naam `main.tf`. 

2. Kopiëren resourcedefinities voorbeeld te volgen in de zojuist gemaakte `main.tf` bestand: 

  ```tf
  resource "azurerm_resource_group" "test" {
    name     = "acctestrg"
    location = "West US 2"
  }

  resource "azurerm_virtual_network" "test" {
    name                = "acctvn"
    address_space       = ["10.0.0.0/16"]
    location            = "${azurerm_resource_group.test.location}"
    resource_group_name = "${azurerm_resource_group.test.name}"
  }

  resource "azurerm_subnet" "test" {
    name                 = "acctsub"
    resource_group_name  = "${azurerm_resource_group.test.name}"
    virtual_network_name = "${azurerm_virtual_network.test.name}"
    address_prefix       = "10.0.2.0/24"
  }

  resource "azurerm_public_ip" "test" {
    name                         = "publicIPForLB"
    location                     = "${azurerm_resource_group.test.location}"
    resource_group_name          = "${azurerm_resource_group.test.name}"
    public_ip_address_allocation = "static"
  }

  resource "azurerm_lb" "test" {
    name                = "loadBalancer"
    location            = "${azurerm_resource_group.test.location}"
    resource_group_name = "${azurerm_resource_group.test.name}"

    frontend_ip_configuration {
      name                 = "publicIPAddress"
      public_ip_address_id = "${azurerm_public_ip.test.id}"
    }
  }

  resource "azurerm_lb_backend_address_pool" "test" {
    resource_group_name = "${azurerm_resource_group.test.name}"
    loadbalancer_id     = "${azurerm_lb.test.id}"
    name                = "BackEndAddressPool"
  }

  resource "azurerm_network_interface" "test" {
    count               = 2
    name                = "acctni${count.index}"
    location            = "${azurerm_resource_group.test.location}"
    resource_group_name = "${azurerm_resource_group.test.name}"

    ip_configuration {
      name                          = "testConfiguration"
      subnet_id                     = "${azurerm_subnet.test.id}"
      private_ip_address_allocation = "dynamic"
      load_balancer_backend_address_pools_ids = ["${azurerm_lb_backend_address_pool.test.id}"]
    }
  }

  resource "azurerm_managed_disk" "test" {
    count                = 2
    name                 = "datadisk_existing_${count.index}"
    location             = "${azurerm_resource_group.test.location}"
    resource_group_name  = "${azurerm_resource_group.test.name}"
    storage_account_type = "Standard_LRS"
    create_option        = "Empty"
    disk_size_gb         = "1023"
  }

  resource "azurerm_availability_set" "avset" {
    name                         = "avset"
    location                     = "${azurerm_resource_group.test.location}"
    resource_group_name          = "${azurerm_resource_group.test.name}"
    platform_fault_domain_count  = 2
    platform_update_domain_count = 2
    managed                      = true
  }

  resource "azurerm_virtual_machine" "test" {
    count                 = 2
    name                  = "acctvm${count.index}"
    location              = "${azurerm_resource_group.test.location}"
    availability_set_id   = "${azurerm_availability_set.avset.id}"
    resource_group_name   = "${azurerm_resource_group.test.name}"
    network_interface_ids = ["${element(azurerm_network_interface.test.*.id, count.index)}"]
    vm_size               = "Standard_DS1_v2"

    # Uncomment this line to delete the OS disk automatically when deleting the VM
    # delete_os_disk_on_termination = true

    # Uncomment this line to delete the data disks automatically when deleting the VM
    # delete_data_disks_on_termination = true

    storage_image_reference {
      publisher = "Canonical"
      offer     = "UbuntuServer"
      sku       = "16.04-LTS"
      version   = "latest"
    }

    storage_os_disk {
      name              = "myosdisk${count.index}"
      caching           = "ReadWrite"
      create_option     = "FromImage"
      managed_disk_type = "Standard_LRS"
    }

    # Optional data disks
    storage_data_disk {
      name              = "datadisk_new_${count.index}"
      managed_disk_type = "Standard_LRS"
      create_option     = "Empty"
      lun               = 0
      disk_size_gb      = "1023"
    }

    storage_data_disk {
      name            = "${element(azurerm_managed_disk.test.*.name, count.index)}"
      managed_disk_id = "${element(azurerm_managed_disk.test.*.id, count.index)}"
      create_option   = "Attach"
      lun             = 1
      disk_size_gb    = "${element(azurerm_managed_disk.test.*.disk_size_gb, count.index)}"
    }

    os_profile {
      computer_name  = "hostname"
      admin_username = "testadmin"
      admin_password = "Password1234!"
    }

    os_profile_linux_config {
      disable_password_authentication = false
    }

    tags {
      environment = "staging"
    }
  }
  ```

## <a name="3-initialize-terraform"></a>3. Terraform initialiseren 

De [terraform init opdracht](https://www.terraform.io/docs/commands/init.html) wordt gebruikt voor het initialiseren van een map waarin de configuratiebestanden Terraform - de bestanden die u hebt gemaakt met de vorige secties. U moet altijd worden uitgevoerd de `terraform init` opdracht na het schrijven van een nieuwe Terraform-configuratie. 

> [!TIP]
> De `terraform init` opdracht, wat betekent dat deze kan worden aangeroepen tijdens het opstellen van hetzelfde resultaat idempotent is. Als u in een samenwerkingsomgeving werkt en u denkt dat de configuratiebestanden is gewijzigd, daarom altijd een goed idee om aan te roepen de `terraform init` opdracht voor het uitvoeren van of toepassen van een plan.

Als u wilt initialiseren Terraform, voer de volgende opdracht:

  ```cmd
  terraform init
  ```

  ![Tijdens de initialisatie van Terraform](media/terraform-create-vm-cluster-with-infrastructure/terraform-init.png)

## <a name="4-create-a-terraform-execution-plan"></a>4. Een uitvoeringsplan Terraform maken

De [terraform plan opdracht](https://www.terraform.io/docs/commands/plan.html) gebruikt voor het maken van een plan kan worden uitgevoerd. Voor het genereren van een uitvoeringsplan Terraform aggregeert alle de `.tf` bestanden in de huidige map. 

Als u werkt in een samenwerkingsomgeving waarin de configuratie kan wijzigen tussen het moment dat u het uitvoeringsplan maakt en de tijd u het uitvoeringsplan toepassen, moet u de [terraform plan opdracht-uitvoerparameter](https://www.terraform.io/docs/commands/plan.html#out-path)het uitvoeringsplan opslaan in een bestand. Anders, als u in een omgeving met één persoon werkt, kunt u weglaten de `-out` parameter.

De naam van uw Terraform variabelen-bestand is niet als `terraform.tfvars` en deze niet voldoen aan de `*.auto.tfvars` patroon, moet u het bestand met de naam met opgeven de [terraform plan opdracht var - bestand parameter](https://www.terraform.io/docs/commands/plan.html#var-file-foo) bij het uitvoeren van de `terraform plan`opdracht.

Bij het verwerken van de `terraform plan` opdracht Terraform vernieuwd en bepaalt welke acties nodig zijn voor het bereiken van de gewenste status die is opgegeven in uw configuratiebestanden.

Als u niet opslaan van uw plan kan worden uitgevoerd wilt, voert u de volgende opdracht:

  ```cmd
  terraform plan
  ```

Als u wilt bewaren van uw plan kan worden uitgevoerd, moet de volgende opdracht uitvoeren (vervangt de &lt;pad > aanduiding voor items met de gewenste uitvoerpad):

  ```cmd
  terraform plan -out=<path>
  ```

![Maken van een uitvoeringsplan Terraform](media/terraform-create-vm-cluster-with-infrastructure/terraform-plan.png)

## <a name="5-apply-the-terraform-execution-plan"></a>5. Het uitvoeringsplan Terraform toepassen

De laatste stap van deze zelfstudie is met de [terraform opdracht toepassen](https://www.terraform.io/docs/commands/apply.html) toepassen van de set acties die worden gegenereerd door de `terraform plan` opdracht.

Als u toepassen van de meest recente uitvoeringsplan wilt, voert u de volgende opdracht:

  ```cmd
  terraform apply
  ```

Als u toepassen van een eerder opgeslagen uitvoeringsplan wilt, de volgende opdracht uitvoeren (vervangt de &lt;pad > aanduiding voor items met het pad dat het opgeslagen uitvoeringsplan bevat):

  ```cmd
  terraform apply <path>
  ```

![Een uitvoeringsplan Terraform toepassen](media/terraform-create-vm-cluster-with-infrastructure/terraform-apply.png)

## <a name="next-steps"></a>Volgende stappen

- Bladeren in de lijst van [Azure Terraform modules](https://registry.terraform.io/modules/Azure)
- Maak een [virtuele-machineschaalset met Terraform](terraform-create-vm-scaleset-network-disks-hcl.md)