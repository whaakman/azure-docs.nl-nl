---
title: Uw app implementeren in Azure App Service met een ZIP- of WAR-bestand | Microsoft Docs
description: Informatie over het implementeren van uw app in Azure App Service met een ZIP-bestand (of een WAR-bestand voor Java-ontwikkelaars).
services: app-service
documentationcenter: 
author: cephalin
manager: cfowler
editor: 
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/07/2018
ms.author: cephalin;sisirap
ms.openlocfilehash: 41fb529f6b4ae923f2920919306324c86a2baa45
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/09/2018
---
# <a name="deploy-your-app-to-azure-app-service-with-a-zip-or-war-file"></a>Uw app implementeren in Azure App Service met een ZIP- of WAR-bestand

Dit artikel laat zien hoe u een ZIP-bestand of een WAR-bestand voor het implementeren van uw web-app naar [Azure App Service](app-service-web-overview.md). 

Deze implementatie van het ZIP-bestand gebruikt dezelfde Kudu service die bevoegdheden continue integratie-implementaties. Kudu ondersteunt de volgende functionaliteit voor de implementatie van het ZIP-bestand: 

- Verwijdering van bestanden die zijn overgebleven van een eerdere implementatie.
- Optie voor het proces voor het bouwen van standaard, waaronder pakket herstellen inschakelen.
- [Implementatieaanpassing](https://github.com/projectkudu/kudu/wiki/Configurable-settings#repository-and-deployment-related-settings), waaronder de implementatiescripts uitgevoerd.  
- Implementatielogboeken van de. 

Zie voor meer informatie [Kudu documentatie](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file).

De implementatie van het WAR-bestand implementeert uw [WAR](https://wikipedia.org/wiki/WAR_(file_format)) bestand in App Service aan uw Java-web-app uitvoeren. Zie [implementeren WAR-bestand](#deploy-war-file).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Vereisten

De stappen in dit artikel:

* [Maak een App Service-app](/azure/app-service/), of gebruik een app die u hebt gemaakt voor een andere zelfstudie.

## <a name="create-a-project-zip-file"></a>Een ZIP-bestand van het project maken

>[!NOTE]
> Als u de bestanden in een ZIP-bestand hebt gedownload, eerst de bestanden uitpakken. Bijvoorbeeld, als u een ZIP-bestand hebt gedownload van GitHub, u kunt geen implementeren dat bestand als-is. GitHub voegt extra geneste mappen, niet met App Service werken. 
>

Navigeer naar de hoofdmap van uw app-project in een lokale terminalvenster. 

Deze map moet bevatten de post-bestand naar uw web-app zoals _index.html_, _index.php_, en _app.js_. Het kan ook bevatten management pakketbestanden zoals _project.json_, _composer.json_, _package.json_, _bower.json_, en _requirements.txt_.

Maak een ZIP-archief van alle bestanden in uw project. De volgende opdracht maakt gebruik van het standaardhulpprogramma in de terminal:

```
# Bash
zip -r <file-name>.zip .

# PowerShell
Compress-Archive -Path * -DestinationPath <file-name>.zip
``` 

[!INCLUDE [Deploy ZIP file](../../includes/app-service-web-deploy-zip.md)]

## <a name="deploy-zip-file-with-azure-cli"></a>Het ZIP-bestand met Azure CLI implementeren

Controleer of dat uw versie van Azure CLI is 2.0.21 of hoger. Om te zien welke versie u hebt, voer `az --version` opdracht in uw terminal-venster.

Het geüploade ZIP-bestand naar uw web-app implementeren met behulp van de [az webapp implementatie bron config-zip](/cli/azure/webapp/deployment/source?view=azure-cli-latest#az_webapp_deployment_source_config_zip) opdracht.  

Het volgende voorbeeld implementeert u het ZIP-bestand dat u hebt geüpload. Wanneer u een lokale installatie van Azure CLI, geef het pad naar uw lokale ZIP-bestand voor `--src`.   

```azurecli-interactive
az webapp deployment source config-zip --resource-group myResouceGroup --name <app_name> --src clouddrive/<filename>.zip
```

Met deze opdracht worden de bestanden en mappen uit het ZIP-bestand geïmplementeerd in de standaardmap voor de App Service-toepassing (`\home\site\wwwroot`) en wordt de app opnieuw opgestart. Als er een aanvullend aangepast opbouwproces is geconfigureerd, wordt dit ook uitgevoerd. Zie voor meer informatie [Kudu documentatie](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file).

[!INCLUDE [app-service-deploy-zip-push-rest](../../includes/app-service-deploy-zip-push-rest.md)]  

## <a name="deploy-war-file"></a>Implementeer WAR-bestand

Als u wilt implementeren een WAR-bestand in App Service, een POST-aanvraag naar https://<app_name>.scm.azurewebsites.net/api/wardeploy te verzenden. De POST-aanvraag moet het bestand .war in de hoofdtekst van bericht bevatten. De referenties voor de implementatie voor uw app zijn opgegeven in de aanvraag via HTTP basisverificatie. 

Voor de verificatie van de HTTP-BASISVERIFICATIE moet u de referenties voor uw App Service-implementatie. Zie voor het instellen van referenties voor uw implementatie [instellen en opnieuw instellen op gebruikersniveau referenties](app-service-deployment-credentials.md#userscope).

### <a name="with-curl"></a>Met cURL

Het volgende voorbeeld wordt het hulpprogramma cURL om een bestand .war te implementeren. Vervang de tijdelijke aanduidingen `<username>`, `<war_file_path>`, en `<app_name>`. Wanneer u hierom cURL, typ het wachtwoord.

```bash
curl -X POST -u <username> --data-binary @"<war_file_path>" https://<app_name>.scm.azurewebsites.net/api/wardeploy
```

### <a name="with-powershell"></a>Met PowerShell

Het volgende voorbeeld wordt [Invoke-RestMethod](/powershell/module/microsoft.powershell.utility/invoke-restmethod) een aanvraag waarin het bestand .war te verzenden. Vervang de tijdelijke aanduidingen `<deployment_user>`, `<deployment_password>`, `<zip_file_path>`, en `<app_name>`.

```PowerShell
$username = "<deployment_user>"
$password = "<deployment_password>"
$filePath = "<war_file_path>"
$apiUrl = "https://<app_name>.scm.azurewebsites.net/api/wardeploy"
$base64AuthInfo = [Convert]::ToBase64String([Text.Encoding]::ASCII.GetBytes(("{0}:{1}" -f $username, $password)))
Invoke-RestMethod -Uri $apiUrl -Headers @{Authorization=("Basic {0}" -f $base64AuthInfo)} -Method POST -InFile $filePath -ContentType "multipart/form-data"
```

## <a name="next-steps"></a>Volgende stappen

Probeer meer geavanceerde implementatiescenario's voor [implementeren naar Azure met Git](app-service-deploy-local-git.md). Implementatie op basis van GIT in Azure kunt versiebeheer, pakket herstellen en MSBuild.

## <a name="more-resources"></a>Meer bronnen

* [Kudu: Implementeren vanaf een zip-bestand](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file)
* [Referenties voor Azure App Service-implementatie](app-service-deploy-ftp.md)
