---
title: Het implementeren van een conversatie cursist bot - cognitieve Microsoft-Services | Microsoft Docs
titleSuffix: Azure
description: Informatie over het implementeren van een conversatie cursist bot.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 77cc998227d996a6e52b1b5629204da5dc735ede
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345267"
---
# <a name="how-to-deploy-a-conversation-learner-bot"></a>Het implementeren van een conversatie cursist bot

Dit document wordt uitgelegd hoe een conversatie cursist-bot implementatie van lokaal of in Azure.

## <a name="prerequisite-determine-the-application-id"></a>Voorwaarde: bepalen de toepassings-ID 

Als u wilt een bot buiten de conversatie cursist gebruikersinterface uitvoeren, moet u de conversatie cursist toepassings-ID die door de bot wordt gebruikt, dat wil zeggen, de ID van de machine learning-model in de cloud conversatie cursist instellen.  (Daarentegen wanneer de bot via de gebruikersinterface van de cursist conversatie wordt uitgevoerd, de gebruikersinterface kiest welke id).  

Dit is het verkrijgen van de toepassings-ID:

1. Start uw bot en de gebruikersinterface van de cursist conversatie.  Raadpleeg de Quick Start guide voor volledige instructies; samengevat:

    In een opdrachtvenster:

    ```
    [open a command window]
    cd my-bot
    npm start
    ```

    In het opdrachtvenster andere

    ```bash
    [open second command prompt window]
    cd cl-bot-01
    npm run ui
    ```

2. Open browser http://localhost:5050 

3. Klik op de conversatie cursist-toepassing die u wilt ophalen van de ID voor

4. Klik op 'Instellingen' in de navigatiebalk aan de linkerkant.

5. De GUID van de App-ID' wordt weergegeven boven aan de pagina.

## <a name="option-1-deploying-a-conversation-learner-bot-to-run-locally"></a>Optie 1: Implementatie van een conversatie cursist bot lokaal uitvoeren

Dit implementeert een bot op uw lokale computer en ziet u hoe u toegang hebt tot het gebruik van de emulator Bot Framework.

### <a name="configure-your-bot-for-access-outside-the-conversation-learner-ui"></a>Uw bot voor toegang tot buiten de conversatie cursist gebruikersinterface configureren

Wanneer een bot lokaal wordt uitgevoerd, het toevoegen van de toepassings-ID aan de bot `.env` bestand:

    ```
    CONVERSATION_LEARNER_APP_ID=<YOUR_APP_ID>
    ```

Start uw bot:

    ```
    [open a command window]
    cd my-bot
    npm start
    ```

De bot wordt nu lokaal uitgevoerd.  U kunt openen met de emulator Bot-Framework.

### <a name="download-and-install-the-emulator"></a>Download en installeer de emulator

    ```
    git clone https://github.com/Microsoft/BotFramework-Emulator
    npm install
    npm run build
    npm start
    ```

### <a name="connect-the-emulator-to-your-bot"></a>Verbinding maken met de emulator uw bot

1. In de linkerbovenhoek van de emulator in het vak 'Geef de eindpunt-URL', voer `http://127.0.0.1:3978/api/messages`.  De andere velden leeg laat en klik op 'Verbinding maken'.

2. U bent nu conversatie met uw bot.

## <a name="option-2-deploy-to-azure"></a>Optie 2: Implementeren in Azure

Publiceer de conversatie cursist bot lijkt op dezelfde manier als die u andere bot wilt publiceren. Op een hoog niveau uploaden van uw code naar een gehoste website, stel de juiste configuratiewaarden en registreer de bot met verschillende kanalen. Gedetailleerde instructies vindt u in deze video waarin wordt getoond hoe uw bot using Azure Bot Service publiceren.

Zodra de bot is geïmplementeerd en uitgevoerd u verschillende kanalen verbinding mee kan maken zoals Facebook, Teams, Skype enzovoort met behulp van een Azure-Bot kanaal te registreren. Voor documentatie over die Zie: https://docs.microsoft.com/en-us/bot-framework/bot-service-quickstart-registration

Hieronder vindt u stapsgewijze instructies voor het implementeren van een conversatie cursist Bot naar Azure.  Deze instructies wordt ervan uitgegaan dat de bron-bot beschikbaar van een cloud-gebaseerde bron zoals VSTS, GitHub, BitBucket of OneDrive is, en uw bot voor continue implementatie configureert.

1. Meld u aan bij de Azure portal op https://portal.azure.com

2. Maak een nieuwe resource met 'Web-App Bot' 

    1. Geef een naam op voor de bot
    2. Klik op 'Bot sjabloon', 'Node.js' kiest, kies 'Basic' en klik op de knop 'Select'
    3. Klik op 'maken' voor het maken van de Web-App-Bot.
    4. Wacht tot de Web-App Bot resource moet worden gemaakt.

3. Bewerk de Web-App Bot resource die u zojuist hebt gemaakt in de Azure-portal.

    1. Klik op 'Toepassingsinstellingen' nav item aan de linkerkant
    1. Ga naar de sectie 'App-instellingen'
    2. Deze instellingen toevoegen:

        Omgevingsvariabele | waarde
        --- | --- 
        CONVERSATION_LEARNER_SERVICE_URI | "https://westus.api.cognitive.microsoft.com/conversationlearner/v1.0/"
        CONVERSATION_LEARNER_APP_ID      | Aanvraag-Id-GUID, verkregen via de gebruikersinterface van de conversatie cursist onder 'instellingen' voor de app >
        LUIS_AUTHORING_KEY               | LUIS sleutel voor deze app ontwerpen
    
    4. Klik op 'Opslaan' boven aan de pagina
    5. Nav-item 'Build' aan de linkerkant openen
    6. Klik op "Continue implementatie configureren" 
    7. Klik op het pictogram 'Instellingen' onder implementaties
    8. Klik op 'Instellingen vereist'
    9. Selecteer de bron waarin uw code bot beschikbaar is en configureer de bron.
