---
title: Het gebruik van versie taggen met een Model van de cursist conversatie - Microsoft Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Informatie over het gebruik van versiebeheer en taggen met een Conversatiecursist-model.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 5073d3ab967c4c4e1b90636c247839875a6aa0d7
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58086306"
---
# <a name="how-to-use-version-tagging"></a>Het gebruik van versie Tagging

In deze zelfstudie ziet u hoe u versies van het Model van de cursist conversatie label toe en stel welke versie is 'live'.  

## <a name="requirements"></a>Vereisten
Deze zelfstudie vereist de Bot Framework-Emulator gebruiken om te maken met het Log-dialoogvensters, niet de Webgebruikersinterface voor logboek dialoogvenster.  

In deze zelfstudie is vereist dat de algemene zelfstudie Bot wordt uitgevoerd:

    npm run tutorial-general

## <a name="details"></a>Details

Met tags versies van het Model zijn statisch. u kunt geen bewerken of deze te wijzigen. Tijdens het bewerken van uw model, bewerkt u altijd de Master versie. Wanneer u een nieuwe Tag toevoegt, wordt een momentopname van het Model in Conversatiecursist vastgelegd op dat punt in tijd. 

Uw Bot wordt de versie van het Model dat u hebt geselecteerd als de 'Live'-versie gebruiken, maar deze heeft afgemeld kunnen worden weergegeven wanneer de 'bewerken van code' is ingesteld op 'Master'. Als de eigenschap 'Tag bewerken' van het Model is ingesteld op iets anders dan 'Master', kunt u de momentopname van het Model bekijken, maar u kunt deze niet wijzigen op geen enkele manier.

## <a name="steps"></a>Stappen

### <a name="install-the-bot-framework-emulator"></a>Installeert u de Emulator van Bot Framework

1. Ga naar [https://github.com/Microsoft/BotFramework-Emulator](https://github.com/Microsoft/BotFramework-Emulator).
2. Download en installeer de emulator.

### <a name="create-a-model"></a>Een Model maken

1. Uit het Model lijst-startpagina, klikt u op de `New Model` knop.
2. In de `Name` veldtype, "-zelfstudie-18-versiebeheer," Druk op enter.
4. Klik op 'Instellingen' in het linkerdeelvenster.
5. Kopieer de inhoud van het veld CONVERSATION_LEARNER_MODEL_ID naar het Klembord.

### <a name="configure-the-emulator"></a>Configureren van de Emulator

1. Open het bestand '.env' in de hoofdmap Conversatiecursist.
2. Een regel toegevoegd aan het bestand '.env' als volgt:
    - `CONVERSATION_LEARNER_MODEL_ID=[paste-model-id-from-clipboard-here]`
3. De Conversatiecursist-service opnieuw starten door vanaf de opdrachtprompt wordt afgesloten en opnieuw uit te voeren:
    - `npm run tutorial-general`
4. In Bot Framework-Emulator, maak een nieuwe bot-configuratie, stelt u de eindpunt-URL op `http://localhost:3978/api/messages`

### <a name="version-1"></a>Versie 1

We maken één actie voor versie 1.

1. In het linkerdeelvenster van de Web-UI, klikt u op 'Acties' en klik vervolgens op de `New Action` knop.
2. In de 'van de Bot-Response' Voer ' Hallo er (versie 1) '.
3. Klik op de knop `Save`.

Nu u we dit markeren als 'versie 1"van het Model.

1. In het linkerdeelvenster klikt u op 'instellingen' en klik op de ![](../media/tutorial18_version_tags.PNG)"Versie Tags"-pictogram om weer te geven de `New Tag` knop waarop u moet klikken.
    - Geef het de naam "Versie 1"
1. In de 'Live code' vervolgkeuzelijst selecteren ' versie 1".  
    - Kanalen met behulp van deze Bot wordt nu 'versie 1"van het Model gebruiken.
    - De entiteiten, acties en Train-dialoogvensters van deze versie 1-Model kunnen niet meer worden gewijzigd.
    - Als u 'Versie 1' als 'Bewerken van code' selecteert kunt u zich alleen het Model weergeven en deze niet bewerken.
    - Laat de eigenschap 'Tag bewerken' ingesteld op 'Master', is de enige versie van het model dat kan worden bewerkt.

U ziet nu "Versie 1" in het raster 'Versie Tags'.

### <a name="version-2"></a>Versie 2

Nu bewerken we het Model om u te onderscheiden van versie 1.

1. In het linker deelvenster Klik op 'Acties'.
2. Klik in het raster acties op "Hallo er (versie 1) '.
3. De 'van de Bot-response' veld wijzigen ' Hallo er (versie 2) '.
4. Klik op de knop `Save`.
5. Klik op de knop `New Action`.
6. In de 'van de Bot response' veldtype, "doei doei (versie 2)".

### <a name="confirm-bot-framework-emulator-is-using-version-1"></a>Controleer of Bot Framework Emulator met behulp van versie 1

1. Typ in het bericht "Hallo, er" in de Emulator Bot Framework.
2. U ziet dat de Bot met reageert "Hallo er (versie 1) '.
    - Hiermee wordt gecontroleerd dat versie 1 'live' is.

### <a name="view-the-conversation-logs-in-conversation-learner-web-ui"></a>Bekijk de conversatie-Logboeken in gesprek Learner-Webgebruikersinterface

1. In het linkerdeelvenster klikt u op het "Log-dialoogvensters"
    - Als u dialoogvensters niet ziet, klikt u op de vernieuwknop.
2. U ziet de tag "Versie 1" in het raster.
3. Klik in het raster op "Hallo er (versie 1)"

> [!NOTE]
> We kunnen correcties maken door te kiezen uit alle beschikbare Conversatiecursist-functionaliteit, echter deze wijzigingen worden gemaakt met de Master en niet met versie 1.

U hebt nu gezien hoe versiebeheer werkt en hoe u kan communiceren met de Bot met behulp van de Bot Framework-Emulator.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Demo - wachtwoord opnieuw instellen](./demo-password-reset.md)
