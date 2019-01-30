---
title: Over Spraakomzetting - spraakservices
titlesuffix: Azure Cognitive Services
description: De spraak-API-Service kunt u end-to-end-, realtime, meerdere talen vertaling van spraak toevoegen aan uw toepassingen, hulpprogramma's en apparaten. Dezelfde API kan worden gebruikt voor zowel spraak-naar-spraak- en spraak naar tekst converteren.
services: cognitive-services
author: erhopf
manager: cgronlun
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: erhopf
ms.custom: seodec18
ms.openlocfilehash: 8519e05062ad1b8b8b20dcf5caafddaa07550049
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55228726"
---
# <a name="about-the-speech-translation-api"></a>Over de Spraakomzettings-API

De spraak-API-Service kunt u end-to-end-, realtime, meerdere talen vertaling van spraak toevoegen aan uw toepassingen, hulpprogramma's en apparaten. Dezelfde API kan worden gebruikt voor zowel spraak-naar-spraak- en spraak naar tekst converteren.

Met de spraak-API van Translator clienttoepassingen streamen gesproken audio naar de service en het ontvangen van een stream van resultaten. Deze resultaten opnemen de herkende tekst in de source-taal en de vertaling in de doel-taal. Tijdelijke vertalingen kunnen worden opgegeven voordat een utterance voltooid is, op dat moment een laatste omzetting wordt opgegeven.

(Optioneel) een kunstmatige audioversie van de laatste vertaling voor te bereiden, waar spraak-naar-spraak-omzetting inschakelen.

De Spraakomzettings-API gebruikt een WebSockets-protocol voor een kanaal full-duplex-communicatie tussen de client en de server. Maar u hoeft niet te bekommeren om WebSockets; de spraak-SDK verwerkt die voor u.

De Spraakomzettings-API maakt gebruik van dezelfde technologieën die verschillende Microsoft-producten services en. Deze service wordt al gebruikt door duizenden bedrijven wereldwijd in hun toepassingen en werkstromen.

## <a name="about-the-technology"></a>Over de technologie

Onderliggende vertaling-engine van Microsoft zijn twee verschillende manieren: statistische machinevertalingen (SMT) en neurale machinevertalingen (NMT). De laatste, een benadering van kunstmatige intelligentie die gebruikmaakt van neurale netwerken, is de moderne aanpak van machinevertalingen. NMT biedt betere vertalingen, niet alleen meer nauwkeurige, maar ook meer gestroomlijnde en natuurlijke. De belangrijkste reden hiervoor is dat NMT gebruikmaakt van de volledige context van een zin bij het vertalen van woorden.

Vandaag de dag is Microsoft gemigreerd naar NMT voor de meest populaire talen die gebruikmaakt van SMT alleen voor minder vaak gebruikte talen. Alle [beschikbare talen voor spraak-naar-spraak vertaling](language-support.md#speech-translation) worden aangestuurd door NMT. Spraak naar tekst converteren kan gebruikmaken van SMT of NMT, afhankelijk van de combinatie van taal. Als de doeltaal voor NMT wordt ondersteund, wordt de volledige vertaling verzorgd door NMT. Als de doel-taal wordt niet ondersteund door NMT, is de omzetting van een hybride van NMT en SMT, met behulp van Engels als een "draaien" tussen de twee talen.

De verschillen tussen modellen zijn intern voor de NAT-engine. Eindgebruikers kunnen u alleen de kwaliteit van de verbeterde vertaling met name voor Chinese, Japans en Arabisch ziet.

> [!NOTE]
> Wilt u meer informatie over de technologie achter NAT-engine van Microsoft? Zie [machinevertalingen](https://www.microsoft.com/en-us/translator/mt.aspx).

## <a name="next-steps"></a>Volgende stappen

* [Uw proefabonnement voor Speech ophalen](https://azure.microsoft.com/try/cognitive-services/)
* [Informatie over het vertalen van gesproken tekst in C#](how-to-translate-speech-csharp.md)
* [Informatie over het vertalen van gesproken tekst in C++](how-to-translate-speech-cpp.md)
* [Informatie over het vertalen van gesproken tekst in Java](how-to-translate-speech-java.md)
