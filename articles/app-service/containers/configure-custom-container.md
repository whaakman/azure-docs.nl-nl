---
title: Configureren van de klant containers - Azure App Service | Microsoft Docs
description: Informatie over het configureren van Node.js-apps werken in Azure App Service
services: app-service
documentationcenter: ''
author: cephalin
manager: jpconnock
editor: ''
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/28/2019
ms.author: cephalin
ms.openlocfilehash: 1e5faa8d356b891d825586414c0a1a1b9fa47090
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/22/2019
ms.locfileid: "60001878"
---
# <a name="configure-a-custom-linux-container-for-azure-app-service"></a>Een aangepaste Linux-container configureren voor Azure App Service

Dit artikel leest u hoe het configureren van een aangepaste Linux-container uit te voeren op Azure App Service.

Deze handleiding bevat de belangrijkste concepten en instructies voor containeropslag van Linux-apps in App Service. Als u Azure App Service nog nooit hebt gebruikt, volgt u de [aangepaste container snelstartgids](quickstart-docker-go.md) en [zelfstudie](tutorial-custom-docker-image.md) eerste. Er is ook een [Snelstartgids voor apps met meerdere containers](quickstart-multi-container.md) en [zelfstudie](tutorial-multi-container-app.md).

## <a name="configure-port-number"></a>Poortnummer configureren

De webserver in uw aangepaste installatiekopie gebruikt mogelijk een andere poort dan 80. U vertelt Azure over de poort die gebruikmaakt van uw aangepaste met behulp van de `WEBSITES_PORT` app-instelling. De GitHub-pagina voor het [Python-voorbeeld in deze zelfstudie](https://github.com/Azure-Samples/docker-django-webapp-linux) laat zien dat u `WEBSITES_PORT` in moet stellen op _8000_. U kunt dit instellen door te voeren [ `az webapp config appsettings set` ](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az-webapp-config-appsettings-set) opdracht in de Cloud Shell. Bijvoorbeeld:

```azurecli-interactive
az webapp config appsettings set --resource-group <resource-group-name> --name <app-name> --settings WEBSITES_PORT=8000
```

## <a name="configure-environment-variables"></a>Omgevingsvariabelen configureren

Uw aangepaste container kunt omgevingsvariabelen die moeten extern worden opgegeven. U kunt ze in doorgeven door uit te voeren [ `az webapp config appsettings set` ](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az-webapp-config-appsettings-set) opdracht in de Cloud Shell. Bijvoorbeeld:

```azurecli-interactive
az webapp config appsettings set --resource-group <resource-group-name> --name <app-name> --settings WORDPRESS_DB_HOST="myownserver.mysql.database.azure.com"
```

Deze methode werkt zowel voor één container-apps of apps in meerdere containers, waar de omgevingsvariabelen die zijn opgegeven in de *docker-compose.yml* bestand.

## <a name="use-persistent-shared-storage"></a>Permanente gedeelde opslag gebruiken

U kunt de */home* Active directory in het bestandssysteem van uw app te blijven behouden bestanden via opnieuw wordt opgestart en deze delen tussen exemplaren. De `/home` in uw app is opgegeven voor het inschakelen van uw container-app voor toegang tot permanente opslag.

Wanneer de permanente opslag is uitgeschakeld en schrijft naar de `/home` directory worden niet opgeslagen in de app opnieuw wordt opgestart of over meerdere instanties. De enige uitzondering hierop is de `/home/LogFiles` directory, dat wordt gebruikt voor het opslaan van de logboeken van Docker en containers. Wanneer de permanente opslag is ingeschakeld, worden alle schrijfbewerkingen naar de `/home` directory worden doorgevoerd en kan worden geopend door alle exemplaren van een scale-out-app.

Permanente opslag is standaard *uitgeschakeld*. Als u wilt in- of uitschakelen, stelt de `WEBSITES_ENABLE_APP_SERVICE_STORAGE` app-instelling door te voeren [ `az webapp config appsettings set` ](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az-webapp-config-appsettings-set) opdracht in de Cloud Shell. Bijvoorbeeld:

```azurecli-interactive
az webapp config appsettings set --resource-group <resource-group-name> --name <app-name> --settings WEBSITES_ENABLE_APP_SERVICE_STORAGE=true
```

> [!NOTE]
> U kunt ook [configureren van uw eigen permanente opslag](how-to-serve-content-from-azure-storage.md).

## <a name="enable-ssh"></a>SSH inschakelen

SSH maakt veilige communicatie tussen een container en een client mogelijk. In de order voor een aangepaste container voor de ondersteuning van SSH, moet u deze toevoegen in de docker-bestand zelf.

> [!TIP]
> Alle ingebouwde Linux-containers hebben de SSH-instructies in de afbeelding opslagplaatsen toegevoegd. U kunt de volgende instructies met doorloopt de [Node.js 10.14 opslagplaats](https://github.com/Azure-App-Service/node/blob/master/10.14) om te zien hoe het er wordt ingeschakeld.

- Gebruik de [uitvoeren](https://docs.docker.com/engine/reference/builder/#run) instructies voor het installeren van de SSH-server en stel het wachtwoord voor het hoofdaccount op `"Docker!"`. Bijvoorbeeld: voor een installatiekopie op basis van [Alpine Linux](https://hub.docker.com/_/alpine), moet u de volgende opdrachten:

    ```Dockerfile
    RUN apk add openssh \
         && echo "root:Docker!" | chpasswd 
    ```

    Deze configuratie toestaan niet van externe verbindingen naar de container. SSH is alleen beschikbaar via `https://<app-name>.scm.azurewebsites.net` en geverifieerd met de publicatiereferenties.

- Toevoegen [dit sshd_config-bestand](https://github.com/Azure-App-Service/node/blob/master/10.14/sshd_config) naar de opslagplaats voor installatiekopieën en gebruik de [kopie](https://docs.docker.com/engine/reference/builder/#copy) instructies voor het kopiëren van het bestand naar de */etc/ssh/* directory. Voor meer informatie over *sshd_config* bestanden, Zie [OpenBSD documentatie](https://man.openbsd.org/sshd_config).

    ```Dockerfile
    COPY sshd_config /etc/ssh/
    ```

    > [!NOTE]
    > Het *sshd_config*-bestand moet de volgende items bevatten:
    > - `Ciphers`moet ten minste één item in deze lijst bevatten: `aes128-cbc,3des-cbc,aes256-cbc`.
    > - `MACs`moet ten minste één item in deze lijst bevatten: `hmac-sha1,hmac-sha1-96`.

- Gebruik de [BLOOTSTELLEN](https://docs.docker.com/engine/reference/builder/#expose) instructies voor het openen van poort 2222 in de container. Hoewel het hoofdwachtwoord bekend is, is poort 2222 niet toegankelijk is vanaf internet. Het is toegankelijk door containers in het Brugnetwerk van een virtueel privénetwerk.

    ```Dockerfile
    EXPOSE 80 2222
    ```

- In het script opstarten voor de container, start u de SSH-server.

    ```bash
    /usr/sbin/sshd
    ```

    Zie voor een voorbeeld hoe de standaard [Node.js 10.14 container](https://github.com/Azure-App-Service/node/blob/master/10.14/startup/init_container.sh) de SSH-server wordt gestart.

## <a name="access-diagnostic-logs"></a>Toegang tot diagnostische logboeken

[!INCLUDE [Access diagnostic logs](../../../includes/app-service-web-logs-access-no-h.md)]

## <a name="configure-multi-container-apps"></a>Meerdere container-apps configureren

- [Permanente opslag in Docker Compose gebruiken](#use-persistent-storage-in-docker-compose)
- [Beperkingen voor Preview](#preview-limitations)
- [Docker Compose-opties](#docker-compose-options)
- [Configuratieopties voor Kubernetes](#kubernetes-configuration-options)

### <a name="use-persistent-storage-in-docker-compose"></a>Permanente opslag in Docker Compose gebruiken

Meerdere containers apps zoals WordPress nodig voor permanente opslag te laten functioneren. Als u wilt inschakelen, uw Docker Compose-configuratie moet verwijzen naar een opslaglocatie *buiten* de container. Opslaglocaties in de container niet wijzigingen buiten de app opnieuw moet worden behouden.

Permanente opslag inschakelen door in te stellen de `WEBSITES_ENABLE_APP_SERVICE_STORAGE` app instellen, met behulp van de [az webapp config appsettings set](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az-webapp-config-appsettings-set) opdracht in Cloud Shell.

```azurecli-interactive
az webapp config appsettings set --resource-group <resource-group-name> --name <app-name> --settings WEBSITES_ENABLE_APP_SERVICE_STORAGE=TRUE
```

In uw *docker-compose.yml* bestand, wijs de `volumes` optie naar `${WEBAPP_STORAGE_HOME}`. 

`WEBAPP_STORAGE_HOME` is een omgevingsvariabele in App Service die is toegewezen aan de permanente opslag voor uw app. Bijvoorbeeld:

```yaml
wordpress:
  image: wordpress:latest
  volumes:
  - ${WEBAPP_STORAGE_HOME}/site/wwwroot:/var/www/html
  - ${WEBAPP_STORAGE_HOME}/phpmyadmin:/var/www/phpmyadmin
  - ${WEBAPP_STORAGE_HOME}/LogFiles:/var/log
```

### <a name="use-custom-storage-in-docker-compose"></a>Aangepaste storage gebruiken in het Docker Compose

Azure Storage (Azure Files of Azure Blob) kan worden gekoppeld met meerdere containers apps met behulp van de aangepaste-id. Uitvoeren als u de naam van de aangepaste-id, [ `az webapp config storage-account list --name <app_name> --resource-group <resource_group>` ](/cli/azure/webapp/config/storage-account?view=azure-cli-latest#az-webapp-config-storage-account-list).

In uw *docker-compose.yml* bestand, wijs de `volumes` optie naar `custom-id`. Bijvoorbeeld:

```yaml
wordpress:
  image: wordpress:latest
  volumes:
  - <custom-id>:<path_in_container>
```

### <a name="preview-limitations"></a>Preview-beperkingen

Meerdere containers is momenteel in preview. De volgende functies van de App Service-platform worden niet ondersteund:

- Verificatie / autorisatie
- Beheerde identiteiten

### <a name="docker-compose-options"></a>Docker Compose-opties

De volgende lijsten ziet u ondersteunde en niet-ondersteunde Docker Compose configuratieopties:

#### <a name="supported-options"></a>Ondersteunde opties

- command
- entrypoint
- environment
- image
- ports
- restart
- services
- volumes

#### <a name="unsupported-options"></a>Niet-ondersteunde opties

- build (niet toegestaan)
- depends_on (genegeerd)
- networks (genegeerd)
- secrets (genegeerd)
- andere poorten dan 80 en 8080 (genegeerd)

> [!NOTE]
> Andere opties niet expliciet worden beschreven worden in de openbare Preview genegeerd.

### <a name="kubernetes-configuration-options"></a>Configuratieopties voor Kubernetes

De volgende configuratieopties worden ondersteund voor Kubernetes:

- argumenten
- command
- containers
- image
- name
- ports
- spec

> [!NOTE]
> Andere opties niet expliciet worden beschreven worden niet in de openbare Preview ondersteund.
>

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Zelfstudie: Implementeren vanaf de privé-containeropslagplaats](tutorial-custom-docker-image.md)

> [!div class="nextstepaction"]
> [Zelfstudie: Meerdere containers WordPress-app](tutorial-multi-container-app.md)
