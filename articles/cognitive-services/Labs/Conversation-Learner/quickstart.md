---
title: Over het maken van een Conversatiecursist-model met behulp van Node.js - Microsoft Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Informatie over het maken van een Conversatiecursist-model met behulp van Node.js.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 02501cba75bd64d6a09fce671d2949759b5cd577
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55207017"
---
# <a name="create-a-conversation-learner-model-using-nodejs"></a>Maak een Conversatiecursist-model met behulp van Node.js

Conversatiecursist vermindert de complexiteit van het bouwen van bots. Hiermee kunt een hybride ontwikkeling werkstroom kunnen de code incheck- en machine learning om de code die zijn vereist voor het schrijven van bots te verminderen. Bepaalde vaste delen van uw model, zoals controleren als de gebruiker is aangemeld of maken van een API-aanvraag voor het controleren van de store voorraad, kunnen nog steeds worden gecodeerd. Andere wijzigingen in de selectie van de status en actie kunnen echter worden geleerd van voorbeeld-dialoogvensters komt overeen met de domeinexpert of de ontwikkelaar.

## <a name="invitation-required"></a>Inschrijving vereist

*Een uitnodiging is voor toegang tot Project Conversatiecursist vereist.*

Conversatiecursist project bestaat uit een SDK die u toevoegt aan uw bot, en een cloudservice die de SDK heeft toegang tot voor machine learning.  Toegang tot de cloudservice Project conversatie Leaner vereist op dit moment een uitnodiging.  Als u dit nog niet hebt uitgenodigd, [een uitnodiging](https://aka.ms/conversation-learner-request-invite).  Als u niet een uitnodiging hebt ontvangen, moet u geen toegang tot de cloud-API.

## <a name="prerequisites"></a>Vereisten

- Knooppunt 8.5.0 of hoger en npm 5.3.0 of hoger. Installeren van [ https://nodejs.org ](https://nodejs.org).
  
- LUIS authoring sleutel:

  1. Meld u aan bij [ http://www.luis.ai ](http://www.luis.ai).

  2. Klik op uw naam in de rechterbovenhoek, klikt u vervolgens op 'instellingen'

  3. Het ontwerpen van de sleutel wordt weergegeven op de resulterende pagina

  (2 rollen uw LUIS ontwerpen sleutel fungeert.  Eerst fungeren als uw Conversatiecursist sleutel ontwerpen.  Ten tweede Conversatiecursist LUIS gebruikt voor het ophalen van de entiteit; het ontwerpen van sleutel LUIS wordt LUIS om modellen te maken uit uw naam gebruikt)

- Google Chrome webbrowser. Installeren van [ https://www.google.com/chrome/index.html ](https://www.google.com/chrome/index.html).

- git. Installeren van [ https://git-scm.com/downloads ](https://git-scm.com/downloads).

- VSCode. Installeren van [ https://code.visualstudio.com/ ](https://code.visualstudio.com/). Let op: dit wordt aanbevolen, niet vereist.

## <a name="quick-start"></a>Snel starten 

1. Installeren en maken:

    ```bash    
    git clone https://github.com/Microsoft/ConversationLearner-Samples cl-bot-01
    cd cl-bot-01
    npm install
    npm run build
    ```

    > [!NOTE]
    > Tijdens de `npm install`, kunt u deze fout negeren als het zich voordoet: `gyp ERR! stack Error: Can't find Python executable`

2. Configureren:

   Maak een bestand met de naam `.env` in de map `cl-bot-01`.  De inhoud van het bestand moet zijn:

   ```
   NODE_ENV=development
   LUIS_AUTHORING_KEY=<your LUIS authoring key>
   ```

3. Bot starten:

    ```
    npm start
    ```

    Hiermee wordt de algemene leeg bot uitgevoerd `cl-bot-01/src/app.ts`.

3. Conversatiecursist gebruikersinterface worden uitgevoerd:

    ```bash
    [open second command prompt window]
    cd cl-bot-01
    npm run ui
    ```

4. Open de browser naar http://localhost:5050 

U bent nu met Conversatiecursist kunt en maken en een model Conversatiecursist leren.  

> [!NOTE]
> Conversatiecursist Project is starten, beschikbaar op uitnodiging.  Als http://localhost:5050 ziet u een HTTP `403` fout, dit betekent dat uw account is niet uitgenodigd.  Neem [een uitnodiging](https://aka.ms/conversation-learner-request-invite).

## <a name="tutorials-demos-and-switching-between-bots"></a>Zelfstudies, demo's en schakelen tussen bots

De bovenstaande instructies aan de slag met de algemene leeg bot.  Een zelfstudie uitvoeren of bot in plaats daarvan demo:

1. Als u de Conversatiecursist web UI openen hebt, terug naar de lijst met modellen in de http://localhost:5050/home.
    
2. Als een andere bot wordt uitgevoerd (zoals `npm start` of `npm run demo-pizza`), stopt.  U hoeft niet te stoppen van de gebruikersinterface of de webbrowser sluiten.

3. Een demo-bot uitvoeren vanaf de opdrachtregel (stap 2 hierboven).  Demo's zijn onder andere:

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

4. Als u nog niet bent, Ga naar de Conversatiecursist-webgebruikersinterface in Chrome met het laden van http://localhost:5050/home. 

5. Klik op "Zelfstudies importeren" en selecteer de demo-model in de gebruikersinterface van de conversatie-Learner die overeenkomt met de demo die u aan de slag.

Bronbestanden voor de demo's staan in `cl-bot-01/src/demos`

## <a name="create-a-bot-which-includes-back-end-code"></a>Maken van een bot waaronder back-end-code

1. Als u de Conversatiecursist web UI openen hebt, terug naar de lijst met modellen in de http://localhost:5050/home.
    
2. Als een bot wordt uitgevoerd (zoals `npm run demo-pizza`), stopt.  U hoeft niet te stoppen van de gebruikersinterface of de webbrowser sluiten.

3. Indien gewenst, de code in bewerken `cl-bot-01/src/app.ts`.

4. Bouwen en bot opnieuw te starten:

    ```bash    
    npm run build
    npm start
    ```

5. Als u nog niet bent, Ga naar de Conversatiecursist-webgebruikersinterface in Chrome met het laden van http://localhost:5050/home. 

6. Een nieuw Conversatiecursist-model maken in de gebruikersinterface en onderwijs start.

7. Codewijzigingen in aanbrengen `cl-bot-01/src/app.ts`, herhaalt u de stappen vanaf stap 2 hierboven.

## <a name="vscode"></a>VSCode

Vscode, er worden configuraties voor elke demo, en voor de 'leeg 'bot in uitgevoerd `cl-bot-01/src/app.ts`.  Open de `cl-bot-01` VSCode-map.

## <a name="advanced-configuration"></a>Geavanceerde configuratie

Er is een sjabloon `.env.example` bestand ziet u welke omgeving variabelen die u instellen kunt voor het configureren van de voorbeelden.

U kunt aanpassen dat deze worden gebruikt om te voorkomen van conflicten tussen andere services die op uw computer wordt uitgevoerd door toe te voegen een `.env` -bestand naar de hoofdmap van project:

```bash
cp .env.example .env
```

Dit maakt gebruik van de standaardconfiguratie, waarmee u uw bot lokaal uitvoeren, en gaan Conversatiecursist gebruiken.  (Later op voor het implementeren van uw bot voor Bot Framework, enkele bewerkingen van dit bestand is nodig.)

## <a name="support"></a>Ondersteuning

- Voeg aan uw vragen op [Stack Overflow](https://stackoverflow.com) met 'microsoft cognitive'
- Een functie aanvragen op onze [User Voice-pagina](https://aka.ms/conversation-learner-uservoice)
- Open een probleem op onze [GitHub-opslagplaats](https://github.com/Microsoft/ConversationLearner-Samples)

## <a name="contributing"></a>Bij te dragen

Op dit project is de [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/) (Microsoft Open Source-gedragscode) van toepassing. Raadpleeg voor meer informatie de [Microsoft Open Source Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) (Veelgestelde vragen over de Microsoft Open Source-gedragscode). Als u aanvullende vragen of opmerkingen hebt, neemt u contact op met [opencode@microsoft.com](mailto:opencode@microsoft.com).

## <a name="source-repositories"></a>Bron-opslagplaatsen

- [conversationlearner-samples](https://github.com/Microsoft/ConversationLearner-Samples)
- [conversationlearner-sdk](https://github.com/Microsoft/ConversationLearner-SDK)
- [conversationlearner-models](https://github.com/Microsoft/ConversationLearner-Models)
- [conversationlearner-ui](https://github.com/Microsoft/ConversationLearner-UI)
- [conversationlearner-webchat](https://github.com/Microsoft/ConversationLearner-WebChat)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Hallo mensen](./tutorials/01-hello-world.md)
