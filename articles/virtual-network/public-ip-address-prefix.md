---
title: Azure openbaar IP-adresvoorvoegsel | Microsoft Docs
description: Meer informatie over wat een Azure openbare IP-adresvoorvoegsel is en hoe kunt dat u voorspelbare openbare IP-adressen toewijzen aan uw resources.
services: virtual-network
documentationcenter: na
author: anavinahar
manager: narayan
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/24/2018
ms.author: anavin
ms.openlocfilehash: 5496be93a5241621cd4dc5e873e4386f8ed6c992
ms.sourcegitcommit: cdf0e37450044f65c33e07aeb6d115819a2bb822
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57195214"
---
# <a name="public-ip-address-prefix"></a>Openbare IP-adresvoorvoegsel

Een openbare IP-adresvoorvoegsel is een gereserveerd bereik met IP-adressen voor uw openbare eindpunten in Azure. Azure wijst een aaneengesloten reeks adressen aan uw abonnement op basis van hoeveel u opgeeft. Als u niet bekend met openbare adressen bent, Zie [openbare IP-adressen.](virtual-network-ip-addresses-overview-arm.md#public-ip-addresses)

Openbare IP-adressen worden toegewezen uit een groep met de adressen in elke Azure-regio. U kunt [downloaden](https://www.microsoft.com/download/details.aspx?id=56519) de lijst met bereiken maakt gebruik van Azure voor elke regio. 40.121.0.0/16 is bijvoorbeeld een van meer dan 100 bereiken in die Azure wordt gebruikt in de regio VS-Oost. Bereik omvat ook de bruikbare adressen van 40.121.0.1 - 40.121.255.254.

U een openbaar IP-adresvoorvoegsel dat in een Azure-regio en een abonnement maken door een naam op te geven en het aantal adressen wilt u het voorvoegsel om op te nemen. Bijvoorbeeld, als u een openbaar IP-adresvoorvoegsel van/28 maakt, toewijst Azure 16-adressen uit een van de bereiken ligt voor u. U weet niet welke bereik Azure wordt toegewezen wanneer u het bereik hebt gemaakt, maar de adressen aaneengesloten zijn. Openbare IP-adresvoorvoegsels hebben een betaling te doen. Zie voor meer informatie, [prijzen van openbare IP-adressen](https://azure.microsoft.com/pricing/details/ip-addresses).

> [!IMPORTANT]
> Openbare IP-voorvoegsel is in een openbare preview in beperkte regio's. U kunt [informatie over wat het betekent Preview](https://azure.microsoft.com/support/legal/preview-supplemental-terms/). Openbaar IP-voorvoegsel is momenteel beschikbaar in: West-Centraal VS, VS-West, VS-West 2, VS-midden, Noord-Europa, West-Europa en Zuidoost-Azië. Ga voor een bijgewerkte lijst met regio's, [Azure-updates](https://azure.microsoft.com/updates/?product=virtual-network).

## <a name="why-create-a-public-ip-address-prefix"></a>Waarom een openbare IP-adresvoorvoegsel maken?

Bij het maken van resources voor openbaar IP-adres toewijzen Azure beschikbaar openbare IP-adres van een van de bereiken die worden gebruikt in een regio. Wanneer Azure wijst het adres toe, u weet wat het adres is, maar pas Azure wijst het adres toe, u niet weet welk adres kan worden toegewezen. Dit kan problemen veroorzaken wanneer, bijvoorbeeld, u of uw zakelijke partners firewallregels instellen waarmee specifieke IP-adressen zijn. Telkens wanneer die u een nieuw openbaar IP-adres aan een resource toewijzen, moet het adres worden toegevoegd aan de firewallregel. Wanneer u adressen aan uw resources in een openbare IP-adresvoorvoegsel toewijzen, moeten firewallregels niet telkens wanneer u een van de adressen worden bijgewerkt omdat het hele bereik kan worden toegevoegd aan een regel.

## <a name="benefits"></a>Voordelen

- U kunt resources voor openbaar IP-adres maken vanuit een bekende bereik.
- U of uw zakelijke partners kunnen firewall-regels maken met bereiken die zijn openbare IP-adressen die u momenteel hebt toegewezen, evenals de adressen die u nog niet hebt toegewezen. Dit elimineert de noodzaak om te wijzigen van firewall-regels, zoals u IP-adressen aan nieuwe resources toewijzen.
- De standaardgrootte van een bereik dat u kunt maken is/28 of 16 IP-adressen.
- Er zijn geen beperkingen over hoeveel bereiken die u kunt maken, er zijn echter beperkingen met betrekking tot het maximum aantal statische openbare IP-adressen die u kunt in een Azure-abonnement hebben. Het aantal bereiken die u maakt, kan niet als gevolg hiervan meer statische openbare IP-adressen dan er in uw abonnement kunt omvatten. Zie voor meer informatie, [Azure-limieten](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).
- De adressen die u maakt met behulp van adressen uit het voorvoegsel kunnen worden toegewezen aan een Azure-resource die u kunt een openbaar IP-adres aan toewijzen.
- U kunt gemakkelijk zien welke IP-adressen die zijn toegewezen en is nog niet toegewezen binnen het bereik.

## <a name="scenarios"></a>Scenario's
U kunt de volgende resources koppelen aan een statisch openbaar IP-adres uit een voorvoegsel:

|Resource|Scenario|Stappen|
|---|---|---|
|Virtuele machines| Koppelen van openbare IP-adressen uit een voorvoegsel aan uw virtuele machines in Azure verminderd als het gaat om opname in de whitelist IP-adressen in een firewall. U kunt gewoon een hele voorvoegsel met één firewallregel whitelist. Wanneer u met virtuele machines in Azure schaalt, kunt u IP-adressen uit hetzelfde voorvoegsel zodat kosten, tijd en beheeroverhead koppelen.| IP-adressen uit een voorvoegsel koppelen aan uw virtuele machine: 1. [Maak een voorvoegsel.](manage-public-ip-address-prefix.md) 2. [Maak een IP-adres van het voorvoegsel.](manage-public-ip-address-prefix.md) 3. [De IP-adres aan de netwerkinterface van de virtuele machine koppelen.](virtual-network-network-interface-addresses.md#add-ip-addresses)
| Load balancers | Koppelen van openbare IP-adressen uit een voorvoegsel aan uw frontend-IP-configuratie of de regel voor uitgaande van een Load Balancer zorgt ervoor dat vereenvoudiging van uw Azure openbare IP-adresruimte. U kunt uw scenario vereenvoudigen door het opschonen van de uitgaande verbindingen naar worden afkomstig is van een reeks aaneengesloten IP-adressen die zijn gedefinieerd met het openbare IP-voorvoegsel. | IP-adressen uit een voorvoegsel koppelen aan uw Load balancer: 1. [Maak een voorvoegsel.](manage-public-ip-address-prefix.md) 2. [Maak een IP-adres van het voorvoegsel.](manage-public-ip-address-prefix.md) 3. Bij het maken van de Load Balancer, selecteert u of het IP-adres gemaakt in stap 2 hierboven als de front-end-IP-adres van de Load Balancer bijwerken. |
| Azure Firewall | U kunt een openbaar IP-adres uit een voorvoegsel voor uitgaande SNAT. Dit betekent dat alle uitgaande virtuele netwerkverkeer wordt omgezet naar de [Azure Firewall](../firewall/overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) openbaar IP-adres. Omdat deze IP afkomstig zijn uit een vooraf vastgestelde voorvoegsel, is het heel eenvoudig tevoren weten wat uw openbare IP-voetafdruk in Azure eruit. | 1. [Maak een voorvoegsel.](manage-public-ip-address-prefix.md) 2. [Maak een IP-adres van het voorvoegsel.](manage-public-ip-address-prefix.md) 3. Wanneer u [implementeren van de Azure-firewall](../firewall/tutorial-firewall-deploy-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json#deploy-the-firewall), moet u het eerder toegewezen IP-adres uit het voorvoegsel selecteren.|

## <a name="constraints"></a>Beperkingen

- U kunt de IP-adressen voor het voorvoegsel niet opgeven. Azure wijst het IP-adressen voor het voorvoegsel, op basis van de grootte die u opgeeft.
- U kunt het bereik niet wijzigen als u het voorvoegsel hebt gemaakt.
- Het bereik is voor IPv4-adressen. Het bereik bevat geen IPv6-adressen.
- Alleen statische openbare IP-adressen die zijn gemaakt met de standaard-SKU kunnen worden toegewezen uit het voorvoegsel van het bereik. Zie voor meer informatie over openbare IP-adres van SKU's [openbaar IP-adres](virtual-network-ip-addresses-overview-arm.md#public-ip-addresses).
- Adressen uit het bereik kunnen alleen worden toegewezen aan Azure Resource Manager-resources. Adressen kunnen niet worden toegewezen aan resources in het klassieke implementatiemodel.
- Alle openbare IP-adressen die zijn gemaakt op basis van het voorvoegsel moeten aanwezig zijn in de dezelfde Azure-regio en abonnement bevinden als het voorvoegsel en moeten worden toegewezen aan resources in dezelfde regio en hetzelfde abonnement.
- U kunt een voorvoegsel niet verwijderen als adressen binnen het worden toegewezen aan het openbare IP-adres-resources die zijn gekoppeld aan een resource. Alle openbare IP-adres-resources die zijn toegewezen IP-adressen uit het voorvoegsel eerst ontkoppelen.


## <a name="next-steps"></a>Volgende stappen

- [Maak](manage-public-ip-address-prefix.md) een openbare IP-adresvoorvoegsel
