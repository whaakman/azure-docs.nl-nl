---
title: Over het implementeren van een bot Conversatiecursist - Microsoft Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Informatie over het implementeren van een bot Conversatiecursist.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 44d4d3b7b82a2f23f973a9c94cb66c81c430efa8
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55225309"
---
# <a name="how-to-deploy-a-conversation-learner-bot"></a>Over het implementeren van een bot Conversatiecursist

Dit document wordt uitgelegd hoe u een bot Conversatiecursist--hetzij lokaal of in Azure implementeren.

## <a name="prerequisite-determine-the-model-id"></a>Voorwaarde: bepalen de model-ID 

Als u wilt een bot buiten de gebruikersinterface van de cursist conversatie uitvoeren, moet u de Conversatiecursist model-ID die de bot gebruiken wilt, dat wil zeggen, de ID van de machine learning-model in de cloud Conversatiecursist instellen.  (Daarentegen bij het uitvoeren van de bot via de gebruikersinterface van de cursist conversatie, de gebruikersinterface kiest welk model-id).  

Dit is het verkrijgen van de model-ID:

1. Start uw bot en de gebruikersinterface van de cursist conversatie.  Zie de handleiding voor volledige instructies; Samenvatting:

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

2. Open de browser naar http://localhost:5050 

3. Klik op het Conversatiecursist model dat u wilt ophalen van de ID voor

4. Klik op 'Instellingen' in de navigatiebalk aan de linkerkant.

5. De GUID "Model-ID" wordt aan de bovenkant van de pagina weergegeven.

## <a name="option-1-deploying-a-conversation-learner-bot-to-run-locally"></a>Optie 1: Implementatie van een bot Conversatiecursist lokaal uitvoeren

Dit implementeert een bot op uw lokale computer en laat zien hoe u het kunt openen met behulp van de emulator van de Bot Framework.

### <a name="configure-your-bot-for-access-outside-the-conversation-learner-ui"></a>Configureren van uw bot voor toegang tot buiten de gebruikersinterface van de cursist gesprek

Wanneer een bot lokaal uitvoert, het toevoegen van de toepassings-ID aan van de bot `.env` bestand:

    ```
    CONVERSATION_LEARNER_MODEL_ID=<YOUR_MODEL_ID>
    ```

Start uw bot:

    ```
    [open a command window]
    cd my-bot
    npm start
    ```

De bot wordt nu lokaal uitgevoerd.  U kunt openen met de emulator Bot Framework.

### <a name="download-and-install-the-emulator"></a>Download en installeer de emulator

    ```
    git clone https://github.com/Microsoft/BotFramework-Emulator
    npm install
    npm run build
    npm start
    ```

### <a name="connect-the-emulator-to-your-bot"></a>Verbinding maken met de emulator van uw bot

1. In de linkerbovenhoek van de emulator in het vak 'Voer uw eindpunt-URL', voer `http://127.0.0.1:3978/api/messages`.  De andere velden leeg laat, en klik op 'Verbinding maken'.

2. U bent nu discussiëren met uw bot.

## <a name="option-2-deploy-to-azure"></a>Optie 2: Implementeren in Azure

Publiceer uw bot Conversatiecursist die vergelijkbaar is met dezelfde manier als die u een andere bot wilt publiceren. Op hoog niveau, uw code te uploaden naar een gehoste website, de juiste configuratie-waarden instellen en vervolgens de bot registreren met verschillende kanalen. Gedetailleerde instructies vindt u in deze video waarin wordt getoond hoe uw bot met behulp van Azure Bot Service publiceren.

Zodra de bot wordt geïmplementeerd en uitgevoerd u verschillende kanalen verbinding mee kan maken, zoals Facebook, Teams, Skype enzovoort met behulp van een Azure Bot kanaal te registreren. Voor documentatie over dit proces Zie: https://docs.microsoft.com/bot-framework/bot-service-quickstart-registration

Hieronder vindt u stapsgewijze instructies voor het implementeren van een cursist conversatie Bot naar Azure.  Deze instructies wordt ervan uitgegaan dat uw bot-bron is beschikbaar via een cloud-gebaseerde bron, zoals Azure DevOps-Services, GitHub, BitBucket of OneDrive, configureert u uw bot voor continue implementatie.

1. Meld u aan bij Azure portal op https://portal.azure.com

2. Maak een nieuwe 'Web App-Bot'-resource 

    1. Geef een naam op voor de bot
    2. Op 'Bot sjabloon', 'Node.js' kiest, kies 'Basic', en vervolgens klikt u op de knop 'Selecteren'
    3. Klik op 'maken' voor het maken van de Web-App-Bot.
    4. Wacht tot de Web App-Bot-bron moet worden gemaakt.

3. Bewerk de Web App-Bot-resource die u zojuist hebt gemaakt in de Azure-portal.

    1. Klik op 'Instellingen' nav item aan de linkerkant
    1. Schuif omlaag naar het gedeelte 'App-instellingen'
    2. Voeg deze instellingen toe:

        Omgevingsvariabele | waarde
        --- | --- 
        CONVERSATION_LEARNER_SERVICE_URI | "https://westus.api.cognitive.microsoft.com/conversationlearner/v1.0/"
        CONVERSATION_LEARNER_MODEL_ID      | Aanvraag-Id-GUID, verkregen via de gebruikersinterface van de cursist conversatie onder 'instellingen' voor het model >
        LUIS_AUTHORING_KEY               | LUIS-sleutel voor dit model ontwerpen
    
    4. Klik op 'Opslaan' aan de bovenkant van de pagina
    5. Open 'Build' nav-item aan de linkerkant
    6. Klik op "Continue implementatie configureren" 
    7. Klik op het pictogram 'Instellen' onder implementaties
    8. Klik op 'Instellingen vereist'
    9. Selecteer de bron waar de code van uw bot beschikbaar is en configureer de bron.
