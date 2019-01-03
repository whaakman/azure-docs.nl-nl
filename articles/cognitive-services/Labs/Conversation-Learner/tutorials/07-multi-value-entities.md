---
title: Over het gebruik van meerdere waarden entiteiten met een Model van de cursist conversatie - Microsoft Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Informatie over het gebruik van meerdere waarden entiteiten met een Model van de cursist conversatie.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 2363dde7dc2462adde730fa9a4883ffb6c558f3f
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/27/2018
ms.locfileid: "53796738"
---
# <a name="how-to-use-multi-value-entities-with-a-conversation-learner-model"></a>Over het gebruik van meerdere waarden entiteiten met een model Conversatiecursist
In deze zelfstudie leert de eigenschap met meerdere waarden van entiteiten.

## <a name="video"></a>Video

[![Meerdere waarden entiteiten zelfstudie Preview](https://aka.ms/cl_Tutorial_v3_MultiValued_Preview)](https://aka.ms/cl_Tutorial_v3_MultiValued)

## <a name="requirements"></a>Vereisten
In deze zelfstudie is vereist dat de algemene zelfstudie bot wordt uitgevoerd

    npm run tutorial-general

## <a name="details"></a>Details
Meerdere waarden entiteiten oplopen waarden in een lijst, in plaats van een enkele waarde opslaan.  Deze entiteiten zijn nuttig wanneer gebruikers meer dan één waarde kunnen opgeven. Toppings op een pizza bijvoorbeeld.

Entiteiten die zijn gemarkeerd als meerdere waarden elk herkende exemplaar van de entiteit toegevoegd aan een lijst in het geheugen van de Bot heeft. Volgende erkenning toegevoegd aan van de entiteit-waarde, in plaats van worden overschreven.

## <a name="steps"></a>Stappen

### <a name="create-the-model"></a>Het Model maken

1. In de Web-gebruikersinterface, klikt u op "Nieuwe Model."
2. Typ in het veld 'Name', 'MultiValueEntities' en druk op enter.
3. Klik op de knop 'Maken'.

### <a name="entity-creation"></a>Entiteit maken

1. In het linkerdeelvenster klikt u op "Entiteiten" en vervolgens de knop 'Nieuwe entiteit'.
2. Selecteer 'Aangepaste getraind' voor het "entiteitstype'.
3. Typ 'toppings' voor "Naam van de entiteit."
4. Het selectievakje 'Meerdere waarden'.
    - Entiteiten van meerdere waarden worden verzameld van een of meer waarden in de entiteit.
5. Schakel het selectievakje 'Negatable'.
    - De eigenschap 'Negatable' is in een andere zelfstudie besproken.
6. Klik op de knop 'Maken'.

![](../media/tutorial6_entities.PNG)

### <a name="create-the-first-action"></a>De eerste actie maken

1. Klik op 'Acties' en vervolgens de knop 'Nieuwe actie' in het linkerdeelvenster.
2. In de 'van de Bot response...' veld, typt u "Hier worden de toppings: $toppings"
    - De toonaangevende dollarteken geeft aan dat de verwijzing naar een entiteit
3. Klik op de knop 'Maken'.

### <a name="create-the-second-action"></a>De tweede actie maken

1. Klik op 'Acties' en vervolgens de knop 'Nieuwe actie' in het linkerdeelvenster.
2. In de 'van de Bot response...' veld, typt u 'welke toppings wilt u dat?'
3. Typ in het veld "Diskwalificeren voorziet", "toppings."
4. Klik op de knop 'Maken'.

U hebt nu twee acties.

![](../media/tutorial6_actions.PNG)

### <a name="train-the-model"></a>Het Model te trainen

1. In het linkerdeelvenster klikt u op "Train-dialoogvensters" en vervolgens de knop 'Nieuwe Train dialoogvenster'.
2. In het deelvenster chat, waarbij de status "Typ uw bericht …", typt u 'Hallo.'
3. Klik op de knop 'Acties Score'.
4. Selecteer het antwoord, "welke toppings wilt u?"
    - Het percentiel is 100%, omdat de enige geldige actie op basis van de beperkingen.
5. In het deelvenster chat, waarbij de status "Typ uw bericht …", typt u 'kaas en paddestoelen'
6. Klik op 'kaas' en kies het label "+ toppings"
7. Klik op 'paddestoelen' en kies het label "+ toppings"
8. Klik op de knop 'Acties Score'.
9. Selecteer het antwoord "Hier worden de toppings: $toppings"
10. In het deelvenster chat, waarbij de status "Typ uw bericht …", typt u 'peper toevoegen'
11. Klik op 'peper' en kies het label "+ toppings"
12. Klik op de knop 'Acties Score'.
13. Selecteer het antwoord "Hier worden de toppings: $toppings"
14. In het deelvenster chat, waarbij de status "Typ uw bericht …", typt u "kaas verwijderen"
15. Klik op 'kaas' en kies het label '-toppings "
16. Klik op de knop 'Acties Score'.
17. Selecteer het antwoord "Hier worden de toppings: $toppings"

![](../media/tutorial5_dialogs.PNG)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Vooraf getrainde entiteiten](./08-pre-trained-entities.md)
