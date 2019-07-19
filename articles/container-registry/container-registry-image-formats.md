---
title: Inhouds indelingen Azure Container Registry
description: Meer informatie over ondersteunde inhouds indelingen in Azure Container Registry.
services: container-registry
author: dlepow
manager: gwallace
ms.service: container-registry
ms.topic: article
ms.date: 04/18/2019
ms.author: danlep
ms.openlocfilehash: c804dd7b73ea4a51c02c8b342a4ac60d992ec7c5
ms.sourcegitcommit: f5075cffb60128360a9e2e0a538a29652b409af9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/18/2019
ms.locfileid: "68310679"
---
# <a name="content-formats-supported-in-azure-container-registry"></a>Inhouds indelingen die worden ondersteund in Azure Container Registry

Gebruik een privé-opslag plaats in Azure Container Registry om een van de volgende inhouds indelingen te beheren. 

## <a name="docker-compatible-container-images"></a>Docker-compatibele container installatie kopieën

De volgende indelingen van de docker-container installatie kopieën worden ondersteund:

* [Manifest voor docker-installatie kopie v2, schema 1](https://docs.docker.com/registry/spec/manifest-v2-1/)

* [Docker-installatie kopie-manifest v2, schema 2](https://docs.docker.com/registry/spec/manifest-v2-2/) : bevat manifest lijsten waarmee registers meerdere platform afbeeldingen kunnen opslaan onder één ' image: tag '-verwijzing

## <a name="oci-images"></a>OCI-afbeeldingen

Azure Container Registry biedt ook ondersteuning voor installatie kopieën die voldoen aan de indeling van het [opening container Initiative (OCI)-afbeelding](https://github.com/opencontainers/image-spec/blob/master/spec.md). Pakket indelingen zijn onder andere de indeling van de [enkelvouds afbeelding](https://www.sylabs.io/2018/03/sif-containing-your-containers/).

## <a name="helm-charts"></a>Helm grafieken

Azure Container Registry kunt opslag plaatsen hosten voor [helm-grafieken](https://helm.sh/), een verpakkings indeling die wordt gebruikt voor het snel beheren en implementeren van toepassingen voor Kubernetes. [Helm-client](https://docs.helm.sh/using_helm/#installing-helm) versie 2.11.0 of hoger wordt ondersteund.

## <a name="next-steps"></a>Volgende stappen

* Zie installatie kopieën [pushen en pullen](container-registry-get-started-docker-cli.md) met Azure container Registry.

* Gebruik [ACR-taken](container-registry-tasks-overview.md) om container installatie kopieën te bouwen en te testen. 

* Gebruik de [Moby-BuildKit](https://github.com/moby/buildkit) voor het maken en inpakken van containers in OCI-indeling.

* Stel een [helm-opslag plaats](container-registry-helm-repos.md) in die wordt gehost in azure container Registry. 


