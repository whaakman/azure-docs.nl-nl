---
title: Inzicht in uw sleutels LUIS - Azure | Microsoft Docs
description: Language Understanding (LUIS) gebruiken om te ontwerpen van uw app en uw endpoing query.
services: cognitive-services
author: v-geberr
manager: kaiqb
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: article
ms.date: 03/23/2018
ms.author: v-geberr
ms.openlocfilehash: d22b2ba332996d31b1db4ef4d095f0a4b443ba16
ms.sourcegitcommit: 5a7f13ac706264a45538f6baeb8cf8f30c662f8f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37109355"
---
# <a name="keys-in-luis"></a>Sleutels in LUIS
LUIS maakt gebruik van twee sleutels: [ontwerpen](#programmatic-key) en [eindpunt](#endpoint-key). De authoring sleutel wordt automatisch voor u gemaakt wanneer u uw account LUIS maakt. Wanneer u klaar bent voor uw app LUIS publiceren, moet u [maakt u de eindpuntsleutel](luis-how-to-azure-subscription.md#create-luis-endpoint-key), [toewijzen](Manage-keys.md#assign-endpoint-key) aan uw app LUIS en [gebruik in combinatie met de query eindpunt](#use-endpoint-key-in-query). 

|Sleutel|Doel|
|--|--|
|[Sleutel ontwerpen](#programmatic-key)|Ontwerpen, publiceren, het beheren van deelnemers, versiebeheer|
|[eindpuntsleutel](#endpoint-key)| Uitvoeren van query 's|

Het is belangrijk te ontwerpen LUIS apps in [regio's](luis-reference-regions.md#publishing-regions) waar u ook wilt publiceren en query's uitvoeren.

<a name="programmatic-key" ></a>
## <a name="authoring-key"></a>Sleutel ontwerpen

Een authoring sleutel, ook wel bekend als een sleutel starter wordt automatisch gemaakt wanneer u een account LUIS maken en het is gratis. U hebt één authoring sleutel via uw LUIS apps voor elk ontwerp [regio](luis-reference-regions.md). De authoring sleutel is opgegeven voor het ontwerpen van uw app LUIS of voor het testen van endpoint-query's. 

Als u wilt de authoring sleutel vinden, moet u zich aanmelden bij [LUIS] [ LUIS] en klik op de accountnaam van de in de rechterbovenhoek navigatiebalk openen **Accountinstellingen**.

![Sleutel ontwerpen](./media/luis-concept-keys/programatic-key.png)

Wanneer u ervoor **productie endpoint-query's**, maakt u een Azure [LUIS abonnement](https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/). 

> [!CAUTION]
> Voor het gemak veel van de voorbeelden de ontwerp-toets gebruiken omdat het een paar eindpunt aanroepen in biedt de [quotum](luis-boundaries.md#key-limits).  

## <a name="endpoint-key"></a>eindpuntsleutel
 Wanneer u moet **productie endpoint-query's**, maak een [LUIS sleutel](https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/) in de Azure portal. Vergeet niet de naam die wordt gebruikt om de sleutel te maken, moet u deze wanneer u de sleutel aan de app toevoegen...

Wanneer het abonnement LUIS proces is voltooid, [de sleutel toevoegen](Manage-keys.md#assign-endpoint-key) met de app op de **publiceren** pagina. 

De eindpuntsleutel kan een quotum van het eindpunt treffers op basis van de usage-plan dat u hebt opgegeven bij het maken van de sleutel. Zie [cognitieve prijzen van Services](https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/?v=17.23h) voor informatie over prijzen.

De eindpuntsleutel kan worden gebruikt voor alle LUIS-apps of voor specifieke LUIS apps. 

Gebruik niet de eindpuntsleutel voor het ontwerpen van LUIS apps. 

## <a name="use-endpoint-key-in-query"></a>Eindpuntsleutel in query gebruiken
Het eindpunt LUIS twee stijlen van query accepteert, maken beide gebruik van de sleutel, maar eindpunt op verschillende plaatsen:

|Term|Voorbeeld-url en sleutel locatie|
|--|--|
|[GET](https://westus.dev.cognitive.microsoft.com/docs/services/5819c76f40a6350ce09de1ac/operations/5819c77140a63516d81aee78)|https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/98998dcf-66d2-468e-840a-7c7c57549b5a?subscription-key=your-endpoint-key-here&verbose=true&timezoneOffset=0&q=turn op de lichten<br><br>queryreekswaarde voor `subscription-key`<br><br>Wijzig de waarde van uw eindpunt query voor de `subscription-key` uit de authoring (starter)-sleutel aan de nieuwe eindpuntsleutel om te kunnen gebruiken van de snelheid van LUIS eindpunt sleutel quotum. Als u de sleutel maken en toewijzen van de sleutel maar wijzig de waarde van de endpoint-query voor abonnement-sleutel niet ', u niet het quotum voor uw eindpunt gebruikt.|
|[POST](https://westus.dev.cognitive.microsoft.com/docs/services/5819c76f40a6350ce09de1ac/operations/5819c77140a63516d81aee79)| https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/98998dcf-66d2-468e-840a-7c7c57549b5a<br><br> de waarde van de header voor `Ocp-Apim-Subscription-Key`<br><br>Wijzig de waarde van uw eindpunt query voor de `Ocp-Apim-Subscription-Key` uit de authoring (starter)-sleutel aan de nieuwe eindpuntsleutel om te kunnen gebruiken van de snelheid van LUIS eindpunt sleutel quotum. Als u de sleutel maken en toewijzen van de sleutel, maar wijzig niet de waarde van de endpoint-query voor `Ocp-Apim-Subscription-Key`, u niet het quotum voor uw eindpunt gebruikt.|

## <a name="api-usage-of-ocp-apim-subscription-key"></a>Gebruik van de API van Ocp-Apim-Subscription-Key
De LUIS APIs gebruiken de header `Ocp-Apim-Subscription-Key`. Naam van de header wordt niet gewijzigd op basis van welke sleutel en een set API's dat u gebruikt. De header ingesteld op de authoring sleutel voor authoring API's. Als u het eindpunt gebruikt, stelt u de header voor de eindpuntsleutel. 

U kunt de endpoint-sleutel voor het ontwerpen van API's niet doorgeven. Als u dit doet, krijgt u een 401 fout: toegang is geweigerd vanwege ongeldige eindpuntsleutel. 

## <a name="key-limits"></a>Limieten
Zie [sleutel limieten](luis-boundaries.md#key-limits) en [Azure-regio's](luis-reference-regions.md). De authoring sleutel is gratis en die wordt gebruikt voor het ontwerpen. De sleutel van de endpoint LUIS een gratis laag heeft, maar moet worden door u gemaakt en gekoppeld aan uw app LUIS op de **publiceren** pagina. Deze kan niet worden gebruikt voor het ontwerpen, maar alleen eindpunt query's.

Publishing regio's zijn anders dan de regio's ontwerpen. Zorg ervoor dat u een app in de ontwerphandleiding regio die overeenkomt met de publicatie regio die u wilt maken.

## <a name="key-limit-errors"></a>Sleutel limiet fouten
Als u meer dan uw per tweede quotum foutbericht 429 HTTP-fout. Als u meer dan uw per maand quotum, er een foutbericht HTTP 403. Corrigeer deze fouten met het ophalen van een LUIS [eindpunt](#endpoint-key) sleutel [toewijzen](Manage-keys.md#assign-endpoint-key) de sleutel met de app op de **publiceren** pagina van de [LUIS] [ LUIS] website.

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [concepten](Manage-Keys.md#assign-endpoint-key) over ontwerp- en eindpunt sleutels.

[LUIS]: https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-regions#luis-website
