---
title: Koppelen van een volume emptyDir in Azure Container Instances
description: Meer informatie over het koppelen van een volume emptyDir voor het delen van gegevens tussen de containers in de containergroep van een in Azure Container Instances
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: article
ms.date: 02/08/2018
ms.author: danlep
ms.openlocfilehash: d91706da898e84effc6194a74dce69a66be0f4ac
ms.sourcegitcommit: cf438e4b4e351b64fd0320bf17cc02489e61406a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/08/2019
ms.locfileid: "67657609"
---
# <a name="mount-an-emptydir-volume-in-azure-container-instances"></a>Koppelen van een volume emptyDir in Azure Container Instances

Meer informatie over het koppelen van een *emptyDir* volume voor het delen van gegevens tussen de containers in de containergroep van een in Azure Container Instances.

> [!NOTE]
> Koppelen van een *emptyDir* volume is momenteel beperkt tot Linux-containers. Terwijl we werken om alle functies op Windows-containers, vindt u de huidige platform verschillen in de [overzicht](container-instances-overview.md#linux-and-windows-containers).

## <a name="emptydir-volume"></a>emptyDir-volume

De *emptyDir* volume biedt een beschrijfbare directory toegankelijk voor elke container in een container bevinden. Containers in de groep kunnen lezen en schrijven van dezelfde bestanden die in het volume, en kunnen worden gekoppeld met behulp van de dezelfde of verschillende paden in elke container.

Sommige voorbeeld wordt gebruikt voor een *emptyDir* volume:

* Scratchruimte
* Het plaatsen van controlepunten tijdens langlopende taken
* Store-gegevens opgehaald door een sidecar-container en geleverd door een App-container

Gegevens in een *emptyDir* volume worden persistent gemaakt via container crashes. Containers die opnieuw worden opgestart, maar zijn niet noodzakelijkerwijs om vast te leggen van de gegevens in een *emptyDir* volume.

## <a name="mount-an-emptydir-volume"></a>Een volume emptyDir koppelen

Voor het koppelen van een volume emptyDir in een containerexemplaar, moet u implementeren met behulp van een [Azure Resource Manager-sjabloon](/azure/templates/microsoft.containerinstance/containergroups).

Eerst, vullen het `volumes` -matrix in de containergroep `properties` gedeelte van de sjabloon. Vervolgens voor elke container in containergroep waarin u wilt koppelen de *emptyDir* volume, vullen het `volumeMounts` matrix in de `properties` sectie van de containerdefinitie van de.

De volgende Resource Manager-sjabloon maakt bijvoorbeeld een containergroep die bestaat uit twee containers, van welke koppelingen het *emptyDir* volume:

<!-- https://github.com/Azure/azure-docs-json-samples/blob/master/container-instances/aci-deploy-volume-emptydir.json -->
[!code-json[volume-emptydir](~/azure-docs-json-samples/container-instances/aci-deploy-volume-emptydir.json)]

Zie voor een voorbeeld van de implementatie van de containers-instantie met een Azure Resource Manager-sjabloon [groepen met meerdere containers in Azure Container Instances implementeren](container-instances-multi-container-group.md).

## <a name="next-steps"></a>Volgende stappen

Informatie over het koppelen van andere typen in Azure Container Instances:

* [Een Azure-bestandsshare in Azure Containerexemplaren koppelen](container-instances-volume-azure-files.md)
* [Koppelen van een volume gitRepo in Azure Containerexemplaren](container-instances-volume-gitrepo.md)
* [Koppelen van een geheime volume in Azure Container Instances](container-instances-volume-secret.md)