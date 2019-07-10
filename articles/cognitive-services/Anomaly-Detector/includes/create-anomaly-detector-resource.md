---
title: Ondersteuning voor containers
titleSuffix: Azure Cognitive Services
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 7/5/2019
ms.author: dapine
ms.openlocfilehash: b40f1833f08074cb0a8d45fe3afc6bac7cbac7f0
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2019
ms.locfileid: "67717071"
---
## <a name="create-an-anomaly-detector-resource"></a>Maak een resource Anomaliedetectie Detector

1. Meld u aan bij de [Azure-portal](https://portal.azure.com)
1. Klik op [maken **Anomaliedetectie Detector** ](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesAnomalyDetector) resource
1. Voer alle vereiste instellingen:

    |Instelling|Waarde|
    |--|--|
    |Name|Gewenste naam (2-64 tekens)|
    |Subscription|Selecteer de juiste abonnement|
    |Location|Selecteer een locatie in de buurt en beschikbaar|
    |Prijscategorie|`F0` -de minimale prijscategorie|
    |Resourcegroep|Selecteer een beschikbare resourcegroep|
    |Preview-versie bevestiging selectievakje (vereist)|Of u hebt gelezen de **Preview** ziet|

1. Klik op **maken** en wacht tot de resource moet worden gemaakt. Nadat deze is gemaakt, gaat u naar de resourcepagina
1. Verzamelen geconfigureerd `endpoint` en een API-sleutel:

    |Resource-tabblad in de Portal|Instelling|Waarde|
    |--|--|--|
    |**Overzicht**|Eindpunt|Kopieer het eindpunt. Het lijkt op `https://westus2.api.cognitive.microsoft.com/`|
    |**sleutels**|API-sleutel|1 van de twee sleutels kopiëren. Het is een 32 tekenreeks alfanumerieke tekens zonder spaties of streepjes, `xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`.|



