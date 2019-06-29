---
title: Ondersteuning voor containers
titleSuffix: Azure Cognitive Services
description: Informatie over het maken van een cognitive services text analytics-resource.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.topic: include
ms.date: 06/26/2019
ms.author: dapine
ms.openlocfilehash: 022ffcd806d4d4f89f8a8cf256a541518ea12602
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67455102"
---
## <a name="create-a-cognitive-services-text-analytics-resource"></a>Een Cognitive Services Text Analytics-resource maken

1. Meld u aan bij de [Azure-portal](https://portal.azure.com)
1. Selecteer **+ een resource maken**, gaat u naar **AI + Machine Learning > Text Analytics**, of klik op [maken **Text Analytics**](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextAnalytics)
1. Voer alle vereiste instellingen:

    |Instelling|Waarde|
    |--|--|
    |Name|Gewenste naam (2-64 tekens)|
    |Abonnement|Selecteer de juiste abonnement|
    |Location|Selecteer een locatie in de buurt|
    |Prijscategorie|`S` -de prijscategorie standard|
    |Resourcegroep|Selecteer een beschikbare resourcegroep|

1. Klik op **maken** en wacht tot de resource moet worden gemaakt. Nadat deze is gemaakt, wordt uw browser automatisch omleidt naar de zojuist gemaakte resource-pagina
1. Verzamelen geconfigureerd `endpoint` en een API-sleutel:

    |Resource-tabblad in de Portal|Instelling|Value|
    |--|--|--|
    |**Overzicht**|Eindpunt|Kopieer het eindpunt. Het lijkt op `https://northeurope.api.cognitive.microsoft.com/text/analytics/v2.0`|
    |**sleutels**|API-sleutel|1 van de twee sleutels kopiëren. Het is een 32 tekenreeks alfanumerieke tekens zonder spaties of streepjes, `xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`.|