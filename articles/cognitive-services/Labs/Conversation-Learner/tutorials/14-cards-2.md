---
title: 'Over het gebruiken van kaarten met een model Conversatiecursist, deel 2: Microsoft Cognitive Services | Microsoft Docs'
titleSuffix: Azure
description: Informatie over het gebruiken van kaarten met een Conversatiecursist-model.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 1c7c88742c69041594006add76f7e3c642c64dec
ms.sourcegitcommit: 4e5ac8a7fc5c17af68372f4597573210867d05df
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/20/2018
ms.locfileid: "39170569"
---
# <a name="how-to-use-cards-part-1-of-2"></a>Het gebruik van kaarten (deel 1 van 2)
Deze zelfstudie laat zien hoe u een kaart invulbaar formulier toevoegen aan uw bot. U ziet hoe velden in het formulier in op entiteiten verplaatsen.

Conversatiecursist wordt verwacht dat de kaart definitiebestanden zich bevinden in een map genaamd 'kaarten', die is aanwezig in de map waar de bot wordt gestart.

## <a name="video"></a>Video

[![Zelfstudie 14-Preview](http://aka.ms/cl-tutorial-14-preview)](http://aka.ms/blis-tutorial-14)

## <a name="requirements"></a>Vereisten
In deze zelfstudie is vereist dat de algemene zelfstudie bot wordt uitgevoerd

    npm run tutorial-general

## <a name="details"></a>Details

Kaarten zijn UI-elementen waarmee de gebruiker een optie te selecteren in de conversatie. 

### <a name="open-the-demo"></a>Open de demo

Klik in de lijst Model van de web-UI op zelfstudie-14-kaarten-2. 

### <a name="the-card"></a>De kaart

De definitie van de kaart is op de volgende locatie: C:\<installedpath\>\src\cards\shippingAddress.json.

Deze kaart verzamelt drie velden van het verzendadres: straat, plaats en staat.

![](../media/tutorial14_card.PNG)

### <a name="actions"></a>Acties

We hebben drie acties gemaakt. Zoals u hieronder ziet, is de eerste actie in een kaart.

![](../media/tutorial14_actions.PNG)

We gaan nu kijken hoe het actietype kaart wordt gemaakt op:

- U ziet de straat waar het type van het Input.text en de ID is.
- Er is op deze manier adres plaats en een vervolgkeuzelijst met ID van de adres-status.

De id's zijn belangrijk omdat als de velden zijn ingevuld en verzonden, worden de namen van entiteiten die deze waarden in de bot ontvangt.

## <a name="entities"></a>Entiteiten
We hebben drie entiteiten die overeenkomen met de kaart zoals we hierboven al hebben gezien gedefinieerd.

![](../media/tutorial14_entities.PNG)

## <a name="actions"></a>Acties

We hebben hebt twee acties gedefinieerd.

![](../media/tutorial14_actions.PNG)

- De eerste is de verzending adreskaart waarbij het actietype kaart en sjabloon wordt geselecteerd in de vervolgkeuzelijst als shippingAddress.
- De tweede is een eenvoudige actie om te lezen terug het verzendadres.

![](../media/tutorial14_sa_card.PNG)

### <a name="train-dialog"></a>Dialoogvenster van de trein

We nemen een dialoogvenster voor onderwijs.

1. Klik op de trein dialoogvensters, en vervolgens Nieuw dialoogvenster van de trein.
1. Voer 'Hallo'.
2. Klik op Score actie.
3. Klik op 'Verzendadres'.
4. Vul in de kaart en het verzenden.
    - U ziet dat nu de waarden in het geheugen van de entiteit zijn verplaatst. Geen parseren is nodig omdat het formulier gepartitioneerd al de invoer.
5. Klik op Score acties.
3. Klik op selecteren ' verzenden naar $Address... '.
4. Klik op gereed testen.

![](../media/tutorial14_train_dialog.PNG)

U hebt nu gezien hoe om waarden te verkrijgen van de kaart met invulbare velden en vervolgkeuzelijsten, en voor het vastleggen en ze in bot entiteiten te verzamelen.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Vertakkingen en ongedaan maken](./15-branching-and-undo.md)
