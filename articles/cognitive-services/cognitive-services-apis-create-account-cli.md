---
title: Een Cognitive Services-account maken met de Azure CLI
titlesuffix: Azure Cognitive Services
description: Het maken van een Azure Cognitive Services API's-account met de Azure CLI.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.topic: conceptual
ms.date: 06/26/2019
ms.author: aahi
ms.openlocfilehash: acafc2c42c2946632496b646d001c58d6b48c2a6
ms.sourcegitcommit: cf438e4b4e351b64fd0320bf17cc02489e61406a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/08/2019
ms.locfileid: "67657717"
---
# <a name="create-a-cognitive-services-account-using-the-azure-command-line-interfacecli"></a>Een Cognitive Services-account maken met de Azure Command-Line Interface(CLI)

In deze snelstartgids leert u hoe u zich aanmelden voor Azure Cognitive Services en het maken van een account met een enkele service of meerdere service-abonnement, met de [Azure Command Line Interface(CLI)](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). Deze services worden vertegenwoordigd door Azure [resources](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal), waarmee u verbinding maken met een of meer van de Azure Cognitive Services-API's.

## <a name="prerequisites"></a>Vereisten

* Een geldig Azure-abonnement. [Maak een account](https://azure.microsoft.com/free/) gratis.
* De [Interface(CLI) Azure vanaf de opdrachtregel](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)

[!INCLUDE [cognitive-services-subscription-types](../../includes/cognitive-services-subscription-types.md)]

## <a name="install-the-azure-cli-and-sign-in"></a>Azure CLI installeren en aanmelden 

Installeer de [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). Als u zich bij de lokale installatie van de CLI, wilt uitvoeren het [az login](https://docs.microsoft.com/cli/azure/reference-index#az-login) opdracht:

```console
az login
```

U kunt ook de groene **uitproberen** knop deze opdrachten uitvoeren in uw browser.
 
## <a name="create-a-new-azure-cognitive-services-resource-group"></a>Maak een nieuwe resourcegroep voor Azure Cognitive Services

Uw abonnement op Cognitive Services worden vertegenwoordigd door de Azure-resources. Alle Cognitive Services-account (en de bijbehorende Azure-resource) moeten behoren tot een Azure-resourcegroep.

### <a name="choose-your-resource-group-location"></a>Kies de locatie voor resourcegroep

Voor het maken van een resource, moet u een van de Azure-locaties beschikbaar voor uw abonnement. U kunt een lijst met beschikbare locaties met ophalen van de [az account list-locations](/cli/azure/account#az-account-list-locations) opdracht. De meeste Cognitive Services zijn toegankelijk vanaf verschillende locaties. De optie die het dichtst bij u kiezen of zien welke locaties zijn beschikbaar voor de service.

> [!IMPORTANT]
> * Vergeet niet uw Azure-locatie, als u deze nodig heeft bij het aanroepen van de Azure Cognitive Services.
> * De beschikbaarheid van sommige Cognitive Services kan per regio verschillen. Zie voor meer informatie, [Azure-producten per regio](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services).  

```azurecli-interactive
az account list-locations \
    --query "[].{Region:name}" \
    --out table
```

Nadat u uw azure-locatie hebt, maakt u een nieuwe resourcegroep in de Azure CLI met de [az-groep maken](/cli/azure/group#az-group-create) opdracht.

Vervang in het volgende voorbeeld wordt de azure-locatie `westus2` met een van de Azure-locaties beschikbaar voor uw abonnement.

```azurecli-interactive
az group create \
    --name cognitive-services-resource-group \
    --location westus2
```

## <a name="create-a-cognitive-services-resource"></a>Een Cognitive Services-resource maken

### <a name="choose-a-cognitive-service-and-pricing-tier"></a>Een cognitive service en de prijscategorie kiezen

Wanneer u een nieuwe resource maakt, moet u weten welk "type" van de service die u gebruiken wilt, samen met de [prijscategorie](https://azure.microsoft.com/pricing/details/cognitive-services/) (of sku) u wilt. U gebruikt deze en andere gegevens als parameters bij het maken van de resource.

> [!NOTE]
> Veel Cognitive services hebben een gratis laag die u gebruiken kunt om te proberen de service. Gebruiken voor het gebruik van de gratis laag, `F0` als de sku voor uw resource.

### <a name="vision"></a>Vision

| Service                    | Kind                      |
|----------------------------|---------------------------|
| Computer Vision            | `ComputerVision`          |
| Custom Vision - voorspelling | `CustomVision.Prediction` |
| Custom Vision - Training   | `CustomVision.Training`   |
| Face-API                   | `Face`                    |
| Form Recognizer            | `FormRecognizer`          |
| Ink Recognizer             | `InkRecognizer`           |

### <a name="search"></a>Search

| Service            | Kind                  |
|--------------------|-----------------------|
| Bing Automatische suggesties   | `Bing.Autosuggest.v7` |
| Bing Aangepaste zoekopdrachten | `Bing.CustomSearch`   |
| Bing Entiteiten zoeken | `Bing.EntitySearch`   |
| Bing Zoeken        | `Bing.Search.v7`      |
| Bing Spellingcontrole   | `Bing.SpellCheck.v7`  |

### <a name="speech"></a>Speech

| Service            | Kind                 |
|--------------------|----------------------|
| Spraakservices    | `SpeechServices`     |
| Spraakherkenning | `SpeakerRecognition` |

### <a name="language"></a>Taal

| Service            | Kind                |
|--------------------|---------------------|
| Formulier begrijpen | `FormUnderstanding` |
| LUIS               | `LUIS`              |
| QnA Maker          | `QnAMaker`          |
| Tekstanalyse     | `TextAnalytics`     |
| Tekstomzetting   | `TextTranslation`   |

### <a name="decision"></a>Besluit

| Service           | Kind               |
|-------------------|--------------------|
| Anomaly Detector  | `AnomalyDetector`  |
| Content Moderator | `ContentModerator` |
| Personalizer      | `Personalizer`     |

U kunt een lijst met beschikbare Cognitive Service "type" vinden met de [az cognitiveservices account list-soorten](https://docs.microsoft.com/cli/azure/cognitiveservices/account?view=azure-cli-latest#az-cognitiveservices-account-list-kinds) opdracht:

```azurecli-interactive
az cognitiveservices account list-kinds
```

### <a name="add-a-new-resource-to-your-resource-group"></a>Een nieuwe resource toevoegen aan de resourcegroep

Gebruik wilt maken en zich abonneren op een nieuwe Cognitive Services-resource, de [az cognitiveservices account maken](https://docs.microsoft.com/cli/azure/cognitiveservices/account?view=azure-cli-latest#az-cognitiveservices-account-create) opdracht. Met deze opdracht wordt een nieuwe factureerbare bron toegevoegd aan de resourcegroep die eerder hebt gemaakt. Wanneer u uw nieuwe resource maakt, moet u weten welk "type" van de service die u gebruiken wilt, samen met de prijzen laag (of sku) en een Azure-locatie:

U kunt een (gratis) F0-resource maken voor de detectie van afwijkingen, met de naam `anomaly-detector-resource` met de onderstaande opdracht.

```azurecli-interactive
az cognitiveservices account create \
    --name anomaly-detector-resource \
    --group cognitive-services-resource-group \
    --kind AnomalyDetector \
    --sku F0 \
    --location westus2 \
    --yes
```

## <a name="get-the-keys-for-your-subscription"></a>De sleutels voor uw abonnement ophalen

Als u wilt aanmelden bij de lokale installatie van de opdrachtregel Interface(CLI), gebruikt u de [az login](https://docs.microsoft.com/cli/azure/reference-index?view=azure-cli-latest#az-login) opdracht.

```console
az login
```

Gebruik de [az cognitiveservices account sleutels lijst](https://docs.microsoft.com/cli/azure/cognitiveservices/account/keys?view=azure-cli-latest#az-cognitiveservices-account-keys-list) opdracht voor het ophalen van de sleutels voor uw Cognitive Service.

```azurecli-interactive
    az cognitiveservices account keys list \
    --name anomaly-detector-resource \
    --resource-group cognitive-services-resource-group
```

[!INCLUDE [cognitive-services-environment-variables](../../includes/cognitive-services-environment-variables.md)]

## <a name="clean-up-resources"></a>Resources opschonen

Als u wilt wissen en verwijderen van een Cognitive Services-abonnement, kunt u de resource of resourcegroep verwijderen. Als u de resourcegroep verwijdert, verwijdert u ook alle andere resources die zijn gekoppeld aan de resourcegroep.

Gebruik de opdracht az group delete te verwijderen van de resourcegroep en alle bijbehorende resources, met inbegrip van het nieuwe opslagaccount.

```azurecli-interactive
az group delete --name storage-resource-group
```

## <a name="see-also"></a>Zie ook

* [Verifiëren van aanvragen voor Azure Cognitive Services](authentication.md)
* [Wat is Azure Cognitive Services?](Welcome.md)
* [Ondersteuning van natuurlijke taal](language-support.md)
* [Ondersteuning voor docker-containers](cognitive-services-container-support.md)
