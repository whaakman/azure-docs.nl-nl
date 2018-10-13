---
title: bestand opnemen
description: bestand opnemen
services: storage
author: wmgries
ms.service: storage
ms.topic: include
ms.date: 07/18/2018
ms.author: wgries
ms.custom: include file
ms.openlocfilehash: a29f1c4a625552dd958884c6a172bee470e61ca6
ms.sourcegitcommit: c282021dbc3815aac9f46b6b89c7131659461e49
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/12/2018
ms.locfileid: "49312422"
---
| Resource | Doel | Vaste limiet |
|----------|--------------|------------|
| Opslagsynchronisatieservices per abonnement | 15 Opslagsynchronisatieservices | Nee |
| Synchronisatiegroepen per Opslagsynchronisatieservice | 100 synchronisatiegroepen | Ja |
| Geregistreerde servers per Opslagsynchronisatieservice | 99 servers | Ja |
| Cloudeindpunten per groep voor synchronisatie | 1 cloudeindpunt | Ja |
| Servereindpunten per groep voor synchronisatie | 50 servereindpunten | Nee |
| Servereindpunten per server | servereindpunten 33 99 | Ja, maar is afhankelijk van configuratie (CPU, geheugen, volumes, bestand verloop, aantal bestanden, enz.) |
| Grootte van eindpunt | 4 TiB | Nee |
| -Bestandssysteem (mappen en bestanden) per groep voor synchronisatie | 25 miljoen objecten | Nee |
| Maximum aantal bestandssysteemobjecten (mappen en bestanden) in een map | 200.000 objecten | Ja |
| Lengte van maximaal object (mappen en bestanden) | 255 tekens | Ja |
| Maximale grootte van object (mappen en bestanden) security descriptor | 4 KiB | Ja |
| Bestandsgrootte | 100 GiB | Nee |
| Minimale bestandsgrootte voor een bestand in tiers worden verdeeld | 64 KiB | Ja |
| Gelijktijdige synchronisatiesessies | 2 active synchronisatiesessies per processor- of maximaal 8 active synchronisatiesessies per server | Ja |
