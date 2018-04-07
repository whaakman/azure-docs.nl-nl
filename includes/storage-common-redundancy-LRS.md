---
title: bestand opnemen
description: bestand opnemen
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 03/26/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: 58d81bd4c1ce4b3af91a335039f62df08b340576
ms.sourcegitcommit: 3a4ebcb58192f5bf7969482393090cb356294399
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/06/2018
---
Lokaal redundante opslag (LRS) is zodanig ontworpen dat ten minste 99.999999999% (11 9 van) de duurzaamheid van objecten gedurende een bepaald jaar door het repliceren van uw gegevens binnen een schaaleenheid opslag. Een schaaleenheid opslag wordt gehost in een datacentrum in de regio waarin u uw opslagaccount hebt gemaakt. Een schrijfaanvraag naar een opslagaccount LRS retourneert is pas mogelijk nadat de gegevens is geschreven naar alle replica's. Deze replica's elk bevinden zich in afzonderlijke domeinen met fouten en domeinen binnen één scale opslageenheid bijwerken.

Een schaaleenheid storage is een verzameling van rekken opslagknooppunten. Een domein met fouten (FD) is een groep van knooppunten die kunnen worden beschouwd als knooppunten van hetzelfde fysieke rek en een fysieke eenheid van de fout vertegenwoordigen. Een upgradedomein (UD) is een groep van knooppunten die samen zijn bijgewerkt tijdens het proces van een service-upgrade (implementatie). De replica's zijn verdeeld over UDs en FDs binnen één schaaleenheid voor opslag. Deze architectuur zorgt ervoor dat uw gegevens beschikbaar is als een hardwarefout van invloed is op één rack of wanneer knooppunten worden bijgewerkt tijdens een implementatie.

LRS is de laagste kosten replicatie-optie en biedt de minste duurzaamheid ten opzichte van andere opties. Noodsituatie een datacenter-niveau (bijvoorbeeld brand of overstroming), zijn alle replica's verloren of onherstelbare. Om dit risico te beperken, wordt aangeraden met behulp van de zone-redundante opslag (ZRS) of geografisch redundante opslag (GRS).

* Als uw toepassing gegevens die u kunt eenvoudig opnieuw worden samengesteld opgeslagen als er gegevensverlies optreedt, kunt u kiezen voor LRS.
* Sommige toepassingen zijn beperkt tot het repliceren van gegevens alleen binnen een land vanwege beheervereisten gegevens. In sommige gevallen mogelijk de gekoppelde regio's waarvoor gegevens worden gerepliceerd voor GRS-accounts in een ander land. Zie voor meer informatie over gekoppelde regio's [Azure-regio's](https://azure.microsoft.com/regions/).
