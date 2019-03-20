---
title: Conversatiecursist gebruiken met andere technologieën - Microsoft Cognitive Services bouwen bot | Microsoft Docs
titleSuffix: Azure
description: Informatie over het gebruik van Conversatiecursist met andere technologieën bouwen bot.
services: cognitive-services
author: mattm
manager: larsliden
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 07/13/2018
ms.author: v-jaswel
ms.openlocfilehash: b90fc42aa56bfc813ec464670336dea75cff0f0e
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58117435"
---
# <a name="how-to-use-conversation-learner-with-other-bot-building-technologies"></a>Conversatiecursist gebruiken met andere technologieën bouwen bot

In deze zelfstudie bevat informatie over Conversatiecursist gebruiken met andere bot het bouwen van technologieën en hoe geheugen (of staat) kan worden gedeeld tussen deze technologieën. 

## <a name="video"></a>Video

[![Hybride Bots zelfstudie Preview](https://aka.ms/cl_Tutorial_v3_Hybrid_Applications_Preview)](https://aka.ms/cl_Tutorial_v3_Hybrid_Applications)

## <a name="requirements"></a>Vereisten
Deze zelfstudie vereist log dialoogvensters, niet de Webgebruikersinterface voor logboek dialoogvenster maken met de bot-emulator. Meer informatie over het instellen van de Emulator van Bot Framework vindt [hier](https://docs.microsoft.com/azure/bot-service/bot-service-debug-emulator?view=azure-bot-service-4.0). 

In deze zelfstudie is vereist dat de hybride zelfstudie bot wordt uitgevoerd:

    npm run tutorial-hybrid

## <a name="details"></a>Details

Hoewel Conversatiecursist controle, moet alle statussen ten opzichte van de sessie van de cursist conversatie worden opgeslagen in de Conversatiecursist geheugenbeheer. Dit is nodig omdat de machine learning gebruikmaakt van de status om te bepalen hoe de conversatie station. Externe status kan worden doorgegeven in de Conversatiecursist in de OnSessionStartCallback die wordt aangeroepen wanneer de sessie begint. Interne status kan worden geretourneerd uit door de OnSessionEndCallback wanneer de sessie wordt beëindigd.

U kunt bijna Conversatiecursist beschouwen als een aanroep van de functie die een initiële status neemt en retourneert de waarden.

In dit voorbeeld maakt u een hybride-bot met behulp van twee verschillende systemen:
1. Een Model van de cursist gesprek <br/>
    Conversatie learner model wordt gebruikt om te bepalen van de volgende actie van de bot op basis van de huidige sessie. Dit deel van de bot duurt een stukje beginstatus `isOpen` (waarmee wordt aangegeven of een winkel open of gesloten is) en een andere stukje status retourneert `purchaseItem` (de naam van de gebruiker van een item aankopen).

2. Tekst die overeenkomt met <br />
    Gewoon kijkt naar binnenkomende tekst voor specifieke tekenreeksen en reageert. Dit deel van de bot de Bots beheert andere mechanismen voor opslag en is verantwoordelijk voor het starten van de CL-sessie. Om precies het beheren van drie variabelen: `usingConversationLearner`, `storeIsOpen`, en `purchaseItem`.

Begin met het model dat wordt gebruikt in deze demo bekijken.

### <a name="open-the-demo"></a>Open de demo

In de web-UI, klik op "Zelfstudies importeren" en selecteer het model met de naam '-zelfstudie-16-HybridBot'.

## <a name="entities"></a>Entiteiten

Open de pagina entiteiten en u ziet twee entiteiten: `isOpen` en `purchaseItem`

Om te begrijpen hoe deze entiteiten worden gebruikt, opent u het bestand: `C:\<installedpath>\src\demos\tutorialHybrid.ts` om te kijken naar de callbacks.

U ziet dat de code in `OnSessionStartCallback` kopieert u de waarde van `storeIsOpen` uit BotBuilder conversatie opslag als de waarde van de `isOpen` entiteit, zodat deze beschikbaar om Conversatiecursist is. Zie de volgende code:

![](../media/tutorial17_sessionstart.PNG)

Evenzo, de code in `OnSessionEndCallback` (als de sessie is beëindigd als gevolg van een geleerde-activiteit en niet alleen een time-out) kopieert u de waarde van de entiteit `purchaseItem` out BotBuilder opslag `purchaseItem`. Zie de volgende code:

![](../media/tutorial17_sessionend.PNG)

Nu we de acties bekijken.

## <a name="actions"></a>Acties

U ziet dat het model heeft vier acties.

De beoogde regels voor de acties zijn als volgt:

- Als de `isOpen` entiteit is ingesteld, wordt de Bot vragen "Wat wilt u kopen?" en op te slaan die in de `puchaseItem` sleuf.
- Als `isOpen` niet is ingesteld, de Bot dicteert 'Ik ben je er nu gesloten'.
- De andere twee acties zijn van het type `END_SESSION`.
- De actie END_SESSION wordt aan ConversationLearner wordt aangegeven dat de conversatie is voltooid.

### <a name="overall-bot-logic"></a>Algemene Bot logica

Eerst ziet u dat als de status van de Bot `usingConversationLearner` vlag is ingesteld, we besturingselement doorgeven aan Conversatiecursist. Als dat niet het geval is, geven we de controle op iets anders.  In dit voorbeeld laten we eenvoudige tekst die overeenkomt met, maar dit wordt mogelijk andere Bot-technologie met inbegrip van LUIS, QnA maker en zelfs een ander exemplaar van Conversatiecursist.

We moeten een manier voor de gebruiker om te openen en sluit de winkel, dit doen we een tekenreeks vergelijken met 'open store' en 'sluiten store' en stel de vlag 'storeIsOpen'.

Vervolgens moeten we een manier voor het activeren van snelle controle aan ons Model van de cursist conversatie. Wanneer we met de tekenreeks 'shop overeenkomen' doen we het volgende:
- Stel de `usingConversationLearner` vlag in het geheugen van de Bot.
- Roep de methode 'StartSession' op het Model van de cursist conversatie.  Hiermee wordt de 'onSessionStartCallback' die wordt geïnitialiseerd geactiveerd de `isOpen` entiteitswaarde

Hieronder vindt u:

![](../media/tutorial17_useConversationLearner.PNG)

Ook doen we tekst overeenkomen met "Geschiedenis" welke die laatste aankoop-item wordt weergegeven.
Ten slotte, als u iets anders typt, geven we weer de beschikbare opdrachten

## <a name="train-dialog"></a>Train Dialog

Het model is al vooraf getrainde voor deze zelfstudie.  We zullen de volledige-bot om te zien van het effect van het begin en einde van de callbacks sessie in de praktijk testen.

## <a name="testing-the-bot"></a>De Bot testen

In tegenstelling tot één conversatie Leaner model bots niet mogelijk om dit te testen in de gebruikersinterface van de cursist conversatie zoals u deze alleen kunt zien wat wordt verwerkt door het Model van de cursist conversatie.

### <a name="install-the-bot-framework-emulator"></a>Installeert u de emulator van Bot framework

- Ga naar [https://github.com/Microsoft/BotFramework-Emulator](https://github.com/Microsoft/BotFramework-Emulator).
- Download en installeer de emulator.

### <a name="configure-the-emulator"></a>Configureren van de emulator

- Open de emulator en controleer of dat de URL is gemaakt voor uw bot wordt uitgevoerd op dezelfde poort. Waarschijnlijk: `http://localhost:3978/api/messages`

### <a name="test"></a>Testen 

#### <a name="scenario-1-store-is-closed"></a>Scenario 1: Store is gesloten
1. Voer 'shop'. Dit wordt verwerkt door de tekst die overeenkomt met en krijgt u besturingselement aan het model Conversatiecursist.
2. Voer 'Hallo'.  Omdat `isOpen` waarde niet is ingesteld, wordt de bot dicteert 'Ik ben je er nu gesloten' en de sessie beëindigen.

#### <a name="scenario-2-store-is-open"></a>Scenario 2: Store is geopend
1. Voer 'open store'.  Dit stelt de `isOpen` op ' True '.
1. Voer 'shop'.
1. Voer 'Hallo'.  Omdat `isOpen` waarde is ingesteld op true, de bot dicteert "Wat wilt u kopen?"
1. Voer 'voorzitterschap'. 'stoel' wordt opgeslagen in het geheugen als de entiteit CL `purchaseItem`. De callback van de sessie beëindigen wordt aangeroepen die deze waarde opgehaald uit naar de conversatie-store.
1. Voer 'Geschiedenis.  De bot wordt bijvoorbeeld 'U hebt gekocht stoel' als dit uw laatste is `purchaseItem`.

## <a name="conclusion"></a>Conclusie

Met wat u hierboven hebt geleerd zou het mogelijk om te combineren Conversatiecursist met een andere technologie voor het bouwen van Bot.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Vertakkingen en ongedaan maken](./17-branch-undo.md)
