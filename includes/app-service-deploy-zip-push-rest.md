## <a name="rest"></a>Het ZIP-bestand met de REST-API's implementeren 

U kunt de [implementatieservice REST-API's](https://github.com/projectkudu/kudu/wiki/REST-API) voor het implementeren van het ZIP-bestand naar uw app in Azure. Als u wilt implementeren, een POST-aanvraag naar https://<app_name>.scm.azurewebsites.net/api/zipdeploy te verzenden. De POST-aanvraag moet het ZIP-bestand in de berichthoofdtekst van bevatten. De referenties voor de implementatie voor uw app zijn opgegeven in de aanvraag via HTTP basisverificatie. Zie voor meer informatie de [.zip push implementatie verwijzing](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file). 

Voor de verificatie van de HTTP-BASISVERIFICATIE moet u de referenties voor uw App Service-implementatie. Zie voor het instellen van referenties voor uw implementatie [instellen en opnieuw instellen op gebruikersniveau referenties](../articles/app-service/app-service-deployment-credentials.md#userscope).

### <a name="with-curl"></a>Met cURL

Het volgende voorbeeld wordt het hulpprogramma cURL voor het implementeren van een ZIP-bestand. Vervang de tijdelijke aanduidingen `<username>`, `<password>`, `<zip_file_path>`, en `<app_name>`. Wanneer u hierom cURL, typ het wachtwoord.

```bash
curl -X POST -u <deployment_user> --data-binary @"<zip_file_path>" https://<app_name>.scm.azurewebsites.net/api/zipdeploy
```

Deze aanvraag activeert push-implementatie van het geüploade ZIP-bestand. U kunt de huidige en eerdere implementaties bekijken met behulp van het eindpunt https://<app_name>.scm.azurewebsites.net/api/deployments, zoals weergegeven in de volgende cURL-voorbeeld. Opnieuw vervangen `<app_name>` met de naam van uw app en `<deployment_user>` aan de gebruikersnaam van de referenties voor uw implementatie.

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

Deze aanvraag activeert push-implementatie van het geüploade ZIP-bestand. Voer de volgende opdrachten om te controleren van de huidige en eerdere implementaties. Opnieuw in en vervang de `<app_name>` tijdelijke aanduiding.

```bash
$apiUrl = "https://<app_name>.scm.azurewebsites.net/api/deployments"
Invoke-RestMethod -Uri $apiUrl -Headers @{Authorization=("Basic {0}" -f $base64AuthInfo)} -UserAgent $userAgent -Method GET
```
