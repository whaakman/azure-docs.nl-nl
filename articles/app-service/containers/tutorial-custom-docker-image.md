---
title: Een aangepaste Docker-installatiekopie gebruiken voor Web App for Containers - Azure | Microsoft Docs
description: Een aangepaste Docker-installatiekopie voor Web App for Containers gebruiken.
keywords: azure-app-service, web-app, linux, docker, container
services: app-service
documentationcenter: ''
author: SyntaxC4
manager: SyntaxC4
editor: ''
ms.assetid: b97bd4e6-dff0-4976-ac20-d5c109a559a8
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 10/24/2017
ms.author: cfowler
ms.custom: mvc
ms.openlocfilehash: acfe066d9ad4882bcff85b7fd51dc7d3b2278235
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/16/2018
---
# <a name="use-a-custom-docker-image-for-web-app-for-containers"></a>Een aangepaste Docker-installatiekopie gebruiken voor Web App for Containers

[Web App for Containers](app-service-linux-intro.md) biedt ingebouwde Docker-installatiekopieën op Linux met ondersteuning voor specifieke versies, zoals PHP 7.0 en Node.js 4.5. Web App for Containers gebruikt de Docker-containertechnologie voor het hosten van zowel ingebouwde installatiekopieën als aangepaste installatiekopieën, als een platform en als een service. In deze zelfstudie leert u hoe u een aangepaste Docker-installatiekopie moet bouwen en deze moet implementeren in de Web App for Containers. Dit patroon is handig als de taal van keuze niet is inbegrepen in de ingebouwde installatiekopieën, of wanneer uw toepassing een specifieke configuratie vereist die niet door de ingebouwde installatiekopieën wordt verschaft.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een aangepaste Docker-installatiekopie implementeren in Azure
> * Configureren van omgevingsvariabelen om de container uit te voeren
> * De Docker-installatiekopie bijwerken en opnieuw implementeren
> * Verbinding maken met de container via SSH
> * Een persoonlijke Docker-installatiekopie implementeren in Azure

[!INCLUDE [Free trial note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Vereisten

Voor deze zelfstudie hebt u het volgende nodig:

* [Git](https://git-scm.com/downloads)
* Een actief [Azure-abonnement](https://azure.microsoft.com/pricing/free-trial/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
* [Docker](https://docs.docker.com/get-started/#setup)
* Een [Docker Hub-account](https://docs.docker.com/docker-id/)

## <a name="download-the-sample"></a>Het voorbeeld downloaden

Voer de volgende opdracht uit in een terminalvenster. Hiermee wordt de voorbeeld-app-opslagplaats gekloond naar uw lokale computer en navigeert u naar de map met de voorbeeldcode.

```bash
git clone https://github.com/Azure-Samples/docker-django-webapp-linux.git --config core.autocrlf=input
cd docker-django-webapp-linux
```

## <a name="build-the-image-from-the-docker-file"></a>De installatiekopie maken van het Docker-bestand

Bekijk in de Git-opslagplaats het _Docker-bestand_. Dit bestand beschrijft de Python-omgeving die vereist is om uw toepassing uit te voeren. De installatiekopie stelt bovendien een [SSH](https://www.ssh.com/ssh/protocol/)-server in voor veilige communicatie tussen de container en de host.

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

Voer om de Docker-installatiekopie te bouwen de `docker build` opdracht in en geef een naam, _mydockerimage_, op en de tag _v1.0.0_. Vervang _\<docker-id>_ met de ID van uw Docker Hub-account.

```bash
docker build --tag <docker-id>/mydockerimage:v1.0.0 .
```

De opdracht geeft een uitvoer die er ongeveer als volgt uitziet:

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

Test of de build werkt door de Docker-container uit te voeren. Geef de opdracht [`docker run`](https://docs.docker.com/engine/reference/commandline/run/) en geef de naam en het label van de afbeelding door. Zorg ervoor dat u de poort specificeert met behulp van het argument `-p`.

```bash
docker run -p 2222:8000 <docker-ID>/mydockerimage:v1.0.0
```

Controleer of de web-app en de container goed werken door naar `http://localhost:2222` te bladeren.

![Web-app lokaal testen](./media/app-service-linux-using-custom-docker-image/app-service-linux-browse-local.png)

## <a name="push-the-docker-image-to-docker-hub"></a>Push de Docker-installatiekopie naar Docker Hub

Een register is een toepassing die als host fungeert voor installatiekopieën en installatiekopie- en containerservices biedt. Om uw installatiekopie te delen, moet u deze naar een register pushen. 

<!-- Depending on your requirements, you may have your docker images in a Public Docker Registry, such as Docker Hub, or a Private Docker Registry such as Azure Container Registry. Select the appropriate tab for your scenario below (your selection will switch multiple tabs on this page). -->

> [!NOTE]
> Naar een persoonlijk Docker-register pushen? Zie de optionele instructies voor het [gebruik van een Docker-installatiekopie uit een persoonlijke register](#use-a-docker-image-from-any-private-registry-optional).

<!--## [Docker Hub](#tab/docker-hub)-->

Docker Hub is een register voor de Docker-installatiekopieën waarmee u uw eigen openbare of particuliere opslagplaatsen kunt hosten. Gebruik voor het pushen van een aangepaste Docker-installatiekopie naar de openbare Docker Hub de opdracht [`docker push`](https://docs.docker.com/engine/reference/commandline/push/) en geef een volledige naam en tag voor de installatiekopie. Een volledige naam en tag voor de installatiekopie lijkt op het volgende voorbeeld:

```
<docker-id>/image-name:tag
```

Voordat u een installatiekopie kunt pushen, moet u zich aanmelden bij Docker-Hub met behulp van de opdracht [`docker login`](https://docs.docker.com/engine/reference/commandline/login/). Vervang _\<docker-id>_ met uw accountnaam en typ uw wachtwoord bij de opdrachtprompt in de console.

```bash
docker login --username <docker-id>
```

Een bericht 'aanmelding geslaagd' bevestigt dat u bent aangemeld. Nadat u bent aangemeld, kunt u de installatiekopie naar Docker Hub pushen met de opdracht [`docker push`](https://docs.docker.com/engine/reference/commandline/push/).

```bash
docker push <docker-id>/mydockerimage:v1.0.0
```

Controleer of de push-bewerking is voltooid door de opdrachtuitvoer te inspecteren.

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

## <a name="deploy-app-to-azure"></a>App in Azure implementeren

U kunt systeemeigen Linux-toepassingen in de cloud hosten met behulp van Azure Web Apps. Om een Web App for Containers te maken, moet u Azure CLI-opdrachten uitvoeren die een groep maken, vervolgens een serviceabonnement, en ten slotte de web-app zelf. 

### <a name="create-a-resource-group"></a>Een resourcegroep maken

[!INCLUDE [Create resource group](../../../includes/app-service-web-create-resource-group-linux-no-h.md)] 

### <a name="create-a-linux-app-service-plan"></a>Een Linux App Service-abonnement maken

[!INCLUDE [Create app service plan](../../../includes/app-service-web-create-app-service-plan-linux-no-h.md)] 

### <a name="create-a-web-app"></a>Een webtoepassing maken

Maak in de Cloud Shell een [web-app](app-service-linux-intro.md) in het `myAppServicePlan`App Service-plan met de opdracht [`az webapp create`](/cli/azure/webapp?view=azure-cli-latest#az_webapp_create). Vergeet niet om _<appname>_ te vervangen met een unieke app-naam en _\<docker-ID>_ met uw Docker-ID.

```azurecli-interactive
az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app_name> --deployment-container-image-name <docker-ID>/mydockerimage:v1.0.0
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
  "defaultHostName": "<app_name>.azurewebsites.net",
  "deploymentLocalGitUrl": "https://<username>@<app_name>.scm.azurewebsites.net/<app_name>.git",
  "enabled": true,
  < JSON data removed for brevity. >
}
```

### <a name="configure-environment-variables"></a>Omgevingsvariabelen configureren

De meeste Docker-afbeeldingen hebben omgevingsvariabelen die moeten worden geconfigureerd. Als u een bestaande Docker-installatiekopie gebruikt die is gemaakt door iemand anders, gebruikt de installatiekopie mogelijk een andere poort dan 80. U vertelt Azure over de poort waarvan uw installatiekopie gebruikmaakt met behulp van de app-instelling `WEBSITES_PORT`. De GitHub-pagina voor het [Python-voorbeeld in deze zelfstudie](https://github.com/Azure-Samples/docker-django-webapp-linux) laat zien dat u `WEBSITES_PORT` in moet stellen op _8000_.

Gebruik de opdracht [`az webapp config appsettings set`](/cli/azure/webapp/config/appsettings?view=azure-cli-latest#az_webapp_config_appsettings_set) in Cloud Shell om de app-instellingen in te stellen. App-instellingen zijn hoofdlettergevoelig en door spaties gescheiden.

```azurecli-interactive
az webapp config appsettings set --resource-group myResourceGroup --name <app_name> --settings WEBSITES_PORT=8000
```

<!-- Depending on your requirements, you may have your docker images in a Public Docker Registry, such as Docker Hub, or a Private Docker Registry, such as Azure Container Registry. Select the appropriate tab for your scenario below: -->

> [!NOTE]
> Implementeren vanuit een Persoonlijk Docker-register? Zie de optionele instructies voor het [gebruik van een Docker-installatiekopie uit een persoonlijke register](#use-a-docker-image-from-any-private-registry-optional).

<!-- # [Docker Hub](#tab/docker-hub)-->

<!-- # [Private Registry](#tab/private-registry)

// Place Private Registry text back here once Tabbed Conceptual bug is fixed

---
-->

### <a name="test-the-web-app"></a>De web-app testen

Controleer of de web-app werkt door erheen te bladeren (`http://<app_name>azurewebsites.net`). 

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

Nadat u het Python-bestand hebt gewijzigd en dit hebt opgeslagen, moet u de nieuwe Docker-installatiekopie opnieuw opbouwen en pushen. Start de web-app opnieuw om de wijzigingen door te voeren. Gebruik dezelfde opdrachten die u eerder in deze zelfstudie hebt gebruikt. U kunt verwijzen naar [Een installatiekopie van het Docker-bestand bouwen](#build-the-image-from-the-docker-file) en [De Docker-installatiekopie naar Docker Hub pushen](#push-the-docker-image-to-docker-hub). De web-app testen door de instructies te volgen in [De web-app testen](#test-the-web-app).

## <a name="connect-to-web-app-for-containers-using-ssh"></a>Verbinding maken met Web App for Containers via SSH

SSH maakt veilige communicatie tussen een container en een client mogelijk. Om een aangepaste Docker-installatiekopie SSH te laten ondersteunen, moet u deze in een Docker-bestand compileren. U schakelt SSH in het Docker-bestand zelf in. De SSH-instructies zijn al toegevoegd aan het Docker-voorbeeldbestand, zodat u deze instructies met uw eigen aangepaste installatiekopie kunt volgen:

* Een [RUN](https://docs.docker.com/engine/reference/builder/#run)-instructie die `apt-get` aanroept en vervolgens het wachtwoord instelt voor het hoofdaccount op `"Docker!"`.

    ```docker
    ENV SSH_PASSWD "root:Docker!"
    RUN apt-get update \
            && apt-get install -y --no-install-recommends dialog \
            && apt-get update \
      && apt-get install -y --no-install-recommends openssh-server \
      && echo "$SSH_PASSWD" | chpasswd 
    ```

    > [!NOTE]
    > Deze configuratie staat geen externe verbindingen naar de container toe. SSH is alleen beschikbaar via de Kudu/SCM-Site. De Kudu/SCM-site wordt geverifieerd met de publicatiereferenties.

* Een [COPY](https://docs.docker.com/engine/reference/builder/#copy)-instructie die de Docker-engine opdraagt om het bestand [sshd_config](http://man.openbsd.org/sshd_config) te kopiëren naar de map */etc/ssh/*. Het configuratiebestand moet worden gebaseerd op [dit sshd_config-bestand](https://github.com/Azure-App-Service/node/blob/master/6.11.1/sshd_config).

    ```docker
    COPY sshd_config /etc/ssh/
    ```

    > [!NOTE]
    > Het *sshd_config*-bestand moet de volgende items bevatten: 
    > * `Ciphers`moet ten minste één item in deze lijst bevatten: `aes128-cbc,3des-cbc,aes256-cbc`.
    > * `MACs`moet ten minste één item in deze lijst bevatten: `hmac-sha1,hmac-sha1-96`.

* Een instructie [EXPOSE](https://docs.docker.com/engine/reference/builder/#expose) toont poort 2222 in de container. Hoewel het hoofdwachtwoord bekend is, is poort 2222 niet toegankelijk vanaf het internet. Het is een interne poort die alleen toegankelijk is voor containers in het brugnetwerk van een particulier virtueel netwerk. Hierna kopiëren opdrachten SSH-configuratiedetails en start de service `ssh`.

    ```docker
    EXPOSE 8000 2222
    ```

* Zorg ervoor dat u [de ssh-service start](https://github.com/Azure-App-Service/node/blob/master/6.9.3/startup/init_container.sh) met behulp van een shell-script in de map /bin.
 
    ```bash
    #!/bin/bash
    service ssh start
    ```
     
### <a name="open-ssh-connection-to-container"></a>SSH-verbinding met de container openen

Web App for Containers staat geen externe verbindingen naar de container toe. SSH is alleen beschikbaar via de Kudu-site, die toegankelijk is via `https://<app_name>.scm.azurewebsites.net`.

Als u wilt verbinden, bladert u naar `https://<app_name>.scm.azurewebsites.net/webssh/host` en meldt u zich aan met uw Azure-account.

U wordt vervolgens omgeleid naar een pagina die een interactieve console weergeeft. 

U wilt controleren of bepaalde toepassingen worden uitgevoerd in de container. Om de container te controleren en de actieve processen te verifiëren, geeft u de opdracht `top` bij de opdrachtprompt.

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

Gefeliciteerd. U heeft een aangepaste Docker-installatiekopie geconfigureerd voor een Web App for Containers.

## <a name="use-a-private-image-from-docker-hub-optional"></a>Gebruik een persoonlijke installatiekopie uit Docker Hub (optioneel)

In [Een web app maken](#create-a-web-app) heeft u een installatiekopie opgegeven in Docker Hub in de opdracht `az webapp create`. Dit is geschikt voor een openbare installatiekopie. Voor het gebruik van een persoonlijke installatiekopie dient u uw Docker-account-ID en -wachtwoord in uw Azure-web-app te configureren.

In de Cloud Shell volgt u de opdracht `az webapp create` met [`az webapp config container set`](/cli/azure/webapp/config/container?view=azure-cli-latest#az_webapp_config_container_set). Vervang *\<app_naam>*, en ook _\<docker-id>_ en _\<wachtwoord>_ met uw Docker-ID en -wachtwoord.

```azurecli-interactive
az webapp config container set --name <app_name> --resource-group myResourceGroup --docker-registry-server-user <docker-id> --docker-registry-server-password <password>
```

De opdracht toont uitvoer die vergelijkbaar is met de volgende JSON-tekenreeks, die laat zien dat het wijzigen van de configuratie is voltooid:

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

## <a name="use-a-docker-image-from-any-private-registry-optional"></a>Een Docker-installatiekopie uit een persoonlijk register gebruiken (optioneel)

In dit gedeelte vindt u informatie over het gebruik van een Docker-installatiekopie uit een persoonlijk register in Web App for Containers. Azure Container Registry wordt gebruikt als voorbeeld. De stappen voor het gebruik van andere persoonlijke registers zijn vergelijkbaar. 

Azure Container Registry is een beheerde Docker-service van Azure voor het hosten van persoonlijke installatiekopieën. De implementaties kunnen van elk type zijn, met inbegrip van [Docker Swarm](https://docs.docker.com/engine/swarm/), [Kubernetes](https://kubernetes.io/), en Web App for Containers. 

### <a name="create-an-azure-container-registry"></a>Een Azure Container Registry maken

Gebruik in de Cloud Shell de opdracht [`az acr create`](/cli/azure/acr?view=azure-cli-latest#az_acr_create) voor het maken van een Azure Container Registry. Geef de naam, resourcegroep, en `Basic` door voor de SKU. Beschikbare SKU's zijn `Classic`, `Basic`, `Standard`, en `Premium`.

```azurecli-interactive
az acr create --name <azure-container-registry-name> --resource-group myResourceGroup --sku Basic --admin-enabled true
```

Het maken van een container geeft de volgende uitvoer:

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

### <a name="log-in-to-azure-container-registry"></a>Inloggen in Azure Container Registry

Om een installatiekopie naar het register te pushen, moet u referenties opgeven zodat het register de push-bewerking accepteert. U kunt deze referenties ophalen met behulp van de opdracht [`az acr show`](/cli/azure/acr?view=azure-cli-latest#az_acr_show) in de Cloud Shell. 

```azurecli-interactive
az acr credential show --name <azure-container-registry-name>
```

De opdracht geeft twee wachtwoorden die kunnen worden gebruikt met de gebruikersnaam.

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

Meld u vanuit uw lokale terminalvenster aan bij Azure Container Registry met behulp van de opdracht `docker login`. De servernaam is vereist om verbinding te maken. Gebruik de indeling `{azure-container-registry-name>.azurecr.io`. Typ uw wachtwoord in de console in bij de opdrachtprompt.

```bash
docker login <azure-container-registry-name>.azurecr.io --username <registry-username>
```

Bevestig dat de aanmelding is geslaagd. 

### <a name="push-an-image-to-azure-container-registry"></a>Installatiekopie pushen naar Azure Container Registry

> [!NOTE]
> Als u uw eigen installatiekopie gebruikt, labelt u deze als volgt:
> ```bash
> docker tag <azure-container-registry-name>.azurecr.io/mydockerimage
> ```

Push de installatiekopie met behulp van de opdracht `docker push`. Tag de installatiekopie met de naam van het register, gevolgd door de naam van de installatiekopie en de tag.

```bash
docker push <azure-container-registry-name>.azurecr.io/mydockerimage:v1.0.0
```

Controleer of de push-bewerking geslaagd is in het toevoegen van een container in het register door een lijst van de ACR opslagplaatsen weer te geven. 

```azurecli-interactive
az acr repository list -n <azure-container-registry-name>
```

Een lijst van de installatiekopieën in het register bevestigt dat `mydockerimage` zich in het register bevindt.

```json
[
  "mydockerimage"
]
```

### <a name="configure-web-app-to-use-the-image-from-azure-container-registry-or-any-private-registry"></a>Web App configureren voor het gebruik van de installatiekopie uit Azure Container Registry (of een persoonlijk register)

U kunt Web App for Containers configureren zodat deze een container uitvoert die is opgeslagen in Azure Container Registry. Het gebruik van Azure Container Registry werkt net als bij alle persoonlijke registers. De stappen voor het voltooien van deze taak zijn dus vergelijkbaar als u uw eigen persoonlijke register wilt gebruiken.

Voer in de Cloud Shell [`az acr credential show`](/cli/azure/acr/credential?view=azure-cli-latest#az_acr_credential_show) uit om de gebruikersnaam en het wachtwoord voor Azure Container Registry weer te geven. Kopieer de gebruikersnaam en een van de wachtwoorden zodat u deze in de volgende stap kunt gebruiken voor het configureren van de web-app.

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

Voer in de Cloud Shell de opdracht [`az webapp config container set`](/cli/azure/webapp/config/container?view=azure-cli-latest#az_webapp_config_container_set) uit om de aangepaste Docker-installatiekopie toe te wijzen aan de web-app. Vervang *\<app_naam>*, *\<docker-register-server-url>*, _\<register-username>_ en _\<password>_. Voor Azure Container Registry is *\<docker-register-server-url>* in de indeling `https://<azure-container-registry-name>.azurecr.io`. Als u van een register naast Docker Hub gebruikmaakt, moet de installatiekopie met de volledig gekwalificeerde domeinnaam (FQDN) van uw register beginnen. Voor Azure Container Registry ziet dit er uit als `<azure-container-registry>.azurecr.io/mydockerimage`. 

```azurecli-interactive
az webapp config container set --name <app_name> --resource-group myResourceGroup --docker-custom-image-name <azure-container-registry-name>.azurecr.io/mydockerimage --docker-registry-server-url https://<azure-container-registry-name>.azurecr.io --docker-registry-server-user <registry-username> --docker-registry-server-password <password>
```

> [!NOTE]
> `https://`is vereist in *\<docker-registry-server-url>*.
>

De opdracht toont uitvoer die vergelijkbaar is met de volgende JSON-tekenreeks, die laat zien dat het wijzigen van de configuratie is voltooid:

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
> [Een Docker Python- en PostgreSQL-web-app maken in Azure](tutorial-docker-python-postgresql-app.md)
