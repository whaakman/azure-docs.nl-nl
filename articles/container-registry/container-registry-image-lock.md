---
title: Vergrendelen van een installatiekopie in Azure Container Registry
description: De kenmerken voor een container-installatiekopie of een opslagplaats instellen, zodat deze kan niet worden verwijderd of overschreven in een Azure container registry.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: article
ms.date: 02/19/2019
ms.author: danlep
ms.openlocfilehash: ebbfaba158e7ddb669111f097eb1adde2373aa6c
ms.sourcegitcommit: 223604d8b6ef20a8c115ff877981ce22ada6155a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/22/2019
ms.locfileid: "58361282"
---
# <a name="lock-a-container-image-in-an-azure-container-registry"></a>Vergrendelen van een containerinstallatiekopie in een Azure container registry

In een Azure container registry, kunt u de versie van een installatiekopie of een opslagplaats vergrendelen zodat deze kan niet worden verwijderd of bijgewerkt. Als u wilt vergrendelen van een installatiekopie of een opslagplaats, werken de kenmerken ervan met behulp van de Azure CLI-opdracht [az acr repository update][az-acr-repository-update]. 

In dit artikel is vereist dat u de Azure CLI in Azure Cloud Shell uitvoert of lokaal (versie 2.0.55 of hoger aanbevolen). Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren][azure-cli].

## <a name="scenarios"></a>Scenario's

Een gelabelde installatiekopie in Azure Container Registry is standaard *veranderlijke*, zodat met de juiste machtigingen kunt u herhaaldelijk bijwerken en een installatiekopie met dezelfde tag naar een register pushen. Containerinstallatiekopieën kunnen ook worden [verwijderd](container-registry-delete.md) indien nodig. Dit gedrag is handig bij het ontwikkelen van installatiekopieën en moet een grootte voor uw register te onderhouden.

Echter, wanneer u een containerinstallatiekopie naar productie implementeert, moet u mogelijk een *onveranderbare* containerinstallatiekopie. Een installatiekopie van een onveranderbare is een App die u kan niet per ongeluk verwijderen of te overschrijven. Gebruik de [az acr repository update] [ az-acr-repository-update] opdracht uit om de kenmerken van de opslagplaats, zodat u kunt:

* De versie van een installatiekopie of een hele opslagplaats vergrendelen

* Een versie van installatiekopie of een opslagplaats te beveiligen tegen verwijderen, maar updates toestaan

* Voorkomen dat leesbewerkingen (pull) op de versie van een installatiekopie of een volledige-opslagplaats

Zie de volgende secties voor meer voorbeelden.

## <a name="lock-an-image-or-repository"></a>Vergrendelen van een afbeelding of de opslagplaats 

### <a name="show-the-current-repository-attributes"></a>De huidige opslagplaats kenmerken weergeven
Als u wilt zien van de huidige kenmerken van de opslagplaats, voer de volgende [az acr repository show] [ az-acr-repository-show] opdracht:

```azurecli
az acr repository show \
    --name myregistry --repository myrepo
    --output jsonc
```

### <a name="show-the-current-image-attributes"></a>De huidige kenmerken van de installatiekopie van weergeven
Als u wilt zien van de huidige kenmerken van een tag, voer de volgende [az acr repository show] [ az-acr-repository-show] opdracht:

```azurecli
az acr repository show \
    --name myregistry --image image:tag \
    --output jsonc
```

### <a name="lock-an-image-by-tag"></a>Vergrendelen van een installatiekopie op label

Te vergrendelen. de *myrepo / myimage:tag* afbeelding *myregistry*, voer de volgende [az acr repository update] [ az-acr-repository-update] opdracht:

```azurecli
az acr repository update \
    --name myregistry --image myrepo/myimage:tag \
    --write-enabled false
```

### <a name="lock-an-image-by-manifest-digest"></a>Vergrendelen van een installatiekopie van een door manifest digest

Te vergrendelen. een *myrepo/myimage* installatiekopie geïdentificeerd door manifest digest (SHA-256-hash, weergegeven als `sha256:...`), voer de volgende opdracht uit. (Als wilt weten het manifest digest die zijn gekoppeld aan een of meer installatiekopielabels, voert de [az acr repository show-manifesten] [ az-acr-repository-show-manifests] opdracht.)

```azurecli
az acr repository update \
    --name myregistry --image myrepo/myimage@sha256:123456abcdefg \
    --write-enabled false
```

### <a name="lock-a-repository"></a>Een opslagplaats vergrendelen

Te vergrendelen. de *myrepo/myimage* opslagplaats en alle installatiekopieën in, voer de volgende opdracht:

```azurecli
az acr repository update \
    --name myregistry --repository myrepo/myimage \
    --write-enabled false
```

## <a name="protect-an-image-or-repository-from-deletion"></a>Een afbeelding of opslagplaats beveiligen tegen verwijderen

### <a name="protect-an-image-from-deletion"></a>Een installatiekopie te beveiligen tegen verwijderen

Om toe te staan de *myrepo / myimage:tag* afbeelding die moet worden bijgewerkt, maar is niet verwijderd, voert u de volgende opdracht uit:

```azurecli
az acr repository update \
    --name myregistry --image myrepo/myimage:tag \
    --delete-enabled false --write-enabled true
```

### <a name="protect-a-repository-from-deletion"></a>Een opslagplaats te beveiligen tegen verwijderen

De volgende opdracht stelt de *myrepo/myimage* opslagplaats, zodat deze kan niet worden verwijderd. Afzonderlijke afbeeldingen kunnen nog steeds worden bijgewerkt of verwijderd.

```azurecli
az acr repository update \
    --name myregistry --repository myrepo/myimage \
    --delete-enabled false --write-enabled true
```

## <a name="prevent-read-operations-on-an-image-or-repository"></a>Voorkomen dat leesbewerkingen op een afbeelding of de opslagplaats

Om te voorkomen dat leesbewerkingen (pull) op de *myrepo / myimage:tag* afbeelding, voer de volgende opdracht uit:

```azurecli
az acr repository update \
    --name myregistry --image myrepo/myimage:tag \
    --read-enabled false
```

Om te voorkomen dat leesbewerkingen op alle installatiekopieën in de *myrepo/myimage* opslagplaats, voer de volgende opdracht uit:

```azurecli
az acr repository update \
    --name myregistry --repository myrepo/myimage \
    --read-enabled false
```

## <a name="unlock-an-image-or-repository"></a>Een afbeelding of opslagplaats ontgrendelen

Herstellen van het standaardgedrag van de *myrepo / myimage:tag* installatiekopie, zodat kan worden verwijderd en bijgewerkt, voert u de volgende opdracht uit:

```azurecli
az acr repository update \
    --name myregistry --image myrepo/myimage:tag \
    --delete-enabled true --write-enabled true
```

Herstellen van het standaardgedrag van de *myrepo/myimage* opslagplaats en alle onderliggende installatiekopieën zodat ze kunnen worden verwijderd en worden bijgewerkt, voer de volgende opdracht:

```azurecli
az acr repository update \
    --name myregistry --repository myrepo/myimage \
    --delete-enabled true --write-enabled true
```

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd over het gebruik van de [az acr repository update] [ az-acr-repository-update] opdracht uit om te voorkomen dat verwijderen of bijwerken van de versies van een installatiekopie in een opslagplaats. Om extra kenmerken, Zie de [az acr repository update] [ az-acr-repository-update] opdrachten.

Als de kenmerken voor een versie van installatiekopie of een opslagplaats instellen wilt weergeven, gebruikt de [az acr repository show] [ az-acr-repository-show] opdracht.

Zie voor meer informatie over de delete-bewerkingen [verwijderen van installatiekopieën van containers in Azure Container Registry][container-registry-delete].

<!-- LINKS - Internal -->
[az-acr-repository-update]: /cli/azure/acr/repository#az-acr-repository-update
[az-acr-repository-show]: /cli/azure/acr/repository#az-acr-repository-show
[az-acr-repository-show-manifests]: /cli/azure/acr/repository#az-acr-repository-show-manifests
[azure-cli]: /cli/azure/install-azure-cli
[container-registry-delete]: container-registry-delete.md

