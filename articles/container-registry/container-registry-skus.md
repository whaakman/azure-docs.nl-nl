---
title: Azure Container Registry Sku's
description: Vergelijk de verschillende service lagen die beschikbaar zijn in Azure Container Registry.
services: container-registry
author: dlepow
manager: gwallace
ms.service: container-registry
ms.topic: article
ms.date: 05/06/2019
ms.author: danlep
ms.openlocfilehash: bf620178a0c10661126b3e52c7b908ccc9a90d89
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68311889"
---
# <a name="azure-container-registry-skus"></a>Azure Container Registry Sku's

Azure Container Registry (ACR) is beschikbaar in meerdere service lagen, ook wel Sku's genoemd. Deze Sku's bieden voorspel bare prijzen en verschillende opties om af te stemmen op de capaciteits-en gebruiks patronen van uw privé-docker-REGI ster in Azure.

| SKU | Beheerd | Description |
| --- | :-------: | ----------- |
| **Basic** | Ja | Een rendabel toegangspunt voor ontwikkelaars die meer willen leren over Azure Container Registry. Basis registers hebben dezelfde programmatische mogelijkheden als standaard en Premium (zoals integratie van Azure Active Directory [verificatie](container-registry-authentication.md#individual-login-with-azure-ad), het [verwijderen van afbeeldingen][container-registry-delete]en webhooks). [][container-registry-webhook] De inbegrepen opslag-en afbeeldings doorvoer zijn echter het meest geschikt voor lagere gebruiks scenario's. |
| **Standard** | Ja | Standaard-registers bieden dezelfde mogelijkheden als basis, met meer inbegrepen opslag en de door Voer van de installatie kopie. Standard-registers moeten voldoen aan de behoeften van de meeste productiescenario's. |
| **Premium** | Ja | Premium-registers bieden de hoogste hoeveelheid inbegrepen opslag en gelijktijdige bewerkingen, waardoor scenario's met hoge volumes mogelijk zijn. Naast een hogere afbeeldings doorvoer voegt Premium functies toe, waaronder [geo-replicatie][container-registry-geo-replication] voor het beheren van één REGI ster in meerdere regio's, [vertrouwen van inhoud](container-registry-content-trust.md) voor het ondertekenen van installatie kopieën en firewalls [en virtuele netwerken (preview)](container-registry-vnet.md) tot Beperk de toegang tot het REGI ster. |
|  Klassiek (*niet beschikbaar na 2019 april*) | Nee | Deze SKU heeft de eerste versie van de Azure Container Registry-service in azure ingeschakeld. Klassieke registers worden ondersteund door een opslag account dat door Azure in uw abonnement wordt gemaakt. Hiermee wordt de mogelijkheid beperkt om ACR mogelijkheden op hoger niveau te bieden, zoals een verhoogde door Voer en geo-replicatie. |

> [!IMPORTANT]
> De klassieke register-SKU wordt **afgeschaft**en zal na **2019 april**niet meer beschikbaar zijn. U kunt het beste Basic, Standard of Premium gebruiken voor alle nieuwe registers. Alle bestaande klassieke registers moeten worden bijgewerkt vóór 2019 april. Zie [een klassiek REGI ster upgraden][container-registry-upgrade]voor informatie over de upgrade.

De basis-, standaard-en Premium-Sku's (gezamenlijk *beheerde registers*genoemd) bieden allemaal dezelfde programmatische mogelijkheden. Ze profiteren ook van [installatie kopie opslag][container-registry-storage] die volledig wordt beheerd door Azure. Het kiezen van een SKU op een hoger niveau biedt meer prestaties en schaal baarheid. Met meerdere service lagen kunt u aan de slag met Basic en vervolgens converteren naar Standard en Premium als uw register gebruik toeneemt.

## <a name="sku-feature-matrix"></a>SKU-functie matrix

De volgende tabel bevat een overzicht van de functies en limieten van de service lagen Basic, Standard en Premium.

[!INCLUDE [container-instances-limits](../../includes/container-registry-limits.md)]

## <a name="changing-skus"></a>Sku's wijzigen

U kunt de SKU van een REGI ster wijzigen met de Azure CLI of in de Azure Portal. U kunt vrijelijk verplaatsen tussen beheerde Sku's zolang de SKU die u overschakelt, de vereiste maximale opslag capaciteit heeft. Wanneer u overschakelt naar een van de beheerde Sku's van klassiek, kunt u niet teruggaan naar klassiek. Dit is een eenrichtings conversie.

### <a name="azure-cli"></a>Azure-CLI

Als u wilt scha kelen tussen Sku's in de Azure CLI, gebruikt u de opdracht [AZ ACR update][az-acr-update] . Als u bijvoorbeeld wilt overschakelen op Premium:

```azurecli
az acr update --name myregistry --sku Premium
```

### <a name="azure-portal"></a>Azure Portal

Selecteer in het **overzicht** van het container register in de Azure Portal **Update**en selecteer vervolgens een nieuwe **SKU** in de vervolg keuzelijst SKU.

![SKU'S voor container Registry bijwerken in Azure Portal][update-registry-sku]

Als u een klassiek REGI ster hebt, kunt u geen beheerde SKU selecteren in het Azure Portal. In plaats daarvan moet u eerst een [upgrade uitvoeren][container-registry-upgrade] naar een beheerd REGI ster.

## <a name="pricing"></a>Prijzen

Zie [container Registry prijzen][container-registry-pricing]voor prijs informatie over elk van de Azure container Registry sku's.

Zie [prijs informatie voor band breedte](https://azure.microsoft.com/pricing/details/bandwidth/)voor meer informatie over de prijzen voor gegevens overdracht. 

## <a name="next-steps"></a>Volgende stappen

**Azure Container Registry-schema**

Ga naar het [ACR-plan][acr-roadmap] op github voor informatie over aanstaande functies in de service.

**Azure Container Registry UserVoice**

Bied en stem op nieuwe functie suggesties in [ACR UserVoice][container-registry-uservoice].

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
