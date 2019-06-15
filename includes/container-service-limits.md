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
ms.openlocfilehash: a2729af6a689daa551fc01f585324d53a8770a9b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66142998"
---
| Resource | Standaardlimiet |
| --- | :--- |
| Maximum aantal clusters per abonnement | 100 |
| Maximum aantal knooppunten per cluster | 100 |
| Maximale schillen per knooppunt: [basisnetwerken][basic-networking] met Kubenet | 110 |
| Maximale schillen per knooppunt: [Geavanceerde netwerken] [ advanced-networking] met Azure Container-VPN-Interface | Implementatie van Azure CLI: 30<sup>1</sup><br />Azure Resource Manager-sjabloon: 30<sup>1</sup><br />Portal-implementatie: 30 |

<sup>1</sup>wanneer u een cluster Azure Kubernetes Service (AKS) met de Azure CLI of een Resource Manager-sjabloon implementeert, deze waarde kan worden geconfigureerd maximaal 250 schillen per knooppunt. U kunt maximaal schillen per knooppunt niet configureren nadat u al een AKS-cluster hebt geïmplementeerd, of als u een cluster implementeren met behulp van de Azure-portal.<br />

<!-- LINKS - Internal -->
[basic-networking]: ../articles/aks/concepts-network.md#kubenet-basic-networking
[advanced-networking]: ../articles/aks/concepts-network.md#azure-cni-advanced-networking

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
