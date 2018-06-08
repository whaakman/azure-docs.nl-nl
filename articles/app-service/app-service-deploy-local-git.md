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
ms.openlocfilehash: 88cc9ff4979c2e6a4a14a7d531054c1a842deaf8
ms.sourcegitcommit: 3c3488fb16a3c3287c3e1cd11435174711e92126
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/07/2018
ms.locfileid: "34849800"
---
# <a name="local-git-deployment-to-azure-app-service"></a>Lokale Git-implementatie op de Azure App Service

Deze instructies handleiding laat zien hoe uw code te implementeren [Azure App Service](app-service-web-overview.md) van een Git-opslagplaats op uw lokale computer.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Vereisten

De stappen in deze handleiding instructies:

* [Installeer Git](http://www.git-scm.com/downloads).
* Onderhouden van een lokale Git-opslagplaats met code die u wilt implementeren.

Als u een voorbeeld-opslagplaats wilt volgen, voer de volgende opdracht in uw lokale terminalvenster:

```bash
git clone https://github.com/Azure-Samples/nodejs-docs-hello-world.git
```

[!INCLUDE [Prepare repository](../../includes/app-service-deploy-prepare-repo.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="deploy-from-local-git-with-kudu-builds"></a>Implementeren vanaf de lokale Git met Kudu builds

De eenvoudigste manier om in te schakelen voor uw app met de server van de build Kudu lokale Git-implementatie is de Cloud-Shell gebruiken.

### <a name="create-a-deployment-user"></a>Een implementatiegebruiker maken

[!INCLUDE [Configure a deployment user](../../includes/configure-deployment-user-no-h.md)]

### <a name="enable-local-git-with-kudu"></a>Lokale Git met Kudu inschakelen

Inschakelen van lokale Git-implementatie voor uw app met de server van de build Kudu [ `az webapp deployment source config-local-git` ](/cli/azure/webapp/deployment/source?view=azure-cli-latest#az_webapp_deployment_source_config_local_git) in de Cloud-Shell.

```azurecli-interactive
az webapp deployment source config-local-git --name <app_name> --resource-group <group_name>
```

Als u wilt maken in plaats daarvan een Git-app, voer [ `az webapp create` ](/cli/azure/webapp?view=azure-cli-latest#az_webapp_create) in de Cloud-Shell met de `--deployment-local-git` parameter.

```azurecli-interactive
az webapp create --name <app_name> --resource-group <group_name> --plan <plan_name> --deployment-local-git
```

De `az webapp create` opdracht geeft u iets soortgelijks als in de volgende uitvoer:

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

Voeg, eenmaal terug in het _lokale terminalvenster_, een externe Azure-instantie toe aan uw lokale Git-opslagplaats. Vervang  _\<url >_ met de URL van de externe Git die u hebt gekregen [Git inschakelen voor uw app](#enable-git-for-you-app).

```bash
git remote add azure <url>
```

Push naar de externe Azure-instantie om uw app te implementeren met de volgende opdracht. Zorg er als u om een wachtwoord wordt gevraagd voor dat u het wachtwoord dat u hebt gemaakt bij [Een implementatiegebruiker configureren](#configure-a-deployment-user) gebruikt en niet het wachtwoord dat u gebruikt om u aan te melden bij Azure Portal.

```bash
git push azure master
```

Mogelijk ziet u runtime-specifieke automatiseren met behulp van de uitvoer, zoals MSBuild voor ASP.NET-, `npm install` voor Node.js, en `pip install` voor Python. 

Blader naar uw app om te controleren dat de inhoud wordt gedistribueerd.

## <a name="deploy-from-local-git-with-vsts-builds"></a>Implementeren vanaf de lokale Git met VSTS builds

> [!NOTE]
> Voor App Service voor het maken van de benodigde build en definities in uw account VSTS release, uw Azure-account de rol van moet zijn **eigenaar** in uw Azure-abonnement.
>

Om lokale Git-implementatie voor uw app met de Kudu-build-server, gaat u naar uw app in de [Azure-portal](https://portal.azure.com).

Klik in het linkernavigatievenster van uw app-pagina **Implementatiecentrum** > **lokale Git** > **doorgaan**. 

![](media/app-service-deploy-local-git/portal-enable.png)

Klik op **VSTS continue levering** > **gaan**.

![](media/app-service-deploy-local-git/vsts-build-server.png)

In de **configureren** pagina, configureert u een nieuw VSTS-account of een bestaand account opgeven. Wanneer u klaar bent, klikt u op **doorgaan**.

> [!NOTE]
> Als u gebruiken van een bestaand VSTS account die niet wordt vermeld wilt, moet u [de VSTS-account koppelen aan uw Azure-abonnement](https://github.com/projectkudu/kudu/wiki/Setting-up-a-VSTS-account-so-it-can-deploy-to-a-Web-App).

In de **Test** pagina, kies of u wilt laden tests inschakelen en klik vervolgens op **doorgaan**.

Afhankelijk van de [prijscategorie](/pricing/details/app-service/plans/) van uw App Service-abonnement u ziet misschien ook een **implementeren voor fasering** pagina. Kies of u wilt inschakelen, implementatiesites en klik vervolgens op **doorgaan**.

In de **samenvatting** pagina, Controleer uw opties en klik op **voltooien**.

Het duurt enkele minuten voor het account VSTS gereed is. Wanneer het klaar is, kopieert u de URL van de Git-opslagplaats in het midden van de implementatie.

![](media/app-service-deploy-local-git/vsts-repo-ready.png)

Voeg, eenmaal terug in het _lokale terminalvenster_, een externe Azure-instantie toe aan uw lokale Git-opslagplaats. Vervang  _\<url >_ met de URL die u in de vorige stap hebt verkregen.

```bash
git remote add vsts <url>
```

Push naar de externe Azure-instantie om uw app te implementeren met de volgende opdracht. Als u wordt gevraagd door Git Referentiebeheer, aanmelden met uw gebruikers visualstudio.com. Zie voor extra verificatiemethoden [VSTS verificatie overzicht](/vsts/git/auth-overview?view=vsts).

```bash
git push vsts master
```

Als de implementatie is voltooid, kunt u de voortgang van de build op vinden `https://<vsts_account>.visualstudio.com/<project_name>/_build` en de voortgang van de implementatie op `https://<vsts_account>.visualstudio.com/<project_name>/_release`.

Blader naar uw app om te controleren dat de inhoud wordt gedistribueerd.

## <a name="troubleshooting-kudu-deployment"></a>Het oplossen van de Kudu-implementatie

Hier volgen algemene fouten of problemen bij het gebruik van Git publiceren naar een App Service-app in Azure:

---
**Symptoom**: `Unable to access '[siteURL]': Failed to connect to [scmAddress]`

**Oorzaak**: deze fout kan optreden als de app niet actief en werkend.

**Resolutie**: de app te starten in de Azure portal. GIT-implementatie is niet beschikbaar wanneer de Web-App is gestopt.

---
**Symptoom**: `Couldn't resolve host 'hostname'`

**Oorzaak**: deze fout kan optreden als de gegevens hebt ingevoerd bij het maken van de 'azure' externe onjuist is.

**Resolutie**: Gebruik de `git remote -v` opdracht om een lijst van alle afstandsbedieningen, samen met de bijbehorende URL. Controleer of de URL voor de externe 'azure' klopt. Indien nodig, verwijderen en opnieuw maken van deze extern met behulp van de juiste URL.

---
**Symptoom**: `No refs in common and none specified; doing nothing. Perhaps you should specify a branch such as 'master'.`

**Oorzaak**: deze fout kan optreden als u een vertakking tijdens geen opgeeft `git push`, of als u dit nog niet hebt ingesteld de `push.default` waarde in `.gitconfig`.

**Resolutie**: Voer `git push` opnieuw, geven de hoofdvertakking. Bijvoorbeeld:

```bash
git push azure master
```

---
**Symptoom**: `src refspec [branchname] does not match any.`

**Oorzaak**: deze fout kan optreden als u probeert te pushen naar een vertakking dan master in de 'azure' externe.

**Resolutie**: Voer `git push` opnieuw, geven de hoofdvertakking. Bijvoorbeeld:

```bash
git push azure master
```

---
**Symptoom**: `RPC failed; result=22, HTTP code = 5xx.`

**Oorzaak**: deze fout kan optreden als u probeert om een grote git-opslagplaats via HTTPS.

**Resolutie**: Wijzig de git-configuratie op de lokale computer naar de postBuffer groter maken

```bash
git config --global http.postBuffer 524288000
```

---
**Symptoom**: `Error - Changes committed to remote repository but your web app not updated.`

**Oorzaak**: deze fout kan optreden als u een Node.js-app met implementeert een _package.json_ -bestand dat Hiermee geeft u aanvullende vereiste modules.

**Resolutie**: aanvullende berichten met 'npm ERR!' voordat u deze fout moet worden vastgelegd en aanvullende context kan bieden over de fout. Hier volgen enkele bekende oorzaken van deze fout en de bijbehorende 'npm ERR!' Bericht:

* **Ongeldige package.json-bestand**: npm ERR! Afhankelijkheden kan niet worden gelezen.
* **Systeemeigen module die een binaire distributiepunt voor Windows heeft geen**:

  * `npm ERR! \cmd "/c" "node-gyp rebuild"\ failed with 1`

      OF
  * `npm ERR! [modulename@version] preinstall: \make || gmake\`

## <a name="additional-resources"></a>Aanvullende resources

* [Project Kudu documentatie](https://github.com/projectkudu/kudu/wiki)
* [Doorlopende implementatie in Azure App Service](app-service-continuous-deployment.md)
* [Voorbeeld: Web-App maken en implementeren van de code van een lokale Git-opslagplaats (Azure CLI)](./scripts/app-service-cli-deploy-local-git.md?toc=%2fcli%2fazure%2ftoc.json)
* [Voorbeeld: Web-App maken en implementeren van de code van een lokale Git-opslagplaats (PowerShell)](./scripts/app-service-powershell-deploy-local-git.md?toc=%2fpowershell%2fmodule%2ftoc.json)
