---
title: Azure Container Registry-SKU 's
description: Vergelijk de verschillende Servicelagen beschikbaar in Azure Container Registry.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: article
ms.date: 03/26/2019
ms.author: danlep
ms.openlocfilehash: 7fdc1417ac524b422a12a087cf1661040efb9f8a
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/27/2019
ms.locfileid: "58521955"
---
# <a name="azure-container-registry-skus"></a>Azure Container Registry-SKU 's

Azure Container Registry (ACR) is beschikbaar in verschillende Servicelagen, ook wel SKU's. Deze SKU's bieden voorspelbare prijzen en verschillende opties voor het afstemmen van de capaciteit en gebruikspatronen van uw persoonlijke Docker-register in Azure.

| SKU | Managed | Description |
| --- | :-------: | ----------- |
| **Basic** | Ja | Een rendabel toegangspunt voor ontwikkelaars die meer willen leren over Azure Container Registry. Basic registers hebben dezelfde programmatische mogelijkheden als standaard en Premium (zoals Azure Active Directory [verificatie integratie](container-registry-authentication.md#individual-login-with-azure-ad), [installatiekopie verwijderen][container-registry-delete], en [webhooks][container-registry-webhook]). De inbegrepen opslag en doorvoer van de afbeelding zijn echter meest geschikt is voor lagere gebruiksscenario's. |
| **Standard** | Ja | Standard registers bieden dezelfde mogelijkheden als Basic met verbeterde inbegrepen opslag- en image-doorvoer. Standard-registers moeten voldoen aan de behoeften van de meeste productiescenario's. |
| **Premium** | Ja | Premium-registers voorzien in de grootste hoeveelheid inbegrepen opslag en gelijktijdige bewerkingen, voor het inschakelen van scenario's met hoge volumes. Naast de hogere doorvoer van de installatiekopie, Premium-functies zoals voegt [geo-replicatie] [ container-registry-geo-replication] voor het beheren van een enkel register in meerdere regio's [vertrouwen (preview)vaninhoud](container-registry-content-trust.md) voor het ondertekenen van de installatiekopie van code, en [firewalls en virtuele netwerken (preview)](container-registry-vnet.md) toegang te beperken tot het register. |
|  Klassieke (*niet beschikbaar na April 2019*) | Nee | Deze SKU ingeschakeld voor de eerste release van de Azure Container Registry-service in Azure. Klassieke registers worden ondersteund door een storage-account dat in uw abonnement, zodat ze worden beperkt de mogelijkheid voor ACR voor een hoger niveau mogelijkheden, zoals verbeterde doorvoer en geo-replicatie in Azure gemaakt. |

> [!IMPORTANT]
> De SKU wordt Classic-register **afgeschaft**, en is niet beschikbaar na **April 2019**. Wordt u aangeraden Basic, Standard of Premium voor alle nieuwe registers. Alle bestaande klassieke registers moeten worden bijgewerkt vóór April 2019. Zie voor upgrade-informatie [een klassiek register upgraden][container-registry-upgrade].

De Basic, Standard en Premium-SKU's (samen met de naam *beheerde registers*) bieden dezelfde programmatische mogelijkheden. Ze ook allemaal het voordeel van [afbeeldingopslag] [ container-registry-storage] volledig door Azure beheerd. U een hogere SKU kiest, biedt betere prestaties en schaal. Met meerdere Servicelagen, kunt u kunt aan de slag met Basic en vervolgens converteren naar Standard en Premium wanneer uw register gebruik toeneemt.

## <a name="sku-feature-matrix"></a>SKU Functiematrix

De volgende tabel worden de functies en limieten van de Servicelagen Basic, Standard en Premium.

[!INCLUDE [container-instances-limits](../../includes/container-registry-limits.md)]

## <a name="changing-skus"></a>Wijzigen van SKU 's

U kunt de SKU van een register met de Azure CLI of in de Azure-portal wijzigen. U kan bewegen tussen beheerde SKU's, zolang de SKU die u naar omschakelt de vereiste maximale opslagcapaciteit is. Wanneer u overschakelt naar een van de beheerde SKU's van het klassieke implementatiemodel, kan niet u terug verplaatsen naar klassieke--het is een eenrichtingsvertrouwensrelatie conversie.

### <a name="azure-cli"></a>Azure-CLI

Als u wilt verplaatsen tussen SKU's in de Azure CLI, gebruikt u de [az acr update] [ az-acr-update] opdracht. Als u bijvoorbeeld wilt overschakelen naar Premium:

```azurecli
az acr update --name myregistry --sku Premium
```

### <a name="azure-portal"></a>Azure Portal

In het containerregister **overzicht** in Azure portal, selecteert u **Update**, selecteer vervolgens een nieuwe **SKU** in de vervolgkeuzelijst SKU.

![Containerregister-SKU in Azure portal bijwerken][update-registry-sku]

Als u een klassiek register hebt, kunt u een beheerde SKU in de Azure-portal niet selecteren. In plaats daarvan moet u eerst [upgrade] [ container-registry-upgrade] naar een beheerd register.

## <a name="pricing"></a>Prijzen

Zie voor informatie over elk van de Azure Container Registry-SKU's prijzen, [prijzen voor Container Registry][container-registry-pricing].

Zie voor meer informatie over de prijzen voor gegevensoverdracht [Bandwidth Pricing Details](https://azure.microsoft.com/pricing/details/bandwidth/). 

## <a name="next-steps"></a>Volgende stappen

**Roadmap voor Azure Container Registry**

Ga naar de [ACR Roadmap] [ acr-roadmap] op GitHub voor meer informatie over nieuwe functies van de service.

**Azure Container Registry UserVoice**

Verzenden en te stemmen op nieuwe suggesties voor functies in [ACR UserVoice][container-registry-uservoice].

<!-- IMAGES -->
[update-registry-sku]: ./media/container-registry-skus/update-registry-sku.png

<!-- LINKS - External -->
[acr-roadmap]: https://aka.ms/acr/roadmap
[container-registry-pricing]: https://azure.microsoft.com/pricing/details/container-registry/
[container-registry-uservoice]: https://feedback.azure.com/forums/903958-azure-container-registry

<!-- LINKS - Internal -->
[az-acr-update]: /cli/azure/acr#az-acr-update
[container-registry-geo-replication]: container-registry-geo-replication.md
[container-registry-upgrade]: container-registry-upgrade.md
[container-registry-storage]: container-registry-storage.md
[container-registry-delete]: container-registry-delete.md
[container-registry-webhook]: container-registry-webhook.md
