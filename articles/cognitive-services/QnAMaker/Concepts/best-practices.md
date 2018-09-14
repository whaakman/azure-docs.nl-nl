---
title: Aanbevolen procedures - QnA Maker
titlesuffix: Azure Cognitive Services
description: Gebruik deze aanbevolen procedures te verbeteren van uw knowledge base en biedt betere resultaten van uw toepassing/chat bot-eindgebruikers.
services: cognitive-services
author: nstulasi
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/12/2018
ms.author: saneppal
ms.openlocfilehash: c82c117d149da39fba7b9a243aebb3e127540881
ms.sourcegitcommit: f983187566d165bc8540fdec5650edcc51a6350a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/13/2018
ms.locfileid: "45542922"
---
# <a name="best-practices"></a>Aanbevolen procedures
De [kennisdatabase ontwikkelingscyclus](../Concepts/development-lifecycle-knowledge-base.md) loodst u stapsgewijs hoe u kunt u KB end-to-end beheren. Gebruik deze aanbevolen procedures te verbeteren van uw knowledge base en biedt betere resultaten van uw toepassing/chat bot-eindgebruikers.

## <a name="extraction"></a>Extractie
QnA Maker is voortdurend verbeteren van de algoritmen die vragen en antwoorden supereenvoudig van inhoud uitpakken en uitbreiden van de lijst van bestands- en HTML-pagina indelingen die worden ondersteund. Ga als volgt de [richtlijnen](../Concepts/data-sources-supported.md) voor extractie op basis van het type van het document uw extraheren uit. 

Veelgestelde vragen over pagina's moeten in het algemeen zijn zelfstandige en niet gecombineerd met andere informatie. Producthandleidingen moeten wissen koppen en bij voorkeur een indexpagina hebben. 

## <a name="rankingmatching"></a>Rangschikking/vergelijken
Zorg ervoor dat u het beste gebruik van de classificatie-functies die biedt ondersteuning voor QnA Maker wilt maken. In dat geval zal de verbeteren de kans dat een bepaalde gebruiker-query wordt beantwoord met een juiste reactie.

### <a name="add-alternate-questions"></a>Alternatieve vragen toevoegen
[Andere vragen](../How-To/edit-knowledge-base.md) verbeteren de kans op een overeenkomst met de aanvraag voor een gebruiker. Andere vragen zijn nuttig wanneer er zijn meerdere manieren waarop deze vraag kan worden gevraagd. Dit zijn wijzigingen die in de zinsstructuur (bijvoorbeeld *'Is de vervangende domeinpagina beschikbaar?'* versus *"Hebt u Auto-park?"* ) of wijzigingen in de word-stijl en slang (bijvoorbeeld *'Hi'* versus *"Yo"*, *"Er Hallo!"* ).

### <a name="use-metadata-filters"></a>Filters gebruiken om metagegevens
[Metagegevens](../How-To/edit-knowledge-base.md) voegt de mogelijkheid om de resultaten van de gebruikersquery van een op basis van filters vast te stellen. De knowledge base-antwoord kan verschillen op basis van de metadata-tag, zelfs als de query hetzelfde is. Bijvoorbeeld, *"waar is vervangende domeinpagina zich"* een ander antwoord kan hebben als de locatie van de vertakking restaurant - dat wil zeggen, de metagegevens is *locatie: Seattle* versus *locatie: Redmond*.)

### <a name="use-synonyms"></a>Synoniemen gebruiken
Hoewel er enkele ondersteuning van synoniemen in het Engels, gebruikt u [wijzigingen in word](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75fd) synoniemen toevoegen aan trefwoorden die andere vorm (voorbeeld: *kopen* -> *kopen*  of *netbanking* -> *net bankieren*. Synoniemen moeten worden toegevoegd op de QnA Maker-serviceniveau en gedeeld door alle knowledge bases in de service.

### <a name="use-distinct-words-to-differentiate-questions"></a>Gebruik verschillende woorden te onderscheiden van vragen
QnA Maker overeenkomst en positie algoritmen die overeenkomen met de gebruikersquery van een met een vraag in het knowledge base werken het beste als elke vraag verschillende behoeften adressen. Herhaling van hetzelfde woord in te stellen tussen vragen vermindert de kans dat het juiste antwoord is gekozen voor een bepaalde gebruikersquery met deze woorden.

## <a name="collaborate"></a>Samenwerken
QnA Maker kunnen gebruikers [samenwerken](../How-to/collaborate-knowledge-base.md) op een knowledge base. Gebruikers hebben toegang tot de QnA Maker-Azure-resourcegroep nodig voor toegang tot knowledge bases. Sommige organisaties willen uitbesteden de kennisdatabase bewerken en het onderhoud, en nog steeds mogelijk om toegang tot hun Azure-resources te beveiligen. Dit model editor fiatteur kan worden bereikt door het instellen van twee identieke [QnA Maker services](../How-to/set-up-qnamaker-service-azure.md) in verschillende abonnementen en het toewijzen van een van de cyclus bewerken te testen. Zodra het testen is voltooid, de knowledge base-inhoud kunnen worden overgedragen met een [import / export](../Tutorials/migrate-knowledge-base.md) verwerken met de QnA Maker-service van de fiatteur die ten slotte wordt de kennisdatabase publiceren en bijwerken van het eindpunt.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een kennisdatabase bewerken](../How-to/edit-knowledge-base.md)

