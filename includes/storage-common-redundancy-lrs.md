---
title: bestand opnemen
description: bestand opnemen
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 10/24/2018
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: a948a40e638b5f6e042c62ab58c2b7b65a49cd4e
ms.sourcegitcommit: ae45eacd213bc008e144b2df1b1d73b1acbbaa4c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/01/2018
ms.locfileid: "50741977"
---
Lokaal redundante opslag (LRS) biedt ten minste 99,999999999% (11 nines) duurzaamheid van objecten in een bepaald jaar. LRS biedt de duurzaamheid van dit object door te repliceren van uw gegevens naar een opslagschaaleenheid. Een datacenter, zich in de regio waar u uw storage-account hebt gemaakt als host fungeert voor de eenheid voor opslag schalen. Een schrijfaanvraag naar een opslagaccount LRS retourneert is pas mogelijk nadat de gegevens worden geschreven naar alle replica's. Elke replica bevindt zich in afzonderlijke foutdomeinen en updatedomeinen in een opslagschaaleenheid.

Een opslag-schaaleenheid is een verzameling van rekken opslagknooppunten. Een foutdomein (FD) is een groep van knooppunten die staan voor een fysieke eenheid van de fout. Een foutdomein beschouwen als die behoren tot hetzelfde fysieke rack-knooppunten. Een upgrade domain (UD) is een groep van knooppunten die samen zijn bijgewerkt tijdens het proces van een service-upgrade (implementatie). De replica's worden verdeeld over de ud's en FD's binnen een opslagschaaleenheid. Deze architectuur zorgt ervoor dat uw gegevens zijn beschikbaar als een hardwarefout is van invloed op een enkele rack of knooppunten worden bijgewerkt tijdens de upgrade van een service.

LRS is de laagste kosten replicatie-optie en biedt de minste duurzaamheid in vergelijking met andere opties. Als een datacenter noodgevel op het niveau (bijvoorbeeld brand of overstroming), zijn alle replica's verloren of niet kan worden hersteld. Als u wilt dit risico te beperken, wordt aangeraden met behulp van de zone-redundante opslag (ZRS) of geografisch redundante opslag (GRS).

* Als uw toepassing gegevens op die u kunt eenvoudig opnieuw worden samengesteld slaat als er gegevensverlies optreedt, kunt u kiezen voor LRS.
* Sommige toepassingen zijn beperkt tot het repliceren van gegevens binnen een land/regio vanwege de vereisten voor gegevens. In sommige gevallen kunnen de gekoppelde regio's in die de gegevens worden gerepliceerd voor GRS-accounts worden in een ander land. Zie voor meer informatie over gekoppelde regio's, [Azure-regio's](https://azure.microsoft.com/regions/).
