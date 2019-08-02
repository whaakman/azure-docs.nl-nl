---
title: Een Conversation Learner model maken met behulp van node. js-Microsoft Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Meer informatie over het maken van een Conversation Learner model met behulp van node. js.
services: cognitive-services
author: nitinme
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: 7ab32fb421a2c0db72652d1bbf12d312bffd5d1e
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68706539"
---
# <a name="create-a-conversation-learner-model-using-nodejs"></a>Een Conversation Learner model maken met behulp van node. js

Conversation Learner vermindert de complexiteit van het bouwen van bots. Hiermee kan een werk stroom voor het uitvoeren van een hybride ontwikkelings code en machine learning de hoeveelheid code die nodig is voor het schrijven van bots, worden verminderd. Bepaalde vaste delen van uw model, zoals controleren of de gebruiker is aangemeld of een API-aanvraag voor het controleren van de winkel voorraad, kunnen nog steeds worden gecodeerd. Andere wijzigingen in de status en de gekozen actie kunnen echter worden geleerd uit voorbeeld dialoogvensters van het domein expert of Developer.

## <a name="invitation-required"></a>Uitnodiging vereist

*Er is een uitnodiging vereist om toegang te krijgen tot project Conversation Learner.*

Project Conversation Learner bestaat uit een SDK die u toevoegt aan uw bot en een Cloud service die de SDK voor machine learning heeft geopend.  Op dit moment is een uitnodiging vereist voor toegang tot de micro soft-Cloud service voor project conversaties.  Als u nog niet bent uitgenodigd, [vraagt u een uitnodiging](https://aka.ms/conversation-learner-request-invite)aan.  Als u geen uitnodiging hebt ontvangen, hebt u geen toegang tot de Cloud-API.

## <a name="prerequisites"></a>Vereisten

- Knoop punt 8.5.0 of hoger en NPM 5.3.0 of hoger. Installeren vanaf [https://nodejs.org](https://nodejs.org).
  
- LUIS-ontwerp sleutel:

  1. Meld u [https://www.luis.ai](https://www.luis.ai)aan.

  2. Klik op uw naam in de rechter bovenhoek en klik vervolgens op instellingen

  3. De ontwerp sleutel wordt weer gegeven op de resulterende pagina

  (Uw LUIS-ontwerp sleutel heeft twee rollen.  Ten eerste zal het fungeren als uw Conversation Learner-ontwerp sleutel.  Ten tweede Conversation Learner gebruikt LUIS voor het uitpakken van de entiteit. de LUIS-ontwerp sleutel wordt gebruikt om namens u LUIS modellen te maken.

- Google Chrome-webbrowser. Installeren vanaf [https://www.google.com/chrome/index.html](https://www.google.com/chrome/index.html).

- Git. Installeren vanaf [https://git-scm.com/downloads](https://git-scm.com/downloads).

- VSCode. Installeren vanaf [https://code.visualstudio.com/](https://code.visualstudio.com/). Houd er rekening mee dat dit niet vereist is.

## <a name="quick-start"></a>Snel starten 

1. Installeren en bouwen:

    ```bash    
    git clone https://github.com/Microsoft/ConversationLearner-Samples cl-bot-01
    cd cl-bot-01
    npm install
    npm run build
    ```

    > [!NOTE]
    > Tijdens `npm install`kunt u deze fout negeren als deze zich voordoet:`gyp ERR! stack Error: Can't find Python executable`

2. Configureren:

   Maak een bestand met `.env` de naam in `cl-bot-01`de map.  De inhoud van het bestand moet zijn:

   ```
   NODE_ENV=development
   LUIS_AUTHORING_KEY=<your LUIS authoring key>
   ```

3. Begin bot:

    ```
    npm start
    ```

    Hiermee wordt de algemene lege bot uitgevoerd `cl-bot-01/src/app.ts`in.

3. Open browser om`http://localhost:3978`

U gebruikt nu Conversation Learner en kunt een Conversation Learner model maken en leren.  

> [!NOTE]
> Bij het starten is project Conversation Learner beschikbaar per uitnodiging.  Als `http://localhost:3978/ui` er een http `403` -fout wordt weer gegeven, betekent dit dat uw account niet is uitgenodigd.  [Vraag een uitnodiging](https://aka.ms/conversation-learner-request-invite)aan.

## <a name="tutorials-demos-and-switching-between-bots"></a>Zelf studies, demo's en scha kelen tussen bots

De instructie hierboven heeft de algemene lege bot gestart.  U kunt in plaats daarvan een zelf studie of demo-bot uitvoeren:

1. Als u de Web-UI van Conversation Learner hebt geopend, keert u terug naar `http://localhost:3978/ui/home`de lijst met modellen op.
    
2. Als een andere bot wordt uitgevoerd ( `npm start` bijvoorbeeld `npm run demo-pizza`of), stopt u deze.  U hoeft het gebruikers interface proces niet te stoppen of de webbrowser te sluiten.

3. Voer een demo-bot uit vanaf de opdracht regel (stap 2 hierboven).  Demo's zijn onder andere:

   ```bash
   npm run tutorial-general
   npm run tutorial-entity-detection
   npm run tutorial-session-callbacks
   npm run tutorial-api-calls
   npm run tutorial-hybrid
   npm run demo-password
   npm run demo-pizza
   npm run demo-storage
   ```

4. Als u dit nog niet hebt gedaan, gaat u naar de Conversation Learner Web-UI `http://localhost:3978/ui/home`in Chrome door te laden. 

5. Klik op zelf studies importeren en selecteer het demo model in de Conversation Learner gebruikers interface die overeenkomt met de demo die u hebt gestart.

De bron bestanden voor de demo's bevinden zich in`cl-bot-01/src/demos`

## <a name="create-a-bot-which-includes-back-end-code"></a>Een bot maken met back-end-code

1. Als u de Web-UI van Conversation Learner hebt geopend, keert u terug naar `http://localhost:3978/ui/home`de lijst met modellen op.
    
2. Als er een bot wordt uitgevoerd ( `npm run demo-pizza`bijvoorbeeld), stopt u deze.  U hoeft het gebruikers interface proces niet te stoppen of de webbrowser te sluiten.

3. Bewerk desgewenst code in `cl-bot-01/src/app.ts`.

4. Bot opnieuw samen stellen en opnieuw starten:

    ```bash    
    npm run build
    npm start
    ```

5. Als u dit nog niet hebt gedaan, gaat u naar de Conversation Learner Web-UI `http://localhost:3978/ui/home`in Chrome door te laden. 

6. Maak een nieuw Conversation Learner model in de gebruikers interface en start het onderwijs.

7. Als u code wijzigingen wilt `cl-bot-01/src/app.ts`aanbrengen in, herhaalt u de bovenstaande stappen, beginnend bij stap 2.

## <a name="vscode"></a>VSCode

In VSCode zijn er uitvoerings configuraties voor elke demo en voor de ' lege bot ' in `cl-bot-01/src/app.ts`.  Open de `cl-bot-01` map in VSCode.

## <a name="advanced-configuration"></a>Geavanceerde configuratie

Er wordt een sjabloon `.env.example` bestand weer gegeven met de omgevings variabelen die u kunt instellen voor het configureren van de voor beelden.

U kunt deze poorten aanpassen om conflicten te voor komen tussen andere services die worden uitgevoerd op uw `.env` computer door een bestand toe te voegen aan de hoofdmap van het project:

```bash
cp .env.example .env
```

Hierbij wordt gebruikgemaakt van de standaard configuratie, waarmee u uw bot lokaal kunt uitvoeren, en u begint met het gebruik van Conversation Learner.  (Later op om uw bot op het bot-Framework te implementeren, zijn enkele bewerkingen van dit bestand vereist.)

## <a name="support"></a>Ondersteuning

- Vraag een label op [stack overflow](https://stackoverflow.com) met ' micro soft cognitieve '
- Een functie aanvragen op onze [gebruikers spraak pagina](https://aka.ms/conversation-learner-uservoice)
- Open een probleem op onze [github-opslag plaats](https://github.com/Microsoft/ConversationLearner-Samples)

## <a name="contributing"></a>Draagt

Op dit project is de [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/) (Microsoft Open Source-gedragscode) van toepassing. Raadpleeg voor meer informatie de [Microsoft Open Source Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) (Veelgestelde vragen over de Microsoft Open Source-gedragscode). Als u aanvullende vragen of opmerkingen hebt, neemt u contact op met [opencode@microsoft.com](mailto:opencode@microsoft.com).

## <a name="source-repositories"></a>Bron opslagplaatsen

- [conversationlearner-voor beelden](https://github.com/Microsoft/ConversationLearner-Samples)
- [conversationlearner-sdk](https://github.com/Microsoft/ConversationLearner-SDK)
- [conversationlearner-models](https://github.com/Microsoft/ConversationLearner-Models)
- [conversationlearner-ui](https://github.com/Microsoft/ConversationLearner-UI)
- [conversationlearner-webchat](https://github.com/Microsoft/ConversationLearner-WebChat)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Hallo mensen](./tutorials/01-hello-world.md)
