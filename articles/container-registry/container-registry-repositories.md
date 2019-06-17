---
title: Azure Container Registry-opslagplaatsen in Azure portal
description: Klik hier voor meer informatie over het weergeven van Azure Container Registry opslagplaatsen in Azure portal.
services: container-registry
author: cristy
manager: jeconnoc
ms.service: container-registry
ms.topic: article
ms.date: 01/05/2018
ms.author: cristyg
ms.openlocfilehash: 685c978ff206e75d770918f2528a826ad522b706
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64710186"
---
# <a name="view-container-registry-repositories-in-the-azure-portal"></a>Container registry opslagplaatsen weergeven in Azure portal

Azure Container Registry kunt u voor het opslaan van Docker-containerinstallatiekopieën in opslagplaatsen. Door op te slaan de afbeeldingen in opslagplaatsen, kunt u groepen van installatiekopieën (of versies van afbeeldingen) opslaan in een geïsoleerde omgeving. U kunt deze opslagplaatsen opgeven wanneer u installatiekopieën naar het register pushen en hun inhoud in Azure portal weergeven.

## <a name="prerequisites"></a>Vereisten

* **Containerregister**: Een containerregister maken in uw Azure-abonnement. Gebruik bijvoorbeeld de [Azure-portal](container-registry-get-started-portal.md) of de [Azure CLI](container-registry-get-started-azure-cli.md).
* **Docker CLI**: Installeer [Docker] [ docker-install] op uw lokale computer, kunt u met de Docker-opdrachtregelinterface.
* **Containerinstallatiekopie**: Een installatiekopie pushen naar uw containerregister. Zie voor instructies over het push- en pull-installatiekopieën, [pushen en ophalen van een installatiekopie van een](container-registry-get-started-docker-cli.md).

## <a name="view-repositories-in-azure-portal"></a>Opslagplaatsen weergeven in Azure portal

U ziet een lijst van de opslagplaatsen die als host fungeert voor de afbeeldingen, evenals de installatiekopielabels in Azure portal.

Als u de stappen in [pushen en ophalen van een installatiekopie van een](container-registry-get-started-docker-cli.md) (en vervolgens de installatiekopie niet verwijdert), moet u een Nginx-installatiekopie hebben in uw containerregister. De instructies in dit artikel wordt opgegeven dat u de installatiekopie met een naamruimte, de 'voorbeelden' labelen op `/samples/nginx`. Als een refresher de [docker push] [ docker-push] opdracht die is opgegeven in dit artikel is:

```Bash
docker push myregistry.azurecr.io/samples/nginx
```

 Omdat Azure Container Registry biedt ondersteuning voor dergelijke naamruimten voor opslagplaatsen op meerdere niveaus, kunt u het bereik van verzamelingen van installatiekopieën die betrekking hebben op een specifieke app of een verzameling van apps aan verschillende ontwikkelingsteams of operationele teams. Meer informatie over de opslagplaatsen in container Registry, Zie [privé-Docker-containerregisters in Azure](container-registry-intro.md).

Een opslagplaats weergeven:

1. Aanmelden bij de [Azure-portal][portal]
1. Selecteer de **Azure Container Registry** waarnaar u de Nginx-installatiekopie hebt gepusht
1. Selecteer **opslagplaatsen** voor een overzicht van de opslagplaatsen die de afbeeldingen in het register bevatten
1. Selecteer een opslagplaats om de tags installatiekopie vanuit die opslagplaats te bekijken

Als u de Nginx-installatiekopie als gepusht bijvoorbeeld volgens de aanwijzingen in [pushen en ophalen van een installatiekopie van een](container-registry-get-started-docker-cli.md), u moet er ongeveer uitzien als:

![Opslagplaatsen in de portal](./media/container-registry-repositories/container-registry-repositories.png)

## <a name="next-steps"></a>Volgende stappen

Nu u weet dat de basisbeginselen van het weergeven en werken met opslagplaatsen in de portal, kunt u proberen met behulp van Azure Container Registry met een [Azure Kubernetes Service (AKS)](../aks/tutorial-kubernetes-prepare-app.md) cluster.

<!-- LINKS - External -->
[docker-install]: https://docs.docker.com/engine/installation/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[portal]: https://portal.azure.com
