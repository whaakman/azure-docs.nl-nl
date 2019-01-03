---
title: Over het maken van een model Conversatiecursist "Hallo wereld" - Microsoft Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Informatie over het maken van een model Conversatiecursist "Hallo wereld".
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: c08e3d2e8f712f5eb7c56585507a283c7fd040c1
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/27/2018
ms.locfileid: "53796756"
---
# <a name="how-to-create-a-hello-world-model-with-conversation-learner"></a>Over het maken van een 'Hallo wereld'-model met Conversatiecursist

Deze zelfstudie laat zien hoe u aan de slag met Conversatiecursist, met inbegrip van het maken van acties, geven de Bot interactief en correcties van geregistreerde dialoogvensters die afkomstig van eindgebruikers zijn te maken.

## <a name="video"></a>Video

[![Zelfstudie Hallo wereld-voorbeeld](https://aka.ms/cl_Tutorial_v3_HelloWorld_Preview)](https://aka.ms/cl_tutorial_v3_helloworld)


## <a name="requirements"></a>Vereisten
Als u niet hebt gedaan, eerst controleert u of alle stappen van de setup zijn voltooid, met inbegrip van het maken van een `.env` bestand met uw LUIS sleutel ontwerpen.  Zie [snelstartgids](../quickstart.md) voor meer informatie.

In deze zelfstudie is vereist dat de algemene zelfstudie Bot wordt uitgevoerd

    npm run tutorial-general

## <a name="steps"></a>Stappen

Start op de startpagina in de Web-UI.

### <a name="create-the-model"></a>Het Model maken
1. Klik op de knop "Nieuw Model".
2. Voer in het veld "Naam", "Hallo wereld".
3. Klik op de knop 'Maken'.

U ziet nu de weergave van het model dat u hebt gemaakt.

### <a name="create-an-action"></a>Een actie maken
1. Klik op 'Acties' en vervolgens de knop 'Nieuwe actie' in het linkerdeelvenster.
    - Een actie is een SMS-bericht die Conversatiecursist geretourneerd naar de gebruiker, een API-aanroep of een kaart.
2. In de 'van de Bot Response...' veldtype "Hallo".
    - Dit is het antwoord dat de Bot wordt geretourneerd.
3. Klik op de knop 'Maken'.

U hebt gemaakt dat de eerste actie die de Bot uitvoeren kunt, dat wil zeggen een tekstantwoord retourneren.

### <a name="train-dialogs"></a>Dialoogvensters trainen
Dit is waar u het Model voor het reageren op gebruiker-uitingen te trainen.

#### <a name="first-training-dialog"></a>Eerste Training-dialoogvenster

1. In het linkerdeelvenster klikt u op "Train-dialoogvensters" en vervolgens de knop 'Nieuwe Train dialoogvenster'.
2. Type 'Hi', druk op enter.
    - Als u een voorbeeld van wat de gebruiker mogelijk staat in het begin van een gesprek.
3. Klik op de knop 'Acties Score'.
4. Selecteer "Hallo".
    - U zojuist een volledige inschakelen in dit dialoogvenster voorbeeld. 
5. Typ in het antwoord van de gebruiker, "Tot ziens".
6. Klik op de knop 'Acties Score'.
7. Klik op het "+ actie" knop.
8. Type "tot ziens!" veld en klik op de knop 'Maken' in '... van Bot-response'.
    - U ziet de Bot reageerde met dat actie u zojuist hebt gemaakt.
9. Klik op de knop 'Opslaan'. 
    - Dit wordt beëindigd en sla dit dialoogvenster Training.

U hebt nu een dialoogvenster voor Training in het Model, samen met een enkele entiteit en twee acties.

#### <a name="second-training-dialog"></a>Tweede Training-dialoogvenster
Laten we doen een meer training en Zie hoe de Bot reageert.

1. Klik op de knop 'Nieuwe Train dialoogvenster'.
2. Typ in 'Hallo'
    - Dit is vergelijkbaar met het eerste dialoogvenster en we verwachten dat een goede score ophalen van de Bot.
3. Klik op de knop 'Acties Score'.
    - De positie en de score nog steeds mogelijk niet nauwkeurig genoeg en mogelijk extra training.
4. Selecteer "Hallo".
5. Typ in het antwoord van de gebruiker, "tot ziens".
6. Klik op de knop 'Acties Score'.
7. Selecteer "Tot ziens!"
8. Klik op de knop 'Opslaan'.

### <a name="log-dialogs"></a>Logboek-dialoogvensters
Dit is waar u Test, weergeven en corrigeren gesprekken die u of de echte gebruikers met uw Bot hebben gehad.

#### <a name="test-the-model-as-an-end-user"></a>Test het Model als een eindgebruiker
1. Klik op "Log-dialoogvensters" en vervolgens de knop 'Dialoogvenster voor een nieuwe Log' in het linkerdeelvenster.
2. Typ 'Hallo er'.
3. Wacht even, de Bot automatisch moet reageren met "Hallo"
4. Voer 'byebye'
5. Wacht even, opnieuw de Bot automatisch moet reageren met "Hallo".
6. Klik op de knop 'Gedaan testen'.

#### <a name="view-and-correct-a-user-conversation"></a>Weergeven en corrigeren van de conversatie van een gebruiker
Met Log-dialoogvensters, kunt u de lijst met gesprekken weergeven gebruikers gehouden met uw Bot. U kunt ook deze corrigeren van de Bot antwoorden en de interacties als Training-dialoogvensters opslaan als u wilt bewerken. Om dat te doen:
1. Klik in het raster op het logboek van de conversatie.
2. Klik bijvoorbeeld op de laatste Bot-actie "Hallo".
3. Selecteer "Tot ziens!" om op te lossen de Bot.
4. Klik op de knop 'Opslaan als Train dialoogvenster'.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Inleiding tot Training](./02-intro-to-training.md)
