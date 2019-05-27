---
title: Ruby-apps - Azure App Service configureren
description: In deze zelfstudie beschrijft opties voor het ontwerpen en Ruby-apps configureren voor Azure App Service on Linux.
services: app-service\web
documentationcenter: ''
author: cephalin
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 03/28/2019
ms.author: astay;cephalin;kraigb
ms.custom: seodec18
ms.openlocfilehash: 412efac3742acf7ad1cdc3d08f9d90c4d39bad3e
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2019
ms.locfileid: "65956112"
---
# <a name="configure-a-linux-ruby-app-for-azure-app-service"></a>Een Linux-Ruby-app configureren voor Azure App Service

Dit artikel wordt beschreven hoe [Azure App Service](app-service-linux-intro.md) Ruby-apps en hoe u kunt het gedrag van App Service wanneer dat nodig is uitgevoerd. Ruby-apps moeten worden geïmplementeerd met alle de vereiste [pip](https://pypi.org/project/pip/) modules.

Deze handleiding bevat de belangrijkste concepten en instructies voor het Ruby-ontwikkelaars die gebruikmaken van een ingebouwde Linux-container in App Service. Als u Azure App Service nog nooit hebt gebruikt, moet u de [Ruby snelstartgids](quickstart-ruby.md) en [Ruby met PostgreSQL zelfstudie](tutorial-ruby-postgres-app.md) eerste.

## <a name="show-ruby-version"></a>Versie van Ruby weergeven

Om weer te geven van de huidige versie van Ruby, voert u de volgende opdracht uit de [Cloud Shell](https://shell.azure.com):

```azurecli-interactive
az webapp config show --resource-group <resource-group-name> --name <app-name> --query linuxFxVersion
```

Om weer te geven voor alle ondersteunde versies van Ruby, voert u de volgende opdracht uit de [Cloud Shell](https://shell.azure.com):

```azurecli-interactive
az webapp list-runtimes --linux | grep RUBY
```

U kunt een niet-ondersteunde versie van Ruby uitvoeren door het bouwen van uw eigen containerinstallatiekopie in plaats daarvan. Voor meer informatie raadpleegt u [Een aangepaste Docker-installatiekopie gebruiken](tutorial-custom-docker-image.md).

## <a name="set-ruby-version"></a>Ruby-versie instellen

Voer de volgende opdracht uit de [Cloud Shell](https://shell.azure.com) in te stellen van de versie van Ruby op 2.3:

```azurecli-interactive
az webapp config set --resource-group <resource-group-name> --name <app-name> --linux-fx-version "RUBY|2.3"
```

> [!NOTE]
> Als u fouten die vergelijkbaar is met de volgende tijdens de implementatie wordt weergegeven:
> ```
> Your Ruby version is 2.3.3, but your Gemfile specified 2.3.1
> ```
> of
> ```
> rbenv: version `2.3.1' is not installed
> ```
> Dit betekent dat de versie van Ruby geconfigureerd in uw project is anders dan de versie die geïnstalleerd in de container die u uitvoert (`2.3.3` in het bovenstaande voorbeeld). Schakel in het bovenstaande voorbeeld beide *Gemfile* en *.ruby-versie* en controleer of de versie van Ruby is niet ingesteld of is ingesteld op de versie die geïnstalleerd in de container die u uitvoert (`2.3.3` in de bovenstaande voorbeeld).

## <a name="access-environment-variables"></a>Toegang tot omgevingsvariabelen

In App Service, kunt u [app-instellingen](../configure-common.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#configure-app-settings) buiten de app-code. Vervolgens kunt u ze met behulp van de standaard openen [ENV ['< pad-naam >']](https://ruby-doc.org/core-2.3.3/ENV.html) patroon. Voor toegang tot bijvoorbeeld de app-instelling `WEBSITE_SITE_NAME` gebruikt u de volgende code:

```ruby
ENV['WEBSITE_SITE_NAME']
```

## <a name="customize-deployment"></a>Implementatie aanpassen

Wanneer u implementeert een [Git-opslagplaats](../deploy-local-git.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json), of een [Zip-pakket](../deploy-zip.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json) met buildprocessen is ingeschakeld, de implementatie-engine (Kudu) automatisch de volgende stappen na de implementatie wordt standaard uitgevoerd:

1. Controleer of een *Gemfile* bestaat.
1. Voer `bundle clean` uit. 
1. Voer `bundle install --path "vendor/bundle"` uit.
1. Voer `bundle package` naar pakket gems naar leverancier/cache-map.

### <a name="use---without-flag"></a>Gebruik--zonder de vlag

Om uit te voeren `bundle install` met de [--zonder](https://bundler.io/man/bundle-install.1.html) markeren, stelt u de `BUNDLE_WITHOUT` [app-instelling](../configure-common.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#configure-app-settings) naar een door komma's gescheiden lijst met groepen. Bijvoorbeeld, de volgende opdracht wordt het op `development,test`.

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings BUNDLE_WITHOUT="development,test"
```

Als deze instelling is gedefinieerd, wordt de implementatie-engine wordt uitgevoerd `bundle install` met `--without $BUNDLE_WITHOUT`.

### <a name="precompile-assets"></a>Vooraf compileren van assets

De stappen na de implementatie niet vooraf compileren van assets standaard. Om in te schakelen Precompilatie van de asset, stel de `ASSETS_PRECOMPILE` [app-instelling](../configure-common.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#configure-app-settings) naar `true`. Vervolgens de opdracht `bundle exec rake --trace assets:precompile` aan het einde van de stappen na de implementatie wordt uitgevoerd. Bijvoorbeeld:

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings ASSETS_PRECOMPILE=true
```

Zie voor meer informatie, [leveren van statische assets](#serve-static-assets).

## <a name="customize-start-up"></a>Opstarten aanpassen

De Ruby-container wordt standaard de Rails-server gestart in de volgende reeks (Zie voor meer informatie de [opstarten script](https://github.com/Azure-App-Service/ruby/blob/master/2.3.8/startup.sh)):

1. Genereren van een [secret_key_base](https://edgeguides.rubyonrails.org/security.html#environmental-security) waarde, als een niet al bestaat. Deze waarde is vereist voor de app wordt uitgevoerd in productiemodus.
1. Stel de `RAILS_ENV` omgevingsvariabele `production`.
1. Verwijder *.pid* -bestand in de *tmp/procesnamen* directory alleen nog door een eerder gestarte Rails-server.
1. Controleer of alle afhankelijkheden zijn geïnstalleerd. Als dit niet het geval is, probeert gems installeren vanaf de lokale *leverancier/cache* directory.
1. Voer `rails server -e $RAILS_ENV` uit.

U kunt het proces opstarten in de volgende manieren aanpassen:

- [Leveren van statische assets](#serve-static-assets)
- [Uitvoeren in de modus voor niet-productie](#run-in-non-production-mode)
- [Secret_key_base handmatig instellen](#set-secret_key_base-manually)

### <a name="serve-static-assets"></a>Leveren van statische assets

De Rails-server in de Ruby-container wordt uitgevoerd in de productiemodus standaard, en [wordt ervan uitgegaan dat assets vooraf geschreven zijn en worden geleverd door uw webserver](https://guides.rubyonrails.org/asset_pipeline.html#in-production). Voor het leveren van statische elementen uit de Rails-server, moet u twee dingen doen:

- **Vooraf compileren van de assets** - [vooraf compileren van de statische assets lokaal](https://guides.rubyonrails.org/asset_pipeline.html#local-precompilation) en deze handmatig te implementeren. De implementatie-engine verwerkt; het in plaats daarvan ook (Zie [vooraf compileren van assets](#precompile-assets).
- **Inschakelen voor de statische bestanden** : als u wilt leveren van statische elementen uit de Ruby-container, stelt u de `RAILS_SERVE_STATIC_FILES` [stelt de `RAILS_SERVE_STATIC_FILES` app-instelling](../configure-common.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#configure-app-settings) naar `true`. Bijvoorbeeld:

    ```azurecli-interactive
    az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings RAILS_SERVE_STATIC_FILES=true
    ```

### <a name="run-in-non-production-mode"></a>Uitvoeren in de modus voor niet-productie

De Rails-server wordt standaard uitgevoerd in productiemodus. Als u wilt uitvoeren in de Ontwikkelingsmodus, bijvoorbeeld instellen de `RAILS_ENV` [app-instelling](../configure-common.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#configure-app-settings) naar `development`.

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings RAILS_ENV="development"
```

Deze instelling alleen echter zorgt ervoor dat de Rails-server te starten in de Ontwikkelingsmodus die localhost aanvragen alleen accepteert en is niet toegankelijk buiten de container. Voor het accepteren van aanvragen van externe clients, stel de `APP_COMMAND_LINE` [app-instelling](../configure-common.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#configure-app-settings) naar `rails server -b 0.0.0.0`. Deze app-instelling kunt u een aangepaste opdracht uitvoeren in de Ruby-container. Bijvoorbeeld:

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings APP_COMMAND_LINE="rails server -b 0.0.0.0"
```

### <a name="set-secretkeybase-manually"></a>Secret_key_base handmatig instellen

Gebruik uw eigen `secret_key_base` waarde in plaats van App Service genereren van een voor u, instellen zodat de `SECRET_KEY_BASE` [app-instelling](../configure-common.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#configure-app-settings) met de gewenste waarde. Bijvoorbeeld:

```azurecli-interactive
az webapp config appsettings set --name <app-name> --resource-group <resource-group-name> --settings SECRET_KEY_BASE="<key-base-value>"
```

## <a name="access-diagnostic-logs"></a>Toegang tot diagnostische logboeken

[!INCLUDE [Access diagnostic logs](../../../includes/app-service-web-logs-access-no-h.md)]

## <a name="open-ssh-session-in-browser"></a>SSH-sessie openen in browser

[!INCLUDE [Open SSH session in browser](../../../includes/app-service-web-ssh-connect-builtin-no-h.md)]

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Zelfstudie: Rails-app met PostgreSQL](tutorial-ruby-postgres-app.md)

> [!div class="nextstepaction"]
> [App Service Linux Veelgestelde vragen](app-service-linux-faq.md)