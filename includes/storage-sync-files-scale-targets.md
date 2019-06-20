---
title: bestand opnemen
description: bestand opnemen
services: storage
author: wmgries
ms.service: storage
ms.topic: include
ms.date: 05/05/2019
ms.author: wgries
ms.custom: include file
ms.openlocfilehash: 2614c9290bf31813d59ee753a31622bccf0682b8
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67176242"
---
| Resource | Doel | Vaste limiet |
|----------|--------------|------------|
| Opslagsynchronisatieservices per regio | 20 Opslagsynchronisatieservices | Ja |
| Synchronisatiegroepen per Opslagsynchronisatieservice | 100 synchronisatiegroepen | Ja |
| Geregistreerde servers per Opslagsynchronisatieservice | 99 servers | Ja |
| Cloudeindpunten per groep voor synchronisatie | 1 cloudeindpunt | Ja |
| Servereindpunten per groep voor synchronisatie | 50 servereindpunten | Nee |
| Servereindpunten per server | 30 servereindpunten | Ja |
| -Bestandssysteem (mappen en bestanden) per groep voor synchronisatie | 25 miljoen objecten | Nee |
| Maximum aantal bestandssysteemobjecten (mappen en bestanden) in een map | 1 miljoen objecten | Ja |
| Maximale grootte van object (mappen en bestanden) security descriptor | 64 KiB | Ja |
| Bestandsgrootte | 100 GiB | Nee |
| Minimale bestandsgrootte voor een bestand in tiers worden verdeeld | 64 KiB | Ja |
| Gelijktijdige synchronisatiesessies | V4-agent en hoger: De limiet is afhankelijk van beschikbare systeemresources. <BR> V3-agent: Twee ActiveSync-sessies per processor- of maximaal acht ActiveSync sessies per server. | Ja

> [!Note]  
> Een Azure File Sync-eindpunt kunt omhoog schalen naar de grootte van een Azure-bestandsshare. Als de Azure file share-limiet is bereikt, kunnen synchroniseren is niet uitgevoerd.