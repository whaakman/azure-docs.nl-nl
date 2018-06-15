---
title: Aanbevolen procedures cognitieve - QnA Maker - Azure-Services | Microsoft Docs
description: Gebruik deze best practices te verbeteren van uw knowledge base en biedt betere resultaten eindgebruikers van uw toepassing/chat-bot.
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 05/07/2018
ms.author: saneppal
ms.openlocfilehash: 7a85ebbc3892a90e98e73a73425c1f8ec1de0b35
ms.sourcegitcommit: 680964b75f7fff2f0517b7a0d43e01a9ee3da445
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "35345922"
---
# <a name="best-practices"></a>Aanbevolen procedures
De [kennisdatabase ontwikkelingscyclus](../Concepts/development-lifecycle-knowledge-base.md) helpt u op het beheren van KB end-to-end. Gebruik deze best practices te verbeteren van uw knowledge base en biedt betere resultaten eindgebruikers van uw toepassing/chat-bot.

## <a name="extraction"></a>Uitpakken
QnA Maker is voortdurend verbetering van de algoritmen die QnAs van inhoud uitpakken en uitbreiden van de lijst van bestands- en HTML-pagina indelingen die worden ondersteund. Ga als volgt de [richtlijnen](../Concepts/data-sources-supported.md) voor extractie op basis van het type document u extraheren uit. 

In het algemeen moet Veelgestelde vragen over pagina's zelfstandige en niet gecombineerd met andere informatie. Producthandleidingen moeten wissen koppen en bij voorkeur een indexpagina hebben. 

## <a name="rankingmatching"></a>Rangschikking/overeenkomende
Zorg ervoor dat u het beste gebruik van functies ranking die qna Maker ondersteunt doorvoert. In dat geval verbetert de kans dat een bepaalde gebruikersquery met een juiste reactie is beantwoord.

### <a name="add-alternate-questions"></a>Alternatieve vragen toevoegen
[Alternatieve vragen](../How-To/edit-knowledge-base.md) verbeteren de kans op een overeenkomst met de aanvraag voor een gebruiker. Alternatieve vragen zijn nuttig wanneer er zijn meerdere manieren waarin deze vraag kan worden gevraagd. Het kan hierbij gaan wijzigingen in de zinsstructuur (bijvoorbeeld *'Parkeerplaatsen beschikbaar Is?'* versus *'Hebt u Auto park?'* ) of wijzigingen in de word-stijl en taalgebruik (bijvoorbeeld *'Hallo'* versus *'Yo'*, *'Artikel van Hey er!'* ).

### <a name="use-metadata-filters"></a>Filters gebruiken om metagegevens
[Metagegevens](../How-To/edit-knowledge-base.md) voegt de mogelijkheid om de resultaten van de gebruikersquery van een op basis van filters vast te stellen. Het antwoord kennisdatabase kan verschillen op basis van de metadata-code, zelfs als de query hetzelfde is. Bijvoorbeeld: *'waar zich parkeerplaatsen is'* kan een ander antwoord hebben als de locatie van de vertakking restaurant verschilt - dat wil zeggen, de metagegevens is *locatie: Seattle* versus *locatie: Redmond*.)

### <a name="use-synonyms"></a>Synoniemen gebruiken
Hoewel er enkele ondersteuning voor synoniemen in het Engels, gebruik [veranderingen in word](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75fd) synoniemen toevoegen aan sleutelwoorden die andere vorm (voorbeeld: *kopen* -> *kopen*  of *netbanking* -> *bankieren net*. Synoniemen moeten worden toegevoegd op het serviceniveau QnA Maker en gedeeld door alle kennis basissen in de service.

### <a name="use-distinct-words-to-differentiate-questions"></a>Verschillende woorden gebruiken om te onderscheiden van vragen
QnA Maker match en positie algoritmen die overeenkomen met de aanvraag voor een gebruiker met een vraag in het knowledge base werken het beste als elke vraag verschillende behoeften adressen. Herhaling van hetzelfde woord in te stellen tussen vragen vermindert de kans dat het juiste antwoord is gekozen voor een bepaalde gebruikersquery met deze woorden.

## <a name="collaborate"></a>Samenwerken
QnA Maker kunnen gebruikers [samenwerken](../How-to/collaborate-knowledge-base.md) op een knowledge base. Gebruikers nodig toegang tot de resourcegroep QnA Maker van Azure om de kennis bases toegang te krijgen. Sommige organisaties willen uitbesteden de kennisdatabase bewerken en het onderhoud, en nog steeds toegang tot Azure-resources te beveiligen. Dit model editor goedkeurder kan worden uitgevoerd door het instellen van twee identieke [QnA Maker services](../How-to/set-up-qnamaker-service-azure.md) in verschillende abonnementen en het toewijzen van één voor de cyclus bewerken testen. Zodra de test is voltooid, de knowledge base-inhoud kunnen worden overgedragen met een [importeren exporteren](../Tutorials/migrate-knowledge-base.md) verwerken met de service QnA Maker van de goedkeurder die ten slotte publiceren de knowledge base en het eindpunt worden bijgewerkt.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een knowledge base bewerken](../How-to/edit-knowledge-base.md)

