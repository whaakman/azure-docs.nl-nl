---
title: App publiceren
titleSuffix: Azure Cognitive Services
description: Wanneer u klaar bent met het ontwikkelen en testen van uw actieve LUIS-app, het beschikbaar stellen aan uw clienttoepassing door deze te publiceren naar het eindpunt.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: conceptual
ms.date: 01/08/2019
ms.author: diberry
ms.openlocfilehash: 241227270aeede93b74d3d94c782dfe7eaf1fbf2
ms.sourcegitcommit: 818d3e89821d101406c3fe68e0e6efa8907072e7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2019
ms.locfileid: "54121699"
---
# <a name="publish-your-active-trained-app-to-a-staging-or-production-endpoint"></a>Uw app actief, getrainde publiceren naar een fasering of productie-eindpunt

Wanneer u klaar bent met het ontwikkelen en testen van uw actieve LUIS-app, het beschikbaar stellen aan uw clienttoepassing door deze te publiceren naar het eindpunt. 

<a name="publish-your-trained-app-to-an-http-endpoint"></a>

## <a name="publishing"></a>Publiceren

Als u wilt publiceren naar het eindpunt, selecteer **publiceren** rechts in het bovenste deelvenster. 

![Bovenste, rechts navigatiebalk](./media/luis-how-to-publish-app/publish-top-nav-bar.png)

Selecteer de juiste site wanneer het pop-upvenster wordt weergegeven: fasering of productie. Met behulp van twee publishing sleuven, kunt hiermee u twee verschillende versies met gepubliceerde eindpunten of dezelfde versie hebben op twee verschillende eindpunten. 

De app wordt gepubliceerd naar alle regio's die zijn gekoppeld aan de LUIS-resources in de portal LUIS toegevoegd. Bijvoorbeeld, voor een app gemaakt op [www.luis.ai](https://www.luis.ai), als u een LUIS-resource in **westus** en toe te voegen aan de app als een resource, de app wordt gepubliceerd in die regio. Zie voor meer informatie over regio's voor LUIS [regio's](luis-reference-regions.md).
 
![Pop-upvenster publiceren](./media/luis-how-to-publish-app/publish-pop-up.png)

Wanneer uw app is gepubliceerd, wordt een groen geslaagd-melding weergegeven aan de bovenkant van de browser. De groene meldingsbalk bevat ook een koppeling naar de eindpunten. 

![Pop-upvenster met koppeling publiceren naar eindpunt](./media/luis-how-to-publish-app/publish-success.png)

Als u de eindpunt-URL nodig hebt, selecteert u de koppeling. U kunt ook naar de eindpunt-URL's ophalen door het selecteren van **beheren** in het bovenste menu, schakelt u vervolgens **sleutels en eindpunten** in het menu links. 

## <a name="configuring-publish-settings"></a>Publicatie-instellingen configureren

Configureren van publicatie-instellingen hiervoor **beheren** rechts in de bovenste navigatiebalk en selecteer tot slot **Publish Settings**. 

![Instellingen voor Publiceren](./media/luis-how-to-publish-app/publish-settings.png)

### <a name="publish-after-enabling-sentiment-analysis"></a>Na het inschakelen van sentimentanalyse publiceren

<a name="enable-sentiment-analysis"></a>

Sentimentanalyse kunt LUIS integreren met [Tekstanalyse](https://azure.microsoft.com/services/cognitive-services/text-analytics/) sentiment en de sleutel phrase analysis opgeven. 

U hebt geen een Text Analytics-sleutel opgeven en er zijn geen facturering kosten voor deze service kunt u uw Azure-account. Nadat u deze instelling inschakelt, is het permanent. 

Sentimentsgegevens is een score tussen 1 en 0 waarmee wordt aangegeven welke positieve (dichter bij 1) of een negatieve (dichter bij 0) gevoel van de gegevens. Het label voor stemming van `positive`, `neutral`, en `negative` per ondersteunde cultuur. Op dit moment ondersteunt alleen Engels sentiment labels. 

Zie voor meer informatie over de JSON-eindpunt-antwoord met sentimentanalyse, [sentimentanalyse](luis-concept-data-extraction.md#sentiment-analysis)

## <a name="next-steps"></a>Volgende stappen

* Zie [sleutels beheren](./luis-how-to-manage-keys.md) sleutels toevoegen aan Azure-abonnement om LUIS en het instellen van de Bing Spell Check-sleutel en alle intents opgenomen in de resultaten.
* Zie [trainen en testen van uw app](luis-interactive-test.md) voor instructies over het testen van de gepubliceerde app in de testconsole.

