---
title: Conversation Learner standaard configuratie-Microsoft Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Meer informatie over de standaard configuratie van Conversation Learner.
services: cognitive-services
author: nitinme
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: aaef6f5498e5a8da65d1c829feae8b3e85dba0fd
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68705302"
---
# <a name="default-values-and-boundaries"></a>Standaardwaarden en -grenzen

In dit document wordt de standaard configuratie van Conversation Learner en key service-grenzen beschreven.

## <a name="default-configuration"></a>Standaard configuratie

Parameter | Standaardwaarde
--- | --- 
Standaard sessietime-out | 30 minuten

## <a name="boundaries"></a>Grenzen

Parameter | Limiet
--- | --- 
API ontwerpen, maximum aantal HTTP-aanroepen per maand | 5 MIN.
API ontwerpen, maximum aantal HTTP-aanroepen per seconde | 25
Sessie-API, maximum aantal HTTP-aanroepen per maand | 500.000
Sessie-API, maximum aantal HTTP-aanroepen per seconde | 10
Maximum aantal aangepaste entiteiten (niet-programmatisch) per model | Zie [Luis boundarys doc](https://docs.microsoft.com/azure/cognitive-services/luis/luis-boundaries); in de praktijk is het werkelijke getal mogelijk iets kleiner
Maximum aantal vooraf gemaakte entiteiten per model | Zie [Luis boundarys doc](https://docs.microsoft.com/azure/cognitive-services/luis/luis-boundaries)
Maximum aantal entiteiten (in totaal) per model | 100
Maximum aantal acties per model | 32
Maximum aantal dialoog vensters per model | 1000
Maximum aantal gebruikers per trein dialoog venster | 100
Maximum aantal logboek dialoogvensters per model | Geen vooraf ingestelde limiet, maar logboek dialoogvensters worden alleen voor een vaste periode bewaard voordat deze worden verwijderd.  Daarnaast worden in de Conversation Learner gebruikers interface de dialoog vensters van 100 logboeken tegelijk weer gegeven. 
Maximum aantal modellen per gebruiker | Geen limiet voor het instellen
Maximum aantal sequentiële niet-wacht acties | 5 (*)

(*) Na vijf opeenvolgende acties zonder wacht tijd worden alle niet-wacht acties gemaskeerd en worden Conversation Learner voor de beschik bare wacht acties gekozen.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Aan de slag met Conversation Learner](./quickstart.md)
