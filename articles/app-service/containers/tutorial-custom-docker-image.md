---
title: Gebruik een aangepaste Docker-afbeelding voor Web-App voor Containers - Azure | Microsoft Docs
description: Klik hier voor meer informatie over het gebruik van een aangepaste Docker-afbeelding voor Web-App voor Containers.
keywords: Azure app service, web-app, linux, docker, container
services: app-service
documentationcenter: 
author: SyntaxC4
manager: SyntaxC4
editor: 
ms.assetid: b97bd4e6-dff0-4976-ac20-d5c109a559a8
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 10/24/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: 08503a7f6f32125c324173636dbda0548f3ccb8c
ms.sourcegitcommit: 7f1ce8be5367d492f4c8bb889ad50a99d85d9a89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2017
---
# <a name="use-a-custom-docker-image-for-web-app-for-containers"></a>Gebruik een aangepaste Docker-afbeelding voor Web-App voor Containers

[Web-App voor Containers](app-service-linux-intro.md) biedt ingebouwde Docker-installatiekopieën op Linux met ondersteuning voor specifieke versies, zoals PHP 7.0 en Node.js 4.5. Web-App voor Containers gebruikt de Docker-container-technologie voor het hosten van zowel ingebouwde installatiekopieën en aangepaste installatiekopieën als een platform als een service. In deze zelfstudie leert u hoe u een aangepaste Docker-installatiekopie bouwen en deze implementeren in de Web-App voor Containers. Dit patroon is handig als de ingebouwde afbeeldingen zijn niet opgenomen in de taal van keuze, of wanneer uw toepassing vereist een specifieke configuratie die nog niet in de ingebouwde afbeeldingen.

## <a name="prerequisites"></a>Vereisten

Voor deze zelfstudie hebt u het volgende nodig:

* [Git](https://git-scm.com/downloads)
* Een actieve [Azure-abonnement](https://azure.microsoft.com/pricing/free-trial/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
* [Docker](https://docs.docker.com/get-started/#setup)
* Een [Docker-Hub-account](https://docs.docker.com/docker-id/)

[!INCLUDE [Free trial note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="download-the-sample"></a>Het voorbeeld downloaden

Voer de volgende opdracht om een kloon de opslagplaats van de voorbeeld-app op uw lokale computer en wijzigt u vervolgens naar de map waarin zich de voorbeeldcode in een terminalvenster.

```bash
git clone https://github.com/Azure-Samples/docker-django-webapp-linux.git --config core.autocrlf=input
cd docker-django-webapp-linux
```

## <a name="build-the-image-from-the-docker-file"></a>De installatiekopie maken van de Docker-bestand

In de Git-opslagplaats te kijken hoe _Dockerfile_. Dit bestand beschrijft de Python-omgeving die vereist is voor uw toepassing uitvoeren. Bovendien de installatiekopie van het stelt u een [SSH](https://www.ssh.com/ssh/protocol/) server voor veilige communicatie tussen de container en de host.

```docker
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

De Docker-installatiekopie bouwen, voer de `docker build` opdracht in en geef een naam `mydockerimage`, en de tag `v1.0.0`. Vervang `<docker-id>` rekening met uw Hub Docker ID.

```bash
docker build --tag <docker-id>/mydockerimage:v1.0.0 .
```

De opdracht geeft de uitvoer ziet er als volgt:

```
# The output from the commands in this article has been shortened for brevity.

Sending build context to Docker daemon  5.558MB
Step 1/13 : FROM python:3.4
 ---> 9ff45ddb54e9
Step 2/13 : RUN mkdir /code
 ---> Using cache
 ---> f3f3ac01db0a
Step 3/13 : WORKDIR /code
 ---> Using cache
 ---> 38b32f15b442
.
.
.
Step 13/13 : ENTRYPOINT init.sh
 ---> Running in 5904e4c70043
 ---> e7cf08275692
Removing intermediate container 5904e4c70043
Successfully built e7cf08275692
Successfully tagged cephalin/mydockerimage:v1.0.0
```

Test of de build werkt door het uitvoeren van de Docker-container. Probleem de [docker uitvoeren](https://docs.docker.com/engine/reference/commandline/run/) opdracht en de naam en het label van de afbeelding doorgeven. Zorg ervoor dat u de poort met behulp van de `-p` argument.

```bash
docker run -p 2222:8000 <docker-ID>/mydockerimage:v1.0.0
```

Controleer of de web-app en de container goed werken door te bladeren naar `http://localhost:2222`.

![Test web-app lokaal](./media/app-service-linux-using-custom-docker-image/app-service-linux-browse-local.png)

## <a name="push-the-docker-image-to-docker-hub"></a>De installatiekopie van het Docker push naar Docker-Hub

Een register is een toepassing die als host fungeert voor installatiekopieën en biedt services voor services en de container. Om uw installatiekopie delen, moet u dit doorgeven aan een register. 

<!-- Depending on your requirements, you may have your docker images in a Public Docker Registry, such as Docker Hub, or a Private Docker Registry such as Azure Container Registry. Select the appropriate tab for your scenario below (your selection will switch multiple tabs on this page). -->

> [!NOTE]
> Met een persoonlijke Docker-register pushen? Zie de optionele instructies voor het [Push een Docker-installatiekopie naar persoonlijke register](#push-a-docker-image-to-private-registry-optional).

<!--## [Docker Hub](#tab/docker-hub)-->

Docker-Hub is een register voor de Docker-installatiekopieën waarmee u uw eigen opslagplaatsen, openbare of particuliere hosten. Voor het pushen van een aangepaste Docker-installatiekopie naar de openbare Docker-Hub, gebruiken de [docker push](https://docs.docker.com/engine/reference/commandline/push/) opdracht in en geef een naam van de volledige installatiekopie en de tag. Een naam voor de volledige installatiekopie en de tag lijkt op het volgende voorbeeld:

```
<docker-id>/image-name:tag
```

Als u dit nog niet hebt aangemeld met Docker-Hub, moet u doen met behulp van de [docker aanmelding](https://docs.docker.com/engine/reference/commandline/login/) voordat u probeert om een installatiekopie van een opdracht.

```bash
docker login --username <docker-id> --password <docker-hub-password>
```

Een bericht 'aanmelding geslaagd' wordt bevestigd dat u bent aangemeld. Nadat u bent aangemeld, kunt u de installatiekopie van het push naar met behulp van Docker-Hub het [docker push](https://docs.docker.com/engine/reference/commandline/push/) opdracht.

```bash
docker push <docker-id>/mydockerimage:v1.0.0
```

Controleer of dat de push-bewerking is voltooid door in de opdracht-uitvoer.

```
The push refers to a repository [docker.io/<docker-id>/mydockerimage:v1.0.0]
c33197c3f6d4: Pushed
ccd2c850ee43: Pushed
02dff2853466: Pushed
6ce78153632a: Pushed
efef3f03cc58: Pushed
3439624d77fb: Pushed
3a07adfb35c5: Pushed
2fcec228e1b7: Mounted from library/python
97d2d3bae505: Mounted from library/python
95aadeabf504: Mounted from library/python
b456afdc9996: Mounted from library/python
d752a0310ee4: Mounted from library/python
db64edce4b5b: Mounted from library/python
d5d60fc34309: Mounted from library/python
c01c63c6823d: Mounted from library/python
v1.0.0: digest: sha256:21f2798b20555f4143f2ca0591a43b4f6c8138406041f2d32ec908974feced66 size: 3676
```

<!--
# [Private Registry](#tab/private-registry)

// Move Private Registry instructions here when Tabbed Conceptual bug is fixed

---
-->

[!INCLUDE [Try Cloud Shell](../../../includes/cloud-shell-try-it.md)]

## <a name="deploy-app-to-azure"></a>App implementeren in Azure

U kunt systeemeigen Linux-toepassingen in de cloud hosten met behulp van Azure Web Apps. U kunt Azure CLI-opdrachten die een groep, en vervolgens een service-abonnement, en ten slotte de web-app zelf maken moet uitvoeren voor het maken van een Web-App voor Containers. 

### <a name="create-a-resource-group"></a>Een resourcegroep maken

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group-no-h.md)] 

### <a name="create-a-linux-app-service-plan"></a>Een Linux-App Service-abonnement maken

[!INCLUDE [Create app service plan](../../../includes/app-service-web-create-app-service-plan-linux-no-h.md)] 

### <a name="create-a-web-app"></a>Een webtoepassing maken

Maak in de Cloud Shell met de opdracht [az webapp create](/cli/azure/webapp#create) een [web-app](app-service-linux-intro.md) in het App Service-plan `myAppServicePlan`. Vergeet niet om te vervangen `<app_name>` met een unieke app-naam en < docker-ID > met de Docker-ID.

```azurecli-interactive
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app_name> --deployment-container-image-name <docker-ID>/mydockerimage:v1.0.0
```

Wanneer de web-app is gemaakt, ziet de Azure CLI u uitvoer lijkt op het volgende voorbeeld:

```json
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

### <a name="configure-environment-variables"></a>Omgevingsvariabelen configureren

De meeste Docker-afbeeldingen hebben omgevingsvariabelen die moeten worden geconfigureerd. Als u een bestaande Docker-installatiekopie gemaakt door iemand anders gebruikt, kan de installatiekopie van een andere poort dan 80 te gebruiken. U vertelt Azure over de poort die gebruikmaakt van uw installatiekopie met behulp van de `WEBSITES_PORT` app-instelling. De GitHub-pagina voor de [Python voorbeeld in deze zelfstudie](https://github.com/Azure-Samples/docker-django-webapp-linux) bevat die u nodig hebt om in te stellen `WEBSITES_PORT` naar _8000_.

U kunt app-instellingen instellen met de [az webapp config appsettings bijwerken](/cli/azure/webapp/config/appsettings#update) opdracht in de Cloud-Shell. App-instellingen zijn hoofdlettergevoelig en spaties gescheiden.

```azurecli-interactive
az webapp config appsettings set --resource-group myResourceGroup --name <app_name> --settings WEBSITES_PORT=8000
```

<!-- Depending on your requirements, you may have your docker images in a Public Docker Registry, such as Docker Hub, or a Private Docker Registry, such as Azure Container Registry. Select the appropriate tab for your scenario below: -->

> [!NOTE]
> Implementeren vanaf een particuliere Docker-register? Zie de optionele instructies voor het [Web-App configureren om te gebruiken Docker-container uit een persoonlijke register](#configure-web-app-to-use-docker-container-from-a-private-registry-optional).

<!-- # [Docker Hub](#tab/docker-hub)-->

<!-- # [Private Registry](#tab/private-registry)

// Place Private Registry text back here once Tabbed Conceptual bug is fixed

---
-->

### <a name="test-the-web-app"></a>De web-app testen

Controleer of de web-app werkt door te bladeren naar het (`http://<app_name>azurewebsites.net`). 

![Poortconfiguratie voor test web-app](./media/app-service-linux-using-custom-docker-image/app-service-linux-browse-azure.png)

## <a name="change-web-app-and-redeploy"></a>Web-app wijzigen en opnieuw distribueren

Open in uw lokale Git-opslagplaats app/templates/app/index.html. Zoek naar het eerste element van de HTML- en deze te wijzigen.

```python
<nav class="navbar navbar-inverse navbar-fixed-top">
    <div class="container">
      <div class="navbar-header">         
        <a class="navbar-brand" href="#">Azure App Service - Updated Here!</a>       
      </div>            
    </div>
  </nav> 
```

Nadat u hebt de Python-bestand is gewijzigd en dit hebt opgeslagen, moet u opnieuw opbouwen en de installatiekopie van het nieuwe Docker push. Start de web-app om de wijzigingen van kracht te laten worden. Gebruik dezelfde opdrachten die u eerder in deze zelfstudie hebt gebruikt. U kunt verwijzen naar [bouwen van een installatiekopie van het bestand Docker](#build-the-image-from-the-docker-file) en [Push de Docker-installatiekopie op de Hub Docker](#push-the-docker-image-to-docker-hub). De web-app testen door de instructies in [testen van de web-app](#test-the-web-app).

## <a name="connect-to-web-app-for-containers-using-ssh"></a>Verbinding maken met de Web-App voor Containers via SSH

SSH kunt veilige communicatie tussen een container en een client. Om een aangepaste installatiekopie Docker ter ondersteuning van SSH, moet u deze in een Dockerfile maken. Schakelt u SSH in de Docker-bestand zelf. De SSH-instructies zijn al toegevoegd aan de dockerfile voorbeeld zodat u deze instructies met uw eigen aangepaste installatiekopie volgen kunt:

* Een [uitvoeren](https://docs.docker.com/engine/reference/builder/#run) instructie roept `apt-get`, wordt het wachtwoord voor het hoofdaccount op `"Docker!"`.

    ```docker
    ENV SSH_PASSWD "root:Docker!"
    RUN apt-get update \
            && apt-get install -y --no-install-recommends dialog \
            && apt-get update \
      && apt-get install -y --no-install-recommends openssh-server \
      && echo "$SSH_PASSWD" | chpasswd 
    ```

    > [!NOTE]
    > Deze configuratie is niet toegestaan voor externe verbindingen naar de container. SSH is alleen beschikbaar via de Kudu/SCM-Site. De Kudu/SCM-site is geverifieerd met de publicatie referenties.

* Een [kopie](https://docs.docker.com/engine/reference/builder/#copy) instructie de Docker-engine kopiëren zodat de [sshd_config](http://man.openbsd.org/sshd_config) van het bestand in de */enzovoort/ssh/* directory. Het configuratiebestand moet worden gebaseerd op [dit bestand sshd_config](https://github.com/Azure-App-Service/node/blob/master/6.11.1/sshd_config).

    ```docker
    COPY sshd_config /etc/ssh/
    ```

    > [!NOTE]
    > De *sshd_config* bestand moet bevatten de volgende items: 
    > * `Ciphers`moet ten minste één item in deze lijst bevatten: `aes128-cbc,3des-cbc,aes256-cbc`.
    > * `MACs`moet ten minste één item in deze lijst bevatten: `hmac-sha1,hmac-sha1-96`.

* Een [BLOOTSTELLEN](https://docs.docker.com/engine/reference/builder/#expose) instructie dat gegarandeerd 2222 in de container poort. Hoewel het hoofdwachtwoord bekend is, wordt poort 2222 niet toegankelijk vanaf internet. Een interne poort die door containers in het netwerk bridge van een virtueel particulier netwerk alleen toegankelijk is. Hierna is opdrachten SSH configuratiedetails kopiëren en start de `ssh` service.

    ```docker
    EXPOSE 8000 2222

    RUN service ssh start
    ```

### <a name="open-ssh-connection-to-container"></a>SSH verbinding met de container openen

Web-App voor Containers is niet toegestaan voor externe verbindingen naar de container. SSH is alleen beschikbaar via de Kudu-site, die toegankelijk is op `https://<app_name>.scm.azurewebsites.net`.

Als u wilt verbinden, bladert u naar `https://<app_name>.scm.azurewebsites.net/webssh/host` en meld u aan met uw Azure-account.

U wordt vervolgens omgeleid naar een pagina weergegeven met een interactieve console. 

U wilt controleren of dat bepaalde toepassingen worden uitgevoerd in de container. Om te controleren van de container en controleren van de actieve processen, geven de `top` opdracht bij de opdrachtprompt.

```bash
top
```

De `top` opdracht toont alle actieve processen in een container.

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

Gefeliciteerd. U kunt een aangepaste Docker-installatiekopie hebt geconfigureerd voor een Web-App voor Containers.

## <a name="use-a-private-image-from-docker-hub-optional"></a>Gebruik een persoonlijke afbeelding van Docker-Hub (optioneel)

In [maken van een web-app](#create-a-web-app), u een installatiekopie van een opgegeven op de Docker-Hub in de `az webapp create` opdracht. Dit is geschikt voor een openbare-installatiekopie. Voor het gebruik van een installatiekopie van een persoonlijke, moet u uw Docker-account-ID en wachtwoord in uw Azure-web-app configureren.

In de Cloud-Shell, voert u de `az webapp create` opdracht met [az webapp config container set](/cli/azure/webapp/config/container#az_webapp_config_container_set). Vervang  *\<app_naam >*, en ook _< docker-id >_ en  _<password>_  met uw Docker-ID en wachtwoord.

```azurecli-interactive
az webapp config container set --name <app_name> --resource-group myResourceGroup --docker-registry-server-user <docker-id> --docker-registry-server-password <password>
```

De opdracht wordt de uitvoer is vergelijkbaar met de volgende JSON-tekenreeks met wijzigen van de configuratie is voltooid:

```json
[
  {
    "name": "WEBSITES_ENABLE_APP_SERVICE_STORAGE",
    "slotSetting": false,
    "value": "false"
  },
  {
    "name": "DOCKER_REGISTRY_SERVER_USERNAME",
    "slotSetting": false,
    "value": "<docker-id>"
  },
  {
    "name": "DOCKER_REGISTRY_SERVER_PASSWORD",
    "slotSetting": false,
    "value": null
  },
  {
    "name": "DOCKER_CUSTOM_IMAGE_NAME",
    "value": "DOCKER|<image-name-and-tag>"
  }
]
```

## <a name="use-a-docker-image-from-any-private-registry-optional"></a>Gebruik een Docker-installatiekopie van een persoonlijke register (optioneel)

In dit gedeelte vindt u informatie over het gebruik van een Docker-installatiekopie van een persoonlijke register in Web-App voor Containers en Azure Container register wordt gebruikt als een voorbeeld. De stappen voor het gebruik van andere persoonlijke registers lijken. 

Register met Azure Container is een beheerde Docker-service van Azure voor het hosten van persoonlijke installatiekopieën. De implementaties mogelijk elk type, met inbegrip van [Docker Swarm](https://docs.docker.com/engine/swarm/), [Kubernetes](https://kubernetes.io/), en Web-App voor Containers. 

### <a name="create-an-azure-container-registry"></a>Een Azure Container Registry maken

In de Cloud-Shell gebruiken de [az acr maken](https://docs.microsoft.com/cli/azure/acr#az_acr_create) opdracht voor het maken van een Azure Container-register. In de naam van de resourcegroep, doorgeven en `Basic` voor de SKU. Beschikbare SKU's zijn `Classic`, `Basic`, `Standard`, en `Premium`.

```azurecli-interactive
az acr create --name <azure-container-registry-name> --resource-group myResourceGroup --sku Basic --admin-enabled true
```

Een container maken, wordt de volgende uitvoer:

```
 - Finished ..
Create a new service principal and assign access:
  az ad sp create-for-rbac --scopes /subscriptions/resourceGroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/<azure-container-registry-name> --role Owner --password <password>

Use an existing service principal and assign access:
  az role assignment create --scope /subscriptions/resourceGroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/<azure-container-registry-name> --role Owner --assignee <app-id>
{
  "adminUserEnabled": false,
  "creationDate": "2017-08-09T04:21:09.654153+00:00",
  "id": "/subscriptions/<subscriptionId>/resourceGroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/<azure-container-registry-name>",
  "location": "westeurope",
  "loginServer": "<azure-container-registry-name>.azurecr.io",
  "name": "<azure-container-registry-name>",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "sku": {
    "name": "Basic",
    "tier": "Basic"
  },
  "storageAccount": {
    "name": "myazurecontainerre042025"
  },
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}
```

### <a name="log-in-to-azure-container-registry"></a>Aanmelden bij Azure Container register

Om te pushen een afbeelding in het register, moet u referenties opgeven zodat het register de push-bewerking accepteert. U kunt deze referenties ophalen met behulp van de [az acr weergeven](https://docs.microsoft.com/cli/azure/acr/credential#az_acr_credential_show) opdracht in de Cloud-Shell. 

```azurecli-interactive
az acr credential show --name <azure-container-registry-name>
```

De opdracht geeft twee wachtwoorden die kunnen worden gebruikt met de naam van de gebruiker.

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

Vanuit uw lokale terminalvenster, meld u aan bij de Azure-Container register met behulp van de `docker login` opdracht. Naam van de server is vereist om aan te melden. Gebruik de notatie `{azure-container-registry-name>.azurecr.io`.

```bash
docker login <azure-container-registry-name>.azurecr.io --username <registry-username> --password <password> 
```

Bevestig dat de aanmelding is geslaagd. 

### <a name="push-an-image-to-azure-container-registry"></a>Een installatiekopie van een Azure Container register push

> [!NOTE]
> Als u uw eigen installatiekopie, labelen u de afbeelding als volgt:
> ```bash
> docker tag <azure-container-registry-name>.azurecr.io/mydockerimage
> ```

Push de installatiekopie met behulp van de `docker push` opdracht. De installatiekopie met de naam van het register, gevolgd door de installatiekopie met de naam en de tag-code.

```bash
docker push <azure-container-registry-name>.azurecr.io/mydockerimage:v1.0.0
```

Controleer of de push-bewerking is toegevoegd een container in het register door de ACR opslagplaatsen te bieden. 

```azurecli-interactive
az acr repository list -n <azure-container-registry-name>
```

De afbeeldingen in het register aanbieding bevestigt dat `mydockerimage` is in het register.

```json
[
  "mydockerimage"
]
```

### <a name="configure-web-app-to-use-the-image-from-azure-container-registry-or-any-private-registry"></a>Web-App voor het gebruik van een installatiekopie van het register van Azure-Container (of een persoonlijke register) configureren

U kunt de Web-App voor Containers configureren zodat deze een container die is opgeslagen in het register van de Container Azure wordt uitgevoerd. Net als bij alle persoonlijke register, dus als u wilt gebruiken, uw eigen persoonlijke register, de stappen voor het voltooien van deze taak zijn vergelijkbaar is met het Azure-Container-register.

Voer in de Cloud-Shell [az acr referentie weergeven](/cli/azure/acr/credential#az_acr_credential_show) om de gebruikersnaam en het wachtwoord voor het Azure Container register weer te geven. De gebruikersnaam en een van de wachtwoorden kopiëren zodat u deze gebruiken kunt voor het configureren van de web-app in de volgende stap.

```bash
az acr credential show --name <azure-container-registry-name>
```

```json
{
  "passwords": [
    {
      "name": "password",
      "value": "password"
    },
    {
      "name": "password2",
      "value": "password2"
    }
  ],
  "username": "<registry-username>"
}
```

De Cloud-Shell, start de [az webapp config container set](/cli/azure/webapp/config/container#az_webapp_config_container_set) opdracht voor het toewijzen van de aangepaste Docker-installatiekopie aan de web-app. Vervang  *\<app_naam >*,  *\<docker-register-server-url >*,  _\<register-username >_, en  _\<wachtwoord >_. Voor Azure Container register,  *\<docker-register-server-url >* heeft de indeling `https://<azure-container-registry-name>.azurecr.io`. 

```azurecli-interactive
az webapp config container set --name <app_name> --resource-group myResourceGroup --docker-custom-image-name mydockerimage --docker-registry-server-url https://<azure-container-registry-name>.azurecr.io --docker-registry-server-user <registry-username> --docker-registry-server-password <password>
```

> [!NOTE]
> `https://`is vereist in  *\<docker-register-server-url >*.
>

De opdracht wordt de uitvoer is vergelijkbaar met de volgende JSON-tekenreeks met wijzigen van de configuratie is voltooid:

```json
[
  {
    "name": "DOCKER_CUSTOM_IMAGE_NAME",
    "slotSetting": false,
    "value": "mydockerimage"
  },
  {
    "name": "DOCKER_REGISTRY_SERVER_URL",
    "slotSetting": false,
    "value": "<azure-container-registry-name>.azurecr.io"
  },
  {
    "name": "DOCKER_REGISTRY_SERVER_USERNAME",
    "slotSetting": false,
    "value": "<registry-username>"
  },
  {
    "name": "DOCKER_REGISTRY_SERVER_PASSWORD",
    "slotSetting": false,
    "value": null
  }
]
```

[!INCLUDE [Clean-up section](../../../includes/cli-script-clean-up.md)]

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een Docker Python en PostgreSQL web-app in Azure bouwen](tutorial-docker-python-postgresql-app.md)
