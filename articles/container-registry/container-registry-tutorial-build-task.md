---
title: 'Zelfstudie: Het bouwen van containerinstallatiekopieën automatiseren met Azure Container Registry Build'
description: In deze zelfstudie leert u hoe u een build-taak configureert om builds van containerinstallatiekopieën automatisch te activeren in de cloud wanneer u broncode naar een Git-opslagplaats doorvoert.
services: container-registry
author: mmacy
manager: jeconnoc
ms.service: container-registry
ms.topic: tutorial
ms.date: 05/07/2018
ms.author: marsma
ms.custom: mvc
ms.openlocfilehash: fba499441d092f4dce09d13d607dfc5de65d98b2
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="tutorial-automate-container-image-builds-with-azure-container-registry-build"></a>Zelfstudie: Het bouwen van containerinstallatiekopieën automatiseren met Azure Container Registry Build

Naast [Quick Build](container-registry-tutorial-quick-build.md) biedt ACR Build ondersteuning voor geautomatiseerde builds van Docker-containerinstallatiekopieën met de *build-taak*. In deze zelfstudie leert u hoe u de Azure CLI gebruikt om een build-taak te maken die builds van installatiekopieën automatisch activeert in de cloud wanneer u broncode naar een Git-opslagplaats doorvoert.

In deze zelfstudie, het tweede deel in de serie, leert u het volgende:

> [!div class="checklist"]
> * Een build-taak maken
> * De build-taak testen
> * Status van de build weergeven
> * De build-taak activeren met een code-doorvoer

In deze zelfstudie wordt ervan uitgegaan dat u de stappen in de [vorige zelfstudie](container-registry-tutorial-quick-build.md) hebt voltooid. Als u dit nog niet hebt gedaan, voert u de stappen in de sectie [Vereisten](container-registry-tutorial-quick-build.md#prerequisites) van de vorige zelfstudie uit voordat u doorgaat.

> [!IMPORTANT]
> ACR Build is momenteel in preview en wordt alleen ondersteund door Azure containerregisters in de regio’s **VS-Oost** (oostvs) en **West-Europa** (westeuropa). Previews worden voor u beschikbaar gesteld op voorwaarde dat u akkoord gaat met de [aanvullende gebruiksvoorwaarden][terms-of-use]. Sommige aspecten van deze functionaliteit kunnen wijzigen voordat deze functionaliteit algemeen beschikbaar wordt.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u de Azure CLI lokaal wilt gebruiken, moet u Azure CLI versie **2.0.32** of hoger hebben geïnstalleerd. Voer `az --version` uit om de versie te bekijken. Als u de CLI wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren][azure-cli].

## <a name="prerequisites"></a>Vereisten

### <a name="get-sample-code"></a>Voorbeeldcode ophalen

In deze zelfstudie wordt ervan uitgegaan dat u de stappen in de [vorige zelfstudie](container-registry-tutorial-quick-build.md) hebt voltooid en dat u de voorbeeldopslagplaats hebt gesplitst en gekloond. Als u dit nog niet hebt gedaan, voert u de stappen in de sectie [Vereisten](container-registry-tutorial-quick-build.md#prerequisites) van de vorige zelfstudie uit voordat u doorgaat.

### <a name="container-registry"></a>Containerregister

U moet een Azure-containerregister in uw Azure-abonnement hebben om deze zelfstudie te voltooien. Als u een register nodig hebt, zie dan de [vorige zelfstudie](container-registry-tutorial-quick-build.md) of [Snelstartgids: Een containerregister maken met de Azure CLI](container-registry-get-started-azure-cli.md).

## <a name="build-task"></a>Build-taak

Een build-taak definieert de eigenschappen van een geautomatiseerde build, met inbegrip van de locatie van de broncode van de containerinstallatiekopie en de gebeurtenis die de build activeert. Als een gebeurtenis die is gedefinieerd in de build-taak optreedt, zoals het doorvoeren van gegevens naar een Git-opslagplaats, initieert ACR Build een build van een containerinstallatiekopie in de cloud. Het stuurt dan vervolgens standaard een met succes gebouwde installatiekopie naar het Azure-containerregister dat is opgegeven in de taak.

ACR Build ondersteunt momenteel de volgende triggers voor build-taken:

* Doorvoeren naar een Git-opslagplaats
* Basisinstallatiekopieën bijwerken

## <a name="create-a-build-task"></a>Een build-taak maken

In deze sectie maakt u eerst een persoonlijk toegangstoken (PAT) van een GitHub voor gebruik met ACR Build. Vervolgens maakt u een build-taak die een build activeert wanneer code wordt doorgevoerd naar uw fork van de opslagplaats.

### <a name="create-a-github-personal-access-token"></a>Een persoonlijk toegangstoken van GitHub maken

Om een build op een doorvoer naar een Git-opslagplaats te activeren, heeft ACR Build een persoonlijk toegangstoken (PAT) nodig voor toegang tot de opslagplaats. Volg deze stappen om een PAT te genereren in GitHub:

1. Navigeer naar de pagina PAT maken op GitHub op https://github.com/settings/tokens/new
1. Geef een korte **beschrijving** voor het token op, bijvoorbeeld 'Demo ACR build-taak'
1. Onder **opslagplaats** schakelt u **opslagplaats:status** en **public_repo** in

   ![Schermafbeelding van de pagina Persoonlijk toegangstoken genereren in GitHub][build-task-01-new-token]

1. Selecteer de knop **Token genereren** (u wordt mogelijk gevraagd om uw wachtwoord te bevestigen)
1. Kopieer en bewaar het gegenereerde token in een **beveiligde locatie** (u gebruikt dit token bij het definiëren van een build-taak in de volgende sectie)

   ![Schermafbeelding van het gegeneerde persoonlijke toegangstoken in GitHub][build-task-02-generated-token]

### <a name="create-the-build-task"></a>De build-taak maken

Nu u de stappen hebt voltooid die nodig zijn om ACR Build in te schakelen om toewijzingsstatus te lezen en webhooks te maken in een opslagplaats, kunt u een build-taak maken die een build van een containerinstallatiekopie activeert op doorvoeracties naar de opslagplaats.

Vul eerst deze shell-omgevingsvariabelen met waarden die geschikt zijn voor uw omgeving. Dit is niet strikt vereist, maar vereenvoudigt de uitvoer van meerregelige Azure CLI-opdrachten in deze zelfstudie. Als u deze niet invult, moet u elke waarde handmatig vervangen waar ze worden weergegeven in de voorbeeldopdrachten.

```azurecli-interactive
ACR_NAME=mycontainerregistry # The name of your Azure container registry
GIT_USER=gituser             # Your GitHub user account name
GIT_PAT=personalaccesstoken  # The PAT you generated in the previous section
```

Maak nu de build-taak door de volgende [az acr build-task create][az-acr-build-task-create]-opdracht uit te voeren:

```azurecli-interactive
az acr build-task create \
    --registry $ACR_NAME \
    --name buildhelloworld \
    --image helloworld:{{.Build.ID}} \
    --context https://github.com/$GIT_USER/acr-build-helloworld-node \
    --branch master \
    --git-access-token $GIT_PAT
```

Deze build-taak geeft aan dat elke keer dat code wordt doorgevoerd naar de *master*-vertakking in de opslagplaats zoals is opgegeven door `--context`, ACR Build dan de containerinstallatiekopie van de code in die vertakking bouwt. Het argument `--image` geeft een parameterwaarde van `{{.Build.ID}}` aan voor het versiegedeelte van de tag van de installatiekopie, zodat de gebouwde installatiekopie correleert met een specifieke build en uniek is gelabeld.

De uitvoer van een geslaagde [az acr build-task create][az-acr-build-task-create]-opdracht is vergelijkbaar met het volgende:

```console
$ az acr build-task create \
>     --registry $ACR_NAME \
>     --name buildhelloworld \
>     --image helloworld:{{.Build.ID}} \
>     --context https://github.com/$GIT_USER/acr-build-helloworld-node \
>     --branch master \
>     --git-access-token $GIT_PAT
{
  "additionalProperties": {},
  "alias": "buildhelloworld",
  "creationDate": "2018-04-18T23:14:45.905395+00:00",
  "id": "/subscriptions/<subscriptionID>/resourceGroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/mycontainerregistry/buildTasks/buildhelloworld",
  "location": "eastus",
  "name": "buildhelloworld",
  "platform": {
    "additionalProperties": {},
    "cpu": 1,
    "osType": "Linux"
  },
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "sourceRepository": {
    "additionalProperties": {},
    "isCommitTriggerEnabled": true,
    "repositoryUrl": "https://github.com/gituser/acr-build-helloworld-node",
    "sourceControlAuthProperties": null,
    "sourceControlType": "Github"
  },
  "status": "enabled",
  "tags": null,
  "timeout": null,
  "type": "Microsoft.ContainerRegistry/registries/buildTasks"
}

```

## <a name="test-the-build-task"></a>De build-taak testen

U hebt nu een build-taak die uw build definieert. Als u de build-definitie wilt testen, activeert u een build handmatig door de opdracht [az acr build-task run][az-acr-build-task-run] uit te voeren:

```azurecli-interactive
az acr build-task run --registry $ACR_NAME --name buildhelloworld
```

Met de opdracht `az acr build-task run` streamt u standaard de logboekuitvoer naar de console wanneer u de opdracht uitvoert. Hier ziet u dat de uitvoer aangeeft dat build **oostvs2** in de wachtrij is geplaatst en is gebouwd.

```console
$ az acr build-task run --registry mycontainerregistry --name buildhelloworld
Queued a build with build-id: eastus2.
Starting to stream the logs...
Cloning into '/root/acr-builder/src'...
time="2018-04-19T00:06:20Z" level=info msg="Running command git checkout master"
Already on 'master'
Your branch is up to date with 'origin/master'.
ffef1347389a008c9a8bfdf8c6a0ed78b0479894
time="2018-04-19T00:06:20Z" level=info msg="Running command git rev-parse --verify HEAD"
time="2018-04-19T00:06:20Z" level=info msg="Running command docker build --pull -f Dockerfile -t mycontainerregistry.azurecr.io/helloworld:eastus2 ."
Sending build context to Docker daemon  182.8kB
Step 1/5 : FROM node:9-alpine
9: Pulling from library/node
Digest: sha256:bd7b9aaf77ab2ce1e83e7e79fc0969229214f9126ced222c64eab49dc0bdae90
Status: Image is up to date for node:9-alpine
 ---> aa3e171e4e95
Step 2/5 : COPY . /src
 ---> e1c04dc2993b

[...]

6e5e20cbf4a7: Layer already exists
b69680cb4898: Pushed
b54af9b858b7: Pushed
eastus2: digest: sha256:9a7b73d06077ced2a02f7462f53e31a3e51e95ea5544fbcdb01e2fef094da1b6 size: 2423
time="2018-04-19T00:06:51Z" level=info msg="Running command docker inspect --format \"{{json .RepoDigests}}\" mycontainerregistry.azurecr.io/helloworld:eastus2"
"["mycontainerregistry.azurecr.io/helloworld@sha256:9a7b73d06077ced2a02f7462f53e31a3e51e95ea5544fbcdb01e2fef094da1b6"]"
time="2018-04-19T00:06:51Z" level=info msg="Running command docker inspect --format \"{{json .RepoDigests}}\" node:9-alpine"
"["node@sha256:bd7b9aaf77ab2ce1e83e7e79fc0969229214f9126ced222c64eab49dc0bdae90"]"
ACR Builder discovered the following dependencies:
- image:
    registry: mycontainerregistry.azurecr.io
    repository: helloworld
    tag: eastus2
    digest: sha256:9a7b73d06077ced2a02f7462f53e31a3e51e95ea5544fbcdb01e2fef094da1b6
  runtime-dependency:
    registry: registry.hub.docker.com
    repository: library/node
    tag: 9-alpine
    digest: sha256:5149aec8f508d48998e6230cdc8e6832cba192088b442c8ef7e23df3c6892cd3
  git:
    git-head-revision: 6944c6bd0602f96e5fecf56ff8d66e2d268223e3

Build complete
Build ID: eastus2 was successful after 39.789138274s
```

## <a name="view-build-status"></a>Status van de build weergeven

Het kan af en toe nuttig zijn om de status weer te geven van een doorlopende build die u niet handmatig hebt geactiveerd. Bijvoorbeeld wanneer u problemen oplost met builds die zijn geactiveerd door broncodedoorvoeracties. In deze sectie activeert u een handmatige build, maar onderdrukt u het standaardgedrag van het streamen van het build-logboek naar uw console. Vervolgens gebruikt u de opdracht `az acr build-task logs` om de lopende build te bewaken.

Activeer eerst een build handmatig zoals u eerder hebt gedaan, maar geef het argument `--no-logs` op om logboekregistratie naar de console te onderdrukken:

```azurecli-interactive
az acr build-task run --registry $ACR_NAME --name buildhelloworld --no-logs
```

Gebruik vervolgens de opdracht `az build-task logs` om het logboek van de momenteel actieve build weer te geven:

```azurecli-interactive
az acr build-task logs --registry $ACR_NAME
```

Het logboek voor de momenteel actieve build wordt gestreamd naar de console en ziet er ongeveer als volgt uit (hier afgekapt weergegeven):

```console
$ az acr build-task logs --registry $ACR_NAME
Showing logs for the last updated build...
Build-id: eastus3

[...]

Build complete
Build ID: eastus3 was successful after 30.076988169s
```

## <a name="trigger-a-build-with-a-commit"></a>Een build activeren met een doorvoer

Nu u de build-taak hebt getest door deze handmatig uit te voeren, gaat u deze automatisch activeren met een wijziging in de broncode.

Controleer eerst of u zich bevindt in de map waarin de lokale kloon van de [opslagplaats][sample-repo] staat:

```azurecli-interactive
cd acr-build-helloworld-node
```

Voer vervolgens de volgende opdrachten uit om een nieuw bestand te maken, door te voeren en pushen naar uw fork van de opslagplaats op GitHub:

```azurecli-interactive
echo "Hello World!" > hello.txt
git add hello.txt
git commit -m "Testing ACR Build"
git push origin master
```

U wordt mogelijk gevraagd om uw GitHub-referenties op te geven wanneer u de opdracht `git push` uitvoert. Geef uw GitHub-gebruikersnaam op en voer het persoonlijke toegangstoken (PAT) in dat u eerder hebt gemaakt voor het wachtwoord.

```console
$ git push origin master
Username for 'https://github.com': <github-username>
Password for 'https://githubuser@github.com': <personal-access-token>
```

Zodra u een doorvoer naar de opslagplaats hebt gepusht, wordt de webhook die is gemaakt door ACE Build geactiveerd en wordt een build in Azure Container Registry in gang gezet. Geef de build-logboeken weer voor de build die momenteel wordt uitgevoerd om de voortgang van de build te verifiëren en controleren:

```azurecli-interactive
az acr build-task logs --registry $ACR_NAME
```

De uitvoer is vergelijkbaar met de volgende, waarin de op dit moment uitgevoerde (of de laatst uitgevoerde) build wordt getoond:

```console
$ az acr build-task logs --registry $ACR_NAME
Showing logs for the last updated build...
Build-id: eastus4

[...]

Build complete
Build ID: eastus4 was successful after 28.9587031s
```

## <a name="list-builds"></a>Builds weergeven

Voor een overzicht van de builds die ACR Build heeft voltooid voor het register, voert u de opdracht [az acr build-task-list-builds][az-acr-build-task-list-builds] uit:

```azurecli-interactive
az acr build-task list-builds --registry $ACR_NAME --output table
```

De uitvoer van de opdracht ziet er ongeveer als volgt uit. De builds die ACR Build heeft uitgevoerd, worden weergegeven. 'Git Commit' wordt weergegeven in de kolom TRIGGER voor de meest recente build:

```console
$ az acr build-task list-builds --registry $ACR_NAME --output table
BUILD ID    TASK             PLATFORM    STATUS     TRIGGER     STARTED               DURATION
----------  ---------------  ----------  ---------  ----------  --------------------  ----------
eastus4     buildhelloworld  Linux       Succeeded  Git Commit  2018-04-20T22:50:27Z  00:00:35
eastus3     buildhelloworld  Linux       Succeeded  Manual      2018-04-20T22:47:19Z  00:00:30
eastus2     buildhelloworld  Linux       Succeeded  Manual      2018-04-20T22:46:14Z  00:00:55
eastus1                                  Succeeded  Manual      2018-04-20T22:38:22Z  00:00:55
```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie leert u hoe u een build-taak gebruikt om builds van containerinstallatiekopieën automatisch te activeren in Azure wanneer u broncode naar een Git-opslagplaats doorvoert. Ga verder naar de volgende zelfstudie voor informatie over het maken van build-taken die builds activeren wanneer de basisinstallatiekopie van de containerinstallatiekopie wordt bijgewerkt.

> [!div class="nextstepaction"]
> [Builds automatiseren op basis van installatiekopie-update](container-registry-tutorial-base-image-update.md)

<!-- LINKS - External -->
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/
[sample-repo]: https://github.com/Azure-Samples/acr-build-helloworld-node

<!-- LINKS - Internal -->
[azure-cli]: /cli/azure/install-azure-cli
[az-acr-build-task]: /cli/azure/acr#az-acr-build-task
[az-acr-build-task-create]: /cli/azure/acr#az-acr-build-task-create
[az-acr-build-task-run]: /cli/azure/acr#az-acr-build-task-run
[az-acr-build-task-list-builds]: /cli/azure/acr#az-acr-build-task-list-build

<!-- IMAGES -->
[build-task-01-new-token]: ./media/container-registry-tutorial-build-tasks/build-task-01-new-token.png
[build-task-02-generated-token]: ./media/container-registry-tutorial-build-tasks/build-task-02-generated-token.png
