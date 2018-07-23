---
title: Demo Conversatiecursist model, pizza volgorde - Microsoft Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Informatie over het maken van een demo Conversatiecursist-model.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 052ef249f3367a562e5598b90533c0e52ed75df4
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/20/2018
ms.locfileid: "39171381"
---
# <a name="demo-pizza-order"></a>Demo: Pizza volgorde
Deze demonstratie ziet u een pizza bot bestellen. Het ondersteunt ordening van een enkele pizza met deze functionaliteit:

- pizza toppings in gebruiker uitingen herkennen
- controleren of pizza toppings op voorraad of niet verkrijgbaar zijn en het reageren op de juiste wijze
- pizza toppings vanuit een eerdere bestelling onthouden en aanbieding op - een nieuwe bestelling beginnen met de dezelfde toppings

## <a name="video"></a>Video

[![Demo Pizza-Preview](http://aka.ms/cl-demo-pizza-preview)](http://aka.ms/blis-demo-pizza)

## <a name="requirements"></a>Vereisten
In deze zelfstudie is vereist dat de volgorde pizza bot wordt uitgevoerd

    npm run demo-pizza

### <a name="open-the-demo"></a>Open de demo

Klik in de lijst Model van de web-UI op TutorialDemo Pizza volgorde. 

## <a name="entities"></a>Entiteiten

U hebt drie entiteiten gemaakt.

- Toppings: deze entiteit wordt de toppings die de gebruiker gevraagd om oplopen. Het bevat de geldige toppings die er op voorraad zijn. Er wordt gecontroleerd om te zien of een BBC in of niet verkrijgbaar.
- NietInVoorraad: deze entiteit wordt gebruikt voor communicatie terug naar de gebruiker dat de geselecteerde BBC zich niet in voorraad.
- LastToppings: zodra een bestelling wordt geplaatst, wordt deze entiteit gebruikt om te bieden aan de gebruiker de lijst met toppings van de volgorde.

![](../media/tutorial_pizza_entities.PNG)

### <a name="actions"></a>Acties

U hebt een set acties, met inbegrip van de gebruiker wordt gevraagd ze willen op hun pizza gemaakt om aan te geven wat ze hebben toegevoegd tot nu toe, enzovoort.

Er zijn ook twee API-aanroepen:

- FinalizeOrder: om de volgorde voor de pizza
- UseLastToppings: voor het migreren van de toppings van eerdere bestelling 

![](../media/tutorial_pizza_actions.PNG)

### <a name="training-dialogs"></a>Dialoogvensters voor training
U kunt een handvol training-dialoogvensters hebt gedefinieerd. 

![](../media/tutorial_pizza_dialogs.PNG)

Bijvoorbeeld: we proberen een sessie voor onderwijs.

1. Klik op de trein dialoogvensters, en vervolgens Nieuw dialoogvenster van de trein.
1. Voer 'order een pizza'.
2. Klik op Score actie.
3. Klik om te selecteren 'Wat wilt u dat op uw pizza?'
4. Voer 'paddestoelen en kaas'.
    - U ziet dat als Toppings LUIS heeft het label. Als dat niet juist is, kan u Klik om te markeren en vervolgens deze te corrigeren.
    - Het teken '+' naast de entiteit houdt in dat deze wordt toegevoegd aan de set toppings.
5. Klik op Score acties.
    - U ziet dat `mushrooms` en `cheese` zich niet in het geheugen voor Toppings.
3. Klik om te selecteren, hebt u $Toppings uw pizza'
    - U ziet dat dit is een niet-wait-actie, zodat de bot wordt u gevraagd voor de volgende actie.
6. Selecteer 'Wilt u iets anders?'
7. Voer 'paddestoelen verwijderen en peppers toevoegen'.
    - U ziet dat `mushroom` heeft een '-' aanmelding ernaast en om te worden verwijderd. En `peppers` heeft een teken '+' ernaast en toe te voegen aan de toppings.
2. Klik op Score actie.
    - U ziet dat `peppers` nu in vet weergegeven omdat deze is er nieuw is. En `mushrooms` is gepasseerd uit.
8. Klik om te selecteren, hebt u $Toppings uw pizza'
6. Selecteer 'Wilt u iets anders?'
7. Voer 'add erwten'.
    - `Peas` een voorbeeld van een toegenomen dat niet verkrijgbaar is is. Het is nog steeds met het label een toegenomen.
2. Klik op Score actie.
    - `Peas` wordt weergegeven als NietInVoorraad.
    - Als u wilt zien hoe dit is gebeurd, opent u de code hier `C:\<\installedpath>\src\demos\demoPizzaOrder.ts`. De methode EntityDetectionCallback kijken. Deze methode wordt aangeroepen na elke BBC om te zien of er op voorraad. Zo niet, het wist u deze uit de set van toppings en toevoegt aan de entiteit NietInVoorraad. De variabele inStock is boven deze methode met de lijst in voorraad toppings gedefinieerd.
6. Selecteer 'Er zijn geen $OutOfStock'.
7. Selecteer 'Wilt u iets anders?'
8. Voer 'Nee'.
9. Klik op Score actie.
10. Selecteer 'FinalizeOrder' API-aanroep. 
    - Hiermee wordt de functie 'FinalizeOrder' is gedefinieerd in code aanroepen. Dit toppings worden gewist en wordt uw bestelling is onderweg. 
2. Voer 'een andere volgorde'. Een nieuwe bestelling wordt gestart.
9. Klik op Score actie.
    - 'kaas' en 'peppers' zijn in het geheugen als toppings van de laatste order.
1. Selecteer 'Wilt u $LastToppings'.
2. Voer 'Ja'
3. Klik op Score actie.
    - De bot wil de UseLastToppings actie te ondernemen. De tweede van de twee callbackmethoden is. De laatste order toppings kopiëren naar toppings wordt en laatste toppings wissen. Dit is een manier om de laatste order onthouden en als de gebruiker aangeeft dat ze een andere pizza wilt dat, biedt deze toppings als opties.
2. Klik om te selecteren, hebt u $Toppings uw pizza'.
3. Selecteer 'Wilt u iets anders?'
8. Voer 'Nee'.
4. Klik op het onderwijs gereed.

![](../media/tutorial_pizza_callbackcode.PNG)

![](../media/tutorial_pizza_apicalls.PNG)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Demo - VR-app starten](./demo-vr-app-launcher.md)
