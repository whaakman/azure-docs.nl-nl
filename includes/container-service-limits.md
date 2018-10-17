---
title: bestand opnemen
description: bestand opnemen
services: container-service
author: dlepow
ms.service: container-service
ms.topic: include
ms.date: 08/31/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 71294824bd3dd5215c388cfcd44382c7eee123ad
ms.sourcegitcommit: 67abaa44871ab98770b22b29d899ff2f396bdae3
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/08/2018
ms.locfileid: "48874017"
---
| Resource | Standaardlimiet |
| --- | :--- |
| Maximaal aantal clusters per abonnement | 100 |
| Maximum aantal knooppunten per cluster | 100 |
| Maximaal aantal pods per knooppunt: [basisnetwerken][basic-networking] met Kubenet | 110 |
| Maximaal aantal pods per knooppunt [geavanceerde netwerken][advanced-networking] met Azure CNI | Implementatie van Azure CLI: 30<sup>1</sup><br />Resource Manager-sjabloon: 30<sup>1</sup><br />Portal-implementatie: 30 |

<sup>1</sup> Deze waarde kan worden geconfigureerd tijdens het implementeren van het cluster wanneer een AKS-cluster wordt geïmplementeerd met behulp van de Azure CLI of een Resource Manager-sjabloon.<br />

<!-- LINKS - Internal -->
[basic-networking]: ../articles/aks/networking-overview.md#basic-networking
[advanced-networking]: ../articles/aks/networking-overview.md#advanced-networking

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
