---
title: Azure-Container register SKU 's
description: "Vergelijking tussen de verschillende servicecategorieën beschikbaar in Azure Container register"
services: container-registry
author: stevelas
manager: timlt
ms.service: container-registry
ms.topic: article
ms.date: 10/16/2017
ms.author: stevelas
ms.openlocfilehash: cf7724dd9e3e870cfd7ec0b5d2e4ea1d0694e9de
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/05/2017
---
# <a name="azure-container-registry-skus"></a>Azure-Container register SKU 's

Azure Container register (ACR) is beschikbaar in meerdere Servicelagen SKU's genoemd. Deze SKU's bieden voorspelbare prijzen en verschillende opties voor hoe u wilt gebruiken van uw persoonlijke Docker-register in Azure. Als u een hogere SKU, kunt meer prestaties en schaalbaarheid. Alle SKU's bieden echter dezelfde programmatische mogelijkheden voor het inschakelen van een ontwikkelaar aan de slag met Basic en converteren naar Standard en Premium als register gebruik toeneemt.

## <a name="basic"></a>Basic
Een geoptimaliseerde kosten toegangspunt voor meer informatie over Azure-Container register ontwikkelaars. Basic registers hebben dezelfde programmatische mogelijkheden als Standard en Premium (integratie van Azure Active Directory-verificatie, verwijderen van een installatiekopie en web hooks), er zijn echter beperkingen voor de grootte en informatie over het gebruik.

## <a name="standard"></a>Standard
Standaard registers bieden dezelfde functionaliteit als basis, met hogere opslaglimieten en image-doorvoer. Standaard registers moeten voldoen aan de behoeften van de meeste scenario's voor productie.

## <a name="premium"></a>Premium
Premium-registers bieden hogere limieten van de beperkingen, zoals opslag en gelijktijdige bewerkingen, het inschakelen van grootschalige scenario's in. Naast de hogere doorvoercapaciteit van de installatiekopie, Premium profiteert u onder functies, zoals [geo-replicatie](container-registry-geo-replication.md) onderhouden voor het beheren van een enkele register over meerdere regio's, een netwerk sluit Register in op elke implementatie.

## <a name="classic"></a>Klassiek
Het register klassieke SKU ingeschakeld voor de initiële versie van de Azure-Container Registry-service in Azure. Klassieke registers worden ondersteund door een opslagaccount dat Azure in uw abonnement Hiermee beperkt u de mogelijkheid voor ACR maakt om een hoger niveau mogelijkheden, zoals verbeterde doorvoer en geo-replicatie. We zullen de klassieke SKU in de toekomst afschaffen vanwege de beperkte mogelijkheden.

> [!NOTE]
> Vanwege de geplande buitengebruikstelling van het register klassieke SKU, wordt u aangeraden dat u Basic, Standard of Premium gebruiken voor alle nieuwe registers. Zie voor meer informatie over het converteren van uw bestaande klassieke register [wijzigen SKU's](#changing-skus).
>

## <a name="registry-sku-feature-matrix"></a>Register SKU Functiematrix

De volgende tabel worden de functies en de ondergrenzen van de servicecategorieën Basic, Standard en Premium.

[!INCLUDE [container-instances-limits](../../includes/container-registry-limits.md)]

## <a name="manage-registry-size"></a>De registergrootte van het beheren
De beperkingen van de opslag van elke SKU zijn bedoeld om uitgelijnd met een typisch scenario: Basic voor aan de slag, standaard voor het merendeel van de apps in productie- en Premium voor hyperschaal prestaties en [geo-replicatie](container-registry-geo-replication.md). Tijdens de levensduur van het register, moet u de grootte beheren door regelmatig ongebruikte inhoud verwijderen.

U vindt het huidige gebruik van een register in het register van de container **overzicht** in de Azure portal:

![Informatie over het gebruik van register in de Azure portal](media/container-registry-skus/registry-overview-quotas.png)

U kunt de grootte van het register beheren door opslagplaatsen in de Azure portal te verwijderen.

Onder **SERVICES**, selecteer **opslagplaatsen**, klik met de rechtermuisknop op de opslagplaats die u wilt verwijderen en selecteer vervolgens **verwijderen**.

![Verwijderen van een opslagplaats in Azure portal](media/container-registry-skus/delete-repository-portal.png)

## <a name="changing-skus"></a>Het wijzigen van SKU 's

U kunt de SKU van het register in de Azure portal kunt wijzigen.

In het register **overzicht** in de Azure portal, selecteer **Update**, selecteer vervolgens een nieuwe **SKU** in de vervolgkeuzelijst SKU.

![Container register SKU in Azure-portal bijwerken](media/container-registry-skus/update-registry-sku.png)

## <a name="changing-from-classic"></a>Het wijzigen van klassiek
Wanneer u een klassiek register naar basis, standaard of Premium, Azure kopieën bestaande container installatiekopieën van het bijbehorende opslagaccount in uw abonnement naar een beheerd door Azure storage-account. Dit proces kan enige tijd duren.

Tijdens de conversie `docker pull` blijft werken, echter `docker push` wordt geblokkeerd tot de conversie is voltooid.

Wanneer het voltooid, wordt het opslagaccount van het abonnement niet langer door ACR gebruikt.

### <a name="why-change-from-classic-to-basic-standard-or-premium"></a>Waarom niet wijzigen van klassiek naar Basic, Standard of Premium?

Vanwege de beperkte mogelijkheden van klassieke registers, wordt u aangeraden dat u uw registers klassieke naar Basic, Standard of Premium-segment wijzigt. Het register deze op een hoger niveau SKU's meer diep geïntegreerd in de mogelijkheden van Azure. Sommige van deze mogelijkheden zijn:

* Azure Active Directory-integratie voor afzonderlijke verificatie (Zie [az acr aanmelding](/cli/azure/acr?view=azure-cli-latest#az_acr_login))
* Ondersteuning voor image en label verwijderen
* [Geo-replicatie](container-registry-geo-replication.md)
* [Webhooks.](container-registry-webhook.md)

Meest, een klassiek register hangt af van het opslagaccount dat Azure automatisch ingericht in uw Azure-abonnement bij het maken van het register. Daarentegen te profiteren van de Basic, Standard en Premium-SKU's *storage beheerd*. Dat wil zeggen beheerd Azure transparant de opslag van uw afbeeldingen voor u--is niet een afzonderlijke opslagaccount gemaakt in uw eigen abonnement.

Enkele van de voordelen van beheerde Basic, Standard en Premium registers opslag:

* Container afbeeldingen zijn [in rust versleuteld](../storage/common/storage-service-encryption.md).
* Installatiekopieën worden opgeslagen met behulp van [geografisch redundante opslag](../storage/common/storage-redundancy.md#geo-redundant-storage), zodat de zeker back-up van uw afbeeldingen met meerdere landen/regio-replicatie.
* Mogelijkheid om te [doorlopen SKU's](#changing-skus), hogere doorvoer inschakelen wanneer u ervoor een hogere SKU kiest. Met elke SKU ACR kunt doorvoer aan uw eisen voldoen als verhogen voor uw behoeften. De onderliggende implementatie van hoe ACR bereikt voor de gewenste doorvoer wordt uitgedrukt als *bedoeling* (door selecteren hogere SKU's), zonder dat u hoeft te beheren, de details van de implementatie.

## <a name="pricing"></a>Prijzen

Zie voor informatie over elk van de Azure-Container register SKU's prijzen [Container register prijzen](https://azure.microsoft.com/pricing/details/container-registry/).

## <a name="next-steps"></a>Volgende stappen

**Azure-Container register Roadmap**

Ga naar de [ACR Roadmap](https://aka.ms/acr/roadmap) op GitHub voor meer informatie over nieuwe functies in de service.

**Azure-Container register UserVoice**

Verzenden en hierop stemmen op nieuwe suggesties voor functies in [ACR UserVoice](https://feedback.azure.com/forums/903958-azure-container-registry).