---
title: 'Snelstartgids: een persoonlijk Docker-register in Azure maken met Azure CLI'
description: Leer snel hoe u een persoonlijk Docker-containerregister maakt met behulp van Azure CLI.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: quickstart
ms.date: 03/03/2018
ms.author: danlep
ms.custom: H1Hack27Feb2017, mvc
ms.openlocfilehash: 57c72056b669865278fa8109cd7f4963a1f0887a
ms.sourcegitcommit: 67abaa44871ab98770b22b29d899ff2f396bdae3
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/08/2018
ms.locfileid: "48855235"
---
# <a name="quickstart-create-a-container-registry-using-the-azure-cli"></a>Snelstart: een containerregister maken met de Azure CLI

Azure Container Registry is een beheerde service voor Docker-containerregisters die wordt gebruikt voor het opslaan van installatiekopieën van persoonlijke Docker-containers. In deze snelstartgids gaat u een exemplaar van Azure Container Registry maken met behulp van de Azure CLI, een containerinstallatiekopie naar het register pushen en ten slotte de container vanuit het register in Azure Container Instances (ACI) implementeren.

Voor deze snelstart moet u versie 2.0.27 of hoger van Azure CLI uitvoeren. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren][azure-cli].

Docker moet ook lokaal zijn geïnstalleerd. Docker biedt pakketten die eenvoudig Docker configureren op elk [Mac][docker-mac]-, [Windows][docker-windows]- of [Linux][docker-linux]-systeem.

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Een resourcegroep maken met de opdracht [az group create][az-group-create]. Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd.

In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *VS - oost*.

```azurecli
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-container-registry"></a>Een containerregister maken

In deze snelstart gaat u een *Basic*-register maken. Azure Container Registry is beschikbaar in verschillende SKU's, zoals kort beschreven in de onderstaande tabel. Zie [SKU's voor containerregisters][container-registry-skus] voor uitgebreide details.

[!INCLUDE [container-registry-sku-matrix](../../includes/container-registry-sku-matrix.md)]

Maak een ACR-exemplaar met behulp van de opdracht [az acr create][az-acr-create]. De registernaam moet uniek zijn binnen Azure en mag 5 tot 50 alfanumerieke tekens bevatten. In het volgende voorbeeld wordt *myContainerRegistry007* gebruikt. Werk deze waarde bij naar een unieke waarde.

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

In de rest van deze snelstart wordt `<acrName>` gebruikt als tijdelijke aanduiding voor de naam van het containerregister.

## <a name="log-in-to-acr"></a>Aanmelden bij ACR

Voordat u installatiekopieën van containers gaat pushen en pullen, moet u zich aanmelden bij het ACR-exemplaar. Gebruik hiervoor de opdracht [az acr login][az-acr-login].

```azurecli
az acr login --name <acrName>
```

Met de opdracht wordt een `Login Succeeded`-bericht geretourneerd nadat deze is voltooid.

## <a name="push-image-to-acr"></a>Installatiekopie naar ACR overdragen met een push-bewerking

Als u een installatiekopie naar een Azure Container Registry wilt pushen, moet u eerst over een installatiekopie beschikken. Als u nog geen lokale containerinstallatiekopieën hebt, voert u de volgende opdracht uit om een bestaande installatiekopie op te halen uit Docker Hub.

```bash
docker pull microsoft/aci-helloworld
```

Voordat u een installatiekopie naar het register kunt pushen, moet u deze taggen met de volledige gekwalificeerde naam van de ACR-aanmeldingsserver. Voer de volgende opdracht uit om de volledig naam van de aanmeldingsserver van het ACR-exemplaar op te halen.

```azurecli
az acr list --resource-group myResourceGroup --query "[].{acrLoginServer:loginServer}" --output table
```

Label de installatiekopie met de opdracht [docker tag][docker-tag]. Vervang `<acrLoginServer>` door de aanmeldingsnaam van het ACR-exemplaar.

```bash
docker tag microsoft/aci-helloworld <acrLoginServer>/aci-helloworld:v1
```

Gebruik ten slotte [docker push][docker-push] om de installatiekopie naar de ACR-instantie te pushen. Vervang `<acrLoginServer>` door de aanmeldingsnaam van het ACR-exemplaar.

```bash
docker push <acrLoginServer>/aci-helloworld:v1
```

## <a name="list-container-images"></a>Containerinstallatiekopieën opvragen

In het volgende voorbeeld worden de opslagplaatsen in een register vermeld:

```azurecli
az acr repository list --name <acrName> --output table
```

Uitvoer:

```bash
Result
----------------
aci-helloworld
```

In het volgende voorbeeld worden de tags in de **aci-helloworld**-opslagplaats vermeld.

```azurecli
az acr repository show-tags --name <acrName> --repository aci-helloworld --output table
```

Uitvoer:

```bash
Result
--------
v1
```

## <a name="deploy-image-to-aci"></a>Installatiekopie implementeren naar ACI

Als u een containerexemplaar wilt implementeren vanuit het register dat u hebt gemaakt, moet u de referenties voor het register opgeven wanneer u het register implementeert. In productiescenario's moet u een [service-principal][container-registry-auth-aci] gebruiken voor toegang tot het register van de container, maar om deze snelstart niet onnodig lang te maken, gebruikt u de volgende opdracht om de gebruiker met beheerdersrechten in te schakelen voor het register:

```azurecli
az acr update --name <acrName> --admin-enabled true
```

Zodra de beheerder is ingeschakeld, is de gebruikersnaam hetzelfde als de registernaam en kunt u het wachtwoord ophalen met deze opdracht:

```azurecli
az acr credential show --name <acrName> --query "passwords[0].value"
```

Voer de volgende opdracht uit om de containerinstallatiekopie te implementeren met 1 processorkern en 1 GB geheugen. Vervang `<acrName>`, `<acrLoginServer>` en `<acrPassword>` door de waarden die u hebt opgehaald met de vorige opdrachten.

```azurecli
az container create --resource-group myResourceGroup --name acr-quickstart --image <acrLoginServer>/aci-helloworld:v1 --cpu 1 --memory 1 --registry-username <acrName> --registry-password <acrPassword> --dns-name-label aci-demo --ports 80
```

Als het goed is, krijgt u een eerste reactie van Azure Resource Manager met details van de container. Als u de status van de container wilt volgen om te zien wanneer deze wordt uitgevoerd, herhaalt u de opdracht [az container show][az-container-show]. Dit duurt normaal gesproken minder dan een minuut.

```azurecli
az container show --resource-group myResourceGroup --name acr-quickstart --query instanceView.state
```

## <a name="view-the-application"></a>De toepassing weergeven

Wanneer de implementatie naar ACI is gelukt, haalt u de FQDN van de container op met de opdracht [az container show][az-container-show]:

```azurecli
az container show --resource-group myResourceGroup --name acr-quickstart --query ipAddress.fqdn
```

Voorbeelduitvoer: `"aci-demo.eastus.azurecontainer.io"`

Als u de actieve toepassing wilt bekijken, navigeert u in uw browser naar het openbare IP-adres.

![Hello world-app in browser][aci-app-browser]

## <a name="clean-up-resources"></a>Resources opschonen

U kunt de opdracht [az group delete][az-group-delete] gebruiken om de resourcegroep, het ACR-exemplaar en alle containerinstallatiekopieën te verwijderen, wanneer u ze niet meer nodig hebt.

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

In deze snelstart hebt u een Azure-containerregister gemaakt met de Azure CLI, een containerinstallatiekopie naar het register gepusht en een exemplaar van het register gestart via Azure Container Instances. Ga verder met de zelfstudie voor Azure Container Instances om meer te leren over ACI.

> [!div class="nextstepaction"]
> [Zelfstudie voor Azure Container Instances][container-instances-tutorial-prepare-app]

<!-- IMAGES> -->
[aci-app-browser]: ../container-instances/media/container-instances-quickstart/aci-app-browser.png


<!-- LINKS - external -->
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-login]: https://docs.docker.com/engine/reference/commandline/login/
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[docker-windows]: https://docs.docker.com/docker-for-windows/

<!-- LINKS - internal -->
[az-acr-create]: /cli/azure/acr#az-acr-create
[az-acr-login]: /cli/azure/acr#az-acr-login
[az-group-create]: /cli/azure/group#az-group-create
[az-group-delete]: /cli/azure/group#az-group-delete
[azure-cli]: /cli/azure/install-azure-cli
[az-container-show]: /cli/azure/container#az-container-show
[container-instances-tutorial-prepare-app]: ../container-instances/container-instances-tutorial-prepare-app.md
[container-registry-skus]: container-registry-skus.md
[container-registry-auth-aci]: container-registry-auth-aci.md
