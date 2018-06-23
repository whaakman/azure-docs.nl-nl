---
title: Het gebruik van versiebeheer en labels met een toepassing conversatie cursist - cognitieve Microsoft-Services | Microsoft Docs
titleSuffix: Azure
description: Informatie over het gebruik versioning en labels met een conversatie cursist-toepassing.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: ea013db078ff33f8597b0e15a8fc951e8ae320e8
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345294"
---
# <a name="how-to-use-versioning-and-tagging"></a>Het gebruik van versiebeheer en labels

Deze zelfstudie laat zien hoe versies van uw app conversatie cursist labelen en instellen welke versie 'live' is.  

## <a name="requirements"></a>Vereisten
Deze zelfstudie, moet u de emulator bot logboek dialoogvensters, niet de Webgebruikersinterface voor logboek dialoogvenster maken.  

Deze zelfstudie is vereist dat de algemene zelfstudie bot wordt uitgevoerd:

    npm run tutorial-general

## <a name="details"></a>Details

Wanneer u bewerkt, bewerkt u altijd de tag genaamd 'master'--kunt u met tags versies van model (die in wezen een momentopname van master), maar met tags versies kan niet worden bewerkt.

## <a name="steps"></a>Stappen

### <a name="install-the-bot-framework-emulator"></a>Installeren van de emulator Bot-framework

- Ga naar [https://github.com/Microsoft/BotFramework-Emulator](https://github.com/Microsoft/BotFramework-Emulator).
- Download en installeer de emulator.

### <a name="create-an-app"></a>Een app maken

1. Klik op nieuwe App
2. Voer in het veld Naam zelfstudie-16-versies
3. Klik op Maken 
4. Klik op instellingen
5. Kopiëren van de App-ID

### <a name="configure-the-emulator"></a>Configureren van de emulator

- Open het bestand .env in de hoofdmap van de conversatie cursist.
- De App-ID als de waarde van CONVERSATION_LEARNER_APP_ID plakken
- De conversatie cursist-service opnieuw starten door vanaf de opdrachtprompt afgesloten en opnieuw uit te voeren:
 
    npm zelfstudie algemene uitvoeren 

### <a name="actions"></a>Acties

We gaan een actie maken:

1. Overschakelen naar de Web-UI.
1. Klik op acties en vervolgens nieuwe actie.
2. Voer in het antwoord ' Hallo daar (versie 1) '.
3. Klik op opslaan.


![](../media/tutorial16_action1.PNG)

Een nieuw label maken:

3. Klik op 'instellingen' en maak een nieuwe "tag'.
    - Noem het 'versie 1'
4. 'Versie 1' naar 'live' worden ingesteld.  
    - Het effect van het instellen van de live 'versie 1'-tag is dat deze bot met kanalen gebruik van de ' versie 1" tag.
    - Met tags versies van toepassingen worden niet beïnvloed door wijzigingen (acties, entiteiten, toe te voegen train dialoogvensters gewijzigd).  
    - Bewerkingen van een toepassing (acties, entiteiten wijzigen, toe te voegen train dialoogvensters) altijd gemaakt op de 'master'-tag.  Met andere woorden, is 'master' de enige tag die kan worden gewijzigd. andere labels worden momentopnamen worden opgelost.
    - Meld u dialoogvensters in de gebruikersinterface van de conversatie cursist altijd model gebruiken (niet de live code).

![](../media/tutorial16_v1_create.PNG)

Houd rekening met dat de versie is gemaakt in de instellingen:

![](../media/tutorial16_settings.PNG)

We voegen een tweede actie:

1. Klik op acties en vervolgens nieuwe actie.
2. Voer in het antwoord, 'bye bye (versie 2)'.

De eerste actie bewerken:

1. Klik op acties.
2. Klik onder acties op ' Hallo daar (versie 1) '.
3. Wijzigen van het antwoord ' Hallo daar (versie 2) '.

![](../media/tutorial16_hi_there_v2.PNG)

### <a name="switch-to-the-bot-emulator"></a>Overschakelen naar de emulator bot

1. Voer in de gebruikersinterface bot "goodbye".
2. Opmerking de bot met reageert "Hallo daar (versie 1) '.
    - Dit betekent dat de versie 1 is 'live'. 

![](../media/tutorial16_bf_response.PNG)

### <a name="switch-to-the-web-ui"></a>Overschakelen naar de webgebruikersinterface

1. Klik op logboek dialoogvensters (als er geen alle dialoogvensters, vernieuwt de app).
2. Klik op ' Hallo daar (versie 2) "

Houd er rekening mee dat we kiezen uit alle beschikbare acties correcties kunt maken. Deze wijzigingen worden aangebracht aan het model.

U hebt nu gezien hoe versiebeheer werkt en hoe u kunt werken met de bot met behulp van de emulator Bot-framework.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Demo - wachtwoord opnieuw instellen](./demo-password-reset.md)
