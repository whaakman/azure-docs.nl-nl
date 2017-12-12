## <a name="rest"></a>Met REST API's implementeren 
 
U kunt de [implementatieservice REST-API's](https://github.com/projectkudu/kudu/wiki/REST-API) voor het implementeren van het ZIP-bestand naar uw app in Azure. Alleen verzenden naar een POST-aanvraag `https://<app_name>.scm.azurewebsites.net/api/zipdeploy` die het ZIP-bestand van het bericht bevat. De referenties voor de implementatie voor uw app zijn opgegeven in de aanvraag met behulp van HTTP-basisverificatie. Zie voor meer informatie de [.zip push implementatie naslagonderwerp](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file). 

Het hulpprogramma cURL wordt het volgende voorbeeld een aanvraag waarin het ZIP-bestand te verzenden. Op een Mac- of Linux-computer- of Bash via in Windows kunt u cURL uitvoeren vanaf de terminal. Vervang de `<zip_file_path>` tijdelijke aanduiding met het pad naar de locatie van uw project ZIP-bestand. Vervang ook `<app_name>` met de unieke naam van uw app.

Vervang de `<deployment_user>` tijdelijke aanduiding voor de gebruikersnaam van de referenties voor uw implementatie. Wanneer u hierom cURL, typ het wachtwoord. Zie voor meer informatie over het instellen van referenties voor implementatie voor uw app, [instellen en opnieuw instellen op gebruikersniveau referenties](../articles/app-service/app-service-deployment-credentials.md#userscope).   

```bash
curl POST -u <deployment_user> --data-binary @"<zip_file_path>" https://<app_name>.scm.azurewebsites.net/api/zipdeploy
```

Deze aanvraag activeert push-implementatie van het geüploade ZIP-bestand. U kunt de huidige en eerdere implementaties met bekijken de `https://<app_name>.scm.azurewebsites.net/api/deployments` eindpunt, zoals weergegeven in de volgende cURL-voorbeeld. Vervang opnieuw `<app_name>` met de naam van uw app en `<deployment_user>` aan de gebruikersnaam van de referenties voor uw implementatie.

```bash
curl -u <deployment_user> https://<app_name>.scm.azurewebsites.net/api/deployments
```