---
title: Quotatypen in Azure Stack | Microsoft Docs
description: Bekijk de verschillende quotatypen beschikbaar voor services en resources in Azure Stack.
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
ms.date: 07/27/2018
ms.author: brenduns
ms.reviewer: xiaofmao
ms.openlocfilehash: 172c32c1f3796ec95336543d7d0ea149e63cfb22
ms.sourcegitcommit: 7ad9db3d5f5fd35cfaa9f0735e8c0187b9c32ab1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/27/2018
ms.locfileid: "39331133"
---
# <a name="quota-types-in-azure-stack"></a>Quotatypen in Azure Stack

*Is van toepassing op: geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

[Quota](azure-stack-plan-offer-quota-overview.md#plans) bepalen de grenzen van de resources die een gebruikersabonnement kunt inrichten of gebruiken. Een quotum kunt bijvoorbeeld een gebruiker kan maximaal vijf virtuele machines maken. Elke resource kan een eigen typen quota's hebben.

## <a name="compute-quota-types"></a>COMPUTE quotatypen

| **Type** | **Standaardwaarde** | **Beschrijving** |
| --- | --- | --- |
| Maximumaantal virtuele machines | 20 | Het maximum aantal virtuele machines die een abonnement op deze locatie maken kunt. |
| Maximumaantal cores van virtuele machine | 50 | Het maximum aantal kernen dat een abonnement op deze locatie maken kunt (bijvoorbeeld een A3-VM heeft vier kerngeheugens). |
| Max. aantal beschikbaarheidssets | 10 | Het maximale aantal beschikbaarheidssets die kunnen worden gemaakt op deze locatie. |
| Hiermee stelt u het maximumaantal virtuele-machineschaalset | 20 | Het maximumaantal schaalsets voor virtuele machines dat kan worden gemaakt op deze locatie. |

## <a name="storage-quota-types"></a>Opslagtypen quotum

| **Item** | **Standaardwaarde** | **Beschrijving** |
| --- | --- | --- |
| Maximale capaciteit (GB) |500 |Totaal voor de opslagcapaciteit die kan worden gebruikt door een abonnement op deze locatie. |
| Totale aantal opslagaccounts |20 |Het maximale aantal opslagaccounts die een abonnement op deze locatie maken kunt. |

> [!NOTE]  
> Het kan maximaal twee uur duren voordat een opslaglimiet wordt afgedwongen.

## <a name="network-quota-types"></a>Netwerk quotatypen

| **Item** | **Standaardwaarde** | **Beschrijving** |
| --- | --- | --- |
| Maximum aantal openbare IP-adressen |50 |Het maximum aantal openbare IP-adressen die een abonnement op deze locatie maken kunt. |
| Maximum aantal virtuele netwerken |50 |Het maximum aantal virtuele netwerken die een abonnement op deze locatie maken kunt. |
| Maximum aantal virtuele netwerkgateways |1 |Het maximum aantal virtuele netwerkgateways (VPN-Gateways) die een abonnement op deze locatie maken kunt. |
| Max-netwerkverbindingen |2 |Het maximale aantal netwerkverbindingen (point-to-point of site-naar-site) die een abonnement via alle gateways voor virtueel netwerk op deze locatie maken kunt. |
| Maximum aantal load balancers |50 |Het maximale aantal load balancers die een abonnement op deze locatie maken kunt. |
| Max. aantal NIC's |100 |Het maximale aantal netwerkinterfaces die een abonnement op deze locatie maken kunt. |
| Maximum aantal netwerkbeveiligingsgroepen |50 |Het maximum aantal netwerkbeveiligingsgroepen die een abonnement op deze locatie maken kunt. |

## <a name="view-an-existing-quota"></a>Een bestaande quotum weergeven

1. Zoek op de standaard-dashboard van de beheerportal de **resourceproviders** tegel.
2. Selecteer de service met de quota die u weergeven wilt, zoals **Compute** of **opslag**.
3. Selecteer **quota**, en selecteer vervolgens de quota die u wilt weergeven.

## <a name="next-steps"></a>Volgende stappen

- [Meer informatie over plannen, aanbiedingen en quota's.](azure-stack-plan-offer-quota-overview.md)
- [Quota's tijdens het maken van een plan maken.](azure-stack-create-plan.md)
