---
title: Zelfstudie - een groep met meerdere containers in Azure Container Instances - YAML implementeren
description: In deze zelfstudie leert u hoe u de containergroep van een met meerdere containers in Azure Container Instances implementeren met behulp van een YAML-bestand met de Azure CLI.
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: article
ms.date: 04/03/2019
ms.author: danlep
ms.openlocfilehash: a0a91ece4f219cf822673cd457c064c326b89478
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/18/2019
ms.locfileid: "59006183"
---
# <a name="tutorial-deploy-a-multi-container-group-using-a-yaml-file"></a>Zelfstudie: Een groep met meerdere containers met behulp van een YAML-bestand implementeren

> [!div class="op_single_selector"]
> * [YAML](container-instances-multi-container-yaml.md)
> * [Resource Manager](container-instances-multi-container-group.md)
>

Azure Container Instances biedt ondersteuning voor de implementatie van meerdere containers op één host met behulp van een [containergroep](container-instances-container-groups.md). Een containergroep is handig bij het bouwen van een toepassing sidecar voor logboekregistratie, bewaking of een andere configuratie waarbij een tweede gekoppelde proces in een service nodig heeft.

In deze zelfstudie hebt volgen u stappen voor het uitvoeren van een eenvoudige twee-container sidecar-configuratie door het implementeren van een YAML-bestand met de Azure CLI. Een YAML-bestand bevat een beknopt indeling voor het opgeven van de instellingen voor het exemplaar. In deze zelfstudie leert u procedures om het volgende te doen:

> [!div class="checklist"]
> * Configureren van een YAML-bestand
> * De containergroep implementeren
> * Bekijk de logboeken van de containers

> [!NOTE]
> Groepen met meerdere containers zijn momenteel beperkt tot Linux-containers.

Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="configure-a-yaml-file"></a>Configureren van een YAML-bestand

Implementatie van een groep met meerdere containers met de [az container maken] [ az-container-create] opdracht in de Azure CLI, moet u de configuratie van de container in een YAML-bestand. Het YAML-bestand vervolgens als een parameter doorgeven aan de opdracht.

Begin met de volgende YAML kopiëren naar een nieuw bestand met de naam **implementeren aci.yaml**. In Azure Cloud Shell, kunt u Visual Studio Code om te maken van het bestand in uw werkmap:

```
code deploy-aci.yaml
```

Deze YAML-bestand definieert een containergroep met de naam 'myContainerGroup' met twee containers, een openbaar IP-adres en twee blootgestelde poorten. De containers worden geïmplementeerd vanaf de openbare Microsoft-installatiekopieën. De eerste container in de groep wordt uitgevoerd een internetgerichte-webtoepassing. De tweede de sidecar-container maakt periodiek HTTP-aanvragen voor de web-App uitgevoerd in de eerste container via het lokale netwerk van de containergroep.

```YAML
apiVersion: 2018-10-01
location: eastus
name: myContainerGroup
properties:
  containers:
  - name: aci-tutorial-app
    properties:
      image: mcr.microsoft.com/azuredocs/aci-helloworld:latest
      resources:
        requests:
          cpu: 1
          memoryInGb: 1.5
      ports:
      - port: 80
      - port: 8080
  - name: aci-tutorial-sidecar
    properties:
      image: mcr.microsoft.com/azuredocs/aci-tutorial-sidecar
      resources:
        requests:
          cpu: 1
          memoryInGb: 1.5
  osType: Linux
  ipAddress:
    type: Public
    ports:
    - protocol: tcp
      port: '80'
    - protocol: tcp
      port: '8080'
tags: null
type: Microsoft.ContainerInstance/containerGroups
```

Voor het gebruik van het installatiekopie van een privécontainerregister toevoegen de `imageRegistryCredentials` eigenschap voor de containergroep, met waarden die zijn gewijzigd voor uw omgeving:

```YAML
  imageRegistryCredentials:
  - server: imageRegistryLoginServer
    username: imageRegistryUsername
    password: imageRegistryPassword
```

## <a name="deploy-the-container-group"></a>De containergroep implementeren

Maak een resourcegroep met de [az-groep maken] [ az-group-create] opdracht:

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

Implementeren van de containergroep met de [az container maken] [ az-container-create] opdracht, de YAML-bestand als een argument doorgegeven:

```azurecli-interactive
az container create --resource-group myResourceGroup --file deploy-aci.yaml
```

U ontvangt binnen enkele seconden een eerste reactie van Azure.

## <a name="view-deployment-state"></a>Status van de implementatie weergeven

Als u wilt de status van de implementatie weergeven, gebruikt u de volgende [az container show] [ az-container-show] opdracht:

```azurecli-interactive
az container show --resource-group myResourceGroup --name myContainerGroup --output table
```

Als u de actieve App weergeven wilt, gaat u naar het IP-adres in uw browser. Het IP-adres is bijvoorbeeld `52.168.26.124` in de voorbeelduitvoer:

```bash
Name              ResourceGroup    Status    Image                                                                                               IP:ports              Network    CPU/Memory       OsType    Location
----------------  ---------------  --------  --------------------------------------------------------------------------------------------------  --------------------  ---------  ---------------  --------  ----------
myContainerGroup  danlep0318r      Running   mcr.microsoft.com/azuredocs/aci-tutorial-sidecar,mcr.microsoft.com/azuredocs/aci-helloworld:latest  20.42.26.114:80,8080  Public     1.0 core/1.5 gb  Linux     eastus
```

## <a name="view-container-logs"></a>Containerlogboeken ophalen

Weergave van de logboekuitvoer van een container met de [az container logs] [ az-container-logs] opdracht. De `--container-name` argument geeft u de container waarin om op te halen van Logboeken. In dit voorbeeld wordt de `aci-tutorial-app` container is opgegeven.

```azurecli-interactive
az container logs --resource-group myResourceGroup --name myContainerGroup --container-name aci-tutorial-app
```

Uitvoer:

```console
listening on port 80
::1 - - [21/Mar/2019:23:17:48 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
::1 - - [21/Mar/2019:23:17:51 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
::1 - - [21/Mar/2019:23:17:54 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
```

Als u wilt zien van de logboeken voor het sidecar-container, voeren een vergelijkbare opdracht op te geven de `aci-tutorial-sidecar` container.

```azurecli-interactive
az container logs --resource-group myResourceGroup --name myContainerGroup --container-name aci-tutorial-sidecar
```

Uitvoer:

```console
Every 3s: curl -I http://localhost                          2019-03-21 20:36:41

  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0  1663    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0
HTTP/1.1 200 OK
X-Powered-By: Express
Accept-Ranges: bytes
Cache-Control: public, max-age=0
Last-Modified: Wed, 29 Nov 2017 06:40:40 GMT
ETag: W/"67f-16006818640"
Content-Type: text/html; charset=UTF-8
Content-Length: 1663
Date: Thu, 21 Mar 2019 20:36:41 GMT
Connection: keep-alive
```

Zoals u ziet, is de sidecar regelmatig maken van een HTTP-aanvraag naar de belangrijkste webtoepassing via een van de groep lokale netwerk om ervoor te zorgen dat deze wordt uitgevoerd. In dit voorbeeld sidecar kan worden uitgebreid voor het activeren van een waarschuwing als er een HTTP-responscode dan ontvangen `200 OK`.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie gebruikt u een YAML-bestand voor het implementeren van een groep met meerdere containers in Azure Container Instances. U hebt geleerd hoe u:

> [!div class="checklist"]
> * Een YAML-bestand voor een groep met meerdere containers configureren
> * De containergroep implementeren
> * Bekijk de logboeken van de containers

U kunt ook opgeven dat een groep met meerdere containers met een [Resource Manager-sjabloon](container-instances-multi-container-group.md). Resource Manager-sjabloon kan gemakkelijk worden aangepast voor scenario's wanneer u nodig hebt om extra Azure-service-resources met de containergroep te implementeren.

<!-- LINKS - External -->


<!-- LINKS - Internal -->
[aci-tutorial]: ./container-instances-tutorial-prepare-app.md
[az-container-create]: /cli/azure/container#az-container-create
[az-container-logs]: /cli/azure/container#az-container-logs
[az-container-show]: /cli/azure/container#az-container-show
[az-group-create]: /cli/azure/group#az-group-create
[az-group-deployment-create]: /cli/azure/group/deployment#az-group-deployment-create
[template-reference]: https://docs.microsoft.com/azure/templates/microsoft.containerinstance/containergroups
