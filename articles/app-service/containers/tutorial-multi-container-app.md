---
title: Een app met meerdere containers maken in Web App for Containers - Azure App Service
description: Informatie over het gebruik van meerdere containers op Azure met Docker Compose, WordPress en MySQL.
keywords: Azure appservice, web-app, linux, docker compose, multicontainer, meerdere containers, web-app voor containers, meerdere containers, container, wordpress, azure db voor mysql, productiedatabase met containers
services: app-service
documentationcenter: ''
author: msangapu
manager: jeconnoc
editor: ''
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/29/2019
ms.author: msangapu
ms.openlocfilehash: d1176d2d70dcd4de3e4287a1973de027c061f291
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67055550"
---
# <a name="tutorial-create-a-multi-container-preview-app-in-web-app-for-containers"></a>Zelfstudie: Een app met meerdere containers (preview) maken in Web App for Containers

Met behulp van [Web App for Containers](app-service-linux-intro.md) kunt u op een flexibele manier Docker-installatiekopieën gebruiken. In deze zelfstudie leert u hoe u een app met meerdere containers maakt met behulp van WordPress en MySQL. U maakt deze zelfstudie in Cloud Shell af, maar u kunt deze opdrachten ook lokaal uitvoeren met het opdrachtregelhulpprogramma [Azure CLI](/cli/azure/install-azure-cli) (2.0.32 of later).

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een Docker Compose-configuratie converteren die kan worden gebruikt voor Web App for Containers
> * Een app met meerdere containers implementeren in Azure
> * Toepassingsinstellingen toevoegen
> * Permanente opslag voor uw containers gebruiken
> * Verbinding maken met Azure Database for MySQL
> * Problemen oplossen

[!INCLUDE [Free trial note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Vereisten

Voor deze zelfstudie, moet u ervaring met [Docker Compose](https://docs.docker.com/compose/).

## <a name="download-the-sample"></a>Het voorbeeld downloaden

Voor deze zelfstudie gebruikt u het samenstellingsbestand van [Docker](https://docs.docker.com/compose/wordpress/#define-the-project), maar bewerkt u dit zo dat hierin Azure Database for MySQL, permanente opslag en Redis worden opgenomen. U vindt het configuratiebestanden in [Azure-voorbeelden](https://github.com/Azure-Samples/multicontainerwordpress).

[!code-yml[Main](../../../azure-app-service-multi-container/docker-compose-wordpress.yml)]

Zie voor ondersteunde configuratie-opties, [Docker Compose opties](configure-custom-container.md#docker-compose-options).

Maak een map voor de zelfstudie in de Cloud Shell en ga er vervolgens naartoe.

```bash
mkdir tutorial

cd tutorial
```

Voer vervolgens de volgende opdracht uit om de voorbeeld-app-opslagplaats te klonen naar de map voor de zelfstudie. Ga vervolgens maar de map `multicontainerwordpress`.

```bash
git clone https://github.com/Azure-Samples/multicontainerwordpress

cd multicontainerwordpress
```

## <a name="create-a-resource-group"></a>Een resourcegroep maken

[!INCLUDE [resource group intro text](../../../includes/resource-group.md)]

Maak een resourcegroep in Cloud Shell met de opdracht [`az group create`](/cli/azure/group?view=azure-cli-latest#az-group-create). In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *US - zuid-centraal*. Als u alle ondersteunde locaties voor App Service op Linux in prijscategorie **Standard** wilt zien, voert u de opdracht [`az appservice list-locations --sku S1 --linux-workers-enabled`](/cli/azure/appservice?view=azure-cli-latest#az-appservice-list-locations) uit.

```azurecli-interactive
az group create --name myResourceGroup --location "South Central US"
```

In het algemeen maakt u een resourcegroep en resources in een regio bij u in de buurt.

Wanneer de opdracht is voltooid, laat een JSON-uitvoer u de eigenschappen van de resource-groep zien.

## <a name="create-an-azure-app-service-plan"></a>Een Azure App Service-plan maken

Maak in Cloud Shell een App Service-plan in de resourcegroep met de opdracht [`az appservice plan create`](/cli/azure/appservice/plan?view=azure-cli-latest#az-appservice-plan-create).

<!-- [!INCLUDE [app-service-plan](app-service-plan-linux.md)] -->

In het volgende voorbeeld wordt een App Service-plan gemaakt met de naam `myAppServicePlan` in de prijscategorie **Standard** (`--sku S1`) en in een Linux-container (`--is-linux`).

```azurecli-interactive
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku S1 --is-linux
```

Wanneer het App Service-plan is gemaakt, toont Cloud Shell soortgelijke informatie als in het volgende voorbeeld:

```json
{
  "adminSiteName": null,
  "appServicePlanName": "myAppServicePlan",
  "geoRegion": "South Central US",
  "hostingEnvironmentProfile": null,
  "id": "/subscriptions/0000-0000/resourceGroups/myResourceGroup/providers/Microsoft.Web/serverfarms/myAppServicePlan",
  "kind": "linux",
  "location": "South Central US",
  "maximumNumberOfWorkers": 1,
  "name": "myAppServicePlan",
  < JSON data removed for brevity. >
  "targetWorkerSizeId": 0,
  "type": "Microsoft.Web/serverfarms",
  "workerTierName": null
}
```

### <a name="docker-compose-with-wordpress-and-mysql-containers"></a>Docker Compose met WordPress- en MySQL-containers

## <a name="create-a-docker-compose-app"></a>Een Docker Compose-app maken

Maak in Cloud Shell een [web-app](app-service-linux-intro.md) met meerdere containers in het `myAppServicePlan` App Service-plan met de opdracht [az webapp create](/cli/azure/webapp?view=azure-cli-latest#az-webapp-create). Vergeet niet om te vervangen  _\<app-naam >_ door een unieke naam.

```azurecli-interactive
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app-name> --multicontainer-config-type compose --multicontainer-config-file docker-compose-wordpress.yml
```

Wanneer de web-app is gemaakt, toont Cloud Shell soortgelijke uitvoer als in het volgende voorbeeld:

```json
{
  "additionalProperties": {},
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app-name>.azurewebsites.net",
  "enabled": true,
  < JSON data removed for brevity. >
}
```

### <a name="browse-to-the-app"></a>Bladeren naar de app

Blader naar de geïmplementeerde app in (`http://<app-name>.azurewebsites.net`). Het laden van de app kan een paar minuten duren. Als u een foutbericht ontvangt, wacht u een paar minuten en vernieuwt u de browser. Lees [containerlogboeken](#find-docker-container-logs) als u problemen ondervindt en deze wilt oplossen.

![Voorbeeld-app met meerdere containers in Web App for Containers][1]

**Gefeliciteerd**, u hebt een app met meerdere containers gemaakt in Web App for Containers. Vervolgens configureert u uw app voor gebruik met Azure Database for MySQL. Installeer WordPress nu nog niet.

## <a name="connect-to-production-database"></a>Verbinding maken met de productiedatabase

Het wordt niet aanbevolen databasecontainers te gebruiken in een productieomgeving. De lokale containers zijn niet schaalbaar. In plaats hiervan gebruikt u Azure Database for MySQL, dat kan worden geschaald.

### <a name="create-an-azure-database-for-mysql-server"></a>Een Azure-database voor MySQL-server maken

Maak een Azure Database for MySQL-server met de opdracht [`az mysql server create`](/cli/azure/mysql/server?view=azure-cli-latest#az-mysql-server-create).

Vervang in de volgende opdracht, de naam van uw MySQL-server waarin u ziet de  _&lt;mysql-server-naam >_ tijdelijke aanduiding (geldige tekens zijn `a-z`, `0-9`, en `-`). Deze naam maakt deel uit van de hostnaam van de MySQL-server (`<mysql-server-name>.database.windows.net`) en moet globaal uniek zijn.

```azurecli-interactive
az mysql server create --resource-group myResourceGroup --name <mysql-server-name>  --location "South Central US" --admin-user adminuser --admin-password My5up3rStr0ngPaSw0rd! --sku-name B_Gen4_1 --version 5.7
```

Het kan enkele minuten duren voordat de server is gemaakt. Wanneer de MySQL-server is gemaakt, toont Cloud Shell informatie die lijkt op de informatie in het volgende voorbeeld:

```json
{
  "administratorLogin": "adminuser",
  "administratorLoginPassword": null,
  "fullyQualifiedDomainName": "<mysql-server-name>.database.windows.net",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.DBforMySQL/servers/<mysql-server-name>",
  "location": "southcentralus",
  "name": "<mysql-server-name>",
  "resourceGroup": "myResourceGroup",
  ...
}
```

### <a name="configure-server-firewall"></a>Een serverfirewall configureren

Maak een firewallregel voor uw MySQL-server om clientverbindingen toe te staan met behulp van de opdracht [`az mysql server firewall-rule create`](/cli/azure/mysql/server/firewall-rule?view=azure-cli-latest#az-mysql-server-firewall-rule-create). Als zowel het IP-beginadres als het IP-eindadres zijn ingesteld op 0.0.0.0, wordt de firewall alleen geopend voor andere Azure-resources.

```azurecli-interactive
az mysql server firewall-rule create --name allAzureIPs --server <mysql-server-name> --resource-group myResourceGroup --start-ip-address 0.0.0.0 --end-ip-address 0.0.0.0
```

> [!TIP]
> U kunt uw firewallregel nog beperkender maken door [alleen de uitgaande IP-adressen te gebruiken die in uw app worden gebruikt](../overview-inbound-outbound-ips.md?toc=%2fazure%2fapp-service%2fcontainers%2ftoc.json#find-outbound-ips).
>

### <a name="create-the-wordpress-database"></a>De WordPress-database maken

```azurecli-interactive
az mysql db create --resource-group myResourceGroup --server-name <mysql-server-name> --name wordpress
```

Wanneer de database is gemaakt, wordt in Cloud Shell informatie weergegeven die vergelijkbaar is met de informatie in het volgende voorbeeld:

```json
{
  "additionalProperties": {},
  "charset": "latin1",
  "collation": "latin1_swedish_ci",
  "id": "/subscriptions/12db1644-4b12-4cab-ba54-8ba2f2822c1f/resourceGroups/myResourceGroup/providers/Microsoft.DBforMySQL/servers/<mysql-server-name>/databases/wordpress",
  "name": "wordpress",
  "resourceGroup": "myResourceGroup",
  "type": "Microsoft.DBforMySQL/servers/databases"
}
```

### <a name="configure-database-variables-in-wordpress"></a>Databasevariabelen in WordPress configureren

Als u de WordPress-app wilt verbinden met deze nieuwe MySQL-server, configureert u een aantal specifieke WordPress-omgevingsvariabelen, inclusief het SSL CA-pad dat is gedefinieerd door `MYSQL_SSL_CA`. Hieronder vindt u [Baltimore CyberTrust Root](https://www.digicert.com/digicert-root-certificates.htm) van [DigiCert](https://www.digicert.com/) in de [aangepaste installatiekopie](https://docs.microsoft.com/azure/app-service/containers/tutorial-multi-container-app#use-a-custom-image-for-mysql-ssl-and-other-configurations).

Als u deze wijzigingen wilt aanbrengen, gebruikt u de opdracht [az webapp config appsettings set](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az-webapp-config-appsettings-set) in Cloud Shell. App-instellingen zijn hoofdlettergevoelig en door spaties gescheiden.

```azurecli-interactive
az webapp config appsettings set --resource-group myResourceGroup --name <app-name> --settings WORDPRESS_DB_HOST="<mysql-server-name>.mysql.database.azure.com" WORDPRESS_DB_USER="adminuser@<mysql-server-name>" WORDPRESS_DB_PASSWORD="My5up3rStr0ngPaSw0rd!" WORDPRESS_DB_NAME="wordpress" MYSQL_SSL_CA="BaltimoreCyberTrustroot.crt.pem"
```

Wanneer de app-instelling is gemaakt, toont Cloud Shell soortgelijke informatie als in het volgende voorbeeld:

```json
[
  {
    "name": "WORDPRESS_DB_HOST",
    "slotSetting": false,
    "value": "<mysql-server-name>.mysql.database.azure.com"
  },
  {
    "name": "WORDPRESS_DB_USER",
    "slotSetting": false,
    "value": "adminuser@<mysql-server-name>"
  },
  {
    "name": "WORDPRESS_DB_NAME",
    "slotSetting": false,
    "value": "wordpress"
  },
  {
    "name": "WORDPRESS_DB_PASSWORD",
    "slotSetting": false,
    "value": "My5up3rStr0ngPaSw0rd!"
  },
  {
    "name": "MYSQL_SSL_CA",
    "slotSetting": false,
    "value": "BaltimoreCyberTrustroot.crt.pem"
  }
]
```

Zie voor meer informatie over omgevingsvariabelen [omgevingsvariabelen configureren](configure-custom-container.md#configure-environment-variables).

### <a name="use-a-custom-image-for-mysql-ssl-and-other-configurations"></a>Een aangepaste installatiekopie gebruiken voor MySQL SSL en andere configuraties

Standaard wordt SSL gebruikt in Azure Database for MySQL. Voor WordPress is aanvullende configuratie vereist voor het gebruik van SSL met MySQL. De WordPress-officiële installatiekopie' biedt geen de aanvullende configuratie, maar een [aangepaste installatiekopie](https://github.com/Azure-Samples/multicontainerwordpress) is voorbereid voor uw gemak. In de praktijk voegt u de gewenste wijzigingen toe aan uw eigen installatiekopie.

De aangepaste installatiekopie is gebaseerd op de 'officiële installatiekopie' van [WordPress van Docker-Hub](https://hub.docker.com/_/wordpress/). De volgende wijzigingen zijn aangebracht in deze aangepaste installatiekopie voor de Azure Database for MySQL:

* [Hiermee wordt het certificatiebestand Baltimore Cyber Trust Root voor SSL toegevoegd aan MySQL.](https://github.com/Azure-Samples/multicontainerwordpress/blob/5669a89e0ee8599285f0e2e6f7e935c16e539b92/docker-entrypoint.sh#L61)
* [Hiermee wordt de app-instelling voor het certificaat MySQL SSL-certificeringsinstantie gebruikt in wp-config.php van WordPress.](https://github.com/Azure-Samples/multicontainerwordpress/blob/5669a89e0ee8599285f0e2e6f7e935c16e539b92/docker-entrypoint.sh#L163)
* [Hiermee worden WordPress-definities voor MYSQL_CLIENT_FLAGS toegevoegd die nodig zijn voor MySQL SSL.](https://github.com/Azure-Samples/multicontainerwordpress/blob/5669a89e0ee8599285f0e2e6f7e935c16e539b92/docker-entrypoint.sh#L164)

De volgende wijzigingen zijn aangebracht voor Redis (die worden gebruikt in een latere sectie):

* [Hiermee wordt de PHP-uitbreiding voor Redis v4.0.2 toegevoegd.](https://github.com/Azure-Samples/multicontainerwordpress/blob/5669a89e0ee8599285f0e2e6f7e935c16e539b92/Dockerfile#L35)
* [Hiermee wordt uitpakken toegevoegd dat nodig is om het bestand uit te pakken.](https://github.com/Azure-Samples/multicontainerwordpress/blob/5669a89e0ee8599285f0e2e6f7e935c16e539b92/docker-entrypoint.sh#L71)
* [Hiermee wordt de WordPress-invoegtoepassing Redis Object Cache 1.3.8 toegevoegd.](https://github.com/Azure-Samples/multicontainerwordpress/blob/5669a89e0ee8599285f0e2e6f7e935c16e539b92/docker-entrypoint.sh#L74)
* [Hiermee wordt de app-instelling voor de Redis-hostnaam gebruikt in wp-config.php van WordPress.](https://github.com/Azure-Samples/multicontainerwordpress/blob/5669a89e0ee8599285f0e2e6f7e935c16e539b92/docker-entrypoint.sh#L162)

Als u de aangepaste installatiekopie wilt gebruiken, werkt u het bestand docker-compose-wordpress.yml bij. Typ `nano docker-compose-wordpress.yml` in Cloud Shell om de nano-teksteditor te openen. Wijzig de `image: wordpress` om `image: microsoft/multicontainerwordpress` te gebruiken. U hebt de databasecontainer niet meer nodig. Verwijder de sectie `db`, `environment`, `depends_on`, en `volumes` uit het configuratiebestand. Uw bestand moet eruitzien als de volgende code:

```yaml
version: '3.3'

services:
   wordpress:
     image: microsoft/multicontainerwordpress
     ports:
       - "8000:80"
     restart: always
```

Sla uw wijzigingen op en sluit nano af. Sla op met de opdracht `^O` en sluit af met `^X`.

### <a name="update-app-with-new-configuration"></a>De app bijwerken met de nieuwe configuratie

Configureer in Cloud Shell uw [web-app](app-service-linux-intro.md) met meerdere containers opnieuw met de opdracht [az webapp config container set](/cli/azure/webapp/config/container?view=azure-cli-latest#az-webapp-config-container-set). Vergeet niet om te vervangen  _\<app-naam >_ met de naam van de web-app die u eerder hebt gemaakt.

```azurecli-interactive
az webapp config container set --resource-group myResourceGroup --name <app-name> --multicontainer-config-type compose --multicontainer-config-file docker-compose-wordpress.yml
```

Wanneer de app opnieuw is geconfigureerd, wordt in Cloud Shell informatie weergegeven die vergelijkbaar is met de informatie in het volgende voorbeeld:

```json
[
  {
    "name": "DOCKER_CUSTOM_IMAGE_NAME",
    "value": "COMPOSE|dmVyc2lvbjogJzMuMycKCnNlcnZpY2VzOgogICB3b3JkcHJlc3M6CiAgICAgaW1hZ2U6IG1zYW5nYXB1L3dvcmRwcmVzcwogICAgIHBvcnRzOgogICAgICAgLSAiODAwMDo4MCIKICAgICByZXN0YXJ0OiBhbHdheXM="
  }
]
```

### <a name="browse-to-the-app"></a>Bladeren naar de app

Blader naar de geïmplementeerde app in (`http://<app-name>.azurewebsites.net`). Azure Database for MySQL wordt nu gebruikt voor de app.

![Voorbeeld van app met meerdere containers in Web App for Containers][1]

## <a name="add-persistent-storage"></a>Permanente opslag toevoegen

Uw app met meerdere containers wordt nu uitgevoerd in Web App for Containers. Als u WordPress echter nu installeert uw app later opnieuw start, ziet u dat uw WordPress-installatie is verdwenen. Dit gebeurt omdat de configuratie van Docker Compose momenteel naar een opslaglocatie in de container verwijst. De bestanden die zijn geïnstalleerd in de container, blijven niet behouden nadat de app opnieuw is gestart. In deze sectie gaat u [permanente opslag toevoegen](configure-custom-container.md#use-persistent-shared-storage) naar uw WordPress-container.

### <a name="configure-environment-variables"></a>Omgevingsvariabelen configureren

Als u wilt gebruiken voor permanente opslag, schakelt u deze instelling in App Service. Als u deze wijziging wilt aanbrengen, gebruikt u de opdracht [az webapp config appsettings set](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az-webapp-config-appsettings-set) in Cloud Shell. App-instellingen zijn hoofdlettergevoelig en door spaties gescheiden.

```azurecli-interactive
az webapp config appsettings set --resource-group myResourceGroup --name <app-name> --settings WEBSITES_ENABLE_APP_SERVICE_STORAGE=TRUE
```

Wanneer de app-instelling is gemaakt, toont Cloud Shell soortgelijke informatie als in het volgende voorbeeld:

```json
[
  < JSON data removed for brevity. >
  {
    "name": "WORDPRESS_DB_NAME",
    "slotSetting": false,
    "value": "wordpress"
  },
  {
    "name": "WEBSITES_ENABLE_APP_SERVICE_STORAGE",
    "slotSetting": false,
    "value": "TRUE"
  }
]
```

### <a name="modify-configuration-file"></a>Configuratiebestand wijzigen

Typ `nano docker-compose-wordpress.yml` in Cloud Shell om de nano-teksteditor te openen.

Met de optie `volumes` wordt het bestandssysteem toegewezen aan een map in de container. `${WEBAPP_STORAGE_HOME}` is een omgevingsvariabele in App Service die is toegewezen aan de permanente opslag voor uw app. U gebruikt deze omgevingsvariabele in de volumeoptie zodat de WordPress-bestanden worden geïnstalleerd in de permanente opslag in plaats van de container. Breng de volgende wijzigingen aan in het bestand:

Voeg in de sectie `wordpress` de optie `volumes` toe, zoals in de volgende code:

```yaml
version: '3.3'

services:
   wordpress:
     image: microsoft/multicontainerwordpress
     volumes:
      - ${WEBAPP_STORAGE_HOME}/site/wwwroot:/var/www/html
     ports:
       - "8000:80"
     restart: always
```

### <a name="update-app-with-new-configuration"></a>De app bijwerken met de nieuwe configuratie

Configureer in Cloud Shell uw [web-app](app-service-linux-intro.md) met meerdere containers opnieuw met de opdracht [az webapp config container set](/cli/azure/webapp/config/container?view=azure-cli-latest#az-webapp-config-container-set). Vergeet niet om te vervangen  _\<app-naam >_ door een unieke naam.

```azurecli-interactive
az webapp config container set --resource-group myResourceGroup --name <app-name> --multicontainer-config-type compose --multicontainer-config-file docker-compose-wordpress.yml
```

Nadat uw opdracht wordt uitgevoerd, wordt er uitvoer weergegeven die vergelijkbaar is met de uitvoer in het volgende voorbeeld:

```json
[
  {
    "name": "WEBSITES_ENABLE_APP_SERVICE_STORAGE",
    "slotSetting": false,
    "value": "TRUE"
  },
  {
    "name": "DOCKER_CUSTOM_IMAGE_NAME",
    "value": "COMPOSE|dmVyc2lvbjogJzMuMycKCnNlcnZpY2VzOgogICBteXNxbDoKICAgICBpbWFnZTogbXlzcWw6NS43CiAgICAgdm9sdW1lczoKICAgICAgIC0gZGJfZGF0YTovdmFyL2xpYi9teXNxbAogICAgIHJlc3RhcnQ6IGFsd2F5cwogICAgIGVudmlyb25tZW50OgogICAgICAgTVlTUUxfUk9PVF9QQVNTV09SRDogZXhhbXBsZXBhc3MKCiAgIHdvcmRwcmVzczoKICAgICBkZXBlbmRzX29uOgogICAgICAgLSBteXNxbAogICAgIGltYWdlOiB3b3JkcHJlc3M6bGF0ZXN0CiAgICAgcG9ydHM6CiAgICAgICAtICI4MDAwOjgwIgogICAgIHJlc3RhcnQ6IGFsd2F5cwogICAgIGVudmlyb25tZW50OgogICAgICAgV09SRFBSRVNTX0RCX1BBU1NXT1JEOiBleGFtcGxlcGFzcwp2b2x1bWVzOgogICAgZGJfZGF0YTo="
  }
]
```

### <a name="browse-to-the-app"></a>Bladeren naar de app

Blader naar de geïmplementeerde app in (`http://<app-name>.azurewebsites.net`).

De WordPress-container maakt nu gebruik van Azure Database for MySQL en permanente opslag.

## <a name="add-redis-container"></a>De Redis-container toevoegen

 De 'officiële installatiekopie' van WordPress bevat niet de afhankelijkheden voor Redis. Deze afhankelijkheden en de aanvullende configuratie die nodig is voor het gebruik van Redis met WordPress, zijn voor u gemaakt in deze [aangepaste installatiekopie](https://github.com/Azure-Samples/multicontainerwordpress). In de praktijk voegt u de gewenste wijzigingen toe aan uw eigen installatiekopie.

De aangepaste installatiekopie is gebaseerd op de 'officiële installatiekopie' van [WordPress van Docker-Hub](https://hub.docker.com/_/wordpress/). De volgende wijzigingen zijn aangebracht in deze aangepaste installatiekopie voor Redis:

* [Hiermee wordt de PHP-uitbreiding voor Redis v4.0.2 toegevoegd.](https://github.com/Azure-Samples/multicontainerwordpress/blob/5669a89e0ee8599285f0e2e6f7e935c16e539b92/Dockerfile#L35)
* [Hiermee wordt uitpakken toegevoegd dat nodig is om het bestand uit te pakken.](https://github.com/Azure-Samples/multicontainerwordpress/blob/5669a89e0ee8599285f0e2e6f7e935c16e539b92/docker-entrypoint.sh#L71)
* [Hiermee wordt de WordPress-invoegtoepassing Redis Object Cache 1.3.8 toegevoegd.](https://github.com/Azure-Samples/multicontainerwordpress/blob/5669a89e0ee8599285f0e2e6f7e935c16e539b92/docker-entrypoint.sh#L74)
* [Hiermee wordt de app-instelling voor de Redis-hostnaam gebruikt in wp-config.php van WordPress.](https://github.com/Azure-Samples/multicontainerwordpress/blob/5669a89e0ee8599285f0e2e6f7e935c16e539b92/docker-entrypoint.sh#L162)

Voeg de Redis-container toe onderaan het configuratiebestand, zoals in het volgende voorbeeld:

```yaml
version: '3.3'

services:
   wordpress:
     image: microsoft/multicontainerwordpress
     ports:
       - "8000:80"
     restart: always

   redis:
     image: redis:3-alpine
     restart: always
```

### <a name="configure-environment-variables"></a>Omgevingsvariabelen configureren

Schakel deze instelling, `WP_REDIS_HOST`, in App Service in als u Redis wilt gebruiken. Dit is een *vereiste instelling* voor WordPress om te communiceren met de Redis-host. Als u deze wijziging wilt aanbrengen, gebruikt u de opdracht [az webapp config appsettings set](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az-webapp-config-appsettings-set) in Cloud Shell. App-instellingen zijn hoofdlettergevoelig en door spaties gescheiden.

```azurecli-interactive
az webapp config appsettings set --resource-group myResourceGroup --name <app-name> --settings WP_REDIS_HOST="redis"
```

Wanneer de app-instelling is gemaakt, toont Cloud Shell soortgelijke informatie als in het volgende voorbeeld:

```json
[
  < JSON data removed for brevity. >
  {
    "name": "WORDPRESS_DB_USER",
    "slotSetting": false,
    "value": "adminuser@<mysql-server-name>"
  },
  {
    "name": "WP_REDIS_HOST",
    "slotSetting": false,
    "value": "redis"
  }
]
```

### <a name="update-app-with-new-configuration"></a>De app bijwerken met de nieuwe configuratie

Configureer in Cloud Shell uw [web-app](app-service-linux-intro.md) met meerdere containers opnieuw met de opdracht [az webapp config container set](/cli/azure/webapp/config/container?view=azure-cli-latest#az-webapp-config-container-set). Vergeet niet om te vervangen  _\<app-naam >_ door een unieke naam.

```azurecli-interactive
az webapp config container set --resource-group myResourceGroup --name <app-name> --multicontainer-config-type compose --multicontainer-config-file compose-wordpress.yml
```

Nadat uw opdracht wordt uitgevoerd, wordt er uitvoer weergegeven die vergelijkbaar is met de uitvoer in het volgende voorbeeld:

```json
[
  {
    "name": "DOCKER_CUSTOM_IMAGE_NAME",
    "value": "COMPOSE|dmVyc2lvbjogJzMuMycKCnNlcnZpY2VzOgogICBteXNxbDoKICAgICBpbWFnZTogbXlzcWw6NS43CiAgICAgdm9sdW1lczoKICAgICAgIC0gZGJfZGF0YTovdmFyL2xpYi9teXNxbAogICAgIHJlc3RhcnQ6IGFsd2F5cwogICAgIGVudmlyb25tZW50OgogICAgICAgTVlTUUxfUk9PVF9QQVNTV09SRDogZXhhbXBsZXBhc3MKCiAgIHdvcmRwcmVzczoKICAgICBkZXBlbmRzX29uOgogICAgICAgLSBteXNxbAogICAgIGltYWdlOiB3b3JkcHJlc3M6bGF0ZXN0CiAgICAgcG9ydHM6CiAgICAgICAtICI4MDAwOjgwIgogICAgIHJlc3RhcnQ6IGFsd2F5cwogICAgIGVudmlyb25tZW50OgogICAgICAgV09SRFBSRVNTX0RCX1BBU1NXT1JEOiBleGFtcGxlcGFzcwp2b2x1bWVzOgogICAgZGJfZGF0YTo="
  }
]
```

### <a name="browse-to-the-app"></a>Bladeren naar de app

Blader naar de geïmplementeerde app in (`http://<app-name>.azurewebsites.net`).

Voer de stappen uit en installeer WordPress.

### <a name="connect-wordpress-to-redis"></a>WordPress verbinden met Redis

Aanmelden bij de WordPress-beheerder. Selecteer **Invoegtoepassingen** in het linkernavigatievenster en selecteer vervolgens **Geïnstalleerde invoegtoepassingen**.

![WordPress-invoegtoepassingen selecteren][2]

Alle invoegtoepassingen hier weergeven

Zoek op de pagina met invoegtoepassingen naar **Redis-objectcache** en klik op **Activeren**.

![Redis activeren][3]

Klik op **Instellingen**.

![Klik op Instellingen][4]

Klik op de knop **Objectcache inschakelen**.

![Klik op de knop Objectcache inschakelen][5]

WordPress wordt verbonden met de Redis-server. De **status** van de verbinding wordt weergegeven op dezelfde pagina.

![WordPress wordt verbonden met de Redis-server. De **status** van de verbinding wordt weergegeven op dezelfde pagina.][6]

**Gefeliciteerd**, u hebt WordPress verbonden met Redis. De app die klaar is voor productie, maakt nu gebruik van **Azure Database for MySQL, permanente opslag en Redis**. U kunt uw App Service-plan nu uitbreiden naar meerdere exemplaren.

## <a name="find-docker-container-logs"></a>Logboeken voor Docker-containers zoeken

Als u problemen hebt met het gebruik van meerdere containers, kunt u de logboeken van de containers openen door te bladeren naar: `https://<app-name>.scm.azurewebsites.net/api/logs/docker`.

De uitvoer is vergelijkbaar met de uitvoer in het volgende voorbeeld:

```json
[
   {
      "machineName":"RD00XYZYZE567A",
      "lastUpdated":"2018-05-10T04:11:45Z",
      "size":25125,
      "href":"https://<app-name>.scm.azurewebsites.net/api/vfs/LogFiles/2018_05_10_RD00XYZYZE567A_docker.log",
      "path":"/home/LogFiles/2018_05_10_RD00XYZYZE567A_docker.log"
   }
]
```

U ziet een logboek voor elke container en een extra logboek voor het bovenliggende proces. Kopieer de desbetreffende waarde `href` in de browser om het logboek weer te geven.

[!INCLUDE [Clean-up section](../../../includes/cli-script-clean-up.md)]

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:
> [!div class="checklist"]
> * Een Docker Compose-configuratie converteren die kan worden gebruikt voor Web App for Containers
> * Een app met meerdere containers implementeren in Azure
> * Toepassingsinstellingen toevoegen
> * Permanente opslag voor uw containers gebruiken
> * Verbinding maken met Azure Database for MySQL
> * Problemen oplossen

Ga door naar de volgende zelfstudie om te leren hoe u een aangepaste DNS-naam aan uw app kunt toewijzen.

> [!div class="nextstepaction"]
> [Zelfstudie: Aangepaste DNS-naam toewijzen aan uw app](../app-service-web-tutorial-custom-domain.md)

Of Ga naar andere resources:

> [!div class="nextstepaction"]
> [Aangepaste container configureren](configure-custom-container.md)

<!--Image references-->
[1]: ./media/tutorial-multi-container-app/azure-multi-container-wordpress-install.png
[2]: ./media/tutorial-multi-container-app/wordpress-plugins.png
[3]: ./media/tutorial-multi-container-app/activate-redis.png
[4]: ./media/tutorial-multi-container-app/redis-settings.png
[5]: ./media/tutorial-multi-container-app/enable-object-cache.png
[6]: ./media/tutorial-multi-container-app/redis-connected.png
