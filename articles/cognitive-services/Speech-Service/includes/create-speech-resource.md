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
ms.openlocfilehash: 3c42bf2b2acc2472741bd603ea9d653a314ecc40
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67711822"
---
## <a name="create-a-speech-resource"></a>Een spraak-resource maken

1. Meld u aan bij de [Azure-portal](https://portal.azure.com)
1. Klik op [maken **spraak** ](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesSpeechServices) resource
1. Voer alle vereiste instellingen:

    |Instelling|Waarde|
    |--|--|
    |Name|Gewenste naam (2-64 tekens)|
    |Subscription|Selecteer de juiste abonnement|
    |Location|Selecteer een locatie in de buurt en beschikbaar|
    |Prijscategorie|`F0` -de minimale prijscategorie|
    |Resourcegroep|Selecteer een beschikbare resourcegroep|

1. Klik op **maken** en wacht tot de resource moet worden gemaakt. Nadat deze is gemaakt, gaat u naar de resourcepagina
1. Verzamelen geconfigureerd `endpoint` en een API-sleutel:

    |Resource-tabblad in de Portal|Instelling|Value|
    |--|--|--|
    |**Overzicht**|Eindpunt|Kopieer het eindpunt. Het lijkt op `https://speech.cognitiveservices.azure.com/sts/v1.0/issuetoken`|
    |**sleutels**|API-sleutel|1 van de twee sleutels kopiëren. Het is een 32 tekenreeks alfanumerieke tekens zonder spaties of streepjes, `xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`.|
