---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 04/24/2019
ms.author: glenga
ms.openlocfilehash: a3f75b7273164abc5318f16e9ab8d9883ff0c0aa
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/23/2019
ms.locfileid: "66132150"
---
## <a name="test"></a>De functie testen in Azure

Gebruiken we cURL om de geïmplementeerde functie te testen. Met behulp van de URL die u hebt gekopieerd uit de vorige stap, voeg de queryreeks `&name=<yourname>` naar de URL, zoals in het volgende voorbeeld:

```bash
curl https://myfunctionapp.azurewebsites.net/api/httptrigger?code=cCr8sAxfBiow548FBDLS1....&name=<yourname>
```

![cURL gebruiken voor het aanroepen van de functie in Azure.](./media/functions-test-function-code/functions-azure-cli-function-test-curl.png) 

U kunt ook de gekopieerde URL in naar het adres van uw webbrowser plakken. Nogmaals, voeg de queryreeks `&name=<yourname>` naar de URL voordat u de aanvraag hebt uitgevoerd.

![Een webbrowser gebruiken voor de functie aanroepen.](./media/functions-test-function-code/functions-azure-cli-function-test-browser.png)  
