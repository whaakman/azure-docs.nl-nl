---
title: Overzicht van de knooppunten voor VMware-oplossing door CloudSimple - Azure
description: Meer informatie over CloudSimple knooppunten en concepten.
author: sharaths-cs
ms.author: dikamath
ms.date: 04/10/2019
ms.topic: article
ms.service: vmware
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: fb82e31d58d9955efc3b147eccf2b82b8768aeee
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/17/2019
ms.locfileid: "67165806"
---
# <a name="cloudsimple-nodes-overview"></a>Overzicht van de knooppunten CloudSimple

Een knooppunt is:

* Een speciale bare-metalcomputers compute host waarop VMware ESXi-hypervisor is geïnstalleerd  
* Een eenheid van computers waarop u kunt inrichten of reserveren voor het maken van persoonlijke clouds  
* Beschikbaar in te richten of te reserveren in een regio waar de service CloudSimple beschikbaar is

Knooppunten zijn de bouwstenen van een privécloud.  Voor het maken van een privécloud, moet u minimaal drie knooppunten van de dezelfde SKU.  Als u wilt een privécloud wilt uitbreiden, moet u extra knooppunten toevoegen.  U kunt knooppunten toevoegen aan een bestaand cluster. Of u kunt een nieuw cluster maken met het inrichten van knooppunten in de Azure-portal en deze koppelen aan de CloudSimple-service.  Alle knooppunten die ingericht zijn zichtbaar in de CloudSimple-service.  U kunt een privécloud maken van de ingerichte knooppunten op CloudSimple-Portal.

## <a name="provisioned-nodes"></a>Ingerichte knooppunten

Ingerichte knooppunten bieden betalen per gebruik capaciteit. Knooppunten wordt ingericht, kunt u snel uw VMware-cluster op aanvraag kunt schalen. U kunt knooppunten toevoegen of verwijderen van een ingerichte knooppunt omlaag uw VMware-cluster schalen. ingerichte knooppunten zijn op maandbasis in rekening gebracht en verrekend met het abonnement waar ze zijn ingericht:

* Als u voor uw Azure-abonnement met creditcard betalen, wordt de kaart wordt onmiddellijk gefactureerd.
* Als u kosten in rekening per factuur gebracht, worden de kosten in rekening gebracht op uw volgende factuur weergegeven.

## <a name="vmware-solution-by-cloudsimple-nodes-sku"></a>VMware-oplossing door CloudSimple knooppunten SKU

De volgende typen knooppunten zijn beschikbaar voor het inrichten of reservering.

| SKU | CS28 - knooppunt | CS36 - knooppunt |
|-----|-------------|-------------|
| CPU | 2x2.2 GHz, 28 kernen (56 HT) | 2x2.3 GHz, 36 kernen (72 HT) |
| RAM | 256 GB | 512 GB |
| Cache Disk |  1.6 TB NVMe | 3.2-TB NVMe |
| Capaciteit van schijf | 5.625 TB onbewerkte | 11,25 TB onbewerkte |
| Opslagtype | Alle Flash | Alle Flash |

## <a name="limits"></a>Limits

Het volgende knooppunt-limieten gelden voor privéclouds.

| Resource | Limiet |
|----------|-------|
| Minimum aantal knooppunten aan een privécloud maken | 3 |
| Maximum aantal knooppunten in een Cluster in een privécloud | 16 |
| Maximum aantal knooppunten in een privécloud | 64 |
| Minimum aantal knooppunten in een nieuw Cluster | 3 |

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over het [knooppunten in te richten](create-nodes.md)
* Meer informatie over [Private Cloud](cloudsimple-private-cloud.md)