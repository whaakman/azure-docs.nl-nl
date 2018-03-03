---
title: Openbare IP-adres verbruik weergeven in Azure-Stack | Microsoft Docs
description: Beheerders kunnen het verbruik van openbare IP-adressen weergeven in een regio
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 0f77be49-eafe-4886-8c58-a17061e8120f
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/28/2018
ms.author: mabrigg
ms.openlocfilehash: 50bf01d6de6105d3041c6bb88e803f3d110f751d
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/02/2018
---
# <a name="view-public-ip-address-consumption-in-azure-stack"></a>Openbare IP-adres verbruik in Azure Stack weergeven

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

Als de cloudbeheerder van een, kunt u het volgende bekijken:
 - Het aantal openbare IP-adressen die zijn toegewezen aan tenants.
 - Het aantal openbare IP-adressen die nog steeds beschikbaar voor toewijzing zijn.
 - Het percentage van het openbare IP-adressen die zijn toegewezen op die locatie.

De **openbare IP-adres aan de groep gebruik** tegel toont het aantal openbare IP-adressen voor openbare IP-adresgroepen verbruikt. Voor elk IP-adres toont de tegel gebruik voor de tenant IaaS VM instanties, fabric-infrastructuurservices en openbare IP-adresbronnen die expliciet zijn gemaakt door tenants.

Het doel van de tegel is Azure Stack operators geven een beeld van het aantal openbare IP-adressen die op deze locatie wordt gebruikt. Het aantal kan beheerders bepalen of ze weinig op deze resource.

De **openbare IP-adressen** menu-item onder **Tenantbronnen** geeft een lijst van de openbare IP-adressen die zijn *expliciet gemaakt door tenants*. U vindt het menu-item op de **resourceproviders**, **netwerk** deelvenster. Het aantal **gebruikt** openbare IP-adressen op de **openbare IP-adres aan de groep gebruik** tegel is altijd (groter dan) verschilt het nummer op het **openbare IP-adressen** tegel onder  **Tenant Resources**.

## <a name="view-the-public-ip-address-usage-information"></a>Het openbare IP-adres gebruiksgegevens weergeven
Het totale aantal openbare IP-adressen in de regio zijn verbruikt weergeven:

1. Selecteer in de Azure-Stack-beheerdersportal **meer services**onder **servervirtualisatie**, selecteer **resourceproviders**.
2. In de lijst met **Resourceproviders**, selecteer **netwerk**.
3. De **netwerk** deelvenster ziet u de **openbare IP-adres aan de groep gebruik** -tegel in de **overzicht** sectie.

![Netwerk Resource Provider deelvenster](media/azure-stack-viewing-public-ip-address-consumption/image01.png)

De **gebruikt** nummer geeft het aantal openbare IP-adressen toegewezen uit openbare IP-adresgroepen. De **vrije** getal is het aantal openbare IP-van openbare IP adressen-adresgroepen die niet zijn toegewezen en zijn nog steeds beschikbaar. De **% gebruikt** nummer geeft het aantal gebruikt of adressen toegewezen als een percentage van het totale aantal openbare IP-adressen in het openbare IP-adresgroepen op die locatie.

## <a name="view-the-public-ip-addresses-that-were-created-by-tenant-subscriptions"></a>De openbare IP-adressen die zijn gemaakt door tenant-abonnementen weergeven
Selecteer **openbare IP-adressen** onder **Tenantbronnen**. Bekijk de lijst met openbare IP-adressen expliciet gemaakt door tenant-abonnementen in een specifieke regio.

![Tenant openbare IP-adressen](media/azure-stack-viewing-public-ip-address-consumption/image02.png)

U ziet u mogelijk dat sommige openbare IP-adressen die dynamisch toegewezen worden weergegeven in de lijst. Echter, een adres niet is gekoppeld aan deze nog. De bron van het adres is gemaakt in de Resource-Provider van het netwerk, maar nog niet in de netwerkcontroller.

De netwerkcontroller geen een adres toe aan de resource totdat deze verbinding heeft met een interface, een netwerkinterfacekaart (NIC), een load balancer of een virtuele netwerkgateway. Wanneer het openbare IP-adres wordt gebonden aan een interface, wijst de netwerkcontroller is een IP-adres. Het adres weergegeven in de **adres** veld.

## <a name="view-the-public-ip-address-information-summary-table"></a>De openbare IP-adres informatie samenvattingstabel weergeven
In andere gevallen openbare IP-adressen toegewezen om te bepalen of het adres moet worden weergegeven in één lijst of een andere.

| **Toewijzing geval van openbare IP-adres** | **Wordt weergegeven in de samenvatting van gebruik** | **Wordt weergegeven in de lijst met tenant openbare IP-adressen** |
| --- | --- | --- |
| Dynamische openbare IP-adres nog niet toegewezen aan een NIC of een load balancer (tijdelijke) |Nee |Ja |
| Dynamische openbare IP-adres toegewezen aan een NIC of een load balancer. |Ja |Ja |
| Statisch openbaar IP-adres toegewezen aan een tenant-NIC of een load balancer. |Ja |Ja |
| Statisch openbaar IP-adres toegewezen aan een service-eindpunt van de fabric-infrastructuur. |Ja |Nee |
| Openbaar IP-adres is impliciet gemaakt voor IaaS VM-instanties en gebruikt voor de uitgaande NAT in het virtuele netwerk. Deze worden achter de schermen gemaakt wanneer een tenant een VM-instantie maakt zodat virtuele machines informatie uit met het Internet verzenden kunnen. |Ja |Nee |

## <a name="next-steps"></a>Volgende stappen
[Storage-Accounts in Azure-Stack beheren](azure-stack-manage-storage-accounts.md)