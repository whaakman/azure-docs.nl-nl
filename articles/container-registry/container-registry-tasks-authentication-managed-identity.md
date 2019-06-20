---
title: Gebruik van een beheerde identiteit met Azure Container Registry-taken
description: Een Azure Container Registry taak toegang bieden tot Azure-resources met inbegrip van andere persoonlijke containerregisters een beheerde identiteit voor Azure-Resources toe te wijzen.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: article
ms.date: 06/12/2019
ms.author: danlep
ms.openlocfilehash: 5b60727472a06aaac8ccd3dce8609461e8972311
ms.sourcegitcommit: 72f1d1210980d2f75e490f879521bc73d76a17e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/14/2019
ms.locfileid: "67148034"
---
# <a name="use-an-azure-managed-identity-in-acr-tasks"></a>Gebruik van een door Azure beheerde identiteit in de ACR-taken 

Gebruik een [beheerde identiteit voor de Azure-resources](../active-directory/managed-identities-azure-resources/overview.md) om te verifiëren vanuit ACR taken een Azure container registry of andere Azure-resources, zonder dat u hoeft te bieden of de referenties in code te beheren. Een beheerde identiteit bijvoorbeeld gebruiken om te halen of duw containerinstallatiekopieën naar een andere register als een stap in een taak.

In dit artikel leert u meer informatie over beheerde identiteiten en hoe u:

> [!div class="checklist"]
> * Een systeem toegewezen identiteit of een gebruiker toegewezen identiteit voor een ACR-taak inschakelen
> * De identiteit toegang verlenen tot Azure-resources, zoals andere Azure-containerregisters
> * De beheerde identiteit gebruiken voor toegang tot de resources van een taak 

Voor het maken van de Azure-resources in dit artikel is vereist dat u de Azure CLI versie 2.0.66 uitvoert of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren][azure-cli].

## <a name="why-use-a-managed-identity"></a>Waarom een beheerde identiteit gebruiken?

Een beheerde identiteit voor Azure-resources biedt Azure-services met een automatisch beheerde identiteit in Azure Active Directory (Azure AD). U kunt configureren [bepaalde Azure-resources](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md), met inbegrip van de ACR-taken met een beheerde identiteit. Vervolgens gebruikt u de identiteit voor toegang tot andere Azure-resources, zonder het doorgeven van referenties in code of scripts.

Beheerde identiteiten zijn twee typen:

* *Gebruiker toegewezen identiteiten*, die u kunt toewijzen aan meerdere resources en voor zolang als uw wilt behouden. Gebruiker toegewezen identiteiten zijn momenteel in preview.

* Een *system beheerde identiteit*, die uniek is voor een bepaalde resource, zoals een ACR-taak en de looptijd is de levensduur van die resource.

Na het instellen van een Azure-resource met een beheerde identiteit, de identiteit toegang verlenen tot een andere bron, net als bij alle beveiligings-principal. Bijvoorbeeld, een beheerde identiteit een rol toewijzen met pull, push als pull of andere machtigingen aan een privécontainerregister in Azure. (Zie voor een volledige lijst van register rollen [Azure Container Registry-rollen en machtigingen](container-registry-roles.md).) U kunt een identiteit toegang geven tot een of meer resources.

## <a name="create-container-registries"></a>Container Registry maken

Voor deze zelfstudie moet u drie containerregisters:

* U kunt het eerste register maken en ACR taken uitvoeren. In dit artikel wordt dit register bron met de naam *myregistry*. 
* De tweede en derde registers zijn doel-registers voor de eerste voorbeeld van de taak om een installatiekopie die Hiermee bouwt te pushen. In dit artikel wordt de doel-registers zijn benoemde *customregistry1* en *customregistry2*.

Vervangen door uw eigen register in latere stappen.

Als u de benodigde Azure-containerregisters nog geen hebt, raadpleegt u [Quick Start: Een privé containerregister maken met de Azure CLI](container-registry-get-started-azure-cli.md). U hoeft niet te installatiekopieën nog in het register pushen.

## <a name="example-task-with-a-system-assigned-identity"></a>Voorbeeld: Taak met een door het systeem toegewezen identiteit

In dit voorbeeld laat zien hoe maakt u een [taak meerdere stappen](container-registry-tasks-multi-step.md) met een door het systeem toegewezen identiteit. De taak wordt gemaakt van een afbeelding en vervolgens gebruikt om de identiteit te verifiëren met twee doel registers om de installatiekopie te pushen.

De stappen voor deze taak voorbeeld zijn gedefinieerd in een [YAML-bestand](container-registry-tasks-reference-yaml.md) met de naam `testtask.yaml`. Het bestand bevindt zich in de map multipleRegistries van de [acr-taken](https://github.com/Azure-Samples/acr-tasks) voorbeelden van de opslagplaats. Het bestand wordt hier weergegeven:

```yml
version: v1.0.0
steps:
  - build: -t {{.Values.REGISTRY1}}/hello-world:{{.Run.ID}} . -f hello-world.dockerfile
  - push: ["{{.Values.REGISTRY1}}/hello-world:{{.Run.ID}}"]
  - build: -t {{.Values.REGISTRY2}}/hello-world:{{.Run.ID}} . -f hello-world.dockerfile
  - push: ["{{.Values.REGISTRY2}}/hello-world:{{.Run.ID}}"]
```

### <a name="create-task-with-system-assigned-identity"></a>Taak maken met het systeem toegewezen identiteit

Maken van de taak *meerdere reg* door het uitvoeren van de volgende [az acr-taak maken] [ az-acr-task-create] opdracht. De context van de taak is de map multipleRegistries van de opslagplaats met voorbeelden en de opdracht verwijst naar het bestand `testtask.yaml` in de opslagplaats. De `--assign-identity` parameter zonder extra waarde maakt u een systeem toegewezen identiteit voor de taak. Deze taak is ingesteld, zodat u moet deze handmatig te activeren, maar u deze instellen kunt om uit te voeren wanneer doorvoeracties naar de opslagplaats worden gepusht of een pull-aanvraag wordt gedaan. 

```azurecli
az acr task create \
  --registry myregistry \
  --name multiple-reg \
  --context https://github.com/Azure-Samples/acr-tasks.git#:multipleRegistries \
  --file testtask.yaml \
  --commit-trigger-enabled false \
  --pull-request-trigger-enabled false \
  --assign-identity
```

In de uitvoer van de opdracht, de `identity` sectie ziet u een identiteit van het type `SystemAssigned` is ingesteld in de taak. De `principalId` is de service-principal-ID van de identiteit:

```console
[...]
  "identity": {
    "principalId": "xxxxxxxx-2703-42f9-97d0-xxxxxxxxxxxx",
    "tenantId": "xxxxxxxx-86f1-41af-91ab-xxxxxxxxxxxx",
    "type": "SystemAssigned",
    "userAssignedIdentities": null
  },
  "location": "eastus",
[...]
``` 

Gebruik de [az acr taak weergeven] [ az-acr-task-show] opdracht voor het opslaan van de `principalId` in een variabele, voor gebruik in latere opdrachten:

```azurecli
principalID=$(az acr task show --name multiple-reg --registry myregistry --query identity.principalId --output tsv)
```

### <a name="give-identity-push-permissions-to-two-target-container-registries"></a>Identiteit push machtigingen tot twee containerregisters van doel

In deze sectie geeft u de Identiteitsmachtigingen van het systeem toegewezen om naar de twee target-registers, met de naam te pushen *customregistry1* en *customregistry2*.

Voor het eerst gebruiken de [az acr show] [ az-acr-show] opdracht voor het ophalen van de resource-ID van elk register en de id's opgeslagen in variabelen:

```azurecli
reg1_id=$(az acr show --name customregistry1 --query id --output tsv)
reg2_id=$(az acr show --name customregistry2 --query id --output tsv)
```

Gebruik de [az roltoewijzing maken] [ az-role-assignment-create] opdracht voor het toewijzen van de identiteit het `acrpush` elk register-rol. Deze rol heeft machtigingen voor het pull-abonnementen en push-installatiekopieën naar een containerregister.

```azurecli
az role assignment create --assignee $principalID --scope $reg1_id --role acrpush
az role assignment create --assignee $principalID --scope $reg2_id --role acrpush
```

### <a name="add-target-registry-credentials-to-task"></a>Doel-registerreferenties toevoegen aan de taak

Gebruik nu de [az acr taak referentie toevoegen] [ az-acr-task-credential-add] opdracht van de ID-referenties toevoegen aan de taak zodat deze kan worden geverifieerd met beide registers doel.

```azurecli
az acr task credential add \
  --name multiple-reg \
  --registry myregistry \
  --login-server customregistry1.azurecr.io \
  --use-identity [system]

az acr task credential add \
  --name multiple-reg \
  --registry myregistry \
  --login-server customregistry2.azurecr.io \
  --use-identity [system]
```

### <a name="manually-run-the-task"></a>De taak uitvoeren handmatig

Gebruik de [az acr-taak uitvoeren] [ az-acr-task-run] opdracht voor het handmatig activeren van de taak. De `--set` parameter wordt gebruikt om door te geven in de namen van de aanmelding van de twee target-registers als waarden voor de variabelen voor de `REGISTRY1` en `REGISTRY2`.

```azurecli
az acr task run \
  --name multiple-reg \
  --registry myregistry \
  --set REGISTRY1=customregistry1.azurecr.io \
  --set REGISTRY2=customregistry2.azurecr.io
```

De uitvoer ziet er ongeveer zo uit:

```console
Sending build context to Docker daemon  4.096kB
Step 1/1 : FROM hello-world
 ---> fce289e99eb9
Successfully built fce289e99eb9
Successfully tagged customregistry2.azurecr.io/hello-world:cf31
2019/05/31 22:16:02 Successfully executed container: acb_step_0
2019/05/31 22:16:02 Executing step ID: acb_step_1. Timeout(sec): 600, Working directory: 'multipleRegistries', Network: 'acb_default_network'
2019/05/31 22:16:02 Pushing image: customregistry2.azurecr.io/hello-world:cf31, attempt 1
The push refers to repository [customregistry2.azurecr.io/hello-world]
af0b15c8625b: Preparing
af0b15c8625b: Pushed
cf31: digest: sha256:92c7f9c92844bbbb5d0a101b22f7c2a7949e40f8ea90c8b3bc396879d95e899a size: 524
2019/05/31 22:16:08 Successfully pushed image: customregistry2.azurecr.io/hello-world:cf31
2019/05/31 22:16:08 Executing step ID: acb_step_2. Timeout(sec): 600, Working directory: 'multipleRegistries', Network: 'acb_default_network'
2019/05/31 22:16:08 Scanning for dependencies...
2019/05/31 22:16:08 Successfully scanned dependencies
2019/05/31 22:16:08 Launching container with name: acb_step_2
Sending build context to Docker daemon  4.096kB
Step 1/1 : FROM hello-world
 ---> fce289e99eb9
Successfully built fce289e99eb9
Successfully tagged customregistry1.azurecr.io/hello-world:cf31
2019/05/31 22:16:09 Successfully executed container: acb_step_2
2019/05/31 22:16:09 Executing step ID: acb_step_3. Timeout(sec): 600, Working directory: 'multipleRegistries', Network: 'acb_default_network'
2019/05/31 22:16:09 Pushing image: customregistry1.azurecr.io/hello-world:cf31, attempt 1
The push refers to repository [customregistry1.azurecr.io/hello-world]
af0b15c8625b: Preparing
af0b15c8625b: Pushed
cf31: digest: sha256:92c7f9c92844bbbb5d0a101b22f7c2a7949e40f8ea90c8b3bc396879d95e899a size: 524
2019/05/31 22:16:21 Successfully pushed image: customregistry1.azurecr.io/hello-world:cf31
2019/05/31 22:16:21 Step ID: acb_step_0 marked as successful (elapsed time in seconds: 1.985291)
2019/05/31 22:16:21 Populating digests for step ID: acb_step_0...
2019/05/31 22:16:22 Successfully populated digests for step ID: acb_step_0
2019/05/31 22:16:22 Step ID: acb_step_1 marked as successful (elapsed time in seconds: 5.743225)
2019/05/31 22:16:22 Step ID: acb_step_2 marked as successful (elapsed time in seconds: 1.925959)
2019/05/31 22:16:22 Populating digests for step ID: acb_step_2...
2019/05/31 22:16:24 Successfully populated digests for step ID: acb_step_2
2019/05/31 22:16:24 Step ID: acb_step_3 marked as successful (elapsed time in seconds: 11.061057)
2019/05/31 22:16:24 The following dependencies were found:
2019/05/31 22:16:24
- image:
    registry: customregistry2.azurecr.io
    repository: hello-world
    tag: cf31
    digest: sha256:92c7f9c92844bbbb5d0a101b22f7c2a7949e40f8ea90c8b3bc396879d95e899a
  runtime-dependency:
    registry: registry.hub.docker.com
    repository: library/hello-world
    tag: latest
    digest: sha256:6f744a2005b12a704d2608d8070a494ad1145636eeb74a570c56b94d94ccdbfc
  git:
    git-head-revision: 05275dca2bc61f584085ca913c39d509236f576b
- image:
    registry: customregistry1.azurecr.io
    repository: hello-world
    tag: cf31
    digest: sha256:92c7f9c92844bbbb5d0a101b22f7c2a7949e40f8ea90c8b3bc396879d95e899a
  runtime-dependency:
    registry: registry.hub.docker.com
    repository: library/hello-world
    tag: latest
    digest: sha256:6f744a2005b12a704d2608d8070a494ad1145636eeb74a570c56b94d94ccdbfc
  git:
    git-head-revision: 05275dca2bc61f584085ca913c39d509236f576b

Run ID: cf31 was successful after 35s
```

## <a name="example-task-with-a-user-assigned-identity"></a>Voorbeeld: Taak met de identiteit van een gebruiker toegewezen

In dit voorbeeld maakt u de identiteit van een gebruiker toegewezen met machtigingen voor het lezen van geheimen vanuit een Azure key vault. U toewijzen deze identiteit aan een met meerdere stappen taak die leest het geheim en zich in de Azure CLI bij het lezen van de tag de installatiekopie van een installatiekopie is gebaseerd.

### <a name="create-a-key-vault-and-store-a-secret"></a>Een sleutelkluis maken en opslaan van een geheim

Als u nodig hebt, maak eerst een resourcegroep met de naam *myResourceGroup* in de *eastus* locatie met de volgende [az-groep maken] [ az-group-create]opdracht:

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

Gebruik de [az keyvault maken] [ az-keyvault-create] opdracht om een sleutelkluis te maken. Zorg ervoor dat een unieke key vault-naam opgeven. 

```azurecli-interactive
az keyvault create --name mykeyvault --resource-group myResourceGroup --location eastus
```

Een voorbeeld-geheim Store in de sleutelkluis met de [az keyvault secret set] [ az-keyvault-secret-set] opdracht:

```azurecli
az keyvault secret set \
  --name SampleSecret \
  --value "Hello ACR Tasks!" \
  --description ACRTasksecret  \
  --vault-name mykeyvault
```

Bijvoorbeeld, kunt u het opslaan van referenties voor verificatie met een persoonlijk dockerregister, zodat u kunt een persoonlijke installatiekopie ophalen.

### <a name="create-an-identity"></a>Een identiteit maken

Maak een identiteit met de naam *myACRTasksId* in uw abonnement met de [az-identiteit maken] [ az-identity-create] opdracht. U kunt de dezelfde resourcegroep die u eerder hebt gebruikt om te maken van een container registry key vault is, of een ander account gebruiken.

```azurecli-interactive
az identity create --resource-group myResourceGroup --name myACRTasksId
```

Gebruik voor het configureren van de identiteit in de volgende stappen uit de [az identiteit show] [ az-identity-show] opdracht voor het opslaan van de identiteit van de bron- en service-principal-ID in de variabelen.

```azurecli
# Get resource ID of the user-assigned identity
resourceID=$(az identity show --resource-group myResourceGroup --name myACRTasksId --query id --output tsv)

# Get service principal ID of the user-assigned identity
principalID=$(az identity show --resource-group myResourceGroup --name myACRTasksId --query principalId --output tsv)
```

### <a name="grant-identity-access-to-keyvault-to-read-secret"></a>Identiteit toegang verlenen tot Key Vault secret lezen

Voer de volgende [az keyvault-beleid instellen] [ az-keyvault-set-policy] -opdracht uit om een toegangsbeleid voor de sleutelkluis. Het volgende voorbeeld kunt de gebruiker toegewezen identiteit geheimen ophalen uit de key vault. Deze toegang later opnieuw nodig is voor een geslaagde uitvoering van een taak meerdere stappen.

```azurecli-interactive
 az keyvault set-policy --name mykeyvault --resource-group myResourceGroup --object-id $principalID --secret-permissions get
```

### <a name="grant-identity-reader-access-to-the-resource-group-for-registry"></a>Identity-lezerstoegang verlenen aan de resourcegroep voor register

Voer de volgende [az roltoewijzing maken] [ az-role-assignment-create] opdracht om toe te wijzen de identiteit een rol van lezer, in dit geval op de resourcegroep met de bron-register. Deze rol is later nodig om een taak meerdere stappen worden uitgevoerd.

```azurecli
az role assignment create --role reader --resource-group myResourceGroup --assignee $principalID
```

### <a name="create-task-with-user-assigned-identity"></a>Taak met de gebruiker toegewezen identiteit maken

Maak nu een [taak meerdere stappen](container-registry-tasks-multi-step.md) en toe te wijzen de identiteit van de gebruiker toegewezen. Voor deze taak voorbeeld maakt u een [YAML-bestand](container-registry-tasks-reference-yaml.md) met de naam `managed-identities.yaml` in een lokale werkmap en plak de volgende inhoud. Zorg ervoor dat u de naam van de sleutelkluis in het bestand vervangen door de naam van uw key vault

```yml
version: v1.0.0
# Replace mykeyvault with the name of your key vault
secrets:
  - id: name
    keyvault: https://mykeyvault.vault.azure.net/secrets/SampleSecret
steps:
  # Verify that the task can access the secret in the key vault
  - cmd: bash -c 'if [ -z "$MY_SECRET" ]; then echo "Secret not resolved"; else echo "Secret resolved"; fi'
    env: 
      - MY_SECRET='{{.Secrets.name}}' 

  # Build/push the website image to source registry, using dockerfile in the Azure-Samples repo
  - cmd: docker build -t {{.Run.Registry}}/my-website:{{.Run.ID}} https://github.com/Azure-Samples/aci-helloworld.git
  - push: 
    - "{{.Run.Registry}}/my-website:{{.Run.ID}}"
  
  # Login to Azure CLI with identity and list the tags to verify that the image is in the registry
  - cmd: microsoft/azure-cli az login --identity
  - cmd: microsoft/azure-cli az acr repository show-tags -n {{.Values.registryName}} --repository my-website
```

Deze taak doet het volgende:

* Controleert of deze toegang heeft tot het geheim in uw key vault. Deze stap is voor demonstratiedoeleinden te gebruiken. In een Praktijkscenario moet u mogelijk de stap van een taak voor het ophalen van referenties voor toegang tot een persoonlijke Docker Hub-opslagplaats.
* Bouwt en pushes de `mywebsite` installatiekopie naar het register van de bron.
* Logboeken naar de Azure CLI aan lijst met de `my-website` image labels in het register van de bron.

Maak een taak met de naam *msitask* en geeft die door de resource-ID van de gebruiker toegewezen identiteit u eerder hebt gemaakt. In dit voorbeeld van de taak is gemaakt op basis van de `managed-identities.yaml` bestand dat u in uw lokale werkmap hebt opgeslagen, zodat u moet deze handmatig te activeren.

```azurecli
az acr task create \
  --name msitask \
  --registry myregistry \
  --context /dev/null \
  --file managed-identities.yaml \
  --pull-request-trigger-enabled false \
  --commit-trigger-enabled false \
  --assign-identity $resourceID
```

In de uitvoer van de opdracht, de `identity` sectie ziet u een identiteit van het type `UserAssigned` is ingesteld in de taak. De `principalId` is de service-principal-ID van de identiteit:

```console
[...]
"identity": {
    "principalId": null,
    "tenantId": null,
    "type": "UserAssigned",
    "userAssignedIdentities": {
      "/subscriptions/xxxxxxxx-d12e-4760-9ab6-xxxxxxxxxxxx/resourcegroups/myResourceGroup/providers/Microsoft.ManagedIdentity/userAssignedIdentities/myACRTasksId": {
        "clientId": "xxxxxxxx-f17e-4768-bb4e-xxxxxxxxxxxx",
        "principalId": "xxxxxxxx-1335-433d-bb6c-xxxxxxxxxxxx"
      }
[...]
```

### <a name="manually-run-the-task"></a>De taak uitvoeren handmatig

Gebruik de [az acr-taak uitvoeren] [ az-acr-task-run] opdracht voor het handmatig activeren van de taak. De `--set` parameter wordt gebruikt om door te geven in de naam van het register op de taak:

```azurecli
az acr task run --name msitask --registry myregistry --set registryName=myregistry  
```

Uitvoer ziet u het geheim is opgelost, de installatiekopie is gemaakt en gepusht, en de taak meldt zich aan bij de Azure CLI met de identiteit op de tag installatiekopie lezen uit het register van de bron:

```console
Queued a run with ID: cf32
Waiting for an agent...
2019/06/10 23:25:37 Downloading source code...
2019/06/10 23:25:38 Finished downloading source code
2019/06/10 23:25:39 Using acb_vol_4e4a0a7c-b6ef-4ec5-b40f-3436fc5eb0f5 as the home volume
2019/06/10 23:25:41 Creating Docker network: acb_default_network, driver: 'bridge'
2019/06/10 23:25:41 Successfully set up Docker network: acb_default_network
2019/06/10 23:25:41 Setting up Docker configuration...
2019/06/10 23:25:42 Successfully set up Docker configuration
2019/06/10 23:25:42 Logging in to registry: myregistry.azurecr.io
2019/06/10 23:25:43 Successfully logged into myregistry.azurecr.io
2019/06/10 23:25:43 Executing step ID: acb_step_0. Timeout(sec): 600, Working directory: '', Network: 'acb_default_network'
2019/06/10 23:25:43 Launching container with name: acb_step_0
Secret resolved
2019/06/10 23:25:44 Successfully executed container: acb_step_0
2019/06/10 23:25:44 Executing step ID: acb_step_1. Timeout(sec): 600, Working directory: '', Network: 'acb_default_network'
2019/06/10 23:25:44 Launching container with name: acb_step_1
Sending build context to Docker daemon  74.75kB

[...]

cf32: digest: sha256:cbb4aa83b33f6959d83e84bfd43ca901084966a9f91c42f111766473dc977f36 size: 1577
2019/06/10 23:26:05 Successfully pushed image: myregistry.azurecr.io/my-website:cf32
2019/06/10 23:26:05 Executing step ID: acb_step_3. Timeout(sec): 600, Working directory: '', Network: 'acb_default_network'
2019/06/10 23:26:05 Launching container with name: acb_step_3
[
  {
    "environmentName": "AzureCloud",
    "id": "xxxxxxxx-d12e-4760-9ab6-xxxxxxxxxxxx",
    "isDefault": true,
    "name": "DanLep Internal Consumption",
    "state": "Enabled",
    "tenantId": "xxxxxxxx-86f1-41af-91ab-xxxxxxxxxxxx",
      "user": {
      "assignedIdentityInfo": "MSI",
      "name": "systemAssignedIdentity",
      "type": "servicePrincipal"
    }
  }
]
2019/06/10 23:26:09 Successfully executed container: acb_step_3
2019/06/10 23:26:09 Executing step ID: acb_step_4. Timeout(sec): 600, Working directory: '', Network: 'acb_default_network'
2019/06/10 23:26:09 Launching container with name: acb_step_4
[
  "cf32"
]
2019/06/10 23:26:14 Successfully executed container: acb_step_4
2019/06/10 23:26:14 Step ID: acb_step_0 marked as successful (elapsed time in seconds: 1.025312)
2019/06/10 23:26:14 Step ID: acb_step_1 marked as successful (elapsed time in seconds: 13.703823)
2019/06/10 23:26:14 Step ID: acb_step_2 marked as successful (elapsed time in seconds: 6.791506)
2019/06/10 23:26:14 Step ID: acb_step_3 marked as successful (elapsed time in seconds: 3.852972)
2019/06/10 23:26:14 Step ID: acb_step_4 marked as successful (elapsed time in seconds: 5.079079)
```


## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd over het gebruik van beheerde identiteiten met Azure Container Registry taken en hoe u:

> [!div class="checklist"]
> * Een systeem toegewezen identiteit inschakelen of de gebruiker zijn toegewezen aan een ACR-taak
> * De identiteit toegang verlenen tot Azure-resources, zoals andere Azure-containerregisters
> * De beheerde identiteit gebruiken voor toegang tot de resources van een taak  

* Meer informatie over [beheerde identiteiten voor een Azure-resources](/azure/active-directory/managed-identities-azure-resources/).

<!-- LINKS - Internal -->
[az-login]: /cli/azure/reference-index#az-login
[az-acr-login]: /cli/azure/acr#az-acr-login
[az-acr-show]: /cli/azure/acr#az-acr-show
[az-role-assignment-create]: /cli/azure/role/assignment#az-role-assignment-create
[az-acr-login]: /cli/azure/acr#az-acr-login
[az-identity-create]: /cli/azure/identity?view=azure-cli-latest#az-identity-create
[az-identity-show]: /cli/azure/identity#az-identity-show
[azure-cli]: /cli/azure/install-azure-cli
[az-acr-task-create]: /cli/azure/acr/task#az-acr-task-create
[az-acr-task-show]: /cli/azure/acr/task#az-acr-task-show
[az-acr-task-run]: /cli/azure/acr/task#az-acr-task-run
[az-acr-task-list-runs]: /cli/azure/acr/task#az-acr-task-list-runs
[az-acr-task-credential-add]: /cli/azure/acr/task/credential#az-acr-task-credential-add
[az-group-create]: /cli/azure/group?#az-group-create
[az-keyvault-create]: /cli/azure/keyvault?#az-keyvault-create
[az-keyvault-secret-set]: /cli/azure/keyvault/secret#az-keyvault-secret-set
[az-keyvault-set-policy]: /cli/azure/keyvault#az-keyvault-set-policy
