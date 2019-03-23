---
title: Groepen met meerdere containers in Azure Container Instances met Azure CLI en YAML implementeren
description: Informatie over het implementeren van een containergroep met meerdere containers in Azure Container Instances met behulp van de Azure CLI en een YAML-bestand.
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: article
ms.date: 03/21/2019
ms.author: danlep
ms.openlocfilehash: 10f2340bd85da3dabcd50d51a4dd56d58d31675b
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/22/2019
ms.locfileid: "58372434"
---
# <a name="deploy-a-multi-container-container-group-with-yaml"></a>Een multi-containertoepassingen-containergroep met YAML implementeren

Azure Container Instances biedt ondersteuning voor de implementatie van meerdere containers op één host met behulp van een [containergroep](container-instances-container-groups.md). Groepen met meerdere containers containers zijn nuttig bij het bouwen van een toepassing sidecar voor logboekregistratie, bewaking of een andere configuratie waarbij een tweede gekoppelde proces in een service nodig heeft.

Er zijn twee methoden voor het implementeren van meerdere containergroepen met de Azure CLI:

* YAML-bestand implementeren (in dit artikel)
* [Sjabloonimplementatie van Resource Manager](container-instances-multi-container-group.md)

Vanwege de YAML-indeling meer beknopte aard, implementatie met een YAML-bestand wordt aanbevolen wanneer uw implementatie bevat *alleen* containerinstanties. Als u nodig hebt voor het implementeren van aanvullende Azure-service-resources (bijvoorbeeld een Azure Files-share) op het moment van implementatie van de containers-instantie, wordt sjabloonimplementatie van Resource Manager-aanbevolen.

> [!NOTE]
> Groepen met meerdere containers zijn momenteel beperkt tot Linux-containers. Terwijl er wordt geprobeerd om alle functies op Windows-containers, vindt u de huidige platform verschillen in [quota en beschikbaarheid in regio's voor Azure Container Instances](container-instances-quotas.md).

## <a name="configure-the-yaml-file"></a>Het YAML-bestand configureren

Implementatie van een containergroep met meerdere containers met de [az container maken] [ az-container-create] opdracht in de Azure CLI, moet u de configuratie van de container in een YAML-bestand opgeven en vervolgens geeft het YAML-bestand als een de parameter aan de opdracht.

Begin met de volgende YAML kopiëren naar een nieuw bestand met de naam **implementeren aci.yaml**.

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

## <a name="view-logs"></a>Logboeken weergeven

Weergave van de logboekuitvoer van een container met de [az container logs] [ az-container-logs] opdracht. De `--container-name` argument geeft u de container waarin om op te halen van Logboeken. In dit voorbeeld wordt is de eerste container opgegeven.

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

Als u wilt zien van de logboeken voor de container zijspan, moet u dezelfde opdracht op te geven de naam van het tweede uitvoeren.

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

Zoals u ziet, is de sidecar regelmatig maken van een HTTP-aanvraag naar de belangrijkste webtoepassing via een van de groep lokale netwerk om ervoor te zorgen dat deze wordt uitgevoerd. In dit voorbeeld sidecar kan worden uitgebreid voor het activeren van een waarschuwing als er een HTTP-responscode dan 200 OK ontvangen.

## <a name="deploy-from-private-registry"></a>Implementeren vanuit een persoonlijk register

Als u wilt gebruiken een persoonlijk containerregister-installatiekopie, zijn onder andere de volgende YAML met waarden die zijn gewijzigd voor uw omgeving:

```YAML
  imageRegistryCredentials:
  - server: imageRegistryLoginServer
    username: imageRegistryUsername
    password: imageRegistryPassword
```

De volgende YAML implementeert bijvoorbeeld een containergroep met een enkele container waarvan de installatiekopie wordt opgehaald uit een persoonlijke Azure Container Registry met de naam 'myregistry':

```YAML
apiVersion: 2018-10-01
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

## <a name="export-container-group-to-yaml"></a>Containergroep exporteren naar YAML

U kunt de configuratie van een bestaande containergroep exporteren naar een YAML-bestand met behulp van de Azure CLI-opdracht [az container exporteren][az-container-export].

Handig voor het behouden van de configuratie van de containergroep van een, export kunt u voor het opslaan van de configuratie van uw container in versiebeheer voor 'configuratie als code'. Of het geëxporteerde bestand als uitgangspunt gebruiken bij het ontwikkelen van een nieuwe configuratie in YAML.

Exporteren van de configuratie voor de containergroep die u eerder hebt gemaakt met behulp van de volgende [az container exporteren] [ az-container-export] opdracht:

```azurecli-interactive
az container export --resource-group myResourceGroup --name myContainerGroup --file deployed-aci.yaml
```

Er is geen uitvoer wordt weergegeven als de opdracht geslaagd is, maar u kunt de inhoud van het bestand om te zien van het resultaat bekijken. Bijvoorbeeld, de eerste paar regels met `head`:

```console
$ head deployed-aci.yaml
additional_properties: {}
apiVersion: '2018-06-01'
location: eastus
name: myContainerGroup
properties:
  containers:
  - name: aci-tutorial-app
    properties:
      environmentVariables: []
      image: mcr.microsoft.com/azuredocs/aci-helloworld:latest
```

## <a name="next-steps"></a>Volgende stappen

Dit artikel hebben we de stappen die nodig zijn voor het implementeren van een instantie meerdere containers, Azure container beschreven. Zie de zelfstudie Azure Container Instances voor een Azure Container Instances ervaring met end-to-end, inclusief het gebruik van een persoonlijke Azure container registry.

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
