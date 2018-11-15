---
title: Azure Container Registry-bestandsindelingen
description: Meer informatie over ondersteunde bestandsindelingen in Azure Container Registry.
services: container-registry
author: dlepow
manager: jeconnoc
ms.service: container-registry
ms.topic: article
ms.date: 10/31/2018
ms.author: danlep
ms.openlocfilehash: e7155604339bc634078fd022e05ede5f902bc0d8
ms.sourcegitcommit: 0b7fc82f23f0aa105afb1c5fadb74aecf9a7015b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2018
ms.locfileid: "51634705"
---
# <a name="content-formats-supported-in-azure-container-registry"></a>Inhoud die wordt ondersteund in Azure Container Registry

Gebruik een privé-opslagplaats in Azure Container Registry voor het beheren van een van de volgende inhoud indelingen. 

## <a name="docker-compatible-container-images"></a>Compatibel is met docker-containerinstallatiekopieën

* [Docker-installatiekopie Manifest V2, Schema 1](https://docs.docker.com/registry/spec/manifest-v2-1/)

* [Docker-installatiekopie Manifest V2, Schema 2](https://docs.docker.com/registry/spec/manifest-v2-2/) -bevat het Manifest bevat waarmee registers tot afbeeldingen onder een enkele 'installatiekopie: tag'-verwijzing op te slaan

* [Container initiatief (OCI) installatiekopie indelingsspecificatie openen](https://github.com/opencontainers/image-spec/blob/master/spec.md) 


## <a name="helm-charts"></a>Helm-grafieken

Azure Container Registry biedt ook opslagplaatsen voor [Helm-grafieken](https://helm.sh/), een verpakkingsindeling die wordt gebruikt om snel te beheren en implementeren van toepassingen voor Kubernetes. [Helm client](https://docs.helm.sh/using_helm/#installing-helm) versie 2.11.0 of hoger wordt ondersteund.

## <a name="next-steps"></a>Volgende stappen

* Zie hoe u [pushen en ophalen van](container-registry-get-started-docker-cli.md) installatiekopieën met Azure Container Registry.

* Gebruik [ACR taken](container-registry-tasks-overview.md) bouwen en testen van containerinstallatiekopieën. 

* Gebruik de [Moby BuildKit](https://github.com/moby/buildkit) te maken en containers in OCI-indeling.

* Instellen van een [Helm-opslagplaats](container-registry-helm-repos.md) die wordt gehost in Azure Container Registry. 


