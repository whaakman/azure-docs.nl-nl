---
title: Azure Container Registry-bestandsindelingen
description: Meer informatie over ondersteunde bestandsindelingen in Azure Container Registry.
services: container-registry
author: dlepow
manager: jeconnoc
ms.service: container-registry
ms.topic: article
ms.date: 04/18/2019
ms.author: danlep
ms.openlocfilehash: fe129847e685c7151a9b7ad7ea65abbd38530733
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60827456"
---
# <a name="content-formats-supported-in-azure-container-registry"></a>Inhoud die wordt ondersteund in Azure Container Registry

Gebruik een privé-opslagplaats in Azure Container Registry voor het beheren van een van de volgende inhoud indelingen. 

## <a name="docker-compatible-container-images"></a>Compatibel is met docker-containerinstallatiekopieën

De volgende Docker-container-installatiekopie indelingen worden ondersteund:

* [Docker-installatiekopie Manifest V2, Schema 1](https://docs.docker.com/registry/spec/manifest-v2-1/)

* [Docker-installatiekopie Manifest V2, Schema 2](https://docs.docker.com/registry/spec/manifest-v2-2/) -bevat het Manifest bevat waarmee registers tot afbeeldingen onder een enkele 'installatiekopie: tag'-verwijzing op te slaan

## <a name="oci-images"></a>OCI installatiekopieën

Azure Container Registry biedt ook ondersteuning voor installatiekopieën die voldoen aan de [Open Container initiatief OCI ()-installatiekopie indelingsspecificatie](https://github.com/opencontainers/image-spec/blob/master/spec.md). Verpakking notaties zijn onder andere [Singularity installatiekopie indeling (SIF)](https://www.sylabs.io/2018/03/sif-containing-your-containers/).

## <a name="helm-charts"></a>Helm-grafieken

Azure Container Registry kunt hosten opslagplaatsen voor [Helm-grafieken](https://helm.sh/), een verpakkingsindeling die wordt gebruikt om snel te beheren en implementeren van toepassingen voor Kubernetes. [Helm client](https://docs.helm.sh/using_helm/#installing-helm) versie 2.11.0 of hoger wordt ondersteund.

## <a name="next-steps"></a>Volgende stappen

* Zie hoe u [pushen en ophalen van](container-registry-get-started-docker-cli.md) installatiekopieën met Azure Container Registry.

* Gebruik [ACR taken](container-registry-tasks-overview.md) bouwen en testen van containerinstallatiekopieën. 

* Gebruik de [Moby BuildKit](https://github.com/moby/buildkit) te maken en containers in OCI-indeling.

* Instellen van een [Helm-opslagplaats](container-registry-helm-repos.md) die wordt gehost in Azure Container Registry. 


