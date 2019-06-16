---
title: LiveEvent Staten en facturering in Azure Media Services | Microsoft Docs
description: In dit onderwerp biedt een overzicht van Azure Media Services LiveEvent Staten en facturering.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 01/28/2019
ms.author: juliako
ms.openlocfilehash: 2907b5be7f8d5fda3d510484179e80b065ab64b0
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67074897"
---
# <a name="live-event-states-and-billing"></a>Live gebeurtenis Staten en facturering

In Azure Media Services, een Live gebeurtenis begint zodra de status verandert in facturering **met**. Als u wilt stoppen met de Live-gebeurtenis van facturering, moet u de Live-gebeurtenis beëindigen.

Wanneer **LiveEventEncodingType** op uw [Live gebeurtenis](https://docs.microsoft.com/rest/api/media/liveevents) is ingesteld op standaard of Premium1080p, Media Services automatisch moet worden uitgeschakeld een Live gebeurtenis die is nog steeds in de **met** 12 status uur nadat de invoer feed verbroken is en er zijn geen **uitvoer Live**s uitgevoerd. Maar u nog steeds gefactureerd voor de tijd die de Live-gebeurtenis is in de **met** staat.

## <a name="states"></a>States

De Live-gebeurtenis kan zich in een van de volgende statussen.

|Status|Description|
|---|---|
|**Gestopt**| Dit is de beginstatus van de Live gebeurtenis na het maken van (tenzij autostart is ingesteld op ' True '.) Er is geen facturering vindt plaats in deze status. In deze status kunnen de eigenschappen van Live gebeurtenissen kunnen worden bijgewerkt, maar streaming niet toegestaan.|
|**Starten**| De Live gebeurtenis wordt gestart en resources worden toegewezen. Er is geen facturering vindt plaats in deze status. Updates of streaming zijn niet toegestaan in deze status. Als een fout optreedt, wordt de Live gebeurtenis teruggezet naar de status gestopt.|
|**Wordt uitgevoerd**| De Live gebeurtenis resources zijn toegewezen, opnemen en preview-URL's zijn gegenereerd en is geschikt voor ontvangst van live streams. Facturering is op dit moment actief. U moet expliciet Stop aanroepen in de resource van de livegebeurtenis om verdere facturering stop te zetten.|
|**Stoppen**| De Live gebeurtenis wordt gestopt en de resources zijn verlopen. Geen facturering vindt plaats in deze tijdelijke status. Updates of streaming zijn niet toegestaan in deze status.|
|**Verwijderen**| De Live gebeurtenis wordt verwijderd. Geen facturering vindt plaats in deze tijdelijke status. Updates of streaming zijn niet toegestaan in deze status.|

## <a name="next-steps"></a>Volgende stappen

- [Overzicht van live streaming](live-streaming-overview.md)
- [Zelfstudie voor live streaming](stream-live-tutorial-with-api.md)
