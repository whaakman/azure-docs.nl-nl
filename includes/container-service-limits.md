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
ms.openlocfilehash: 23c25953d2f493d2dd799bfd11dbbb69db002d1b
ms.sourcegitcommit: a65b424bdfa019a42f36f1ce7eee9844e493f293
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/04/2019
ms.locfileid: "55736159"
---
| Resource | Standaardlimiet |
| --- | :--- |
| Maximaal aantal clusters per abonnement | 100 |
| Maximum aantal knooppunten per cluster | 100 |
| Maximum aantal pods per knooppunt: [basisnetwerken][basic-networking] met Kubenet | 110 |
| Maximum aantal pods per knooppunt: [geavanceerde netwerken][advanced-networking] met Azure CNI | Implementatie van Azure CLI: 30<sup>1</sup><br />Resource Manager-sjabloon: 30<sup>1</sup><br />Portal-implementatie: 30 |

<sup>1</sup> Als u een AKS-cluster implementeert met behulp van de Azure CLI of een Resource Manager-sjabloon, kan deze waarde worden geconfigureerd tot maximaal **110 pods per knooppunt**. U kunt geen maximum aantal pods per node configureren als u het AKS-cluster al hebt geïmplementeerd, of als u een cluster met behulp van de Azure-portal hebt geïmplementeerd.<br />

<!-- LINKS - Internal -->
[basic-networking]: ../articles/aks/concepts-network.md#kubenet-basic-networking
[advanced-networking]: ../articles/aks/concepts-network.md#azure-cni-advanced-networking

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
