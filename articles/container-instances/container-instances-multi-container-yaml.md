---
title: Groepen in Azure Containerexemplaren met Azure CLI en YAML meerdere container implementeren
description: Informatie over het implementeren van een containergroep met meerdere containers in Azure Containerexemplaren met behulp van de Azure CLI en een YAML-bestand.
services: container-instances
author: mmacy
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 06/08/2018
ms.author: marsma
ms.openlocfilehash: 5dfee15e978d2dba0f50d1dc4b78953698389950
ms.sourcegitcommit: 3c3488fb16a3c3287c3e1cd11435174711e92126
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/07/2018
ms.locfileid: "34851275"
---
# <a name="deploy-a-multi-container-container-group-with-yaml"></a>Een containergroep met meerdere container met YAML implementeren

Azure Containerexemplaren ondersteunt de implementatie van meerdere containers naar één host met behulp van een [containergroep](container-instances-container-groups.md). Meerdere container containergroepen kunnen nuttig zijn bij het bouwen van een toepassing ter voor logboekregistratie, bewaken of een andere configuratie waarbij een tweede gekoppelde proces in een service nodig heeft.

Er zijn twee methoden voor het implementeren van meerdere containergroepen met de Azure CLI:

* Implementatie van YAML-bestand (in dit artikel)
* [Sjabloonimplementatie van Resource Manager](container-instances-multi-container-group.md)

Vanwege de YAML-indeling meer beknopte aard, implementatie met een YAML-bestand wordt aanbevolen wanneer uw implementatie bevat *alleen* containerexemplaren. Als u nodig hebt voor het implementeren van extra Azure-service-resources (bijvoorbeeld een share Azure-bestanden) op het moment van implementatie van de container-exemplaar, wordt sjabloonimplementatie van Resource Manager-aanbevolen.

> [!NOTE]
> Meerdere container groepen zijn momenteel beperkt tot Linux containers. Terwijl we proberen om te zorgen dat alle functies op Windows-containers, vindt u de huidige platform verschillen in [quota en beschikbaarheid in regio's voor exemplaren van Azure-Container](container-instances-quotas.md).

## <a name="configure-the-yaml-file"></a>Het bestand YAML configureren

Voor het implementeren van een containergroep met meerdere container met de [az container maken] [ az-container-create] opdracht in de Azure CLI, moet u de configuratie van de container op te geven in een bestand YAML en geeft het bestand YAML als een de parameter voor de opdracht.

Gestart door de volgende YAML kopiëren naar een nieuw bestand met de naam **implementeren aci.yaml**.

Dit bestand YAML definieert een containergroep met twee containers, een openbare IP-adres en twee blootgestelde poorten. De eerste container in de groep een internetgerichte webserver-toepassing wordt uitgevoerd. De tweede container, de ter maakt periodiek HTTP-aanvragen naar de webtoepassing die wordt uitgevoerd in de eerste container via het lokale netwerk van de containergroep.

```YAML
apiVersion: 2018-06-01
location: eastus
name: myContainerGroup
properties:
  containers:
  - name: aci-tutorial-app
    properties:
      image: microsoft/aci-helloworld:latest
      resources:
        requests:
          cpu: 1
          memoryInGb: 1.5
      ports:
      - port: 80
      - port: 8080
  - name: aci-tutorial-sidecar
    properties:
      image: microsoft/aci-tutorial-sidecar
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

## <a name="deploy-the-container-group"></a>De containergroep implementeren

Maak een resourcegroep met de [az groep maken] [ az-group-create] opdracht:

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

Implementeren van de containergroep met de [az container maken] [ az-container-create] het YAML-bestand als een argument wordt doorgegeven, opdracht:

```azurecli-interactive
az container create --resource-group myResourceGroup --name myContainerGroup -f deploy-aci.yaml
```

U ontvangt binnen enkele seconden een eerste reactie van Azure.

## <a name="view-deployment-state"></a>Implementatiestatus weergeven

Als u wilt de status van de implementatie weergeven, gebruikt u de volgende [az container weergeven] [ az-container-show] opdracht:

```azurecli-interactive
az container show --resource-group myResourceGroup --name myContainerGroup --output table
```

Als u de actieve toepassing weergeven wilt, gaat u naar het IP-adres in uw browser. Het IP-adres is bijvoorbeeld `52.168.26.124` in dit voorbeeld van uitvoer:

```bash
Name              ResourceGroup    ProvisioningState    Image                                                           IP:ports               CPU/Memory       OsType    Location
----------------  ---------------  -------------------  --------------------------------------------------------------  ---------------------  ---------------  --------  ----------
myContainerGroup  myResourceGroup  Succeeded            microsoft/aci-helloworld:latest,microsoft/aci-tutorial-sidecar  52.168.26.124:80,8080  1.0 core/1.5 gb  Linux     eastus
```

## <a name="view-logs"></a>Logboeken weergeven

Weergave van de logboekuitvoer van een container met de [az container logboeken] [ az-container-logs] opdracht. De `--container-name` argument Hiermee geeft u de container waarin voor het ophalen van Logboeken. In dit voorbeeld wordt is de eerste container opgegeven.

```azurecli-interactive
az container logs --resource-group myResourceGroup --name myContainerGroup --container-name aci-tutorial-app
```

Uitvoer:

```console
listening on port 80
::1 - - [09/Jan/2018:23:17:48 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
::1 - - [09/Jan/2018:23:17:51 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
::1 - - [09/Jan/2018:23:17:54 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
```

Overzicht van de logboeken voor de container side auto dezelfde opdracht geven de tweede containernaam worden uitgevoerd.

```azurecli-interactive
az container logs --resource-group myResourceGroup --name myContainerGroup --container-name aci-tutorial-sidecar
```

Uitvoer:

```console
Every 3s: curl -I http://localhost                          2018-01-09 23:25:11

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
Date: Tue, 09 Jan 2018 23:25:11 GMT
Connection: keep-alive
```

Zoals u ziet, is de ter periodiek een HTTP-aanvraag die naar de belangrijkste webtoepassing via een van de groep lokale netwerk om ervoor te zorgen dat deze wordt uitgevoerd. In dit voorbeeld ter kan worden uitgebreid activeren van een waarschuwing als er een HTTP-antwoordcode dan 200 OK ontvangen.

## <a name="deploy-from-private-registry"></a>Implementeren vanaf persoonlijke register

Voor het gebruik van een installatiekopie van privé-container register, zijn onder andere de volgende YAML met waarden die zijn gewijzigd voor uw omgeving:

```YAML
  imageRegistryCredentials:
  - server: imageRegistryLoginServer
    username: imageRegistryUsername
    password: imageRegistryPassword
```

De volgende YAML implementeert bijvoorbeeld een containergroep met een enkele container waarvan de installatiekopie is opgehaald uit een persoonlijke Azure-Container register met de naam 'myregistry':

```YAML
apiVersion: 2018-06-01
location: eastus
name: myContainerGroup2
properties:
  containers:
  - name: aci-tutorial-app
    properties:
      image: myregistry.azurecr.io/aci-helloworld:latest
      resources:
        requests:
          cpu: 1
          memoryInGb: 1.5
      ports:
      - port: 80
  osType: Linux
  ipAddress:
    type: Public
    ports:
    - protocol: tcp
      port: '80'
  imageRegistryCredentials:
  - server: myregistry.azurecr.io
    username: myregistry
    password: REGISTRY_PASSWORD
tags: null
type: Microsoft.ContainerInstance/containerGroups
```

## <a name="export-container-group-to-yaml"></a>De containergroep exporteren naar YAML

U kunt de configuratie van een bestaande containergroep exporteren naar een YAML-bestand met de Azure CLI-opdracht [az container export][az-container-export].

Nuttig voor het behouden van de configuratie van de containergroep van een, exporteren kunt u de configuraties van container opslaan in versiebeheer voor '-configuratie als code'. Of het geëxporteerde bestand als uitgangspunt gebruiken bij het ontwikkelen van een nieuwe configuratie in YAML.

De configuratie voor de containergroep die u eerder hebt gemaakt met de volgende exporteren [az container export] [ az-container-export] opdracht:

```azurecli-interactive
az container export --resource-group rg604 --name myContainerGroup --file deployed-aci.yaml
```

Er is geen uitvoer wordt weergegeven als de opdracht geslaagd is, maar u kunt de inhoud van het bestand om te zien van het resultaat weergeven. Bijvoorbeeld de eerste paar regels met `head`:

```console
$ head deployed-aci.yaml
apiVersion: 2018-02-01-preview
location: eastus
name: myContainerGroup
properties:
  containers:
  - name: aci-tutorial-app
    properties:
      environmentVariables: []
      image: microsoft/aci-helloworld:latest
      ports:
```

> [!NOTE]
> Vanaf Azure CLI versie 2.0.34, er is een [bekende probleem] [ cli-issue-6525] in die geëxporteerd containergroepen een oudere versie van de API van opgeven **2018-02-01-preview** (gezien in de vorige JSON-Uitvoervoorbeeld). Als u implementeren met behulp van het geëxporteerde bestand met YAML wilt, kunt u veilig bijwerken de `apiVersion` waarde in het geëxporteerde bestand YAML naar **2018-06-01**.

## <a name="next-steps"></a>Volgende stappen

In dit artikel betrekking op de stappen die nodig zijn voor het implementeren van een exemplaar van de container voor meerdere Azure-container. Zie de zelfstudie exemplaren van Azure-Container voor een end-to-end Azure Containerexemplaren ervaring, inclusief het gebruik van een register persoonlijke Azure-container.

> [!div class="nextstepaction"]
> [Zelfstudie voor Azure Container Instances][aci-tutorial]

<!-- LINKS - External -->
[cli-issue-6525]: https://github.com/Azure/azure-cli/issues/6525

<!-- LINKS - Internal -->
[aci-tutorial]: ./container-instances-tutorial-prepare-app.md
[az-container-create]: /cli/azure/container#az-container-create
[az-container-export]: /cli/azure/container#az-container-export
[az-container-logs]: /cli/azure/container#az-container-logs
[az-container-show]: /cli/azure/container#az-container-show
[az-group-create]: /cli/azure/group#az-group-create
[az-group-deployment-create]: /cli/azure/group/deployment#az-group-deployment-create
[template-reference]: https://docs.microsoft.com/azure/templates/microsoft.containerinstance/containergroups
