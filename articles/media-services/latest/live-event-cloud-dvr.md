---
title: Azure Media Services LiveEvent en een cloud-DVR | Microsoft Docs
description: In dit artikel wordt uitgelegd wat LiveOutput is en hoe u een cloud-DVR.
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
ms.date: 01/14/2019
ms.author: juliako
ms.openlocfilehash: 3646c6167f901fe43080d39df42fdb127b1c7fc2
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54828142"
---
# <a name="using-a-cloud-dvr"></a>Een cloud-DVR gebruiken

Een [LiveOutput](https://docs.microsoft.com/rest/api/media/liveoutputs) kunt u voor het beheren van de eigenschappen van de uitgaande live stream, zoals hoeveel van de stroom is geregistreerd (bijvoorbeeld, de capaciteit van de cloud-DVR) en of viewers bekijken van de live stream kunnen starten. De relatie tussen een **LiveEvent** en de bijbehorende **LiveOutput**s is vergelijkbaar met traditionele televisie broadcast, waarbij een kanaal (**LiveEvent**) vertegenwoordigt een constante streamen van video en een opname (**LiveOutput**) is afgestemd op een specifiek tijdstip-segment (bijvoorbeeld 's avonds nieuws van 18:30:00 uur op 19:00 uur). U kunt met behulp van een Digital Video Recorder (DVR) televisie vastleggen – de vergelijkbare functie in LiveEvents wordt beheerd via de eigenschap ArchiveWindowLength. Het is een ISO 8601-timespan duur (bijvoorbeeld PTHH:MM:SS), waarmee wordt Hiermee geeft u de capaciteit van de DVR en kan worden ingesteld van minimaal 3 minuten tot een maximum van 25 uur.

## <a name="liveoutput"></a>LiveOutput

De **ArchiveWindowLength** waarde bepaalt hoe ver terug in tijd die een viewer kan zoeken vanaf de huidige live positie.  **ArchiveWindowLength** bepaalt ook hoe lang de clientmanifesten kunnen groeien.

Stel dat u een game football zijn streaming en deze heeft een **ArchiveWindowLength** van slechts 30 minuten. Een viewer die begint met het bekijken van de gebeurtenis 45 minuten nadat het spel gestart kan worden gezocht naar maximaal de 15 minuten is ingeschakeld. Uw **LiveOutput**s voor het spel wordt voortgezet totdat de **LiveEvent** is gestopt, maar de inhoud die valt buiten **ArchiveWindowLength** wordt altijd verwijderd uit opslag en kan niet worden hersteld. In dit voorbeeld wordt de video tussen het begin van de gebeurtenis en de 15 minuten is ingeschakeld zouden zijn verwijderd van uw DVR en van de container in blob-opslag voor de [Asset](https://docs.microsoft.com/rest/api/media/assets). Het archief kan niet worden hersteld en wordt verwijderd uit de container in Azure blob-opslag.

Elke **LiveOutput** is gekoppeld aan een **Asset**, die wordt gebruikt om vast te leggen van de video in de gekoppelde Azure blob storage-container. Als u wilt de LiveOutput publiceren, moet u een **StreamingLocator** voor die **Asset**. Na het maken van een [StreamingLocator](https://docs.microsoft.com/rest/api/media/streaminglocators), kunt u een streaming-URL die u voor uw gebruikers leveren kunt maken.

Een **LiveEvent** ondersteunt maximaal drie gelijktijdig actieve **LiveOutput**s, zodat u maximaal 3 opnamen/archieven van een live stream maken kunt. Hierdoor kunt u verschillende onderdelen van een gebeurtenis naar behoefte publiceren en archiveren. Stel dat u nodig hebt voor het uitzenden van een 24 x 7 live lineaire feed en maak "opnamen' van de andere programma's gedurende de dag kunnen aanbieden aan klanten als on-demand inhoud voor de weergave bijwerken. In dit scenario voor u eerst een primaire LiveOutput maken met een korte archiefvenster van 1 uur of minder – dit de primaire live stream dat uw kijkers zou afstemmen op is. U maakt een **StreamingLocator** voor deze **LiveOutput** en deze publiceren naar uw toepassing of website als de feed 'Live'. Terwijl de **LiveEvent** is uitgevoerd, kunt u via een programma maken een tweede gelijktijdige **LiveOutput** aan het begin van een programma (of 5 minuten vroeg voor aantal ingangen later trim). Deze tweede **LiveOutput** 5 minuten na afloop van het programma kan worden verwijderd. Met deze tweede **Asset**, kunt u een nieuwe **StreamingLocator** dit programma publiceren als een activum op aanvraag in de catalogus van uw toepassing. U kunt dit proces herhalen meerdere keren voor andere programma grenzen of belangrijke functies die u wilt delen als on-demand video's, terwijl de 'Live' feed van de eerste **LiveOutput** blijft om uit te zenden van de lineaire feed. 

> [!NOTE]
> **LiveOutput**s starten bij het maken en te stoppen wanneer verwijderd. Wanneer u verwijdert de **LiveOutput**, verwijdert u niet de onderliggende **Asset** en de inhoud in de asset. 
>
> Als u hebt gepubliceerd de **LiveOutput** asset met behulp van een **StreamingLocator**, wordt de **LiveEvent** (tot de lengte van het DVR-venster) blijft weergegeven tot de **StreamingLocator**van verlopen of verwijderd, afhankelijk van wat eerst komt.

## <a name="next-steps"></a>Volgende stappen

- [Overzicht van live streaming](live-streaming-overview.md)
- [Zelfstudie voor live streaming](stream-live-tutorial-with-api.md)

