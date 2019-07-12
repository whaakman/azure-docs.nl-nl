---
title: Implementeren vanuit lokale Git-repo - Azure App Service
description: Informatie over het inschakelen van lokale Git-implementatie in Azure App Service.
services: app-service
documentationcenter: ''
author: cephalin
manager: cfowler
ms.assetid: ac50a623-c4b8-4dfd-96b2-a09420770063
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/18/2019
ms.author: dariagrigoriu;cephalin
ms.reviewer: dariac
ms.custom: seodec18
ms.openlocfilehash: fa961e43408fed014be2266c14c7972d39435b97
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67836928"
---
# <a name="local-git-deployment-to-azure-app-service"></a>Lokale Git-implementatie in Azure App Service

In deze gebruiksaanwijzing ziet u hoe u uw app te implementeren [Azure App Service](overview.md) vanuit een Git-opslagplaats op uw lokale computer.

## <a name="prerequisites"></a>Vereisten

De stappen in deze gebruiksaanwijzing:

- [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]
  
- [Installeer Git](https://www.git-scm.com/downloads).
  
- Beschikken over een lokale Git-opslagplaats met code die u wilt implementeren. Voer de volgende opdracht in uw lokale terminalvenster voor het downloaden van de opslagplaats van een voorbeeld:
  
  ```bash
  git clone https://github.com/Azure-Samples/nodejs-docs-hello-world.git
  ```

[!INCLUDE [Prepare repository](../../includes/app-service-deploy-prepare-repo.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="deploy-with-kudu-build-server"></a>Met Kudu maken server implementeren

De eenvoudigste manier om in te schakelen van lokale Git-implementatie voor uw app met de Kudu-App-build-server is met Azure Cloud Shell. 

### <a name="configure-a-deployment-user"></a>Een implementatiegebruiker configureren

[!INCLUDE [Configure a deployment user](../../includes/configure-deployment-user-no-h.md)]

### <a name="get-the-deployment-url"></a>De implementatie-URL ophalen

Als u de URL om in te schakelen van lokale Git-implementatie voor een bestaande app, voer [ `az webapp deployment source config-local-git` ](/cli/azure/webapp/deployment/source?view=azure-cli-latest#az-webapp-deployment-source-config-local-git) in de Cloud Shell. Vervang \<app-naam > en \<group-name > met de namen van uw app en de Azure-resourcegroep.

```azurecli-interactive
az webapp deployment source config-local-git --name <app-name> --resource-group <group-name>
```

Of Voer voor het maken van een nieuwe app met Git-functionaliteit [ `az webapp create` ](/cli/azure/webapp?view=azure-cli-latest#az-webapp-create) in de Cloud Shell met de `--deployment-local-git` parameter. Vervang \<app-naam >, \<-naam >, en \<plan-name > met de namen voor uw nieuwe Git-app, de Azure-resourcegroep en de Azure App Service-plan.

```azurecli-interactive
az webapp create --name <app-name> --resource-group <group-name> --plan <plan-name> --deployment-local-git
```

De opdracht retourneert een URL, zoals: `https://<deployment-username>@<app-name>.scm.azurewebsites.net/<app-name>.git`. Deze URL gebruiken om uw app in de volgende stap te implementeren.

In plaats van deze URL op accountniveau, kunt u ook een lokale Git inschakelen met behulp van de referenties op app-niveau. Deze referenties voor elke app wordt automatisch gegenereerd door Azure App Service. 

De app-referenties ophalen door het uitvoeren van de volgende opdracht uit in de Cloud Shell. Vervang \<app-naam > en \<group-name > met de naam van uw app en de naam van de Azure-resourcegroep.

```azurecli-interactive
az webapp deployment list-publishing-credentials --name <app-name> --resource-group <group-name> --query scmUri --output tsv
```

Gebruik de URL die wordt geretourneerd voor het implementeren van uw app in de volgende stap.

### <a name="deploy-the-web-app"></a>De web-app implementeren

1. Open een lokale terminalvenster op uw lokale Git-opslagplaats en toevoegen van een externe Azure-instantie. In de volgende opdracht, vervangt u \<url > met de implementatie gebruikersspecifieke URL of app-specifieke URL die u hebt verkregen in de vorige stap.
   
   ```bash
   git remote add azure <url>
   ```
   
1. Push naar externe met de Azure `git push azure master`. 
   
1. In de **Git Credential Manager** venster, Geef uw [implementatie gebruikerswachtwoord](#configure-a-deployment-user), niet uw Azure-aanmelden-wachtwoord.
   
1. Controleer de uitvoer. Runtime-specifieke-automatisering, zoals MSBuild voor ASP.NET, ziet u mogelijk `npm install` voor Node.js, en `pip install` voor Python. 
   
1. Blader naar uw app in Azure portal om te controleren dat de inhoud wordt geïmplementeerd.

## <a name="deploy-with-azure-pipelines-builds"></a>Implementeren met Azure-pijplijnen builds

Als uw account de vereiste machtigingen beschikt heeft, kunt u Azure-pijplijnen (Preview) instellen voor het inschakelen van lokale Git-implementatie voor uw app. 

- Uw Azure-account moet machtigingen voor het schrijven naar Azure Active Directory en het maken van een service hebben. 
  
- Uw Azure-account moet hebben de **eigenaar** rol in uw Azure-abonnement.

- U moet een beheerder in het Azure DevOps-project dat u wilt gebruiken.

Lokale Git-implementatie voor uw app met Azure-pijplijnen (Preview) inschakelen:

1. Navigeer naar de pagina met uw Azure App Service-app in de [Azure-portal](https://portal.azure.com), en selecteer **Implementatiecentrum** in het menu links.
   
1. Op de **Implementatiecentrum** weergeeft, schakelt **lokale Git**, en selecteer vervolgens **doorgaan**. 
   
   ![Selecteer lokale Git, en selecteer vervolgens doorgaan](media/app-service-deploy-local-git/portal-enable.png)
   
1. Op de **Build-provider** weergeeft, schakelt **Azure pijplijnen (Preview)** , en selecteer vervolgens **doorgaan**. 
   
   ![Selecteer Azure pijplijnen (Preview) en selecteer vervolgens doorgaan.](media/app-service-deploy-local-git/pipeline-builds.png)

1. Op de **configureren** pagina, configureren van een nieuwe Azure DevOps-organisatie, of geef een bestaande organisatie, en selecteer vervolgens **doorgaan**.
   
   > [!NOTE]
   > Als uw bestaande Azure DevOps-organisatie niet wordt weergegeven, moet u wellicht koppelen aan uw Azure-abonnement. Zie voor meer informatie, [definiëren van uw CD release-pijplijn](/azure/devops/pipelines/apps/cd/deploy-webdeploy-webapps#cd).
   
1. Afhankelijk van uw App Service-plan [prijscategorie](https://azure.microsoft.com/pricing/details/app-service/plans/), ziet u mogelijk een **implementeren voor fasering** pagina. Kies of u wilt [inschakelen implementatiesites](deploy-staging-slots.md), en selecteer vervolgens **doorgaan**.
   
1. Op de **samenvatting** pagina, Controleer de instellingen en selecteer vervolgens **voltooien**.
   
1. Wanneer de pijplijn Azure klaar is, kopieert u de URL van de Git-opslagplaats van de **Implementatiecentrum** pagina te gebruiken in de volgende stap. 
   
   ![Kopieer de URL van de Git-opslagplaats](media/app-service-deploy-local-git/vsts-repo-ready.png)

1. Toevoegen in uw lokale terminalvenster een externe Azure-instantie aan uw lokale Git-opslagplaats. Vervang in de opdracht \<url > door de URL van de Git-opslagplaats die u hebt verkregen in de vorige stap.
   
   ```bash
   git remote add azure <url>
   ```
   
1. Push naar externe met de Azure `git push azure master`. 
   
1. Op de **Git Credential Manager** pagina, meld u aan met uw gebruikersnaam visualstudio.com. Zie voor andere verificatiemethoden [Services van Azure DevOps-verificatieoverzicht](/vsts/git/auth-overview?view=vsts).
   
1. Zodra de implementatie is voltooid, geven de voortgang van de build op `https://<azure_devops_account>.visualstudio.com/<project_name>/_build`, en de voortgang van de implementatie op `https://<azure_devops_account>.visualstudio.com/<project_name>/_release`.
   
1. Blader naar uw app in Azure portal om te controleren dat de inhoud wordt geïmplementeerd.

[!INCLUDE [What happens to my app during deployment?](../../includes/app-service-deploy-atomicity.md)]

## <a name="troubleshoot-deployment"></a>Problemen met implementatie oplossen

Ziet u mogelijk de volgende algemene foutberichten weergegeven wanneer u Git gebruiken om te publiceren naar een App Service-app in Azure:

|Message|Oorzaak|Oplossing
---|---|---|
|`Unable to access '[siteURL]': Failed to connect to [scmAddress]`|De app niet actief en werkend.|De app te starten in Azure portal. GIT-implementatie is niet beschikbaar wanneer de web-app is gestopt.|
|`Couldn't resolve host 'hostname'`|De adresgegevens voor de externe 'azure' is onjuist.|Gebruik de `git remote -v` opdracht om een lijst van alle remotes, samen met de bijbehorende URL. Controleer of dat de URL voor de externe 'azure' juist is. Indien nodig, verwijderen en opnieuw maken van deze externe met behulp van de juiste URL.|
|`No refs in common and none specified; doing nothing. Perhaps you should specify a branch such as 'master'.`|U hebt opgegeven een vertakking tijdens `git push`, of u dit nog niet hebt ingesteld de `push.default` waarde in de `.gitconfig`.|Voer `git push` opnieuw op te geven van de master-vertakking: `git push azure master`.|
|`src refspec [branchname] does not match any.`|U probeert te pushen naar een vertakking dan master in de 'azure' remote.|Voer `git push` opnieuw op te geven van de master-vertakking: `git push azure master`.|
|`RPC failed; result=22, HTTP code = 5xx.`|Deze fout kan optreden als u probeert een grote git-opslagplaats pushen via HTTPS.|Wijzigen van de git-configuratie op de lokale machine om de `postBuffer` groter. Bijvoorbeeld: `git config --global http.postBuffer 524288000`.|
|`Error - Changes committed to remote repository but your web app not updated.`|U hebt geïmplementeerd op een Node.js-app met een _package.json_ -bestand dat aanvullende vereiste modules specificeert.|Controleer de `npm ERR!` foutberichten voordat u deze fout voor meer context over de fout. Hieronder vindt u de bekende oorzaken van deze fout, en de bijbehorende `npm ERR!` berichten:<br /><br />**Onjuist gevormd package.json-bestand**: `npm ERR! Couldn't read dependencies.`<br /><br />**Systeemeigen module geen een binaire distributie voor Windows**:<br />`npm ERR! \cmd "/c" "node-gyp rebuild"\ failed with 1` <br />of <br />`npm ERR! [modulename@version] vooraf: \make || gmake\`|

## <a name="additional-resources"></a>Aanvullende resources

- [Project Kudu-documentatie](https://github.com/projectkudu/kudu/wiki)
- [Continue implementatie in Azure App Service](deploy-continuous-deployment.md)
- [Voorbeeld: Een web-app maken en code implementeren vanuit een lokale Git-opslagplaats (Azure CLI)](./scripts/cli-deploy-local-git.md?toc=%2fcli%2fazure%2ftoc.json)
- [Voorbeeld: Een web-app maken en code implementeren vanuit een lokale Git-opslagplaats (PowerShell)](./scripts/powershell-deploy-local-git.md?toc=%2fpowershell%2fmodule%2ftoc.json)
