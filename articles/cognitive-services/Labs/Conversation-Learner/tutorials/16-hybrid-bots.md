---
title: Conversation Learner gebruiken met andere bot-bouw technologieën-Microsoft Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Meer informatie over het gebruik van Conversation Learner met andere build-technologieën voor bot.
services: cognitive-services
author: mattm
manager: larsliden
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 07/13/2018
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: c964c62c34f952a547d077e93e7bb4d0eb7b192d
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68703683"
---
# <a name="how-to-use-conversation-learner-with-other-bot-building-technologies"></a>Conversation Learner gebruiken met andere bot-bouw technologieën

In deze zelf studie wordt beschreven hoe u Conversation Learner kunt gebruiken met andere bot-bouw technologieën en hoe het geheugen (of de status) kan worden gedeeld tussen deze technologieën. 

## <a name="video"></a>Video

[![Preview van hybride bots zelf studie](https://aka.ms/cl_Tutorial_v3_Hybrid_Applications_Preview)](https://aka.ms/cl_Tutorial_v3_Hybrid_Applications)

## <a name="requirements"></a>Vereisten
In deze zelf studie moet de bot-emulator worden gebruikt om dialoog vensters voor logboeken te maken, niet in de webgebruikersinterface van het logboek. Meer informatie over het instellen van de bot Framework emulator is [hier](https://docs.microsoft.com/azure/bot-service/bot-service-debug-emulator?view=azure-bot-service-4.0)beschikbaar. 

Voor deze zelf studie moet de Hybrid zelf studie bot worden uitgevoerd:

    npm run tutorial-hybrid

## <a name="details"></a>Details

Terwijl Conversation Learner besturings element is, moet alle status ten opzichte van de Conversation Learner-sessie worden opgeslagen in het geheugen beheer van de Conversation Learner. Dit is nodig omdat de machine learning de status gebruikt om te bepalen hoe de conversatie moet worden gebrand. Externe status kan worden door gegeven aan de Conversation Learner in de OnSessionStartCallback die wordt aangeroepen wanneer de sessie wordt gestart. De interne status kan worden geretourneerd door de OnSessionEndCallback wanneer de sessie wordt beëindigd.

U kunt Conversation Learner nadenken als een functie aanroep waarbij een begin status wordt gemaakt en waarden worden geretourneerd.

In dit voor beeld maakt u een hybride bot met behulp van twee verschillende systemen:
1. Een Conversation Learner model <br/>
    Maakt gebruik van het leer model van de conversatie om de volgende actie van de bot te bepalen op basis van de huidige sessie. In dit deel van de bot wordt één van de begin `isOpen` statussen gebruikt (waarmee wordt aangegeven of een archief is geopend of gesloten) en een `purchaseItem` ander stuk staat (de naam van een item dat door de gebruiker wordt gekocht) wordt geretourneerd.

2. Tekst zoeken <br />
    U kunt gewoon zoeken naar inkomende tekst voor specifieke teken reeksen en reageert. Dit deel van de bot beheert de andere opslag mechanismen van het bots en is verantwoordelijk voor het starten van de LC-sessie. Met name worden er drie variabelen `usingConversationLearner`beheerd `storeIsOpen`:, `purchaseItem`en.

Laten we eens kijken naar het model dat wordt gebruikt in deze demo.

### <a name="open-the-demo"></a>De demo openen

Klik in de webgebruikersinterface op zelf studies importeren en selecteer het model met de naam ' zelf studie-16-HybridBot '.

## <a name="entities"></a>Entiteiten

Open de pagina entiteiten en noteer twee entiteiten: `isOpen` en`purchaseItem`

Als u wilt weten hoe deze entiteiten worden gebruikt, opent u `C:\<installedpath>\src\demos\tutorialHybrid.ts` het bestand: om de retour aanroepen te bekijken.

U ziet dat de code `OnSessionStartCallback` in de `storeIsOpen` waarde van van BotBuilder-conversatie opslag als de waarde van `isOpen` de entiteit kopieert, zodat deze beschikbaar is voor Conversation learner. Zie de volgende code:

![](../media/tutorial17_sessionstart.PNG)

De code in `OnSessionEndCallback` (als de sessie is beëindigd vanwege een geleerde activiteit en niet alleen een time-out), kopieert ook de `purchaseItem` waarde van entiteit uit `purchaseItem`naar BotBuilder Storage. Zie de volgende code:

![](../media/tutorial17_sessionend.PNG)

Laten we nu de acties eens bekijken.

## <a name="actions"></a>Acties

U ziet dat het model vier acties heeft.

De beoogde regels voor de acties zijn als volgt:

- Als de `isOpen` entiteit is ingesteld, wordt de bot gevraagd "wat wilt u kopen?" en sla het op in `puchaseItem` de sleuf.
- Als `isOpen` niet is ingesteld, krijgt de bot de melding ' Ik ben bezig met sluiten '.
- De andere twee acties zijn van het type `END_SESSION`.
- De END_SESSION-actie geeft aan ConversationLearner dat de conversatie is voltooid.

### <a name="overall-bot-logic"></a>Algemene bot-logica

Eerst ziet u dat als de vlag status van `usingConversationLearner` de bot is ingesteld, het besturings element wordt door gegeven aan Conversation learner. Als dat niet het geval is, wordt het besturings element door gegeven aan iets anders.  In dit voor beeld wordt eenvoudige tekst overeenkomst weer gegeven, maar dit kan elke andere bot-technologie zijn, waaronder LUIS, QnA maker en zelfs een andere instantie van Conversation Learner.

We hebben een manier nodig om de Store te openen en sluiten, dus we voeren een teken reeks uit met ' open Store ' en ' Store sluiten ' en stellen de vlag ' storeIsOpen ' in.

We hebben nu een manier nodig om de beschik bare controle naar ons Conversation Learner model te activeren. Wanneer we overeenkomen met de teken reeks ' winkelen ', gaan we het volgende doen:
- Stel de `usingConversationLearner` vlag in het geheugen van de bot in.
- Roep de methode ' StartSession ' aan op ons Conversation Learner model.  Hiermee wordt de "onSessionStartCallback" geactiveerd waarmee de `isOpen` entiteits waarde wordt geïnitialiseerd

Zie hieronder:

![](../media/tutorial17_useConversationLearner.PNG)

We hebben ook een tekst die overeenkomt met ' geschiedenis ', waarmee het laatste aankoop item wordt weer gegeven.
Ten slotte, als er iets anders is getypt, worden de beschik bare gebruikers opdrachten weer gegeven

## <a name="train-dialog"></a>Dialoog venster trainen

Voor deze zelf studie is het model al vooraf getraind.  We testen de volledige bot om het effect van de retour aanroepen voor starten en beëindigen van de sessie in de praktijk te bekijken.

## <a name="testing-the-bot"></a>De bot testen

In tegens telling tot een niet-gereduceerd model bots in één gesprek kunt u dit niet testen in de Conversation Learner-gebruikers interface, omdat alleen kan worden weer gegeven wat er door het Conversation Learner model wordt verwerkt.

### <a name="install-the-bot-framework-emulator"></a>De bot Framework-emulator installeren

- Ga naar [https://github.com/Microsoft/BotFramework-Emulator](https://github.com/Microsoft/BotFramework-Emulator).
- Down load en installeer de emulator.

### <a name="configure-the-emulator"></a>De emulator configureren

- Open de emulator en zorg ervoor dat de URL is gericht op dezelfde poort waarop uw bot wordt uitgevoerd. Vaak`http://localhost:3978/api/messages`

### <a name="test"></a>Testen 

#### <a name="scenario-1-store-is-closed"></a>Scenario 1: De Store is gesloten
1. Voer ' Shop ' in. Dit wordt afgehandeld door de tekst overeenkomst en geeft de controle over het Conversation Learner model.
2. Voer ' Hallo ' in.  Omdat `isOpen` de waarde niet is ingesteld, krijgt de bot de melding ' Ik vind dat we zijn gesloten ' en beëindigt hij de sessie.

#### <a name="scenario-2-store-is-open"></a>Scenario 2: De Store is geopend
1. Voer ' open Store ' in.  Hiermee wordt de `isOpen` waarde ingesteld op waar.
1. Voer ' Shop ' in.
1. Voer ' Hallo ' in.  Omdat `isOpen` waarde is ingesteld op True, zegt de bot ' wat wilt u kopen? '
1. Voer ' stoel ' in. ' stoel ' wordt als entiteit `purchaseItem`opgeslagen in LC-geheugen. De call back van de eind sessie wordt aangeroepen, waarbij deze waarde naar het conversatie archief wordt gekopieerd.
1. Voer ' geschiedenis ' in.  De bot zegt u ' u hebt stoel ', omdat dit uw laatste `purchaseItem`is.

## <a name="conclusion"></a>Conclusie

Met wat u hierboven hebt geleerd, zou u Conversation Learner kunnen combi neren met andere bouw technologie voor bot.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Vertakkingen en ongedaan maken](./17-branch-undo.md)
