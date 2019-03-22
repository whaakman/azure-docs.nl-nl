---
title: Entiteit Resolvers in een Model van de cursist conversatie - Microsoft Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Informatie over het gebruik van entiteit Resolvers in Conversatiecursist.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: cca78e2536a922165f40bbcbabcae005021aa70b
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58167844"
---
# <a name="entity-resolvers"></a>Entiteit Resolvers

In deze zelfstudie ziet u hoe u entiteit Resolvers in Conversatiecursist

## <a name="video"></a>Video

[![Entiteit Resolvers zelfstudie Preview](https://aka.ms/cl_Tutorial_v3_EntityResolvers_Preview)](https://aka.ms/cl_Tutorial_v3_EntityResolvers)

## <a name="requirements"></a>Vereisten
In deze zelfstudie is vereist dat de algemene zelfstudie Bot wordt uitgevoerd

    npm run tutorial-general

## <a name="details"></a>Details

- Conflictoplosser Type is een optionele eigenschap van aangepaste entiteiten.
- Entiteit Resolvers gebruikmaken van de kracht van de kenmerken van de vooraf getrainde entiteit in LUIS voor aanvullende details, heldere berichten van uw aangepaste entiteit.

## <a name="steps"></a>Stappen

Start op de startpagina in de Web-UI.

### <a name="create-the-model"></a>Het Model maken

1. Selecteer **nieuw Model**.
2. Voer **entiteit Resolvers** voor **naam**.
3. Selecteer **Maken**.

### <a name="create-a-pair-of-entities"></a>Maak een sleutelpaar met een van de entiteiten

1. Selecteer **entiteiten** in het linker deelvenster, klikt u vervolgens **nieuwe entiteit**.
2. Voer **vertrek** voor **entiteitnaam**.
3. Selecteer **datetimeV2** voor **conflictoplosser Type**.
4. Selecteer **Maken**. Het informatieve pop-upvenster sluiten door te selecteren **OK**.
5. Herhaal stap 1-4 voor het maken van een tweede entiteit met de naam **retourneren** met **datetimeV2** conflictoplosser type.

![](../media/T09_entities.png)

### <a name="create-a-pair-of-actions"></a>Maak een sleutelpaar met een van de acties

1. Selecteer **acties** in het linker deelvenster, klikt u vervolgens **nieuwe actie**.
2. Voer **u verlaten op $departure en retourneren op $return** voor **van Bot-antwoord...** .
    - BELANGRIJK - als te typen in de $[entityName], moet u druk op enter of klik op voor de entiteit in de vervolgkeuzelijst anders Conversatiecursist worden beschouwd als deze optie om te worden van tekst in plaats van een entiteit.
    - U ziet dat de **vereist entiteiten** veld ook krijgen deze entiteiten en kan niet worden verwijderd. Dit voorkomt dat deze actie niet langer beschikbaar totdat beide entiteiten aanwezig zijn vereist.
3. Selecteer **Maken**.
4. Selecteer **nieuwe actie** te maken van een tweede actie.
5. Voer **wanneer u van plan bent om te reizen?** voor **van Bot-antwoord...** .
6. Voer **vertrek** en **retourneren** voor **diskwalificeren entiteiten**. Deze Vertel ons Bot deze actie niet uitvoeren als een van deze entiteiten een waarde bevatten.
7. Selecteer **Maken**.

![](../media/T09_actions.png)

### <a name="training"></a>Training

1. Bekijk de **Training: [Status]** in de linkerbovenhoek voor **voltooid**.
    - Als het hierdoor te lang duurt, kunt u de koppeling "Vernieuwen" klikken.
    - Training status is 'Voltooid' nodig zodat onze Resolvers entiteit werken wanneer we het Model te trainen.

2. In het linkerdeelvenster klikt u op 'Train-dialoogvensters', en klik vervolgens op de knop 'Nieuwe Train dialoogvenster'.
3. Type in de eerste gebruiker-utterance 'book me een vlucht'. 
4. Klik op de knop 'Acties Score'.
5. Selecteer het antwoord, "Wanneer u van plan bent om te reizen?".
6. Als de gebruiker reageren met, 'morgen verlaten en retourneren zondag volgende week'.
    - U ziet hoe Conversatiecursist schakelt u twee 'Vooraf getrainde datum' entiteiten in dat de gebruiker heeft gedetecteerd.
7. In het deelvenster "Entiteit detectie", selecteert u de tekst 'morgen' en deze te labelen als "vertrek"
8. Label ook de tekst 'Zondag volgende week' als 'return'
9. Klik op de knop 'Acties Score'.
    - U ziet hoe het deelvenster 'Geheugen' bevat de datums vertrek en keer terug.
    - Beweeg de muisaanwijzer over elkaar en bekijk hoe de entiteiten date-objecten die duidelijk vastleggen van de werkelijke datum in plaats van "Zondag" of 'morgen' zijn.
10. Selecteer de "u verlaat op...' Bot-antwoord.
11. Klik op de knop 'Opslaan'.

![](../media/T09_training.png)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Alternatieve invoer](./10-alternative-inputs.md)
