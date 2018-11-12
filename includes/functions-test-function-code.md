---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/04/2018
ms.author: glenga
ms.openlocfilehash: ae88a6132c79d9d9f930f6445e53b0f0452c9f2a
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51262604"
---
## <a name="test"></a>De functie testen

Gebruik cURL om de geïmplementeerde functie te testen op een Mac- of Linux-computer of met Bash op Windows. Voer de volgende cURL-opdracht uit, waarbij u de plaatsaanduiding `<app_name>` vervangt door de naam van de functie-app die u hebt gemaakt. Voeg de queryreeks `&name=<yourname>` toe aan de URL.

```bash
curl https://<app_name>.azurewebsites.net/api/MyHttpTrigger?name=<yourname>
```  

![Het antwoord van de functie weergegeven in een browser.](./media/functions-test-function-code/functions-azure-cli-function-test-curl.png)  

Als cURL niet beschikbaar is op uw opdrachtregel, moet u gewoon dezelfde URL opgeven op de adresbalk van uw webbrowser. Vervang weer de plaatsaanduiding `<app_name>` door de naam van uw functie-app, voeg de queryreeks `&name=<yourname>` toe aan de URL en voer de aanvraag uit.

    https://<app_name>.azurewebsites.net/api/MyHttpTrigger?name=<yourname>
   
![Het antwoord van de functie weergegeven in een browser.](./media/functions-test-function-code/functions-azure-cli-function-test-browser.png)  
