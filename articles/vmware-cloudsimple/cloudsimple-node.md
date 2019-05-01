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
ms.openlocfilehash: b3c8fca1dd93f379860cc3b084fbb14d4a0c6380
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/26/2019
ms.locfileid: "64577358"
---
# <a name="cloudsimple-nodes-overview"></a>Overzicht van de knooppunten CloudSimple

Een knooppunt is:

* Een speciale bare-metalcomputers compute host waarop VMware ESXi-hypervisor is geïnstalleerd  
* Een eenheid van computers waarop u kunt aanschaffen of reserveren voor het maken van persoonlijke clouds  
* Beschikbaar voor aankoop of gereserveerd in een regio waar de service CloudSimple beschikbaar is

Knooppunten zijn de bouwstenen van een privécloud.  Voor het maken van een privécloud, moet u minimaal drie knooppunten van de dezelfde SKU.  Als u wilt een privécloud wilt uitbreiden, moet u extra knooppunten toevoegen.  U kunt knooppunten toevoegen aan een bestaand cluster. Of u kunt een nieuw cluster maken met het aanschaffen van knooppunten in de Azure-portal en deze koppelen aan de CloudSimple-service.  Alle knooppunten die zijn aangeschaft, zijn zichtbaar onder de CloudSimple-service.  U kunt een privécloud maken van de aangeschafte knooppunten op CloudSimple-Portal.

## <a name="purchased-nodes"></a>Aangeschafte knooppunten

Aangeschafte knooppunten bieden betalen per gebruik capaciteit. Kopen van knooppunten, kunt u snel uw VMware-cluster op aanvraag kunt schalen. U kunt knooppunten toevoegen of verwijderen van een aangeschafte knooppunt omlaag uw VMware-cluster schalen. Aangeschafte knooppunten zijn op maandbasis in rekening gebracht en verrekend met het abonnement waar ze worden gekocht:

* Als u voor uw Azure-abonnement met creditcard betalen, wordt de kaart wordt onmiddellijk gefactureerd.
* Als u kosten in rekening per factuur gebracht, worden de kosten in rekening gebracht op uw volgende factuur weergegeven.

## <a name="vmware-solution-by-cloudsimple-nodes-sku"></a>VMware-oplossing door CloudSimple knooppunten SKU

De volgende typen knooppunten zijn beschikbaar voor aankoop of reservering.

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

* Meer informatie over het [knooppunten kopen](create-nodes.md)
* Meer informatie over [Private Cloud](cloudsimple-private-cloud.md)