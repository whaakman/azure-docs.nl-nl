---
title: Aanbevolen procedures - QnA Maker
titlesuffix: Azure Cognitive Services
description: Gebruik deze aanbevolen procedures te verbeteren van uw knowledge base en biedt betere resultaten van uw toepassing/chat bot-eindgebruikers.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/24/2018
ms.author: tulasim
ms.custom: seodec18
ms.openlocfilehash: 63025a299f6b7b48936be9702522a832a1d77330
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2018
ms.locfileid: "53074794"
---
# <a name="best-practices-of-a-qna-maker-knowledge-base"></a>Best practices van een kennisdatabase QnA Maker
De [kennisdatabase ontwikkelingscyclus](../Concepts/development-lifecycle-knowledge-base.md) helpt u over het beheren van uw KB van begin tot einde. Gebruik deze aanbevolen procedures te verbeteren van uw knowledge base en biedt betere resultaten van uw toepassing/chat bot-eindgebruikers.

## <a name="extraction"></a>Extractie
De QnA Maker-service wordt voortdurend verbeteren van de algoritmen die vragen en antwoorden supereenvoudig uitpakken van inhoud en de lijst met ondersteunde bestands- en HTML-indelingen uit te vouwen. Ga als volgt de [richtlijnen](../Concepts/data-sources-supported.md) voor het ophalen van gegevens op basis van uw documenttype. 

Veelgestelde vragen over pagina's moeten in het algemeen zijn zelfstandige en niet gecombineerd met andere informatie. Producthandleidingen moeten wissen koppen en bij voorkeur een indexpagina hebben. 

## <a name="chit-chat"></a>Chit Chat
Chit chat toevoegen aan uw bot, maak uw bot aantrekkelijke, en meer conversatie met kleine hoeveelheid werk. U kunt eenvoudig chit chat gegevenssets voor 3 vooraf gedefinieerde persoonlijkheden toevoegen bij het maken van uw KB en deze op elk gewenst moment wijzigen. Meer informatie over het [chit chat toevoegen aan uw KB](../How-To/chit-chat-knowledge-base.md). 

### <a name="choosing-a-personality"></a>Een persoonlijkheid kiezen
Chit chat wordt ondersteund voor 3 vooraf gedefinieerde persoonlijkheden: 

|Persoonlijkheden|
|--|
|De medewerker|
|De vriend|
|De Comic|

De antwoorden variëren van formele tot informele en vast. U kunt de persoonlijkheid die dichtst is afgestemd op de toon die u wilt gebruiken voor uw bot moet selecteren. U kunt de gegevenssets weergeven en kies een die als basis voor uw bot fungeert en pas vervolgens de antwoorden. 

### <a name="edit-bot-specific-questions"></a>Bot-specifieke vragen bewerken
Er zijn enkele bot-specifieke vragen die deel uitmaken van de gegevensset chit chat en algemene antwoorden zijn is ingevuld. Wijzig deze vragen te beantwoorden om de details van uw bot beste weer te geven. 

U wordt aangeraden de volgende chit chat vragen en antwoorden supereenvoudig specifieker maken:

* Wie bent u?
* Wat kunt u doen?
* Hoe oud bent u?
* Die u hebt gemaakt?
* Hallo!
   

## <a name="rankingscoring"></a>Rangschikking/scoren
Zorg ervoor dat u het beste gebruik van de classificatie-functies die biedt ondersteuning voor QnA Maker wilt maken. In dat geval zal de verbeteren de kans dat een bepaalde gebruiker-query wordt beantwoord met een juiste reactie.

### <a name="choosing-a-threshold"></a>Het kiezen van een drempelwaarde
De standaard-betrouwbaarheidsscore die wordt gebruikt als een drempelwaarde is 50, maar u dit voor uw KB op basis van uw behoeften wijzigen kunt. Omdat elke KB anders is, moet u testen en kies de drempelwaarde die is het beste geschikt is voor uw KB. Meer informatie over de [betrouwbaarheidsscore](../Concepts/confidence-score.md). 


### <a name="add-alternate-questions"></a>Alternatieve vragen toevoegen
[Andere vragen](../How-To/edit-knowledge-base.md) verbeteren de kans op een overeenkomst met de aanvraag voor een gebruiker. Andere vragen zijn nuttig wanneer er zijn meerdere manieren waarop deze vraag kan worden gevraagd. Deze kunt wijzigingen in de zinsstructuur en word-stijl opnemen.

|Oorspronkelijke query|Alternatieve query 's|Wijzigen| 
|--|--|--|
|Is de vervangende domeinpagina beschikbaar?|Hebt u Auto-park?|zinsstructuur|
 |Hallo|Yo<br>Hallo er!|Word-stijl en taalgebruik|

### <a name="use-metadata-filters"></a>Filters gebruiken om metagegevens
[Metagegevens](../How-To/edit-knowledge-base.md) voegt de mogelijkheid om de resultaten van de gebruikersquery van een op basis van filters vast te stellen. De knowledge base-antwoord kan verschillen op basis van de metadata-tag, zelfs als de query hetzelfde is. Bijvoorbeeld, *"waar is vervangende domeinpagina zich"* een ander antwoord kan hebben als de locatie van de vertakking restaurant - dat wil zeggen, de metagegevens is *locatie: Seattle* versus *locatie: Redmond*.

### <a name="use-synonyms"></a>Synoniemen gebruiken
Hoewel er enkele ondersteuning van synoniemen in het Engels, gebruikt u [wijzigingen in word](https://westus.dev.cognitive.microsoft.com/docs/services/5a93fcf85b4ccd136866eb37/operations/5ac266295b4ccd1554da75fd) synoniemen toevoegen aan trefwoorden die andere vorm. Synoniemen moeten worden toegevoegd op de QnA Maker-serviceniveau en gedeeld door alle knowledge bases in de service.

|Oorspronkelijke word|Synoniemen|
|--|--|
|Kopen|kopen<br>netbanking<br>NET bankieren|

### <a name="use-distinct-words-to-differentiate-questions"></a>Gebruik verschillende woorden te onderscheiden van vragen
QnA Maker overeenkomst en positie algoritmen, die overeenkomen met de gebruikersquery van een met een vraag in het knowledge base, werken het beste als elke vraag verschillende nodig adressen. Herhaling van hetzelfde woord in te stellen tussen vragen vermindert de kans dat het juiste antwoord is gekozen voor een bepaalde gebruikersquery met deze woorden. 

Bijvoorbeeld, mogelijk hebt u twee afzonderlijke vragen en antwoorden supereenvoudig met de volgende vragen:

|Vragen en antwoorden supereenvoudig|
|--|
|waar is de vervangende domeinpagina *locatie*|
|waar is de atm *locatie*|

Omdat deze twee vragen en antwoorden supereenvoudig worden geformuleerd met zeer gelijksoortige woorden, deze gelijkenis kan leiden tot vergelijkbaar scores voor veel query's van gebruikers die zijn geformuleerd, zoals *"waar is de `<x>` locatie '*. In plaats daarvan probeert duidelijk onderscheid te maken met query's zoals *"waar is de vervangende domeinpagina partij"* en *"waar is de atm"*, door te vermijden om woorden als 'locatie' die mogelijk veel vragen in uw KB. 


## <a name="collaborate"></a>Samenwerken
QnA Maker kunnen gebruikers [samenwerken](../How-to/collaborate-knowledge-base.md) op een knowledge base. Gebruikers moeten toegang hebben tot de resourcegroep Azure QnA Maker voor toegang tot knowledge bases. Sommige organisaties willen uitbesteden de kennisdatabase bewerken en het onderhoud, en nog steeds mogelijk om toegang tot hun Azure-resources te beveiligen. Dit model editor goedkeurder wordt gedaan door het instellen van twee identieke [QnA Maker services](../How-to/set-up-qnamaker-service-azure.md) in verschillende abonnementen en selecteer een van de cyclus bewerken te testen. Nadat het testen is voltooid, de knowledge base-inhoud worden overgedragen met een [import / export](../Tutorials/migrate-knowledge-base.md) verwerken met de QnA Maker-service van de fiatteur die ten slotte wordt de kennisdatabase publiceren en bijwerken van het eindpunt.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een kennisdatabase bewerken](../How-to/edit-knowledge-base.md)
