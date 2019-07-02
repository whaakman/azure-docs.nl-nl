---
author: aahill
ms.author: aahi
ms.service: cognitive-services
ms.topic: include
ms.date: 06/20/2019
ms.openlocfilehash: 1ea0b01997d3d5cecff73f951c51de5898c2f07a
ms.sourcegitcommit: 837dfd2c84a810c75b009d5813ecb67237aaf6b8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2019
ms.locfileid: "67503467"
---
De volgende Azure CLI-opdrachten, een resource detectie van afwijkingen in de gratis prijscategorie wordt ingericht. Klik op de **uitproberen** knop, plak de code en druk op enter om uit te voeren deze in de Azure cloudshell. De sleutels voor het verifiëren van uw toepassing wordt afgedrukt. Nadat deze is voltooid, [maken van een omgevingsvariabele](../articles/cognitive-services/cognitive-services-apis-create-account.md#configure-an-environment-variable-for-authentication) voor uw een van de sleutels, met de naam `ANOMALY_DETECTOR_KEY`.

> [!NOTE]
> * U kunt eventueel het volgende doen:
>    * Maak een resource met de [Azure-portal](../articles/cognitive-services/cognitive-services-apis-create-account.md), of [Azure CLI](../articles/cognitive-services/cognitive-services-apis-create-account.md) op uw lokale computer.
>    * krijgen een [proefversie sleutel](https://azure.microsoft.com/try/cognitive-services/#decision) geldig voor zeven dagen gratis. Nadat het aanmelden beschikbaar zullen zijn op de [Azure-website](https://azure.microsoft.com/try/cognitive-services/my-apis/).
>    * deze resource weergeven op de [Azure-portal](https://portal.azure.com/). 

Cognitive Services worden vertegenwoordigd door de Azure-resources die u inricht. Alle Cognitive Services-account (en de bijbehorende Azure-resource) moeten behoren tot een Azure-resourcegroep.

1. Een Azure-resourcegroep maken

    ```azurecli-interactive
    az group create \
        --name example-anomaly-detector-resource-group \
        --location westus2
    ```

2. Maken van een bron voor de detectie van afwijkingen in de gratis laag

    ```azurecli-interactive
    az cognitiveservices account create \
        --name example-anomaly-detector-resource \
        --resource-group example-anomaly-detector-resource-group \
        --kind AnomalyDetector \
        --sku F0 \
        --location westus2 \
        --yes
    ```

3. De sleutels voor uw resource weergeven

    ```azurecli-interactive
    az cognitiveservices account keys list \
        --name example-anomaly-detector-resource \
        --resource-group example-anomaly-detector-resource-group
    ```