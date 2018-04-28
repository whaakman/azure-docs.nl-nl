---
title: Een volume emptyDir in Azure Containerexemplaren koppelen
description: Meer informatie over het koppelen van een volume emptyDir om gegevens tussen de containers in een containergroep in Azure Containerexemplaren te delen
services: container-instances
author: mmacy
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 02/08/2018
ms.author: marsma
ms.openlocfilehash: 89289a7a0bb5c486c662d528c5014bdbd8eebaca
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="mount-an-emptydir-volume-in-azure-container-instances"></a>Een volume emptyDir in Azure Containerexemplaren koppelen

Meer informatie over het koppelen van een *emptyDir* volume voor het delen van gegevens tussen de containers in een containergroep in Azure Containerexemplaren.

> [!NOTE]
> Koppelen van een *emptyDir* volume is momenteel beperkt tot Linux containers. Hoewel we ons best doen om alle functies beschikbaar te maken voor Windows-containers, kunnen de [quota en beschikbaarheid in regio´s voor Azure Container Instances](container-instances-quotas.md) variëren op de verschillende platforms.

## <a name="emptydir-volume"></a>emptyDir volume

De *emptyDir* volume biedt een beschrijfbare map toegankelijk is voor elke container in een containergroep. Containers in de groep kunnen lezen en schrijven van dezelfde bestanden die in het volume en kan worden gekoppeld met behulp van de dezelfde of verschillende paden in elke container.

Sommige voorbeeld wordt gebruikt voor een *emptyDir* volume:

* Scratchruimte
* Het plaatsen van controlepunten tijdens langlopende taken worden uitgevoerd
* Opslaan van gegevens door een container ter opgehaald en geleverd door een toepassingscontainer

Gegevens in een *emptyDir* volume via container crashes worden bewaard. Containers die opnieuw worden opgestart, echter niet worden gegarandeerd om vast te leggen van de gegevens in een *emptyDir* volume.

## <a name="mount-an-emptydir-volume"></a>Een volume emptyDir koppelen

Een volume emptyDir in een container-exemplaar koppelen, moet u implementeren met behulp van een [Azure Resource Manager-sjabloon](/azure/templates/microsoft.containerinstance/containergroups).

Eerst vullen de `volumes` matrix in de containergroep `properties` gedeelte van de sjabloon. Vervolgens voor elke container in de containergroep waarin u wilt koppelen van de *emptyDir* volume, vullen de `volumeMounts` -matrix in de `properties` sectie van de definitie van de container.

De volgende Resource Manager-sjabloon maakt u bijvoorbeeld een containergroep die bestaat uit twee containers elk welke koppelingen het *emptyDir* volume:

<!-- https://github.com/Azure/azure-docs-json-samples/blob/master/container-instances/aci-deploy-volume-emptydir.json -->
[!code-json[volume-emptydir](~/azure-docs-json-samples/container-instances/aci-deploy-volume-emptydir.json)]

Zie voor een voorbeeld van de implementatie van de container-exemplaar met een Azure Resource Manager-sjabloon [implementeren meerdere container groepen in Azure Containerexemplaren](container-instances-multi-container-group.md).

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het koppelen van andere volumetypen in Azure Containerexemplaren:

* [Een Azure-bestandsshare in Azure Containerexemplaren koppelen](container-instances-volume-azure-files.md)
* [Koppelen van een volume gitRepo in Azure Containerexemplaren](container-instances-volume-gitrepo.md)
* [Koppelen van een geheime volume in Azure Containerexemplaren](container-instances-volume-secret.md)