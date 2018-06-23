---
title: Demo conversatie cursist toepassing, pizza volgorde - cognitieve Microsoft-Services | Microsoft Docs
titleSuffix: Azure
description: Informatie over het maken van een demo conversatie cursist toepassing.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 99cd89c4f4430f2d65ed0963e3092d51a83842d7
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345321"
---
# <a name="demo-pizza-order"></a>Demo: Pizza volgorde
Deze demo ziet u een pizza bot bestellen. Het ondersteunt ordening van een enkele pizza met deze functionaliteit:

- herkent pizza toppings in utterances gebruiker
- controle of pizza toppings zijn in voorraad of buiten stock en reageert op de juiste wijze
- Onthoud pizza toppings van een vorige volgorde en aan te bieden tot - een nieuwe order beginnen met de dezelfde toppings

## <a name="requirements"></a>Vereisten
Deze zelfstudie vereist dat de volgorde pizza bot wordt uitgevoerd

    npm run demo-pizza

### <a name="open-the-demo"></a>Open de demo

Klik in de lijst met Apps van de webgebruikersinterface, op TutorialDemo Pizza volgorde. 

## <a name="entities"></a>Entiteiten

We hebt drie entiteiten gemaakt.

- Toppings: kan de toppings die de gebruiker gevraagd wordt verzameld. Dit omvat de geldige toppings in stock. Controleert deze of een toegenomen is in- of stock.
- NietInVoorraad: dit wordt gebruikt om te communiceren terug naar de gebruiker dat niet in stock geselecteerde toegenomen is.
- LastToppings: wanneer een order is geplaatst, wordt deze entiteit gebruikt om te bieden aan de gebruiker de lijst met toppings op volgorde.

![](../media/tutorial_pizza_entities.PNG)

### <a name="actions"></a>Acties

Er is een reeks acties, met inbegrip van de gebruiker wordt gevraagd ze willen op hun pizza, om aan te geven wat ze tot nu toe hebt toegevoegd, enzovoort gemaakt.

Er zijn twee API-aanroepen:

- FinalizeOrder: de volgorde voor de pizza plaatsen
- UseLastToppings: voor het migreren van de toppings van vorige volgorde 

![](../media/tutorial_pizza_actions.PNG)

### <a name="training-dialogs"></a>Training dialoogvensters
We hebben een handvol training dialoogvensters gedefinieerd. 

![](../media/tutorial_pizza_dialogs.PNG)

Als u bijvoorbeeld een sessie onderwijs eens te proberen.

1. Klik op de trein dialoogvensters, klikt u vervolgens nieuwe Train dialoogvenster.
1. Voer in volgorde van een pizza.
2. Klik op de actie Score.
3. Klik om te selecteren, wat wilt u dat op uw pizza?'
4. Voer 'paddestoelen en kaas'.
    - Kennisgeving LUIS heeft met beide als Toppings labels. Als dat niet juist is, kunt u Selecteer vervolgens corrigeren.
    - Het teken '+' naast de entiteit houdt in dat deze wordt toegevoegd aan de set toppings.
5. Klik op Score acties.
    - Kennisgeving paddestoelen en kaas zich niet in het geheugen voor Toppings.
3. Schakel 'er $Toppings op uw pizza'
    - U ziet dat dit is een niet-wait-actie zodat de bot naar de volgende actie vraagt.
6. Selecteer, wilt u iets anders?'
7. Voer 'paddestoelen verwijderen en pepers toevoegen'.
    - Kennisgeving **mushroom** heeft een '-' aanmelding ernaast om te worden verwijderd. En pepers heeft '+' aan toppings toe te voegen.
2. Klik op de actie Score.
    - Kennisgeving **pepers** worden nu in vet weergegeven zoals deze nieuw is. En **paddestoelen** is gepasseerd uit.
8. Schakel 'er $Toppings op uw pizza'
6. Selecteer, wilt u iets anders?'
7. Voer 'add erwten'.
    - Erwten zijn een voorbeeld van een toegenomen die buiten stock. Houd er rekening mee dat het nog steeds wordt aangeduid als een toegenomen.
2. Klik op de actie Score.
    - Erwten weergegeven als NietInVoorraad.
    - Om te zien hoe dit het geval, open laten we de code aan C:\<\installedpath > \src\demos\demoPizzaOrder.ts. En noteer de EntityDetectionCallback-methode. Deze methode wordt aangeroepen na elke toegenomen om te zien of deze in voorraad is. Zo niet, deze wordt deze uit de set toppings gewist en worden toegevoegd aan de entiteit NietInVoorraad. De variabele inStock is boven deze methode met de lijst met op voorraad toppings gedefinieerd.
6. Selecteer 'Er zijn momenteel geen $OutOfStock'.
7. Selecteer, wilt u iets anders?'
8. Voer 'no'.
9. Klik op de actie Score.
10. Selecteer 'FinalizeOrder' API-aanroep. 
    - Dit zal de functie 'FinalizeOrder' is gedefinieerd in code aanroepen. Dit toppings wist en uw bestelling is onderweg retourneert. 
2. Voer 'een andere volgorde'. We zijn een nieuwe order op te starten.
9. Klik op de actie Score.
    - Opmerking kaas en pepers zijn in het geheugen toppings van de laatste volgorde.
1. Selecteer, wilt u dat de $LastToppings'.
2. Voer 'yes'
3. Klik op de actie Score.
    - De bot wil de UseLastToppings actie te ondernemen. Dat is de seconde van de twee methoden voor de retouraanroep. De laatste order toppings kopiëren naar toppings worden en de laatste toppings wissen. Dit is een manier onthouden van de laatste volgorde, en als de gebruiker dat ze willen een andere pizza, die toppings als opties.
2. Klik op 'er $Toppings op uw pizza'.
3. Selecteer, wilt u iets anders?'
8. Voer 'no'.
4. Klik op het onderwijs gereed.

![](../media/tutorial_pizza_callbackcode.PNG)

![](../media/tutorial_pizza_apicalls.PNG)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Demo - VR-app starten](./demo-vr-app-launcher.md)
