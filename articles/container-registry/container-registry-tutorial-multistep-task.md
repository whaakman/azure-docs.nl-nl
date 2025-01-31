---
title: Zelfstudie - taken meerdere stappen container - Azure Container Registry taken
description: In deze zelfstudie leert u over het configureren van een Azure Container Registry taak automatisch een WebTest met meerdere stappen om werkstromen te activeren kunt maken, uitvoeren en pushen van containerinstallatiekopieën in de cloud wanneer u broncode aan een Git-opslagplaats.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: tutorial
ms.date: 05/09/2019
ms.author: danlep
ms.custom: seodec18, mvc
ms.openlocfilehash: 09b8e5d31bc6a4ec24633889920e2768bb7ce538
ms.sourcegitcommit: f6c85922b9e70bb83879e52c2aec6307c99a0cac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/11/2019
ms.locfileid: "65546571"
---
# <a name="tutorial-run-a-multi-step-container-workflow-in-the-cloud-when-you-commit-source-code"></a>Zelfstudie: De werkstroom van een container met meerdere stappen in de cloud worden uitgevoerd wanneer u broncode

Naast een [snelle taak](container-registry-tutorial-quick-task.md), ACR taken biedt ondersteuning voor meerdere stappen, meerdere-container op basis van werkstromen die automatisch activeren kunnen wanneer u de broncode aan een Git-opslagplaats. 

In deze zelfstudie leert u hoe u voorbeeld YAML-bestanden gebruiken voor het definiëren van taken meerdere stappen die bouwen, uitvoert, en een of meer containerinstallatiekopieën naar een register pushen wanneer u broncode. Zie voor het maken van een taak waarmee alleen een één installatiekopie build op doorvoercode [zelfstudie: Compileren van de container-installatiekopieën in de cloud automatiseren wanneer u broncode](container-registry-tutorial-build-task.md). Zie voor een overzicht van de ACR-taken, [besturingssysteem automatiseren en framework patchen met ACR taken](container-registry-tasks-overview.md),

In deze zelfstudie:

> [!div class="checklist"]
> * De taak van een WebTest met meerdere stappen met behulp van een YAML-bestand definiëren
> * Een taak maken
> * (Optioneel)-referenties toevoegen aan de taak voor het inschakelen van toegang tot een andere register
> * De taak testen
> * Taakstatus weergeven
> * De taak activeren met een code-doorvoer

In deze zelfstudie wordt ervan uitgegaan dat u de stappen in de [vorige zelfstudie](container-registry-tutorial-quick-task.md) hebt voltooid. Als u dit nog niet hebt gedaan, voert u de stappen in de sectie [Vereisten](container-registry-tutorial-quick-task.md#prerequisites) van de vorige zelfstudie uit voordat u doorgaat.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u de Azure CLI lokaal gebruikt wilt, moet u Azure CLI-versie hebben **2.0.62** of hoger geïnstalleerd en aangemeld [az login][az-login]. Voer `az --version` uit om de versie te bekijken. Als u de CLI wilt installeren of upgraden, raadpleegt u [Azure CLI installeren][azure-cli].

[!INCLUDE [container-registry-task-tutorial-prereq.md](../../includes/container-registry-task-tutorial-prereq.md)]

## <a name="create-a-multi-step-task"></a>Een taak meerdere stappen maken

Nu dat u de stappen die nodig zijn om in te schakelen ACR taken doorvoeren status lezen en webhooks maken in een opslagplaats hebt voltooid, maakt u een taak meerdere stappen waarmee het bouwen, het uitvoeren en het pushen een containerinstallatiekopie wordt geactiveerd.

### <a name="yaml-file"></a>YAML-bestand

U definieert de stappen voor een taak meerdere stappen in een [YAML-bestand](container-registry-tasks-reference-yaml.md). De eerste taak van de WebTest met meerdere stappen voorbeeld voor deze zelfstudie is gedefinieerd in het bestand `taskmulti.yaml`, deze bevindt zich in de hoofdmap van de GitHub-opslagplaats die u hebt gekloond:

```yml
version: v1.0.0
steps:
# Build target image
- build: -t {{.Run.Registry}}/hello-world:{{.Run.ID}} -f Dockerfile .
# Run image 
- cmd: -t {{.Run.Registry}}/hello-world:{{.Run.ID}}
  id: test
  detach: true
  ports: ["8080:80"]
- cmd: docker stop test
# Push image
- push:
  - {{.Run.Registry}}/hello-world:{{.Run.ID}}
```

Deze taak meerdere stappen doet het volgende:

1. Wordt uitgevoerd een `build` stap voor het bouwen van een installatiekopie van het Dockerfile in de werkmap. De doelen van de installatiekopie van het `Run.Registry`, het register waar de taak wordt uitgevoerd en is gemarkeerd met een unieke ACR-taken uitvoeren ID. 
1. Wordt uitgevoerd een `cmd` stap uit te voeren van de afbeelding in een tijdelijke container. In dit voorbeeld wordt een langlopende-container gestart op de achtergrond en retourneert de container-ID en vervolgens de container gestopt. In een Praktijkscenario, u advies inwinnen bij stappen voor het testen van de container die wordt uitgevoerd om te controleren of dat deze correct wordt uitgevoerd.
1. In een `push` stap, de installatiekopie die is gemaakt naar de run-register gepusht.

### <a name="task-command"></a>Taakopdracht

Vul eerst deze shell-omgevingsvariabelen met waarden die geschikt zijn voor uw omgeving. Hoewel deze stap strikt genomen niet vereist is, vereenvoudigt u hiermee de uitvoering van meerregelige Azure CLI-opdrachten in deze zelfstudie. Als u deze omgevingsvariabelen niet vullen, moet u handmatig elke waarde vervangen waar deze wordt weergegeven in de voorbeeldopdrachten.

```azurecli-interactive
ACR_NAME=<registry-name>        # The name of your Azure container registry
GIT_USER=<github-username>      # Your GitHub user account name
GIT_PAT=<personal-access-token> # The PAT you generated in the previous section
```

Maak nu de taak door het uitvoeren van de volgende [az acr-taak maken] [ az-acr-task-create] opdracht:

```azurecli-interactive
az acr task create \
    --registry $ACR_NAME \
    --name example1 \
    --context https://github.com/$GIT_USER/acr-build-helloworld-node.git \
    --branch master \
    --file taskmulti.yaml \
    --git-access-token $GIT_PAT
```

Deze taak geeft aan dat alle tijdcode doorgevoerd naar de *master* vertakking in de opslagplaats die is opgegeven door `--context`, ACR taken wordt de taak meerdere stappen uitgevoerd vanuit de code in dat filiaal. Het YAML-bestand dat is opgegeven door `--file` uit de opslagplaats hoofdmap definieert de stappen. 

De uitvoer van een geslaagde [az acr task create][az-acr-task-create]-opdracht is vergelijkbaar met het volgende:

```console
{
  "agentConfiguration": {
    "cpu": 2
  },
  "creationDate": "2019-05-03T03:14:31.763887+00:00",
  "credentials": null,
  "id": "/subscriptions/<Subscription ID>/resourceGroups/myregistry/providers/Microsoft.ContainerRegistry/registries/myregistry/tasks/taskmulti",
  "location": "westus",
  "name": "example1",
  "platform": {
    "architecture": "amd64",
    "os": "linux",
    "variant": null
  },
  "provisioningState": "Succeeded",
  "resourceGroup": "myresourcegroup",
  "status": "Enabled",
  "step": {
    "baseImageDependencies": null,
    "contextAccessToken": null,
    "contextPath": "https://github.com/gituser/acr-build-helloworld-node.git",
    "taskFilePath": "taskmulti.yaml",
    "type": "FileTask",
    "values": [],
    "valuesFilePath": null
  },
  "tags": null,
  "timeout": 3600,
  "trigger": {
    "baseImageTrigger": {
      "baseImageTriggerType": "Runtime",
      "name": "defaultBaseimageTriggerName",
      "status": "Enabled"
    },
    "sourceTriggers": [
      {
        "name": "defaultSourceTriggerName",
        "sourceRepository": {
          "branch": "master",
          "repositoryUrl": "https://github.com/gituser/acr-build-helloworld-node.git",
          "sourceControlAuthProperties": null,
          "sourceControlType": "Github"
        },
        "sourceTriggerEvents": [
          "commit"
        ],
        "status": "Enabled"
      }
    ]
  },
  "type": "Microsoft.ContainerRegistry/registries/tasks"
}
```

## <a name="test-the-multi-step-workflow"></a>Testen van de werkstroom met meerdere stappen

Als u wilt testen van de taak meerdere stappen, het handmatig activeren door het uitvoeren van de [az acr-taak uitvoeren] [ az-acr-task-run] opdracht:

```azurecli-interactive
az acr task run --registry $ACR_NAME --name example1
```

Met de opdracht `az acr task run` streamt u standaard de logboekuitvoer naar de console wanneer u de opdracht uitvoert. De uitvoer toont de voortgang van het uitvoeren van elk van de stappen. De onderstaande uitvoer is samengevoegd om belangrijke stappen weer te geven.

```console
Queued a run with ID: cf19
Waiting for an agent...
2019/05/03 03:03:31 Downloading source code...
2019/05/03 03:03:33 Finished downloading source code
2019/05/03 03:03:33 Using acb_vol_cfe6bd55-3076-4215-8091-6a81aec3d1b1 as the home volume
2019/05/03 03:03:33 Creating Docker network: acb_default_network, driver: 'bridge'
2019/05/03 03:03:34 Successfully set up Docker network: acb_default_network
2019/05/03 03:03:34 Setting up Docker configuration...
2019/05/03 03:03:34 Successfully set up Docker configuration
2019/05/03 03:03:34 Logging in to registry: myregistry.azurecr.io
2019/05/03 03:03:35 Successfully logged into myregistry.azurecr.io
2019/05/03 03:03:35 Executing step ID: acb_step_0. Working directory: '', Network: 'acb_default_network'
2019/05/03 03:03:35 Scanning for dependencies...
2019/05/03 03:03:36 Successfully scanned dependencies
2019/05/03 03:03:36 Launching container with name: acb_step_0
Sending build context to Docker daemon  24.06kB

[...]

Successfully built f669bfd170af
Successfully tagged myregistry.azurecr.io/hello-world:cf19
2019/05/03 03:03:43 Successfully executed container: acb_step_0
2019/05/03 03:03:43 Executing step ID: acb_step_1. Working directory: '', Network: 'acb_default_network'
2019/05/03 03:03:43 Launching container with name: acb_step_1
279b1cb6e092b64c8517c5506fcb45494cd5a0bd10a6beca3ba97f25c5d940cd
2019/05/03 03:03:44 Successfully executed container: acb_step_1
2019/05/03 03:03:44 Executing step ID: acb_step_2. Working directory: '', Network: 'acb_default_network'
2019/05/03 03:03:44 Pushing image: myregistry.azurecr.io/hello-world:cf19, attempt 1

[...]

2019/05/03 03:03:46 Successfully pushed image: myregistry.azurecr.io/hello-world:cf19
2019/05/03 03:03:46 Step ID: acb_step_0 marked as successful (elapsed time in seconds: 7.425169)
2019/05/03 03:03:46 Populating digests for step ID: acb_step_0...
2019/05/03 03:03:47 Successfully populated digests for step ID: acb_step_0
2019/05/03 03:03:47 Step ID: acb_step_1 marked as successful (elapsed time in seconds: 0.827129)
2019/05/03 03:03:47 Step ID: acb_step_2 marked as successful (elapsed time in seconds: 2.112113)
2019/05/03 03:03:47 The following dependencies were found:
2019/05/03 03:03:47
- image:
    registry: myregistry.azurecr.io
    repository: hello-world
    tag: cf19
    digest: sha256:6b981a8ca8596e840228c974c929db05c0727d8630465de536be74104693467a
  runtime-dependency:
    registry: registry.hub.docker.com
    repository: library/node
    tag: 9-alpine
    digest: sha256:8dafc0968fb4d62834d9b826d85a8feecc69bd72cd51723c62c7db67c6dec6fa
  git:
    git-head-revision: 1a3065388a0238e52865db1c8f3e97492a43444c

Run ID: cf19 was successful after 18s
```

## <a name="trigger-a-build-with-a-commit"></a>Een build activeren met een doorvoer

Nu u de taak hebt getest door deze handmatig uit te voeren, gaat u deze automatisch activeren met een wijziging in de broncode.

Controleer eerst of u zich bevindt in de map waarin de lokale kloon van de [opslagplaats][sample-repo] staat:

```azurecli-interactive
cd acr-build-helloworld-node
```

Voer vervolgens de volgende opdrachten uit om een nieuw bestand te maken, door te voeren en pushen naar uw fork van de opslagplaats op GitHub:

```azurecli-interactive
echo "Hello World!" > hello.txt
git add hello.txt
git commit -m "Testing ACR Tasks"
git push origin master
```

U wordt mogelijk gevraagd om uw GitHub-referenties op te geven wanneer u de opdracht `git push` uitvoert. Geef uw GitHub-gebruikersnaam op en voer het persoonlijke toegangstoken (PAT) in dat u eerder hebt gemaakt voor het wachtwoord.

```console
$ git push origin master
Username for 'https://github.com': <github-username>
Password for 'https://githubuser@github.com': <personal-access-token>
```

Wanneer u een wijziging naar de opslagplaats hebt gepusht, wordt de webhook is gemaakt door ACR taken wordt gestart en begint de taak in Azure Container Registry. Geef de logboeken weer voor de taak die momenteel wordt uitgevoerd om de voortgang van de build te verifiëren en controleren:

```azurecli-interactive
az acr task logs --registry $ACR_NAME
```

De uitvoer is vergelijkbaar met de volgende, waarin de op dit moment uitgevoerde (of de laatst uitgevoerde) taak wordt getoond:

```console
$ az acr task logs --registry $ACR_NAME
Showing logs of the last created run.
Run ID: cf1d

[...]

Run ID: cf1d was successful after 37s
```

## <a name="list-builds"></a>Builds weergeven

Voor een overzicht van de taken die ACR Tasks heeft voltooid voor het register, voert u de opdracht [az acr task-list-runs][az-acr-task-list-runs] uit:

```azurecli-interactive
az acr task list-runs --registry $ACR_NAME --output table
```

De uitvoer van de opdracht ziet er ongeveer als volgt uit. De runs die ACR Tasks heeft uitgevoerd, worden weergegeven. 'Git Commit' wordt weergegeven in de kolom TRIGGER voor de meest recente taak:

```console
$ az acr task list-runs --registry $ACR_NAME --output table

RUN ID    TASK       PLATFORM    STATUS     TRIGGER    STARTED               DURATION
--------  ---------  ----------  ---------  ---------  --------------------  ----------
cf1d      example1   linux       Succeeded  Commit     2019-05-03T04:16:44Z  00:00:37
cf1c      example1   linux       Succeeded  Commit     2019-05-03T04:16:44Z  00:00:39
cf1b      example1   linux       Succeeded  Manual     2019-05-03T03:10:30Z  00:00:31
cf1a      example1   linux       Succeeded  Commit     2019-05-03T03:09:32Z  00:00:31
cf19      example1   linux       Succeeded  Manual     2019-05-03T03:03:30Z  00:00:21
```

## <a name="create-a-multi-registry-multi-step-task"></a>Een WebTest met meerdere stappen meerdere register taak maken

ACR-taken standaard heeft machtigingen voor push of pull installatiekopieën vanuit het register waarop de taak wordt uitgevoerd. Het is raadzaam om uit te voeren van een taak meerdere stappen die gericht is op een of meer registers naast het register uitvoeren. Bijvoorbeeld, moet u mogelijk voor het bouwen van afbeeldingen in een register en installatiekopieën met verschillende codes opslaan in een tweede register die wordt gebruikt door een productiesysteem. In dit voorbeeld ziet u hoe u een dergelijke taak maken en geef referenties op voor een andere register.

Als u nog een tweede register hebt, maakt u een voor dit voorbeeld. Als u een register nodig hebt, raadpleegt u de [vorige zelfstudie](container-registry-tutorial-quick-task.md) of [Quickstart: een containerregister maken met de Azure-CLI](container-registry-get-started-azure-cli.md).

Voor het maken van de taak, moet u de naam van de aanmeldingsserver register, dit van het formulier is *mycontainerregistrydate.azurecr.io* (zonder hoofdletters). In dit voorbeeld gebruikt u het tweede register voor het opslaan van installatiekopieën die zijn gemarkeerd door build datum.

### <a name="yaml-file"></a>YAML-bestand

De tweede taak van de WebTest met meerdere stappen voorbeeld voor deze zelfstudie is gedefinieerd in het bestand `taskmulti-multiregistry.yaml`, deze bevindt zich in de hoofdmap van de GitHub-opslagplaats die u hebt gekloond:

```yml
version: v1.0.0
steps:
# Build target images
- build: -t {{.Run.Registry}}/hello-world:{{.Run.ID}} -f Dockerfile .
- build: -t {{.Values.regDate}}/hello-world:{{.Run.Date}} -f Dockerfile .
# Run image 
- cmd: -t {{.Run.Registry}}/hello-world:{{.Run.ID}}
  id: test
  detach: true
  ports: ["8080:80"]
- cmd: docker stop test
# Push images
- push:
  - {{.Run.Registry}}/hello-world:{{.Run.ID}}
  - {{.Values.regDate}}/hello-world:{{.Run.Date}}
```

Deze taak meerdere stappen doet het volgende:

1. Wordt uitgevoerd twee `build` stappen voor het bouwen van installatiekopieën van het Dockerfile in de werkmap:
    * De eerste doelen de `Run.Registry`, het register waar de taak wordt uitgevoerd en is voorzien van de ACR-taken uitvoeren ID. 
    * De tweede is gericht op het register dat wordt geïdentificeerd door de waarde van `regDate`, die u bij het maken van de taak ingesteld (of via een externe `values.yaml` bestand doorgegeven aan `az acr task create`). Deze installatiekopie is gemarkeerd met de datum uitvoeren.
1. Wordt uitgevoerd een `cmd` stap uit te voeren op een van de ingebouwde containers. In dit voorbeeld wordt een langlopende-container gestart op de achtergrond en retourneert de container-ID en vervolgens de container gestopt. In een Praktijkscenario test u mogelijk een container die wordt uitgevoerd om te controleren of dat deze correct wordt uitgevoerd.
1. In een `push` stap, de installatiekopieën die zijn gemaakt, de eerste in het register uitvoeren, de tweede in het register dat wordt geïdentificeerd door pushes `regDate`.

### <a name="task-command"></a>Taakopdracht

De taak met behulp van de eerder gedefinieerd variabelen voor de shell-omgeving maken door het uitvoeren van de volgende [az acr-taak maken] [ az-acr-task-create] opdracht. Vervang de naam van het register voor de *mycontainerregistrydate*.

```azurecli-interactive
az acr task create \
    --registry $ACR_NAME \
    --name example2 \
    --context https://github.com/$GIT_USER/acr-build-helloworld-node.git \
    --branch master \
    --file taskmulti-image.yaml \
    --git-access-token $GIT_PAT \
    --set regDate=mycontainerregistrydate.azurecr.io
```

### <a name="add-task-credential"></a>Taak referentie toevoegen

Push-installatiekopieën op het register dat wordt geïdentificeerd door de waarde van `regDate`, gebruikt u de [az acr taak referentie toevoegen] [ az-acr-task-credential-add] opdracht aanmeldingsreferenties voor die register toegevoegd aan de taak.

In dit voorbeeld wordt aangeraden dat u maakt een [service-principal](container-registry-auth-service-principal.md) met toegang tot het register binnen het bereik van de *AcrPush* rol. Voor het maken van de service-principal, ziet deze [Azure CLI-script](https://github.com/Azure-Samples/azure-cli-samples/blob/master/container-registry/service-principal-create/service-principal-create.sh).

Geef de service principal toepassings-ID en het wachtwoord in de volgende `az acr task credential add` opdracht:

```azurecli-interactive
az acr task credential add --name example2 \
    --registry $ACR_NAME \
    --login-server mycontainerregistrydate.azurecr.io \
    --username <service-principal-application-id> \
    --password <service-principal-password>
```

De CLI retourneert de naam van de aanmeldingsserver van het register die u hebt toegevoegd.

### <a name="test-the-multi-step-workflow"></a>Testen van de werkstroom met meerdere stappen

Zoals in het voorgaande voorbeeld, voor het testen van de taak meerdere stappen, het handmatig activeren door het uitvoeren van de [az acr-taak uitvoeren] [ az-acr-task-run] opdracht. Voor het activeren van de taak met een wijziging in de Git-opslagplaats, Zie de sectie [een build met een wijziging activeren](#trigger-a-build-with-a-commit).

```azurecli-interactive
az acr task run --registry $ACR_NAME --name example2
```

Met de opdracht `az acr task run` streamt u standaard de logboekuitvoer naar de console wanneer u de opdracht uitvoert. Als voorheen, de uitvoer toont de voortgang van het uitvoeren van elk van de stappen. De uitvoer is samengevoegd om weer te geven van de belangrijkste stappen

Uitvoer:

```console
Queued a run with ID: cf1g
Waiting for an agent...
2019/05/03 04:33:39 Downloading source code...
2019/05/03 04:33:41 Finished downloading source code
2019/05/03 04:33:42 Using acb_vol_4569b017-29fe-42bd-83b2-25c45a8ac807 as the home volume
2019/05/03 04:33:42 Creating Docker network: acb_default_network, driver: 'bridge'
2019/05/03 04:33:43 Successfully set up Docker network: acb_default_network
2019/05/03 04:33:43 Setting up Docker configuration...
2019/05/03 04:33:44 Successfully set up Docker configuration
2019/05/03 04:33:44 Logging in to registry: mycontainerregistry.azurecr.io
2019/05/03 04:33:45 Successfully logged into mycontainerregistry.azurecr.io
2019/05/03 04:33:45 Logging in to registry: mycontainerregistrydate.azurecr.io
2019/05/03 04:33:47 Successfully logged into mycontainerregistrydate.azurecr.io
2019/05/03 04:33:47 Executing step ID: acb_step_0. Working directory: '', Network: 'acb_default_network'
2019/05/03 04:33:47 Scanning for dependencies...
2019/05/03 04:33:47 Successfully scanned dependencies
2019/05/03 04:33:47 Launching container with name: acb_step_0
Sending build context to Docker daemon  25.09kB

[...]

Successfully tagged mycontainerregistry.azurecr.io/hello-world:cf1g
2019/05/03 04:33:55 Successfully executed container: acb_step_0
2019/05/03 04:33:55 Executing step ID: acb_step_1. Working directory: '', Network: 'acb_default_network'
2019/05/03 04:33:55 Scanning for dependencies...
2019/05/03 04:33:56 Successfully scanned dependencies
2019/05/03 04:33:56 Launching container with name: acb_step_1
Sending build context to Docker daemon  25.09kB

[...]

Successfully tagged mycontainerregistrydate.azurecr.io/hello-world:20190503-043342z
2019/05/03 04:33:57 Successfully executed container: acb_step_1
2019/05/03 04:33:57 Executing step ID: acb_step_2. Working directory: '', Network: 'acb_default_network'
2019/05/03 04:33:57 Launching container with name: acb_step_2
721437ff674051b6be63cbcd2fa8eb085eacbf38d7d632f1a079320133182101
2019/05/03 04:33:58 Successfully executed container: acb_step_2
2019/05/03 04:33:58 Executing step ID: acb_step_3. Working directory: '', Network: 'acb_default_network'
2019/05/03 04:33:58 Launching container with name: acb_step_3
test
2019/05/03 04:34:09 Successfully executed container: acb_step_3
2019/05/03 04:34:09 Executing step ID: acb_step_4. Working directory: '', Network: 'acb_default_network'
2019/05/03 04:34:09 Pushing image: mycontainerregistry.azurecr.io/hello-world:cf1g, attempt 1
The push refers to repository [mycontainerregistry.azurecr.io/hello-world]

[...]

2019/05/03 04:34:12 Successfully pushed image: mycontainerregistry.azurecr.io/hello-world:cf1g
2019/05/03 04:34:12 Pushing image: mycontainerregistrydate.azurecr.io/hello-world:20190503-043342z, attempt 1
The push refers to repository [mycontainerregistrydate.azurecr.io/hello-world]

[...]

2019/05/03 04:34:19 Successfully pushed image: mycontainerregistrydate.azurecr.io/hello-world:20190503-043342z
2019/05/03 04:34:19 Step ID: acb_step_0 marked as successful (elapsed time in seconds: 8.125744)
2019/05/03 04:34:19 Populating digests for step ID: acb_step_0...
2019/05/03 04:34:21 Successfully populated digests for step ID: acb_step_0
2019/05/03 04:34:21 Step ID: acb_step_1 marked as successful (elapsed time in seconds: 2.009281)
2019/05/03 04:34:21 Populating digests for step ID: acb_step_1...
2019/05/03 04:34:23 Successfully populated digests for step ID: acb_step_1
2019/05/03 04:34:23 Step ID: acb_step_2 marked as successful (elapsed time in seconds: 0.795440)
2019/05/03 04:34:23 Step ID: acb_step_3 marked as successful (elapsed time in seconds: 11.446775)
2019/05/03 04:34:23 Step ID: acb_step_4 marked as successful (elapsed time in seconds: 9.734973)
2019/05/03 04:34:23 The following dependencies were found:
2019/05/03 04:34:23
- image:
    registry: mycontainerregistry.azurecr.io
    repository: hello-world
    tag: cf1g
    digest: sha256:75354e9edb995e8661438bad9913deed87a185fddd0193811f916d684b71a5d2
  runtime-dependency:
    registry: registry.hub.docker.com
    repository: library/node
    tag: 9-alpine
    digest: sha256:8dafc0968fb4d62834d9b826d85a8feecc69bd72cd51723c62c7db67c6dec6fa
  git:
    git-head-revision: 9d9023473c46a5e2c315681b11eb4552ef0faccc
- image:
    registry: mycontainerregistrydate.azurecr.io
    repository: hello-world
    tag: 20190503-043342z
    digest: sha256:75354e9edb995e8661438bad9913deed87a185fddd0193811f916d684b71a5d2
  runtime-dependency:
    registry: registry.hub.docker.com
    repository: library/node
    tag: 9-alpine
    digest: sha256:8dafc0968fb4d62834d9b826d85a8feecc69bd72cd51723c62c7db67c6dec6fa
  git:
    git-head-revision: 9d9023473c46a5e2c315681b11eb4552ef0faccc

Run ID: cf1g was successful after 46s
```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd over het maken van meerdere stappen, meerdere-container op basis van taken die automatisch activeren wanneer u source code in een Git-opslagplaats doorvoeren. Voor geavanceerde functies van meerdere stappen taken, zoals het uitvoeren van parallelle en afhankelijke stap, Zie de [ACR taken YAML-verwijzing](container-registry-tasks-reference-yaml.md). Ga verder naar de volgende zelfstudie voor informatie over het maken van taken die builds activeren wanneer de basisinstallatiekopie van de containerinstallatiekopie wordt bijgewerkt.

> [!div class="nextstepaction"]
> [Builds automatiseren op basis van installatiekopie-update](container-registry-tutorial-base-image-update.md)

<!-- LINKS - External -->
[sample-repo]: https://github.com/Azure-Samples/acr-build-helloworld-node

<!-- LINKS - Internal -->
[azure-cli]: /cli/azure/install-azure-cli
[az-acr-task]: /cli/azure/acr/task
[az-acr-task-create]: /cli/azure/acr/task#az-acr-task-create
[az-acr-task-run]: /cli/azure/acr/task#az-acr-task-run
[az-acr-task-list-runs]: /cli/azure/acr/task#az-acr-task-list-runs
[az-acr-task-credential-add]: /cli/azure/acr/task/credential#az-acr-task-credential-add    
[az-login]: /cli/azure/reference-index#az-login

<!-- IMAGES -->
[build-task-01-new-token]: ./media/container-registry-tutorial-build-tasks/build-task-01-new-token.png
[build-task-02-generated-token]: ./media/container-registry-tutorial-build-tasks/build-task-02-generated-token.png
