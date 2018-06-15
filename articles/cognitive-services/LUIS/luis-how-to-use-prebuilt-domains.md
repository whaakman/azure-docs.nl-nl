---
title: Gebruik van vooraf gedefinieerde domeinen in LUIS apps in Azure | Microsoft Docs
description: Informatie over het gebruik van vooraf gedefinieerde domeinen in Language Understanding Intelligent Service (LUIS)-toepassingen.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 12/13/2017
ms.author: v-geberr
ms.openlocfilehash: 72ca0def8528adae5e46a02fa5bfccd14a3b6ab4
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "35345872"
---
# <a name="use-prebuilt-domains-in-luis-apps"></a>Vooraf gedefinieerde domeinen in LUIS apps gebruiken  

Language Understanding (LUIS) biedt *vooraf gedefinieerde domeinen*, die is een vooraf gedefinieerde set [intents](luis-how-to-add-intents.md) en [entiteiten](luis-concept-entity-types.md) die samen worden gebruikt voor domeinen of algemene categorieën clienttoepassingen. De vooraf gedefinieerde domeinen vooraf zijn getraind en u kunt toevoegen aan uw app LUIS. De intents en entiteiten in een vooraf gedefinieerde domein kunnen volledig worden aangepast wanneer u deze hebt toegevoegd aan uw app - u ze met utterances van uw systeem trainen kunt zodat ze voor uw gebruikers werken. U kunt een heel domein van de vooraf gedefinieerde als uitgangspunt gebruiken om aan te passen of lenen slechts een paar intents of entiteiten van een vooraf gedefinieerde domein. 

Bladeren de **vooraf gedefinieerde domeinen** tabblad om te zien van andere vooraf gemaakte domeinen die u in uw app kunt gebruiken. Klik op de tegel voor een domein toe te voegen aan uw app, of klik op **meer** in de tegel voor meer informatie over de intents en entiteiten.

> [!TIP]
> U vindt een volledige lijst van de vooraf gedefinieerde domeinen in de [vooraf gedefinieerde domeinen verwijzing](./luis-reference-prebuilt-domains.md).

![Vooraf gedefinieerde domein toevoegen](./media/luis-how-to-prebuilt-domain-entities/add-prebuilt-domain.png)


## <a name="add-a-prebuilt-domain"></a>Een vooraf gedefinieerde domein toevoegen
In de **vooraf gedefinieerde domeinen** tabblad, het domein RestaurantReservation vinden en klik op **domein toevoegen**. Zodra het vooraf gedefinieerde domein is toegevoegd aan uw app LUIS, opent u **Intents** en klik op de bedoeling RestaurantReservation.Reserve. U kunt zien dat veel voorbeeld utterances al zijn opgegeven en zijn gelabeld met entiteiten.

![De bedoeling RestaurantReservation.Reserve](./media/luis-how-to-prebuilt-domain-entities/prebuilt-domain-restaurant-reservation.png)


## <a name="designing-luis-apps-from-prebuilt-domains"></a>Apps van vooraf gedefinieerde domeinen LUIS ontwerpen
Wanneer u vooraf gedefinieerde domeinen in uw app LUIS, kunt u een heel domein van de vooraf gedefinieerde aanpassen of begint met een paar van de intents en entiteiten.

## <a name="customizing-an-entire-prebuilt-domain"></a>Een heel domein van de vooraf gedefinieerde aanpassen
Vooraf gedefinieerde domeinen zijn ontworpen om te algemeen. Ze bevatten veel intents en entiteiten, die u kiezen kunt uit voor het aanpassen van een app op uw behoeften. Als u start vanuit een heel domein van de vooraf gedefinieerde aanpassen, verwijdert u de intents en de entiteiten die uw app niet hoeft te gebruiken. U kunt ook enkele intents of entiteiten toevoegen aan de set die het vooraf gedefinieerde domein al biedt. Als u bijvoorbeeld de **gebeurtenissen** vooraf gemaakte domein voor een sport-app gebeurtenis, kunt u entiteiten voor sportteams toevoegen. Wanneer u start [utterances bieden](luis-how-to-add-example-utterances.md) naar LUIS, omvatten termen die specifiek voor uw app zijn. LUIS leert deze kunnen herkennen en aangepast, zodat deze het vooraf gedefinieerde domein intents en entiteiten aan de behoeften van uw app. 

> [!TIP]
> De intents en entiteiten in een vooraf gedefinieerde domein werken het beste samen. Het is beter combineren intents en entiteiten van hetzelfde domein indien mogelijk.
> * Er is een best practice gerelateerde intents gebruiken in hetzelfde domein. Als u aanpast bijvoorbeeld de `MakeReservation` opzet in de **plaatsen** domein, selecteer vervolgens de `Cancel` opzet van de **plaatsen** domein in plaats van de bedoeling annuleren in het **Gebeurtenissen** of **hulpprogramma's** domeinen.

## <a name="changing-the-behavior-of-a-prebuilt-domain-intent"></a>Het wijzigen van het gedrag van het doel van een vooraf gedefinieerde domein
Mogelijk vindt u een vooraf gedefinieerde domein bevat de opzet die vergelijkbaar is met de opzet die u in uw app LUIS wilt hebben, maar u wilt zich anders gedragen. Bijvoorbeeld, de **plaatsen** vooraf gemaakte domein biedt een `MakeReservation` bedoeld voor het maken van een restaurant-reservering, maar u wilt dat uw app te gebruiken die bedoeld om u te maken. In dat geval kunt u het gedrag van die intentie utterances verstrekken aan LUIS over hotel reserveringen maken en deze labels met behulp van de `MakeReservation` opzet, dus klik LUIS kan worden retrained herkent de `MakeReservation` opzet in een aanvraag bij een hotel boek .

> [!TIP]
> Bekijk het domein van de hulpprogramma's voor het vooraf gedefinieerde intents die u voor gebruik in een domein aanpassen kunt. U kunt bijvoorbeeld toevoegen `Utilities.Repeat` naar uw app en de trein herkennen elke gebruiker acties mogelijk wilt herhalen in uw toepassing.


## <a name="next-step"></a>Volgende stap

Een vooraf gedefinieerde domein aanpassen door meer voorbeeld utterances toe te voegen.

> [!div class="nextstepaction"]
> [Voorbeeld utterances toevoegen](./luis-how-to-add-example-utterances.md)

## <a name="additional-resources"></a>Aanvullende resources

Zie de [vooraf gedefinieerde domeinen verwijzing](./luis-reference-prebuilt-domains.md) voor meer informatie over de vooraf gedefinieerde domeinen.
