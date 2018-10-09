---
title: Koppelen van een geheime volume in Azure Container Instances
description: Meer informatie over het koppelen van een geheime volume voor het opslaan van gevoelige gegevens voor toegang door containerexemplaren
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: article
ms.date: 07/19/2018
ms.author: danlep
ms.openlocfilehash: 9aa80cf3cb02237cea11e370151eda8c67c7b10e
ms.sourcegitcommit: 67abaa44871ab98770b22b29d899ff2f396bdae3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/08/2018
ms.locfileid: "48856748"
---
# <a name="mount-a-secret-volume-in-azure-container-instances"></a>Koppelen van een geheime volume in Azure Container Instances

Gebruik een *geheim* volume gevoelige informatie aan de containers in een containergroep opgeven. De *geheim* volume uw geheimen worden opgeslagen in bestanden in het volume toegankelijk is voor de containers in de containergroep. Doordat geheimen in een *geheim* volume, kunt u voorkomen dat gevoelige gegevens, zoals SSH-sleutels of databasereferenties toevoegen aan uw toepassingscode.

Alle *geheim* volumes worden ondersteund door [tmpfs][tmpfs], een bestandssysteem RAM-geheugen ondersteund; de inhoud ervan worden nooit naar niet-vluchtige opslag geschreven.

> [!NOTE]
> *Geheim* volumes zijn momenteel beperkt tot Linux-containers. Meer informatie over het doorgeven van beveiligde omgevingsvariabelen voor zowel Windows als Linux-containers in [omgevingsvariabelen instellen](container-instances-environment-variables.md). Terwijl er wordt geprobeerd om alle functies op Windows-containers, vindt u de huidige platform verschillen in [quota en beschikbaarheid in regio's voor Azure Container Instances](container-instances-quotas.md).

## <a name="mount-secret-volume---azure-cli"></a>Geheime volume - Azure CLI te koppelen

Voor het implementeren van een container met een of meer geheimen met behulp van de Azure CLI, bevatten de `--secrets` en `--secrets-mount-path` parameters in de [az container maken] [ az-container-create] opdracht. In dit voorbeeld koppelt een *geheim* volume dat bestaat uit twee geheimen, door 'mysecret1' en 'mysecret2,' op `/mnt/secrets`:

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name secret-volume-demo \
    --image microsoft/aci-helloworld \
    --secrets mysecret1="My first secret FOO" mysecret2="My second secret BAR" \
    --secrets-mount-path /mnt/secrets
```

De volgende [az container exec] [ az-container-exec] uitvoer ziet u een shell openen in de container die wordt uitgevoerd, de bestanden in het geheime volume aanbieding en vervolgens hun inhoud weergeven:

```console
$ az container exec --resource-group myResourceGroup --name secret-volume-demo --exec-command "/bin/sh"
/usr/src/app # ls -1 /mnt/secrets
mysecret1
mysecret2
/usr/src/app # cat /mnt/secrets/mysecret1
My first secret FOO
/usr/src/app # cat /mnt/secrets/mysecret2
My second secret BAR
/usr/src/app # exit
Bye.
```

## <a name="mount-secret-volume---yaml"></a>Geheime volume - YAML koppelen

U kunt ook implementeren met containergroepen met de Azure CLI en een [YAML-sjabloon](container-instances-multi-container-yaml.md). Implementeren met YAML-sjabloon is de aanbevolen methode bij het implementeren van groepen met containers die bestaat uit meerdere containers.

Wanneer u met een YAML-sjabloon implementeert, moet de geheime waarden zijn **Base64-gecodeerd** in de sjabloon. Echter, de geheime waarden worden weergegeven als tekst zonder opmaak binnen de bestanden in de container.

De volgende YAML-sjabloon definieert een containergroep met een container die koppelt een *geheim* volume aan `/mnt/secrets`. Het geheime volume heeft twee geheimen, "mysecret1" en "mysecret2."

```yaml
apiVersion: '2018-06-01'
location: eastus
name: secret-volume-demo
properties:
  containers:
  - name: aci-tutorial-app
    properties:
      environmentVariables: []
      image: microsoft/aci-helloworld:latest
      ports: []
      resources:
        requests:
          cpu: 1.0
          memoryInGB: 1.5
      volumeMounts:
      - mountPath: /mnt/secrets
        name: secretvolume1
  osType: Linux
  restartPolicy: Always
  volumes:
  - name: secretvolume1
    secret:
      mysecret1: TXkgZmlyc3Qgc2VjcmV0IEZPTwo=
      mysecret2: TXkgc2Vjb25kIHNlY3JldCBCQVIK
tags: {}
type: Microsoft.ContainerInstance/containerGroups
```

Opslaan als wilt implementeren met behulp van de sjabloon YAML, de voorgaande YAML naar een bestand met de naam `deploy-aci.yaml`, voer de [az container maken] [ az-container-create] opdracht met de `--file` parameter:

```azurecli-interactive
# Deploy with YAML template
az container create --resource-group myResourceGroup --file deploy-aci.yaml
```

## <a name="mount-secret-volume---resource-manager"></a>Geheime volume - Resource Manager te koppelen

Naast de CLI en YAML-implementatie, kunt u de containergroep van een met behulp van een Azure implementeren [Resource Manager-sjabloon](/azure/templates/microsoft.containerinstance/containergroups).

Eerst, vullen het `volumes` -matrix in de containergroep `properties` gedeelte van de sjabloon. Wanneer u met een Resource Manager-sjabloon implementeert, moet de geheime waarden zijn **Base64-gecodeerd** in de sjabloon. Echter, de geheime waarden worden weergegeven als tekst zonder opmaak binnen de bestanden in de container.

Vervolgens voor elke container in containergroep waarin u wilt koppelen de *geheim* volume, vullen het `volumeMounts` matrix in de `properties` sectie van de containerdefinitie van de.

De volgende Resource Manager-sjabloon definieert een containergroep met een container die koppelt een *geheim* volume aan `/mnt/secrets`. Het geheime volume heeft twee geheimen, "mysecret1" en "mysecret2."

<!-- https://github.com/Azure/azure-docs-json-samples/blob/master/container-instances/aci-deploy-volume-secret.json --> [!code-json[volume-secret](~/azure-docs-json-samples/container-instances/aci-deploy-volume-secret.json)]

Opslaan als wilt implementeren met behulp van de Resource Manager-sjabloon, de eerder vermelde JSON naar een bestand met de naam `deploy-aci.json`, voer de [az group deployment maken] [ az-group-deployment-create] opdracht met de `--template-file` parameter:

```azurecli-interactive
# Deploy with Resource Manager template
az group deployment create --resource-group myResourceGroup --template-file deploy-aci.json
```

## <a name="next-steps"></a>Volgende stappen

### <a name="volumes"></a>Volumes

Informatie over het koppelen van andere typen in Azure Container Instances:

* [Een Azure-bestandsshare in Azure Containerexemplaren koppelen](container-instances-volume-azure-files.md)
* [Een volume emptyDir in Azure Containerexemplaren koppelen](container-instances-volume-emptydir.md)
* [Koppelen van een volume gitRepo in Azure Containerexemplaren](container-instances-volume-gitrepo.md)

### <a name="secure-environment-variables"></a>Variabelen voor de veilige omgeving

Een andere methode voor het leveren van gevoelige informatie aan containers (met inbegrip van Windows-containers), is via het gebruik van [secure omgevingsvariabelen](container-instances-environment-variables.md#secure-values).

<!-- LINKS - External -->
[tmpfs]: https://wikipedia.org/wiki/Tmpfs

<!-- LINKS - Internal -->
[az-container-create]: /cli/azure/container#az-container-create
[az-container-exec]: /cli/azure/container#az-container-exec
[az-group-deployment-create]: /cli/azure/group/deployment#az-group-deployment-create
