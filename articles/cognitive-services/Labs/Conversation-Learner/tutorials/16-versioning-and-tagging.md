---
title: Het gebruik van versiebeheer en taggen met een model Conversatiecursist - Microsoft Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Informatie over het gebruik van versiebeheer en taggen met een Conversatiecursist-model.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: c7f23d989cbfa0ece9e404a0fe0feb68cf5fddb2
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/20/2018
ms.locfileid: "39170542"
---
# <a name="how-to-use-versioning-and-tagging"></a>Het gebruik van versiebeheer en labels

In deze zelfstudie ziet u hoe u versies van uw model Conversatiecursist label toe en stel welke versie is 'live'.  

## <a name="requirements"></a>Vereisten
Deze zelfstudie vereist log dialoogvensters, niet de Webgebruikersinterface voor logboek dialoogvenster maken met de bot-emulator.  

In deze zelfstudie is vereist dat de algemene zelfstudie bot wordt uitgevoerd:

    npm run tutorial-general

## <a name="details"></a>Details

Tijdens het bewerken van, u altijd de tag met de naam 'master' bewerkt--u kunt gecodeerde versies maken in de mastervertakking (die in feite een momentopname van de master), maar u kunt gecodeerde versies niet bewerken.

## <a name="steps"></a>Stappen

### <a name="install-the-bot-framework-emulator"></a>Installeert u de emulator van Bot framework

- Ga naar [https://github.com/Microsoft/BotFramework-Emulator](https://github.com/Microsoft/BotFramework-Emulator).
- Download en installeer de emulator.

### <a name="create-an-model"></a>Een model maken

1. Klik op Nieuw Model
2. Voer in het veld Naam zelfstudie-16-versiebeheer
3. Klik op Maken 
4. Klik op instellingen
5. Kopieer de Model-ID

### <a name="configure-the-emulator"></a>Configureren van de emulator

- Open het .env-bestand in de hoofdmap Conversatiecursist.
- Plak de Model-ID als de waarde van CONVERSATION_LEARNER_MODEL_ID
- De Conversatiecursist-service opnieuw starten door vanaf de opdrachtprompt wordt afgesloten en opnieuw uit te voeren:
 
    npm-zelfstudie-algemene uitvoeren 

### <a name="actions"></a>Acties

Laten we een actie maken:

1. Schakel over naar de Web-UI.
1. Klik op acties en vervolgens nieuwe actie.
2. Voer in het antwoord ' Hallo er (versie 1) '.
3. Klik op Opslaan.


![](../media/tutorial16_action1.PNG)

Maak een nieuwe tag:

3. Klik op 'instellingen' en maak een nieuwe 'tag'.
    - Noem deze "versie 1"
4. "Versie 1' naar 'live' worden ingesteld.  
    - Het effect van het instellen van de live 'versie 1'-tag is dat kanalen met behulp van deze bot wordt gebruikt in de "1"-versietag.
    - Met tags versies van modellen worden niet beïnvloed door bewerkingen (acties, entiteiten, toevoegen van de trein dialoogvensters wijzigen).  
    - Bewerkingen van een model (acties, entiteiten wijzigen, toevoegen van de trein dialoogvensters) worden altijd gemaakt op de 'master'-tag.  Met andere woorden, is 'master' de enige tag die kan worden gewijzigd. andere codes zijn momentopnamen vast.
    - Meld u dialoogvensters in de gebruikersinterface van de cursist conversatie altijd model gebruiken (niet de live code).

![](../media/tutorial16_v1_create.PNG)

De versie is gemaakt in de instellingen:

![](../media/tutorial16_settings.PNG)

Laten we een tweede actie toevoegen:

1. Klik op acties en vervolgens nieuwe actie.
2. Voer in antwoord, "doei doei (versie 2)".

De eerste actie bewerken:

1. Klik op acties.
2. Onder Acties, klikt u op ' Hallo er (versie 1) '.
3. Wijzigen van het antwoord op "Hallo er (versie 2) '.

![](../media/tutorial16_hi_there_v2.PNG)

### <a name="switch-to-the-bot-emulator"></a>Schakel over naar de bot-emulator

1. Voer in de gebruikersinterface bot "tot ziens".
2. De bot reageert met "Hallo er (versie 1) '.
    - Hiermee wordt aangegeven dat versie 1 is 'live'. 

![](../media/tutorial16_bf_response.PNG)

### <a name="switch-to-the-web-ui"></a>Schakel over naar de Web-UI

1. Klik op logboek-dialoogvensters (als er geen alle dialoogvensters, klikt u op de vernieuwknop).
2. Klik op ' Hallo er (versie 2) "

> [!NOTE]
> We kunnen correcties maken door te kiezen uit alle beschikbare acties. Deze wijzigingen worden doorgevoerd in de master.

U hebt nu gezien hoe versiebeheer werkt en hoe u kan communiceren met de bot met behulp van de Bot framework-emulator.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Demo - wachtwoord opnieuw instellen](./demo-password-reset.md)
