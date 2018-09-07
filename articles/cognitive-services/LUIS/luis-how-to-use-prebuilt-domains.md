---
title: Vooraf gemaakte domeinen gebruiken voor het bouwen van apps sneller in LUIS-apps
titleSuffix: Azure Cognitive Services
description: Language Understanding (LUIS) biedt vooraf gemaakte domeinen die zijn vooraf gedefinieerde sets intenties en entiteiten die voor domeinen samenwerken of algemene categorieën van clienttoepassingen. De vooraf gemaakte domeinen vooraf is getraind en gereed zijn voor u om toe te voegen aan uw LUIS-app.
services: cognitive-services
author: diberry
manager: cjgronlund
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 09/06/2018
ms.author: diberry
ms.openlocfilehash: 80a83fdb1e2b84ac03d0ae0cf969f1635f00cca0
ms.sourcegitcommit: ebd06cee3e78674ba9e6764ddc889fc5948060c4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44053720"
---
# <a name="use-prebuilt-domains-in-luis-apps"></a>Vooraf gemaakte domeinen in LUIS-apps gebruiken  

Language Understanding (LUIS) biedt *vooraf gemaakte domeinen*, die zijn vooraf gedefinieerde sets [intents](luis-how-to-add-intents.md) en [entiteiten](luis-concept-entity-types.md) die samen worden gebruikt voor domeinen of algemene categorieën clienttoepassingen. De vooraf gemaakte domeinen vooraf is getraind en gereed zijn voor u om toe te voegen aan uw LUIS-app. De intenties en entiteiten in een vooraf gedefinieerde domein kunnen volledig worden aangepast wanneer u ze hebt toegevoegd aan uw app - u ze met uitingen van uw systeem trainen kunt, zodat ze voor uw gebruikers werken. U kunt een volledig domein van de vooraf gedefinieerde gebruiken als uitgangspunt voor aanpassing of lenen slechts een paar intents of entiteiten op basis van een vooraf gedefinieerde domein. 

Ga de **vooraf gemaakte domeinen** tabblad om te bekijken van de andere vooraf gemaakte domeinen die u in uw app gebruiken kunt. Klik op de tegel voor een domein toe te voegen aan uw app, of klik op **meer** in de tegel voor meer informatie over de intenties en entiteiten.

> [!TIP]
> U vindt een volledige lijst van de vooraf gemaakte domeinen in de [vooraf gemaakte domeinen verwijzing](./luis-reference-prebuilt-domains.md).

![Vooraf gedefinieerde domein toevoegen](./media/luis-how-to-prebuilt-domain-entities/add-prebuilt-domain.png)


## <a name="add-a-prebuilt-domain"></a>Een vooraf gedefinieerd domein toevoegen
In de **vooraf gemaakte domeinen** tabblad, het domein RestaurantReservation vinden en op **domein toevoegen**. Nadat het vooraf gedefinieerde domein is toegevoegd aan uw LUIS-app, opent u **Intents** en klik op het doel RestaurantReservation.Reserve. U ziet dat veel voorbeeld uitingen al zijn opgegeven en met het label met entiteiten.

![RestaurantReservation.Reserve doel](./media/luis-how-to-prebuilt-domain-entities/prebuilt-domain-restaurant-reservation.png)


## <a name="designing-luis-apps-from-prebuilt-domains"></a>Het ontwerpen van LUIS-apps uit vooraf gemaakte domeinen
Wanneer u vooraf gemaakte domeinen in uw LUIS-app, kunt u een volledig domein van de vooraf gedefinieerde aanpassen of begin met een paar van de intenties en entiteiten.

## <a name="customizing-an-entire-prebuilt-domain"></a>Een volledig domein van de vooraf gedefinieerde aanpassen
Vooraf gemaakte domeinen zijn ontworpen om te worden algemene. Ze bevatten veel intenties en entiteiten, die u kiezen kunt uit een app aan uw behoeften aanpassen. Als u vanaf het aanpassen van een volledig domein van de vooraf gedefinieerde start, verwijdert u de intenties en entiteiten die uw app niet hoeven te gebruiken. U kunt ook enkele intents of entiteiten toevoegen aan de set waarmee het bestaande domein al. Als u bijvoorbeeld de **gebeurtenissen** vooraf gemaakte domein voor een app van de gebeurtenis sport, kunt u om toe te voegen entiteiten voor sportteams. Wanneer u begint met [bieden uitingen](luis-how-to-add-example-utterances.md) naar LUIS, omvatten voorwaarden die specifiek voor uw app zijn. LUIS leert herkennen en aangepast, zodat het bestaande domein intenties en entiteiten aan de behoeften van uw app. 

> [!TIP]
> De intenties en entiteiten in een vooraf gedefinieerde domein werken het beste samen. Is het beter om intenties en entiteiten op basis van hetzelfde domein indien mogelijk te combineren.
> * Een best practice is het gebruik van gerelateerde intents van hetzelfde domein bevinden. Bijvoorbeeld, als het aanpassen van de `MakeReservation` intentie in de **plaatsen** domein, selecteer vervolgens de `Cancel` intentie van de **plaatsen** domein in plaats van de intentie annuleren in de **Gebeurtenissen** of **hulpprogramma's voor** domeinen.

## <a name="changing-the-behavior-of-a-prebuilt-domain-intent"></a>Het gedrag van de intentie van een vooraf gedefinieerde domein wijzigen
Mogelijk vindt u een vooraf gedefinieerde domein bevat een intent die vergelijkbaar is met een doel dat u in uw LUIS-app wilt hebben, maar u wilt zich anders gedragen. Bijvoorbeeld, de **plaatsen** vooraf gemaakte domein biedt een `MakeReservation` doel voor het maken van een reservering restaurant, maar u wilt dat uw app maken met dit doel. In dat geval kunt u het gedrag van dit doel wijzigen door uitingen leveren aan LUIS over het maken van reserveringen hotel en labelen deze met behulp van de `MakeReservation` intentie, dus klik LUIS kan opnieuw worden getraind om te herkennen de `MakeReservation` intentie in een aanvraag bij een hotel boek .

> [!TIP]
> Kijk eens naar het domein van de hulpprogramma's voor vooraf gedefinieerde intents die u voor gebruik in elk domein aanpassen kunt. Bijvoorbeeld, u kunt toevoegen `Utilities.Repeat` aan uw app en train herkennen elke gebruiker acties mogelijk wilt herhalen in uw toepassing.


## <a name="next-step"></a>Volgende stap

Een vooraf gedefinieerde domein aanpassen door meer voorbeeld uitingen toe te voegen.

> [!div class="nextstepaction"]
> [Voorbeeld utterances toevoegen](./luis-how-to-add-example-utterances.md)

## <a name="additional-resources"></a>Aanvullende resources

Zie de [vooraf gemaakte domeinen verwijzing](./luis-reference-prebuilt-domains.md) voor meer informatie over de vooraf gemaakte domeinen.
