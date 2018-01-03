---
title: Azure-Container register SKU 's
description: "Vergelijk de verschillende servicecategorieën beschikbaar in Azure Container register."
services: container-registry
author: mmacy
manager: timlt
ms.service: container-registry
ms.topic: article
ms.date: 12/20/2017
ms.author: marsma
ms.openlocfilehash: 15179fa3e3567f92a5eae69ba9a684addc3138dd
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2017
---
# <a name="azure-container-registry-skus"></a>Azure-Container register SKU 's

Azure Container register (ACR) is beschikbaar in meerdere Servicelagen SKU's genoemd. Deze SKU's bieden voorspelbare prijzen en verschillende opties voor het uitlijnen van de capaciteit en gebruikspatronen van uw persoonlijke Docker-registratie in Azure.

| SKU | Managed | Beschrijving |
| --- | :-------: | ----------- |
| **Basic** | Ja | Een geoptimaliseerde kosten toegangspunt voor meer informatie over Azure-Container register ontwikkelaars. Basic registers hebben dezelfde programmatische mogelijkheden als Standard en Premium (integratie van Azure Active Directory-verificatie, verwijderen van een installatiekopie en web hooks), er zijn echter beperkingen voor de grootte en informatie over het gebruik. |
| **Standard** | Ja | Standaard registers bieden dezelfde functionaliteit als basis, met hogere opslaglimieten en image-doorvoer. Standaard registers moeten voldoen aan de behoeften van de meeste scenario's voor productie. |
| **Premium** | Ja | Premium-registers bieden hogere limieten van de beperkingen, zoals opslag en gelijktijdige bewerkingen, het inschakelen van grootschalige scenario's in. Naast de hogere doorvoercapaciteit van de installatiekopie, Premium profiteert u onder functies, zoals [geo-replicatie] [ container-registry-geo-replication] voor het beheren van een enkele register over meerdere regio's, onderhouden van een netwerk sluit register aan elk de implementatie. |
| Klassiek | Nee | Het register klassieke SKU ingeschakeld voor de initiële versie van de Azure-Container Registry-service in Azure. Klassieke registers worden ondersteund door een opslagaccount dat Azure in uw abonnement Hiermee beperkt u de mogelijkheid voor ACR maakt om een hoger niveau mogelijkheden, zoals verbeterde doorvoer en geo-replicatie. We zullen de klassieke SKU in de toekomst afschaffen vanwege de beperkte mogelijkheden. |

U kiest dat een hogere SKU biedt meer prestaties en schaalbaarheid, bieden alle beheerde SKU's echter dezelfde programmatische mogelijkheden. Met meerdere Servicelagen, kunt u aan de slag kunt met Basic en vervolgens converteren naar Standard en Premium als uw register gebruik toeneemt.

> [!NOTE]
> Vanwege de geplande buitengebruikstelling van het register klassieke SKU, wordt u aangeraden dat u Basic, Standard of Premium gebruiken voor alle nieuwe registers. Zie voor meer informatie over het converteren van uw bestaande klassieke register [upgraden van een klassiek register][container-registry-upgrade].
>

## <a name="managed-vs-unmanaged"></a>Beheerde versus onbeheerde

Het Basic, Standard en Premium-SKU's gezamenlijk worden aangeduid als *beheerd* registries en klassieke registers als *onbeheerde*. Het belangrijkste verschil tussen de twee is hoe uw container-installatiekopieën worden opgeslagen.

### <a name="managed-basic-standard-premium"></a>Beheerd (Basic, Standard, Premium)

Beheerde registers worden ondersteund door een Azure Storage-account die door Azure worden beheerd. Dat wil zeggen, het opslagaccount dat u uw installatiekopieën worden opgeslagen niet wordt weergegeven in uw Azure-abonnement. Er zijn diverse voordelen die is opgedaan met behulp van een van de beheerde register SKU's, die worden beschreven in gedetailleerde [upgraden van een klassiek register][container-registry-upgrade]. Dit artikel is gericht op de beheerde register SKU's en hun mogelijkheden.

### <a name="unmanaged-classic"></a>Niet-beheerde (klassiek)

Klassieke registers zijn 'zonder begeleiding' in de zin dat het opslagaccount dat een klassiek register een back-up zich bevindt binnen *uw* Azure-abonnement. Hierdoor bent u verantwoordelijk voor het beheer van het opslagaccount waarin de container-installatiekopieën worden opgeslagen. Met niet-beheerde registers u tussen SKU's niet overschakelen naar behoefte (anders dan [upgraden] [ container-registry-upgrade] met een beheerde register), en de verschillende functies van beheerde registers zijn niet beschikbaar (bijvoorbeeld: verwijdering van de afbeelding container, [geo-replicatie] [ container-registry-geo-replication] en [webhooks][container-registry-webhook]).

Zie voor meer informatie over het upgraden van een klassiek register op een van de beheerde SKU's [upgraden van een klassiek register][container-registry-upgrade].

## <a name="sku-feature-matrix"></a>Matrix van SKU-functie

De volgende tabel worden de functies en de ondergrenzen van de servicecategorieën Basic, Standard en Premium.

[!INCLUDE [container-instances-limits](../../includes/container-registry-limits.md)]

## <a name="changing-skus"></a>Het wijzigen van SKU 's

U kunt de SKU van het register met de Azure CLI of in de Azure-portal wijzigen. U kan bewegen tussen beheerde SKU's zolang de SKU u naar omschakelt de vereiste maximale capaciteit heeft. Als u overschakelt naar een van de beheerde SKU's van klassieke, verplaatsen u niet terug naar de klassieke--is een eenzijdige conversie.

### <a name="azure-cli"></a>Azure-CLI

Als u wilt verplaatsen tussen SKU's in de Azure CLI, gebruikt u de [az acr update] [ az-acr-update] opdracht. Als u bijvoorbeeld overschakelen naar Premium:

```azurecli
az acr update --name myregistry --sku Premium
```

### <a name="azure-portal"></a>Azure Portal

In het register van de container **overzicht** in de Azure portal, selecteer **Update**, selecteer vervolgens een nieuwe **SKU** in de vervolgkeuzelijst SKU.

![Container register SKU in Azure-portal bijwerken][update-registry-sku]

Als u een klassiek register hebt, kunt u een beheerde SKU binnen de Azure-portal niet selecteren. In plaats daarvan moet u eerst [upgrade] [ container-registry-upgrade] met een beheerde register (Zie [wijzigen van klassieke](#changing-from-classic)).

## <a name="changing-from-classic"></a>Het wijzigen van klassiek

Er zijn aanvullende overwegingen rekening te houden bij het migreren van een niet-beheerde klassieke register op een van de beheerde Basic, Standard of Premium-SKU's. Als uw register klassieke een groot aantal afbeeldingen bevat en vele gigabytes, kan het migratieproces enige tijd duren. Bovendien `docker push` bewerkingen zijn uitgeschakeld totdat de migratie voltooid is.

Zie voor meer informatie over het bijwerken van het register klassiek naar een van de beheerde SKU's [upgraden van een container klassieke register][container-registry-upgrade].

## <a name="pricing"></a>Prijzen

Zie voor informatie over elk van de Azure-Container register SKU's prijzen [Container register prijzen][container-registry-pricing].

## <a name="next-steps"></a>Volgende stappen

**Azure-Container register Roadmap**

Ga naar de [ACR Roadmap] [ acr-roadmap] op GitHub voor meer informatie over nieuwe functies in de service.

**Azure-Container register UserVoice**

Verzenden en hierop stemmen op nieuwe suggesties voor functies in [ACR UserVoice][container-registry-uservoice].

<!-- IMAGES -->
[update-registry-sku]: ./media/container-registry-skus/update-registry-sku.png

<!-- LINKS - External -->
[acr-roadmap]: https://aka.ms/acr/roadmap
[container-registry-pricing]: https://azure.microsoft.com/pricing/details/container-registry/
[container-registry-uservoice]: https://feedback.azure.com/forums/903958-azure-container-registry

<!-- LINKS - Internal -->
[az-acr-update]: /cli/azure/acr#az_acr_update
[container-registry-geo-replication]: container-registry-geo-replication.md
[container-registry-upgrade]: container-registry-upgrade.md
[container-registry-webhook]: container-registry-webhook.md
