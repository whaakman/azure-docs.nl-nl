---
title: bestand opnemen
description: bestand opnemen
services: container-service
author: dlepow
ms.service: container-service
ms.topic: include
ms.date: 10/11/2018
ms.author: danlep
ms.custom: include file
ms.openlocfilehash: 4251f379c517d5ccfd0430987e3d5280208590ff
ms.sourcegitcommit: 3a7c1688d1f64ff7f1e68ec4bb799ba8a29a04a8
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/17/2018
ms.locfileid: "49400397"
---
| Resource | Standaardlimiet |
| --- | :--- |
| Maximaal aantal clusters per abonnement | 100 |
| Maximum aantal knooppunten per cluster | 100 |
| Maximaal aantal pods per knooppunt: [basisnetwerken][basic-networking] met Kubenet | 110 |
| Maximaal aantal pods per knooppunt [geavanceerde netwerken][advanced-networking] met Azure CNI | Implementatie van Azure CLI: 30<sup>1</sup><br />Resource Manager-sjabloon: 30<sup>1</sup><br />Portal-implementatie: 30 |

<sup>1</sup> Als u een AKS-cluster implementeert met behulp van de Azure CLI of een Resource Manager-sjabloon, kan deze waarde worden geconfigureerd tot maximaal **110 pods per knooppunt**. U kunt geen maximum aantal pods per node configureren als u het AKS-cluster al hebt geïmplementeerd, of als u een cluster met behulp van de Azure-portal hebt geïmplementeerd.<br />

<!-- LINKS - Internal -->
[basic-networking]: ../articles/aks/concepts-network.md#basic-networking
[advanced-networking]: ../articles/aks/concepts-network.md#advanced-networking

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
