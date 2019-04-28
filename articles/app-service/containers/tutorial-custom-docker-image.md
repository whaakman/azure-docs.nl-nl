---
title: Een aangepaste installatiekopie bouwen en voor Web App for Containers - Azure App Service | Microsoft Docs
description: Een aangepaste Docker-installatiekopie voor Web App for Containers gebruiken.
keywords: azure-app-service, web-app, linux, docker, container
services: app-service
documentationcenter: ''
author: msangapu
manager: jeconnoc
editor: ''
ms.assetid: b97bd4e6-dff0-4976-ac20-d5c109a559a8
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 03/27/2019
ms.author: msangapu
ms.custom: seodec18
ms.openlocfilehash: 8463ffcb9d9983ff435c01f75dd48f68bde31767
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "62117582"
---
# <a name="tutorial-build-a-custom-image-and-run-in-app-service-from-a-private-registry"></a>Zelfstudie: Een aangepaste installatiekopie bouwen en uitvoeren in App Service uit een persoonlijk register

[App Service](app-service-linux-intro.md) biedt ingebouwde Docker-installatiekopieën op Linux met ondersteuning voor specifieke versies, zoals PHP 7.0 en Node.js 4.5. App Service maakt gebruik van de Docker-containertechnologie voor het hosten van zowel ingebouwde installatiekopieën als aangepaste installatiekopieën als een platform als een service. In deze zelfstudie leert u hoe u een aangepaste installatiekopie bouwen en uitvoeren in App Service. Dit patroon is handig als de taal van keuze niet is inbegrepen in de ingebouwde installatiekopieën, of wanneer uw toepassing een specifieke configuratie vereist die niet door de ingebouwde installatiekopieën wordt verschaft.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een aangepaste installatiekopie implementeren in een privécontainerregister
> * De aangepaste installatiekopie uitvoert in App Service
> * Omgevingsvariabelen configureren
> * Bijwerken en opnieuw implementeren van de installatiekopie
> * Toegang tot diagnostische logboeken
> * Verbinding maken met de container via SSH

[!INCLUDE [Free trial note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Vereisten

Voor deze zelfstudie hebt u het volgende nodig:

* [Git](https://git-scm.com/downloads)
* [Docker](https://docs.docker.com/get-started/#setup)

## <a name="download-the-sample"></a>Het voorbeeld downloaden

Voer de volgende opdracht uit in een terminalvenster. Hiermee wordt de voorbeeld-app-opslagplaats gekloond naar uw lokale computer en navigeert u naar de map met de voorbeeldcode.

```bash
git clone https://github.com/Azure-Samples/docker-django-webapp-linux.git --config core.autocrlf=input
cd docker-django-webapp-linux
```

## <a name="build-the-image-from-the-docker-file"></a>De installatiekopie maken van het Docker-bestand

Bekijk in de Git-opslagplaats het _Docker-bestand_. Dit bestand beschrijft de Python-omgeving die vereist is om uw toepassing uit te voeren. De installatiekopie stelt bovendien een [SSH](https://www.ssh.com/ssh/protocol/)-server in voor veilige communicatie tussen de container en de host.

```Dockerfile
FROM python:3.4

RUN mkdir /code
WORKDIR /code
ADD requirements.txt /code/
RUN pip install -r requirements.txt
ADD . /code/

# ssh
ENV SSH_PASSWD "root:Docker!"
RUN apt-get update \
        && apt-get install -y --no-install-recommends dialog \
        && apt-get update \
    && apt-get install -y --no-install-recommends openssh-server \
    && echo "$SSH_PASSWD" | chpasswd 

COPY sshd_config /etc/ssh/
COPY init.sh /usr/local/bin/
    
RUN chmod u+x /usr/local/bin/init.sh
EXPOSE 8000 2222
#CMD ["python", "/code/manage.py", "runserver", "0.0.0.0:8000"]
ENTRYPOINT ["init.sh"]
```

Bouw de Docker-installatiekopie met de `docker build` opdracht.

```bash
docker build --tag mydockerimage .
```

Test of de build werkt door de Docker-container uit te voeren. Geef de opdracht [`docker run`](https://docs.docker.com/engine/reference/commandline/run/) en geef de naam en het label van de afbeelding door. Zorg ervoor dat u de poort specificeert met behulp van het argument `-p`.

```bash
docker run -p 8000:8000 mydockerimage
```

Controleer of de web-app en de container goed werken door naar `http://localhost:8000` te bladeren.

![Web-app lokaal testen](./media/app-service-linux-using-custom-docker-image/app-service-linux-browse-local.png)

[!INCLUDE [Try Cloud Shell](../../../includes/cloud-shell-try-it.md)]

## <a name="deploy-app-to-azure"></a>App in Azure implementeren

Voor het maken van een app die gebruikmaakt van de installatiekopie die u zojuist hebt gemaakt, kunt u Azure CLI-opdrachten die een resourcegroep maken, wordt de image gepusht en vervolgens maakt u de App Service-plan web-app uit te voeren uitvoeren.

### <a name="create-a-resource-group"></a>Een resourcegroep maken

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group-linux-no-h.md)] 

### <a name="create-an-azure-container-registry"></a>Een Azure Container Registry maken

Gebruik in de Cloud Shell de opdracht [`az acr create`](/cli/azure/acr?view=azure-cli-latest#az-acr-create) voor het maken van een Azure Container Registry.

```azurecli-interactive
az acr create --name <azure-container-registry-name> --resource-group myResourceGroup --sku Basic --admin-enabled true
```

### <a name="sign-in-to-azure-container-registry"></a>Aanmelden bij Azure Container Registry

Als u wilt een installatiekopie naar het register pushen, moet u verifiëren met het persoonlijke register. In de Cloud Shell, gebruikt u de [ `az acr show` ](/cli/azure/acr?view=azure-cli-latest#az-acr-show) opdracht voor het ophalen van de referenties van het register die u hebt gemaakt.

```azurecli-interactive
az acr credential show --name <azure-container-registry-name>
```

De uitvoer geeft twee wachtwoorden, samen met de naam van de gebruiker.

```json
<
  "passwords": [
    {
      "name": "password",
      "value": "{password}"
    },
    {
      "name": "password2",
      "value": "{password}"
    }
  ],
  "username": "<registry-username>"
}
```

Vanuit uw lokale terminalvenster, moet u zich aanmelden bij de Azure Container Registry met behulp van de `docker login` opdracht, zoals wordt weergegeven in het volgende voorbeeld. Vervang  *\<azure-container-register-name >* en  *\<register-username >* met waarden voor uw register. Wanneer u hierom wordt gevraagd, typt u in een van de wachtwoorden van de vorige stap.

```bash
docker login <azure-container-registry-name>.azurecr.io --username <registry-username>
```

Bevestig dat de aanmelding is geslaagd.

### <a name="push-image-to-azure-container-registry"></a>Installatiekopie pushen naar Azure Container Registry

De lokale installatiekopie taggen voor de Azure Container Registry. Bijvoorbeeld:
```bash
docker tag mydockerimage <azure-container-registry-name>.azurecr.io/mydockerimage:v1.0.0
```

Push de installatiekopie met behulp van de opdracht `docker push`. Tag de installatiekopie met de naam van het register, gevolgd door de naam van de installatiekopie en de tag.

```bash
docker push <azure-container-registry-name>.azurecr.io/mydockerimage:v1.0.0
```

Terug in de Cloud Shell, moet u controleren dat de push-bewerking voltooid is.

```azurecli-interactive
az acr repository list -n <azure-container-registry-name>
```

U kunt de volgende uitvoer moeten krijgen.

```json
[
  "mydockerimage"
]
```

### <a name="create-app-service-plan"></a>Een App Service-plan maken

[!INCLUDE [Create app service plan](../../../includes/app-service-web-create-app-service-plan-linux-no-h.md)]

### <a name="create-web-app"></a>Een web-app maken

Maak in de Cloud Shell een [web-app](app-service-linux-intro.md) in het `myAppServicePlan`App Service-plan met de opdracht [`az webapp create`](/cli/azure/webapp?view=azure-cli-latest#az-webapp-create). Vervang  _\<app-naam >_ met een unieke app-naam en  _\<azure-container-register-name >_ met de registernaam van uw.

```azurecli-interactive
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app-name> --deployment-container-image-name <azure-container-registry-name>.azurecr.io/mydockerimage:v1.0.0
```

Wanneer de web-app is gemaakt, toont de Azure CLI soortgelijke uitvoer als in het volgende voorbeeld:

```json
{
  "availabilityState": "Normal",
  "clientAffinityEnabled": true,
  "clientCertEnabled": false,
  "cloningInfo": null,
  "containerSize": 0,
  "dailyMemoryTimeQuota": 0,
  "defaultHostName": "<app-name>.azurewebsites.net",
  "deploymentLocalGitUrl": "https://<username>@<app-name>.scm.azurewebsites.net/<app-name>.git",
  "enabled": true,
  < JSON data removed for brevity. >
}
```

### <a name="configure-registry-credentials-in-web-app"></a>Registerreferenties in web-app configureren

Voor App Service voor het ophalen van de persoonlijke installatiekopie, moet deze informatie over uw register en de installatiekopie. In de Cloud Shell, bieden ze met de [ `az webapp config container set` ](/cli/azure/webapp/config/container?view=azure-cli-latest#az-webapp-config-container-set) opdracht. Vervang  *\<app-naam >*,  *\<azure-container-register-name >*,  _\<register-username >_, en  _\<wachtwoord >_.

```azurecli-interactive
az webapp config container set --name <app-name> --resource-group myResourceGroup --docker-custom-image-name <azure-container-registry-name>.azurecr.io/mydockerimage:v1.0.0 --docker-registry-server-url https://<azure-container-registry-name>.azurecr.io --docker-registry-server-user <registry-username> --docker-registry-server-password <password>
```

> [!NOTE]
> Bij het gebruik van een register dan Docker Hub, `--docker-registry-server-url` moeten zijn opgemaakt als `https://` gevolgd door de volledig gekwalificeerde domeinnaam van het register.
>

### <a name="configure-environment-variables"></a>Omgevingsvariabelen configureren

De meeste Docker-installatiekopieën gebruiken aangepaste omgevingsvariabelen, zoals een andere poort dan 80. Instrueert u App Service over de poort waarvan uw installatiekopie met behulp van gebruikmaakt de `WEBSITES_PORT` app-instelling. De GitHub-pagina voor het [Python-voorbeeld in deze zelfstudie](https://github.com/Azure-Samples/docker-django-webapp-linux) laat zien dat u `WEBSITES_PORT` in moet stellen op _8000_.

Gebruik de opdracht [`az webapp config appsettings set`](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az-webapp-config-appsettings-set) in Cloud Shell om de app-instellingen in te stellen. App-instellingen zijn hoofdlettergevoelig en door spaties gescheiden.

```azurecli-interactive
az webapp config appsettings set --resource-group myResourceGroup --name <app-name> --settings WEBSITES_PORT=8000
```

### <a name="test-the-web-app"></a>De web-app testen

Controleer of de web-app werkt door erheen te bladeren (`http://<app-name>.azurewebsites.net`).

> [!NOTE]
> De eerste keer dat u toegang de app tot, kan dit de enige tijd duren omdat App Service nodig heeft om op te halen van de volledige afbeelding. Als er is een time-out opgetreden voor de browser, alleen de pagina te vernieuwen.

![Poortconfiguratie voor web-app testen](./media/app-service-linux-using-custom-docker-image/app-service-linux-browse-azure.png)

## <a name="change-web-app-and-redeploy"></a>De web-app bijwerken en opnieuw implementeren

Open in uw lokale Git-opslagplaats app/templates/app/index.html. Zoek naar het eerste HTML-element en wijzig het in.

```python
<nav class="navbar navbar-inverse navbar-fixed-top">
    <div class="container">
      <div class="navbar-header">
        <a class="navbar-brand" href="#">Azure App Service - Updated Here!</a>
      </div>
    </div>
  </nav>
```

Nadat u het Python-bestand hebt gewijzigd en dit hebt opgeslagen, moet u de nieuwe Docker-installatiekopie opnieuw opbouwen en pushen. Start de web-app opnieuw om de wijzigingen door te voeren. Gebruik dezelfde opdrachten die u eerder in deze zelfstudie hebt gebruikt. U kunt verwijzen naar [bouwen van een installatiekopie van het Docker-bestand](#build-the-image-from-the-docker-file) en [installatiekopie pushen naar Azure Container Registry](#push-image-to-azure-container-registry). De web-app testen door de instructies te volgen in [De web-app testen](#test-the-web-app).

## <a name="access-diagnostic-logs"></a>Toegang tot diagnostische logboeken

[!INCLUDE [Access diagnostic logs](../../../includes/app-service-web-logs-access-no-h.md)]

## <a name="enable-ssh-connections"></a>SSH-verbindingen inschakelen

SSH maakt veilige communicatie tussen een container en een client mogelijk. Om in te schakelen SSH-verbinding naar de container, moet uw aangepaste installatiekopie voor deze worden geconfigureerd. Laten we eens de voorbeeldopslagplaats die al de vereiste configuratie heeft.

* In de [Dockerfile](https://github.com/Azure-Samples/docker-django-webapp-linux/blob/master/Dockerfile), de volgende code wordt de SSH-server geïnstalleerd en stelt u ook de aanmeldingsreferenties.

    ```Dockerfile
    ENV SSH_PASSWD "root:Docker!"
    RUN apt-get update \
            && apt-get install -y --no-install-recommends dialog \
            && apt-get update \
      && apt-get install -y --no-install-recommends openssh-server \
      && echo "$SSH_PASSWD" | chpasswd 
    ```

    > [!NOTE]
    > Deze configuratie staat geen externe verbindingen naar de container toe. SSH is alleen beschikbaar via de Kudu/SCM-Site. De Kudu/SCM-site wordt geverifieerd met uw Azure-account.

* De [Dockerfile](https://github.com/Azure-Samples/docker-django-webapp-linux/blob/master/Dockerfile#L18) kopieën de [sshd_config](https://github.com/Azure-Samples/docker-django-webapp-linux/blob/master/sshd_config file in the repository) naar de */etc/ssh/* directory.

    ```Dockerfile
    COPY sshd_config /etc/ssh/
    ```

* De [Dockerfile](https://github.com/Azure-Samples/docker-django-webapp-linux/blob/master/Dockerfile#L22) toont poort 2222 in de container. Het is een interne poort die alleen toegankelijk is voor containers in het brugnetwerk van een particulier virtueel netwerk. 

    ```Dockerfile
    EXPOSE 8000 2222
    ```

* De [vermelding script](https://github.com/Azure-Samples/docker-django-webapp-linux/blob/master/init.sh#L5) de SSH-server wordt gestart.

      ```bash
      #!/bin/bash
      service ssh start
    ```

### Open SSH connection to container

SSH connection is available only through the Kudu site, which is accessible at `https://<app-name>.scm.azurewebsites.net`.

To connect, browse to `https://<app-name>.scm.azurewebsites.net/webssh/host` and sign in with your Azure account.

You are then redirected to a page displaying an interactive console.

You may wish to verify that certain applications are running in the container. To inspect the container and verify running processes, issue the `top` command at the prompt.

```bash
top
```

De opdracht `top` toont alle actieve processen in een container.

```
PID USER      PR  NI    VIRT    RES    SHR S %CPU %MEM     TIME+ COMMAND
 1 root      20   0  945616  35372  15348 S  0.0  2.1   0:04.63 node
20 root      20   0   55180   2776   2516 S  0.0  0.2   0:00.00 sshd
42 root      20   0  944596  33340  15352 S  0.0  1.9   0:05.80 node /opt/s+
56 root      20   0   59812   5244   4512 S  0.0  0.3   0:00.93 sshd
58 root      20   0   20228   3128   2664 S  0.0  0.2   0:00.00 bash
62 root      20   0   21916   2272   1944 S  0.0  0.1   0:03.15 top
63 root      20   0   59812   5344   4612 S  0.0  0.3   0:00.03 sshd
65 root      20   0   20228   3140   2672 S  0.0  0.2   0:00.00 bash
71 root      20   0   59812   5380   4648 S  0.0  0.3   0:00.02 sshd
73 root      20   0   20228   3160   2696 S  0.0  0.2   0:00.00 bash
77 root      20   0   21920   2304   1972 R  0.0  0.1   0:00.00 top
```

Gefeliciteerd! U kunt een aangepaste Linux-container hebt geconfigureerd in App Service.

[!INCLUDE [Clean-up section](../../../includes/cli-script-clean-up.md)]

## <a name="next-steps"></a>Volgende stappen

Wat u hebt geleerd:

> [!div class="checklist"]
> * Een aangepaste installatiekopie implementeren in een privécontainerregister
> * De aangepaste installatiekopie uitvoert in App Service
> * Omgevingsvariabelen configureren
> * Bijwerken en opnieuw implementeren van de installatiekopie
> * Toegang tot diagnostische logboeken
> * Verbinding maken met de container via SSH

Ga door naar de volgende zelfstudie om te leren hoe u een aangepaste DNS-naam aan uw app kunt toewijzen.

> [!div class="nextstepaction"]
> [Zelfstudie: Aangepaste DNS-naam toewijzen aan uw app](../app-service-web-tutorial-custom-domain.md)

Of Ga naar andere resources:

> [!div class="nextstepaction"]
> [Aangepaste container configureren](configure-custom-container.md)

> [!div class="nextstepaction"]
> [Zelfstudie: Meerdere containers WordPress-app](tutorial-multi-container-app.md)
