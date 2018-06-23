---
title: Language Understanding (LUIS) in Azure Veelgestelde vragen | Microsoft Docs
description: Vind antwoorden op veelgestelde vragen over Language Understanding (LUIS)
author: v-geberr
manager: kaiqb
services: cognitive-services
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 05/07/2018
ms.author: v-geberr
ms.openlocfilehash: fd63ffd312e3ac17a6376eb3c9bef8f1978e3935
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "36333612"
---
# <a name="language-understanding-faq"></a>Language Understanding-Veelgestelde vragen

In dit artikel bevat antwoorden op veelgestelde vragen over Language Understanding (LUIS).

## <a name="luis-authoring"></a>LUIS ontwerpen

### <a name="what-are-the-luis-best-practices"></a>Wat zijn de best practices LUIS? 
Beginnen met de [Authoring cyclus](luis-concept-app-iteration.md), leest u de [aanbevolen procedures](luis-concept-best-practices.md). 

### <a name="what-is-the-best-way-to-start-building-my-app-in-luis"></a>Wat is de beste manier om het bouwen van mijn app in LUIS starten?

De beste manier om uw app te bouwen is via een [incrementele proces](luis-concept-app-iteration.md). 

### <a name="what-is-a-good-practice-to-model-the-intents-of-my-app-should-i-create-more-specific-or-more-generic-intents"></a>Wat is het verstandig als model voor de intentie van mijn app? Moet ik meer specifieke of meer algemene intents maken?

Kies intents die niet dus algemeen zijn garantie worden overlappende, maar niet zo specifiek dat maakt het moeilijk voor LUIS onderscheid maken tussen vergelijkbare intents. Discriminative specifieke intents maken, is een van de aanbevolen procedures voor het modelleren LUIS.

### <a name="is-it-important-to-train-the-none-intent"></a>Is het belangrijk dat u de opzet geen trainen?

Ja, is het raadzaam om de training van uw **geen** opzet met meer utterances als u meer labels aan andere intents toevoegen. Een goede verhouding is 1 of 2-labels toegevoegd aan **geen** voor elke 10 labels toegevoegd aan de opzet. Deze verhouding verhoogt de discriminative macht LUIS.

### <a name="how-can-i-correct-spelling-mistakes-in-utterances"></a>Hoe los ik spellingcontrole uit te voeren in utterances

Zie de [Bing spellen controleren API V7](luis-tutorial-bing-spellcheck.md) zelfstudie. LUIS limieten opgelegd door Bing spellen controleren API V7 afgedwongen. 

### <a name="how-do-i-edit-my-luis-app-programmatically"></a>Hoe kan ik mijn app LUIS programmatisch bewerken?
Kunt u uw app LUIS programmatisch bewerken de [Authoring API](https://aka.ms/luis-authoring-apis). Zie [aanroepen LUIS authoring API](./luis-quickstart-node-add-utterance.md) en [een LUIS App programmatisch met behulp van Node.js](./luis-tutorial-node-import-utterances-csv.md) voor voorbeelden van hoe het ontwerpen-API aan te roepen. De API Authoring vereist het gebruik van een [authoring sleutel](luis-concept-keys.md#authoring-key) in plaats van een eindpuntsleutel. Programmatische authoring kan maximaal 1.000.000 aanroepen per maand en vijf transacties per seconde. Zie voor meer informatie over de sleutels die u met LUIS gebruikt [sleutels beheren](./luis-concept-keys.md).

### <a name="where-is-the-pattern-feature-that-provided-regular-expression-matching"></a>Waar is de opgegeven reguliere expressie patroon-functie die overeenkomt met?
De vorige **patroonfunctie** momenteel is afgeschaft, vervangen door  **[patronen](luis-concept-patterns.md)**. 

### <a name="how-do-i-use-an-entity-to-pull-out-the-correct-data"></a>Hoe gebruik ik een entiteit voor het ophalen van de juiste gegevens? 
Zie [entiteiten](luis-concept-entity-types.md) en [ophalen van gegevens](luis-concept-data-extraction.md).

### <a name="should-variations-of-an-example-utterance-include-punctuation"></a>Variaties van een voorbeeld utterance leestekens bevatten? 
De verschillende variaties als voorbeeld utterances naar de bedoeling toevoegen of toevoegen van het patroon van de utterance voorbeeld met de [syntaxis voor het negeren van](luis-concept-patterns.md#pattern-syntax) de leestekens. 

## <a name="luis-endpoint"></a>LUIS eindpunt

### <a name="why-does-luis-add-spaces-to-the-query-around-or-in-the-middle-of-words"></a>Waarom wordt LUIS spaties toegevoegd aan de query rond of in het midden woorden
LUIS [basis van woordgrenzen](luis-glossary.md#token) de utterance op basis van de [cultuur](luis-supported-languages.md#tokenization). Zowel de oorspronkelijke waarde en de waarde van de tokens zijn beschikbaar voor [ophalen van gegevens](luis-concept-data-extraction.md#tokenized-entity-returned).

### <a name="how-do-i-create-and-assign-a-luis-endpoint-key"></a>Hoe maken en toewijzen van een LUIS eindpuntsleutel?
[Maak de eindpuntsleutel](luis-how-to-azure-subscription.md#create-luis-endpoint-key) in Azure voor uw [service](https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/) niveau. [Toewijzen van de sleutel](Manage-keys.md#assign-endpoint-key) op de **[publiceren](publishapp.md)** pagina. Er is geen overeenkomstige API voor deze actie. Vervolgens moet u de HTTP-aanvraag op het eindpunt naar [gebruik van de nieuwe eindpuntsleutel](luis-concept-keys.md#use-endpoint-key-in-query).

### <a name="how-do-i-interpret-luis-scores"></a>Hoe interpreteer ik LUIS scores? 
Uw systeem, moet de hoogste scoreprofiel bedoeling ongeacht de waarde ervan gebruiken. Bijvoorbeeld, betekent een score hieronder 0,5 (minder dan 50%) niet noodzakelijkerwijs dat LUIS lage vertrouwen heeft. Biedt meer trainingsgegevens kan u helpen de score van de meest waarschijnlijke bedoeling verhogen.

### <a name="why-dont-i-see-my-endpoint-hits-in-my-apps-dashboard"></a>Waarom zie mijn eindpunt treffers in mijn app Dashboard niet?
De totale eindpunt treffers in uw app-Dashboard worden regelmatig bijgewerkt, maar de metrische gegevens die zijn gekoppeld aan uw abonnement LUIS sleutel in de Azure portal vaker worden bijgewerkt. 

Als er geen bijgewerkte eindpunt treffers in het Dashboard, aanmelden bij de Azure-portal en de bron die is gekoppeld aan uw abonnement LUIS sleutel vinden en openen **metrische gegevens** selecteren de **totaal aantal aanroepen** metriek. Als de abonnementssleutel wordt gebruikt voor meer dan één LUIS app, toont de metrische gegevens in de Azure portal het cumulatieve aantal aanroepen vanuit alle LUIS apps die worden gebruikt.

### <a name="my-luis-app-was-working-yesterday-but-today-im-getting-403-errors-i-didnt-change-the-app-how-do-i-fix-it"></a>Mijn app LUIS werkte gisteren maar tegenwoordig ik krijg 403 fouten. Ik de app is niet gewijzigd. Hoe kan ik oplossen? 
Na de [instructies](#how-do-i-create-and-assign-a-luis-endpoint-key) in de volgende veelgestelde vragen over het maken van een eindpunt LUIS sleutel en deze toewijzen aan de app. Vervolgens moet u de HTTP-aanvraag op het eindpunt naar [gebruik van de nieuwe eindpuntsleutel](luis-concept-keys.md#use-endpoint-key-in-query).

### <a name="how-do-i-secure-my-luis-endpoint"></a>Hoe worden mijn LUIS-eindpunt kunt beveiligen? 
Zie [beveiligen van het eindpunt](luis-concept-security.md#securing-the-endpoint).

## <a name="working-within-luis-limits"></a>Binnen de grenzen LUIS werkt

### <a name="what-is-the-maximum-number-of-intents-and-entities-that-a-luis-app-can-support"></a>Wat is het maximum aantal intents en entiteiten die ondersteuning voor een app LUIS biedt?
Zie de [grenzen](luis-boundaries.md) verwijzing.

### <a name="i-want-to-build-a-luis-app-with-more-than-the-maximum-number-of-intents-what-should-i-do"></a>Ik wil een App LUIS met meer dan het maximum aantal intents. Wat moet ik doen?

Zie [Best practices voor intents](luis-concept-intent.md#if-you-need-more-than-the-maximum-number-of-intents).

### <a name="i-want-to-build-an-app-in-luis-with-more-than-the-maximum-number-of-entities-what-should-i-do"></a>Ik wil een app in LUIS met meer dan het maximum aantal entiteiten maken. Wat moet ik doen?

Zie [aanbevolen procedures voor entiteiten](luis-concept-entity-types.md#if-you-need-more-than-the-maximum-number-of-entities)

### <a name="what-are-the-limits-on-the-number-and-size-of-phrase-lists"></a>Wat zijn de limieten van het aantal en grootte van woordgroep bevat?
Voor de maximale lengte van een [woordgroepenlijst](./luis-concept-feature.md), Zie de [grenzen](luis-boundaries.md) verwijzing.

### <a name="what-are-the-limits-on-example-utterances"></a>Wat zijn de gebruiksbeperkingen voorbeeld utterances?
Zie de [grenzen](luis-boundaries.md) verwijzing.

## <a name="testing-and-training"></a>Testen en training

### <a name="i-see-some-errors-in-the-batch-testing-pane-for-some-of-the-models-in-my-app-how-can-i-address-this-problem"></a>Er is een aantal fouten in de batch deelvenster voor sommige modellen in mijn app testen. Hoe kan ik dit probleem oplossen?

De fouten erop duiden dat er een discrepantie is tussen de labels en de voorspellingen van uw modellen. Om het probleem op te lossen, voert u een of beide van de volgende taken:
* Om te helpen verbeteren onderscheid tussen intents LUIS, kunt u meer labels toevoegen.
* Om te helpen LUIS leert sneller, woordgroep lijst onderdelen toevoegen die domeinspecifieke woordenlijst introduceren.

Zie de [tests in Batch](luis-tutorial-batch-testing.md) zelfstudie.

### <a name="when-an-app-is-exported-then-reimported-into-a-new-app-with-a-new-app-id-the-luis-prediction-scores-are-different-why-does-this-happen"></a>Wanneer een app is geëxporteerd en geïmporteerd in een nieuwe app (met een nieuwe app-ID), is de voorspelling LUIS scores zijn verschillend. Waarom komt dat? 

Zie [voorspelling verschillen tussen de exemplaren van dezelfde app](luis-concept-prediction-score.md#differences-with-predictions).

## <a name="app-publishing"></a>App-publicatie

### <a name="what-is-the-tenant-id-in-the-add-a-key-to-your-app-window"></a>Wat is de tenant-ID in het venster "Een sleutel aan uw app toevoegen"?
In Azure vertegenwoordigt een tenant de client of organisatie die is gekoppeld aan een service. Uw tenant-ID niet vinden in de Azure-portal in de **map-ID** vak door te selecteren **Azure Active Directory** > **beheren**  >  **Eigenschappen**.

![Tenant-ID in de Azure-portal](./media/luis-manage-keys/luis-assign-key-tenant-id.png)

### <a name="why-are-there-more-subscription-keys-on-my-apps-publish-page-than-i-assigned-to-the-app"></a>Waarom zijn er meer abonnement sleutels op mijn app publiceren pagina dan ik toegewezen aan de app? 
Elke app LUIS heeft authoring/starter sleutel. LUIS abonnement sleutels gemaakt tijdens de tijdsperiode GA zijn zichtbaar op de pagina publiceren ongeacht als u deze hebt toegevoegd aan de app. Dit is gedaan om de migratie GA eenvoudiger. Nieuwe LUIS abonnement sleutels worden niet weergegeven op de pagina publiceren. 

## <a name="app-management"></a>Appbeheer

### <a name="how-do-i-transfer-ownership-of-a-luis-app"></a>Hoe ik het eigendom overdraagt van een app LUIS?
Als u wilt een app LUIS overbrengen naar een ander Azure-abonnement, de app LUIS exporteren en importeren met behulp van een nieuw account. Werk de LUIS app-ID in de clienttoepassing waarin deze wordt aangeroepen. De nieuwe app mogelijk enigszins verschillen LUIS scores geretourneerd uit de oorspronkelijke app. 

### <a name="how-do-i-download-a-log-of-user-utterances"></a>Hoe kan ik een logboek van de gebruiker utterances downloaden?
Standaard wordt in uw app LUIS utterances registreert van gebruikers. Als u wilt een logboek van utterances die gebruikers naar uw app LUIS verzenden downloaden, gaat u naar **mijn Apps**, en klik op het weglatingsteken (***...*** ) in het overzicht voor uw app. Klik vervolgens op **exporteren eindpunt logboeken**. Het logboek is opgemaakt als een bestand met door komma's gescheiden waarden (CSV).

### <a name="how-can-i-disable-the-logging-of-utterances"></a>Hoe kan ik de logboekregistratie van utterances uitschakelen?
U kunt de logboekregistratie van gebruiker utterances uitschakelen door in te stellen `log=false` in de eindpunt-URL die uw clienttoepassing voor query LUIS gebruikt. Het uitschakelen van logboekregistratie uitgeschakeld echter de mogelijkheid van uw app LUIS utterances voorstellen of verbeteren de prestaties die gebaseerd op [active learning](luis-concept-review-endpoint-utterances.md#what-is-active-learning). Als u instelt `log=false` vanwege de privacy van gegevens weergegeven, u kunt downloaden van een record van deze gebruiker utterances van LUIS of deze utterances gebruiken om uw app te verbeteren.

Logboekregistratie is de enige opslag van utterances. 

### <a name="why-dont-i-want-all-my-endpoint-utterances-logged"></a>Waarom niet ik wil dat alle mijn eindpunt utterances vastgelegd?
Als u het logboek voor analyse van de voorspelling gebruikt, worden niet test utterances in het logboek worden vastgelegd.

## <a name="data-management"></a>Gegevensbeheer

### <a name="can-i-delete-data-from-luis"></a>Kan ik gegevens uit LUIS verwijderen? 

* U kunt altijd voorbeeld utterances gebruikt voor het trainen van LUIS verwijderen. Als u een voorbeeld utterance van uw app LUIS verwijdert, wordt verwijderd uit de webservice LUIS en is niet beschikbaar voor exporteren.
* U kunt utterances verwijderen uit de lijst met gebruikers utterances die LUIS wordt voorgesteld in de **bekijken eindpunt utterances** pagina. Utterances verwijderen uit deze lijst voorkomt dat deze wordt voorgesteld, maar niet verwijderen van Logboeken.
* Als u een account verwijdert, worden alle apps verwijderd, samen met hun voorbeeld utterances en de logboeken. De gegevens wordt behouden op de servers gedurende 60 dagen voordat deze wordt permanent verwijderd.

## <a name="language-and-translation-support"></a>Taal en NAT-ondersteuning 

### <a name="i-have-an-app-in-one-language-and-want-to-create-a-parallel-app-in-another-language-what-is-the-easiest-way-to-do-so"></a>Ik heb een app in één taal en wilt maken van een parallelle app in een andere taal. Wat is de eenvoudigste manier om dit te doen?
1. Exporteer uw app.
2. De gelabelde utterances in het JSON-bestand van de geëxporteerde app de doel-taal vertalen.
3. Mogelijk moet u de namen van de intents en entiteiten wijzigen of ze laten zoals ze zijn.
4. Ten slotte, importeer de app met een LUIS-app in de doel-taal.

## <a name="app-notification"></a>App-melding

### <a name="why-did-i-get-an-email-saying-im-almost-out-of-quota"></a>Waarom krijg ik een e-mailbericht dat ik ben bijna buiten quotum
Uw ontwerp/starter-sleutel is alleen toegestaan voor 1000 eindpunt vraagt een maand. Een abonnementssleutel LUIS (gratis of betaalde) maken en gebruiken van deze sleutel bij het doorvoeren van endpoint-query's. Als u eindpunt query's vanuit een bot of een andere clienttoepassing doorvoert, moet u de er LUIS endpoint-sleutel wijzigen. 

## <a name="integrating-luis"></a>Integratie van LUIS

### <a name="where-is-my-luis-app-created-during-the-azure-web-app-bot-subscription-process"></a>Waar wordt mijn app LUIS gemaakt tijdens het Azure-web-app bot abonnement?
Als u een sjabloon LUIS selecteren en selecteer de **Selecteer** knop in het deelvenster van de sjabloon, het linkerdeelvenster om op te nemen van het type van de sjabloon wordt gewijzigd en u wordt gevraagd in welke regio de LUIS-sjabloon wilt maken. Het proces van web-app bot maken echter een abonnement LUIS niet.

![LUIS sjabloon web app bot regio](./media/luis-faq/web-app-bot-location.png)

### <a name="what-luis-regions-support-bot-framework-speech-priming"></a>Welke gebieden LUIS ondersteuning Bot Framework spraak voorbereiden?
[Spraak voorbereiden](https://docs.microsoft.com/bot-framework/bot-service-manage-speech-priming) wordt alleen ondersteund voor LUIS apps in het centrale (V.S.)-exemplaar. 

## <a name="luis-service"></a>LUIS service 

### <a name="is-luis-available-on-premise-or-in-private-cloud"></a>LUIS beschikbare on-premises is of in een privécloud?
Nee. 

## <a name="changes-to-the-docs"></a>Wijzigingen in de documenten

### <a name="where-did-the-tutorials-go"></a>Waar zijn de zelfstudies gebleven? 
De artikelen die eerder in de zelfstudie sectie zijn worden nu in de sectie procedures van de documenten. 

|Zelfstudie|
|--|
|LUIS integreren met een bot met [C#](luis-csharp-tutorial-build-bot-framework-sample.md) en [Node.js](luis-nodejs-tutorial-build-bot-framework-sample.md)|
|Application Insights toevoegen aan een Bot met [C#](luis-tutorial-bot-csharp-appinsights.md) en [Node.js](luis-tutorial-function-appinsights.md)|
|Een LUIS App programmatisch met behulp van [Node.js](luis-tutorial-node-import-utterances-csv.md)|
|Gebruik [samengestelde entiteit](luis-tutorial-composite-entity.md) gegroepeerde gegevens moeten worden opgehaald|
|Voeg [entiteit lijst](luis-tutorial-list-entity.md) voor verbeterde entiteit detectie met behulp van Node.js|
|Voorspelling nauwkeuriger met een [woordgroepenlijst](luis-tutorial-interchangeable-phrase-list.md), [patronen](luis-tutorial-pattern.md), en [batch testen](luis-tutorial-batch-testing.md)|
|[Spelling corrigeren](luis-tutorial-batch-testing.md) met Bing spellen controleren API v7

### <a name="at-the-build-2018-conference-i-heard-about-a-language-understanding-feature-or-demo-but-i-dont-remember-what-it-was-called"></a>Ik heb gehoord een Language Understanding-functie of demo op de Build 2018 conferentie, maar ik niet meer weet wat het werd aangeroepen? 

De volgende functies zijn uitgebracht op de Build 2018 conferentie:

|Naam|Inhoud|
|--|--|
|Verbeteringen|[Reguliere expressie](luis-concept-data-extraction.md##regular-expression-entity-data) entiteit en [sleutel woordgroep](luis-concept-data-extraction.md#key-phrase-extraction-entity-data) entiteit
|Patronen|Patronen [concept](luis-concept-patterns.md), [zelfstudie](luis-tutorial-pattern.md), [instructies](luis-how-to-model-intent-pattern.md)<br>[Patterns.Any](luis-concept-entity-types.md) entiteit concept inclusief [expliciete lijst](luis-concept-patterns.md#explicit-lists) voor uitzonderingen<br>[Rollen](luis-concept-roles.md) concept|
|Integraties|[Tekstanalyse](https://docs.microsoft.com/azure/cognitive-services/text-analytics/) integratie van [gevoel analyse](publishapp.md#enable-sentiment-analysis)<br>[Spraak](https://docs.microsoft.com/azure/cognitive-services/speech) integratie van [spraak voorbereiden](publishapp.md#enable-speech-priming) in combinatie met [Speech SDK](https://aka.ms/SpeechSDK)|
|Hulpprogramma voor verzending|Onderdeel van [BotBuilder-tools](https://github.com/Microsoft/botbuilder-tools), opdrachtregel Dispatch [hulpprogramma](luis-concept-enterprise.md#when-you-need-to-combine-several-luis-and-qna-maker-apps) meerdere LUIS en QnA Maker apps combineren tot één LUIS app voor betere opzet opname in een Bot

Aanvullende authoring [API routes](https://github.com/Microsoft/LUIS-Samples/blob/master/authoring-routes.md) zijn opgenomen. 

Videos: 
* [Azure vrijdag om Build 2018: Cognitieve Services - taal (LUIS)](https://channel9.msdn.com/Shows/Azure-Friday/At-Build-2018-Cognitive-Services-Language-LUIS/player)
* [Build 2018 AI Show - wat is er nieuw met Language Understanding-Service](https://channel9.msdn.com/Shows/AI-Show/Whats-New-with-Language-Understanding-Service-LUIS/player)
* [2018 sessie build - Bot intelligence spraakmogelijkheden en NLU aanbevolen procedures](https://channel9.msdn.com/events/Build/2018/BRK3208)
* [2018 build - LUIS Updates](https://channel9.msdn.com/events/Build/2018/THR3118/player)

Projecten: 
* [Contoso Cafe bot](https://github.com/botbuilderbuild2018/build2018demo) demo - broncode op Github

## <a name="next-steps"></a>Volgende stappen

Zie de volgende bronnen voor meer informatie over LUIS:
* [Stack-overloop vragen gelabeld met LUIS](https://stackoverflow.com/questions/tagged/luis)
* [MSDN-taal begrijpen Intelligent Services-Forum (LUIS)](https://social.msdn.microsoft.com/forums/azure/home?forum=LUIS) 

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website
