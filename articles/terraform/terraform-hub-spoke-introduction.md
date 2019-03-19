---
title: Maken van een hub- en -spokenetwerktopologie hybride met Terraform in Azure
description: Zelfstudie ter illustratie van de referentiearchitectuur van een hele hybride-netwerk maken in Azure met behulp van Terraform
services: terraform
ms.service: azure
keywords: terraform, hub en spoke, netwerken, hybride netwerken, devops, virtuele machine, azure, vnet-peering, virtueel netwerkapparaat
author: VaijanathB
manager: jeconnoc
ms.author: vaangadi
ms.topic: tutorial
ms.date: 03/01/2019
ms.openlocfilehash: 648369d89bd2b5b08171e1f6f5482c81bfba3c66
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58010335"
---
# <a name="tutorial-create-a-hub-and-spoke-hybrid-network-topology-with-terraform-in-azure"></a>Zelfstudie: Maken van een hub- en -spokenetwerktopologie hybride met Terraform in Azure

In deze reeks zelfstudies ziet u hoe u Terraform gebruiken om te implementeren in Azure een [hub en spoke-netwerktopologie](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke). 

Een hub en spoke-topologie is een manier om workloads te isoleren tijdens het delen van algemene services. Deze services zijn identiteit en beveiliging. De hub is een virtueel netwerk (VNet) die als een centrale verbindingspunt met een on-premises netwerk fungeert. De punten zijn VNets die zijn gekoppeld aan de hub. Gedeelde services worden geïmplementeerd in de hub, terwijl afzonderlijke werkbelastingen zijn geïmplementeerd in spoke-netwerken.

Deze zelfstudie bestaat uit de volgende taken:

> [!div class="checklist"]
> * HCL (HashiCorp Language) gebruiken voor het indelen van hub en spoke hybride referentie-architectuur netwerkbronnen
> * Netwerk van de hub apparaat-resources maken met behulp van Terraform
> * Hub-netwerk maken in Azure om te fungeren als gemeenschappelijk tijdstip voor alle resources met behulp van Terraform
> * Afzonderlijke werkbelastingen als knooppunt VNets in Azure maken met behulp van Terraform
> * Gebruik Terraform tot stand brengen van gateways en verbindingen tussen locatie en Azure-netwerken
> * VNet-peerings tot spoke-netwerken maken met behulp van Terraform

## <a name="prerequisites"></a>Vereisten

- **Azure-abonnement**: Als u nog een Azure-abonnement hebt, maakt u een [gratis Azure-account](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) voordat u begint.

- **Terraform installeren en configureren**: Voor het inrichten van VM's en andere infrastructuur in Azure, [Terraform installeren en configureren](/azure/virtual-machines/linux/terraform-install-configure)

## <a name="hub-and-spoke-topology-architecture"></a>Hub en spoke-topologie-architectuur

In de hub en spoke-topologie is de hub een VNet. Het VNet fungeert als een centraal punt van connectiviteit naar uw on-premises netwerk. De spokes of spaken zijn VNets die via peering zijn verbonden met de hub, en die kunnen worden gebruikt om workloads te isoleren. Verkeer stroomt tussen het on-premises datacentrum en de hub via een ExpressRoute- of VPN-gateway-verbinding. De volgende afbeelding ziet u de onderdelen van een hub en spoke-topologie:

![Hub en spoke-topologie-architectuur in Azure](./media/terraform-hub-and-spoke-tutorial-series/hub-spoke-architecture.png)

## <a name="benefits-of-the-hub-and-spoke-topology"></a>Voordelen van de hub en spoke-topologie

Een hub en spoke-netwerktopologie is een manier om workloads te isoleren tijdens het delen van algemene services. Deze services zijn identiteit en beveiliging. De hub is een VNet die als een centrale verbindingspunt met een on-premises netwerk fungeert. De punten zijn VNets die zijn gekoppeld aan de hub. Gedeelde services worden geïmplementeerd in de hub, terwijl afzonderlijke werkbelastingen zijn geïmplementeerd in spoke-netwerken. Hier volgen enkele voordelen van de hub en spoke-netwerktopologie:

- **Kostenbesparingen** door het centraliseren van services op één locatie die kunnen worden gedeeld door meerdere werkbelastingen. Deze werkbelastingen omvatten virtuele netwerkapparaten en DNS-servers.
- **Limieten in abonnementen overwinnen** door peering van VNets uit verschillende abonnementen naar de centrale hub.
- **Scheiding van problemen** tussen centrale IT (SecOps, InfraOps) en workloads (DevOps).

## <a name="typical-uses-for-the-hub-and-spoke-architecture"></a>Wordt doorgaans gebruikt voor de hub en spoke-architectuur

Het wordt doorgaans gebruikt voor een hub en spoke-architectuur zijn onder andere:

- Veel klanten hebben de werkbelastingen die zijn geïmplementeerd in verschillende omgevingen. Deze omgevingen zijn ontwikkelen, testen en productie. In veel gevallen moeten deze werkbelastingen delen van services zoals DNS, -id's, NTP of AD DS. Deze gedeelde services kunnen worden geplaatst in de hub VNet. Op die manier kunnen is elke omgeving geïmplementeerd op een knooppunt, isolatie onderhouden.
- Workloads die geen verbinding met elkaar is vereist, maar toegang tot gedeelde services vereisen.
- Ondernemingen die centrale controle van beveiligingsaspecten vereisen.
- Ondernemingen die gescheiden beheer voor de workloads in elk knooppunt vereist.

## <a name="preview-the-demo-components"></a>Voorbeeld van de demo-onderdelen

Als u met elke zelfstudie in deze reeks werkt, worden verschillende onderdelen worden gedefinieerd in afzonderlijke Terraform-scripts. De demo-architectuur gemaakt en geïmplementeerd bestaat uit de volgende onderdelen:

- **On-premises netwerk**. Een lokaal privénetwerk dat met een organisatie. Voor de hub en spoke-referentiearchitectuur wordt een VNet in Azure gebruikt voor het simuleren van een on-premises netwerk.

- **VPN-apparaat**. Een VPN-apparaat of de service biedt de externe verbinding met de on-premises netwerk. Het VPN-apparaat is mogelijk een toestel hardware of een softwareoplossing. 

- **Hub VNet**. De hub is het middelpunt van connectiviteit met uw on-premises netwerk en een plaats voor het hosten van services. Deze services kunnen worden gebruikt door de andere workloads die in het knooppunt VNets worden gehost.

- **Gatewaysubnet**. De VNet-gateways zijn ondergebracht in hetzelfde subnet bevinden.

- **Knooppunt VNets**. Knooppunten kunnen worden gebruikt om werkbelastingen te isoleren in hun eigen VNets, afzonderlijk beheerd vanaf andere knooppunten. Elke workload kan meerdere lagen bevatten, met meerdere subnetten die zijn verbonden via Azure-load balancers. 

- **VNet-peering**. Twee VNets kunnen worden verbonden met behulp van een peering verbinding. Peeringverbindingen zijn niet-transitieve verbindingen met lage latentie tussen VNets. Na de peering wisselen de VNets verkeer met behulp van de Azure-backbone, zonder een router. In een hub en spoke-netwerktopologie gebruikt VNet-peering verbinding maken met de hub van elk knooppunt. U kunt vnets in de dezelfde regio bevinden, of verschillende regio's.

## <a name="create-the-directory-structure"></a>De directorystructuur maken

Maak de map waarin uw Terraform-configuratiebestanden voor de demo is opgeslagen.

1. Blader naar de [Azure-portal](https://portal.azure.com).

1. Open [Azure Cloud Shell](/azure/cloud-shell/overview). Als u nog geen omgeving hebt geselecteerd, selecteert u **Bash** als uw omgeving.

    ![Cloud Shell-prompt](./media/terraform-common/azure-portal-cloud-shell-button-min.png)

1. Ga naar de map `clouddrive`.

    ```bash
    cd clouddrive
    ```

1. Maak een directory met de naam `hub-spoke`.

    ```bash
    mkdir hub-spoke
    ```

1. Ga naar de nieuwe map:

    ```bash
    cd hub-spoke
    ```

## <a name="declare-the-azure-provider"></a>De Azure-provider declareren

Maak het Terraform-configuratiebestand waarin de Azure-provider wordt gedeclareerd.

1. Open een nieuw bestand met de naam in Cloud Shell `main.tf`.

    ```bash
    code main.tf
    ```

1. Plak de volgende code in de editor:

    ```JSON
    provider "azurerm" {
        version = "~>1.22"
    }
    ```

1. Sla het bestand op en sluit de editor af.

## <a name="create-the-variables-file"></a>Het bestand variabelen maken

Het configuratiebestand Terraform voor algemene variabelen die worden gebruikt in verschillende scripts maken.

1. Open een nieuw bestand met de naam in Cloud Shell `variables.tf`.

    ```bash
    code variables.tf
    ```

1. Plak de volgende code in de editor:

    ```JSON
    variable "location" {
      description = "Location of the network"
      default     = "centralus"
    }
    
    variable "username" {
      description = "Username for Virtual Machines"
      default     = "testadmin"
    }
    
    variable "password" {
      description = "Password for Virtual Machines"
      default     = "Password1234!"
    }
    
    variable "vmsize" {
      description = "Size of the VMs"
      default     = "Standard_DS1_v2"
    }
    ```

1. Sla het bestand op en sluit de editor af.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"] 
> [On-premises virtueel netwerk maken met Terraform in Azure](./terraform-hub-spoke-on-prem.md)
