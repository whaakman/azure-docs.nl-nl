---
title: Afbeelding van opslag in Azure Container register
description: Informatie over hoe uw Docker-container-installatiekopieën worden opgeslagen in Azure Container register, met inbegrip van beveiliging, redundantie en capaciteit.
services: container-registry
author: mmacy
manager: jeconnoc
ms.service: container-registry
ms.topic: article
ms.date: 03/21/2018
ms.author: marsma
ms.openlocfilehash: 92e60b4213cb80d193a7c35f68b8f9fd099481d7
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
ms.locfileid: "32165092"
---
# <a name="container-image-storage-in-azure-container-registry"></a>Container installatiekopie opslag in Azure Container register

Elke [Basic, Standard en Premium](container-registry-skus.md) Azure container register voordelen van Azure storage geavanceerde functies, zoals versleuteling in rust voor de installatiekopie van gegevensbeveiliging en geografische redundantie voor gegevensbescherming installatiekopie. De volgende secties worden de functies en de ondergrenzen van de installatiekopie van opslag in Azure Container register (ACR).

## <a name="encryption-at-rest"></a>Codering in rust

Alle installatiekopieën van de container in het register zijn versleuteld in rust. Azure automatisch een installatiekopie van een versleuteld voordat deze wordt opgeslagen en ontsleuteld op het moment wanneer de installatiekopie van het pull-u of uw toepassingen en services.

## <a name="geo-redundant-storage"></a>Geografisch redundante opslag

Azure maakt gebruik van een schema geografisch redundante opslag ter bescherming tegen verlies van uw afbeeldingen container. Azure Container register repliceert automatisch uw container installatiekopieën naar meerdere geografisch verafgelegen datacenters hun gegevensverlies voorkomen in geval van een regionale optreedt.

## <a name="geo-replication"></a>Geo-replicatie

Voor scenario's vereisen nog meer zekerheid voor hoge beschikbaarheid, kunt u overwegen de [geo-replicatie](container-registry-geo-replication.md) functie van Premium registers. Geo-replicatie kunt beschermen tegen het verlies van toegang aan het register in geval van een *totale* regionale mislukt, niet alleen een optreedt. Geo-replicatie biedt andere voordelen, te, zoals de installatiekopie van het netwerk wordt gesloten opslag voor sneller pushes en ophaalt in gedistribueerde scenario's voor ontwikkeling of distributie.

## <a name="image-limits"></a>Limieten voor de installatiekopie

De volgende tabel beschrijft de container installatiekopie en opslaglimieten voor Azure-container registers.

| Resource | Limiet |
| -------- | :---- |
| Opslagplaatsen | Geen limiet |
| Installatiekopieën | Geen limiet |
| Lagen | Geen limiet |
| Tags | Geen limiet|
| Storage | 5 TB |

Zeer groot aantal opslagplaatsen en tags kunnen invloed hebben op de prestaties van het register. Ongebruikte opslagplaatsen, labels en afbeeldingen periodiek verwijderen met behulp van de [Azure CLI](/cli/azure/acr), de ACR [REST-API](/rest/api/containerregistry/), of de [Azure-portal] [ portal] als onderdeel van uw register onderhoud routine. Registerbronnen zoals opslagplaatsen, afbeeldingen en labels verwijderd *kan niet* worden hersteld na verwijdering.

## <a name="storage-cost"></a>Opslagkosten

Zie voor meer informatie over prijzen [prijzen van Azure Container register][pricing].

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de verschillende Azure-Container register SKU's (Basic, Standard, Premium) [Azure Container register SKU's](container-registry-skus.md).

<!-- IMAGES -->

<!-- LINKS - External -->
[portal]: https://portal.azure.com
[pricing]: http://aka.ms/acr/pricing

<!-- LINKS - Internal -->
