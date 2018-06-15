---
title: Een volume gitRepo Azure Containerexemplaren koppelen
description: Meer informatie over het koppelen van een volume gitRepo een Git-opslagplaats in uw containerexemplaren klonen
services: container-instances
author: mmacy
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 04/16/2018
ms.author: marsma
ms.openlocfilehash: e40d841c07534c9c0074c038d1e3c6e435265564
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
ms.locfileid: "32166776"
---
# <a name="mount-a-gitrepo-volume-in-azure-container-instances"></a>Koppelen van een volume gitRepo in Azure Containerexemplaren

Meer informatie over het koppelen van een *gitRepo* volume voor het klonen van een Git-opslagplaats in uw containerexemplaren.

> [!NOTE]
> Koppelen van een *gitRepo* volume is momenteel beperkt tot Linux containers. Hoewel we ons best doen om alle functies beschikbaar te maken voor Windows-containers, kunnen de [quota en beschikbaarheid in regio´s voor Azure Container Instances](container-instances-quotas.md) variëren op de verschillende platforms.

## <a name="gitrepo-volume"></a>gitRepo volume

De *gitRepo* volume koppelt u een map en de opgegeven Git-opslagplaats kloont erin tijdens het opstarten van de container. Met behulp van een *gitRepo* volume in de containerexemplaren, kunt u voorkomen dat de code hiervoor in uw toepassingen toe te voegen.

Wanneer u koppelt een *gitRepo* volume, kunt u drie eigenschappen voor het configureren van het volume instellen:

| Eigenschap | Vereist | Beschrijving |
| -------- | -------- | ----------- |
| `repository` | Ja | De volledige URL, met inbegrip van `http://` of `https://`, van de Git-opslagplaats te worden gekloond.|
| `directory` | Nee | De map waarin de opslagplaats moet worden gekloond. Het pad mag niet bevatten of beginnen met '`..`'.  Als u opgeeft '`.`', de opslagplaats is gekloond in de directory van het volume. Anders wordt is de Git-opslagplaats gekloond in een submap van de opgegeven naam binnen de volume-directory. |
| `revision` | Nee | De commit-hash van de revisie kunnen worden gekloond. Als u niets opgeeft, de `HEAD` revisie wordt gekloond. |

## <a name="mount-gitrepo-volume-azure-cli"></a>Koppelpunt gitRepo volume: Azure CLI

Een volume te koppelen gitRepo bij het implementeren van containerexemplaren met de [Azure CLI](/cli/azure), supply de `--gitrepo-url` en `--gitrepo-mount-path` parameters voor de [az container maken] [ az-container-create] opdracht. Indien gewenst kunt u de map op het volume klonen in opgeven (`--gitrepo-dir`) en de commit-hash van de revisie kunnen worden gekloond (`--gitrepo-revision`).

Met deze voorbeeldopdracht kloont de [aci helloworld] [ aci-helloworld] voorbeeldtoepassing in `/mnt/aci-helloworld` in de container-exemplaar:

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name hellogitrepo \
    --image microsoft/aci-helloworld \
    --dns-name-label aci-demo \
    --ports 80 \
    --gitrepo-url https://github.com/Azure-Samples/aci-helloworld \
    --gitrepo-mount-path /mnt/aci-helloworld
```

Om te controleren of het volume gitRepo is gekoppeld, start u een shell in de container met [az container exec] [ az-container-exec] en de map weergeven:

```console
$ az container exec --resource-group myResourceGroup --name hellogitrepo --exec-command /bin/sh
/usr/src/app # ls -l /mnt/aci-helloworld/
total 16
-rw-r--r--    1 root     root           144 Apr 16 16:35 Dockerfile
-rw-r--r--    1 root     root          1162 Apr 16 16:35 LICENSE
-rw-r--r--    1 root     root          1237 Apr 16 16:35 README.md
drwxr-xr-x    2 root     root          4096 Apr 16 16:35 app
```

## <a name="mount-gitrepo-volume-resource-manager"></a>Koppelpunt gitRepo volume: Resource Manager

Een volume te koppelen gitRepo bij het implementeren van containerexemplaren met een [Azure Resource Manager-sjabloon](/azure/templates/microsoft.containerinstance/containergroups), eerst vullen de `volumes` matrix in de containergroep `properties` gedeelte van de sjabloon. Klik vervolgens voor elke container in de containergroep waarin u wilt koppelen de *gitRepo* volume, vullen de `volumeMounts` -matrix in de `properties` sectie van de definitie van de container.

De volgende Resource Manager-sjabloon maakt bijvoorbeeld een containergroep die bestaan uit een enkele container. De container kloont twee GitHub-opslagplaatsen opgegeven door de *gitRepo* volumeblokken. Het tweede volume bevat aanvullende eigenschappen opgeven van een map voor het klonen naar en de commit-hash van een specifieke revisie om te klonen.

<!-- https://github.com/Azure/azure-docs-json-samples/blob/master/container-instances/aci-deploy-volume-gitrepo.json -->
[!code-json[volume-gitrepo](~/azure-docs-json-samples/container-instances/aci-deploy-volume-gitrepo.json)]

De resulterende directorystructuur van de twee gekloonde repo's gedefinieerd in de voorgaande sjabloon is:

```
/mnt/repo1/aci-helloworld
/mnt/repo2/my-custom-clone-directory
```

Zie voor een voorbeeld van de implementatie van de container-exemplaar met een Azure Resource Manager-sjabloon [implementeren meerdere container groepen in Azure Containerexemplaren](container-instances-multi-container-group.md).

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het koppelen van andere volumetypen in Azure Containerexemplaren:

* [Een Azure-bestandsshare in Azure Containerexemplaren koppelen](container-instances-volume-azure-files.md)
* [Een volume emptyDir in Azure Containerexemplaren koppelen](container-instances-volume-emptydir.md)
* [Koppelen van een geheime volume in Azure Containerexemplaren](container-instances-volume-secret.md)

<!-- LINKS - External -->
[aci-helloworld]: https://github.com/Azure-Samples/aci-helloworld

<!-- LINKS - Internal -->
[az-container-create]: /cli/azure/container#az-container-create
[az-container-exec]: /cli/azure/container#az-container-exec