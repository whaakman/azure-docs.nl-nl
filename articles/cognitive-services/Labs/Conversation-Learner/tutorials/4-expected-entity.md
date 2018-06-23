---
title: Het gebruik van de eigenschap 'verwacht entiteit' van de conversatie cursist acties - cognitieve Microsoft-Services | Microsoft Docs
titleSuffix: Azure
description: Informatie over het gebruik van de eigenschap 'verwacht entiteit' van een conversatie cursist-toepassing.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 83911eba8112cf356af8c4cd562a87f746fbabc5
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345299"
---
# <a name="how-to-use-the-expected-entity-property-of-actions"></a>Het gebruik van de eigenschap 'Verwacht entiteit' acties

Deze zelfstudie wordt het veld 'verwacht entiteit' van acties.

## <a name="requirements"></a>Vereisten
Deze zelfstudie vereist dat de algemene zelfstudie bot wordt uitgevoerd

    npm run tutorial-general

## <a name="details"></a>Details
Gebruik het veld 'verwacht entiteit' van een actie om te communiceren met het systeem waarop u verwacht dat de reactie van de gebruiker aan een actie worden dat naar een entiteit status.

Concrete invulling te geven, als het veld 'verwacht entiteit' van een actie is ingesteld op $entity, klikt u op de volgende gebruiker utterance, wordt het systeem:

1. Proberen eerst, zoals gewoonlijk te vinden met behulp van de machine learning gebaseerde extractie entiteitsmodel entiteiten
2. Als er geen entiteiten worden gevonden in stap 1, als een heuristiek----vervolgens moet u de hele gebruiker utterance toewijzen aan $entity.
3. Roep `EntityDetectionCallback` gebruikelijke en Ga door met de actie selecteren.

## <a name="steps"></a>Stappen

### <a name="create-the-application"></a>De toepassing maken

1. Klik op nieuwe App in de Webgebruikersinterface
2. Voer in het vak Naam ExpectedEntities. Klik vervolgens op maken.

### <a name="create-an-entity"></a>Maak een entiteit

1. Klik op entiteiten en vervolgens nieuwe entiteit.
2. Voer de naam in de naam van de entiteit.
3. Klik op Maken

Houd er rekening mee voor het entiteitstype is 'custom'--Dit betekent dat de entiteit kan worden getraind.  Er zijn ook vooraf samengestelde entiteiten, wat betekent dat hun gedrag kan niet worden gecorrigeerd--deze in een andere zelfstudie vallen.

![](../media/tutorial4_entities.PNG)

### <a name="create-two-actions"></a>Twee acties maken

1. Klik op acties en vervolgens nieuwe actie
2. In het antwoord, typ 'Wat is de naam van uw?'.
3. Voer $name in entiteiten verwacht. Klik op opslaan.
    - Dit betekent dat als deze vraag is gevraagd en het antwoord van de gebruiker beschikt niet over de entiteiten die zijn gedetecteerd, de bot moet wordt ervan uitgegaan dat de volledige antwoord van de gebruiker is deze entiteit.
2. Klik op acties en vervolgens een nieuwe actie voor het maken van een tweede actie.
3. Typ in het antwoord 'Hallo $name'.
    - Houd er rekening mee dat de entiteit wordt automatisch toegevoegd als een disqualifying entiteit. 
4. Op Opslaan klikken

U hebt nu twee acties.

![](../media/tutorial4_actions.PNG)

### <a name="train-the-bot"></a>De bot trainen

1. Klik op de trein dialoogvensters, klikt u vervolgens nieuwe Train dialoogvenster.
2. Typ 'Hallo'.
3. Score acties op en selecteer 'Wat is de naam van uw?'
    - Opmerking dat het antwoord 'Hallo $name' kan niet worden geselecteerd omdat het requies de entiteit $name worden gedefinieerd en $name is niet in het geheugen van de bot.
2. Voer 'david'. 
    - Houd er rekening mee dat de naam is gemarkeerd als een entiteit. Dit is vanwege de heuristiek die we hierboven hebt ingesteld om te selecteren van het antwoord als de entiteit.
5. Klik op Score-acties
    - Opmerking naamwaarde is nu in het geheugen van de bot.
    - 'Hallo $name' is nu beschikbaar als een antwoord. 
6. Selecteer 'Hello $name'.
7. Klik op het onderwijs gereed.

Hier vindt u twee voorbeelden waarin de extractie van machine learning entiteitsmodel een naam, identificeert zodat de heuristiek 'verwacht entiteit' is niet geactiveerd.

1. Klik op nieuwe Train dialoogvenster.
2. Typ 'Mijn naam is david'.
    - Houd er rekening mee dat david als de naam entiteit geeft omdat dit woord voordat die.
2. Klik op Score-acties
3. Selecteer 'Hello $name'.
4. Typ 'Mijn naam is susan'.
    - Houd er rekening mee dat het susan als de naam, identificeert aangezien dit patroon al die.
2. Klik op Score acties.
2. Selecteer 'Hallo susan'.
3. Klik op het onderwijs gereed.

Hier ziet u twee meer voorbeelden waarbij de heuristiek 'verwacht entiteit' activeert, maar is onjuist en hoe kunnen we een correctie zijn.

1. Typ 'Bel mij jose'.
    - Houd er rekening mee dat deze worden niet herkend door de naam als een entiteit.
2. Klik op jose en selecteert u de naam.
3. Klik op Score acties.
4. Selecteer Hallo $name.
5. Klik op het onderwijs gereed.
1. Klik op nieuwe Train dialoogvenster.
2. Voer 'Hallo'.
3. Voer in reactie op 'wat de naam van uw is', 'Ik aangeroepen frank'.
    - Houd er rekening mee dat de hele woordgroep is geselecteerd. Dit is omdat het statistische model een naam niet gevonden heeft, zodat de heuristiek gestart en wordt het volledige antwoord geselecteerd als de naam entiteit.
2. Om dit te corrigeren, klikt u op de gemarkeerde woordgroep en klik vervolgens op de rode x. 
3. Schakel frank en klik vervolgens op naam.
2. Klik op Score-acties
3. Selecteer 'Hello $name'.
4. Klik op het onderwijs gereed.

![](../media/tutorial4_dialogs.PNG)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Negatable entiteiten](./5-negatable-entities.md)
