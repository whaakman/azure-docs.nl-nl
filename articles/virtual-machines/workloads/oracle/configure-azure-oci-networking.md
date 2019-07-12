---
title: Verbinding maken met Azure ExpressRoute met Oracle-Cloudinfrastructuur | Microsoft Docs
description: Verbinding maken met Azure ExpressRoute met Oracle Cloud-infrastructuur (OCI) FastConnect oplossingen voor cross-cloud Oracle-toepassingen inschakelen
documentationcenter: virtual-machines
author: romitgirdhar
manager: gwallace
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/24/2019
ms.author: rogirdh
ms.openlocfilehash: 671d7c8eb9f10e346b49056e1cc117c9882bb6e8
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67707594"
---
# <a name="set-up-a-direct-interconnection-between-azure-and-oracle-cloud-infrastructure"></a>Instellen van een directe koppeling tussen Azure en Oracle Cloud-infrastructuur  

Maakt een [geïntegreerde ervaring multi-cloud](oracle-oci-overview.md) (preview), Microsoft en Oracle bieden direct onderlinge verbinding tussen Azure en Oracle Cloud-infrastructuur (OCI) via [ExpressRoute](../../../expressroute/expressroute-introduction.md) en [ FastConnect](https://docs.cloud.oracle.com/iaas/Content/Network/Concepts/fastconnectoverview.htm). Klanten kunnen via de ExpressRoute- en FastConnect onderlinge verbinding ervaren met lage latentie, hoge doorvoer, persoonlijke directe verbinding tussen de twee clouds.

> [!IMPORTANT]
> De verbinding tussen Microsoft Azure en OCI is in de previewfase. Om in te schakelen met lage latentie connectiviteit tussen Azure en OCI, moeten de Azure-abonnement en OCI tenants eerst worden toegelaten voor deze mogelijkheid.

De volgende afbeelding toont een overzicht van de onderlinge verbinding:

![Cross-cloud-netwerkverbinding](media/configure-azure-oci-networking/azure-oci-connect.png)

## <a name="prerequisites"></a>Vereisten

* Voor het maken van verbinding tussen Azure en OCI, moet u een actief Azure-abonnement en een actieve OCI tenants hebben.

* Connectiviteit is alleen mogelijk, waarbij een peering Azure ExpressRoute-locatie in de nabijheid of in de dezelfde locatie als de FastConnect OCI is. Zie [beperkingen bekijken](oracle-oci-overview.md#preview-limitations).

* Uw Azure-abonnement moet worden toegelaten voor deze preview-functie. Neem contact op met uw Microsoft-vertegenwoordiger voor deze functie inschakelen op uw abonnement.

* Uw tenants OCI moet worden toegelaten voor deze preview-functie. Neem contact op met uw Oracle-vertegenwoordiger voor meer informatie.

## <a name="configure-direct-connectivity-between-expressroute-and-fastconnect"></a>Directe verbinding tussen de ExpressRoute- en FastConnect configureren

1. Een standard ExpressRoute-circuit in uw Azure-abonnement onder een resourcegroep maken. 
    * Tijdens het maken van de ExpressRoute, kies **Oracle Cloud FastConnect** als de service-provider. Voor het maken van een ExpressRoute-circuit, Zie de [stapsgewijze handleiding](../../../expressroute/expressroute-howto-circuit-portal-resource-manager.md).
    * Een Azure ExpressRoute-circuit biedt gedetailleerde bandbreedte-opties, terwijl FastConnect 1, 2, 5 of 10 Gbps ondersteunt. Het verdient daarom kiest u een van deze overeenkomende bandbreedte-opties onder ExpressRoute.

    ![ExpressRoute-circuit maken](media/configure-azure-oci-networking/exr-create-new.png)
1. Noteert u uw ExpressRoute **servicesleutel**. U moet de sleutel tijdens het configureren van uw circuit FastConnect opgeven.

    ![ExpressRoute-Service-sleutel](media/configure-azure-oci-networking/exr-service-key.png)

    > [!IMPORTANT]
    > U wordt gefactureerd voor ExpressRoute-kosten als het ExpressRoute-circuit is ingericht (zelfs als de **Providerstatus** is **niet ingericht**).

1. Twee privé IP-adresruimten van/30 elke die elkaar niet met uw Azure-netwerk of de virtuele cloudnetwerk OCI IP-adresruimte overlappen reserveren. Er wordt verwezen naar het eerste IP-adresruimte als primaire adresruimte en het tweede IP-adresruimte als secundaire adresruimte. Noteer de adressen die u nodig hebt bij het configureren van uw circuit FastConnect.
1. Maak een Gateway voor dynamische routering (DRG). U moet dit bij het maken van uw circuit FastConnect. Zie voor meer informatie de [dynamische Routeringsgateway](https://docs.cloud.oracle.com/iaas/Content/Network/Tasks/managingDRGs.htm) documentatie.
1. Een circuit FastConnect onder uw Oracle-tenant maken. Zie voor meer informatie de [Oracle-documentatie](https://docs.cloud.oracle.com/iaas/Content/Network/Concepts/azure.htm).
  
    * Selecteer onder configuratie van de FastConnect **Microsoft Azure: ExpressRoute** als de provider.
    * Selecteer de dynamische routering van Gateway die u hebt ingericht in de vorige stap.
    * Selecteer de bandbreedte in te richten. Voor optimale prestaties moet de bandbreedte die overeenkomen met de bandbreedte die is geselecteerd bij het maken van het ExpressRoute-circuit.
    * In **Provider servicesleutel**, plak de sleutel van het ExpressRoute-service.
    * Gebruik het eerste/30, privé-IP-adresruimte gehaald in de vorige stap voor de **primaire IP-adres van BGP** en het tweede/30 privé-IP-adresruimte voor de **secundaire BGP IP** adres.
        * Het eerste bruikbaar adres van de twee bereiken voor de Oracle BGP IP-adres (primaire en secundaire) en het tweede adres toewijzen aan de BGP-IP-adres van de klant (vanuit het oogpunt van het FastConnect). Het eerste IP-adres bruikbaar is het tweede IP-adres in het/30-adresruimte (het eerste IP-adres is gereserveerd door Microsoft).
    * Klik op **Create**.
1. Voltooi de FastConnect koppelen aan virtuele cloudnetwerk onder uw tenant Oracle via dynamische Routeringsgateway, met behulp van de routetabel.
1. Ga naar Azure en zorg ervoor dat de **Providerstatus** voor uw ExpressRoute-circuit is gewijzigd in **ingerichte** en die een peering van het type **Azure privé** is ingericht. Dit is een vereiste voor de volgende stappen uit.

    ![De status van de ExpressRoute-provider](media/configure-azure-oci-networking/exr-provider-status.png)
1. Klik op de **Azure privé** peering. U ziet dat de details van de peering automatisch zijn geconfigureerd op basis van de informatie die u hebt ingevoerd bij het instellen van uw circuit FastConnect.

    ![Instellingen voor persoonlijke peering](media/configure-azure-oci-networking/exr-private-peering.png)

## <a name="connect-virtual-network-to-expressroute"></a>Virtueel netwerk verbinden met ExpressRoute

1. Maak een virtueel netwerk en de gateway van virtueel netwerk, als u dat nog niet gedaan hebt. Zie voor meer informatie, de [stapsgewijze handleiding](../../../expressroute/expressroute-howto-add-gateway-portal-resource-manager.md).
1. De verbinding tussen de gateway van virtueel netwerk en uw ExpressRoute-circuit instellen door het uitvoeren van de [Terraform script](https://github.com/microsoft/azure-oracle/tree/master/InterConnect-2) of door het uitvoeren van de PowerShell-opdracht voor [configureren ExpressRoute FastPath](../../../expressroute/expressroute-howto-linkvnet-arm.md#configure-expressroute-fastpath).

Nadat u de netwerkconfiguratie hebt voltooid, kunt u de geldigheid van uw configuratie controleren door te klikken op **ARP-Records ophalen** en **Get routetabel** onder de particuliere ExpressRoute-peering-blade in de Azure-portal.

## <a name="automation"></a>Automation

Microsoft heeft Terraform-scripts voor het inschakelen van automatische implementatie van de netwerk-interconnect gemaakt. De scripts Terraform moeten zich verifiëren met Azure voordat wordt uitgevoerd, omdat hiervoor voldoende machtigingen voor het Azure-abonnement. Verificatie kan worden uitgevoerd met behulp van een [Azure Active Directory service-principal](../../../active-directory/develop/app-objects-and-service-principals.md#service-principal-object) of met behulp van de Azure CLI. Zie voor meer informatie de [Terraform documentatie](https://www.terraform.io/docs/providers/azurerm/auth/azure_cli.html).

De scripts Terraform en de bijbehorende documentatie om te implementeren, de inter-connect vindt u in deze [GitHub-opslagplaats](https://aka.ms/azureociinterconnecttf).

## <a name="monitoring"></a>Bewaking

Installatie van agents voor beide clouds, u kunt gebruikmaken van Azure [(Netwerkprestatiemeter)](../../../expressroute/how-to-npm.md) voor het bewaken van de prestaties van de end-to-end-netwerk. NPM kunt u gemakkelijk netwerkproblemen te identificeren en deze te elimineren.

## <a name="delete-the-interconnect-link"></a>De koppeling interconnect verwijderen

Als u wilt de interconnect verwijderen, moeten in de gegeven specifieke volgorde de volgende stappen gevolgd. Dit niet doet, resulteert in een status ' mislukt heeft"ExpressRoute-circuit.

1. Verwijder de ExpressRoute-verbinding. Verwijderen van de verbinding door te klikken op de **verwijderen** pictogram op de pagina voor de verbinding. Zie voor meer informatie de [documentatie voor ExpressRoute](../../../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md#delete-a-connection-to-unlink-a-vnet).
1. Verwijder de Oracle-FastConnect uit de Oracle Cloud-Console.
1. Wanneer het circuit Oracle FastConnect is verwijderd, kunt u de Azure ExpressRoute-circuit verwijderen.

Op dit moment is de knop verwijderen en -opheffing proces voltooid.

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over de cross-cloud-verbinding tussen OCI en Azure, de [Oracle-documentatie](https://docs.cloud.oracle.com/iaas/Content/Network/Concepts/azure.htm).
* Gebruik [Terraform scripts](https://aka.ms/azureociinterconnecttf) infrastructuur voor het betreffende Oracle-toepassingen via Azure implementeren en configureren van de netwerk-interconnect. 