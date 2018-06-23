---
title: Het maken van een conversatie cursist-toepassing met behulp van Node.js - cognitieve Microsoft-Services | Microsoft Docs
titleSuffix: Azure
description: Informatie over het maken van een conversatie cursist-toepassing met behulp van Node.js.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: a3a51aa86a30b060c8dc4113da69462904d7df54
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345642"
---
# <a name="create-a-conversation-learner-application-using-nodejs"></a>Maak een conversatie cursist-toepassing met behulp van Node.js

Conversatie cursist vermindert de complexiteit van het bouwen van bots. Hiermee kunt een hybride ontwikkeling-werkstroom zodat code hand geschreven en machine learning om Verklein de hoeveelheid code schrijven bots vereist. Bepaalde vaste delen van uw toepassing, zoals het maken van een API-aanvraag naar de store-inventarisatie, Controleer of controleren als de gebruiker is aangemeld kunnen nog steeds worden gecodeerd. Andere wijzigingen in de selectie van de status en actie kunnen echter worden geleerd van voorbeeld dialoogvensters bepaald door de expert domein of de ontwikkelaar.

## <a name="invitation-required"></a>Uitnodiging vereist

*Een uitnodiging is vereist voor toegang tot Project conversatie cursist.*

Project conversatie cursist bestaat uit een SDK die u toevoegt aan uw bot als een cloudservice die de SDK voor machine learning benadert.  Op dit moment vereist toegang tot de cloudservice Project conversatie Leaner een uitnodiging.  Als u dit nog niet hebt gedaan, zijn uitgenodigd [een uitnodiging](https://aka.ms/conversation-learner-request-invite).  Als u een uitnodiging niet hebt ontvangen, kunt u zich geen toegang tot de cloud-API.

## <a name="prerequisites"></a>Vereisten

- Knooppunt 8.5.0 of hoger en npm 5.3.0 of hoger. Installeer vanaf [ https://nodejs.org ](https://nodejs.org).
  
- LUIS authoring sleutel:

  1. Meld u aan bij [ http://www.luis.ai ](http://www.luis.ai).

  2. Klik op uw naam in de rechterbovenhoek, klik op 'instellingen'

  3. Ontwerp van de sleutel wordt weergegeven op de resulterende pagina

  (2 functies uw sleutel authoring LUIS fungeert.  Eerst fungeert deze als uw conversatie cursist sleutel ontwerpen.  Ten tweede conversatie cursist LUIS gebruikt voor het ophalen van de entiteit; de sleutel authoring LUIS wordt LUIS om modellen te maken namens jou gebruikt)

- Google Chrome-webbrowser. Installeer vanaf [ https://www.google.com/chrome/index.html ](https://www.google.com/chrome/index.html).

- GIT. Installeer vanaf [ https://git-scm.com/downloads ](https://git-scm.com/downloads).

- VSCode. Installeer vanaf [ https://code.visualstudio.com/ ](https://code.visualstudio.com/). Houd er rekening mee dat dit wordt aanbevolen, niet vereist.

## <a name="quick-start"></a>Snel starten 

1. Installeren en bouwen:

    ```bash    
    git clone https://github.com/Microsoft/ConversationLearner-Samples cl-bot-01
    cd cl-bot-01
    npm install
    npm run build
    ```

    > [!NOTE]
    > Tijdens de `npm install`, kunt u deze fout negeren als het zich voordoet: `gyp ERR! stack Error: Can't find Python executable`

2. Configureren:

   Maken van een bestand met de naam `.env` in de map `cl-bot-01`.  De inhoud van het bestand moet zijn:

   ```
   LUIS_AUTHORING_KEY=<your LUIS authoring key>
   ```

3. Bot starten:

    ```
    npm start
    ```

    Hiermee wordt de algemene leeg bot uitgevoerd in `cl-bot-01/src/app.ts`.

3. Voer conversatie cursist gebruikersinterface:

    ```bash
    [open second command prompt window]
    cd cl-bot-01
    npm run ui
    ```

4. Open browser http://localhost:5050 

Conversatie cursist nu gebruikt kunt en u maken en een conversatie cursist model leren gebruiken.  

> [!NOTE]
> Project conversatie cursist is starten, beschikbaar via inschrijving.  Als http://localhost:5050 ziet u een HTTP `403` fout, dit betekent dat uw account niet uitgenodigd.  Neem [een uitnodiging](https://aka.ms/conversation-learner-request-invite).

## <a name="tutorials-demos-and-switching-between-bots"></a>Zelfstudies, demo's en schakelen tussen bots

De bovenstaande instructies de algemene leeg bot gestart.  Uitvoeren van een zelfstudie of demo bot in plaats daarvan:

1. Als u het gesprek cursist web UI openen hebt, terug naar de lijst met apps op http://localhost:5050/home.
    
2. Als een andere bot wordt uitgevoerd (zoals `npm start` of `npm run demo-pizza`), stop de toepassing.  U hoeft niet te stoppen van de UI-proces of sluit de webbrowser.

3. Voer een demo-bot vanaf de opdrachtregel (stap 2 hierboven).  Demo's zijn onder andere:

  ```bash
  npm run tutorial-general
  npm run tutorial-entity-detection
  npm run tutorial-session-callbacks
  npm run tutorial-api-calls
  npm run demo-password
  npm run demo-pizza
  npm run demo-storage
  npm run demo-vrapp
  ```

4. Als u niet al, schakel over naar de conversatie cursist-webgebruikersinterface in Chrome door bij het laden van http://localhost:5050/home. 

5. Klik op 'Zelfstudies importeren' (hoeft slechts één keer worden uitgevoerd).  Dit duurt ongeveer een minuut en de conversatie cursist modellen voor de zelfstudies bij uw account conversatie cursist worden gekopieerd.

6. Klik op de demo-model in de gebruikersinterface van de conversatie cursist die overeenkomt met de demo die u hebt gestart.

Bronbestanden voor de demo's zijn in `cl-bot-01/src/demos`

## <a name="create-a-bot-which-includes-back-end-code"></a>Maken van een bot waaronder back-end-code

1. Als u het gesprek cursist web UI openen hebt, terug naar de lijst met apps op http://localhost:5050/home.
    
2. Als een bot wordt uitgevoerd (zoals `npm run demo-pizza`), stop de toepassing.  U hoeft niet te stoppen van de UI-proces of sluit de webbrowser.

3. Indien gewenst, code bewerken in `cl-bot-01/src/app.ts`.

4. Bouwen en bot opnieuw te starten:

    ```bash    
    npm run build
    npm start
    ```

5. Als u niet al, schakel over naar de conversatie cursist-webgebruikersinterface in Chrome door bij het laden van http://localhost:5050/home. 

6. Maak een nieuwe conversatie cursist-toepassing in de gebruikersinterface en onderwijs start.

7. Codewijzigingen in aanbrengen `cl-bot-01/src/app.ts`, herhaalt u de stappen vanaf stap 2 hierboven.

## <a name="vscode"></a>VSCode

In VSCode, er zijn uitgevoerd configuraties voor elke demo en voor de 'leeg bot' in `cl-bot-01/src/app.ts`.  Open de `cl-bot-01` map in VSCode.

## <a name="advanced-configuration"></a>Geavanceerde configuratie

Er is een sjabloon `.env.example` bestand ziet u welke omgeving variabelen die u instellen kunt voor het configureren van de voorbeelden.

U kunt deze poorten om te voorkomen van conflicten tussen andere services die op uw computer wordt uitgevoerd door toe te voegen een `.env` bestand naar de hoofdmap van project:

```bash
cp .env.example .env
```

Dit maakt gebruik van de standaardconfiguratie, waarmee u uw bot lokaal wordt uitgevoerd, en aan de slag met de conversatie cursist.  (Later op voor het implementeren van uw bot aan het Framework Bot, enkele bewerkingen van dit bestand vereist.)

## <a name="support"></a>Ondersteuning

- Geef vragen een tag op [Stack Overflow](https://stackoverflow.com) met 'microsoft cognitieve'
- Verzoek om een functie op onze [User Voice-pagina](https://aka.ms/conversation-learner-uservoice)
- Open een probleem op onze [github-repo-](https://github.com/Microsoft/ConversationLearner-Samples)

## <a name="contributing"></a>Bij te dragen

Op dit project is de [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/) (Microsoft Open Source-gedragscode) van toepassing. Zie voor meer informatie de [Code van Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) of neem contact op met [ opencode@microsoft.com ](mailto:opencode@microsoft.com) met aanvullende vragen of opmerkingen.

## <a name="source-repositories"></a>Bron-opslagplaatsen

- [conversationlearner-voorbeelden](https://github.com/Microsoft/ConversationLearner-Samples)
- [conversationlearner-sdk](https://github.com/Microsoft/ConversationLearner-SDK)
- [conversationlearner modellen](https://github.com/Microsoft/ConversationLearner-Models)
- [conversationlearner-ui](https://github.com/Microsoft/ConversationLearner-UI)
- [conversationlearner webchat](https://github.com/Microsoft/ConversationLearner-WebChat)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Hallo mensen](./tutorials/1-hello-world.md)
