---
title: Abonnementssleutels - Computer Vision ophalen
titlesuffix: Azure Cognitive Services
description: Informatie over het verkrijgen van abonnementssleutels voor aanroepen naar de Computer Vision-API in Azure Cognitive Services.
services: cognitive-services
author: KellyDF
manager: cgronlun
ms.service: cognitive-services
ms.component: computer-vision
ms.topic: article
ms.date: 05/19/2017
ms.author: kefre
ms.custom: seodec18
ms.openlocfilehash: 820531cc2254d9cbc7aaf7e758dd0457b282d892
ms.sourcegitcommit: 7cd706612a2712e4dd11e8ca8d172e81d561e1db
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2018
ms.locfileid: "53580804"
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

* [Prijsopties voor Azure Cognitive Services-API 's](https://azure.microsoft.com/pricing/details/cognitive-services/)
