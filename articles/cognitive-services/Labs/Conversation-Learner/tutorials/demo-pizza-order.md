---
title: Conversation Learners model voor de demo, pizza order-Microsoft Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Meer informatie over het maken van een demo Conversation Learner model.
services: cognitive-services
author: nitinme
manager: nolachar
ms.service: cognitive-services
ms.subservice: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: 4d42121468b8a875ecd13fb936d810728bd4b644
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68703429"
---
# <a name="demo-pizza-order"></a>Demo: Pizza-volg orde
In deze demo ziet u een pizza-volg orde, die ondersteuning biedt voor één pizza-bestelling door:

- Pizza toppings herkennen aan de gebruiker uitingen
- toppings-inventaris beheren en op de juiste wijze reageren
- eerdere bestellingen onthouden en de volg orde van een identieke pizza verhogen

## <a name="video"></a>Video

[![Preview-versie van demo pizza](https://aka.ms/cl_Tutorial_v3_DemoPizzaOrder_Preview)](https://aka.ms/cl_Tutorial_v3_DemoPizzaOrder)

## <a name="requirements"></a>Vereisten
Voor deze zelf studie moet de bot van de pizza-order worden uitgevoerd

    npm run demo-pizza

### <a name="open-the-demo"></a>De demo openen

Klik in de lijst model van de Web-UI op TutorialDemo pizza order. 

## <a name="entities"></a>Entiteiten

Het model bevat drie entiteiten:

- "Toppings" verzamelt de opgegeven toppings van de gebruiker, indien beschikbaar.
- ' OutofStock ' geeft aan dat de gebruiker de geselecteerde topping niet op voor Raad is
- ' LastToppings ' bevat de historische toppings van de vorige volg orde

![](../media/tutorial_pizza_entities.PNG)

### <a name="actions"></a>Acties

Het model bevat een reeks acties waarmee de gebruiker wordt gevraagd om hun topping te selecteren, verzamelde toppings en meer.

Er worden ook twee API-aanroepen meegeleverd:

- ' FinalizeOrder ' verwerkt de uitvoering van de bestelling
- ' UseLastToppings ' verwerkt historische toppings-gegevens

![](../media/tutorial_pizza_actions.PNG)

### <a name="training-dialogs"></a>Trainings dialoogvensters

Er zijn verschillende trainings dialoogvensters gevonden in het model.

![](../media/tutorial_pizza_dialogs.PNG)

We gaan het model nog iets meer trainen door een ander Train-dialoog venster te maken.

1. Klik in het linkerdeel venster op ' dialoog vensters trainen ' en vervolgens op de knop Nieuw trainen dialoog venster.
2. In het deel venster chat, waar het bericht ' Typ uw boodschap... ', Typ in ' een pizza met kaas ' Best Ellen '
    - Het woord "kaas" is geëxtraheerd door het Extractor van de entiteit.
3. Klik op de knop ' Score acties '.
4. Selecteer de reactie "u hebt kaas op uw pizza."
5. Selecteer de reactie ' wilt u iets anders? '
6. In het deel venster chatten, waar het bericht ' Typ uw boodschap... ' typt u ' champignons en pepers toevoegen '.
7. Klik op de knop ' Score acties '.
8. Selecteer de reactie "u hebt kaas, champignons en pepers op uw pizza."
9. Selecteer de reactie ' wilt u iets anders? '
10. In het deel venster chat, waar het bericht ' Typ uw boodschap... ', typt u ' pepers verwijderen en worst toevoegen '
11. Klik op de knop ' Score acties '.
12. Selecteer de reactie "u hebt kaas, champignons en worst op uw pizza."
13. Selecteer de reactie ' wilt u iets anders? '
14. In het deel venster chatten, waar het bericht ' Typ uw boodschap... ' typt u ' Yam toevoegen '
15. Klik op de knop ' Score acties '.
    - De waarde ' Yam ' is toegevoegd aan ' OutofStock ' door de retour aanroep code voor de entiteits detectie, omdat de tekst niet overeenkomt met de ondersteunde ingrediënten.
16. Selecteer de reactie ' OutOfStock '
17. Selecteer de reactie ' wilt u iets anders? '
18. In het deel venster chatten, waar het bericht ' Typ uw boodschap... ' typt u ' nee '
    - De ' nee ' is niet gemarkeerd als een type intentie. In plaats daarvan selecteren we de relevante actie op basis van de huidige context.
19. Klik op de knop ' Score acties '.
20. Selecteer de reactie ' FinalizeOrder '
    - Als u deze actie selecteert, wordt de huidige toppings van de klant opgeslagen in de entiteit ' LastToppings ' en wordt de entiteit ' toppings ' verwijderd door de call back code van FinalizeOrder.
21. In het deel venster chatten, waar het bericht ' Typ uw boodschap... ' typt u ' een andere ' volg orde '
22. Klik op de knop ' Score acties '.
23. Selecteer het antwoord ' wilt u dat kaas, paddestoelen en worst? '.
    - Deze actie is nu beschikbaar omdat de entiteit ' LastToppings ' is ingesteld.
24. In het deel venster chat, waar het bericht ' Typ uw boodschap... ' typt u ' ja '
25. Klik op de knop ' Score acties '.
26. Selecteer de reactie ' UseLastToppings '
27. Selecteer de reactie "u hebt kaas, champignons en worst op uw pizza."
28. Selecteer de reactie ' wilt u iets anders? '

![](../media/tutorial_pizza_callbackcode.PNG)

![](../media/tutorial_pizza_apicalls.PNG)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een Conversation Learner-bot implementeren](../deploy-to-bf.md)
