---
title: Een Conversation Learner-bot implementeren-Microsoft Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Meer informatie over het implementeren van een Conversation Learner bot.
services: cognitive-services
author: nitinme
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: 05fd83506aac26df33f18bec83dcadac8dee2d90
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68705274"
---
# <a name="how-to-deploy-a-conversation-learner-bot"></a>Een Conversation Learner-bot implementeren

In dit document wordt uitgelegd hoe u een Conversation Learner bot, lokaal of in azure implementeert.

## <a name="prerequisite-determine-the-model-id"></a>Vereisten: de model-ID bepalen 

Als u een bot buiten de Conversation Learner gebruikers interface wilt uitvoeren, moet u de Conversation Learner model-ID instellen die door de bot wordt gebruikt. Dit is de ID van het machine learning model in de Conversation Learner Cloud.  (Door daarentegen de bot via de Conversation Learner gebruikers interface uit te voeren, kiest de gebruikers interface welke model-ID.).  

U kunt als volgt de model-ID ophalen:

1. Start uw bot en de Conversation Learner-gebruikers interface.  Raadpleeg de Snelstartgids voor volledige instructies. samenvatten:

    In één opdracht Venster:

    ```
    [open a command window]
    cd my-bot
    npm start
    ```

    In het opdracht venster Anther

    ```bash
    [open second command prompt window]
    cd cl-bot-01
    npm run ui
    ```

2. Open browser om`http://localhost:5050` 

3. Klik op het Conversation Learner model waarvan u de ID wilt ophalen

4. Klik op instellingen in de navigatie balk aan de linkerkant.

5. De GUID ' model ID ' wordt weer gegeven in de buurt van de bovenkant van de pagina.

## <a name="option-1-deploying-a-conversation-learner-bot-to-run-locally"></a>Optie 1: Een Conversation Learner-bot implementeren om lokaal uit te voeren

Hiermee implementeert u een bot op uw lokale machine en ziet u hoe u deze kunt openen met behulp van de bot Framework emulator.

### <a name="configure-your-bot-for-access-outside-the-conversation-learner-ui"></a>Uw bot configureren voor toegang buiten de Conversation Learner gebruikers interface

Wanneer u een bot lokaal uitvoert, voegt u de toepassings-id toe `.env` aan het bestand van de bot:

    ```
    CONVERSATION_LEARNER_MODEL_ID=<YOUR_MODEL_ID>
    ```

Start vervolgens uw bot:

    ```
    [open a command window]
    cd my-bot
    npm start
    ```

De bot is nu lokaal actief.  U kunt deze openen met de bot Framework emulator.

### <a name="download-and-install-the-emulator"></a>De emulator downloaden en installeren

    ```
    git clone https://github.com/Microsoft/BotFramework-Emulator
    npm install
    npm run build
    npm start
    ```

### <a name="connect-the-emulator-to-your-bot"></a>De emulator verbinden met uw bot

1. Voer `http://127.0.0.1:3978/api/messages`in de linkerbovenhoek van de emulator in het vak ' Voer uw eind punt-URL in ' in.  Laat de overige velden leeg en klik op verbinden.

2. U bent nu bezig met het bespreken van uw bot.

## <a name="option-2-deploy-to-azure"></a>Optie 2: Implementeren in Azure

Publiceer uw Conversation Learner-bot op dezelfde manier als u een andere bot publiceert. Op hoog niveau uploadt u uw code naar een gehoste website, stelt u de juiste configuratie waarden in en registreert u de bot met verschillende kanalen. Gedetailleerde instructies vindt u in deze video waarin wordt getoond hoe u uw bot publiceert met Azure Bot Service.

Zodra de bot is geïmplementeerd en wordt uitgevoerd, kunt u verschillende kanalen koppelen, zoals Facebook, teams, Skype, enzovoort. een Azure bot-kanaal registratie gebruiken. Zie voor documentatie over dat proces: https://docs.microsoft.com/bot-framework/bot-service-quickstart-registration

Hieronder vindt u stapsgewijze instructies voor het implementeren van een Conversation Learner bot naar Azure.  In deze instructies wordt ervan uitgegaan dat uw bot-bron beschikbaar is vanuit een Cloud bron, zoals Azure DevOps Services, GitHub, BitBucket of OneDrive, en hoe u uw bot configureert voor continue implementatie.

1. Meld u aan bij de Azure Portal op https://portal.azure.com

2. Een nieuwe resource voor web-app-bot maken 

    1. De bot een naam geven
    2. Klik op ' bot-sjabloon ', kies ' node. js ', kies ' Basic ' en klik vervolgens op de knop ' selecteren '
    3. Klik op maken om de web-app-bot te maken.
    4. Wacht tot de web-app-bot-resource is gemaakt.

3. Bewerk in de Azure Portal de web-app-bot die u zojuist hebt gemaakt.

   1. Klik op het NAV-item ' toepassings instellingen ' aan de linkerkant
   1. Schuif omlaag naar de sectie app-instellingen
   2. Deze instellingen toevoegen:

       Omgevingsvariabele | value
       --- | --- 
       CONVERSATION_LEARNER_SERVICE_URI | "https://westus.api.cognitive.microsoft.com/conversationlearner/v1.0/"
       CONVERSATION_LEARNER_MODEL_ID      | De GUID van de toepassings-id die is verkregen via de Conversation Learner gebruikers interface onder de instellingen voor het model >
       LUIS_AUTHORING_KEY               | LUIS-ontwerp sleutel voor dit model
       LUIS_SUBSCRIPTION_KEY            | Niet vereist, maar wordt aanbevolen voor gepubliceerde bots om te voor komen dat uw ontwerp quotum wordt gebruikt.
    
   4. Klik op opslaan in de buurt van de bovenkant van de pagina
   5. Het NAV-item ' Build ' aan de linkerkant openen
   6. Klik op "continue implementatie configureren" 
   7. Klik op het pictogram installatie onder implementaties
   8. Klik op vereiste instellingen
   9. Selecteer de bron waar uw bot-code beschikbaar is en configureer de bron.
