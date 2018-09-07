---
title: Verbruik van openbare IP-adressen weergeven in Azure Stack | Microsoft Docs
description: Beheerders kunnen het verbruik van openbare IP-adressen weergeven in een regio
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 0f77be49-eafe-4886-8c58-a17061e8120f
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/05/2018
ms.author: mabrigg
ms.openlocfilehash: 0bd4ab27384f8eb31aa520cbff58c71ae58bd6f7
ms.sourcegitcommit: d211f1d24c669b459a3910761b5cacb4b4f46ac9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/06/2018
ms.locfileid: "44022930"
---
# <a name="view-public-ip-address-consumption-in-azure-stack"></a>Verbruik van openbare IP-adressen weergeven in Azure Stack

*Is van toepassing op: geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

Als de cloudbeheerder van een, kunt u het volgende bekijken:
 - Het aantal openbare IP-adressen die zijn toegewezen aan tenants.
 - Het aantal openbare IP-adressen die nog steeds beschikbaar voor toewijzing zijn.
 - Het percentage van de openbare IP-adressen die zijn toegewezen op die locatie.

De **openbare IP-adresgroepen gebruik** tegel ziet u het aantal openbare IP-adressen die worden gebruikt voor openbare IP-adresgroepen. Voor elk IP-adres ziet de tegel u gebruik voor de tenant-IaaS-VM-exemplaren, fabric infrastructuurservices en openbare IP-adres-resources die expliciet zijn gemaakt door tenants.

Het doel van de tegel is Azure Stack-operators geven een beeld van het aantal openbare IP-adressen die op deze locatie wordt gebruikt. Het aantal kan beheerders bepalen of ze worden uitgevoerd met lage voor deze resource.

De **openbare IP-adressen** menu-item onder **Tenantbronnen** geeft een lijst van de openbare IP-adressen die zijn *expliciet gemaakt door tenants*. U vindt het menu-item op de **resourceproviders**, **netwerk** deelvenster. Het aantal **gebruikt** openbare IP-adressen op de **openbare IP-adresgroepen gebruik** tegel verschilt altijd van (groter dan) het aantal op de **openbare IP-adressen** tegel onder  **Resources voor de tenantsleutel**.

## <a name="view-the-public-ip-address-usage-information"></a>Het openbare IP-adres informatie over het gebruik weergeven
Het totale aantal openbare IP-adressen die zijn verbruikt in de regio weergeven:

1. Selecteer in de Azure Stack-beheerdersportal **alle services**. Klik vervolgens onder de **beheer** categorie selecteren **netwerk**.
1. De **netwerk** deelvenster wordt weergegeven de **openbare IP-adresgroepen gebruik** tegel de **overzicht** sectie.

![Deelvenster voor netwerk-Resourceprovider](media/azure-stack-viewing-public-ip-address-consumption/image01.png)

De **gebruikt** getal geeft het aantal toegewezen openbare IP-adressen van openbare IP-adresgroepen. De **gratis** getal geeft het aantal openbare IP-van openbare IP adressen-adresgroepen die niet zijn toegewezen en zijn nog steeds beschikbaar. De **% gebruikt** getal geeft het aantal gebruikt of adressen toegewezen als een percentage van het totale aantal openbare IP-adressen in het openbare IP-adresgroepen op die locatie.

## <a name="view-the-public-ip-addresses-that-were-created-by-tenant-subscriptions"></a>Het openbare IP-adressen die zijn gemaakt door tenant-abonnementen weergeven
Selecteer **openbare IP-adressen** onder **Tenantbronnen**. Bekijk de lijst met openbare IP-adressen die expliciet zijn gemaakt door tenant-abonnementen in een bepaalde regio.

![Openbare IP-adressen van tenant](media/azure-stack-viewing-public-ip-address-consumption/image02.png)

U ziet u mogelijk dat bepaalde openbare IP-adressen die zijn dynamisch toegewezen worden weergegeven in de lijst. Echter, een adres niet is gekoppeld aan deze nog. De bron van het adres is gemaakt in de Netwerkresourceprovider, maar nog niet in de netwerkcontroller.

De netwerkcontroller biedt een adres niet toewijzen aan de resource totdat deze is gekoppeld aan een interface, een netwerkinterfacekaart (NIC), een load balancer of een virtuele netwerkgateway. Wanneer het openbare IP-adres wordt gebonden aan een interface, wijst een IP-adres toe door de netwerkcontroller. Het adres wordt weergegeven in de **adres** veld.

## <a name="view-the-public-ip-address-information-summary-table"></a>De openbare IP-adres informatie samenvattingstabel weergeven
In andere gevallen zijn openbare IP-adressen toegewezen om te bepalen of het adres wordt weergegeven in een lijst of een andere.

| **Openbare IP-adres toewijzing van aanvraag** | **Wordt weergegeven in de samenvatting van gebruik** | **Wordt weergegeven in de lijst met tenant openbare IP-adressen** |
| --- | --- | --- |
| Dynamische openbare IP-adres is nog niet toegewezen aan een NIC of een load balancer (tijdelijke) |Nee |Ja |
| Dynamische openbare IP-adres toegewezen aan een NIC of load balancer. |Ja |Ja |
| Statische openbare IP-adres toegewezen aan een tenant-NIC of load balancer. |Ja |Ja |
| Statische openbare IP-adres toegewezen aan een service-eindpunt van de fabric-infrastructuur. |Ja |Nee |
| Openbare IP-adres is impliciet gemaakt voor IaaS VM-exemplaren en gebruikt voor de uitgaande NAT in het virtuele netwerk. Deze worden gemaakt op de achtergrond wanneer een tenant een VM-exemplaar maakt zodat virtuele machines informatie uit met het Internet verzenden kunnen. |Ja |Nee |

## <a name="next-steps"></a>Volgende stappen
[Storage-Accounts in Azure Stack beheren](azure-stack-manage-storage-accounts.md)