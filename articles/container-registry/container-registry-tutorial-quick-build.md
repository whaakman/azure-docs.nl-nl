---
title: 'Zelfstudie: Containerinstallatiekopieën bouwen in de cloud met Azure Container Registry Build'
description: In deze zelfstudie leert u hoe een Docker-containerinstallatiekopie in Azure bouwt met Azure Container Registry Build (ACR Build) en deze vervolgens implementeert op Azure Container Instances.
services: container-registry
author: mmacy
manager: jeconnoc
ms.service: container-registry
ms.topic: tutorial
ms.date: 05/11/2018
ms.author: marsma
ms.custom: mvc
ms.openlocfilehash: d86c47c890fd15515c590e06b395ca82f9747ffe
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38461466"
---
# <a name="tutorial-build-container-images-in-the-cloud-with-azure-container-registry-build"></a>Zelfstudie: Containerinstallatiekopieën bouwen in de cloud met Azure Container Registry Build

**ACR Build** is een suite met functies in Azure Container Registry die gestroomlijnde en efficiënte builds van Docker-containerinstallatiekopieën maakt in Azure. In dit artikel leert u hoe u de functie *Quick Build* van ACR Build gebruikt. Quick Build breidt de 'interne loop' van uw ontwikkeling uit naar de cloud, zodat u beschikt over succesvalidatie van builds en het automatisch pushen van met succes opgebouwde installatiekopieën naar uw containerregister. Uw installatiekopieën worden systeemeigen gebouwd in de cloud, dicht bij het register, waardoor snellere implementatie mogelijk is.

Al uw Dockerfile-expertise is direct overdraagbaar naar ACR Build. U hoeft uw Dockerfiles niet te wijzigen om in de cloud te bouwen met ACR Build, alleen de opdracht die u uitvoert.

In deze zelfstudie, deel één van een reeks:

> [!div class="checklist"]
> * De broncode van een voorbeeldtoepassing ophalen
> * Een containerinstallatiekopie maken in Azure
> * Een container implementeren in Azure Container Instances

In volgende zelfstudies leert u hoe u de opbouwtaken van ACR Build gebruikt voor geautomatiseerde builds van containerinstallatiekopieën bij codedoorvoer en updates van basisinstallatiekopieën.

[!INCLUDE [container-registry-build-preview-note](../../includes/container-registry-build-preview-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u de Azure CLI lokaal wilt gebruiken, moet u Azure CLI versie **2.0.32** of hoger hebben geïnstalleerd. Voer `az --version` uit om de versie te bekijken. Als u de CLI wilt installeren of upgraden, raadpleegt u [Azure CLI installeren][azure-cli].

## <a name="prerequisites"></a>Vereisten

### <a name="github-account"></a>GitHub-account

Maak een account op https://github.com als u er nog geen hebt. In deze zelfstudiereeks wordt een GitHub-opslagplaats gebruikt om geautomatiseerde builds van installatiekopieën in ACR Build te demonstreren.

### <a name="fork-sample-repository"></a>Voorbeeldopslagplaats fork

Gebruik vervolgens de GitHub UI om de voorbeeldopslagplaats te splitsen naar uw GitHub-account. In deze zelfstudie maakt u een containerinstallatiekopie van de bron in de opslagplaats. In de volgende zelfstudie pusht u een doorvoer naar uw fork van de opslagplaats om een geautomatiseerde build te starten.

Deze opslagplaats splitsen: https://github.com/Azure-Samples/acr-build-helloworld-node

![Schermafbeelding van de knop Fork (gemarkeerd) in GitHub][quick-build-01-fork]

### <a name="clone-your-fork"></a>Uw fork klonen

Wanneer u de opslagplaats hebt gesplitst, kloont u de fork en voert u de map in die de lokale kloon bevat.

Kloon de opslagplaats met `git`, vervang **\<your-github-username\>** door uw GitHub-gebruikersnaam:

```azurecli-interactive
git clone https://github.com/<your-github-username>/acr-build-helloworld-node
```

Geef de map op waarin de broncode zich bevindt:

```azurecli-interactive
cd acr-build-helloworld-node
```

### <a name="bash-shell"></a>Bash-shell

De opdrachten in deze zelfstudiereeks zijn geformatteerd voor de Bash-shell. Als u liever PowerShell, de opdrachtprompt of een andere shell wilt gebruiken, moet u wellicht de regelvoortzetting en indeling van de omgevingsvariabele overeenkomstig aanpassen.

## <a name="build-in-azure-with-acr-build"></a>In Azure bouwen met ACR Build

Nu u de broncode hebt opgehaald naar uw computer, volgt u deze stappen om een containerregister te maken en de containerinstallatiekopie te bouwen met ACR Build.

Om het uitvoeren van de voorbeeldopdrachten eenvoudiger te maken, worden in de zelfstudies in deze serie shell-omgevingsvariabelen gebruikt. Voer de volgende opdracht uit om de variabele `ACR_NAME` in te stellen. Vervang **\<registry-name\>** door een unieke naam voor uw nieuwe containerregister. De registernaam moet uniek zijn binnen Azure en mag 5 tot 50 alfanumerieke tekens bevatten. De andere resources die u in de zelfstudie maakt, zijn gebaseerd op deze naam, dus u hoeft alleen deze eerste variabele te wijzigen.

```azurecli-interactive
ACR_NAME=<registry-name>
```

Nu de omgevingsvariabele van de containerregister is ingevuld, kunt u de rest van de opdrachten in de zelfstudie kopiëren en plakken zonder waarden te bewerken. Voer de volgende opdrachten uit om een resourcegroep en containerregister te maken:

```azurecli-interactive
RES_GROUP=$ACR_NAME # Resource Group name

az group create --resource-group $RES_GROUP --location eastus
az acr create --resource-group $RES_GROUP --name $ACR_NAME --sku Standard --location eastus
```

Nu u een register hebt, gebruikt u ACR Build om een containerinstallatiekopie te bouwen vanaf de voorbeeldcode. Voer de opdracht [az acr build][az-acr-build] uit om een *Quick Build* uit te voeren:

```azurecli-interactive
az acr build --registry $ACR_NAME --image helloacrbuild:v1 .
```

De uitvoer van de opdracht [az acr build][az-acr-build] is vergelijkbaar met de volgende. U ziet het uploaden van de broncode (de 'context') naar Azure en de details van de `docker build`-bewerking die ACR Build uitvoert in de cloud. Omdat ACR Build `docker build` gebruikt om uw afbeeldingen op te bouwen, hoeven er geen wijzigingen te worden aangebracht in uw Dockerfiles om ACR Build direct te gaan gebruiken.

```console
$ az acr build --registry $ACR_NAME --image helloacrbuild:v1 .
Sending build context (4.909 KiB) to ACR.
Queued a build with build ID: aa1
Waiting for a build agent...
Sending build context to Docker daemon  22.53kB
Step 1/5 : FROM node:9-alpine
9-alpine: Pulling from library/node
Digest: sha256:5149aec8f508d48998e6230cdc8e6832cba192088b442c8ef7e23df3c6892cd3
Status: Image is up to date for node:9-alpine
 ---> 7af437a39ec2
Step 2/5 : COPY . /src
 ---> 0c4814714938
Step 3/5 : RUN cd /src && npm install
 ---> Running in 4f77ce7b330d
npm notice created a lockfile as package-lock.json. You should commit this file.
npm WARN helloworld@1.0.0 No repository field.

up to date in 0.096s
Removing intermediate container 4f77ce7b330d
 ---> e0eb24339e07
Step 4/5 : EXPOSE 80
 ---> Running in 872bd29edbc7
Removing intermediate container 872bd29edbc7
 ---> 22ba8d8ffb4e
Step 5/5 : CMD ["node", "/src/server.js"]
 ---> Running in 1a40c05c4122
Removing intermediate container 1a40c05c4122
 ---> 0a9a4b74fb53
Successfully built 0a9a4b74fb53
Successfully tagged mycontainerregistry.azurecr.io/helloacrbuild:v1
time="2018-05-10T19:10:20Z" level=info msg="Running command docker push mycontainerregistry.azurecr.io/helloacrbuild:v1"
The push refers to repository [mycontainerregistry.azurecr.io/helloacrbuild]
d2b301f7ef94: Preparing
d0e0f2bb8747: Preparing
26b0c207c4a9: Preparing
917e7cdebc8b: Preparing
9dfa40a0da3b: Preparing
26b0c207c4a9: Pushed
d2b301f7ef94: Pushed
d0e0f2bb8747: Pushed
9dfa40a0da3b: Pushed
917e7cdebc8b: Pushed
v1: digest: sha256:78d7980b4c80a078192bd4749c27eeae56421079606ed7b7d8ae84dbb04193fd size: 1366
time="2018-05-10T19:11:07Z" level=info msg="Running command docker inspect --format \"{{json .RepoDigests}}\" mycontainerregistry.azurecr.io/helloacrbuild:v1"
"["mycontainerregistry.azurecr.io/helloacrbuild@sha256:78d7980b4c80a078192bd4749c27eeae56421079606ed7b7d8ae84dbb04193fd"]"
time="2018-05-10T19:11:07Z" level=info msg="Running command docker inspect --format \"{{json .RepoDigests}}\" node:9-alpine"
"["node@sha256:5149aec8f508d48998e6230cdc8e6832cba192088b442c8ef7e23df3c6892cd3"]"
ACR Builder discovered the following dependencies:
- image:
    registry: mycontainerregistry.azurecr.io
    repository: helloacrbuild
    tag: v1
    digest: sha256:78d7980b4c80a078192bd4749c27eeae56421079606ed7b7d8ae84dbb04193fd
  runtime-dependency:
    registry: registry.hub.docker.com
    repository: library/node
    tag: 9-alpine
    digest: sha256:5149aec8f508d48998e6230cdc8e6832cba192088b442c8ef7e23df3c6892cd3

Build complete
Build ID: aa1 was successful after 52.522222729s
```

Aan het einde van de uitvoer geeft ACR Build de afhankelijkheden weer die zijn gevonden voor uw installatiekopie. Zo kan ACR Build builds van installatiekopieën op updates van basisinstallatiekopieën automatiseren, bijvoorbeeld wanneer een basisinstallatiekopie wordt bijgewerkt met het besturingssysteem of met frameworkpatches. Later in deze zelfstudiereeks krijgt u meer informatie over de ondersteuning van ACR Build voor updates van basisinstallatiekopieën.

## <a name="deploy-to-azure-container-instances"></a>Implementeren naar Azure Container Instances

ACR Build pusht met succes opgebouwde installatiekopieën standaard naar het register, zodat u ze onmiddellijk vanuit het register kunt implementeren.

In deze sectie maakt u een Azure Key Vault en service-principal. Vervolgens implementeert u de container naar Azure Container Instances (ACI) met de referenties van de service-principal.

### <a name="configure-registry-authentication"></a>Registerverificatie configureren

Alle productiescenario's moeten [service-principals][service-principal-auth] gebruiken voor toegang tot een Azure-containerregister. Met service-principals kunt u voor op rollen gebaseerde toegangsbeheer bieden voor uw containerinstallatiekopieën. U kunt bijvoorbeeld een service-principal configureren met alleen pull-toegang tot een register.

#### <a name="create-key-vault"></a>Sleutelkluis maken

Als u nog geen kluis hebt in [Azure Key Vault](/azure/key-vault/), kunt u met de volgende opdrachten één maken met de Azure CLI.

```azurecli-interactive
AKV_NAME=$ACR_NAME-vault

az keyvault create --resource-group $RES_GROUP --name $AKV_NAME
```

#### <a name="create-service-principal-and-store-credentials"></a>Service-principal maken en referenties opslaan

U moet nu een service-principal maken en de referenties ervan opslaan in uw sleutelkluis.

Gebruik de opdracht [az ad sp create-for-rbac][az-ad-sp-create-for-rbac] om de service-principal te maken en [az keyvault secret set][az-keyvault-secret-set] om het **wachtwoord** van de service-principal in de kluis op te slaan:

```azurecli-interactive
# Create service principal, store its password in AKV (the registry *password*)
az keyvault secret set \
  --vault-name $AKV_NAME \
  --name $ACR_NAME-pull-pwd \
  --value $(az ad sp create-for-rbac \
                --name $ACR_NAME-pull \
                --scopes $(az acr show --name $ACR_NAME --query id --output tsv) \
                --role reader \
                --query password \
                --output tsv)
```

Het argument `--role` in de voorgaande opdracht configureert de service-principal met de rol *lezer*, die deze alleen-push-toegang tot het register verleent. Als u zowel push-als pull-toegang wilt verlenen, wijzigt u het argument `--role` in *Inzender*.

Vervolgens slaat u de *appId* van de service-principal op in de kluis. Deze ID is de **gebruikersnaam** die u voor verificatie doorgeeft aan Azure Container Registry:

```azurecli-interactive
# Store service principal ID in AKV (the registry *username*)
az keyvault secret set \
    --vault-name $AKV_NAME \
    --name $ACR_NAME-pull-usr \
    --value $(az ad sp show --id http://$ACR_NAME-pull --query appId --output tsv)
```

U hebt een Azure Key Vault gemaakt en er twee geheimen in opgeslagen:

* `$ACR_NAME-pull-usr`: de service principal-ID, voor gebruik als de **gebruikersnaam** van het containerregister.
* `$ACR_NAME-pull-pwd`: het service principal-wachtwoord, voor gebruik als het **wachtwoord** van het containerregister.

U kunt nu op naam naar deze geheime gegevens verwijzen wanneer u of uw toepassingen en services installatiekopieën uit het register halen.

### <a name="deploy-container-with-azure-cli"></a>Container implementeren met Azure CLI

Nu de referenties voor de service-principal zijn opgeslagen als Azure Key Vault-geheimen, kunnen uw toepassingen en services deze gebruiken voor toegang tot uw persoonlijke register.

Voer de volgende [az container create][az-container-create]-opdracht in om een containerinstantie te implementeren. De opdracht maakt gebruik van de referenties van de service-principal die zijn opgeslagen in Azure Key Vault om uw containerregister te verifiëren.

```azurecli-interactive
az container create \
    --resource-group $RES_GROUP \
    --name acr-build \
    --image $ACR_NAME.azurecr.io/helloacrbuild:v1 \
    --registry-login-server $ACR_NAME.azurecr.io \
    --registry-username $(az keyvault secret show --vault-name $AKV_NAME --name $ACR_NAME-pull-usr --query value -o tsv) \
    --registry-password $(az keyvault secret show --vault-name $AKV_NAME --name $ACR_NAME-pull-pwd --query value -o tsv) \
    --dns-name-label acr-build-$ACR_NAME \
    --query "{FQDN:ipAddress.fqdn}" \
    --output table
```

De waarde `--dns-name-label` moet uniek zijn binnen Azure, dus de voorgaande opdracht voegt de naam van uw containerregister toe aan het label van de DNS-naam van de container. De uitvoer van de opdracht geeft bijvoorbeeld de Fully Qualified Domain Name (FQDN) van de container weer:

```console
$ az container create \
>     --resource-group $RES_GROUP \
>     --name acr-build \
>     --image $ACR_NAME.azurecr.io/helloacrbuild:v1 \
>     --registry-login-server $ACR_NAME.azurecr.io \
>     --registry-username $(az keyvault secret show --vault-name $AKV_NAME --name $ACR_NAME-pull-usr --query value -o tsv) \
>     --registry-password $(az keyvault secret show --vault-name $AKV_NAME --name $ACR_NAME-pull-pwd --query value -o tsv) \
>     --dns-name-label acr-build-$ACR_NAME \
>     --query "{FQDN:ipAddress.fqdn}" \
>     --output table
FQDN
-------------------------------------------
acr-build-1175.eastus.azurecontainer.io
```

Let op de FQDN van de container; u gaat deze gebruiken in de volgende sectie.

### <a name="verify-deployment"></a>Implementatie verifiëren

Als u de opstartprocedure van de container wilt bekijken, gebruikt u de opdracht [az container attach][az-container-attach]:

```azurecli-interactive
az container attach --resource-group $RES_GROUP --name acr-build
```

De `az container attach`-uitvoer van de container geeft eerst de status van de container weer wanneer deze de installatiekopie ophaalt en start, en bindt vervolgens de STDOUT en STDERR van de lokale console aan die van de container.

```console
$ az container attach --resource-group $RES_GROUP --name acr-build
Container 'acr-build' is in state 'Waiting'...
Container 'acr-build' is in state 'Running'...
(count: 1) (last timestamp: 2018-04-03 19:45:37+00:00) pulling image "mycontainerregistry.azurecr.io/helloacrbuild:v1"
(count: 1) (last timestamp: 2018-04-03 19:45:44+00:00) Successfully pulled image "mycontainerregistry.azurecr.io/helloacrbuild:v1"
(count: 1) (last timestamp: 2018-04-03 19:45:44+00:00) Created container with id 094ab4da40138b36ca15fc2ad5cac351c358a7540a32e22b52f78e96a4cb3413
(count: 1) (last timestamp: 2018-04-03 19:45:44+00:00) Started container with id 094ab4da40138b36ca15fc2ad5cac351c358a7540a32e22b52f78e96a4cb3413

Start streaming logs:
Server running at http://localhost:80
```

Wanneer `Server running at http://localhost:80` wordt weergegeven, gaat u naar de FQDN van de container in de browser om de actieve toepassing weer te geven:

![Schermafbeelding van de voorbeeldtoepassing die wordt weergegeven in de browser][quick-build-02-browser]

Als u de console wilt loskoppelen van de container, klikt u op `Control+C`.

## <a name="clean-up-resources"></a>Resources opschonen

Stop de containerinstantie met de opdracht [az container delete][az-container-delete]:

```azurecli-interactive
az container delete --resource-group $RES_GROUP --name acr-build
```

Als u *alle* resources wilt verwijderen die u in deze zelfstudie hebt gemaakt, met inbegrip van het containerregister, de sleutelkluis en de service-principal, geeft u de volgende opdrachten. Deze resources worden echter gebruikt in de [volgende zelfstudie](container-registry-tutorial-build-task.md) in de reeks. U kunt ze dus bewaren als u direct verdergaat met de volgende zelfstudie.

```azurecli-interactive
az group delete --resource-group $RES_GROUP
az ad sp delete --id http://$ACR_NAME-pull
```

## <a name="next-steps"></a>Volgende stappen

Nu u de binnenste lus hebt getest met een snelle build, configureert u een **build-taak** om containerinstallatiekopieën te activeren wanneer u broncode doorvoert naar een Git-opslagplaats:

> [!div class="nextstepaction"]
> [Automatische builds activeren met build-taken](container-registry-tutorial-build-task.md)

<!-- LINKS - External -->
[sample-archive]: https://github.com/Azure-Samples/acr-build-helloworld-node/archive/master.zip

<!-- LINKS - Internal -->
[azure-cli]: /cli/azure/install-azure-cli
[az-acr-build]: /cli/azure/acr#az-acr-build
[az-ad-sp-create-for-rbac]: /cli/azure/ad/sp#az-ad-sp-create-for-rbac
[az-container-attach]: /cli/azure/container#az-container-attach
[az-container-create]: /cli/azure/container#az-container-create
[az-container-delete]: /cli/azure/container#az-container-delete
[az-keyvault-create]: /cli/azure/keyvault/secret#az-keyvault-create
[az-keyvault-secret-set]: /cli/azure/keyvault/secret#az-keyvault-secret-set
[service-principal-auth]: container-registry-auth-service-principal.md

<!-- IMAGES -->
[quick-build-01-fork]: ./media/container-registry-tutorial-quick-build/quick-build-01-fork.png
[quick-build-02-browser]: ./media/container-registry-tutorial-quick-build/quick-build-02-browser.png
