---
title: Over spraak vertaling | Microsoft Docs
description: Een overzicht van de mogelijkheden van de vertaling van spraak
titleSuffix: Microsoft Cognitive Services
services: cognitive-services
author: v-jerkin
manager: noellelacharite
ms.service: cognitive-services
ms.component: speech-service
ms.topic: article
ms.date: 04/28/2018
ms.author: v-jerkin
ms.openlocfilehash: 43fcde887c21794989aa43540a214ef34893a630
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345819"
---
# <a name="about-the-speech-translation-api"></a>Over de vertaling spraak API

De Microsoft Speech-API kunt u end-to-end-, realtime, meertalige vertaling van spraak toevoegen aan uw toepassingen, hulpprogramma's en apparaten. De dezelfde API kan worden gebruikt voor de vertaling van spraak-naar-spraak zowel spraak naar tekst.

Met de Microsoft Translator Speech-API clienttoepassingen stream spraak geluid om de service en ontvangen van een stream met resultaten. Deze resultaten opnemen de herkende tekst in de taal van de bron en de vertaling in de doel-taal. Tussentijdse vertalingen kunnen worden opgegeven voordat een utterance voltooid is, op dat moment een definitieve vertaling is opgegeven.

Eventueel een gesyntheseerde audio-versie van de laatste vertaling voor te bereiden, waar de vertaling van spraak-naar-spraak inschakelen.

De vertaling Speech-API wordt een WebSockets-protocol gebruikt voor een kanaal full-duplex-communicatie tussen de client en de server. Maar u hoeft niet te bekommeren om WebSockets; de SDK spraak verwerkt die voor u.

De vertaling Speech-API maakt gebruik van dezelfde technologieën die ten grondslag diverse Microsoft-producten en services. Deze service wordt al gebruikt door duizenden bedrijven wereldwijd in hun toepassingen en werkstromen.

## <a name="about-the-technology"></a>Over de technologie

Onderliggende vertaling-engine van Microsoft zijn twee verschillende benaderingen: statistische machinevertaling (SMT) en neural automatische vertaling (NMT). De laatste, een kunstmatige intelligence benadering die gebruikmaakt van neural networks moderne manier te vertalen. NMT biedt betere vertalingen, niet alleen nauwkeuriger, maar ook meer beheersen en natuurlijke. De belangrijkste reden voor deze soepele manier is dat NMT wordt gebruikt voor de volledige context van een zin om woorden.

Vandaag de dag is Microsoft gemigreerd naar NMT voor de meest populaire talen die gebruikmaakt van SMT alleen voor minder vaak gebruikte talen. Alle [talen die beschikbaar zijn voor spraak-naar-spraak vertaling](supported-languages.md#speech-translation) door NMT zijn ingeschakeld. De vertaling van spraak naar tekst kan gebruikmaken van SMT of NMT, afhankelijk van de combinatie van taal. Als de doel-taal wordt ondersteund door NMT, is de volledige vertaling NMT ingeschakeld. Als de taal van het doel wordt niet ondersteund door NMT, is de omzetting een hybride van NMT en SMT, met Engels als een 'pivot' tussen de twee talen.

De verschillen tussen modellen zijn intern voor de NAT-engine. Eindgebruikers ziet alleen de verbeterde vertaling kwaliteit, met name voor Chinees, Japans en Arabisch.

> [!NOTE]
> Wilt u meer leren over de technologie achter NAT-engine van Microsoft? Zie [machinevertaling](https://www.microsoft.com/en-us/translator/mt.aspx).

## <a name="next-steps"></a>Volgende stappen

* [Uw proefabonnement spraak ophalen](https://azure.microsoft.com/try/cognitive-services/)
* [Zie het herkennen van spraak in C#](quickstart-csharp-windows.md)
