---
title: Code implementeren met een ZIP- of WAR-bestand - Azure App Service | Microsoft Docs
description: Leer hoe u uw app implementeren in Azure App Service met een ZIP-bestand (of een WAR-bestand voor Java-ontwikkelaars).
services: app-service
documentationcenter: ''
author: cephalin
manager: cfowler
editor: ''
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/07/2018
ms.author: cephalin;sisirap
ms.custom: seodec18
ms.openlocfilehash: 1f94544a2e7a26ab00432a3564bb752c71254844
ms.sourcegitcommit: 0dd053b447e171bc99f3bad89a75ca12cd748e9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58484877"
---
# <a name="deploy-your-app-to-azure-app-service-with-a-zip-or-war-file"></a>Uw app implementeren in Azure App Service met een ZIP- of WAR-bestand

In dit artikel leest u hoe u een ZIP-bestand of WAR-bestand naar uw web-app te implementeren [Azure App Service](overview.md). 

Deze implementatie ZIP-bestand gebruikt de dezelfde Kudu-service die bevoegdheden continue integratie-implementaties. Kudu ondersteunt de volgende functionaliteit voor de implementatie van de ZIP-bestand: 

- Verwijdering van bestanden die zijn overgebleven van een eerdere implementatie.
- Optie voor het bouwproces standaard, waaronder pakket herstellen inschakelen.
- [Implementatieaanpassing](https://github.com/projectkudu/kudu/wiki/Configurable-settings#repository-and-deployment-related-settings), waaronder uitvoeren van implementatiescripts.  
- Logboeken van de implementatie. 

Zie onze [Kudu-documentatie](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file) voor meer informatie.

De implementatie van WAR-bestand implementeert uw [WAR](https://wikipedia.org/wiki/WAR_(file_format)) bestand in App Service om uit te voeren van uw Java-web-app. Zie [implementeren WAR-bestand](#deploy-war-file).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Vereisten

Voltooi de stappen in dit artikel:

* [Maak een App Service-app](/azure/app-service/), of gebruik een app die u hebt gemaakt voor een andere zelfstudie.

## <a name="create-a-project-zip-file"></a>Een ZIP-bestand van het project maken

>[!NOTE]
> Als u de bestanden in een ZIP-bestand hebt gedownload, dient Pak de bestanden eerst uit te voeren. Bijvoorbeeld, als u een ZIP-bestand gedownload vanuit GitHub, u kunt geen implementeren dat bestand als-is. GitHub voegt extra geneste mappen, niet met App Service werken. 
>

Navigeer in het lokale terminalvenster naar de hoofdmap van uw app-project. 

Deze map moet bevatten de post-bestand naar uw web-app, zoals _index.html_, _index.php_, en _app.js_. Ook kan het pakket management-bestanden, zoals bevatten _project.json_, _composer.json_, _package.json_, _bower.json_, en _requirements.txt_.

Maak een ZIP-archief van alle bestanden in uw project. De volgende opdracht maakt gebruik van het standaardhulpprogramma in de terminal:

```
# Bash
zip -r <file-name>.zip .

# PowerShell
Compress-Archive -Path * -DestinationPath <file-name>.zip
``` 

[!INCLUDE [Deploy ZIP file](../../includes/app-service-web-deploy-zip.md)]

## <a name="deploy-zip-file-with-azure-cli"></a>Implementeer ZIP-bestand met Azure CLI

Zorg ervoor dat uw Azure CLI versie 2.0.21 of hoger. Om te zien welke versie u hebt, uitgevoerd `az --version` opdracht in uw terminalvenster.

Het geüploade ZIP-bestand naar uw web-app implementeren met behulp van de [az webapp deployment source config-zip](/cli/azure/webapp/deployment/source?view=azure-cli-latest#az-webapp-deployment-source-config-zip) opdracht.  

Het volgende voorbeeld implementeert u het ZIP-bestand dat u hebt geüpload. Wanneer u een lokale installatie van Azure CLI gebruikt, geef het pad naar uw lokale ZIP-bestand voor `--src`.   

```azurecli-interactive
az webapp deployment source config-zip --resource-group myResourceGroup --name <app_name> --src clouddrive/<filename>.zip
```

Met deze opdracht worden de bestanden en mappen uit het ZIP-bestand geïmplementeerd in de standaardmap voor de App Service-toepassing (`\home\site\wwwroot`) en wordt de app opnieuw opgestart. Als er een aanvullend aangepast opbouwproces is geconfigureerd, wordt dit ook uitgevoerd. Zie onze [Kudu-documentatie](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file) voor meer informatie.

[!INCLUDE [app-service-deploy-zip-push-rest](../../includes/app-service-deploy-zip-push-rest.md)]  

## <a name="deploy-war-file"></a>WAR-bestand implementeren

Voor het implementeren van een WAR-bestand in App Service, een POST-aanvraag naar https://<app_name>.scm.azurewebsites.net/api/wardeploy te verzenden. Het WAR-bestand moet zijn opgenomen in de hoofdtekst van de POST-aanvraag. De implementatiereferenties voor uw app moet u opgegeven in de aanvraag met behulp van HTTP-basisverificatie. 

Voor de HTTP-basisverificatie moet u de referenties voor uw App Service-implementatie. Zie voor meer informatie over het instellen van de referenties voor implementatie, [instellen en op gebruikersniveau referenties opnieuw instellen](deploy-configure-credentials.md#userscope).

### <a name="with-curl"></a>Met cURL

Het volgende voorbeeld wordt het hulpprogramma cURL implementeren een WAR-bestand. Vervang de tijdelijke aanduidingen `<username>`, `<war_file_path>`, en `<app_name>`. Wanneer u hierom wordt gevraagd door cURL, typt u in het wachtwoord.

```bash
curl -X POST -u <username> --data-binary @"<war_file_path>" https://<app_name>.scm.azurewebsites.net/api/wardeploy
```

### <a name="with-powershell"></a>Met PowerShell

Het volgende voorbeeld wordt [Invoke-RestMethod](/powershell/module/microsoft.powershell.utility/invoke-restmethod) een aanvraag met het .war-bestand te verzenden. Vervang de tijdelijke aanduidingen `<deployment_user>`, `<deployment_password>`, `<zip_file_path>`, en `<app_name>`.

```powershell
$username = "<deployment_user>"
$password = "<deployment_password>"
$filePath = "<war_file_path>"
$apiUrl = "https://<app_name>.scm.azurewebsites.net/api/wardeploy"
$base64AuthInfo = [Convert]::ToBase64String([Text.Encoding]::ASCII.GetBytes(("{0}:{1}" -f $username, $password)))
Invoke-RestMethod -Uri $apiUrl -Headers @{Authorization=("Basic {0}" -f $base64AuthInfo)} -Method POST -InFile $filePath -ContentType "multipart/form-data"
```

[!INCLUDE [What happens to my app during deployment?](../../includes/app-service-deploy-atomicity.md)]

## <a name="next-steps"></a>Volgende stappen

Voor meer geavanceerde implementatiescenario's, proberen [implementeren naar Azure met Git](deploy-local-git.md). Op basis van een GIT-implementatie naar Azure kunt versiebeheer, pakket terugzetten en MSBuild.

## <a name="more-resources"></a>Meer bronnen

* [Kudu: Implementeren vanuit een zip-bestand](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file)
* [Referenties voor Azure App Service-implementatie](deploy-ftp.md)
