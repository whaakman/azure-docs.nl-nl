---
title: bestand opnemen
description: bestand opnemen
services: container-service
author: mmacy
ms.service: container-service
ms.topic: include
ms.date: 08/31/2018
ms.author: marsma
ms.custom: include file
ms.openlocfilehash: 1e8913d31677f3da9b6eb9d2216d8d9ec8b186b4
ms.sourcegitcommit: 31241b7ef35c37749b4261644adf1f5a029b2b8e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/04/2018
ms.locfileid: "43666899"
---
| Resource | Standaardlimiet |
| --- | :--- |
| Maximaal aantal clusters per abonnement | 100 |
| Maximum aantal knooppunten per cluster | 100 |
| Maximaal aantal pods per knooppunt: [basisnetwerken][basic-networking] met Kubenet | 110 |
| Maximaal aantal pods per knooppunt [geavanceerde netwerken][advanced-networking] met Azure CNI | Implementatie van Azure CLI: 110<sup>1</sup><br />Resource Manager-sjabloon: 110<sup>1</sup><br />Portal-implementatie: 30 |

<sup>1</sup> Deze waarde kan worden geconfigureerd tijdens het implementeren van het cluster wanneer een AKS-cluster wordt geïmplementeerd met behulp van de Azure CLI of een Resource Manager-sjabloon.<br />

<!-- LINKS - Internal -->
[basic-networking]: ../articles/aks/networking-overview.md#basic-networking
[advanced-networking]: ../articles/aks/networking-overview.md#advanced-networking

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
