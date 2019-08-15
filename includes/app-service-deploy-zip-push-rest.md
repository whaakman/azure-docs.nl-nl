---
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 08/12/2019
ms.author: cephalin
ms.openlocfilehash: 4f3236c0a167a2b6f7586c6cb5fea8e30f55a86c
ms.sourcegitcommit: 0f54f1b067f588d50f787fbfac50854a3a64fff7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/12/2019
ms.locfileid: "68954057"
---
## <a name="rest"></a>ZIP-bestand implementeren met REST-Api's 

U kunt de [rest api's van de implementatie service](https://github.com/projectkudu/kudu/wiki/REST-API) gebruiken om het zip-bestand te implementeren in uw app in Azure. Als u wilt implementeren, verzendt u een POST-aanvraag naar https://< app_name >. scm. azurewebsites. net/API/zipdeploy. De POST-aanvraag moet het zip-bestand in de hoofd tekst van het bericht bevatten. De implementatiereferenties voor uw app moet u opgegeven in de aanvraag met behulp van HTTP-basisverificatie. Zie voor meer informatie de [referentie. zip push-implementatie](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file). 

Voor de HTTP-basis verificatie hebt u uw App Service implementatie referenties nodig. Zie [referenties voor gebruikers niveau instellen en opnieuw](../articles/app-service/deploy-configure-credentials.md#userscope)instellen om te zien hoe u uw implementatie referenties kunt instellen.

### <a name="with-curl"></a>Met krul

In het volgende voor beeld wordt het gereedschap krul gebruikt voor het implementeren van een zip-bestand. `<username>`Vervang de tijdelijke aanduidingen `<password>` `<zip_file_path>`,, en `<app_name>`. Wanneer u wordt gevraagd door krul, typt u het wacht woord.

```bash
curl -X POST -u <deployment_user> --data-binary @"<zip_file_path>" https://<app_name>.scm.azurewebsites.net/api/zipdeploy
```

Met deze aanvraag wordt push implementatie geactiveerd vanuit het geüploade zip-bestand. U kunt de huidige en eerdere implementaties controleren met behulp `https://<app_name>.scm.azurewebsites.net/api/deployments` van het eind punt, zoals wordt weer gegeven in het volgende krul-voor beeld. Vervang `<app_name>` opnieuw door de naam van uw app en `<deployment_user>` met de gebruikers naam van uw implementatie referenties.

```bash
curl -u <deployment_user> https://<app_name>.scm.azurewebsites.net/api/deployments
```

### <a name="with-powershell"></a>Met PowerShell

In het volgende voor beeld wordt het zip-bestand geüpload [-AzWebapp](/powershell/module/az.websites/publish-azwebapp) . Vervang de tijdelijke `<group-name>`aanduidingen `<app-name>`, en `<zip-file-path>`.

```powershell
Publish-AzWebapp -ResourceGroupName <group-name> -Name <app-name> -ArchivePath <zip-file-path>
```

Met deze aanvraag wordt push implementatie geactiveerd vanuit het geüploade zip-bestand. 

Voer de volgende opdrachten uit om de huidige en eerdere implementaties te bekijken. Vervang opnieuw de `<deployment-user>`tijdelijke aanduidingen, `<app-name>` `<deployment-password>`, en.

```bash
$username = "<deployment-user>"
$password = "<deployment-password>"
$apiUrl = "https://<app-name>.scm.azurewebsites.net/api/deployments"
$base64AuthInfo = [Convert]::ToBase64String([Text.Encoding]::ASCII.GetBytes(("{0}:{1}" -f $username, $password)))
$userAgent = "powershell/1.0"
Invoke-RestMethod -Uri $apiUrl -Headers @{Authorization=("Basic {0}" -f $base64AuthInfo)} -UserAgent $userAgent -Method GET
```
