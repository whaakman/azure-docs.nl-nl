---
title: Een Ruby-App maken en implementeren in App Service op Linux | Microsoft Docs
description: Meer informatie over Ruby om apps te maken met App-Service op Linux.
keywords: Azure app service, linux, besturingssystemen, ruby
services: app-service
documentationcenter: 
author: SyntaxC4
manager: cfowler
editor: 
ms.assetid: 6d00c73c-13cb-446f-8926-923db4101afa
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 10/10/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: a54ef1ae40ba6ea9ad604a29c67e41228c0d5946
ms.sourcegitcommit: 62eaa376437687de4ef2e325ac3d7e195d158f9f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/22/2017
---
# <a name="create-a-ruby-app-in-app-service-on-linux"></a>Een Ruby-App maken in App-Service op Linux

[App-Service op Linux](app-service-linux-intro.md) biedt een zeer schaalbaar, zelf patch webhosting-service. Deze snelstartgids ziet u het maken van een eenvoudige Ruby op Rails toepassing u vervolgens implementeren op Azure als een Web-App op Linux.

![Hallo wereld](./media/quickstart-ruby/hello-world-updated.png)

## <a name="prerequisites"></a>Vereisten

* <a href="https://www.ruby-lang.org/en/documentation/installation/#rubyinstaller" target="_blank">Ruby 2.4.1 installeren of hoger</a>
* <a href="https://git-scm.com/" target="_blank">Git installeren</a>

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="download-the-sample"></a>Het voorbeeld downloaden

Voer de volgende opdracht voor het klonen van de opslagplaats van de voorbeeld-app op uw lokale computer in een terminalvenster:

```bash
git clone https://github.com/Azure-Samples/ruby-docs-hello-world
```

## <a name="run-the-application-locally"></a>De toepassing lokaal uitvoeren

De server rails uitvoeren om de toepassing uit te voeren. Wijzig in de *Hallo wereld* directory, en de `rails server` opdracht start de server.

```bash
cd hello-world\bin
rails server
```

Met behulp van uw webbrowser en navigeer naar `http://localhost:3000` naar de app lokaal testen.

![Hallo wereld](./media/quickstart-ruby/hello-world.png)

## <a name="modify-app-to-display-welcome-message"></a>App om weer te geven welkomstbericht wijzigen

Wijzigen van de toepassing en een welkomstbericht worden weergegeven. Eerst moet u een route instellen door het wijzigen van de *~/workspace/ruby-docs-hello-world/config/routes.rb* bestand opnemen van een route met de naam `hello`.

  ```ruby
  Rails.application.routes.draw do
      #For details on the DSL available within this file, see http://guides.rubyonrails.org/routing.html
      root 'application#hello'
  end
  ```

De domeincontroller van de toepassing wijzigen zodat deze het bericht als HTML naar de browser retourneert. 

Open *~/workspace/hello-world/app/controllers/application_controller.rb* voor bewerken. Wijzig de `ApplicationController` klasse om te zoeken, zoals in het volgende voorbeeld:

  ```ruby
  class ApplicationController > ActionController :: base
    protect_from_forgery with: :exception
    def hello
      render html: "Hello, world from Azure Web App on Linux!"
    end
  end
  ```

Uw app is nu geconfigureerd. Met behulp van uw webbrowser en navigeer naar `http://localhost:3000` om te bevestigen dat de startpagina van de hoofdmap.

![Hallo wereld geconfigureerd](./media/quickstart-ruby/hello-world-configured.png)

[!INCLUDE [Try Cloud Shell](../../../includes/cloud-shell-try-it.md)]

[!INCLUDE [Configure deployment user](../../../includes/configure-deployment-user.md)]

## <a name="create-a-ruby-web-app-on-azure"></a>Een Ruby web-app in Azure maken

Een resourcegroep is vereist voor de activa die nodig zijn voor uw web-app bevatten. Voor het maken van een resourcegroep gebruikt de [az groep maken]() opdracht.

```azurecli-interactive
az group create --location westeurope --name myResourceGroup
```

Gebruik de [az appservice-abonnement maken](https://docs.microsoft.com/cli/azure/appservice/plan#az_appservice_plan_create) opdracht voor het maken van een app service-plan voor uw web-app.

```azurecli-interactive
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --is-linux
```

Vervolgens geven de [az webapp maken](https://docs.microsoft.com/cli/azure/webapp) opdracht voor het maken van de web-app die gebruikmaakt van het zojuist gemaakte service-abonnement. U ziet dat de runtime is ingesteld op `ruby|2.3`. Vergeet niet om te vervangen `<app name>` met een unieke app-naam.

```azurecli-interactive
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app name> \
--runtime "ruby|2.3" --deployment-local-git
```

De uitvoer van de opdracht geeft informatie over de nieuwe web-app, alsmede de URL van de implementatie. Het moet eruitzien als met het volgende voorbeeld. Kopieer de URL voor later gebruik in deze zelfstudie.

```bash
https://<deployment user name>@<app name>.scm.azurewebsites.net/<app name>.git
```

Zodra de web-app is gemaakt, een **overzicht** pagina wordt weergegeven. Ga naar het. De volgende welkomstscherm pagina wordt weergegeven:

![Welkomstscherm pagina](./media/quickstart-ruby/splash-page.png)


## <a name="deploy-your-application"></a>Uw toepassing implementeren

Voer de volgende opdrachten voor het implementeren van de lokale toepassing naar uw Azure-website:

```bash
git remote add azure <Git deployment URL from above>
git add -A
git commit -m "Initial deployment commit"
git push azure master
```

Controleer de implementatie op afstand bewerkingen melden. De opdrachten produceren uitvoer die vergelijkbaar is met de volgende tekst:

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

Zodra de implementatie is voltooid, start opnieuw op uw web-app voor de implementatie van kracht te laten met behulp van de [az webapp herstart](https://docs.microsoft.com/cli/azure/webapp#az_webapp_restart) opdracht als volgt te werk:

```azurecli-interactive
az webapp restart --name <app name> --resource-group myResourceGroup
```

Navigeer naar uw site en de resultaten controleren.

```bash
http://<app name>.azurewebsites.net
```

![bijgewerkte web-app](./media/quickstart-ruby/hello-world-updated.png)

> [!NOTE]
> Terwijl de app opnieuw wordt opgestart, probeert te bladeren op de site, resulteert in een HTTP-statuscode `Error 503 Server unavailable`. Het duurt enkele minuten volledig opnieuw te starten.
>

[!INCLUDE [Clean-up section](../../../includes/cli-script-clean-up.md)]

## <a name="next-steps"></a>Volgende stappen

[Op Linux FAQ-Azure App Service](https://docs.microsoft.com/azure/app-service-web/app-service-linux-faq.md)
