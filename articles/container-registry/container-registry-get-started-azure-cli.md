---
title: Quick Start - een persoonlijke Docker-register maken in Azure met Azure CLI
description: Snel informatie over het maken van een persoonlijke register van de Docker-container met de Azure CLI.
services: container-registry
author: neilpeterson
manager: timlt
ms.service: container-registry
ms.topic: quicksart
ms.date: 12/07/2017
ms.author: nepeters
ms.custom: H1Hack27Feb2017, mvc
ms.openlocfilehash: f31f4e5e2b3fe5db85873894a7f2fa9c415392c1
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="create-a-container-registry-using-the-azure-cli"></a>Een containerregister maken met de Azure-CLI

Azure Container Registry is een beheerde service voor Docker-containerregisters die wordt gebruikt voor het opslaan van installatiekopieën van persoonlijke Docker-containers. Deze handleiding gegevens maken van een Azure Container register-exemplaar met de Azure CLI.

Deze snelstartgids vereist dat u de Azure CLI versie 2.0.21 zijn uitgevoerd of hoger. Voer `az --version` uit om de versie te bekijken. Als u wilt installeren of upgraden, Zie [2.0 voor Azure CLI installeren][azure-cli].

U moet ook Docker lokaal geïnstalleerd hebben. Docker biedt pakketten die eenvoudig Docker op elke configureren [Mac][docker-mac], [Windows][docker-windows], of [Linux] [ docker-linux] system.

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Een resourcegroep maken met de opdracht [az group create][az-group-create]. Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd.

In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *VS Oost*.

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-container-registry"></a>Een containerregister maken

In deze snelstartgids maken we een *Basic* register. Azure Container register is beschikbaar in diverse verschillende SKU's, een korte beschrijving van de volgende tabel. Zie voor uitgebreide informatie over elk [Container register SKU's][container-registry-skus].

[!INCLUDE [container-registry-sku-matrix](../../includes/container-registry-sku-matrix.md)]

Maak een ACR-exemplaar met de [az acr maken] [ az-acr-create] opdracht.

De naam van het register **moeten uniek zijn**. In het volgende voorbeeld *myContainerRegistry007* wordt gebruikt. Hiermee worden bijgewerkt naar een unieke waarde.

```azurecli
az acr create --resource-group myResourceGroup --name myContainerRegistry007 --sku Basic
```

Wanneer het register is gemaakt, is de uitvoer vergelijkbaar met het volgende:

```json
{
  "adminUserEnabled": false,
  "creationDate": "2017-09-08T22:32:13.175925+00:00",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/myContainerRegistry007",
  "location": "eastus",
  "loginServer": "myContainerRegistry007.azurecr.io",
  "name": "myContainerRegistry007",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "sku": {
    "name": "Basic",
    "tier": "Basic"
  },
  "status": null,
  "storageAccount": null,
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}
```

In de rest van deze snelstartgids, gebruiken we `<acrName>` als een tijdelijke aanduiding voor de naam van de container-register.

## <a name="log-in-to-acr"></a>Aanmelden bij ACR

Voordat u installatiekopieën van containers gaat pushen en pullen, moet u zich aanmelden bij het ACR-exemplaar. Gebruik hiervoor de [az acr aanmelding] [ az-acr-login] opdracht.

```azurecli
az acr login --name <acrName>
```

De opdracht retourneert een `Login Succeeded` bericht eenmaal is voltooid.

## <a name="push-image-to-acr"></a>Push-installatiekopie naar ACR

Voor het pushen van een afbeelding met een Azure-Container register, moet u eerst een afbeelding hebben. Als u alle installatiekopieën lokale container nog niet hebt, voer de volgende opdracht voor het ophalen van een bestaande installatiekopie van Docker-Hub.

```bash
docker pull microsoft/aci-helloworld
```

Voordat u kunt een installatiekopie van een push-toe aan het register, moet u het labelen met de volledig gekwalificeerde naam van uw ACR login-server. Voer de volgende opdracht om op te halen van de naam van de server volledig aanmelding van de ACR-exemplaar.

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

Label van de installatiekopie met de [docker-tag] [ docker-tag] opdracht. Vervang `<acrLoginServer>` met de aanmeldingsnaam van de server van uw ACR-exemplaar.

```bash
docker tag microsoft/aci-helloworld <acrLoginServer>/aci-helloworld:v1
```

Gebruik tot slot [docker push] [ docker-push] voor de push-installatiekopie van het naar het ACR-exemplaar. Vervang `<acrLoginServer>` met de aanmeldingsnaam van de server van uw ACR-exemplaar.

```bash
docker push <acrLoginServer>/aci-helloworld:v1
```

## <a name="list-container-images"></a>Containerinstallatiekopieën opvragen

Het volgende voorbeeld worden de opslagplaatsen in een register:

```azurecli
az acr repository list --name <acrName> --output table
```

Uitvoer:

```bash
Result
----------------
aci-helloworld
```

Het volgende voorbeeld bevat de labels van de **aci helloworld** opslagplaats.

```azurecli
az acr repository show-tags --name <acrName> --repository aci-helloworld --output table
```

Uitvoer:

```bash
Result
--------
v1
```

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer deze niet langer nodig is, kunt u de [az groep verwijderen] [ az-group-delete] opdracht om te verwijderen van de resourcegroep, ACR-exemplaar en alle installatiekopieën van de container.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids kunt u een Azure Container Registry gemaakt met de Azure CLI. Als u gebruiken van Azure Container register met exemplaren van Azure-Container wilt, blijven de zelfstudie exemplaren van Azure-Container.

> [!div class="nextstepaction"]
> [Zelfstudie voor Azure Containerexemplaren][container-instances-tutorial-prepare-app]

<!-- LINKS - external -->
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-login]: https://docs.docker.com/engine/reference/commandline/login/
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[docker-windows]: https://docs.docker.com/docker-for-windows/

<!-- LINKS - internal -->
[az-acr-create]: /cli/azure/acr#az_acr_create
[az-acr-login]: /cli/azure/acr#az_acr_login
[az-group-create]: /cli/azure/group#az_group_create
[az-group-delete]: /cli/azure/group#az_group_delete
[azure-cli]: /cli/azure/install-azure-cli
[container-instances-tutorial-prepare-app]: ../container-instances/container-instances-tutorial-prepare-app.md
[container-registry-skus]: container-registry-skus.md