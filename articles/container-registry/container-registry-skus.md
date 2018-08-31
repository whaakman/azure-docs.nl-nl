---
title: Azure Container Registry-SKU 's
description: Vergelijk de verschillende Servicelagen beschikbaar in Azure Container Registry.
services: container-registry
author: mmacy
manager: jeconnoc
ms.service: container-registry
ms.topic: article
ms.date: 08/30/2018
ms.author: marsma
ms.openlocfilehash: eb3a1745677871211df05d18e28d32061f360bac
ms.sourcegitcommit: 1fb353cfca800e741678b200f23af6f31bd03e87
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/30/2018
ms.locfileid: "43301813"
---
# <a name="azure-container-registry-skus"></a>Azure Container Registry-SKU 's

Azure Container Registry (ACR) is beschikbaar in verschillende Servicelagen, ook wel SKU's. Deze SKU's bieden voorspelbare prijzen en verschillende opties voor het afstemmen van de capaciteit en gebruikspatronen van uw persoonlijke Docker-register in Azure.

| SKU | Managed | Beschrijving |
| --- | :-------: | ----------- |
| **Basic** | Ja | Een rendabel toegangspunt voor ontwikkelaars die meer willen leren over Azure Container Registry. Basic registers hebben dezelfde programmatische mogelijkheden als standaard en Premium (integratie van Azure Active Directory-verificatie, verwijdering van afbeelding en webhooks). De inbegrepen opslag en doorvoer van de afbeelding zijn echter meest geschikt is voor lagere gebruiksscenario's. |
| **Standard** | Ja | Standard registers bieden dezelfde mogelijkheden als Basic met verbeterde inbegrepen opslag- en image-doorvoer. Standard-registers moeten voldoen aan de behoeften van de meeste productiescenario's. |
| **Premium** | Ja | Premium-registers voorzien in de grootste hoeveelheid inbegrepen opslag en gelijktijdige bewerkingen, voor het inschakelen van scenario's met hoge volumes. Naast de hogere doorvoer van de installatiekopie, Premium voegt functies zoals [geo-replicatie] [ container-registry-geo-replication] voor het beheren van een enkel register in meerdere regio's en [inhoud vertrouwen (preview)](container-registry-content-trust.md)voor het ondertekenen van de tag de installatiekopie. |
| Klassieke<sup>1</sup> | Nee | Deze SKU ingeschakeld voor de eerste release van de Azure Container Registry-service in Azure. Klassieke registers worden ondersteund door een storage-account dat in uw abonnement, zodat ze worden beperkt de mogelijkheid voor ACR voor een hoger niveau mogelijkheden, zoals verbeterde doorvoer en geo-replicatie in Azure gemaakt. |

<sup>1</sup> de klassieke SKU is **afgeschaft** in **maart 2019**. Basic, Standard of Premium gebruiken voor alle nieuwe container Registry.

Als u kiest dat een hoger niveau SKU biedt betere prestaties en schaal, echter alle beheerde SKU's bieden dezelfde programmatische mogelijkheden. Met meerdere Servicelagen, kunt u kunt aan de slag met Basic en vervolgens converteren naar Standard en Premium wanneer uw register gebruik toeneemt.

## <a name="managed-vs-unmanaged"></a>Beheerde en onbeheerde

De Basic, Standard en Premium-SKU's genoemd *beheerd* registers en klassieke registers als *niet-beheerde*. Het belangrijkste verschil tussen de twee is hoe uw containerinstallatiekopieën worden opgeslagen.

### <a name="managed-basic-standard-premium"></a>Beheerde (Basic, Standard, Premium)

Beheerde registers voordeel uit afbeeldingopslag volledig door Azure beheerd. Dat wil zeggen, weergegeven een opslagaccount waarin de afbeeldingen niet in uw Azure-abonnement. Er zijn verschillende voordelen binnenhaalt wanneer u een van de beheerde registry-SKU's, die worden beschreven in uitgebreide [afbeeldingopslag Container in Azure Container Registry][container-registry-storage]. In dit artikel richt zich op de beheerd register-SKU's en de bijbehorende mogelijkheden.

### <a name="unmanaged-classic"></a>Niet-beheerde (klassiek)

> [!IMPORTANT]
> De klassieke SKU wordt afgeschaft en zal na maart 2019 niet beschikbaar zijn. Basic, Standard of Premium gebruiken voor alle nieuwe registers.

Klassieke registers zijn 'niet-beheerde' in de zin dat de storage-account dat een back-up een Classic-register zich bevindt binnen *uw* Azure-abonnement. Daarom bent u verantwoordelijk voor het beheer van het opslagaccount waarin uw containerinstallatiekopieën worden opgeslagen. Met niet-beheerde registers, kunt u niet overschakelen tussen SKU's wanneer uw behoeften veranderen (anders dan [upgraden] [ container-registry-upgrade] naar een beheerd register), en verschillende functies van beheerde registers zijn niet beschikbaar (bijvoorbeeld: verwijderen van container-installatiekopie, [geo-replicatie][container-registry-geo-replication], en [webhooks][container-registry-webhook]).

Zie voor meer informatie over het upgraden van een Classic-register op een van de beheerde SKU's [een klassiek register upgraden][container-registry-upgrade].

## <a name="sku-feature-matrix"></a>SKU Functiematrix

De volgende tabel worden de functies en limieten van de Servicelagen Basic, Standard en Premium.

[!INCLUDE [container-instances-limits](../../includes/container-registry-limits.md)]

## <a name="changing-skus"></a>Wijzigen van SKU 's

U kunt de SKU van een register met de Azure CLI of in de Azure-portal wijzigen. U kan bewegen tussen beheerde SKU's, zolang de SKU die u naar omschakelt de vereiste maximale opslagcapaciteit is. Als u overschakelt naar een van de beheerde SKU's van het klassieke implementatiemodel, kan niet u terug verplaatsen naar klassieke--het is een eenrichtingsvertrouwensrelatie conversie.

### <a name="azure-cli"></a>Azure-CLI

Als u wilt verplaatsen tussen SKU's in de Azure CLI, gebruikt u de [az acr update] [ az-acr-update] opdracht. Als u bijvoorbeeld wilt overschakelen naar Premium:

```azurecli
az acr update --name myregistry --sku Premium
```

### <a name="azure-portal"></a>Azure Portal

In het containerregister **overzicht** in Azure portal, selecteert u **Update**, selecteer vervolgens een nieuwe **SKU** in de vervolgkeuzelijst SKU.

![Containerregister-SKU in Azure portal bijwerken][update-registry-sku]

Als u een klassiek register hebt, kunt u een beheerde SKU in de Azure-portal niet selecteren. In plaats daarvan moet u eerst [upgrade] [ container-registry-upgrade] naar een beheerd register (Zie [wijzigen van het klassieke implementatiemodel](#changing-from-classic)).

## <a name="changing-from-classic"></a>Wijzigen van het klassieke implementatiemodel

Zijn er aanvullende overwegingen rekening moet houden bij het migreren van een niet-beheerde Classic-register op een van de beheerde Basic, Standard of Premium-SKU's. Als uw Classic-register een groot aantal installatiekopieën bevat en vele gigabytes groot is, kan het migratieproces enige tijd duren. Bovendien `docker push` bewerkingen zijn uitgeschakeld totdat de migratie voltooid is.

Zie voor meer informatie over het bijwerken van uw Classic-register op een van de beheerde SKU's [een container klassiek register upgraden][container-registry-upgrade].

## <a name="pricing"></a>Prijzen

Zie voor informatie over elk van de Azure Container Registry-SKU's prijzen, [prijzen voor Container Registry][container-registry-pricing].

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
[container-registry-webhook]: container-registry-webhook.md
