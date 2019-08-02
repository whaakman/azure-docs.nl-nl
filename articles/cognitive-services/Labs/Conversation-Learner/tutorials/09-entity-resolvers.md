---
title: Resolvers van entiteit in een Conversation Learner model-Azure Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Meer informatie over het gebruik van entiteits-resolvers in Conversation Learner.
services: cognitive-services
author: nitinme
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: 60c4abf1590cb91fd460cc6a2a5ba75a225ebd80
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68704045"
---
# <a name="entity-resolvers"></a>Entiteits conflicten

In deze zelf studie ziet u hoe u met behulp van entiteits-resolvers in Conversation Learner

## <a name="video"></a>Video

[![Preview-versie van zelf studie voor entiteits oplossingen](https://aka.ms/cl_Tutorial_v3_EntityResolvers_Preview)](https://aka.ms/cl_Tutorial_v3_EntityResolvers)

## <a name="requirements"></a>Vereisten
Voor deze zelf studie moet de bot van de algemene zelf studie worden uitgevoerd

    npm run tutorial-general

## <a name="details"></a>Details

- Resolver-type is een optionele eigenschap van aangepaste entiteiten.
- Met de functies voor het oplossen van entiteiten wordt gebruikgemaakt van de kracht van de vooraf getrainde entiteit recognizers in LUIS om meer details en duidelijkheid te bieden voor uw aangepaste entiteit.

## <a name="steps"></a>Stappen

Start op de start pagina in de gebruikers interface van het web.

### <a name="create-the-model"></a>Het model maken

1. Selecteer **Nieuw model**.
2. Voer de voor **naam**van entiteits-resolvers in.
3. Selecteer **Maken**.

### <a name="create-a-pair-of-entities"></a>Een paar entiteiten maken

1. Selecteer **entiteiten** in het linkerdeel venster en vervolgens **nieuwe entiteit**.
2. Voer **vertrek** in als **entiteits naam**.
3. Selecteer **datetimeV2** voor het **type resolver**.
4. Selecteer **Maken**. Sluit het pop-upvenster en selecteer **OK**.
5. Herhaal de stappen 1-4 om een tweede entiteit met de naam **return** with **datetimeV2** resolver te maken.

![](../media/T09_entities.png)

### <a name="create-a-pair-of-actions"></a>Een paar acties maken

1. Selecteer **acties** in het linkerdeel venster en vervolgens op **nieuwe actie**.
2. Voer **in $Departure en retour neren op $Return** voor **de reactie van de bot...**
    - BELANG rijk: wanneer u $ [entitynamenaam] typt, moet u op ENTER drukken of op de entiteit klikken in de vervolg keuzelijst, Conversation Learner anders wordt dit in plaats van een entiteit beschouwd als tekst.
    - U ziet dat in het veld **vereiste entiteiten** ook deze entiteiten worden opgehaald en niet kunnen worden verwijderd. Hiermee wordt voor komen dat deze actie beschikbaar is totdat de vereiste entiteiten aanwezig zijn.
3. Selecteer **Maken**.
4. Selecteer **nieuwe actie** om een tweede actie te maken.
5. Voer **in wanneer u van plan bent om op reis te gaan?** voor **het antwoord van de bot...** .
6. Voer **vertrek** en **retour** in voor diskwalificerende **entiteiten**. Deze actie wordt niet uitgevoerd als een van deze entiteiten een waarde bevat.
7. Selecteer **Maken**.

![](../media/T09_actions.png)

### <a name="training"></a>Training

1. Bekijk de **Training: [status]** in de linkerbovenhoek voor **voltooide taken**.
    - U kunt op de koppeling vernieuwen klikken als dit te lang duurt.
    - De trainings status ' voltooid ' is nodig om ervoor te zorgen dat onze entiteits oplossingen werken wanneer we het model trainen.

2. Klik in het linkerpaneel op ' dialoog vensters trainen ' en klik vervolgens op de knop Nieuw trainen dialoog venster.
3. Typ de eerste gebruiker utterance, "Ik wil een vlucht" boeken. 
4. Klik op de knop ' Score acties '.
5. Selecteer het antwoord ' wanneer bent u van plan om naar reis te gaan? '.
6. Meld u als de gebruiker aan, "verlaat morgen en zondag is de volgende week!".
    - U ziet hoe Conversation Learner twee ' vooraf getrainde ' datum entiteiten in die gebruiker heeft gedetecteerd.
7. Selecteer in het deel venster ' entiteits detectie ' de tekst ' morgen ' en voorzie deze als ' afwijking '
8. Voorzie ook de tekst "zondag volgende week" als "Return"
9. Klik op de knop ' Score acties '.
    - U ziet hoe het deel venster geheugen uw vertrek-en retour datums bevat.
    - Beweeg de muis aanwijzer boven elkaar en Bekijk hoe de entiteiten datum objecten zijn waarmee duidelijk de werkelijke kalender datum wordt vastgelegd in plaats van ' zondag ' of ' morgen '.
10. Selecteer de optie ' u bent bezig met... ' Bot-antwoord.
11. Klik op de knop Opslaan.

![](../media/T09_training.png)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Entiteiten opsommen](./tutorial-enum-set-entity.md)
