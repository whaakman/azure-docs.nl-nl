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
ms.openlocfilehash: fea950e2c13d9b5ce0c3619990961e611edd6626
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55207374"
---
# <a name="how-to-use-negatable-entities-with-a-conversation-learner-model"></a>Over het gebruik van Negatable entiteiten met een Model van de cursist gesprek

In deze zelfstudie ziet u de eigenschap "Negatable" van entiteiten.

## <a name="video"></a>Video

[![Zelfstudie negatable entiteiten-Preview](https://aka.ms/cl_Tutorial_v3_NegatableEntities_Preview)](https://aka.ms/cl_Tutorial_v3_NegatableEntities)

## <a name="requirements"></a>Vereisten
In deze zelfstudie is vereist dat de algemene zelfstudie bot wordt uitgevoerd

    npm run tutorial-general

## <a name="details"></a>Details
De eigenschap 'Negatable' van een entiteit kunt u beide normaal (positief) een label en negatieve exemplaren van de entiteit, leren op basis van positieve en negatieve modellen en schakelt u de waarde van een bestaande entiteit. Entiteiten met een set van de eigenschap 'Negatable' worden Negatable entiteiten in de conversatie Leaner genoemd.

## <a name="steps"></a>Stappen

### <a name="create-the-model"></a>Het Model maken

1. In de Web-gebruikersinterface, klikt u op "Nieuwe Model."
2. Typ in het veld 'Name', 'NegatableEntity' en druk op enter.
3. Klik op de knop 'Maken'.

### <a name="entity-creation"></a>Entiteit maken

1. In het linkerdeelvenster klikt u op "Entiteiten" en vervolgens de knop 'Nieuwe entiteit'.
2. Selecteer 'Aangepast' voor het "entiteitstype'.
3. Typ 'naam' voor "Naam van de entiteit."
4. Schakel het selectievakje 'Negatable'.
    - Controle van deze eigenschap kan de gebruiker de entiteitswaarde van een opgeven, of Stel dat er iets is *niet* de entiteitswaarde van een. In dat geval wordt is het resultaat van het verwijderen van de overeenkomende entiteitswaarde.
5. Klik op de knop 'Maken'.

![](../media/tutorial5_entities.PNG)

### <a name="create-the-first-action"></a>De eerste actie maken

1. Klik op 'Acties' en vervolgens de knop 'Nieuwe actie' in het linkerdeelvenster.
2. In de 'van de Bot response...' veld, typt u "Ik weet niet de naam van uw."
3. Typ in het veld "Diskwalificeren voorziet", "naam".
4. Klik op de knop 'Maken'.

### <a name="create-the-second-action"></a>De tweede actie maken

1. Klik op 'Acties' en vervolgens de knop 'Nieuwe actie' in het linkerdeelvenster.
2. In de 'van de Bot response...' veld, typt u 'Ik weet dat uw naam. Het is $name."
3. Klik op de knop 'Maken'.

> [!NOTE]
> De entiteit 'naam' automatisch is toegevoegd als een 'vereist entiteiten' verwezen in het antwoord.

U hebt nu twee acties.

![](../media/tutorial5_actions.PNG)

### <a name="train-the-model"></a>Het Model te trainen

1. In het linkerdeelvenster klikt u op "Train-dialoogvensters" en vervolgens de knop 'Nieuwe Train dialoogvenster'.
2. In het deelvenster chat, waarbij de status "Typ uw bericht …", typt u "Hallo".
3. Klik op de knop 'Acties Score'.
4. Selecteer het antwoord, "Ik weet niet de naam van uw."
    - Het percentiel is 100%, omdat de enige geldige actie op basis van de beperkingen.
5. In het deelvenster chat, waarbij de status "Typ uw bericht …", typt u "Mijn naam is Frank"
6. Selecteer 'Frank', en kies het label "+ naam"
    - Er zijn twee exemplaren voor de entiteit "naam": '+ naam' en '-naam '.  (+) Plus toegevoegd of de waarde wordt overschreven. (-) Min Hiermee verwijdert u de waarde.
7. Klik op de knop 'Acties Score'.
    - Nu de 'naam' entiteit gedefinieerd als "Jaap" in het geheugen van het Model, dus de 'Ik weet dat uw naam. Het is $name"-actie is beschikbaar.
8. Selecteer het antwoord, "Ik weet dat uw naam. Het is $name."
9. In het deelvenster chat, waarbij de status "Typ uw bericht …", typ "Mijn naam is niet Frank."
10. Selecteer 'Frank' en kies het label '-name "
    - Selecteert u '-naam ' de huidige waarde van de entiteit te wissen.
11. Klik op de knop 'Acties Score'.
12. Selecteer het antwoord, "Ik weet niet de naam van uw."
13. In het deelvenster chat, waarbij de status "Typ uw bericht …", typt u "Mijn naam is Susan."
14. Selecteer 'Susan', en kies het label "+ naam"

![](../media/tutorial5_dialogs.PNG)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Entiteiten van meerdere waarden](./07-multi-value-entities.md)
