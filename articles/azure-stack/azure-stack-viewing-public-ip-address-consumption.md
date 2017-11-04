---
title: Openbare IP-adres verbruik weergeven in Azure-Stack | Microsoft Docs
description: Beheerders kunnen het verbruik van openbare IP-adressen weergeven in een regio
services: azure-stack
documentationcenter: 
author: ScottNapolitan
manager: darmour
editor: 
ms.assetid: 0f77be49-eafe-4886-8c58-a17061e8120f
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 9/25/2017
ms.author: scottnap
ms.openlocfilehash: 7651565eebf6272f307a4ce4790ca19b41bfa826
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="view-public-ip-address-consumption-in-azure-stack"></a>Openbare IP-adres verbruik in Azure Stack weergeven

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

Als de cloudbeheerder van een, kunt u het aantal openbare IP-adressen die zijn toegewezen aan tenants, het aantal openbare IP-adressen die zijn nog steeds beschikbaar voor toewijzing en het percentage van de openbare IP-adressen die zijn toegewezen op die locatie weergeven.

De **openbare IP-adres aan de groep gebruik** tegel ziet u het totale aantal openbare IP-adressen die moeten zijn geconsumeerd voor alle openbare IP-adresgroepen op de fabric, of deze zijn gebruikt voor de tenant IaaS VM-instanties, fabric-infrastructuur Services of openbare IP-adresbronnen die expliciet zijn gemaakt door tenants.

Het doel van deze tegel is Azure Stack-beheerders geven een beeld krijgt van het totale aantal openbare IP-adressen die op deze locatie moeten zijn geconsumeerd. Hiermee kan beheerders bepalen of ze weinig op deze resource.

Op de **resourceproviders**, **netwerk** blade de **openbare IP-adressen** menu-item onder **Tenantbronnen** bevat alleen de openbare IP-adressen die zijn *expliciet gemaakt door tenants*. Als bijvoorbeeld het aantal **gebruikt** openbare IP-adressen op de **openbare IP-adres aan de groep gebruik** tegel is altijd (groter dan) verschilt het nummer op de **openbare IP-adressen** tegel onder **Tenant Resources**.

## <a name="view-the-public-ip-address-usage-information"></a>Het openbare IP-adres gebruiksgegevens weergeven
Het totale aantal openbare IP-adressen in de regio zijn verbruikt weergeven:

1. Klik in de Azure-Stack-beheerdersportal **meer services**onder **servervirtualisatie**, klikt u op **resourceproviders**.
2. In de lijst met **Resourceproviders**, selecteer **netwerk**.
3. De **netwerk** blade geeft de **openbare IP-adres aan de groep gebruik** -tegel in de **overzicht** sectie.

![Netwerk Resource Provider-blade](media/azure-stack-viewing-public-ip-address-consumption/image01.png)

Houd rekening met de **gebruikt** nummer geeft het aantal openbare IP-adressen van alle openbare IP-adresgroepen op die locatie die zijn toegewezen. De **vrije** getal is het aantal openbare IP-van alle openbare IP adressen-adresgroepen die niet zijn toegewezen en zijn nog steeds beschikbaar. De **% gebruikt** nummer geeft het aantal gebruikt of adressen toegewezen als een percentage van het totale aantal openbare IP-adressen in alle openbare IP-adresgroepen op die locatie.

## <a name="view-the-public-ip-addresses-that-were-created-by-tenant-subscriptions"></a>De openbare IP-adressen die zijn gemaakt door tenant-abonnementen weergeven
Een lijst van openbare IP-adressen die expliciet zijn gemaakt door tenant-abonnementen in een specifieke regio wilt bekijken, klikt u op **openbare IP-adressen** onder **Tenantbronnen**.

![Tenant openbare IP-adressen](media/azure-stack-viewing-public-ip-address-consumption/image02.png)

Mogelijk ziet u dat een aantal openbare IP-adressen die dynamisch toegewezen in de lijst weergegeven, maar een adres die zijn gekoppeld aan deze nog niet hebt. Dit is omdat de bron van het adres is in de Resource-Provider van het netwerk maar niet in de netwerkcontroller nog gemaakt.

De netwerkcontroller biedt een adres niet toewijzen aan deze resource totdat deze daadwerkelijk is gekoppeld aan een interface, een netwerkinterfacekaart (NIC), een load balancer of een virtuele netwerkgateway. Wanneer het openbare IP-adres is gebonden aan een interface en wordt deze weergegeven in de netwerkcontroller wijst een IP-adres aan de **adres** veld.

## <a name="view-the-public-ip-address-information-summary-table"></a>De openbare IP-adres informatie samenvattingstabel weergeven
Er zijn een aantal andere gevallen waarin het openbare IP-adressen zijn toegewezen om te bepalen of het adres moet worden weergegeven in één lijst of een andere.

| **Toewijzing geval van openbare IP-adres** | **Wordt weergegeven in de samenvatting van gebruik** | **Wordt weergegeven in de lijst met tenant openbare IP-adressen** |
| --- | --- | --- |
| Dynamische openbare IP-adres nog niet toegewezen aan een NIC of een load balancer (tijdelijke) |Nee |Ja |
| Dynamische openbare IP-adres toegewezen aan een NIC of een load balancer. |Ja |Ja |
| Statisch openbaar IP-adres toegewezen aan een tenant-NIC of een load balancer. |Ja |Ja |
| Statisch openbaar IP-adres toegewezen aan een service-eindpunt van de fabric-infrastructuur. |Ja |Nee |
| Openbaar IP-adres is impliciet gemaakt voor IaaS VM-instanties en gebruikt voor de uitgaande NAT in het virtuele netwerk. Deze worden achter de schermen gemaakt wanneer een tenant een VM-instantie maakt zodat virtuele machines informatie uit met het Internet verzenden kunnen. |Ja |Nee |

## <a name="next-steps"></a>Volgende stappen
[Storage-Accounts in Azure-Stack beheren](azure-stack-manage-storage-accounts.md)