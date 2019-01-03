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
ms.openlocfilehash: 9b35c0fd412dd48137a3cb362f20fae067c80461
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/27/2018
ms.locfileid: "53792625"
---
# <a name="demo-pizza-order"></a>Demo: Pizza volgorde
Deze demonstratie ziet u een bot, één pizza bestellen door ondersteuning te bestellen pizza:

- pizza toppings van gebruiker uitingen herkennen
- toppings inventaris beheren en erop reageren op de juiste wijze
- Vorige bestellingen onthouden en sneller opnieuw ordenen van een identieke pizza

## <a name="video"></a>Video

[![Demo Pizza-Preview](https://aka.ms/cl_Tutorial_v3_DemoPizzaOrder_Preview)](https://aka.ms/cl_Tutorial_v3_DemoPizzaOrder)

## <a name="requirements"></a>Vereisten
In deze zelfstudie is vereist dat de volgorde pizza bot wordt uitgevoerd

    npm run demo-pizza

### <a name="open-the-demo"></a>Open de demo

Klik in de lijst Model van de web-UI op TutorialDemo Pizza volgorde. 

## <a name="entities"></a>Entiteiten

Het model bevat drie entiteiten:

- "Toppings" stelt samen van de gebruiker opgegeven toppings, indien beschikbaar.
- 'NietInVoorraad' geeft de gebruiker dat geselecteerde BBC valt buiten het aandeel
- 'LastToppings' bevat de historische toppings van hun eerdere bestelling

![](../media/tutorial_pizza_entities.PNG)

### <a name="actions"></a>Acties

Het model bevat een set acties die van de gebruiker om de selectie BBC, samengevoegde toppings en meer vraagt.

Twee API-aanroepen worden ook geleverd:

- 'FinalizeOrder' verwerkt orders
- "UseLastToppings" verwerkt toppings historische gegevens

![](../media/tutorial_pizza_actions.PNG)

### <a name="training-dialogs"></a>Dialoogvensters voor training

Verschillende Training-dialoogvensters zijn gevonden in het Model.

![](../media/tutorial_pizza_dialogs.PNG)

Laten we trainen het Model iets meer door het maken van een ander dialoogvenster trainen.

1. In het linkerdeelvenster klikt u op "Train-dialoogvensters" en vervolgens de knop 'Nieuwe Train dialoogvenster'.
2. In het deelvenster chat, waarbij de status "Typ uw bericht …", typt u in 'Een pizza met kaas Order'
    - Het woord 'kaas' is door de entiteit-extractor uitgepakt.
3. Klik op de knop 'Acties Score'.
4. Selecteer het antwoord, "U hebt kaas op uw pizza."
5. Selecteer het antwoord, "Wilt u iets anders?"
6. In het deelvenster chat, waarbij de status "Typ uw bericht …", typt u 'paddestoelen en peppers toevoegen'
7. Klik op de knop 'Acties Score'.
8. Selecteer het antwoord, "U hebt kaas, paddestoelen en peppers op uw pizza."
9. Selecteer het antwoord, "Wilt u iets anders?"
10. In het deelvenster chat, waarbij de status "Typ uw bericht …", typt u 'peppers verwijderen en worst toevoegen'
11. Klik op de knop 'Acties Score'.
12. Selecteer het antwoord, "U hebt kaas, paddestoelen en worst op uw pizza."
13. Selecteer het antwoord, "Wilt u iets anders?"
14. In het deelvenster chat, waarbij de status "Typ uw bericht …", typt u 'yam toevoegen'
15. Klik op de knop 'Acties Score'.
    - De waarde 'yam' is toegevoegd aan 'NietInVoorraad' door de code van de callback in de entiteit detecteren, zoals de tekst komt niet overeen met alle ondersteunde onderdelen.
16. Selecteer het antwoord, "NietInVoorraad"
17. Selecteer het antwoord, "Wilt u iets anders?"
18. In het deelvenster chat, waarbij de status "Typ uw bericht …", typt u "Nee"
    - De "Nee" is niet gemarkeerd als een soort intentie. In plaats daarvan u we selecteert de relevante actie op basis van de huidige context.
19. Klik op de knop 'Acties Score'.
20. Selecteer het antwoord, "FinalizeOrder"
    - Selecteren van deze actie heeft geresulteerd in de huidige toppings van de klant door de code van de callback FinalizeOrder ophalen opgeslagen in het 'LastToppings' entiteit en verwijderen van de entiteit 'Toppings'.
21. In het deelvenster chat, waarbij de status "Typ uw bericht …", typt u in 'een andere order'
22. Klik op de knop 'Acties Score'.
23. Selecteer het antwoord, "Wilt u kaas, paddestoelen en worst?"
    - Deze actie is nu beschikbaar vanwege de 'LastToppings' entiteit wordt ingesteld.
24. In het deelvenster chat, waarbij de status "Typ uw bericht …", typt u "Ja"
25. Klik op de knop 'Acties Score'.
26. Selecteer het antwoord, "UseLastToppings"
27. Selecteer het antwoord, "U hebt kaas, paddestoelen en worst op uw pizza."
28. Selecteer het antwoord, "Wilt u iets anders?"

![](../media/tutorial_pizza_callbackcode.PNG)

![](../media/tutorial_pizza_apicalls.PNG)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Implementatie van een bot Conversatiecursist](../deploy-to-bf.md)
