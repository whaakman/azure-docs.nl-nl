---
title: Entiteit detectie callback gebruiken met de toepassing van een conversatie cursist - cognitieve Microsoft-Services | Microsoft Docs
titleSuffix: Azure
description: Informatie over het entiteit detectie callback gebruiken met een conversatie cursist-toepassing.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: e41ea5930ff0c8395d0c93aa42e224ebfc894ba8
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345287"
---
# <a name="how-to-use-entity-detection-callback"></a>Het gebruik van de entiteit detectie terugbellen

In deze zelfstudie toont de callback van de detectie van entiteit en ziet u een algemene patroon voor het oplossen van entiteiten.

## <a name="requirements"></a>Vereisten
Deze zelfstudie vereist dat de bot 'tutorialEntityDetectionCallback' wordt uitgevoerd.

    npm run tutorial-entity-detection

## <a name="details"></a>Details
Entiteit detectie callback kan met behulp van aangepaste code voor het afhandelen van bedrijfsregels die betrekking hebben op entiteiten. In deze demonstratie gebruiken we retouraanroepen en programmatische entiteiten om op te lossen de plaatsnaam ingevoerd door de gebruiker naar een canonieke naam--bijvoorbeeld, het oplossen van 'de big apple' naar 'new york'.

### <a name="open-the-demo"></a>Open de demo

Klik in de lijst met Apps op zelfstudie-10-EntityDetenctionCallback. 

### <a name="entities"></a>Entiteiten

We hebben drie entiteiten gedefinieerd in de toepassing.

![](../media/tutorial10_entities.PNG)

1. Plaats is een aangepaste entiteit die de gebruiker als tekstinvoer krijgt.
2. CityUnknown is een programmatische entiteit. Dit wordt door het systeem ophalen ingevuld. Als het systeem niet weet welke stad is, wordt het de gebruikersinvoer kopiëren.
3. CityResolved is de plaats die het systeem kent. Dit is de plaats van canonieke naam, die bijvoorbeeld 'big apple' wordt omgezet in 'new york'.

### <a name="actions"></a>Acties

We hebt drie acties gemaakt. 

![](../media/tutorial10_actions.PNG)

1. De eerste actie is, welke stad wilt u?'
2. De tweede ' Ik weet deze plaats, $CityUknown niet. Welke stad wilt u toch?'
3. Het derde is "u $City gezegd, en ik die omgezet naar $CityResolved."

### <a name="callback-code"></a>Callback code

We bekijken de code. U kunt de methode EntityDetectionCallback vinden in de C:\<installedpath > \src\demos\tutorialSessionCallbacks.ts-bestand.

![](../media/tutorial10_callbackcode.PNG)

Deze functie wordt aangeroepen nadat de omzetting van de entiteit heeft plaatsgevonden.
 
- Het eerste wat dat het doet is duidelijk $CityUknown. $CityUknown alleen bewaard voor een enkele inschakelen als deze altijd wordt uitgeschakeld aan het begin.
- We u vervolgens de lijst met plaatsen die zijn herkend. Eerste nemen en proberen te verhelpen.
- De functie dat het oplost (resolveCity) is gedefinieerd verdere boven in de code. Er wordt een lijst met plaatsnamen van de canonieke. De plaatsnaam gevonden in de lijst, wordt deze. Anders raadpleegt 'cityMap' en retourneert de naam van de toegewezen. Als een plaats niet wordt gevonden, is het resultaat null.
- Ten slotte, als de plaats is omgezet naar een naam, we opslaan in $CityKnown entiteit. Anders, schakelt u wat de gebruiker heeft gezegd en $CityUknown entiteit te vullen.

### <a name="train-dialogs"></a>Dialoogvensters trainen

1. Klik op de trein dialoogvensters, klikt u vervolgens nieuwe Train dialoogvenster.
2. Typ 'Hallo'.
3. Score acties op en selecteer 'welke stad wilt u?'
2. Voer 'new york'.
    - Houd er rekening mee dat deze wordt opgehaald herkend als een entiteit plaats.
5. Klik op Score-acties
    - Houd er rekening mee dat plaats en CityResolved zijn ingevuld.
6. 'U $City gezegd en ik die omgezet naar $CityResolved' selecteren.
7. Klik op het onderwijs gereed.

Voeg een ander voorbeeld van dit dialoogvenster:

1. Klik op nieuwe Train dialoogvenster.
2. Typ 'Hallo'.
3. Score acties op en selecteer 'welke stad wilt u?'
2. Voer 'big apple'.
    - Houd er rekening mee dat deze wordt opgehaald herkend als een entiteit plaats.
5. Klik op Score-acties
    - Houd er rekening mee dat CityResolved ziet u het effect van code die wordt uitgevoerd.
6. 'U $City gezegd en ik die omgezet naar $CityResolved' selecteren.
7. Klik op het onderwijs gereed.

Voeg een ander voorbeeld van dit dialoogvenster:

1. Klik op nieuwe Train dialoogvenster.
2. Typ 'Hallo'.
3. Score acties op en selecteer 'welke stad wilt u?'
2. Voer 'foo'.
    - Dit is een voorbeeld van een stad die het systeem niet weet. 
5. Klik op Score-acties
6. Selecteer ' Ik weet deze plaats, $CityUknown niet. Welke stad wilt u toch?'.
7. Voer 'new york'.
8. Klik op Score acties.
    - Houd er rekening mee dat CityUknown is gewist en CityResolved wordt gevuld.
6. 'U $City gezegd en ik die omgezet naar $CityResolved' selecteren.
7. Klik op het onderwijs gereed.

![](../media/tutorial10_bigapple.PNG)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Sessie-retouraanroepen](./11-session-callbacks.md)
