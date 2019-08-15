---
title: bestand opnemen
description: bestand opnemen
services: storage
author: tamram
ms.service: storage
ms.topic: include
ms.date: 06/28/2019
ms.author: tamram
ms.custom: include file
ms.openlocfilehash: b4b050befe160d69fbc44b7f0a2ebbbbad3d705a
ms.sourcegitcommit: df7942ba1f28903ff7bef640ecef894e95f7f335
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/14/2019
ms.locfileid: "69015960"
---
Lokaal redundante opslag (LRS) voorziet in ten minste 99,999999999% (11 Nines) duurzaamheid van objecten in een bepaald jaar. LRS biedt deze object duurzaamheid door uw gegevens naar een opslag schaal eenheid te repliceren. Een Data Center, dat zich bevindt in de regio waar u uw opslag account hebt gemaakt, fungeert als host voor de opslag schaal eenheid. Een schrijf aanvraag naar een LRS-opslag account retourneert pas nadat de gegevens naar alle replica's zijn geschreven. Elke replica bevindt zich in afzonderlijke fout domeinen en upgrade domeinen binnen een opslag schaal eenheid.

Een schaal eenheid voor opslag is een verzameling van opslag knooppunten. Een fout domein (FD) is een groep knoop punten die een fysieke fout eenheid vertegenwoordigen. U kunt een fout domein beschouwen als knoop punten die deel uitmaken van hetzelfde fysieke rack. Een upgrade domein (UD) is een groep knoop punten die bij elkaar worden bijgewerkt tijdens het proces van een service-upgrade (implementatie). De replica's zijn verspreid over de UDs-en Fd's binnen één opslag schaal eenheid. Deze architectuur zorgt ervoor dat uw gegevens beschikbaar zijn als een hardwarefout van invloed is op één rek of wanneer er knoop punten zijn bijgewerkt tijdens een service-upgrade.

LRS is de optie voor de laagste kosten replicatie en biedt de minste duurzaamheid ten opzichte van andere opties. Als er sprake is van een ramp op datacenter niveau (bijvoorbeeld brand of flooding), zijn alle replica's mogelijk verloren of kunnen deze niet meer worden hersteld. Om dit risico te beperken, raadt micro soft aan gebruik te maken van zone-redundante opslag (ZRS), geografisch redundante opslag (GRS) of geo-zone-redundante opslag (GZRS).

* Als uw toepassing gegevens opslaat die gemakkelijk kunnen worden geconstrueerd als er gegevens verlies optreedt, kunt u kiezen voor LRS.
* Sommige toepassingen zijn beperkt tot het repliceren van gegevens in een land/regio als gevolg van vereisten voor gegevens beheer. In sommige gevallen kunnen de gekoppelde regio's waarvan de gegevens voor GRS-accounts worden gerepliceerd, zich in een ander land of een andere regio bevinden. Zie [Azure-regio's](https://azure.microsoft.com/regions/)voor meer informatie over gekoppelde regio's.
