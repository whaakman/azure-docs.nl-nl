---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/04/2018
ms.author: glenga
ms.openlocfilehash: 914c006daf49e22ebec870a549bfdbc63f882647
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/28/2019
ms.locfileid: "55148077"
---
## <a name="test"></a>De functie testen

Gebruik cURL om de geïmplementeerde functie te testen op een Mac- of Linux-computer of met Powershell op Windows. Voer de volgende cURL-opdracht uit, waarbij u de plaatsaanduiding `<app_name>` vervangt door de naam van de functie-app die u hebt gemaakt. Voeg de queryreeks `&name=<yourname>` toe aan de URL.

```powershell
Invoke-WebRequest -Uri "https://<app_name>.azurewebsites.net/api/MyHttpTrigger?name=<yourname>"
```

```bash
curl https://<app_name>.azurewebsites.net/api/MyHttpTrigger?name=<yourname>
```  

![Het antwoord van de functie weergegeven in een browser.](./media/functions-test-function-code/functions-azure-cli-function-test-curl.png)  

Als `cURL` of `Invoke-WebRequest` niet beschikbaar is op uw opdrachtregel, moet u gewoon dezelfde URL opgeven op de adresbalk van uw webbrowser. Vervang weer de plaatsaanduiding `<app_name>` door de naam van uw functie-app, voeg de queryreeks `&name=<yourname>` toe aan de URL en voer de aanvraag uit.

    https://<app_name>.azurewebsites.net/api/MyHttpTrigger?name=<yourname>
   
![Het antwoord van de functie weergegeven in een browser.](./media/functions-test-function-code/functions-azure-cli-function-test-browser.png)  
