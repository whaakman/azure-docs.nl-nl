---
title: Overzicht van knoop punten voor VMware-oplossing door CloudSimple-Azure
description: Meer informatie over CloudSimple-knoop punten en-concepten.
author: sharaths-cs
ms.author: dikamath
ms.date: 04/10/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 346bd046810ebae5142bc23400419857000d0c8e
ms.sourcegitcommit: c8a102b9f76f355556b03b62f3c79dc5e3bae305
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2019
ms.locfileid: "68812571"
---
# <a name="cloudsimple-nodes-overview"></a>Overzicht van CloudSimple-knoop punten

Een knoop punt is:

* Een toegewezen bare-metal Compute-host waarop VMware ESXi Hyper Visor is geïnstalleerd  
* Een reken eenheid die u kunt inrichten of reserveren voor het maken van persoonlijke Clouds  
* Beschikbaar voor het inrichten of reserveren van een regio waar de CloudSimple-service beschikbaar is

Knoop punten zijn bouw stenen van een privécloud.  Als u een privécloud wilt maken, hebt u Mini maal drie knoop punten van dezelfde SKU nodig.  Als u een privécloud wilt uitbreiden, voegt u extra knoop punten toe.  U kunt knoop punten toevoegen aan een bestaand cluster. U kunt ook een nieuw cluster maken door knoop punten in de Azure Portal in te richten en deze te koppelen aan de CloudSimple-service.  Alle geconfigureerde knoop punten zijn zichtbaar in de CloudSimple-service.  U kunt een privécloud maken op basis van de ingerichte knoop punten op de CloudSimple-Portal.

## <a name="provisioned-nodes"></a>Ingerichte knoop punten

Ingerichte knoop punten bieden een betalen naar gebruik-capaciteit. Met het inrichten van knoop punten kunt u uw VMware-cluster op aanvraag snel schalen. U kunt indien nodig knoop punten toevoegen of een ingericht knoop punt verwijderen om uw VMware-cluster omlaag te schalen. ingerichte knoop punten worden maandelijks gefactureerd en worden in rekening gebracht voor het abonnement waar ze zijn ingericht:

* Als u betaalt voor uw Azure-abonnement per credit card, wordt de kaart onmiddellijk gefactureerd.
* Als u per factuur factureert, worden de kosten op uw volgende factuur weer gegeven.

## <a name="vmware-solution-by-cloudsimple-nodes-sku"></a>VMware-oplossing op CloudSimple knoop punten SKU

De volgende typen knoop punten zijn beschikbaar voor inrichting of reserve ring.

| SKU | CS28-knoop punt | CS36-knoop punt |
|-----|-------------|-------------|
| CPU | 2x 2,2 GHz, 28 kernen (56 HT) | 2x 2,3 GHz, 36 kern geheugens (72 HT) |
| RAM | 256 GB | 512 GB |
| Cache schijf |  1,6-TB NVMe | 3,2-TB NVMe |
| Capaciteits schijf | 5,625 TB onbewerkt | 11,25 TB onbewerkt |
| Opslag type | Alle Flash | Alle Flash |

## <a name="limits"></a>Limieten

De volgende limieten voor knoop punten zijn van toepassing op persoonlijke Clouds.

| Resource | Limiet |
|----------|-------|
| Minimum aantal knoop punten voor het maken van een privécloud | 3 |
| Maximum aantal knoop punten in een cluster in een privécloud | 16 |
| Maximum aantal knoop punten in een privécloud | 64 |
| Minimum aantal knoop punten op een nieuw cluster | 3 |

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over het [inrichten van knoop punten](create-nodes.md)
* Meer informatie over [privécloud](cloudsimple-private-cloud.md)