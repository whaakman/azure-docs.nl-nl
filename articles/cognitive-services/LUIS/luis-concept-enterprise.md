---
title: Enterprise-concepten voor een app LUIS - Azure | Microsoft Docs
description: Ontwerpconcepten voor grote LUIS apps begrijpen.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 06/05/2018
ms.author: v-geberr
ms.openlocfilehash: dde7012dee0eb5ea3ac2e1257cb8d2fca5843d4b
ms.sourcegitcommit: 6f6d073930203ec977f5c283358a19a2f39872af
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/11/2018
ms.locfileid: "35350112"
---
# <a name="enterprise-strategies-for-a-luis-app"></a>De strategie van de onderneming voor een app LUIS
Bekijk deze ontwerpstrategieën voor uw app enterprise.

## <a name="when-you-expect-luis-requests-beyond-the-quota"></a>Wanneer u verwacht dat LUIS aanvragen boven het quotum
Als uw app LUIS percentage de toegestane overschrijdt [quotum snelheid](https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/), spreiden van de belasting op meer LUIS apps met de [dezelfde app definitie](#use-multiple-apps-with-same-app-definition) of maak en [meerdere sleutels toewijzen](#assign-multiple-luis-keys-to-same-app) naar de App. 

### <a name="use-multiple-apps-with-same-app-definition"></a>Meerdere apps gebruiken met dezelfde definitie van de app
De oorspronkelijke LUIS app exporteren en importeren van de app in afzonderlijke apps. Elke app heeft een eigen app-ID. Wanneer u publiceert, in plaats van met dezelfde sleutel voor alle apps, maakt u een afzonderlijke sleutel voor elke app. Verdelen over alle apps zodat geen enkele app wordt overbelast. Voeg [Application Insights](luis-tutorial-bot-csharp-appinsights.md) om te controleren van het gebruik. 

Om te kunnen ophalen van de dezelfde bovenste bedoeling tussen alle apps, zorg ervoor dat de opzet voorspelling tussen het eerste en tweede doel is te groot genoeg dat LUIS niet verward is, die verschillende resultaten oplevert tussen apps voor kleine verschillen in utterances. 

Een enkele app aanwijzen als het model. Eventuele utterances die worden voorgesteld voor controle moeten worden toegevoegd aan de master-app vervolgens teruggeplaatst in alle andere apps. Dit is een volledige export van de app, of de gelabelde utterances laden uit het model naar de onderliggende elementen. Bij het laden kan worden gedaan vanuit de [LUIS] [ LUIS] website of de authoring API voor een [eenmalige utterance](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c08) of voor een [batch](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c09). 

Plan een periodieke [beoordeling van de endpoint-utterances](label-suggested-utterances.md) voor actieve learning, zoals elke twee weken vervolgens opnieuw trainen en opnieuw publiceren. 

### <a name="assign-multiple-luis-keys-to-same-app"></a>Meerdere LUIS sleutels aan dezelfde app toewijzen
Als uw app LUIS meer eindpunt treffers dan uw één sleutel quota worden toegestaan bij het maken en toewijzen van meer sleutels naar de app LUIS ontvangt. Maken van een traffic manager of load balancer voor het beheren van de endpoint-query's via de abonnement-sleutels. 

## <a name="when-your-monolithic-app-returns-wrong-intent"></a>Wanneer uw app monolithische verkeerde bedoelingen retourneert
Als uw app bestemd is voor het voorspellen van een groot aantal gebruiker utterances, Overweeg de implementatie van de [dispatch model](#dispatch-tool-and-model). Focus detectie tussen intents is in plaats van het ophalen van verward tussen intents via de app bovenliggende en onderliggende apps kunt op te splitsen van een app monolithische LUIS. 

Plan een periodieke [beoordeling van de endpoint-utterances](label-suggested-utterances.md) voor actieve learning, zoals elke twee weken vervolgens opnieuw trainen en opnieuw publiceren. 

## <a name="when-you-need-to-have-more-than-500-intents"></a>Als u meer dan 500 intents wilt
Stel dat u een office-assistent met meer dan 500 intents ontwikkelt. Als 200 intents verwant zijn aan de vergaderingen plannen, 200 zijn herinneringen, 200 zijn over het ophalen van informatie over collega's, en 200 zijn voor het verzenden van e-mail, groep intents zodat elke groep in een enkele app vervolgens een op het hoogste niveau app maken met elke doel. Gebruik de [verzending van hulpprogramma's en architectuur](#dispatch-tool-and-model) om de app op het hoogste niveau te bouwen. Wijzig uw bot voor het gebruik van de aanroep van trapsgewijze als weergeven in de [dispatch zelfstudie][dispatcher-application-tutorial]. 

## <a name="when-you-need-to-combine-several-luis-and-qna-maker-apps"></a>Wanneer u moet verschillende LUIS en QnA maker apps combineren
Als u hebt verschillende LUIS en QnA maker-apps die u moeten reageren op een bot, gebruik de [dispatch hulpprogramma](#dispatch-tool-and-model) om de app op het hoogste niveau te bouwen. Wijzig uw bot voor het gebruik van de aanroep van trapsgewijze als weergeven in de [dispatch zelfstudie][dispatcher-application-tutorial]. 

## <a name="dispatch-tool-and-model"></a>Hulpprogramma voor verzending en het model
Gebruik de [verzending] [ dispatch-tool] opdrachtregelprogramma gevonden in [BotBuilder-tools](https://github.com/Microsoft/botbuilder-tools) meerdere LUIS en/of QnA Maker apps combineren in een bovenliggende LUIS app. Deze aanpak kunt u een bovenliggend domein waaronder alle onderwerpen en andere onderliggende onderwerpsdomeinen in afzonderlijke apps hebben. 

![Conceptuele afbeelding van dispatch-architectuur](./media/luis-concept-enterprise/dispatch-architecture.png)

Het bovenliggende domein wordt vermeld in LUIS als een **V Dispatch** app. 

![Schermopname van LUIS apps lijst met LUIS app gemaakt door het hulpprogramma voor verzending](./media/luis-concept-enterprise/dispatch.png)

De chatbot ontvangt de utterance vervolgens verzendt naar de bovenliggende LUIS app voor de prognose. De bovenste voorspelde intentie van de bovenliggende app bepaalt welke onderliggende LUIS app naast wordt aangeroepen. De utterance verzendt de chatbot naar de onderliggende-app voor een meer specifiek voorspelling.

Begrijpen hoe deze hiërarchie van aanroepen worden gesteld van het v4-Bot Builder [dispatcher-toepassing-tutorial][dispatcher-application-tutorial].  

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over hoe [testen van een batch](luis-how-to-batch-test.md)

[LUIS]:luis-reference-regions.md
[dispatcher-application-tutorial]:https://aka.ms/bot-dispatch
[dispatch-tool]:https://github.com/Microsoft/botbuilder-tools/tree/master/Dispatch