---
title: Spraak vertaling API-documentatie | Microsoft Docs
titleSuffix: Cognitive Services
description: Gebruik de API van Microsoft Translator spraak vertaling spraak-naar-spraak toevoegen en spraak naar tekstvertaling naar uw toepassingen.
services: cognitive-services
author: Jann-Skotdal
manager: chriswendt1
ms.service: cognitive-services
ms.component: translator-speech
ms.topic: article
ms.date: 3/5/2018
ms.author: v-jansko
ms.openlocfilehash: 15f27e6b5b2fd7384958a660156855fc65f4e558
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344757"
---
# <a name="microsoft-translator-speech-api"></a>Spraak-API van Microsoft Translator
De Microsoft Translator Speech-API kan end-to-end, realtime spraak vertalingen toevoegen aan toepassingen, hulpprogramma's of een oplossing die moeten worden vertaald meertalig spraak ongeacht de doel-OS of ontwikkelingstalen worden gebruikt. De API kan worden gebruikt voor beide spraak spraak en speech text vertaling.

Microsoft Translator tekst API is een Azure-service, onderdeel van de [Microsoft cognitieve Services API verzameling](https://docs.microsoft.com/azure/#pivot=products&panel=cognitive) van machine learning en AI algoritmen in de cloud, gemakkelijk kunnen worden gebruikt in uw ontwikkelingsprojecten.

Met de Microsoft Translator Speech-API clienttoepassingen stream spraak geluid om de service en ontvangen van een stream van de tekst en audio-gebaseerde resultaten, waaronder de herkende tekst in de taal van de bron en de vertaling in de doel-taal. Resultaten van de tekst worden geproduceerd door het toepassen van automatische Speech Recognition (ASR) aangedreven door diep neural netwerken voor de binnenkomende audiostroom. Onbewerkte ASR uitvoer is verder verbeterd door een nieuwe techniek aangeroepen TekstAlsWaar om beter intentie van de gebruiker weer te geven. Bijvoorbeeld, verwijderd TekstAlsWaar disfluencies (hmms en coughs), herhaalde woorden en herstelt de juiste leestekens en hoofdlettergebruik. De mogelijkheid om te maskeren of uitsluiten profanities is ook opgenomen. De opname- en NAT-engines zijn specifiek voor het afhandelen van eigen spraak getraind. 

De vertaling van spraak-service gebruikt Stiltedetectie om te bepalen van het einde van een utterance. Na een onderbreking in de stem activiteit, wordt de service stream terug uiteindelijke resultaat van de voltooide utterance. De service kunt terug gedeeltelijke resultaten, die tussenliggende erkenning en vertalingen voor een utterance in voortgang geeft ook verzenden. 

Voor de vertaling van spraak-naar-spraak biedt de service de mogelijkheid om na te bootsen speech (spraak) uit de gesproken tekst in de doel-talen. Audio-naar-spraak wordt gemaakt in de indeling die is opgegeven door de client. WAV en MP3 indelingen zijn beschikbaar.

De vertaling Speech-API wordt het WebSocket-protocol gebruikt voor een kanaal full-duplex-communicatie tussen de client en de server. 

## <a name="about-microsoft-translator"></a>Over Microsoft Translator
Microsoft Translator is een vertaalservice cloud-gebaseerde computer. De kern van deze service wordt de [conversieprogramma tekst API] (https://www.microsoft.com/en-us/translator/translatorapi.aspx) en conversieprogramma Speech-API die verschillende Microsoft-producten en services inschakelen en worden gebruikt door duizenden bedrijven wereldwijd in hun toepassingen en werkstromen, zodat hun inhoud een wereldwijde doelgroep bereiken.

Meer informatie over de [Microsoft Translator-service](https://www.microsoft.com/en-us/translator/home.aspx)

## <a name="microsoft-translator-neural-machine-translation-nmt"></a>Microsoft Translator Neural machinevertaling (NMT)
De Microsoft Translator Speech-API wordt gebruikt voor de verouderde statistische machinevertaling (SMT) en de nieuwere neural machinevertaling (NMT) voor vertalingen.

Statistische machinevertaling plateau een in termen van de prestaties verbeteren. Vertaling kwaliteit niet meer op een belangrijke manier voor algemene systemen met SMT helpen verbeteren. Een nieuwe vertaling op basis van AI-technologie is gebaseerd op Neural Networks (NN) dynamiek krijgen.

NMT biedt betere vertalingen niet alleen uit een onbewerkte vertaling kwaliteit verwerkt maar ook score berekenen omdat geluid meer beheersen, meer menselijke dan SMT die zijn. De belangrijkste reden voor deze soepele manier is dat NMT wordt gebruikt voor de volledige context van een zin om woorden. SMT duurt slechts de onmiddellijke context van een paar woorden voor en na elk woord.

NMT modellen zijn de kern van de API en niet zichtbaar is voor eindgebruikers. De alleen merkbare verschillen zijn:
* De kwaliteit van de verbeterde vertaling, met name voor talen zoals Chinees en Japans Arabisch
* Het niet compatibel is met de bestaande Hub aanpassingsfuncties (voor gebruik met de API van Microsoft Translator tekst)

Alle ondersteunde spraak vertaling talen zijn aangedreven door NMT. Daarom gebruikt alle spraak-naar-spraak vertaling NMT. 

Spraak tekst vertaling mag een combinatie van NMT en SMT, afhankelijk van de combinatie van taal gebruiken. Als de doel-taal wordt ondersteund door NMT, is de volledige vertaling NMT ingeschakeld. Als de taal van het doel wordt niet ondersteund door NMT, is de omzetting een hybride van NMT en SMT met Engels als een 'pivot' tussen de twee talen. 

Weergave ondersteunde talen op [Microsoft.com](https://www.microsoft.com/en-us/translator/languages.aspx). 

Meer informatie over [hoe NMT werkt](https://www.microsoft.com/en-us/translator/mt.aspx#nnt)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Aanmelden](translator-speech-how-to-signup.md)

> [!div class="nextstepaction"]
> [Gaan coderen](quickstarts/csharp.md)

## <a name="see-also"></a>Zie ook
- [Cognitieve Services-documentatie pagina](https://docs.microsoft.com/azure/#pivot=products&panel=cognitive)
- [Cognitieve Services productpagina](https://azure.microsoft.com/services/cognitive-services/)
- [Oplossing en de prijsgegevens](https://www.microsoft.com/en-us/translator/home.aspx) 
