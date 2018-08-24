## <a name="rest"></a>ZIP-bestand met de REST-API's implementeren 

U kunt de [service REST-API's voor implementatie](https://github.com/projectkudu/kudu/wiki/REST-API) voor het implementeren van het ZIP-bestand in uw app in Azure. Als u wilt implementeren, een POST-aanvraag naar https://<app_name>.scm.azurewebsites.net/api/zipdeploy te verzenden. De POST-aanvraag moet het ZIP-bestand in de hoofdtekst van bericht bevatten. De implementatiereferenties voor uw app moet u opgegeven in de aanvraag met behulp van HTTP-basisverificatie. Zie voor meer informatie de [naslaginformatie over de implementatie van de ZIP-push](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file). 

Voor de HTTP-basisverificatie moet u de referenties voor uw App Service-implementatie. Zie voor meer informatie over het instellen van de referenties voor implementatie, [instellen en op gebruikersniveau referenties opnieuw instellen](../articles/app-service/app-service-deployment-credentials.md#userscope).

### <a name="with-curl"></a>Met cURL

Het volgende voorbeeld wordt het hulpprogramma cURL implementeren een ZIP-bestand. Vervang de tijdelijke aanduidingen `<username>`, `<password>`, `<zip_file_path>`, en `<app_name>`. Wanneer u hierom wordt gevraagd door cURL, typt u in het wachtwoord.

```bash
curl -X POST -u <deployment_user> --data-binary @"<zip_file_path>" https://<app_name>.scm.azurewebsites.net/api/zipdeploy
```

Deze aanvraag wordt geactiveerd voor push-implementatie van het geüploade ZIP-bestand. U kunt de huidige en eerdere implementaties controleren met behulp van de `https://<app_name>.scm.azurewebsites.net/api/deployments` eindpunt, zoals weergegeven in de volgende cURL-voorbeeld. Vervang weer `<app_name>` met de naam van uw app en `<deployment_user>` met de gebruikersnaam van de referenties voor implementatie.

```bash
curl -u <deployment_user> https://<app_name>.scm.azurewebsites.net/api/deployments
```

### <a name="with-powershell"></a>Met PowerShell

Het volgende voorbeeld wordt [Invoke-RestMethod](/powershell/module/microsoft.powershell.utility/invoke-restmethod) een aanvraag waarin het ZIP-bestand te verzenden. Vervang de tijdelijke aanduidingen `<deployment_user>`, `<deployment_password>`, `<zip_file_path>`, en `<app_name>`.

```PowerShell
#PowerShell
$username = "<deployment_user>"
$password = "<deployment_password>"
$filePath = "<zip_file_path>"
$apiUrl = "https://<app_name>.scm.azurewebsites.net/api/zipdeploy"
$base64AuthInfo = [Convert]::ToBase64String([Text.Encoding]::ASCII.GetBytes(("{0}:{1}" -f $username, $password)))
$userAgent = "powershell/1.0"
Invoke-RestMethod -Uri $apiUrl -Headers @{Authorization=("Basic {0}" -f $base64AuthInfo)} -UserAgent $userAgent -Method POST -InFile $filePath -ContentType "multipart/form-data"
```

Deze aanvraag wordt geactiveerd voor push-implementatie van het geüploade ZIP-bestand. Als u wilt controleren van de huidige en eerdere implementaties, moet u de volgende opdrachten uitvoeren. Vervang weer de `<app_name>` tijdelijke aanduiding.

```bash
$apiUrl = "https://<app_name>.scm.azurewebsites.net/api/deployments"
Invoke-RestMethod -Uri $apiUrl -Headers @{Authorization=("Basic {0}" -f $base64AuthInfo)} -UserAgent $userAgent -Method GET
```
