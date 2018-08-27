---
title: Het gebruik van de eigenschap 'verwachte entiteit' van Conversatiecursist acties - Microsoft Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Informatie over het gebruik van de eigenschap 'verwachte entiteit' van een model Conversatiecursist.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: fb9193066c9501341efb779b9f9e2ccace02cccf
ms.sourcegitcommit: f1e6e61807634bce56a64c00447bf819438db1b8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/24/2018
ms.locfileid: "42888201"
---
# <a name="how-to-use-the-expected-entity-property-of-actions"></a>Het gebruik van de eigenschap 'Verwachte entiteit' van acties

In deze zelfstudie ziet u het veld 'verwachte entiteit' van acties.

## <a name="video"></a>Video

[![Zelfstudie 4-Preview](http://aka.ms/cl-tutorial-04-preview)](http://aka.ms/blis-tutorial-04)

## <a name="requirements"></a>Vereisten
In deze zelfstudie is vereist dat de algemene zelfstudie bot wordt uitgevoerd

    npm run tutorial-general

## <a name="details"></a>Details
Gebruik het veld 'verwachte entiteit' van een actie om te communiceren met het systeem dat u verwacht dat de reactie van de gebruiker op een actie is om aan te geven van een entiteit.

Concrete invulling te geven, als het veld 'verwachte entiteit' van een actie is ingesteld op $entity, klikt u op de volgende gebruiker utterance, wordt het systeem:

1. Proberen eerst, zoals gewoonlijk te vinden van entiteiten met behulp van het model van de extractie op basis van machine learning-entiteit
2. Als er geen entiteiten worden gevonden in stap 1, klikt u vervolgens--als een heuristiek--moet u de hele gebruiker utterance toewijzen aan $entity.
3. Bel `EntityDetectionCallback` zoals gewoonlijk en gaat u verder met de actie selecteren.

## <a name="steps"></a>Stappen

### <a name="create-the-model"></a>Het model maken

1. Klik op Nieuw Model in de Web-UI
2. Voer in het vak Naam ExpectedEntities. Klik vervolgens op maken.

### <a name="create-an-entity"></a>Een entiteit maken

1. Klik op entiteiten, en vervolgens nieuwe entiteit.
2. Voer de naam in de naam van de entiteit.
3. Klik op Maken

> [!NOTE]
> Het entiteitstype is 'custom'. Deze waarde betekent dat de entiteit kan worden getraind.  Er zijn ook vooraf gemaakte entiteiten, wat betekent dat hun gedrag kan niet worden aangepast.  Deze entiteiten worden behandeld in de [vervolgbrief entiteiten zelfstudie](./7-built-in-entities.md).

![](../media/tutorial4_entities.PNG)

### <a name="create-two-actions"></a>Twee acties maken

1. Klik op acties en vervolgens nieuwe actie.
2. Typ 'Wat is de naam van uw?' in het antwoord.
3. Voer $name in entiteiten verwacht. Klik op Opslaan.
    - Deze waarde betekent dat als deze vraag wordt gesteld en de reactie van de gebruiker beschikt niet over de entiteiten die zijn gedetecteerd, de bot moet ervan uitgaan dat het geheel van het antwoord van de gebruiker is deze entiteit.
2. Klik op acties en vervolgens een nieuwe actie voor het maken van een tweede actie.
3. Typ in het antwoord 'Hallo $name'.
    - De entiteit wordt automatisch toegevoegd als een vereiste entiteiten. 
4. Klik op Opslaan.

U hebt nu twee acties.

![](../media/tutorial4_actions.PNG)

### <a name="train-the-bot"></a>De bot te trainen

1. Klik op de trein dialoogvensters, en vervolgens Nieuw dialoogvenster van de trein.
2. Typ 'Hallo'.
3. Klik op Score acties en selecteer 'Wat is de naam van uw?'
    - Het antwoord 'Hallo $name' kan niet worden geselecteerd omdat hiervoor de entiteit $name worden gedefinieerd en $name niet in het geheugen van de bot is.
2. Voer 'david'. 
    - De naam is gemarkeerd als een entiteit. Dit is vanwege de heuristiek die we hierboven hebt ingesteld om te selecteren van het antwoord als de entiteit.
5. Klik op Score acties
    - De naamwaarde is nu in het geheugen van de bot.
    - 'Hallo $name' is nu beschikbaar als een antwoord. 
6. Selecteer 'Hallo $name'.
7. Klik op het onderwijs gereed.

Hier volgen twee voorbeelden waarbij een naam door de machine learning-model extractie van entiteit wordt aangeduid, zodat de heuristiek 'verwacht entiteit' wordt niet geactiveerd.

1. Klik op nieuwe dialoogvenster van de trein.
2. Typ 'Mijn naam is david'.
    - Omdat deze is dit woord voordat zichtbaar, het model david identificeert als de naam van entiteit.
2. Klik op Score acties
3. Selecteer 'Hallo $name'.
4. Typ 'Mijn naam is susan'.
    - Omdat deze is dit patroon al gezien, het model susan identificeert als de naam.
2. Klik op Score acties.
2. Selecteer 'Hallo susan'.
3. Klik op het onderwijs gereed.

In de volgende voorbeelden wordt de 'verwachte entiteit' heuristiek wordt geactiveerd, maar is onjuist. De voorbeelden worden vervolgens hoe u een correctie.

1. Typ 'Bel me jose'.
    - Het model wordt de naam niet herkend als een entiteit.
2. Klik op jose en selecteert u de naam.
3. Klik op Score acties.
4. Hallo $name selecteren.
5. Klik op het onderwijs gereed.
1. Klik op nieuwe dialoogvenster van de trein.
2. Voer 'Hallo'.
3. In reactie op 'wat de naam van uw is', voer 'Ik ben frank genaamd'.
    - De volledige woordgroep wordt gemarkeerd. Dit komt doordat de statistische model een naam, niet vinden heeft, zodat de heuristiek is geactiveerd en het volledige antwoord geselecteerd als de naam van entiteit.
2. Om deze te corrigeren, klikt u op de gemarkeerde zin, en klik vervolgens op de rode x. 
3. Schakel frank en klik vervolgens op de naam op.
2. Klik op Score acties
3. Selecteer 'Hallo $name'.
4. Klik op het onderwijs gereed.

![](../media/tutorial4_dialogs.PNG)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Negatable entiteiten](./5-negatable-entities.md)
