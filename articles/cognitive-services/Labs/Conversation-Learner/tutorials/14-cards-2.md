---
title: Het gebruiken van kaarten met een toepassing conversatie cursist, deel 2 - cognitieve Microsoft-Services | Microsoft Docs
titleSuffix: Azure
description: Informatie over het gebruiken van kaarten met een conversatie cursist-toepassing.
services: cognitive-services
author: v-jaswel
manager: nolachar
ms.service: cognitive-services
ms.component: conversation-learner
ms.topic: article
ms.date: 04/30/2018
ms.author: v-jaswel
ms.openlocfilehash: 254f0953fd3e281a35857e69d9795e3decebf45d
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345268"
---
# <a name="how-to-use-cards-part-1-of-2"></a>Het gebruik van kaarten (deel 1 van 2)
Deze zelfstudie laat zien hoe u een kaart invulbaar formulier toevoegt aan uw bot. Deze wordt weergegeven hoe formuliervelden verplaatsen naar entiteiten.

Conversatie cursist verwacht uw definitiebestanden kaart zich bevinden in een map met de naam 'kaarten', die aanwezig is in de map waar de bot wordt gestart.

## <a name="requirements"></a>Vereisten
Deze zelfstudie vereist dat de algemene zelfstudie bot wordt uitgevoerd

    npm run tutorial-general

## <a name="details"></a>Details

Kaarten zijn UI-elementen waarmee de gebruiker een optie selecteren in de conversatie. 

### <a name="open-the-demo"></a>Open de demo

Klik in de lijst met Apps van de webgebruikersinterface, op de zelfstudie-14-kaarten-2. 

### <a name="the-card"></a>De kaart

De definitie van de kaart is op de volgende locatie: C:\<installedpath\>\src\cards\shippingAddress.json.

Deze kaart verzamelt drie velden van het verzendadres: straat, plaats en status.

![](../media/tutorial14_card.PNG)

### <a name="actions"></a>Acties

We hebt drie acties gemaakt. Zoals u hieronder ziet, is de eerste actie een kaart.

![](../media/tutorial14_actions.PNG)

Laten we zien hoe het kaarttype actie is gemaakt:

- U ziet straat waar het type van Input.text en bijbehorende-ID is.
- Er is op deze manier adres plaats en een vervolgkeuzelijst met de ID van de adres-status.

De id's zijn belangrijk omdat wanneer de velden zijn ingevuld en verzonden, worden de namen van entiteiten die deze waarden in de bot ontvangt.

## <a name="entities"></a>Entiteiten
Er zijn drie entiteiten die overeenkomt met de kaart, zoals u hebt gezien hierboven gedefinieerd.

![](../media/tutorial14_entities.PNG)

## <a name="actions"></a>Acties

Er zijn twee acties gedefinieerd.

![](../media/tutorial14_actions.PNG)

- De eerste is de back-upfunctie adreskaart waarbij het Type actie kaart en de sjabloon uit de vervolgkeuzelijst als shippingAddress is geselecteerd.
- De tweede is een eenvoudige actie lezen van het verzendadres.

![](../media/tutorial14_sa_card.PNG)

### <a name="train-dialog"></a>Dialoogvenster Train

We gaan een dialoogvenster onderwijs doorlopen.

1. Klik op de trein dialoogvensters, klikt u vervolgens nieuwe Train dialoogvenster.
1. Voer 'Hallo'.
2. Klik op de actie Score.
3. Klik op 'Verzendadres'.
4. Vul in de kaart en het verzenden.
    - U ziet dat nu de waarden in het geheugen van de entiteit zijn verplaatst. Geen parseren is nodig als het formulier gepartitioneerd al de invoer.
5. Klik op Score acties.
3. Klik op selecteren ' verzenden naar $Address... '.
4. Klik op gereed testen.

![](../media/tutorial14_train_dialog.PNG)

U hebt nu het ophalen van waarden van de kaart die invulbare velden en vervolgkeuzelijsten heeft, en vastleggen en ze te verzamelen in bot entiteiten gezien.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Vertakking en ongedaan maken](./15-branching-and-undo.md)
