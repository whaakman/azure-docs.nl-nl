---
title: Een Ruby-app maken en implementeren in App Service on Linux | Microsoft Docs
description: Meer informatie over het maken van Ruby-apps met App Service on Linux.
keywords: azure app service, linux, oss, ruby
services: app-service
documentationcenter: ''
author: SyntaxC4
manager: cfowler
editor: ''
ms.assetid: 6d00c73c-13cb-446f-8926-923db4101afa
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 10/10/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: 0707835cf4d6891397579cc41de49fc0462b4521
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/14/2018
ms.locfileid: "34158606"
---
# <a name="create-a-ruby-app-in-app-service-on-linux"></a>Een Ruby-app maken in App Service on Linux

[Azure App Service in Linux](app-service-linux-intro.md) biedt een uiterst schaalbare webhostingservice met self-patchfunctie. Deze snelstart laat zien hoe u een eenvoudige [Ruby on Rails](https://rubyonrails.org/)-toepassing maakt die als een web-app kan worden geïmplementeerd in Azure in Linux.

![Hello-world](./media/quickstart-ruby/hello-world-updated.png)

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Vereisten

* <a href="https://www.ruby-lang.org/en/documentation/installation/#rubyinstaller" target="_blank">Ruby 2.3 of hoger installeren</a>
* <a href="https://git-scm.com/" target="_blank">Git installeren</a>

## <a name="download-the-sample"></a>Het voorbeeld downloaden

Voer in een terminalvenster de volgende opdracht uit om de opslagplaats van de voorbeeld-app te klonen op uw lokale computer:

```bash
git clone https://github.com/Azure-Samples/ruby-docs-hello-world
```

## <a name="run-the-application-locally"></a>De toepassing lokaal uitvoeren

De Rails-server moet actief zijn, anders werkt de toepassing niet. Ga naar de map `hello-world` en gebruik de opdracht `rails server` om de server te starten.

```bash
cd hello-world\bin
rails server
```

Navigeer naar `http://localhost:3000` met uw webbrowser om de app lokaal te testen.

![Hello World geconfigureerd](./media/quickstart-ruby/hello-world-configured.png)

[!INCLUDE [Try Cloud Shell](../../../includes/cloud-shell-try-it.md)]

[!INCLUDE [Configure deployment user](../../../includes/configure-deployment-user.md)]

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group-linux.md)]

[!INCLUDE [Create app service plan](../../../includes/app-service-web-create-app-service-plan-linux.md)]

## <a name="create-a-web-app"></a>Een webtoepassing maken

[!INCLUDE [Create web app](../../../includes/app-service-web-create-web-app-ruby-linux-no-h.md)] 

Blader naar de site om uw nieuwe web-app met de ingebouwde installatiekopie te bekijken. Vervang _&lt;app-naam>_ door de naam van uw web-app.

```bash
http://<app_name>.azurewebsites.net
```

Zo zou uw nieuwe web-app er moeten uitzien:

![Welkomstpagina](./media/quickstart-ruby/splash-page.png)

## <a name="deploy-your-application"></a>Uw toepassing implementeren

Voer de volgende opdrachten uit om de lokale toepassing in uw Azure-website te implementeren:

```bash
git remote add azure <Git deployment URL from above>
git add -A
git commit -m "Initial deployment commit"
git push azure master
```

Controleer of de externe implementatiebewerkingen met succes voltooid zijn. De uitvoer van de opdrachten moet er ongeveer uitzien als de volgende tekst:

```bash
remote: Using sass-rails 5.0.6
remote: Updating files in vendor/cache
remote: Bundle gems are installed into ./vendor/bundle
remote: Updating files in vendor/cache
remote: ~site/repository
remote: Finished successfully.
remote: Running post deployment command(s)...
remote: Deployment successful.
To https://<your web app name>.scm.azurewebsites.net/<your web app name>.git
  579ccb....2ca5f31  master -> master
myuser@ubuntu1234:~workspace/<app name>$
```

Als de implementatie is voltooid, moet u de web-app opnieuw opstarten om de implementatie van kracht te laten worden. Hiertoe voert u de opdracht [`az webapp restart`](/cli/azure/webapp?view=azure-cli-latest#az_webapp_restart) uit, zoals in dit voorbeeld:

```azurecli-interactive
az webapp restart --name <app name> --resource-group myResourceGroup
```

Navigeer naar uw site en controleer de resultaten.

```bash
http://<app name>.azurewebsites.net
```

![bijgewerkte web-app](./media/quickstart-ruby/hello-world-updated.png)

> [!NOTE]
> Als u probeert op de site te bladeren terwijl de app opnieuw wordt opgestart, wordt de HTTP-statuscode `Error 503 Server unavailable` geretourneerd. Het opnieuw opstarten kan een paar minuten duren.
>

[!INCLUDE [Clean-up section](../../../includes/cli-script-clean-up.md)]

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Ruby on Rails met MySQL](tutorial-ruby-mysql-app.md)
