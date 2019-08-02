---
title: Versie-tagging gebruiken met een Conversation Learner model-Azure Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Meer informatie over het gebruik van versie beheer en labelen met een Conversation Learner model.
services: cognitive-services
author: nitinme
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: 0279363c039e3ec3c2deac3bc7f71c32c547e9d1
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68703383"
---
# <a name="how-to-use-version-tagging"></a>Versie-tagging gebruiken

In deze zelf studie ziet u hoe u versies van uw Conversation Learner model codeert en hoe u de versie ' live ' kunt instellen.  

## <a name="requirements"></a>Vereisten
Deze zelf studie vereist het gebruik van de bot Framework emulator om dialoog vensters voor logboeken te maken, niet in de webgebruikersinterface van het logboek venster.  

Voor deze zelf studie moet de bot van de algemene zelf studie worden uitgevoerd:

    npm run tutorial-general

## <a name="details"></a>Details

Gelabelde versies van het model zijn statisch; u kunt ze niet bewerken of wijzigen. Wanneer u uw model bewerkt, bewerkt u altijd de hoofd versie. Wanneer u een nieuwe tag toevoegt, wordt in Conversation Learner een moment opname van het model op dat moment vastgelegd. 

Uw bot gebruikt de versie van het model dat u hebt geselecteerd als de ' live ' versie, maar alle conversaties zijn alleen zichtbaar wanneer de ' bewerkings code ' is ingesteld op ' Master '. Als de eigenschap ' code bewerken ' van het model is ingesteld op een andere waarde dan ' Master ', kunt u de moment opname van het model weer geven, maar niet op een wille keurige manier wijzigen.

## <a name="steps"></a>Stappen

### <a name="install-the-bot-framework-emulator"></a>De bot Framework-emulator installeren

1. Ga naar [https://github.com/Microsoft/BotFramework-Emulator](https://github.com/Microsoft/BotFramework-Emulator).
2. Down load en installeer de emulator.

### <a name="create-a-model"></a>Een model maken

1. Klik op de start pagina van de model lijst `New Model` op de knop.
2. In het `Name` veld type, ' zelf studie-18 versie beheer ', druk op ENTER.
4. Klik in het linkerdeel venster op instellingen.
5. Kopieer de inhoud van het veld CONVERSATION_LEARNER_MODEL_ID naar het klem bord.

### <a name="configure-the-emulator"></a>De emulator configureren

1. Open het bestand '. env ' in de hoofdmap van Conversation Learner.
2. Voeg als volgt een regel toe aan het bestand '. env ':
    - `CONVERSATION_LEARNER_MODEL_ID=[paste-model-id-from-clipboard-here]`
3. Start de Conversation Learner-service opnieuw door af te sluiten vanaf de opdracht prompt en opnieuw uit te voeren:
    - `npm run tutorial-general`
4. Maak in de bot Framework-emulator een nieuwe bot-configuratie, stel de eind punt-URL in op`http://localhost:3978/api/messages`

### <a name="version-1"></a>Versie 1

Er wordt één actie gemaakt voor versie 1.

1. Klik in het linkerdeel venster van de webgebruikersinterface op acties en klik vervolgens op de `New Action` knop.
2. Voer in het veld ' bot ' op ' Hallo (versie 1) ' in.
3. Klik op de knop `Save`.

Nu gaan we dit labelen als ' versie 1 ' van het model.

1. Klik in het linkerdeel venster op instellingen en klik vervolgens op het ![](../media/tutorial18_version_tags.PNG)pictogram versie Tags om de `New Tag` knop waarop u moet klikken, weer te geven.
    - Noem het ' versie 1 '
1. Selecteer ' versie 1 ' in de vervolg keuzelijst ' Live tag '.  
    - Nu kanalen die gebruikmaken van deze bot, zullen ' versie 1 ' van ons model gebruiken.
    - De dialoog vensters entiteiten, acties en trainen van dit model van versie 1 kunnen niet meer worden gewijzigd.
    - Als u ' versie 1 ' als de ' bewerkings code ' selecteert, kunt u het model alleen bekijken en niet bewerken.
    - De ' code bewerken ' is ingesteld op ' Master '. Dit is de enige versie van het model die kan worden bewerkt.

Nu ziet u ' versie 1 ' in het raster ' versie Tags '.

### <a name="version-2"></a>Versie 2

Nu gaan we ons model bewerken om het te onderscheiden van versie 1.

1. Klik in het linkerdeel venster op acties.
2. Klik in het acties raster op ' Hallo (versie 1) '.
3. Wijzig het veld ' bot ' op ' hily (versie 2) '.
4. Klik op de knop `Save`.
5. Klik op de knop `New Action`.
6. In het veld type van de bot, "verrijkt (versie 2)".

### <a name="confirm-bot-framework-emulator-is-using-version-1"></a>Bevestigen dat de bot Framework emulator gebruikmaakt van versie 1

1. In de bot Framework emulator typt u het bericht ' er is een '.
2. U ziet dat de bot reageert met ' hily (versie 1) '.
    - Hiermee wordt gecontroleerd of versie 1 ' live ' is.

### <a name="view-the-conversation-logs-in-conversation-learner-web-ui"></a>De gespreks logboeken weer geven in Conversation Learner Web-UI

1. Klik in het linkerdeel venster op dialoog vensters logboeken
    - Als er geen dialoog vensters worden weer geven, klikt u op de knop Vernieuwen.
2. Let op de tag "versie 1" in het raster.
3. Klik in het raster op ' hily (versie 1) '.

> [!NOTE]
> We kunnen correcties aanbrengen door te kiezen uit alle momenteel beschik bare Conversation Learner functionaliteit, maar deze bewerkingen worden wel toegepast op de Master en niet op versie 1.

U hebt nu gezien hoe versie beheer werkt en hoe u kunt communiceren met de bot met behulp van de bot Framework emulator.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Entiteiten opsommen en entiteits acties instellen](./tutorial-enum-set-entity.md)
