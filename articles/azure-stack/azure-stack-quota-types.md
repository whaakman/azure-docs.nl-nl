---
title: Quotatypen in Azure Stack | Microsoft Docs
description: Bekijk de verschillende quotatypen beschikbaar voor services en resources in Azure Stack.
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/15/2018
ms.author: sethm
ms.reviewer: xiaofmao
ms.openlocfilehash: 17326fa60160e084d4c30347b1a765d1f80d01f5
ms.sourcegitcommit: a4e4e0236197544569a0a7e34c1c20d071774dd6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2018
ms.locfileid: "51711528"
---
# <a name="quota-types-in-azure-stack"></a>Quotatypen in Azure Stack

*Is van toepassing op: geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

[Quota](azure-stack-plan-offer-quota-overview.md#plans) bepalen de grenzen van de resources die een gebruikersabonnement kunt inrichten of gebruiken. Een quotum kunt bijvoorbeeld een gebruiker kan maximaal vijf virtuele machines maken. Elke resource kan een eigen typen quota's hebben.

## <a name="compute-quota-types"></a>COMPUTE quotatypen 
| **Type** | **Standaardwaarde** | **Beschrijving** |
| --- | --- | --- |
| Maximum aantal virtuele machines | 50 | Het maximum aantal virtuele machines die een abonnement op deze locatie maken kunt. |
| Maximum aantal cores van virtuele machine | 100 | Het maximum aantal kernen dat een abonnement op deze locatie maken kunt (bijvoorbeeld een A3-VM heeft vier kerngeheugens). |
| Maximum aantal beschikbaarheidssets | 10 | Het maximale aantal beschikbaarheidssets die kunnen worden gemaakt op deze locatie. |
| Hiermee stelt u het maximumaantal virtuele-machineschaalset | 100 | Het maximumaantal schaalsets voor virtuele machines dat kan worden gemaakt op deze locatie. |
| Maximale capaciteit (in GB) van de standard-beheerde schijven | 2048 | De maximale capaciteit van standard beheerde schijven die kunnen worden gemaakt op deze locatie. |
| Maximale capaciteit (in GB) van de premium-beheerde schijf | 2048 | De maximale capaciteit van premium beheerde schijven die kunnen worden gemaakt op deze locatie. |

## <a name="storage-quota-types"></a>Opslagtypen quotum 
| **Item** | **Standaardwaarde** | **Beschrijving** |
| --- | --- | --- |
| Maximale capaciteit (GB) |2048 |Totaal voor de opslagcapaciteit die kan worden gebruikt door een abonnement op deze locatie. |
| Totale aantal opslagaccounts |20 |Het maximale aantal opslagaccounts die een abonnement op deze locatie maken kunt. |

> [!NOTE]  
> Het kan maximaal twee uur duren voordat een opslaglimiet wordt afgedwongen.


## <a name="network-quota-types"></a>Netwerk quotatypen
| **Item** | **Standaardwaarde** | **Beschrijving** |
| --- | --- | --- |
| Maximum aantal openbare IP-adressen |50 |Het maximum aantal openbare IP-adressen die een abonnement op deze locatie maken kunt. |
| Maximale virtuele netwerken |50 |Het maximum aantal virtuele netwerken die een abonnement op deze locatie maken kunt. |
| Maximale virtuele netwerkgateways |1 |Het maximum aantal virtuele netwerkgateways (VPN-Gateways) die een abonnement op deze locatie maken kunt. |
| Maximale netwerkverbindingen |2 |Het maximale aantal netwerkverbindingen (point-to-point of site-naar-site) die een abonnement via alle gateways voor virtueel netwerk op deze locatie maken kunt. |
| Maximale load balancers |50 |Het maximale aantal load balancers die een abonnement op deze locatie maken kunt. |
| Maximum aantal NIC 's |100 |Het maximale aantal netwerkinterfaces die een abonnement op deze locatie maken kunt. |
| Maximum aantal netwerkbeveiligingsgroepen |50 |Het maximum aantal netwerkbeveiligingsgroepen die een abonnement op deze locatie maken kunt. |

## <a name="view-an-existing-quota"></a>Een bestaande quotum weergeven
1. Zoek op de standaard-dashboard van de beheerportal de **resourceproviders** tegel.
2. Selecteer de service met de quota die u weergeven wilt, zoals **Compute** of **opslag**.
3. Selecteer **quota**, en selecteer vervolgens de quota die u wilt weergeven.


## <a name="edit-a-quota"></a>Een quotum bewerken  
U kunt kiezen om de oorspronkelijke configuratie van een quotum in plaats van bewerken [met behulp van een aanvullende plan](create-add-on-plan.md). Wanneer u een quotum hebt bewerkt, de nieuwe configuratie automatisch is van toepassing globaal op alle abonnementen die gebruikmaken van quotum en alle bestaande abonnementen die gebruikmaken van deze abonnementen. Het bewerken van een quotum is anders dan wanneer u een aanvullende plan gebruiken voor een gewijzigde quotum, die een gebruiker ervoor kiest om u te abonneren op. 

### <a name="to-edit-a-quota"></a>Een quotum bewerken  
1. Zoek op de standaard-dashboard van de beheerportal de **resourceproviders** tegel.
2. Selecteer de service met de quota die u wijzigen wilt, zoals **Compute**, **netwerk**, of **opslag**.
3. Selecteer vervolgens **quota**, en selecteer vervolgens de quota die u wilt wijzigen.
4. Op de **quota instellen** deelvenster, bewerk de waarden en selecteer vervolgens **opslaan**. 

De nieuwe waarden voor het quotum van toepassing op alle abonnementen die gebruikmaken van het gewijzigde quotum en voor alle bestaande abonnementen die gebruikmaken van deze plannen. 



## <a name="next-steps"></a>Volgende stappen

- [Meer informatie over plannen, aanbiedingen en quota's.](azure-stack-plan-offer-quota-overview.md)
- [Quota's tijdens het maken van een plan maken.](azure-stack-create-plan.md)
