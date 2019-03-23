---
title: Groepen met meerdere containers in Azure Container Instances implementeren
description: Informatie over het implementeren van een containergroep met meerdere containers in Azure Container Instances met behulp van een Azure Resource Manager-sjabloon.
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: article
ms.date: 06/08/2018
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: 93f73e133e99025b479d0b38512e26088a8eaefa
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/22/2019
ms.locfileid: "58369107"
---
# <a name="deploy-a-multi-container-group-with-a-resource-manager-template"></a>Een groep met meerdere containers met Resource Manager-sjabloon implementeren

Azure Container Instances biedt ondersteuning voor de implementatie van meerdere containers op één host met behulp van een [containergroep](container-instances-container-groups.md). Dit is handig bij het bouwen van een toepassing sidecar voor logboekregistratie, bewaking of een andere configuratie waarbij een tweede gekoppelde proces in een service nodig heeft.

Er zijn twee methoden voor het implementeren van meerdere containergroepen met de Azure CLI:

* Sjabloonimplementatie van Resource Manager-(in dit artikel)
* [Implementatie van YAML-bestand](container-instances-multi-container-yaml.md)

Implementatie met een Resource Manager-sjabloon wordt aanbevolen wanneer u nodig hebt om extra Azure-service-resources (bijvoorbeeld een Azure Files-share) te implementeren op het moment van implementatie van de containers-instantie. Vanwege de YAML-indeling meer beknopte aard, implementatie met een YAML-bestand wordt aanbevolen wanneer uw implementatie bevat *alleen* containerinstanties.

> [!NOTE]
> Groepen met meerdere containers zijn momenteel beperkt tot Linux-containers. Terwijl we werken om alle functies op Windows-containers, vindt u de huidige platform verschillen in [quota en beschikbaarheid in regio's voor Azure Container Instances](container-instances-quotas.md).

Zie voor aanvullende voorbeeldsjablonen, [Azure Resource Manager-sjablonen voor Azure Container Instances](container-instances-samples-rm.md). 

## <a name="configure-the-template"></a>De sjabloon configureren

De secties in dit artikel begeleidt u bij het uitvoeren van een eenvoudige multi-containertoepassingen sidecar-configuratie door het implementeren van een Azure Resource Manager-sjabloon.

Beginnen met het maken van een bestand met de naam `azuredeploy.json`, kopieer vervolgens de volgende JSON naar het.

Deze Resource Manager-sjabloon definieert een containergroep met twee containers, een openbaar IP-adres en twee blootgestelde poorten. De containers worden geïmplementeerd vanaf de openbare Microsoft-installatiekopieën. De eerste container in de groep wordt uitgevoerd een internetgerichte toepassing. De tweede de sidecar-container maakt een HTTP-aanvraag voor de belangrijkste web-App via een van de groep lokale netwerk.

```JSON
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "containerGroupName": {
      "type": "string",
      "defaultValue": "myContainerGroup",
      "metadata": {
        "description": "Container Group name."
      }
    }
  },
  "variables": {
    "container1name": "aci-tutorial-app",
    "container1image": "mcr.microsoft.com/azuredocs/aci-helloworld:latest",
    "container2name": "aci-tutorial-sidecar",
    "container2image": "mcr.microsoft.com/azuredocs/aci-tutorial-sidecar"
  },
  "resources": [
    {
      "name": "[parameters('containerGroupName')]",
      "type": "Microsoft.ContainerInstance/containerGroups",
      "apiVersion": "2018-10-01",
      "location": "[resourceGroup().location]",
      "properties": {
        "containers": [
          {
            "name": "[variables('container1name')]",
            "properties": {
              "image": "[variables('container1image')]",
              "resources": {
                "requests": {
                  "cpu": 1,
                  "memoryInGb": 1.5
                }
              },
              "ports": [
                {
                  "port": 80
                },
                {
                  "port": 8080
                }
              ]
            }
          },
          {
            "name": "[variables('container2name')]",
            "properties": {
              "image": "[variables('container2image')]",
              "resources": {
                "requests": {
                  "cpu": 1,
                  "memoryInGb": 1.5
                }
              }
            }
          }
        ],
        "osType": "Linux",
        "ipAddress": {
          "type": "Public",
          "ports": [
            {
              "protocol": "tcp",
              "port": "80"
            },
            {
                "protocol": "tcp",
                "port": "8080"
            }
          ]
        }
      }
    }
  ],
  "outputs": {
    "containerIPv4Address": {
      "type": "string",
      "value": "[reference(resourceId('Microsoft.ContainerInstance/containerGroups/', parameters('containerGroupName'))).ipAddress.ip]"
    }
  }
}
```

Als u wilt gebruiken een persoonlijk containerregister-installatiekopie, moet u een object toevoegen aan het JSON-document met de volgende indeling. Zie voor een voorbeeld van de implementatie van deze configuratie de [verwijzing naar ACI Resource Manager-sjabloon] [ template-reference] documentatie.

```JSON
"imageRegistryCredentials": [
  {
    "server": "[parameters('imageRegistryLoginServer')]",
    "username": "[parameters('imageRegistryUsername')]",
    "password": "[parameters('imageRegistryPassword')]"
  }
]
```

## <a name="deploy-the-template"></a>De sjabloon implementeren

Een resourcegroep maken met de opdracht [az group create][az-group-create].

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

Implementeer de sjabloon met de [az group deployment maken] [ az-group-deployment-create] opdracht.

```azurecli-interactive
az group deployment create --resource-group myResourceGroup --template-file azuredeploy.json
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

```bash
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

```bash
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

## <a name="next-steps"></a>Volgende stappen

Dit artikel hebben we de stappen die nodig zijn voor het implementeren van een instantie meerdere containers, Azure container beschreven. Zie de zelfstudie Azure Container Instances voor een end-to-end Azure Container Instances-ervaring.

> [!div class="nextstepaction"]
> [Zelfstudie voor Azure Container Instances][aci-tutorial]

<!-- LINKS - Internal -->
[aci-tutorial]: ./container-instances-tutorial-prepare-app.md
[az-container-logs]: /cli/azure/container#az-container-logs
[az-container-show]: /cli/azure/container#az-container-show
[az-group-create]: /cli/azure/group#az-group-create
[az-group-deployment-create]: /cli/azure/group/deployment#az-group-deployment-create
[template-reference]: https://docs.microsoft.com/azure/templates/microsoft.containerinstance/containergroups
