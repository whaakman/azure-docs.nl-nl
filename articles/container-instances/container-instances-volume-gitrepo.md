---
title: Een volume gitRepo Azure Containerexemplaren koppelen
description: Meer informatie over het koppelen van een volume gitRepo een Git-opslagplaats in uw containerexemplaren klonen
services: container-instances
author: mmacy
manager: timlt
ms.service: container-instances
ms.topic: article
ms.date: 02/08/2018
ms.author: marsma
ms.openlocfilehash: 9acde9259fcb392458e7b2fa7d3369776978285e
ms.sourcegitcommit: 95500c068100d9c9415e8368bdffb1f1fd53714e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/14/2018
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

## <a name="mount-a-gitrepo-volume"></a>Een volume gitRepo koppelen

Koppelen van een *gitRepo* volume in een exemplaar van de container, moet u implementeren met behulp van een [Azure Resource Manager-sjabloon](/azure/templates/microsoft.containerinstance/containergroups).

Eerst vullen de `volumes` matrix in de containergroep `properties` gedeelte van de sjabloon. Vervolgens voor elke container in de containergroep waarin u wilt koppelen van de *gitRepo* volume, vullen de `volumeMounts` -matrix in de `properties` sectie van de definitie van de container.

De volgende Resource Manager-sjabloon maakt bijvoorbeeld een containergroep die bestaan uit een enkele container. De container kloont twee GitHub-opslagplaatsen opgegeven door de *gitRepo* volumeblokken. Het tweede volume bevat aanvullende eigenschappen opgeven van een map voor het klonen naar en de commit-hash van een specifieke revisie om te klonen.

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
