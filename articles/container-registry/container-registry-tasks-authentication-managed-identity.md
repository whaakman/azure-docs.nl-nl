---
title: Een beheerde identiteit met Azure Container Registry taken gebruiken
description: Geef een Azure Container Registry taak toegang tot Azure-resources met inbegrip van andere persoonlijke container registers door een beheerde identiteit voor Azure-resources toe te wijzen.
services: container-registry
author: dlepow
manager: gwallace
ms.service: container-registry
ms.topic: article
ms.date: 06/12/2019
ms.author: danlep
ms.openlocfilehash: 46351af375ab4c6e59a3ddfba3c05c1e517fab0d
ms.sourcegitcommit: f5075cffb60128360a9e2e0a538a29652b409af9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/18/2019
ms.locfileid: "68311539"
---
# <a name="use-an-azure-managed-identity-in-acr-tasks"></a>Een door Azure beheerde identiteit gebruiken in ACR-taken 

Gebruik een [beheerde identiteit voor Azure-resources](../active-directory/managed-identities-azure-resources/overview.md) om te verifiëren van ACR-taken in een Azure container Registry of andere Azure-resources, zonder dat u referenties in code hoeft op te geven of te beheren. Gebruik bijvoorbeeld een beheerde identiteit voor het ophalen of pushen van container installatie kopieën naar een ander REGI ster als stap in een taak.

In dit artikel vindt u meer informatie over beheerde identiteiten en over het volgende:

> [!div class="checklist"]
> * Een door het systeem toegewezen identiteit of een door de gebruiker toegewezen identiteit voor een ACR-taak inschakelen
> * De identiteit toegang verlenen tot Azure-resources, zoals andere Azure-container registers
> * De beheerde identiteit gebruiken om toegang te krijgen tot de resources vanuit een taak 

Voor het maken van de Azure-resources moet u voor dit artikel de Azure CLI-versie 2.0.66 of hoger uitvoeren. Voer `az --version` uit om de versie te bekijken. Zie [Azure CLI installeren][azure-cli] als u de CLI wilt installeren of een upgrade wilt uitvoeren.

## <a name="why-use-a-managed-identity"></a>Waarom een beheerde identiteit gebruiken?

Een beheerde identiteit voor Azure-resources biedt Azure-Services met een automatisch beheerde identiteit in Azure Active Directory (Azure AD). U kunt [bepaalde Azure-resources](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md), waaronder ACR-taken, configureren met een beheerde identiteit. Gebruik vervolgens de identiteit voor toegang tot andere Azure-resources zonder referenties in code of scripts door te geven.

Beheerde identiteiten zijn van twee typen:

* Door de *gebruiker toegewezen identiteiten*, die u kunt toewijzen aan meerdere resources en zo lang als u wilt behouden. Door de gebruiker toegewezen identiteiten zijn momenteel beschikbaar als preview-versie.

* Een door het *systeem beheerde identiteit*, die uniek is voor een specifieke resource, zoals een ACR-taak en de laatste tijd voor de levens duur van die resource.

Nadat u een Azure-resource met een beheerde identiteit hebt ingesteld, verleent u de identiteit toegang tot een andere resource, net zoals elke beveiligingsprincipal. Wijs bijvoorbeeld een beheerde identiteit toe met pull, push en pull of andere machtigingen voor een persoonlijk container register in Azure. (Zie [Azure container Registry rollen en machtigingen](container-registry-roles.md)voor een volledige lijst met register rollen.) U kunt een identiteit toegang geven tot een of meer resources.

## <a name="create-container-registries"></a>Container registers maken

Voor deze zelf studie hebt u drie container registers nodig:

* U gebruikt het eerste REGI ster om ACR-taken te maken en uit te voeren. In dit artikel heeft dit bron register de naam *myregistry*. 
* De tweede en derde registers zijn doel registers voor de eerste voorbeeld taak om een installatie kopie die wordt gebouwd, te pushen. In dit artikel krijgen de doel registers de naam *customregistry1* en *customregistry2*.

Vervang door uw eigen register namen in latere stappen.

Als u nog niet beschikt over de benodigde Azure-container registers, raadpleegt [u Quick Start: Maak een persoonlijk container register met behulp van](container-registry-get-started-azure-cli.md)de Azure cli. U hoeft nog geen installatie kopieën naar het REGI ster te pushen.

## <a name="example-task-with-a-system-assigned-identity"></a>Voorbeeld: Taak met een door het systeem toegewezen identiteit

In dit voor beeld ziet u hoe u een [taak met meerdere stappen](container-registry-tasks-multi-step.md) maakt met een door het systeem toegewezen identiteit. De taak bouwt een installatie kopie en gebruikt vervolgens de identiteit voor verificatie met twee doel registers om de installatie kopie te pushen.

De stappen voor deze voorbeeld taak worden gedefinieerd in een [yaml](container-registry-tasks-reference-yaml.md) -bestand `testtask.yaml`met de naam. Het bestand bevindt zich in de map multipleRegistries van de [ACR-taken-](https://github.com/Azure-Samples/acr-tasks) voor beelden opslag plaats. Het bestand wordt hier opnieuw geproduceerd:

```yml
version: v1.0.0
steps:
  - build: -t {{.Values.REGISTRY1}}/hello-world:{{.Run.ID}} . -f hello-world.dockerfile
  - push: ["{{.Values.REGISTRY1}}/hello-world:{{.Run.ID}}"]
  - build: -t {{.Values.REGISTRY2}}/hello-world:{{.Run.ID}} . -f hello-world.dockerfile
  - push: ["{{.Values.REGISTRY2}}/hello-world:{{.Run.ID}}"]
```

### <a name="create-task-with-system-assigned-identity"></a>Taak maken met door het systeem toegewezen identiteit

Maak de taak *meerdere-reg* door de volgende [AZ ACR taak Create][az-acr-task-create] opdracht uit te voeren. De taak context is de map multipleRegistries van de opslag plaats-voor beelden en de opdracht verwijst naar `testtask.yaml` het bestand in de opslag plaats. Met `--assign-identity` de para meter zonder extra waarde wordt een door het systeem toegewezen identiteit voor de taak gemaakt. Deze taak is ingesteld zodat u deze hand matig kunt activeren, maar u kunt deze zo instellen dat deze wordt uitgevoerd wanneer door voeringen naar de opslag plaats worden gepusht of een pull-aanvraag wordt gedaan. 

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

In de uitvoer van de opdracht `identity` ziet u in de sectie een `SystemAssigned` identiteit van het type is ingesteld in de taak. De `principalId` is de Service-Principal-id van de identiteit:

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

Gebruik de opdracht [AZ ACR Task show][az-acr-task-show] om de in `principalId` een variabele op te slaan, zodat u deze in latere opdrachten kunt gebruiken:

```azurecli
principalID=$(az acr task show --name multiple-reg --registry myregistry --query identity.principalId --output tsv)
```

### <a name="give-identity-push-permissions-to-two-target-container-registries"></a>Machtigingen voor het pushen van de identiteit aan twee doel container registers geven

In deze sectie geeft u de door het systeem toegewezen identiteits machtigingen om naar de twee doel registers te pushen, met de naam *customregistry1* en *customregistry2*.

Gebruik eerst de opdracht [AZ ACR show][az-acr-show] om de resource-id van elk REGI ster op te halen en de id's op te slaan in variabelen:

```azurecli
reg1_id=$(az acr show --name customregistry1 --query id --output tsv)
reg2_id=$(az acr show --name customregistry2 --query id --output tsv)
```

Gebruik de opdracht [AZ Role Assignment Create][az-role-assignment-create] om de `acrpush` rol-id aan elk REGI ster toe te wijzen. Deze rol heeft machtigingen om installatie kopieën te halen en te pushen naar een container register.

```azurecli
az role assignment create --assignee $principalID --scope $reg1_id --role acrpush
az role assignment create --assignee $principalID --scope $reg2_id --role acrpush
```

### <a name="add-target-registry-credentials-to-task"></a>Doel register referenties toevoegen aan taak

Gebruik nu de opdracht [AZ ACR taak Credential add][az-acr-task-credential-add] om de referenties van de identiteit toe te voegen aan de taak, zodat deze kan worden geverifieerd met de doel registers.

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

### <a name="manually-run-the-task"></a>De taak hand matig uitvoeren

Gebruik de opdracht [AZ ACR Task run][az-acr-task-run] om de taak hand matig te activeren. De `--set` para meter wordt gebruikt om de namen van de aanmeldings server van de twee doel registers door te geven als `REGISTRY1` waarden `REGISTRY2`voor de taak variabelen en.

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

## <a name="example-task-with-a-user-assigned-identity"></a>Voorbeeld: Taak met een door de gebruiker toegewezen identiteit

In dit voor beeld maakt u een door de gebruiker toegewezen identiteit met machtigingen voor het lezen van geheimen van een Azure-sleutel kluis. U wijst deze identiteit toe aan een taak met meerdere stappen die het geheim leest, een installatie kopie bouwt en zich aanmeldt bij de Azure CLI om de afbeeldings code te lezen.

### <a name="create-a-key-vault-and-store-a-secret"></a>Een sleutel kluis maken en een geheim opslaan

Eerst moet u, indien nodig, een resource groep met de naam *myResourceGroup* maken op de locatie *eastus* met de volgende opdracht [AZ Group Create][az-group-create] :

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

Gebruik de opdracht [AZ Key kluis Create][az-keyvault-create] om een sleutel kluis te maken. Zorg ervoor dat u een unieke naam voor de sleutel kluis opgeeft. 

```azurecli-interactive
az keyvault create --name mykeyvault --resource-group myResourceGroup --location eastus
```

Sla een voor beeld geheim op in de sleutel kluis met behulp van de opdracht [AZ Key kluis Secret set][az-keyvault-secret-set] :

```azurecli
az keyvault secret set \
  --name SampleSecret \
  --value "Hello ACR Tasks!" \
  --description ACRTasksecret  \
  --vault-name mykeyvault
```

Stel dat u referenties wilt opslaan om te verifiëren met een privé-docker-REGI ster, zodat u een persoonlijke installatie kopie kunt ophalen.

### <a name="create-an-identity"></a>Een identiteit maken

Maak een identiteit met de naam *myACRTasksId* in uw abonnement met behulp van de opdracht [AZ Identity Create][az-identity-create] . U kunt dezelfde resource groep gebruiken die u eerder hebt gebruikt voor het maken van een container register of sleutel kluis of een andere.

```azurecli-interactive
az identity create --resource-group myResourceGroup --name myACRTasksId
```

Als u de identiteit in de volgende stappen wilt configureren, gebruikt u de opdracht [AZ Identity show][az-identity-show] om de resource-id van de identiteit en de Service-Principal-id in variabelen op te slaan.

```azurecli
# Get resource ID of the user-assigned identity
resourceID=$(az identity show --resource-group myResourceGroup --name myACRTasksId --query id --output tsv)

# Get service principal ID of the user-assigned identity
principalID=$(az identity show --resource-group myResourceGroup --name myACRTasksId --query principalId --output tsv)
```

### <a name="grant-identity-access-to-keyvault-to-read-secret"></a>Identiteits toegang verlenen aan de sleutel kluis om geheim te lezen

Voer de volgende [AZ-set-Policy][az-keyvault-set-policy] opdracht uit om een toegangs beleid in te stellen op de sleutel kluis. In het volgende voor beeld kan de door de gebruiker toegewezen identiteit worden verkregen van geheimen van de sleutel kluis. Deze toegang is later nodig om een taak met meerdere stappen te kunnen uitvoeren.

```azurecli-interactive
 az keyvault set-policy --name mykeyvault --resource-group myResourceGroup --object-id $principalID --secret-permissions get
```

### <a name="grant-identity-reader-access-to-the-resource-group-for-registry"></a>Identiteits lezers toegang verlenen tot de resource groep voor het REGI ster

Voer de volgende opdracht [AZ Role Assignment Create][az-role-assignment-create] uit om de rol identiteit een lezer toe te wijzen, in dit geval aan de resource groep met het bron register. Deze rol is later nodig om een taak met meerdere stappen te kunnen uitvoeren.

```azurecli
az role assignment create --role reader --resource-group myResourceGroup --assignee $principalID
```

### <a name="create-task-with-user-assigned-identity"></a>Taak maken met door gebruiker toegewezen identiteit

Maak nu een [taak met meerdere stappen](container-registry-tasks-multi-step.md) en wijs deze toe aan de door de gebruiker toegewezen identiteit. Maak voor deze voorbeeld taak een [yaml-bestand](container-registry-tasks-reference-yaml.md) met `managed-identities.yaml` de naam in een lokale werkmap en plak de volgende inhoud. Zorg ervoor dat u de naam van de sleutel kluis in het bestand vervangt door de naam van uw sleutel kluis

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

* Verifieert of het toegang heeft tot het geheim in uw sleutel kluis. Deze stap is bedoeld voor demonstratie doeleinden. In een praktijk scenario hebt u mogelijk een taak stap nodig om referenties op te halen voor toegang tot een privé-docker hub-opslag plaats.
* Bouwt en duwt de `mywebsite` installatie kopie naar het bron register.
* Meldt zich aan bij de Azure cli `my-website` om de afbeeldings codes in het bron register weer te geven.

Maak een taak met de naam *msitask* en geef deze de resource-id van de door de gebruiker toegewezen identiteit die u eerder hebt gemaakt. Deze voorbeeld taak wordt gemaakt op basis `managed-identities.yaml` van het bestand dat u in uw lokale werkmap hebt opgeslagen. u moet dit dus hand matig activeren.

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

In de uitvoer van de opdracht `identity` ziet u in de sectie een `UserAssigned` identiteit van het type is ingesteld in de taak. De `principalId` is de Service-Principal-id van de identiteit:

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

### <a name="manually-run-the-task"></a>De taak hand matig uitvoeren

Gebruik de opdracht [AZ ACR Task run][az-acr-task-run] om de taak hand matig te activeren. De `--set` para meter wordt gebruikt om de naam van het bron register door te geven aan de taak:

```azurecli
az acr task run --name msitask --registry myregistry --set registryName=myregistry  
```

In de uitvoer ziet u dat het geheim is opgelost, de installatie kopie is gemaakt en gepusht. de taak meldt zich aan bij de Azure CLI met de identiteit voor het lezen van de afbeeldings code uit het bron register:

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

In dit artikel hebt u geleerd hoe u beheerde identiteiten gebruikt met Azure Container Registry taken en hoe u:

> [!div class="checklist"]
> * Een door het systeem toegewezen identiteit of door de gebruiker toegewezen voor een ACR-taak inschakelen
> * De identiteit toegang verlenen tot Azure-resources, zoals andere Azure-container registers
> * De beheerde identiteit gebruiken om toegang te krijgen tot de resources vanuit een taak  

* Meer informatie over [beheerde identiteiten voor Azure-resources](/azure/active-directory/managed-identities-azure-resources/).

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
