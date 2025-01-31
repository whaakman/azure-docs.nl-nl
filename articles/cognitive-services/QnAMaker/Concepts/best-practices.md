---
title: Aanbevolen procedures - QnA Maker
titlesuffix: Azure Cognitive Services
description: Gebruik deze aanbevolen procedures te verbeteren van uw knowledge base en biedt betere resultaten van uw toepassing/chat bot-eindgebruikers.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 06/25/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: dd4f85822a5e6615e7ea6e31b4231c04c9d4e88c
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2019
ms.locfileid: "67542857"
---
# <a name="best-practices-of-a-qna-maker-knowledge-base"></a>Best practices van een kennisdatabase QnA Maker

De [kennisdatabase ontwikkelingscyclus](../Concepts/development-lifecycle-knowledge-base.md) helpt u over het beheren van uw KB van begin tot einde. Gebruik deze aanbevolen procedures te verbeteren van uw knowledge base en betere resultaten naar uw clienttoepassing of chat bot van eindgebruikers.

## <a name="extraction"></a>Extractie

De QnA Maker-service wordt voortdurend verbeteren van de algoritmen die vragen en antwoorden supereenvoudig uitpakken van inhoud en de lijst met ondersteunde bestands- en HTML-indelingen uit te vouwen. Ga als volgt de [richtlijnen](../Concepts/data-sources-supported.md) voor het ophalen van gegevens op basis van uw documenttype. 

Veelgestelde vragen over pagina's moeten in het algemeen zijn zelfstandige en niet gecombineerd met andere informatie. Producthandleidingen moeten wissen koppen en bij voorkeur een indexpagina hebben. 

### <a name="configuring-multi-turn"></a>Configureren van meerdere inschakelen

Maak uw knowledge base met meerdere inschakelen extractie ingeschakeld. Als uw knowledge base heeft of vraaghiërarchie moet ondersteunen, kunt u deze hiërarchie opgehaald uit het document of die zijn gemaakt nadat het document wordt opgehaald. 

<!--is this a global setting that can only be configured at kb creation time? -->

## <a name="creating-good-questions-and-answers"></a>Het maken van goede vragen en antwoorden

### <a name="good-questions"></a>Goede vragen

De aanbevolen vragen kunnen eenvoudig. Houd rekening met de sleutel woord of woordgroep voor elke vraag en maakt u een eenvoudige vraag voor deze sleutel of meer woorden. 

Voeg zo veel alternatieve vragen als u nodig hebt, maar de wijzigingen eenvoudig houden toe. Toevoegen van meer woorden of zinsdelen die geen deel uitmaken van het belangrijkste doel van de vraag helpt niet QnA Maker vinden. 


### <a name="add-relevant-alternative-questions"></a>Relevante alternatieve vragen toevoegen

Uw gebruikers vragen met een eigen stijl van tekst kan invoeren `How do I add a toner cartridge to my printer?` of een trefwoord zoals zoeken `toner cartridge`. De knowledge base moet hebben voor zowel stijlen van vragen correct het beste antwoord retourneren. Als u niet zeker weet welke trefwoorden van een klant invoeren, gebruikt u Application Insights-gegevens voor het analyseren van query's.

### <a name="good-answers"></a>Goede antwoorden

De beste antwoorden vindt u eenvoudige antwoorden, maar niet te eenvoudig. Gebruik antwoorden niet zoals `yes` en `no`. Als het antwoord moet aan andere bronnen koppelen of een rijke ervaring met koppelingen en media bieden, gebruikt u [metagegevens tagging](./knowledge-base.md#key-knowledge-base-concepts) onderscheid maken tussen antwoorden, klikt u vervolgens [verzenden van de query](../how-to/metadata-generateanswer-usage.md#generateanswer-request-configuration) met metagegevenstags in de `strictFilters` eigenschap voor de versie van het juiste antwoord.

## <a name="chit-chat"></a>Chit Chat
Chit chat toevoegen aan uw bot, maak uw bot aantrekkelijke, en meer conversatie met kleine hoeveelheid werk. U kunt eenvoudig chit chat gegevenssets uit vooraf gedefinieerde persoonlijkheden toevoegen bij het maken van uw KB en deze op elk gewenst moment wijzigen. Meer informatie over het [chit chat toevoegen aan uw KB](../How-To/chit-chat-knowledge-base.md). 

### <a name="choosing-a-personality"></a>Een persoonlijkheid kiezen
Chit chat wordt ondersteund voor verschillende vooraf gedefinieerde persoonlijkheden: 

|Persoonlijkheid |QnA Maker-gegevensset-bestand |
|---------|-----|
|Professional |[qna_chitchat_professional.tsv](https://qnamakerstore.blob.core.windows.net/qnamakerdata/editorial/qna_chitchat_professional.tsv) |
|Beschrijvende |[qna_chitchat_friendly.tsv](https://qnamakerstore.blob.core.windows.net/qnamakerdata/editorial/qna_chitchat_friendly.tsv) |
|Witty |[qna_chitchat_witty.tsv](https://qnamakerstore.blob.core.windows.net/qnamakerdata/editorial/qna_chitchat_witty.tsv) |
|Caring |[qna_chitchat_caring.tsv](https://qnamakerstore.blob.core.windows.net/qnamakerdata/editorial/qna_chitchat_caring.tsv) |
|Enthousiaste |[qna_chitchat_enthusiastic.tsv](https://qnamakerstore.blob.core.windows.net/qnamakerdata/editorial/qna_chitchat_enthusiastic.tsv) |

De antwoorden variëren van formele tot informele en vast. U kunt de persoonlijkheid die dichtst is afgestemd op de toon die u wilt gebruiken voor uw bot moet selecteren. U vindt de [gegevenssets](https://github.com/Microsoft/BotBuilder-PersonalityChat/tree/master/CSharp/Datasets), en kies een die als basis voor uw bot fungeert, en pas vervolgens de antwoorden. 

### <a name="edit-bot-specific-questions"></a>Bot-specifieke vragen bewerken
Er zijn enkele bot-specifieke vragen die deel uitmaken van de gegevensset chit chat en algemene antwoorden zijn is ingevuld. Wijzig deze vragen te beantwoorden om de details van uw bot beste weer te geven. 

U wordt aangeraden de volgende chit chat vragen en antwoorden supereenvoudig specifieker maken:

* Wie bent u?
* Wat kunt u doen?
* Hoe oud bent u?
* Die u hebt gemaakt?
* Hallo!
   
### <a name="adding-custom-chit-chat-with-a-metadata-tag"></a>Aangepaste chit-chatten met een metadata-tag toevoegen

Als u uw eigen chit chat QnA paren toevoegt, zorg er dan voor dat metagegevens toevoegen, zodat deze vragen te beantwoorden worden geretourneerd. De naam/waarde-paar van metagegevens is `editorial:chitchat`.

## <a name="searching-for-answers"></a>Antwoorden zoeken

GenerateAnswer API maakt gebruik van zowel vragen en het antwoord om te zoeken naar beste antwoorden op query's van een gebruiker.

### <a name="searching-questions-only-when-answer-is-not-relevant"></a>Zoeken naar vragen alleen wanneer het antwoord niet relevant zijn is

Gebruik de [ `RankerType=QuestionOnly` ](#choosing-ranker-type) als u niet wilt om te zoeken naar antwoorden. 

Een voorbeeld hiervan is wanneer de knowledge base een catalogus van acroniemen als vragen met hun volledige formulier als het antwoord is. De waarde van het antwoord helpt niet om te zoeken naar het juiste antwoord.

## <a name="rankingscoring"></a>Rangschikking/scoren
Zorg ervoor dat u het beste gebruik van de classificatie-functies die biedt ondersteuning voor QnA Maker wilt maken. In dat geval zal de verbeteren de kans dat een bepaalde gebruiker-query wordt beantwoord met een juiste reactie.

### <a name="choosing-a-threshold"></a>Het kiezen van een drempelwaarde

De standaardwaarde [betrouwbaarheidsscore](confidence-score.md) die wordt gebruikt als een drempelwaarde 50 is, maar u kunt [de drempel wijzigen](confidence-score.md#set-threshold) voor uw KB op basis van uw behoeften. Omdat elke KB anders is, moet u testen en kies de drempelwaarde die is het beste geschikt is voor uw KB. 

### <a name="choosing-ranker-type"></a>Kerntechnologie type kiezen
Standaard zoekt QnA Maker met vragen en antwoorden. Als u doorzoeken vragen alleen wilt, voor het genereren van een antwoord, gebruikt de `RankerType=QuestionOnly` in de hoofdtekst van bericht van de aanvraag GenerateAnswer.

### <a name="add-alternate-questions"></a>Alternatieve vragen toevoegen
[Andere vragen](../How-To/edit-knowledge-base.md) verbeteren de kans op een overeenkomst met de aanvraag voor een gebruiker. Andere vragen zijn nuttig wanneer er zijn meerdere manieren waarop deze vraag kan worden gevraagd. Deze kunt wijzigingen in de zinsstructuur en word-stijl opnemen.

|Oorspronkelijke query|Alternatieve query 's|Wijzigen| 
|--|--|--|
|Is de vervangende domeinpagina beschikbaar?|Hebt u Auto-park?|zinsstructuur|
 |Hallo|Yo<br>Hallo er!|Word-stijl en taalgebruik|

<a name="use-metadata-filters"></a>

### <a name="use-metadata-tags-to-filter-questions-and-answers"></a>De van metagegevenstags filter vragen en antwoorden gebruiken

[Metagegevens](../How-To/edit-knowledge-base.md) voegt de mogelijkheid om een clienttoepassing op de hoogte moet niet duren voordat alle antwoorden op, maar in plaats daarvan Beperk de resultaten van de gebruikersquery van een op basis van de van metagegevenstags. De knowledge base-antwoord kan verschillen op basis van de metadata-tag, zelfs als de query hetzelfde is. Bijvoorbeeld, *"waar is vervangende domeinpagina zich"* een ander antwoord kan hebben als de locatie van de vertakking restaurant - dat wil zeggen, de metagegevens is *locatie: Seattle* versus *locatie: Redmond*.

### <a name="use-synonyms"></a>Synoniemen gebruiken
Hoewel er enkele ondersteuning van synoniemen in het Engels, gebruikt u niet-hoofdlettergevoelige word wijzigingen via de [wijzigingen API](https://docs.microsoft.com/rest/api/cognitiveservices/qnamaker/alterations/replace) synoniemen toevoegen aan trefwoorden die andere vorm. Synoniemen zijn toegevoegd aan de QnA Maker-serviceniveau en gedeeld door alle knowledge bases in de service.

|Oorspronkelijke word|Synoniemen|
|--|--|
|Kopen|kopen<br>NET-bankieren<br>NET bankieren|

### <a name="use-distinct-words-to-differentiate-questions"></a>Gebruik verschillende woorden te onderscheiden van vragen
QnA Maker van positie-algoritme, die overeenkomt met de gebruikersquery van een met een vraag in het knowledge base, werkt het beste als elke vraag verschillende nodig adressen. Herhaling van hetzelfde woord in te stellen tussen vragen vermindert de kans dat het juiste antwoord is gekozen voor een bepaalde gebruikersquery met deze woorden. 

Bijvoorbeeld, mogelijk hebt u twee afzonderlijke vragen en antwoorden supereenvoudig met de volgende vragen:

|Vragen en antwoorden supereenvoudig|
|--|
|waar is de vervangende domeinpagina *locatie*|
|waar is de ATM *locatie*|

Omdat deze twee vragen en antwoorden supereenvoudig worden geformuleerd met zeer gelijksoortige woorden, deze gelijkenis kan leiden tot vergelijkbaar scores voor veel query's van gebruikers die zijn geformuleerd, zoals *"waar is de `<x>` locatie '* . In plaats daarvan probeert duidelijk onderscheid te maken met query's zoals *"waar is de vervangende domeinpagina partij"* en *"waar is de ATM"* , door te vermijden om woorden als 'locatie' die mogelijk veel vragen in uw KB. 

## <a name="collaborate"></a>Samenwerken
QnA Maker kunnen gebruikers [samenwerken](../How-to/collaborate-knowledge-base.md) op een knowledge base. Gebruikers moeten toegang hebben tot de resourcegroep Azure QnA Maker voor toegang tot knowledge bases. Sommige organisaties willen uitbesteden de kennisdatabase bewerken en het onderhoud, en nog steeds mogelijk om toegang tot hun Azure-resources te beveiligen. Dit model editor goedkeurder wordt gedaan door het instellen van twee identieke [QnA Maker services](../How-to/set-up-qnamaker-service-azure.md) in verschillende abonnementen en selecteer een van de cyclus bewerken te testen. Nadat het testen is voltooid, de knowledge base-inhoud worden overgedragen met een [import / export](../Tutorials/migrate-knowledge-base.md) verwerken met de QnA Maker-service van de fiatteur die ten slotte wordt de kennisdatabase publiceren en bijwerken van het eindpunt.



## <a name="active-learning"></a>Actief leren

[Actief leren](../How-to/improve-knowledge-base.md) biedt de beste taak van het alternatieve vragen stellen wanneer er een breed scala aan kwaliteit en kwantiteit van query's op basis van gebruikers. Het is belangrijk om toe te staan van client-toepassingen gebruikersquery's om deel te nemen in de actieve feedback-lus zonder censuur leren. Als u vragen worden voorgesteld in de QnA Maker portal, kunt u **[filteren op suggesties](../How-To/improve-knowledge-base.md#accept-an-active-learning-suggestion-in-the-knowledge-base)** vervolgens beoordeelt en accepteert of weigert die suggesties. 

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een kennisdatabase bewerken](../How-to/edit-knowledge-base.md)
