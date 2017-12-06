---
title: Met behulp van ACR met een Azure-DC/OS-cluster
description: Een Azure Container Registry gebruiken met een DC/OS-cluster in Azure Container Service
services: container-service
author: julienstroheker
manager: dcaro
ms.service: container-service
ms.topic: tutorial
ms.date: 03/23/2017
ms.author: juliens
ms.custom: mvc
ms.openlocfilehash: 4a3213c28f24e9d1dfc309c6d34771ccc062dae4
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2017
---
# <a name="use-acr-with-a-dcos-cluster-to-deploy-your-application"></a>ACR met een DC/OS-cluster gebruiken om uw toepassing te implementeren

In dit artikel wordt besproken voor het gebruik van Azure Container register met een DC/OS-cluster. ACR kunt u privé opslaan en beheren van installatiekopieën van de container. Deze zelfstudie bevat de volgende taken:

> [!div class="checklist"]
> * Azure Container register implementeren (indien nodig)
> * ACR-verificatie configureren op een DC/OS-cluster
> * Een installatiekopie van een geüpload naar het register van de Container Azure
> * Een installatiekopie van een container uit het register van de Container Azure uitvoeren

U moet een ACS-DC/OS-cluster de stappen in deze zelfstudie. Indien nodig, [dit voorbeeldscript](./../kubernetes/scripts/container-service-cli-deploy-dcos.md) kunt maken voor u.

Voor deze zelfstudie is versie 2.0.4 of hoger van de Azure CLI vereist. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="deploy-azure-container-registry"></a>Register met Azure Container implementeren

Maak indien nodig een registry Azure-Container met de [az acr maken](/cli/azure/acr#create) opdracht. 

Het volgende voorbeeld wordt een register met een willekeurige naam moet worden gegenereerd. Het register wordt ook geconfigureerd met een Administrator-account met de `--admin-enabled` argument.

```azurecli-interactive
az acr create --resource-group myResourceGroup --name myContainerRegistry$RANDOM --sku Basic
```

Zodra het register is gemaakt, voert de Azure CLI gegevens uit de volgende strekking. Noteer de `name` en `loginServer`, deze worden gebruikt in latere stappen.

```azurecli
{
  "adminUserEnabled": false,
  "creationDate": "2017-06-06T03:40:56.511597+00:00",
  "id": "/subscriptions/f2799821-a08a-434e-9128-454ec4348b10/resourcegroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/myContainerRegistry23489",
  "location": "eastus",
  "loginServer": "mycontainerregistry23489.azurecr.io",
  "name": "myContainerRegistry23489",
  "provisioningState": "Succeeded",
  "sku": {
    "name": "Basic",
    "tier": "Basic"
  },
  "storageAccount": {
    "name": "mycontainerregistr034017"
  },
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}
```

Ophalen van de container register referenties met behulp van de [az acr referentie weergeven](/cli/azure/acr/credential) opdracht. Vervang de `--name` met de in de vorige stap hebt genoteerd. Let op van een wachtwoord is vereist in een later stadium.

```azurecli-interactive
az acr credential show --name myContainerRegistry23489
```

Zie voor meer informatie over Azure-Container register [Inleiding tot persoonlijke Docker-container registers](../../container-registry/container-registry-intro.md). 

## <a name="manage-acr-authentication"></a>ACR-verificatie beheren

De conventionele manier aan push als pull-installatiekopie van een persoonlijke register is het eerst worden geverifieerd met het register. Om dit te doen, gebruikt u de `docker login` opdracht op elke client die u moet toegang tot de persoonlijke register. Omdat een DC/OS-cluster veel knooppunten bevatten kan, die allemaal worden geverifieerd met de ACR moet, is het handig zijn voor het automatiseren van dit proces verspreid over alle knooppunten. 

### <a name="create-shared-storage"></a>Gedeelde opslag maken

Dit proces maakt gebruik van een Azure-bestandsshare die is gekoppeld op elk knooppunt in het cluster. Als u al geen gedeelde opslag hebt ingesteld, Zie [instellen van een bestandsshare in een DC/OS-cluster](container-service-dcos-fileshare.md).

### <a name="configure-acr-authentication"></a>ACR-verificatie configureren

Eerst de FQDN-naam van de DC/OS-master ophalen en opslaan in een variabele.

```azurecli-interactive
FQDN=$(az acs list --resource-group myResourceGroup --query "[0].masterProfile.fqdn" --output tsv)
```

Een SSH-verbinding maken met het model (of het eerste model) van uw DC/OS gebaseerde-cluster. Werk de gebruikersnaam als een niet-standaard-waarde is gebruikt bij het maken van het cluster.

```azurecli-interactive
ssh azureuser@$FQDN
```

Voer de volgende opdracht om aan te melden met het register van de Container Azure. Vervang de `--username` met de naam van het register van de container, en de `--password` met een van de opgegeven wachtwoorden. Vervang het laatste argument *mycontainerregistry.azurecr.io* in het voorbeeld met de naam loginServer van het register van de container. 

Deze opdracht worden de waarden voor de verificatie lokaal onder opgeslagen de `~/.docker` pad.

```azurecli-interactive
docker -H tcp://localhost:2375 login --username=myContainerRegistry23489 --password=//=ls++q/m+w+pQDb/xCi0OhD=2c/hST mycontainerregistry.azurecr.io
```

Maak een gecomprimeerd bestand met de verificatie van container registerwaarden.

```azurecli-interactive
tar czf docker.tar.gz .docker
```

Kopieer dit bestand naar de gedeelde clusteropslag. Deze stap maakt u het bestand beschikbaar zijn op alle knooppunten van de DC/OS-cluster.

```azurecli-interactive
cp docker.tar.gz /mnt/share/dcosshare
```

## <a name="upload-image-to-acr"></a>Installatiekopie uploaden naar ACR

Nu vanuit een ontwikkelcomputer of elk ander systeem met Docker geïnstalleerd, een installatiekopie maken en uploaden naar het register van de Container Azure.

Een container van de installatiekopie van het Ubuntu maken.

```azurecli-interactive
docker run ubuntu --name base-image
```

Nu de container vastleggen in een nieuwe installatiekopie. Naam van de installatiekopie moet bevatten de `loginServer` naam van de container registrywith een indeling van `loginServer/imageName`.

```azurecli-interactive
docker -H tcp://localhost:2375 commit base-image mycontainerregistry30678.azurecr.io/dcos-demo
````

Meld u aan in het register van de Azure-Container. Vervang de naam met de naam loginServer, de gebruikersnaam--met de naam van het register van de container, en het - wachtwoord met een van de opgegeven wachtwoorden.

```azurecli-interactive
docker login --username=myContainerRegistry23489 --password=//=ls++q/m+w+pQDb/xCi0OhD=2c/hST mycontainerregistry2675.azurecr.io
```

Ten slotte de installatiekopie uploadt naar het register ACR. In dit voorbeeld wordt een afbeelding met de naam dcos demo geüpload.

```azurecli-interactive
docker push mycontainerregistry30678.azurecr.io/dcos-demo
```

## <a name="run-an-image-from-acr"></a>Uitvoeren van een installatiekopie vanaf ACR

Voor het gebruik van een afbeelding uit het register ACR, maak een bestand namen *acrDemo.json* en kopieer de volgende tekst in de App. Vervangen van naam van de installatiekopie met de containernaam register loginServer en de installatiekopie met de naam, bijvoorbeeld `loginServer/imageName`. Noteer de `uris` eigenschap. Deze eigenschap bevat de locatie van de container register verificatiebestand, die in dit geval is de Azure-bestandsshare die is gekoppeld op elk knooppunt in de DC/OS-cluster.

```json
{
  "id": "myapp",
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "mycontainerregistry30678.azurecr.io/dcos-demo",
      "network": "BRIDGE",
      "portMappings": [
        {
          "containerPort": 80,
          "hostPort": 80,
          "protocol": "tcp",
          "name": "80",
          "labels": null
        }
      ],
      "forcePullImage":true
    }
  },
  "instances": 3,
  "cpus": 0.1,
  "mem": 65,
  "healthChecks": [{
      "protocol": "HTTP",
      "path": "/",
      "portIndex": 0,
      "timeoutSeconds": 10,
      "gracePeriodSeconds": 10,
      "intervalSeconds": 2,
      "maxConsecutiveFailures": 10
  }],
  "uris":  [
       "file:///mnt/share/dcosshare/docker.tar.gz"
   ]
}
```

De toepassing met de CLI DC/Overheadkosten implementeren.

```azurecli-interactive
dcos marathon app add acrDemo.json
```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u DC/OS met Azure Container register met inbegrip van de volgende taken moet configureren:

> [!div class="checklist"]
> * Azure Container register implementeren (indien nodig)
> * ACR-verificatie configureren op een DC/OS-cluster
> * Een installatiekopie van een geüpload naar het register van de Container Azure
> * Een installatiekopie van een container uit het register van de Container Azure uitvoeren
