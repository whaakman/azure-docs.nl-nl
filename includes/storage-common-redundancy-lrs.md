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
ms.sourcegitcommit: d4c076beea3a8d9e09c9d2f4a63428dc72dd9806
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2018
ms.locfileid: "39399988"
---
Lokaal redundante opslag (LRS) is zodanig ontworpen dat ten minste 99,999999999% (11 9's) duurzaamheid van objecten in een bepaald jaar door uw gegevens in een opslagschaaleenheid te repliceren. Een opslagschaaleenheid wordt gehost in een datacenter in de regio waarin u uw storage-account hebt gemaakt. Een schrijfaanvraag naar een opslagaccount LRS retourneert is pas nadat de gegevens naar alle replica's is geschreven. Deze replica's elk bevinden zich in afzonderlijke foutdomeinen en updatedomeinen in een opslagschaaleenheid.

Een opslag-schaaleenheid is een verzameling van rekken opslagknooppunten. Een foutdomein (FD) is een groep van knooppunten die kunnen worden beschouwd als knooppunten die behoren tot hetzelfde fysieke rek en een fysieke eenheid van de fout vertegenwoordigen. Een upgrade domain (UD) is een groep van knooppunten die samen zijn bijgewerkt tijdens het proces van een service-upgrade (implementatie). De replica's worden verdeeld over de ud's en FD's binnen een opslagschaaleenheid. Deze architectuur zorgt ervoor dat uw gegevens beschikbaar is als een hardware-uitval van invloed is op één rack of knooppunten worden bijgewerkt tijdens een implementatie.

LRS is de laagste kosten replicatie-optie en biedt de minste duurzaamheid in vergelijking met andere opties. Als een datacenter noodgevel op het niveau (bijvoorbeeld brand of overstroming), zijn alle replica's verloren of niet kan worden hersteld. Als u wilt dit risico te beperken, wordt aangeraden met behulp van de zone-redundante opslag (ZRS) of geografisch redundante opslag (GRS).

* Als uw toepassing gegevens op die u kunt eenvoudig opnieuw worden samengesteld slaat als er gegevensverlies optreedt, kunt u kiezen voor LRS.
* Sommige toepassingen zijn beperkt tot het repliceren van gegevens binnen een land/regio vanwege de vereisten voor gegevens. In sommige gevallen kunnen de gekoppelde regio's over welke gegevens worden gerepliceerd voor GRS-accounts worden in een ander land. Zie voor meer informatie over gekoppelde regio's, [Azure-regio's](https://azure.microsoft.com/regions/).
