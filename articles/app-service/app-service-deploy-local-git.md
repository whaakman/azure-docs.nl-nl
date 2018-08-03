---
title: Lokale Git-implementatie op de Azure App Service
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
ms.date: 06/05/2018
ms.author: dariagrigoriu;cephalin
ms.openlocfilehash: 0f6a0e2fe3aa632137392efe806aaab265eedf10
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39435539"
---
# <a name="local-git-deployment-to-azure-app-service"></a>Lokale Git-implementatie op de Azure App Service

In deze gebruiksaanwijzing ziet u hoe u implementeert uw code wordt [Azure App Service](app-service-web-overview.md) vanuit een Git-opslagplaats op uw lokale computer.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Vereisten

De stappen in deze gebruiksaanwijzing:

* [Installeer Git](http://www.git-scm.com/downloads).
* Onderhouden een lokale Git-opslagplaats met code die u wilt implementeren.

Als u een opslagplaats met voorbeeld wilt volgen, voer de volgende opdracht in uw lokale terminalvenster:

```bash
git clone https://github.com/Azure-Samples/nodejs-docs-hello-world.git
```

[!INCLUDE [Prepare repository](../../includes/app-service-deploy-prepare-repo.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="deploy-from-local-git-with-kudu-builds"></a>Implementeren vanuit lokale Git met Kudu builds

De eenvoudigste manier om in te schakelen van lokale Git-implementatie voor uw app met de Kudu-build-server is met de Cloud Shell.

### <a name="create-a-deployment-user"></a>Een implementatiegebruiker maken

[!INCLUDE [Configure a deployment user](../../includes/configure-deployment-user-no-h.md)]

### <a name="enable-local-git-with-kudu"></a>Lokale Git met Kudu inschakelen

Als u lokale Git-implementatie voor uw app met de Kudu-build-server, voert u [ `az webapp deployment source config-local-git` ](/cli/azure/webapp/deployment/source?view=azure-cli-latest#az-webapp-deployment-source-config-local-git) in de Cloud Shell.

```azurecli-interactive
az webapp deployment source config-local-git --name <app_name> --resource-group <group_name>
```

Voer voor het maken een app met Git in plaats daarvan [ `az webapp create` ](/cli/azure/webapp?view=azure-cli-latest#az-webapp-create) in de Cloud Shell met de `--deployment-local-git` parameter.

```azurecli-interactive
az webapp create --name <app_name> --resource-group <group_name> --plan <plan_name> --deployment-local-git
```

De `az webapp create` opdracht geeft u er ongeveer uitzien als in de volgende uitvoer:

```json
Local git is configured with url of 'https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git'
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app_name>.azurewebsites.net",
  "deploymentLocalGitUrl": "https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git",
  "enabled": true,
  < JSON data removed for brevity. >
}
```

### <a name="deploy-your-project"></a>Uw project implementeren

Voeg, eenmaal terug in het _lokale terminalvenster_, een externe Azure-instantie toe aan uw lokale Git-opslagplaats. Vervang  _\<url >_ met de URL van de externe Git-instantie die u hebt verkregen via [Git inschakelen voor uw app](#enable-git-for-you-app).

```bash
git remote add azure <url>
```

Push naar de externe Azure-instantie om uw app te implementeren met de volgende opdracht. Zorg er als u om een wachtwoord wordt gevraagd voor dat u het wachtwoord dat u hebt gemaakt bij [Een implementatiegebruiker configureren](#configure-a-deployment-user) gebruikt en niet het wachtwoord dat u gebruikt om u aan te melden bij Azure Portal.

```bash
git push azure master
```

Runtime-specifieke automatisering in de uitvoer, zoals MSBuild voor ASP.NET, ziet u mogelijk `npm install` voor Node.js, en `pip install` voor Python. 

Blader naar uw app om te controleren dat de inhoud wordt geïmplementeerd.

## <a name="deploy-from-local-git-with-vsts-builds"></a>Implementeren vanuit lokale Git met VSTS builds

> [!NOTE]
> Voor App Service voor het maken van de benodigde build en release-definities in uw VSTS-account, moet uw Azure-account de rol van **eigenaar** in uw Azure-abonnement.
>

Om in te schakelen lokale Git-implementatie voor uw app met de Kudu-build-server, gaat u naar uw app in de [Azure-portal](https://portal.azure.com).

In het linkernavigatievenster van de app-pagina, klikt u op **Implementatiecentrum** > **lokale Git** > **doorgaan**. 

![](media/app-service-deploy-local-git/portal-enable.png)

Klik op **VSTS continue levering** > **blijven**.

![](media/app-service-deploy-local-git/vsts-build-server.png)

In de **configureren** pagina, configureren van een nieuwe VSTS-account of een bestaand account opgeven. Wanneer u klaar bent, klikt u op **doorgaan**.

> [!NOTE]
> Als u gebruiken van een bestaande VSTS-account dat niet wordt vermeld wilt, moet u [het VSTS-account koppelen aan uw Azure-abonnement](https://github.com/projectkudu/kudu/wiki/Setting-up-a-VSTS-account-so-it-can-deploy-to-a-Web-App).

In de **Test** pagina, kies of u wilt inschakelen load tests uit en klik vervolgens op **doorgaan**.

Afhankelijk van de [prijscategorie](/pricing/details/app-service/plans/) van uw App Service-plan, ziet u mogelijk ook een **implementeren voor fasering** pagina. Kies of u wilt inschakelen implementatiesites gebruiken en klik vervolgens op **doorgaan**.

In de **samenvatting** pagina, Controleer uw opties en klik op **voltooien**.

Het duurt enkele minuten voor het VSTS-account klaar is. Wanneer dit klaar is, kopieert u de URL van de Git-opslagplaats in het midden van de implementatie.

![](media/app-service-deploy-local-git/vsts-repo-ready.png)

Voeg, eenmaal terug in het _lokale terminalvenster_, een externe Azure-instantie toe aan uw lokale Git-opslagplaats. Vervang  _\<url >_ met de URL die u hebt verkregen via de laatste stap.

```bash
git remote add vsts <url>
```

Push naar de externe Azure-instantie om uw app te implementeren met de volgende opdracht. Wanneer u hierom wordt gevraagd door Git Credential Manager, meldt u zich aan met uw gebruiker visualstudio.com. Zie voor aanvullende verificatiemethoden [VSTS authenticatiesystemen overzicht](/vsts/git/auth-overview?view=vsts).

```bash
git push vsts master
```

Als de implementatie is voltooid, kunt u de voortgang van de build op vinden `https://<vsts_account>.visualstudio.com/<project_name>/_build` en de voortgang van de implementatie op `https://<vsts_account>.visualstudio.com/<project_name>/_release`.

Blader naar uw app om te controleren dat de inhoud wordt geïmplementeerd.

[!INCLUDE [What happens to my app during deployment?](../../includes/app-service-deploy-atomicity.md)]

## <a name="troubleshooting-kudu-deployment"></a>Problemen met Kudu-implementatie oplossen

Hier volgen algemene fouten of problemen bij het publiceren naar een App Service-app in Azure met behulp van Git:

---
**Symptoom**: `Unable to access '[siteURL]': Failed to connect to [scmAddress]`

**Oorzaak**: deze fout kan optreden als de app niet actief en werkend.

**Resolutie**: de app in Azure portal te starten. GIT-implementatie is niet beschikbaar wanneer de Web-App is gestopt.

---
**Symptoom**: `Couldn't resolve host 'hostname'`

**Oorzaak**: deze fout kan optreden als de gegevens hebt ingevoerd bij het maken van de 'azure' remote onjuist is.

**Resolutie**: Gebruik de `git remote -v` opdracht om een lijst van alle remotes, samen met de bijbehorende URL. Controleer of dat de URL voor de externe 'azure' juist is. Indien nodig, verwijderen en opnieuw maken van deze externe met behulp van de juiste URL.

---
**Symptoom**: `No refs in common and none specified; doing nothing. Perhaps you should specify a branch such as 'master'.`

**Oorzaak**: deze fout kan optreden als u geen dat een vertakking tijdens opgeeft `git push`, of als u dit nog niet hebt ingesteld de `push.default` waarde in de `.gitconfig`.

**Resolutie**: uitvoeren `git push` opnieuw op te geven van de master-vertakking. Bijvoorbeeld:

```bash
git push azure master
```

---
**Symptoom**: `src refspec [branchname] does not match any.`

**Oorzaak**: deze fout kan optreden als u probeert te pushen naar een vertakking dan master in de 'azure' remote.

**Resolutie**: uitvoeren `git push` opnieuw op te geven van de master-vertakking. Bijvoorbeeld:

```bash
git push azure master
```

---
**Symptoom**: `RPC failed; result=22, HTTP code = 5xx.`

**Oorzaak**: deze fout kan optreden als u probeert een grote git-opslagplaats pushen via HTTPS.

**Resolutie**: wijzigen van de git-configuratie op de lokale computer naar de postBuffer groter maken

```bash
git config --global http.postBuffer 524288000
```

---
**Symptoom**: `Error - Changes committed to remote repository but your web app not updated.`

**Oorzaak**: deze fout kan optreden als u een Node.js-app met implementeert een _package.json_ -bestand dat aanvullende vereiste modules specificeert.

**Resolutie**: extra berichten met "npm ERR!" voordat u deze fout moet worden vastgelegd en kunt aanvullende context te bieden over de fout. Hier volgen enkele bekende oorzaken van deze fout en de bijbehorende 'npm ERR!" Bericht:

* **Onjuist gevormd package.json-bestand**: npm ERR! Afhankelijkheden kan niet worden gelezen.
* **Systeemeigen module die een binaire distributie voor Windows geen**:

  * `npm ERR! \cmd "/c" "node-gyp rebuild"\ failed with 1`

      OF
  * `npm ERR! [modulename@version] preinstall: \make || gmake\`

## <a name="additional-resources"></a>Aanvullende resources

* [Project Kudu-documentatie](https://github.com/projectkudu/kudu/wiki)
* [Continue implementatie in Azure App Service](app-service-continuous-deployment.md)
* [Voorbeeld: Web-App maken en code implementeren vanuit een lokale Git-opslagplaats (Azure CLI)](./scripts/app-service-cli-deploy-local-git.md?toc=%2fcli%2fazure%2ftoc.json)
* [Voorbeeld: Web-App maken en code implementeren vanuit een lokale Git-opslagplaats (PowerShell)](./scripts/app-service-powershell-deploy-local-git.md?toc=%2fpowershell%2fmodule%2ftoc.json)
