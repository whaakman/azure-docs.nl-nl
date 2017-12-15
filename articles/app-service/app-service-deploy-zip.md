---
title: Uw app implementeren in Azure App Service met een ZIP-bestand | Microsoft Docs
description: Informatie over het implementeren van uw app in Azure App Service met een ZIP-bestand.
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
ms.date: 12/05/2017
ms.author: cephalin;sisirap
ms.openlocfilehash: a0e4df0ef0a1c873f1efcac1d8dbfe3cada18218
ms.sourcegitcommit: 0e4491b7fdd9ca4408d5f2d41be42a09164db775
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2017
---
# <a name="deploy-your-app-to-azure-app-service-with-a-zip-file"></a>Uw app implementeren in Azure App Service met een ZIP-bestand

Dit artikel laat zien hoe u uw web-app te implementeren met een ZIP-bestand [Azure App Service](app-service-web-overview.md). 

Deze implementatie van het ZIP-bestand gebruikt dezelfde Kudu service die bevoegdheden continue integratie-implementaties. Kudu ondersteunt de volgende functionaliteit voor de implementatie van het ZIP-bestand: 

- Verwijdering van bestanden die zijn overgebleven van een eerdere implementatie.
- Optie voor het proces voor het bouwen van standaard, waaronder pakket herstellen inschakelen.
- [Implementatieaanpassing](https://github.com/projectkudu/kudu/wiki/Configurable-settings#repository-and-deployment-related-settings), waaronder de implementatiescripts uitgevoerd.  
- Implementatielogboeken van de. 

Zie voor meer informatie [Kudu documentatie](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file).

## <a name="prerequisites"></a>Vereisten

De stappen in dit artikel:

* [Een App Service-app maken](/azure/app-service/), of gebruik een app die u hebt gemaakt voor een andere zelfstudie.

## <a name="create-a-project-zip-file"></a>Een ZIP-bestand van het project maken

>[!NOTE]
> Als u de bestanden in een ZIP-bestand hebt gedownload, eerst de bestanden uitpakken. Bijvoorbeeld, als u een ZIP-bestand hebt gedownload van GitHub, u kunt geen implementeren dat bestand als-is. GitHub voegt extra geneste mappen, niet met App Service werken. 
>

Navigeer naar de hoofdmap van uw app-project in een lokale terminalvenster. 

Deze map moet bevatten de post-bestand naar uw web-app zoals _index.html_, _index.php_, en _app.js_. Het kan ook bevatten management pakketbestanden zoals _project.json_, _composer.json_, _package.json_, _bower.json_, en _requirements.txt_.

Maak een ZIP-archief van alles in uw project. De volgende opdracht maakt gebruik van het hulpprogramma standaard in de terminal:

```
# Bash
zip -r <file-name>.zip .

# PowerShell
Compress-Archive -Path * -DestinationPath <file-name>.zip
``` 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="upload-zip-file-to-cloud-shell"></a>ZIP-bestand uploaden naar Cloud Shell

Als u kiest in plaats daarvan de Azure CLI uitvoeren vanaf uw lokale computer, moet u deze stap overslaan.

Volg de stappen hier om uw ZIP-bestand uploadt naar de Cloud-Shell. 

[!INCLUDE [app-service-web-upload-zip.md](../../includes/app-service-web-upload-zip-no-h.md)]

Zie voor meer informatie [behouden bestanden in de Azure-Cloud-Shell](../cloud-shell/persisting-shell-storage.md).

## <a name="deploy-zip-file"></a>Het ZIP-bestand implementeren

Het geüploade ZIP-bestand naar uw web-app implementeren met behulp van de [az webapp implementatie bron config-zip](/cli/azure/webapp/deployment/source?view=azure-cli-latest#az_webapp_deployment_source_config_zip) opdracht. Als u geen gebruik van Cloud-Shell te kiezen, controleert u of uw versie van Azure CLI 2.0.21 of hoger. Om te zien welke versie u hebt, voer `az --version` opdracht in het lokale terminalvenster. 

Het volgende voorbeeld implementeert u het ZIP-bestand dat u hebt geüpload. Wanneer u een lokale installatie van Azure CLI, geef het pad naar uw lokale ZIP-bestand voor `--src`.   

```azurecli-interactive
az webapp deployment source config-zip --resource-group myResouceGroup --name <app_name> --src clouddrive/<filename>.zip
```

Met deze opdracht implementeert de bestanden en mappen van het ZIP-bestand van het bestand in de standaardmap voor de App Service-toepassing (`\home\site\wwwroot`) en het opnieuw opstarten van de app. Als een aanvullende aangepaste buildproces is geconfigureerd, wordt deze ook uitgevoerd. Zie voor meer informatie [Kudu documentatie](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file).

De lijst van implementaties voor deze app wilt weergeven, moet u de REST-API's (Zie volgende sectie). 

[!INCLUDE [app-service-deploy-zip-push-rest](../../includes/app-service-deploy-zip-push-rest.md)]  

## <a name="next-steps"></a>Volgende stappen

Probeer meer geavanceerde implementatiescenario's voor [implementeren naar Azure met Git](app-service-deploy-local-git.md). Implementatie op basis van GIT in Azure kunt versiebeheer, pakket herstellen en MSBuild.

## <a name="more-resources"></a>Meer bronnen

* [Kudu: Implementeren vanaf een zip-bestand](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file)
* [Referenties voor Azure App Service-implementatie](app-service-deploy-ftp.md)
