---
title: Over het gebruik van entiteiten met een Model van de cursist conversatie - Microsoft Cognitive Services | Microsoft Docs
titleSuffix: Azure
description: Informatie over het gebruik van entiteiten met een Model van de cursist conversatie.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 4239cbfe369060516653145b04a2bcfa0882f20d
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/27/2018
ms.locfileid: "53796819"
---
# <a name="introduction-to-entities"></a>Inleiding tot entiteiten

Deze zelfstudie maakt u kennis entiteiten, diskwalificeren entiteiten, vereist entiteiten en hun gebruik binnen Conversatiecursist.

## <a name="video"></a>Video

[![Inleiding tot entiteiten zelfstudie Preview](https://aka.ms/cl_Tutorial_v3_IntroEntities_Preview)](https://aka.ms/cl_Tutorial_v3_IntroEntities)

## <a name="requirements"></a>Vereisten

In deze zelfstudie is vereist dat de algemene zelfstudie Bot wordt uitgevoerd

    npm run tutorial-general

## <a name="details"></a>Details

Entiteiten vastleggen de stukjes informatie die de Bot moet de taak, of via de extractie van uitingen van de gebruiker of de toewijzing van aangepaste code uit te voeren. Entiteiten zelf kunnen ook actie beschikbaarheid beperken door expliciet worden geclassificeerd als 'Vereist' of "Diskwalificeren."

- Vereiste entiteiten moeten aanwezig zijn in het geheugen van het Model in volgorde voor de actie moet beschikbaar zijn
- Entiteiten diskwalificeren moet *niet* aanwezig zijn in het geheugen van het Model in volgorde voor de actie beschikbaar

In deze zelfstudie is gericht op aangepaste entiteiten. Vooraf getrainde, zijn meerdere waarden, Negatable entiteiten en programmatische entiteiten geïntroduceerd in andere zelfstudies.

## <a name="steps"></a>Stappen

### <a name="create-the-model"></a>Het Model maken

1. In de Web-gebruikersinterface, klikt u op "Nieuwe Model."
2. Typ in het veld 'Name', 'IntroToEntities' en druk op enter.
3. Klik op de knop 'Maken'.

### <a name="entity-creation"></a>Entiteit maken

1. In het linkerdeelvenster klikt u op "Entiteiten" en vervolgens de knop 'Nieuwe entiteit'.
2. Selecteer 'Aangepaste getraind' voor het "entiteitstype'.
3. Typ 'plaats' voor "Naam van de entiteit."
4. Klik op de knop 'Maken'.

> [!NOTE]
> Het aangepaste getrainde entiteitstype betekent dat deze entiteit kan worden getraind, in tegenstelling tot andere typen entiteiten.

### <a name="create-the-actions"></a>De acties maken

1. Klik op 'Acties' en vervolgens de knop 'Nieuwe actie' in het linkerdeelvenster.
2. In de 'van de Bot response...' veld, typt u "Ik weet niet welke plaats die u wilt."
3. Typ in het veld 'Entiteiten diskwalificeren', 'plaats'.
4. Klik op de knop 'Maken'.

> [!NOTE]
> De entiteit 'plaats' aan 'Entiteiten diskwalificeren' toe te voegen zou deze actie van de Bot rekening gehouden met diskwalificeren als 'plaats' entiteit is gedefinieerd in het geheugen van de Bot.

Maak nu een tweede actie.

1. Klik op 'Acties' en vervolgens de knop 'Nieuwe actie' in het linkerdeelvenster.
2. In de 'van de Bot response...' veld, typt u "het weer in de $city is waarschijnlijk zonnige."
3. Klik op de knop 'Maken'.

> [!NOTE]
> De entiteit 'plaats' automatisch is toegevoegd in de lijst met entiteiten vereist op basis van de reactie.

![](../media/tutorial3_actions.PNG)

### <a name="train-the-model"></a>Het Model te trainen

1. In het linkerdeelvenster klikt u op "Train-dialoogvensters" en vervolgens de knop 'Nieuwe Train dialoogvenster'.
2. In het deelvenster chat, waarbij de status "Typ uw bericht …", typt u "Hallo".
    - Dit simuleert de kant van de gebruiker van de conversatie.
3. Klik op de knop 'Acties Score'.
4. Selecteer het antwoord, "Ik weet niet welke plaats die u wilt."
5. Als de gebruiker reageren met "Seattle".
6. Klik op de knop 'Acties Score'.
7. Selecteer het antwoord, "Weer in $city is waarschijnlijk zonnige."
8. Klik op de knop 'Opslaan'.

![](../media/tutorial3_entities.PNG)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Verwachte entiteit](./05-expected-entity.md)
