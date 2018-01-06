---
title: Azure Container register-opslagplaatsen in de Azure portal
description: Het Azure Container register opslagplaatsen weergeven in de Azure-portal.
services: container-registry
author: cristy
manager: timlt
ms.service: container-registry
ms.topic: article
ms.date: 01/05/2018
ms.author: cristyg
ms.openlocfilehash: 593972e972207a27d1232fcb0c1bf220ac3a8def
ms.sourcegitcommit: 1d423a8954731b0f318240f2fa0262934ff04bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/05/2018
---
# <a name="view-container-registry-repositories-in-the-azure-portal"></a>Container register opslagplaatsen weergeven in de Azure portal

Azure Container register kunt u voor het opslaan van Docker container afbeeldingen in de opslagplaatsen. Afbeeldingen in de opslagplaatsen opslaat, kunt u groepen van installatiekopieën (of versies van afbeeldingen) opslaan in een geïsoleerde omgeving. U kunt deze opslagplaatsen opgeven wanneer u push-installatiekopieën naar het register en bekijk de inhoud in de Azure portal.

## <a name="prerequisites"></a>Vereisten

* **Container register**: het register van een container maken in uw Azure-abonnement. Gebruik bijvoorbeeld de [Azure-portal](container-registry-get-started-portal.md) of de [Azure CLI](container-registry-get-started-azure-cli.md).
* **Docker CLI**: installeren [Docker] [ docker-install] op uw lokale computer, kunt u met de Docker-opdrachtregelinterface.
* **Afbeelding van de container**: een installatiekopie van een Push naar het register van de container. Zie voor instructies over hoe push als pull-installatiekopieën, [Push als pull van een installatiekopie van een](container-registry-get-started-docker-cli.md).

## <a name="view-repositories-in-azure-portal"></a>Weergave-opslagplaatsen in Azure-portal

Hier ziet u een lijst van de opslagplaatsen voor het hosten van uw afbeeldingen, evenals de labels installatiekopie in de Azure portal.

Als u de stappen in [Push als pull van een installatiekopie van een](container-registry-get-started-docker-cli.md) (en vervolgens de installatiekopie niet verwijdert), moet u een installatiekopie van een Nginx hebben in het register van de container. De instructies in dit artikel wordt opgegeven dat u de installatiekopie met een naamruimte, 'voorbeelden' labelen op `/samples/nginx`. Als een refresher de [docker push] [ docker-push] opdracht die is opgegeven in dit artikel is:

```Bash
docker push myregistry.azurecr.io/samples/nginx
```

 Omdat Azure Container register dergelijke naamruimten met meerdere niveaus opslagplaats ondersteunt, kunt u het bereik van verzamelingen van afbeeldingen met betrekking tot een specifieke app of een verzameling van apps naar verschillende ontwikkelings- of operationele teams. Voor meer informatie over opslagplaatsen in container registers Zie [registers voor persoonlijke Docker-container in Azure](container-registry-intro.md).

Een opslagplaats weergeven:

1. Aanmelden bij de [Azure-portal][portal]
1. Selecteer de **Azure Container register** waarnaar u de installatiekopie van het Nginx gepusht
1. Selecteer **opslagplaatsen** voor een overzicht van de opslagplaatsen met de afbeeldingen in het register
1. Selecteer een opslagplaats voor een overzicht van de installatiekopie labels binnen die opslagplaats

Als u de installatiekopie van het Nginx als gepusht bijvoorbeeld volgens de aanwijzingen in [Push als pull van een installatiekopie van een](container-registry-get-started-docker-cli.md), ziet u iets soortgelijks als:

![Opslagplaatsen in de portal](./media/container-registry-repositories/container-registry-repositories.png)

## <a name="next-steps"></a>Volgende stappen

Nu u weet dat de basisprincipes van weer te geven en werken met opslagplaatsen in de portal, proberen met Azure Container register met een [Azure Container Service (AKS)](../aks/tutorial-kubernetes-prepare-app.md) cluster.

<!-- LINKS - External -->
[docker-install]: https://docs.docker.com/engine/installation/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[portal]: https://portal.azure.com
