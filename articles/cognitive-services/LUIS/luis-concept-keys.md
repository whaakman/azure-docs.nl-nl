---
title: Abonnementssleutels
titleSuffix: Language Understadning - Azure Cognitive Services
description: LUIS maakt gebruik van twee sleutels, de gratis authoring sleutel om uw model te maken en de eindpuntsleutel met een datalimiet voor het uitvoeren van query's de voorspelling-eindpunt met uitingen van de gebruiker.
services: cognitive-services
author: diberry
manager: cgronlun
ms.custom: seodec18
ms.service: cognitive-services
ms.component: language-understanding
ms.topic: conceptual
ms.date: 09/10/2018
ms.author: diberry
ms.openlocfilehash: 9b660743f335e1aa9ef7e7b569738d9ee3ca139d
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2018
ms.locfileid: "53104523"
---
# <a name="keys-in-luis"></a>Sleutels in LUIS
LUIS maakt gebruik van twee sleutels: [ontwerpen](#programmatic-key) en [eindpunt](#endpoint-key). De ontwerphandleiding sleutel wordt automatisch voor u gemaakt wanneer u uw LUIS-account maakt. Wanneer u klaar om uw LUIS-app te publiceren bent, moet u [maken van de eindpuntsleutel](luis-how-to-azure-subscription.md#create-luis-endpoint-key), [toewijzen](luis-how-to-manage-keys.md#assign-endpoint-key) aan uw LUIS-app en [gebruiken met de query eindpunt](#use-endpoint-key-in-query). 

|Sleutel|Doel|
|--|--|
|[Sleutel ontwerpen](#programmatic-key)|Ontwerpen, publiceren, beheren van deelnemers, versiebeheer|
|[Eindpuntsleutel](#endpoint-key)| Uitvoeren van query's|

Het is belangrijk om te maken van LUIS-apps in [regio's](luis-reference-regions.md#publishing-regions) waar u ook wilt publiceren en op te vragen.

<a name="programmatic-key" ></a>
## <a name="authoring-key"></a>Sleutel ontwerpen

Een authoring sleutel, ook wel bekend als een starter-sleutel, wordt automatisch gemaakt wanneer u een LUIS-account maakt en het is gratis. U beschikt over een authoring sleutel uit al uw LUIS-apps voor elk ontwerpen [regio](luis-reference-regions.md). De authoring sleutel is opgegeven om te maken van uw LUIS-app of het eindpunt query's testen. 

Als u wilt zoeken in de ontwerphandleiding sleutel, moet u zich aanmelden bij [LUIS](luis-reference-regions.md#luis-website) en klik op de accountnaam in de rechterbovenhoek navigatiebalk openen **Accountinstellingen**.

![Sleutel ontwerpen](./media/luis-concept-keys/programatic-key.png)

Als u wilt maken **productie-eindpunt-query's**, maken van de Azure [LUIS abonnement](https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/). 

> [!CAUTION]
> Voor het gemak veel van de voorbeelden de sleutel ontwerpen gebruiken omdat het een paar eindpunt aanroepen in biedt de [quotum](luis-boundaries.md#key-limits).  

## <a name="endpoint-key"></a>Eindpuntsleutel
 Wanneer u moet **productie-eindpunt-query's**, maak een [LUIS sleutel](https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/) in Azure portal. Vergeet niet de naam die wordt gebruikt om de sleutel te maken, u hebt deze nodig wanneer u de sleutel toevoegen aan de app.

Wanneer het proces van LUIS-abonnement is voltooid, [toewijzen van de sleutel](luis-how-to-manage-keys.md#assign-endpoint-key) naar de app. 

De eindpuntsleutel kan een quotum van eindpunt treffers op basis van het gebruik-abonnement dat u hebt opgegeven bij het maken van de sleutel. Zie [prijzen van Cognitive Services](https://azure.microsoft.com/pricing/details/cognitive-services/language-understanding-intelligent-services/?v=17.23h) voor informatie over prijzen.

De eindpuntsleutel kan worden gebruikt voor al uw LUIS-apps of voor specifieke LUIS-apps. 

Gebruik niet de eindpuntsleutel voor het ontwerpen van LUIS-apps. 

## <a name="use-endpoint-key-in-query"></a>Gebruik de eindpuntsleutel in query
Het eindpunt LUIS accepteert twee stijlen van query, zowel de sleutel, maar voor een eindpunt gebruiken op verschillende plaatsen:

|term|Voorbeeld-url en de sleutel-locatie|
|--|--|
|[GET](https://westus.dev.cognitive.microsoft.com/docs/services/5819c76f40a6350ce09de1ac/operations/5819c77140a63516d81aee78)|`https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2?subscription-key=your-endpoint-key-here&verbose=true&timezoneOffset=0&q=turn%20on%20the%20lights`<br><br>waarde voor de QueryString voor `subscription-key`<br><br>Wijzig de waarde van uw eindpunt query voor de `subscription-key` uit de authoring (starter)-sleutel, aan de nieuwe eindpuntsleutel als u wilt gebruiken, de snelheid van LUIS-eindpunt sleutel quotum. Als u de sleutel maken en toewijzen van de sleutel maar wijzig de waarde van de query eindpunt voor abonnement-sleutel niet ', u niet met behulp van uw quotum aanvragen voor de sleutel van eindpunt.|
|[POST](https://westus.dev.cognitive.microsoft.com/docs/services/5819c76f40a6350ce09de1ac/operations/5819c77140a63516d81aee79)| `https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/df67dcdb-c37d-46af-88e1-8b97951ca1c2`<br><br> de waarde van de header voor `Ocp-Apim-Subscription-Key`<br><br>Wijzig de waarde van uw eindpunt query voor de `Ocp-Apim-Subscription-Key` uit de authoring (starter)-sleutel, aan de nieuwe eindpuntsleutel als u wilt gebruiken, de snelheid van LUIS-eindpunt sleutel quotum. Als u de sleutel maken en toewijzen van de sleutel, maar wijzig niet de waarde van de query eindpunt voor `Ocp-Apim-Subscription-Key`, u niet met behulp van uw quotum aanvragen voor de sleutel van eindpunt.|

De app-ID die wordt gebruikt in de vorige URL's, `df67dcdb-c37d-46af-88e1-8b97951ca1c2`, is de openbare IoT-app die wordt gebruikt voor de [interactieve demonstratie](https://azure.microsoft.com/services/cognitive-services/language-understanding-intelligent-service/). 

## <a name="api-usage-of-ocp-apim-subscription-key"></a>API-gebruik van de Ocp-Apim-Subscription-Key
De LUIS APIs gebruiken de header `Ocp-Apim-Subscription-Key`. De header-naam wordt niet gewijzigd op basis van welke sleutel en een set API's dat u gebruikt. De koptekst van de sleutel die ontwerpen voor authoring API's instellen. Als u het eindpunt gebruikt, stelt u de header aan de eindpuntsleutel. 

U kunt de eindpuntsleutel voor het ontwerpen van API's niet doorgeven. Als u dit doet, krijgt u een 401-fout - toegang is geweigerd vanwege ongeldige eindpuntsleutel. 

## <a name="key-limits"></a>Limieten
Zie [sleutel limieten](luis-boundaries.md#key-limits) en [Azure-regio's](luis-reference-regions.md). De ontwerphandleiding sleutel is gratis en die wordt gebruikt voor het ontwerpen. De eindpuntsleutel LUIS is een gratis laag, maar moet worden door u gemaakt en die zijn gekoppeld aan uw LUIS-app op de **publiceren** pagina. Het kan niet worden gebruikt voor het ontwerpen, maar alleen eindpunt query's.

Publicatie-regio's wijken af van het ontwerpen van regio's. Zorg ervoor dat u een app maken in de ontwerphandleiding regio overeenkomt met de gewenste regio voor publiceren.

## <a name="key-limit-errors"></a>Belangrijkste limiet fouten
Als u meer dan uw per seconde quotum, ontvangt u fout HTTP 429. Als u meer dan uw per maand quotum, ontvangt u een HTTP 403-fout. Los de fouten met het ophalen van een LUIS [eindpunt](#endpoint-key) sleutel [toe te wijzen](luis-how-to-manage-keys.md#assign-endpoint-key) de sleutel met de app op de **publiceren** pagina van de [LUIS](luis-reference-regions.md#luis-website) website.

## <a name="automating-assignment-of-the-endpoint-key"></a>Toewijzing van de eindpuntsleutel automatiseren

Als u wilt de eindpuntsleutel aan een LUIS-app toewijst, moet u de website van LUIS gebruiken voor de juiste ontwerpen en publicatie [regio's](luis-reference-regions.md). Er is **geen** geautomatiseerde methode van dit te doen, ongeacht het mechanisme zoals met een Azure resource manager-script, Azure CLI, programmatische SDK, of met de API's.

## <a name="next-steps"></a>Volgende stappen

* Informatie over [concepten](luis-how-to-manage-keys.md#assign-endpoint-key) over ontwerp- en eindpunt-codes.