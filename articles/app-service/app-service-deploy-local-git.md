---
title: Lokale Git-implementatie op de Azure App Service
description: Informatie over het inschakelen van lokale Git-implementatie in Azure App Service.
services: app-service
documentationcenter: 
author: cephalin
manager: cfowler
ms.assetid: ac50a623-c4b8-4dfd-96b2-a09420770063
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/05/2018
ms.author: dariagrigoriu;cephalin
ms.openlocfilehash: 4cbe26055bdbf906223a327ab8cf94bebe9e7998
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/08/2018
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

## <a name="prepare-your-repository"></a>Voorbereiden van uw opslagplaats

Zorg ervoor dat de hoofdmap van uw opslagplaats de juiste bestanden in uw project heeft.

| Runtime | Root directory-bestanden |
|-|-|
| ASP.NET (alleen Windows) | _*.sln_, _*.csproj_, of _default.aspx_ |
| ASP.NET Core | _*.sln_ of _*.csproj_ |
| PHP | _index.php_ |
| Ruby (alleen voor Linux) | _Gemfile_ |
| Node.js | _Server.js_, _app.js_, of _package.json_ met een startscript |
| Python (alleen Windows) | _\*.PY_, _requirements.txt_, of _runtime.txt_ |
| HTML | _default.htm_, _default.html_, _default.asp_, _index.htm_, _index.html_, of  _iisstart.htm_ |
| Webtaken | _\<job_name > / uitvoeren. \<extensie >_ onder _App\_gegevens/taken/continue_ (voor doorlopende webtaken) of _App\_gegevens, taken/geactiveerd_ (voor geactiveerd WebJobs). Zie voor meer informatie [Kudu WebJobs-documentatie](https://github.com/projectkudu/kudu/wiki/WebJobs) |
| Functies | Zie [continue implementatie voor Azure Functions](../azure-functions/functions-continuous-deployment.md#continuous-deployment-requirements). |

U kunt opnemen voor het aanpassen van uw implementatie, een _.deployment_ bestand in de hoofdmap van de opslagplaats. Zie voor meer informatie [implementaties aanpassen](https://github.com/projectkudu/kudu/wiki/Customizing-deployments) en [aangepaste implementatiescript](https://github.com/projectkudu/kudu/wiki/Custom-Deployment-Script).

> [!NOTE]
> Zorg ervoor dat `git commit` alle wijzigingen die u wilt implementeren.
>
>

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

[!INCLUDE [Configure a deployment user](../../includes/configure-deployment-user.md)]

## <a name="enable-git-for-your-app"></a>Git inschakelen voor uw app

Inschakelen van Git-implementatie voor een bestaande App Service-app [ `az webapp deployment source config-local-git` ](/cli/azure/webapp/deployment/source?view=azure-cli-latest#az_webapp_deployment_source_config_local_git) in de Cloud-Shell.

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

## <a name="deploy-your-project"></a>Uw project implementeren

Voeg, eenmaal terug in het _lokale terminalvenster_, een externe Azure-instantie toe aan uw lokale Git-opslagplaats. Vervang  _\<url >_ met de URL van de externe Git die u hebt gekregen [Git inschakelen voor uw app](#enable-git-for-you-app).

```bash
git remote add azure <url>
```

Push naar de externe Azure-instantie om uw app te implementeren met de volgende opdracht. Zorg er als u om een wachtwoord wordt gevraagd voor dat u het wachtwoord dat u hebt gemaakt bij [Een implementatiegebruiker configureren](#configure-a-deployment-user) gebruikt en niet het wachtwoord dat u gebruikt om u aan te melden bij Azure Portal.

```bash
git push azure master
```

Mogelijk ziet u runtime-specifieke automatiseren met behulp van de uitvoer, zoals MSBuild voor ASP.NET-, `npm install` voor Node.js, en `pip install` voor Python. 

Zodra de implementatie is voltooid, uw app in de Azure-portal hebt nu een record van uw `git push` in de **implementatieopties** pagina.

![](./media/app-service-deploy-local-git/deployment_history.png)

Blader naar uw app om te controleren dat de inhoud wordt gedistribueerd.

## <a name="troubleshooting"></a>Problemen oplossen

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
* **Systeemeigen module die geen heeft een binaire distributiepunt voor Windows**:

  * `npm ERR! \cmd "/c" "node-gyp rebuild"\ failed with 1`

      OF
  * `npm ERR! [modulename@version] preinstall: \make || gmake\`

## <a name="additional-resources"></a>Aanvullende resources

* [Project Kudu documentatie](https://github.com/projectkudu/kudu/wiki)
* [Doorlopende implementatie in Azure App Service](app-service-continuous-deployment.md)
