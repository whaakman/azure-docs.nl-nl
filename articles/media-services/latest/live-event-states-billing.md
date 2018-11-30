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
ms.date: 11/16/2018
ms.author: juliako
ms.openlocfilehash: 1a49f62d7b5e21fe9d6483f71b729a9100aff1a3
ms.sourcegitcommit: 56d20d444e814800407a955d318a58917e87fe94
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/29/2018
ms.locfileid: "52585533"
---
# <a name="liveevent-states-and-billing"></a>LiveEvent Staten en facturering

In Azure Media Services, een LiveEvent begint zodra de status verandert in facturering **met**. Als u wilt stoppen met het LiveEvent van facturering, moet u de LiveEvent stoppen.

Wanneer **LiveEventEncodingType** op uw [LiveEvent](https://docs.microsoft.com/rest/api/media/liveevents) is ingesteld op standaard (basis), Media Services automatisch moet worden uitgeschakeld eventuele LiveEvent die nog steeds in de **met** status 12 uur Nadat de invoer feed verbroken is en er zijn geen **LiveOutput**s uitgevoerd. Maar u nog steeds gefactureerd voor de tijd die de LiveEvent is in de **met** staat.

## <a name="states"></a>Staten

De LiveEvent kan zich in een van de volgende statussen.

|Status|Beschrijving|
|---|---|
|**Gestopt**| Dit is de beginstatus van de LiveEvent na het maken van (tenzij autostart is ingesteld op ' True '.) Er is geen facturering vindt plaats in deze status. In deze status kunnen de eigenschappen van het LiveEvent worden bijgewerkt, maar streaming niet toegestaan.|
|**Starten**| De LiveEvent wordt gestart en resources worden toegewezen. Er is geen facturering vindt plaats in deze status. Updates of streaming zijn niet toegestaan in deze status. Als een fout optreedt, wordt de LiveEvent teruggezet naar de status gestopt.|
|**Wordt uitgevoerd**| De LiveEvent resources zijn toegewezen, opnemen en preview-URL's zijn gegenereerd en is geschikt voor ontvangst van live streams. Facturering is op dit moment actief. U moet stoppen expliciet aanroepen op de resource LiveEvent verder facturering is gestopt.|
|**Stoppen**| De LiveEvent wordt gestopt en de resources zijn verlopen. Geen facturering vindt plaats in deze tijdelijke status. Updates of streaming zijn niet toegestaan in deze status.|
|**Verwijderen**| De LiveEvent wordt verwijderd. Geen facturering vindt plaats in deze tijdelijke status. Updates of streaming zijn niet toegestaan in deze status.|

## <a name="next-steps"></a>Volgende stappen

- [Overzicht van live streaming](live-streaming-overview.md)
- [Zelfstudie voor live streaming](stream-live-tutorial-with-api.md)
