---
title: Over het gebruik van Negatable entiteiten met een Model van de cursist conversatie - Microsoft Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Informatie over het gebruik van Negatable entiteiten met een Model van de cursist conversatie.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 78dc759632c4fc3116a59ea1e5bc0b93200bca45
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58168184"
---
# <a name="how-to-use-negatable-entities-with-a-conversation-learner-model"></a>Over het gebruik van Negatable entiteiten met een Model van de cursist gesprek

In deze zelfstudie ziet u de eigenschap "Negatable" van entiteiten.

## <a name="video"></a>Video

[![Zelfstudie negatable entiteiten-Preview](https://aka.ms/cl_Tutorial_v3_NegatableEntities_Preview)](https://aka.ms/cl_Tutorial_v3_NegatableEntities)

## <a name="requirements"></a>Vereisten
In deze zelfstudie is vereist dat de algemene zelfstudie Bot wordt uitgevoerd

    npm run tutorial-general

## <a name="details"></a>Details
De eigenschap 'Negatable' van een entiteit kunt u beide normaal (positief) een label en negatieve exemplaren van de entiteit, leren op basis van positieve en negatieve modellen en schakelt u de waarde van een bestaande entiteit. Entiteiten met een set van de eigenschap 'Negatable' worden Negatable entiteiten in de conversatie Leaner genoemd.

## <a name="steps"></a>Stappen

Start op de startpagina in de Web-UI.

### <a name="create-the-model"></a>Het Model maken

1. Selecteer **nieuw Model**.
2. Voer **NegatableEntity** voor **naam**.
3. Selecteer **Maken**.

### <a name="entity-creation"></a>Entiteit maken

1. Selecteer **entiteiten** in het linker deelvenster, klikt u vervolgens **nieuwe entiteit**.
2. Selecteer **aangepaste getraind** voor **entiteitstype**.
3. Voer **naam** voor **entiteitnaam**.
4. Controleer **Negatable** zodat gebruikers kunnen de entiteitswaarde van een opgeven, of Stel dat er is iets *niet* een entiteit waarde waardoor de overeenkomstige entiteitswaarde verwijderen.
5. Selecteer **Maken**.

![](../media/T06_entity_create.png)

### <a name="create-the-first-action"></a>De eerste actie maken

1. Selecteer **acties** in het linker deelvenster, klikt u vervolgens **nieuwe actie**.
2. Voer **ik weet niet uw naam.** voor **van Bot-antwoord...** .
3. Voer **naam** voor **diskwalificeren voorziet**.
4. Selecteer **Maken**.

![](../media/T06_action_create_1.png)

### <a name="create-the-second-action"></a>De tweede actie maken

1. Selecteer **acties** in het linker deelvenster, klikt u vervolgens **nieuwe actie**.
2. Voer **ik weet dat uw naam. Het is $name.** voor **van Bot-antwoord...** .
3. Selecteer **Maken**.

> [!NOTE]
> De **naam** entiteit is automatisch toegevoegd als een **vereist entiteiten** verwezen in de reactie utterance.

U hebt nu twee acties.

![](../media/T06_action_create_2.png)

### <a name="train-the-model"></a>Het Model te trainen

1. Selecteer **Train-dialoogvensters** in het linker deelvenster, klikt u vervolgens **nieuwe dialoogvenster van de trein**.
2. Voer **hello** voor utterance van de gebruiker in het deelvenster links chat.
3. Selecteer **Score acties**.
4. Selecteer **ik weet niet uw naam.** uit de lijst met acties. Het percentiel is 100% als de enige geldige actie op basis van de beperkingen.
5. Voer **mijn naam is Frank** voor utterance van de gebruiker in het deelvenster links chat.
6. Markeer **Frank** Selecteer **+ naam**. Negatable entiteiten hebben twee instanties: (+) plus wordt toegevoegd of overschrijft de waarde; (-) min Hiermee verwijdert u de waarde.
7. Selecteer **Score acties**. De **naam** nu entiteit wordt gedefinieerd als **Frank** in het geheugen van het Model, waardoor de **ik weet dat uw naam. Het is $name** actie beschikbaar is.
8. Selecteer **ik weet dat uw naam. Het is $name.** uit de lijst met acties.
9. Voer **mijn naam is geen Frank.** voor utterance van de gebruiker in het deelvenster links chat.
10. Markeer **Frank** Selecteer **-naam** om te wissen van de waarde van de **naam** entiteit.
11. Selecteer **Score acties**.
12. Selecteer **ik weet niet uw naam.** uit de lijst met acties.
13. Voer **Susan, Mijn naam is.** voor de derde utterance van de gebruiker in het deelvenster links chat.
14. Markeer **Susan** vervolgens **+ naam** 

![](../media/T06_training.png)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Entiteiten van meerdere waarden](./07-multi-value-entities.md)
