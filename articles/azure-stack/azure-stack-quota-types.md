---
title: Quotatypen in Azure-Stack | Microsoft Docs
description: Bekijk de voor verschillende quotatypen beschikbaar voor services en bronnen in Azure-Stack.
services: azure-stack
documentationcenter: ''
author: brenduns
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/08/2018
ms.author: brenduns
ms.reviewer: xiaofmao
ms.openlocfilehash: 52d469236d607a145430134c24c4237346dafb4c
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="quota-types-in-azure-stack"></a>Quotatypen in Azure-Stack

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

[Quota's](azure-stack-plan-offer-quota-overview.md#plans) definiëren de grenzen van de resources die een gebruikerabonnement kan inrichten of gebruiken. Een quotum kunt bijvoorbeeld een gebruiker tot vijf virtuele machines maken. Elke bron kan een eigen typen quota's hebben.

## <a name="compute-quota-types"></a>Quotatypen berekenen
| **Type** | **Standaardwaarde** | **Beschrijving** |
| --- | --- | --- |
| Maximumaantal virtuele machines | 20 | Het maximale aantal virtuele machines die een abonnement op deze locatie kan maken. |
| Maximumaantal kernen voor virtuele machine | 50 | Het maximum aantal kernen die een abonnement op deze locatie maken kunt (bijvoorbeeld een A3-VM vier kernen heeft). |
| Hiermee stelt u het maximumaantal beschikbaarheid | 10 | Het maximum aantal beschikbaarheidssets die kunnen worden gemaakt op deze locatie. |
| Hiermee stelt u het maximumaantal virtuele-machineschaalset | 20 | Het maximum aantal virtuele-machineschaalsets die kunnen worden gemaakt op deze locatie. |



## <a name="storage-quota-types"></a>Opslagtypen quotum
| **item** | **Standaardwaarde** | **Beschrijving** |
| --- | --- | --- |
| Maximale capaciteit (GB) |500 |Totale capaciteit die kan worden gebruikt door een abonnement op deze locatie. |
| Totaal aantal storage-accounts |20 |Het maximum aantal opslagaccounts die een abonnement op deze locatie kan maken. |

> [!NOTE]  
> Het kan maximaal twee uur duren voordat een opslaglimiet wordt afgedwongen. 
> 


## <a name="network-quota-types"></a>Quotum netwerktypen
| **item** | **Standaardwaarde** | **Beschrijving** |
| --- | --- | --- |
| Maximum aantal openbare IP-adressen |50 |Het maximum aantal openbare IP-adressen die een abonnement op deze locatie maken kunt. |
| Maximum aantal virtuele netwerken |50 |Het maximum aantal virtuele netwerken die een abonnement op deze locatie kan maken. |
| Maximum aantal virtuele netwerkgateways |1 |Het maximum aantal virtuele netwerkgateways (VPN-Gateways) die een abonnement op deze locatie maken kunt. |
| Maximum aantal netwerkverbindingen |2 |Het maximale aantal netwerkverbindingen (point-to-point of site-naar-site) die een abonnement voor alle virtuele netwerkgateways op deze locatie maken kunt. |
| Maximum aantal load balancers |50 |Het maximum aantal load balancers die een abonnement op deze locatie kan maken. |
| Max. aantal NIC's |100 |Het maximum aantal netwerkinterfaces die een abonnement op deze locatie maken kunt. |
| Maximum aantal netwerkbeveiligingsgroepen |50 |Het maximum aantal netwerkbeveiligingsgroepen die een abonnement op deze locatie maken kunt. |

## <a name="view-an-existing-quota"></a>Een bestaande quotum weergeven
1. Klik op **meer services** > **Resourceproviders**.
2. Selecteer de service met het quotum dat u wilt weergeven.
3. Klik op **quota**, en selecteer de quota die u wilt weergeven.

## <a name="next-steps"></a>Volgende stappen
[Meer informatie over plannen, aanbiedingen en quota's.](azure-stack-plan-offer-quota-overview.md)

[Quota's maken tijdens het maken van een plan.](azure-stack-create-plan.md)
