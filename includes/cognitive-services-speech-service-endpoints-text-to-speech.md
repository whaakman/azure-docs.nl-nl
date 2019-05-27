---
author: wolfma61
ms.service: cognitive-services
ms.topic: include
ms.date: 05/06/2019
ms.author: wolfma
ms.openlocfilehash: d5a4b3a07854c2664de7ec60f3677b666798a9bd
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/23/2019
ms.locfileid: "66145358"
---
### <a name="standard-and-neural-voices"></a>Standaard- en neurale stemmen

Gebruik deze tabel om te bepalen van de beschikbaarheid van de standaard- en neurale stemmen door regio/het eindpunt:

| Regio | Eindpunt | Standard stemmen | Neurale stemmen |
|--------|----------|-----------------|---------------|
| Australië - oost | `https://australiaeast.tts.speech.microsoft.com/cognitiveservices/v1` | Ja | Ja |
| Canada - midden | `https://canadacentral.tts.speech.microsoft.com/cognitiveservices/v1` | Ja | Ja |
| US - centraal | `https://centralus.tts.speech.microsoft.com/cognitiveservices/v1` | Ja | Nee |
| Azië - oost | `https://eastasia.tts.speech.microsoft.com/cognitiveservices/v1` | Ja | Nee |
| US - oost | `https://eastus.tts.speech.microsoft.com/cognitiveservices/v1` | Ja | Ja |
| US - oost 2 | `https://eastus2.tts.speech.microsoft.com/cognitiveservices/v1` | Ja | Nee |
| Frankrijk - centraal | `https://francecentral.tts.speech.microsoft.com/cognitiveservices/v1` | Ja | Nee |
| India - centraal | `https://centralindia.tts.speech.microsoft.com/cognitiveservices/v1` | Ja | Ja |
| Japan - oost | `https://japaneast.tts.speech.microsoft.com/cognitiveservices/v1` | Ja | Nee |
| Korea Centraal | `https://koreacentral.tts.speech.microsoft.com/cognitiveservices/v1` | Ja | Nee |
| US - noord-centraal | `https://northcentralus.tts.speech.microsoft.com/cognitiveservices/v1` | Ja | Nee |
| Europa - noord | `https://northeurope.tts.speech.microsoft.com/cognitiveservices/v1` | Ja | Nee |
| US - zuid-centraal | `https://southcentralus.tts.speech.microsoft.com/cognitiveservices/v1` | Ja | Ja |
| Azië - zuidoost | `https://southeastasia.tts.speech.microsoft.com/cognitiveservices/v1` | Ja | Ja |
| Verenigd Koninkrijk Zuid | `https://uksouth.tts.speech.microsoft.com/cognitiveservices/v1` | Ja | Ja |
| Europa -west | `https://westeurope.tts.speech.microsoft.com/cognitiveservices/v1` | Ja | Ja |
| US - west | `https://westus.tts.speech.microsoft.com/cognitiveservices/v1` | Ja | Nee |
| US - west 2 | `https://westus2.tts.speech.microsoft.com/cognitiveservices/v1` | Ja | Ja |

### <a name="custom-voices"></a>Aangepaste stemmen

Als u een aangepaste spraakstijl hebt gemaakt, gebruikt u het eindpunt dat u hebt gemaakt. U kunt ook de onderstaande eindpunten vervangen de `{deploymentId}` met de implementatie-ID voor uw stem-model.

| Regio | Eindpunt |
|--------|----------|
| Australië - oost | `https://australiaeast.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Canada - midden | `https://canadacentral.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| US - centraal | `https://centralus.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Azië - oost | `https://eastasia.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| US - oost | `https://eastus.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| US - oost 2 | `https://eastus2.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Frankrijk - centraal | `https://francecentral.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| India - centraal | `https://centralindia.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Japan - oost | `https://japaneast.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Korea Centraal | `https://koreacentral.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| US - noord-centraal | `https://northcentralus.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Europa - noord | `https://northeurope.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| US - zuid-centraal | `https://southcentralus.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Azië - zuidoost | `https://southeastasia.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Verenigd Koninkrijk Zuid | `https://uksouth.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| Europa -west | `https://westeurope.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| US - west | `https://westus.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
| US - west 2 | `https://westus2.voice.speech.microsoft.com/cognitiveservices/v1?deploymentId={deploymentId}` |
