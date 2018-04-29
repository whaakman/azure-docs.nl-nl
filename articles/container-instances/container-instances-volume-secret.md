---
title: Koppelen van een geheime volume in Azure Containerexemplaren
description: Meer informatie over het koppelen van een geheime volume voor het opslaan van gevoelige informatie voor toegang door uw containerexemplaren
services: container-instances
author: mmacy
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 02/08/2018
ms.author: marsma
ms.openlocfilehash: 47abb4c305b1997fa3f44c07aa85dd01e5a94f1f
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="mount-a-secret-volume-in-azure-container-instances"></a>Koppelen van een geheime volume in Azure Containerexemplaren

Meer informatie over het koppelen van een *geheim* volume in de containerexemplaren voor het opslaan en ophalen van gevoelige informatie door de containers in de containergroepen.

> [!NOTE]
> Koppelen van een *geheim* volume is momenteel beperkt tot Linux containers. Hoewel we ons best doen om alle functies beschikbaar te maken voor Windows-containers, kunnen de [quota en beschikbaarheid in regio´s voor Azure Container Instances](container-instances-quotas.md) variëren op de verschillende platforms.

## <a name="secret-volume"></a>geheime volume

U kunt een *geheim* volume om gevoelige informatie naar de containers in de containergroep van een te geven. De *geheim* volume uw opgegeven geheimen worden opgeslagen in de bestanden in het volume, die de containers in uw containergroep kunnen bekijken. Met behulp van geheimen in een *geheim* volume, kunt u voorkomen dat gevoelige gegevens, zoals SSH-sleutels of databasereferenties plaatsen in de toepassingscode.

Alle *geheim* volumes worden ondersteund door [tmpfs][tmpfs], een RAM-geheugen ondersteund bestandssysteem; hun inhoud nooit naar niet-vluchtige opslag zijn geschreven.

## <a name="mount-a-secret-volume"></a>Een geheim volume koppelen

Koppelen van een *geheim* volume in een exemplaar van de container, moet u implementeren met behulp van een [Azure Resource Manager-sjabloon](/azure/templates/microsoft.containerinstance/containergroups).

Eerst vullen de `volumes` matrix in de containergroep `properties` gedeelte van de sjabloon. Vervolgens voor elke container in de containergroep waarin u wilt koppelen van de *geheim* volume, vullen de `volumeMounts` -matrix in de `properties` sectie van de definitie van de container.

De volgende Resource Manager-sjabloon maakt bijvoorbeeld een containergroep die bestaan uit een enkele container. De container koppelingen een *geheim* volume dat bestaat uit twee geheimen Base64-gecodeerd.

<!-- https://github.com/Azure/azure-docs-json-samples/blob/master/container-instances/aci-deploy-volume-secret.json -->
[!code-json[volume-secret](~/azure-docs-json-samples/container-instances/aci-deploy-volume-secret.json)]

Zie voor een voorbeeld van de implementatie van de container-exemplaar met een Azure Resource Manager-sjabloon [implementeren meerdere container groepen in Azure Containerexemplaren](container-instances-multi-container-group.md).

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het koppelen van andere volumetypen in Azure Containerexemplaren:

* [Een Azure-bestandsshare in Azure Containerexemplaren koppelen](container-instances-volume-azure-files.md)
* [Een volume emptyDir in Azure Containerexemplaren koppelen](container-instances-volume-emptydir.md)
* [Koppelen van een volume gitRepo in Azure Containerexemplaren](container-instances-volume-gitrepo.md)

<!-- LINKS - External -->
[tmpfs]: https://wikipedia.org/wiki/Tmpfs