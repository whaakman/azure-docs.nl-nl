---
title: Entiteit detectie callback gebruiken met een model Conversatiecursist - Microsoft Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Informatie over het gebruik van entiteit detectie callback met een Conversatiecursist-model.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: e9a3b0a2be58313266b949b907e4eb49a318a6dc
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51260071"
---
# <a name="how-to-use-entity-detection-callback"></a>Het gebruik van entiteit detectie terugbellen

In deze zelfstudie ziet u de callback van de detectie van entiteit en ziet u een algemeen patroon voor het oplossen van entiteiten.

## <a name="video"></a>Video

[![Zelfstudie-10-Preview](https://aka.ms/cl-tutorial-10-preview)](https://aka.ms/blis-tutorial-10)

## <a name="requirements"></a>Vereisten
In deze zelfstudie vereist dat de `tutorialEntityDetectionCallback` bot wordt uitgevoerd.

    npm run tutorial-entity-detection

## <a name="details"></a>Details
Detectie-callback entiteit kunt met behulp van aangepaste code voor het afhandelen van bedrijfsregels die betrekking hebben op entiteiten. In deze demo maakt gebruik van retouraanroepen en programmatische entiteiten op te lossen de plaatsnaam ingevoerd door de gebruiker naar een canonieke naam, bijvoorbeeld, het oplossen van 'de big apple' naar 'Antwerpen'.

### <a name="open-the-demo"></a>Open de demo

Klik in de modellijst op zelfstudie-10-EntityDetectionCallback. 

### <a name="entities"></a>Entiteiten

Drie entiteiten zijn gedefinieerd in het model.

![](../media/tutorial10_entities.PNG)

1. De plaat is een aangepaste entiteit die de gebruiker als tekstinvoer biedt.
2. CityUnknown is een programmatische entiteit. Deze entiteit worden de waarden door het systeem. Het kopieert u de invoer van de gebruiker als het systeem niet weet welke plaats is.
3. CityResolved is de plaats die het systeem kent. Deze entiteit is de plaats canonieke naam, die bijvoorbeeld 'big apple' naar 'new york' wordt opgelost.

### <a name="actions"></a>Acties

Drie acties zijn gedefinieerd in het model.

![](../media/tutorial10_actions.PNG)

1. De eerste actie is 'welke stad wilt u dat?'
2. De tweede is ' Ik weet niet deze plaats, $CityUknown. Welke plaats wilt u dat?'
3. De derde is "je al zei $City en ik die omgezet naar $CityResolved."

### <a name="callback-code"></a>Callback-code

We bekijken de code. U kunt de methode EntityDetectionCallback vinden in de C:\<installedpath > \src\demos\tutorialSessionCallbacks.ts-bestand.

![](../media/tutorial10_callbackcode.PNG)

Deze functie wordt aangeroepen nadat de entiteit oplossing werd gevonden.
 
- Het eerste wat dat het doet is duidelijk $CityUknown. $CityUknown blijven alleen behouden voor een enkel inschakelen als deze altijd wordt uitgeschakeld aan het begin.
- Haal vervolgens de lijst plaatsen die zijn herkend. Duren voordat het eerste certificaat en probeert om dit te verhelpen.
- De functie die wordt omgezet deze (resolveCity) is gedefinieerd verdere boven in de code. Er wordt een lijst van canonical plaatsnamen. De naam van de stad is gevonden in de lijst, wordt deze. Anders gezocht in 'cityMap' en retourneert de naam van de toegewezen. Als dit niet een stad kunt vinden, is het resultaat null.
- Ten slotte, als de plaats is omgezet naar een naam, we opslaan in $CityKnown entiteit. Anders, schakelt u wat de gebruiker heeft gezegd en $CityUknown entiteit te vullen.

### <a name="train-dialogs"></a>Dialoogvensters trainen

1. Klik op de trein dialoogvensters, en vervolgens Nieuw dialoogvenster van de trein.
2. Typ 'Hallo'.
3. Klik op Score acties en selecteer 'welke stad wilt u dat?'
2. Voer 'new york'.
    - De tekst wordt herkend als een entiteit plaats.
5. Klik op Score acties
    - `City` en `CityResolved` zijn ingevuld.
6. Selecteer 'je al zei $City en ik die omgezet naar $CityResolved'.
7. Klik op het onderwijs gereed.

Voeg een ander voorbeeld van dit dialoogvenster:

1. Klik op nieuwe dialoogvenster van de trein.
2. Typ 'Hallo'.
3. Klik op Score acties en selecteer 'welke stad wilt u dat?'
2. Voer 'big apple'.
    - De tekst wordt herkend als een entiteit plaats.
5. Klik op Score acties
    - `CityResolved` ziet u het effect van code die wordt uitgevoerd.
6. Selecteer 'je al zei $City en ik die omgezet naar $CityResolved'.
7. Klik op het onderwijs gereed.

Voeg een ander voorbeeld van dit dialoogvenster:

1. Klik op nieuwe dialoogvenster van de trein.
2. Typ 'Hallo'.
3. Klik op Score acties en selecteer 'welke stad wilt u dat?'
2. Voer "foo".
    - Dit is een voorbeeld van een plaats die het systeem niet weet. 
5. Klik op Score acties
6. Selecteer ' Ik weet niet deze plaats, $CityUknown. Welke plaats wilt u dat?'.
7. Voer 'new york'.
8. Klik op Score acties.
    - `CityUknown` is uitgeschakeld, en `CityResolved` is gevuld.
6. Selecteer 'je al zei $City en ik die omgezet naar $CityResolved'.
7. Klik op het onderwijs gereed.

![](../media/tutorial10_bigapple.PNG)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Sessie retouraanroepen](./11-session-callbacks.md)
