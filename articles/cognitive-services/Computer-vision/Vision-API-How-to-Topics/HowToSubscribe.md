---
title: Abonnementssleutels ophalen voor de Computer Vision-API | Microsoft Docs
description: Informatie over het verkrijgen van abonnementssleutels voor aanroepen naar de Computer Vision-API in Cognitive Services.
services: cognitive-services
author: KellyDF
manager: corncar
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: article
ms.date: 05/19/2017
ms.author: kefre
ms.openlocfilehash: 682266df9e1a1f8209904697358d10bbaa633cf1
ms.sourcegitcommit: e8f443ac09eaa6ef1d56a60cd6ac7d351d9271b9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "35760229"
---
# <a name="how-to-obtain-subscription-keys"></a>Abonnementssleutels ophalen

Computer Vision-services vereist speciale abonnementssleutels. Voor elke aanroep naar de Computer Vision-API is een abonnementssleutel vereist. Deze sleutel moet worden doorgegeven via een tekenreeksparameter of zijn opgegeven in de aanvraagheader.

Als u zich voor abonnementssleutels, Zie [abonnementen](https://azure.microsoft.com/try/cognitive-services/). Het is gratis te registreren. Prijzen voor deze services is onderhevig aan wijzigingen.

>[!NOTE]
Uw abonnementssleutels zijn geldig voor slechts één van deze [Microsoft Azure-regio's](https://azure.microsoft.com/regions/). 

| Regio | Adres |
|---|---|
| US - west | westus.API.cognitive.Microsoft.com |
| US - oost 2 | eastus2.API.cognitive.Microsoft.com |
| US - west-centraal | westcentralus.API.cognitive.Microsoft.com |
| Europa -west | westeurope.API.cognitive.Microsoft.com |
| Azië - zuidoost | southeastasia.API.cognitive.Microsoft.com |

Als u zich registreert met behulp van de gratis proefversie van Computer Vision, uw abonnementssleutels zijn geldig voor de **westcentral** regio (`https://westcentralus.api.cognitive.microsoft.com/`). Dat is het meest voorkomende geval. Echter, als u zich aanmeldt voor Computer Vision met uw Microsoft Azure-via account de [ https://azure.microsoft.com/ ](https://azure.microsoft.com/) website, u de regio opgeven voor de evaluatie van de voorgaande lijst met regio's.

Bijvoorbeeld, als u zich registreren voor Computer Vision met uw Microsoft Azure-account en u geeft `westus` voor uw regio, moet u de `westus` regio voor uw REST API-aanroepen (`https://westus.api.cognitive.microsoft.com/`).

Als u de regio voor uw abonnementssleutel nadat u hebt verkregen van de sleutel van uw proefversie vergeet, kunt u uw regio op [ https://azure.microsoft.com/try/cognitive-services/my-apis/ ](https://azure.microsoft.com/try/cognitive-services/my-apis/).

### <a name="related-links"></a>Verwante koppelingen:

* [Prijsopties voor cognitieve API's van Microsoft](https://azure.microsoft.com/pricing/details/cognitive-services/)
