---
title: Veelgestelde vragen over de Language Understanding (LUIS) in Azure | Microsoft Docs
description: Vind antwoorden op veelgestelde vragen over Language Understanding (LUIS)
author: diberry
manager: cjgronlund
services: cognitive-services
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 07/26/2018
ms.author: diberry
ms.openlocfilehash: a266055c7685dca87d985775584a9ea7a4bda883
ms.sourcegitcommit: 2ad510772e28f5eddd15ba265746c368356244ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/28/2018
ms.locfileid: "43128517"
---
# <a name="language-understanding-faq"></a>Veelgestelde vragen over de Language Understanding

In dit artikel bevat antwoorden op veelgestelde vragen over Language Understanding (LUIS).

## <a name="luis-authoring"></a>LUIS ontwerpen

### <a name="what-are-the-luis-best-practices"></a>Wat zijn de aanbevolen procedures van LUIS? 
Beginnen met de [ontwerpen cyclus](luis-concept-app-iteration.md), leest u de [aanbevolen procedures](luis-concept-best-practices.md). 

### <a name="what-is-the-best-way-to-start-building-my-app-in-luis"></a>Wat is de beste manier om te beginnen met het samenstellen van mijn app in LUIS?

De beste manier om uw app te bouwen, is via een [incrementele proces](luis-concept-app-iteration.md). 

### <a name="what-is-a-good-practice-to-model-the-intents-of-my-app-should-i-create-more-specific-or-more-generic-intents"></a>Wat is een goede gewoonte om het model van de intenties van mijn app? Moet ik specifieker of een meer algemene intents maken?

Kies intents die niet dus algemeen zijn worden overlappende, maar niet zo specifiek die het maakt het moeilijk voor LUIS onderscheid maken tussen vergelijkbare intents. Het maken van discriminative specifieke intents is een van de aanbevolen procedures voor het modelleren van LUIS.

### <a name="is-it-important-to-train-the-none-intent"></a>Is het belangrijk om te trainen de intentie geen?

Ja, is het raadzaam om het trainen van uw **geen** intentie met meer uitingen als u meer labels aan andere intents toevoegen. Een goede verhouding is 1 of 2 labels op **geen** voor elke 10 labels toegevoegd aan een doel. Deze verhouding verhoogt de discriminative kracht van LUIS.

### <a name="how-can-i-correct-spelling-mistakes-in-utterances"></a>Hoe kan ik in het corrigeren van spelfouten in uitingen?

Zie de [Bing spellingcontrole controleren-API-versie 7](luis-tutorial-bing-spellcheck.md) zelfstudie. LUIS afgedwongen limieten opgelegd door Bing spellingcontrole controleren-API-versie 7. 

### <a name="how-do-i-edit-my-luis-app-programmatically"></a>Hoe kan ik mijn LUIS-app via een programma bewerken?
Als u wilt uw LUIS-app via een programma bewerken, gebruikt u de [API ontwerpen](https://aka.ms/luis-authoring-apis). Zie [aanroepen LUIS API ontwerpen](./luis-quickstart-node-add-utterance.md) en [een LUIS-App via een programma met behulp van Node.js](./luis-tutorial-node-import-utterances-csv.md) voor voorbeelden van hoe u de API ontwerpen aan te roepen. De API ontwerpen vereist het gebruik van een [ontwerpen sleutel](luis-concept-keys.md#authoring-key) in plaats van een eindpuntsleutel. Programmatische ontwerpen kan maximaal 1.000.000 aanroepen per maand en 5 transacties per seconde. Zie voor meer informatie over de sleutels die u met LUIS gebruikt [sleutels beheren](./luis-concept-keys.md).

### <a name="where-is-the-pattern-feature-that-provided-regular-expression-matching"></a>Waar is de functie patroon die reguliere expressie opgegeven die overeenkomt met?
De vorige **patroonfunctie** momenteel is afgeschaft, vervangen door  **[patronen](luis-concept-patterns.md)**. 

### <a name="how-do-i-use-an-entity-to-pull-out-the-correct-data"></a>Hoe gebruik ik een entiteit voor het ophalen van de juiste gegevens? 
Zie [entiteiten](luis-concept-entity-types.md) en [gegevensextractie](luis-concept-data-extraction.md).

### <a name="should-variations-of-an-example-utterance-include-punctuation"></a>Moeten variaties van een voorbeeld-utterance leestekens bevatten? 
De verschillende variaties als voorbeeld uitingen met de intent toevoegen of het patroon van de voorbeeld-utterance met toevoegen de [syntaxis voor het negeren](luis-concept-patterns.md#pattern-syntax) de leestekens. 

### <a name="does-luis-currently-support-cortana"></a>Ondersteunt LUIS momenteel Cortana?

Cortana vooraf gebouwde apps zijn afgeschaft in 2017. Ze worden niet meer ondersteund. 

## <a name="luis-endpoint"></a>LUIS-eindpunt

### <a name="why-does-luis-add-spaces-to-the-query-around-or-in-the-middle-of-words"></a>Waarom LUIS spaties toevoegen aan de query rond of in het midden woorden
LUIS [basis van woordgrenzen](luis-glossary.md#token) de utterance op basis van de [cultuur](luis-supported-languages.md#tokenization). Zowel de oorspronkelijke waarde en de waarde van de tokens zijn beschikbaar voor [gegevensextractie](luis-concept-data-extraction.md#tokenized-entity-returned).

### <a name="how-do-i-create-and-assign-a-luis-endpoint-key"></a>Hoe ik maken en toewijzen van een LUIS eindpuntsleutel?
[Maken van de eindpuntsleutel](luis-how-to-azure-subscription.md#create-luis-endpoint-key) in Azure voor uw [service](https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/) niveau. [Toewijzen van de sleutel](luis-how-to-manage-keys.md#assign-endpoint-key) op de **[publiceren](luis-how-to-publish-app.md)** pagina. Er is geen overeenkomende API voor deze actie. Vervolgens moet u de HTTP-aanvraag naar het eindpunt naar [gebruikt u de nieuwe eindpuntsleutel](luis-concept-keys.md#use-endpoint-key-in-query).

### <a name="how-do-i-interpret-luis-scores"></a>Hoe ik LUIS scores worden geïnterpreteerd? 
Uw systeem moet de hoogste score intentie, ongeacht de waarde ervan gebruiken. Bijvoorbeeld, betekent een score lager (minder dan 50%) 0,5 niet noodzakelijkerwijs dat LUIS weinig vertrouwen heeft. Biedt meer trainingsgegevens, kan u helpen verhogen van de score van de meest waarschijnlijke intentie.

### <a name="why-dont-i-see-my-endpoint-hits-in-my-apps-dashboard"></a>Waarom zie ik geen mijn treffers eindpunt in het Dashboard van mijn app?
De totale eindpunt treffers in uw app Dashboard worden regelmatig bijgewerkt, maar de metrische gegevens die zijn gekoppeld aan uw LUIS-eindpuntsleutel in Azure portal vaker worden bijgewerkt. 

Als er geen bijgewerkte eindpunt treffers in het Dashboard, meld u aan bij de Azure-portal en de resource die is gekoppeld aan de eindpuntsleutel LUIS zoeken en openen **metrische gegevens** selecteren de **totaal aantal aanroepen** metrische gegevens. Als de eindpuntsleutel wordt gebruikt voor meer dan één LUIS-app, ziet u de metrische gegevens in Azure portal het cumulatieve aantal aanroepen van alle LUIS-apps die worden gebruikt.

### <a name="my-luis-app-was-working-yesterday-but-today-im-getting-403-errors-i-didnt-change-the-app-how-do-i-fix-it"></a>Mijn app LUIS werkte gisteren, maar vandaag ik krijg 403 fouten. Kan ik niet hebt de app gewijzigd. Hoe herstel ik deze? 
Na de [instructies](#how-do-i-create-and-assign-a-luis-endpoint-key) in de volgende veelgestelde vragen over het maken van een LUIS-eindpuntsleutel en deze toewijzen aan de app. Vervolgens moet u de HTTP-aanvraag naar het eindpunt naar [gebruikt u de nieuwe eindpuntsleutel](luis-concept-keys.md#use-endpoint-key-in-query).

### <a name="how-do-i-secure-my-luis-endpoint"></a>Hoe beveilig ik mijn eindpunt LUIS? 
Zie [beveiligen van het eindpunt](luis-concept-security.md#securing-the-endpoint).

## <a name="working-within-luis-limits"></a>U werkt binnen de grenzen van LUIS

### <a name="what-is-the-maximum-number-of-intents-and-entities-that-a-luis-app-can-support"></a>Wat is het maximum aantal intenties en entiteiten dat een LUIS-app kan worden ondersteund?
Zie de [grenzen](luis-boundaries.md) verwijzing.

### <a name="i-want-to-build-a-luis-app-with-more-than-the-maximum-number-of-intents-what-should-i-do"></a>Ik wil een LUIS-app ontwikkelen met meer dan het maximum aantal intents. Wat moet ik doen?

Zie [aanbevolen procedures voor intents](luis-concept-intent.md#if-you-need-more-than-the-maximum-number-of-intents).

### <a name="i-want-to-build-an-app-in-luis-with-more-than-the-maximum-number-of-entities-what-should-i-do"></a>Ik wil een app bouwen in LUIS met meer dan het maximum aantal entiteiten. Wat moet ik doen?

Zie [aanbevolen procedures voor entiteiten](luis-concept-entity-types.md#if-you-need-more-than-the-maximum-number-of-entities)

### <a name="what-are-the-limits-on-the-number-and-size-of-phrase-lists"></a>Wat zijn de limieten van het aantal en grootte van woordgroep bevat?
Voor de maximale lengte van een [woordgroepenlijst](./luis-concept-feature.md), Zie de [grenzen](luis-boundaries.md) verwijzing.

### <a name="what-are-the-limits-on-example-utterances"></a>Wat zijn de limieten op voorbeeld uitingen?
Zie de [grenzen](luis-boundaries.md) verwijzing.

## <a name="testing-and-training"></a>Testen en trainen

### <a name="i-see-some-errors-in-the-batch-testing-pane-for-some-of-the-models-in-my-app-how-can-i-address-this-problem"></a>Ik zie een aantal fouten in het deelvenster voor enkele van de modellen in mijn app testen batch. Hoe kan ik dit probleem oplossen?

De fouten erop duiden dat er een discrepantie tussen de labels en de voorspellingen op basis van uw modellen. Om het probleem op te lossen, kunt u een of beide van de volgende taken:
* Om te helpen verbeteren discriminatie tussen intents LUIS, kunt u meer labels toevoegen.
* Toevoegen om te helpen sneller meer LUIS, woordgroep-lijst met functies die domeinspecifieke vocabulaire introduceren.

Zie de [Batch testen](luis-tutorial-batch-testing.md) zelfstudie.

### <a name="when-an-app-is-exported-then-reimported-into-a-new-app-with-a-new-app-id-the-luis-prediction-scores-are-different-why-does-this-happen"></a>Wanneer een app is geëxporteerd en geïmporteerd in een nieuwe app (met een nieuwe app-ID), is de voorspelling LUIS scores zijn verschillend. Waarom gebeurt dit? 

Zie [voorspelling verschillen tussen exemplaren van dezelfde app](luis-concept-prediction-score.md#differences-with-predictions).

## <a name="app-publishing"></a>App-publicatie

### <a name="what-is-the-tenant-id-in-the-add-a-key-to-your-app-window"></a>Wat is de tenant-ID in het venster 'Toevoegen een sleutel aan uw app'?
In Azure vertegenwoordigt een tenant de client of organisatie die is gekoppeld aan een service. Uw tenant-ID niet vinden in Azure portal in de **map-ID** selectievakje in naast selecteren **Azure Active Directory** > **beheren**  >  **Eigenschappen**.

![Tenant-ID in Azure portal](./media/luis-manage-keys/luis-assign-key-tenant-id.png)

<a name="why-are-there-more-subscription-keys-on-my-apps-publish-page-than-i-assigned-to-the-app"></a>
### <a name="why-are-there-more-endpoint-keys-on-my-apps-publish-page-than-i-assigned-to-the-app"></a>Waarom zijn er meer eindpunt sleutels op mijn app pagina publiceren dan ik heb toegewezen aan de app? 
Elke LUIS-app heeft ontwerpen/starter sleutel. LUIS-eindpunt sleutels gemaakt tijdens de periode van algemene beschikbaarheid worden weergegeven op uw pagina publiceren weergegeven, ongeacht of u ze hebt toegevoegd aan de app. Dit is gedaan om de migratie van de algemene beschikbaarheid te vereenvoudigen. Geen nieuwe LUIS eindpunt sleutels worden niet weergegeven op de pagina publiceren. 

## <a name="app-management"></a>App-beheer

### <a name="how-do-i-transfer-ownership-of-a-luis-app"></a>Hoe ik het eigendom overdraagt van een LUIS-app?
Als u wilt een LUIS-app overzetten naar een ander Azure-abonnement, de LUIS-app exporteren en importeren met behulp van een nieuw account. Werk de LUIS-app-ID in de clienttoepassing die wordt aangeroepen. De nieuwe app mogelijk enigszins LUIS scores geretourneerd uit de oorspronkelijke app. 

### <a name="how-do-i-download-a-log-of-user-utterances"></a>Hoe kan ik een logboek van de gebruiker uitingen downloaden?
Uw LUIS-app registreert standaard uitingen van gebruikers. Als u wilt een logboek van uitingen die gebruikers naar uw LUIS-app verzenden downloaden, gaat u naar **mijn Apps**, en klik op het weglatingsteken (***...*** ) in de aanbieding voor uw app. Klik vervolgens op **eindpunt-logboeken exporteren**. Het logboek wordt opgemaakt als een bestand met door komma's gescheiden waarden (CSV).

### <a name="how-can-i-disable-the-logging-of-utterances"></a>Hoe kan ik de registratie van uitingen uitschakelen?
U kunt de registratie van de gebruiker uitingen uitschakelen door in te stellen `log=false` in de eindpunt-URL die uw clienttoepassing maakt gebruik van LUIS query. Echter, uw LUIS-app kunnen voorstellen uitingen of prestaties die gebaseerd op het uitschakelen van logboekregistratie uitgeschakeld [actief leren](luis-concept-review-endpoint-utterances.md#what-is-active-learning). Als u instelt `log=false` vanwege problemen van de privacy van gegevens, kan u een record van deze gebruiker uitingen van LUIS downloaden of die uitingen gebruiken voor het verbeteren van uw app.

Logboekregistratie is de enige opslag van uitingen. 

### <a name="why-dont-i-want-all-my-endpoint-utterances-logged"></a>Waarom niet ik wilt dat alle van de eindpunt-uitingen aangemeld?
Als u van het logboek voor de voorspelling analyse gebruikmaakt, test-uitingen in het logboek niet vastlegt.

## <a name="data-management"></a>Gegevensbeheer

### <a name="can-i-delete-data-from-luis"></a>Kan ik gegevens verwijderen van LUIS? 

* U kunt altijd voorbeeld uitingen die wordt gebruikt voor het trainen van LUIS verwijderen. Als u een voorbeeld-utterance uit uw LUIS-app verwijdert, wordt verwijderd uit de LUIS-webservice en is niet beschikbaar voor het exporteren.
* U kunt uitingen verwijderen uit de lijst met gebruikers-uitingen die LUIS voorgesteld in de **bekijken eindpunt uitingen** pagina. Uitingen verwijderen uit deze lijst voorkomt dat deze wordt voorgesteld, maar niet verwijderd van Logboeken.
* Als u een account verwijdert, worden alle apps verwijderd, samen met hun voorbeeld uitingen en Logboeken. De gegevens worden bewaard op de servers gedurende 60 dagen voordat deze worden definitief verwijderd.

### <a name="does-microsoft-access-my-luis-app-data-for-its-own-purposes-for-example-to-enhance-luis-or-microsoft-in-general"></a>Microsoft toegang tot mijn LUIS-app-gegevens voor een eigen doeleinden, bijvoorbeeld, ter verbetering van de LUIS of Microsoft in het algemeen? 

Nee. Het gegevensmodel van de LUIS-app wordt niet gebruikt door LUIS voor het verbeteren van LUIS als een platform of gebruikt door Microsoft op geen enkele manier. De gegevens van elke app staat los en eigendom zijn alleen door de gebruiker en de deelnemers. 

Meer informatie over [gebruikersprivacy](luis-user-privacy.md), [extra beveiliging, naleving](luis-concept-security.md#security-compliance), en [gegevensopslag](luis-concept-data-storage.md).

## <a name="language-and-translation-support"></a>Ondersteuning voor taal en vertaling 

### <a name="i-have-an-app-in-one-language-and-want-to-create-a-parallel-app-in-another-language-what-is-the-easiest-way-to-do-so"></a>Ik heb een app in één taal en een parallelle app maken in een andere taal wilt. Wat is de eenvoudigste manier om dit te doen?
1. Exporteer uw app.
2. De gelabelde uitingen in de JSON-bestand van de geëxporteerde app naar de doel-taal vertalen.
3. Mogelijk moet u de namen van de intenties en entiteiten wijzigen of ze laten zoals ze zijn.
4. Ten slotte de app met een LUIS-app in de doeltaal importeren.

## <a name="app-notification"></a>App-melding

### <a name="why-did-i-get-an-email-saying-im-almost-out-of-quota"></a>Waarom krijg ik een e-mailbericht dat ik heb bijna buiten het quotum?
Uw sleutel ontwerpen/starter is alleen toegestaan voor 1000 eindpunt query's per maand. Maak een LUIS-eindpuntsleutel (gratis of betaald) en gebruikt deze sleutel als eindpunt query's. Als u endpoint-query's vanuit een bot of een andere clienttoepassing maakt, moet u de LUIS-eindpuntsleutel er wijzigen. 

## <a name="integrating-luis"></a>LUIS integreren

### <a name="where-is-my-luis-app-created-during-the-azure-web-app-bot-subscription-process"></a>Waar wordt mijn LUIS-app gemaakt tijdens de Azure-web-app-bot-abonnement?
Als u een LUIS-sjabloon selecteren en selecteer de **Selecteer** knop in het deelvenster van de sjabloon, het deelvenster aan de linkerkant om op te nemen van het type van de sjabloon wordt gewijzigd en u wordt gevraagd in welke regio de LUIS-sjabloon wilt maken. De web-app-bot proces maken echter een LUIS-abonnement niet.

![LUIS sjabloon web-app-bot regio](./media/luis-faq/web-app-bot-location.png)

### <a name="what-luis-regions-support-bot-framework-speech-priming"></a>In welke regio LUIS ondersteuning voor Bot Framework spraak voorbereiden?
[Spraak voorbereiden](https://docs.microsoft.com/bot-framework/bot-service-manage-speech-priming) wordt alleen ondersteund voor LUIS-apps in het centrale (VS)-exemplaar. 

## <a name="luis-service"></a>LUIS-service 

### <a name="is-luis-available-on-premises-or-in-private-cloud"></a>LUIS on-premises beschikbaar is of in een privécloud?
Nee. 


### <a name="at-the-build-2018-conference-i-heard-about-a-language-understanding-feature-or-demo-but-i-dont-remember-what-it-was-called"></a>Ik gehoord over een functie voor Language Understanding of een demo maar ik niet meer weet wat het werd aangeroepen op de Build-conferentie 2018? 

De volgende functies zijn uitgebracht op de Build 2018-conferentie:

|Naam|Inhoud|
|--|--|
|Verbeteringen|[Reguliere expressie](luis-concept-data-extraction.md##regular-expression-entity-data) entiteit en [sleutel woordgroep](luis-concept-data-extraction.md#key-phrase-extraction-entity-data) entiteit
|Patronen|Patronen [concept](luis-concept-patterns.md), [zelfstudie](luis-tutorial-pattern.md), [procedures](luis-how-to-model-intent-pattern.md)<br>[Patterns.Any](luis-concept-entity-types.md) entiteit met inbegrip van concept [expliciete lijst](luis-concept-patterns.md#explicit-lists) voor uitzonderingen<br>[Rollen](luis-concept-roles.md) concept|
|Integraties|[Tekstanalyse](https://docs.microsoft.com/azure/cognitive-services/text-analytics/) integratie van [sentimentanalyse](luis-how-to-publish-app.md#enable-sentiment-analysis)<br>[Spraak](https://docs.microsoft.com/azure/cognitive-services/speech) integratie van spraak voorbereiden in combinatie met [spraak SDK](https://aka.ms/SpeechSDK)|
|Hulpprogramma voor verzending|Onderdeel van [BotBuilder-hulpprogramma's](https://github.com/Microsoft/botbuilder-tools), verzending vanaf de opdrachtregel [hulpprogramma](luis-concept-enterprise.md#when-you-need-to-combine-several-luis-and-qna-maker-apps) meerdere LUIS en QnA Maker apps combineren tot één LUIS-app voor betere intentieherkenning in een Bot

Aanvullende ontwerpen [API-routes](https://github.com/Microsoft/LUIS-Samples/blob/master/authoring-routes.md) zijn opgenomen. 

Video's: 
* [Azure Friday Build 2018: Cognitive Services - taal (LUIS)](https://channel9.msdn.com/Shows/Azure-Friday/At-Build-2018-Cognitive-Services-Language-LUIS/player)
* [Build 2018 AI Show - wat is er nieuw met Language Understanding Service](https://channel9.msdn.com/Shows/AI-Show/Whats-New-with-Language-Understanding-Service-LUIS/player)
* [Build 2018-sessie - Bot-intelligentie, spraakmogelijkheden en aanbevolen NLU-procedures](https://channel9.msdn.com/events/Build/2018/BRK3208)
* [Build 2018 - LUIS-Updates](https://channel9.msdn.com/events/Build/2018/THR3118/player)

Projecten: 
* [Contoso Cafe bot](https://github.com/botbuilderbuild2018/build2018demo) demo - broncode van Github

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over LUIS, de volgende bronnen:
* [Stack Overflow vragen voorzien van LUIS](https://stackoverflow.com/questions/tagged/luis)
* [MSDN-Language Understanding Intelligent Services (LUIS)-Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=LUIS) 