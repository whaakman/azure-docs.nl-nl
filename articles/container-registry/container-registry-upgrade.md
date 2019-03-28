---
title: Een klassieke Azure-containerregister upgraden
description: Profiteer van de uitgebreide reeks functies van Basic, Standard en Premium-containerregisters beheerd door uw niet-beheerde klassiek containerregister upgraden.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: article
ms.date: 03/26/2019
ms.author: danlep
ms.openlocfilehash: a5099feee34eb5497b68987485412e29ad5d5365
ms.sourcegitcommit: 6da4959d3a1ffcd8a781b709578668471ec6bf1b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/27/2019
ms.locfileid: "58521513"
---
# <a name="upgrade-a-classic-container-registry"></a>Een container klassiek register upgraden

Azure Container Registry (ACR) is beschikbaar in verschillende Servicelagen, [wel SKU's](container-registry-skus.md). De eerste release van de ACR aangeboden een enkel SKU klassiek, die beschikt niet over de verschillende functies documentatiesysteem dat in de Basic, Standard en Premium-SKU's (gezamenlijk bekend als *beheerd* registers).

De klassieke SKU wordt afgeschaft en zal na April 2019 niet beschikbaar zijn. Dit artikel wordt uitgelegd hoe u uw niet-beheerde Classic-register migreren naar een van de beheerde SKU's zodat u van hun uitgebreide functieset profiteren kunt.

## <a name="why-upgrade"></a>Waarom een upgrade uitvoeren?

De SKU wordt Classic-register **afgeschaft**, en is niet beschikbaar na **April 2019**. Alle bestaande klassieke registers moeten worden bijgewerkt vóór April 2019. Portal beheerfuncties van klassieke registers worden geëlimineerd. Het maken van nieuwe klassieke registers wordt uitgeschakeld na April 2019.

Vanwege de geplande buitengebruikstelling en beperkte mogelijkheden van klassieke niet-beheerde registers, moeten alle klassieke registers worden bijgewerkt naar beheerde registers (Basic, Standard of Premium). De Register integreren-SKU's deze op een hoger niveau dieper in de mogelijkheden van Azure. Zie voor meer informatie over de prijzen en mogelijkheden van de verschillende Servicelagen, [Container Registry-SKU's](container-registry-skus.md).

De Classic-register, is afhankelijk van het opslagaccount dat Azure automatisch in uw Azure-abonnement inricht bij het maken van het register. Daarentegen is de basis, standaard en Premium-SKU's profiteren van Azure [geavanceerde opslagfuncties](container-registry-storage.md) op transparante wijze de opslag van uw installatiekopieën voor u. Een afzonderlijk opslagaccount is niet gemaakt in uw eigen abonnement.

Beheerd register-opslag biedt de volgende voordelen:

* Containerinstallatiekopieën worden [in rust versleuteld met](container-registry-storage.md#encryption-at-rest).
* Installatiekopieën worden opgeslagen met behulp van [geografisch redundante opslag](container-registry-storage.md#geo-redundant-storage), ingeschakeld, back-up van uw afbeeldingen met replicatie voor meerdere regio's (alleen Premium-SKU).
* Mogelijkheid om naar eigen inzicht [verplaatsen tussen SKU's](container-registry-skus.md#changing-skus), hogere doorvoer inschakelen wanneer u een hogere SKU kiest. Met elke SKU kunt ACR voldoen aan de vereisten voor uw doorvoer naarmate uw behoeften toenemen.
* Geïntegreerde beveiligingsmodel voor het register en de opslag biedt vereenvoudigde rights management. U beheert machtigingen alleen voor de container registry, zonder te moeten ook machtigingen voor een afzonderlijk opslagaccount beheren.

Zie voor meer informatie over de installatiekopie van opslag in de ACR [afbeeldingopslag Container in Azure Container Registry](container-registry-storage.md).

## <a name="migration-considerations"></a>Overwegingen bij migraties

Wanneer u een klassiek register naar een beheerd register upgraden, moet Azure alle bestaande containerinstallatiekopieën uit het ACR gemaakte opslagaccount in uw abonnement kopiëren naar een beheerd door Azure storage-account. Dit proces kan enkele minuten tot enkele uren duren, afhankelijk van de grootte van het register. Naar schatting verwacht een migratietijd van ongeveer 0,5 GiB per minuut.

Tijdens de conversie `docker push` bewerkingen worden uitgeschakeld tijdens de laatste 10% van de migratie. `docker pull` blijft normaal functioneren.

Niet verwijderen of wijzigen van de inhoud van het opslagaccount dat een back-up uw Classic-register tijdens de conversie. In dat geval kan leiden tot beschadiging van uw containerinstallatiekopieën.

Nadat de migratie voltooid is, wordt het opslagaccount in uw abonnement die oorspronkelijk ondersteund uw Classic-register niet meer gebruikt door Azure Container Registry. Nadat u hebt gecontroleerd of de migratie gelukt is, kunt u het verwijderen van het storage-account om u te helpen de kosten minimaliseren.

>[!IMPORTANT]
> Een upgrade uitvoert van klassiek naar een van de beheerde SKU's is een **eenrichtingsproces**. Nadat u hebt een klassiek register naar Basic, Standard of Premium geconverteerd, kunt u niet terugkeren naar de klassieke. U kunt echter vrijelijk verplaatsen tussen beheerde SKU's met voldoende capaciteit voor uw register.

## <a name="how-to-upgrade"></a>Upgrade uitvoeren

U kunt een niet-beheerde klassiek register upgraden naar een van de beheerde SKU's op verschillende manieren. In de volgende secties beschrijven we de procedure voor het gebruik van de [Azure CLI] [ azure-cli] en de [Azure-portal][azure-portal].

## <a name="upgrade-in-azure-cli"></a>Upgrade uitvoeren in de Azure CLI

Als u een Classic-register in de Azure CLI bijwerken, uitvoeren van de [az acr update] [ az-acr-update] opdracht en geeft u de nieuwe SKU voor het register. In het volgende voorbeeld wordt een Classic-register met de naam *myclassicregistry* is bijgewerkt naar de Premium-SKU:

```azurecli-interactive
az acr update --name myclassicregistry --sku Premium
```

Wanneer de migratie voltooid is, ziet u uitvoer die vergelijkbaar is met de volgende. U ziet dat de `sku` "Premium" is en de `storageAccount` is `null`, waarmee wordt aangegeven dat Azure nu opslag wordt beheerd door de installatiekopie voor dit register.

```JSON
{
  "adminUserEnabled": false,
  "creationDate": "2017-12-12T21:23:29.300547+00:00",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresourcegroup/providers/Microsoft.ContainerRegistry/registries/myregistry",
  "location": "eastus",
  "loginServer": "myregistry.azurecr.io",
  "name": "myregistry",
  "provisioningState": "Succeeded",
  "resourceGroup": "myresourcegroup",
  "sku": {
    "name": "Premium",
    "tier": "Premium"
  },
  "status": null,
  "storageAccount": null,
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
}
```

Als u een beheerd register waarvan maximale capaciteit kleiner dan de grootte van uw register klassiek is SKU, ontvangt u een vergelijkbaar met het volgende foutbericht weergegeven.

`Cannot update the registry SKU due to reason: Registry size 12936251113 bytes exceeds the quota value 10737418240 bytes for SKU Basic. The suggested SKU is Standard.`

Als u een vergelijkbaar foutbericht ontvangt, voert u de [az acr update] [ az-acr-update] opdracht opnieuw uit en geeft u de voorgestelde SKU, die de volgende hoogste niveau SKU die kan worden aangepast aan uw installatiekopieën.

## <a name="upgrade-in-azure-portal"></a>Een upgrade uitvoert in Azure portal

Wanneer u een klassiek register upgraden met behulp van Azure portal, selecteert Azure automatisch de Standard- of Premium-SKU, afhankelijk van welke SKU kan worden aangepast aan uw installatiekopieën. Bijvoorbeeld, als uw register minder dan 100 GiB in afbeeldingen bevat, Azure automatisch geselecteerd en de Classic-register converteert naar Standard (100 GiB maximum).

Als u uw Classic-register bijwerken met behulp van Azure portal, gaat u naar het containerregister **overzicht** en selecteer **Upgrade naar beheerd register**.

![Klassiek register upgraden knop in de gebruikersinterface van Azure portal][update-classic-01-upgrade]

Selecteer **OK** om te bevestigen dat u wilt upgraden naar een beheerd register.

Tijdens de migratie, geeft de portal aan die van het register **Inrichtingsstatus** is *bijwerken*. Zoals eerder aangegeven, `docker push` bewerkingen worden uitgeschakeld tijdens de laatste 10% van de migratie. Niet delete of update het opslagaccount dat wordt gebruikt door het register klassiek terwijl de migratie uitgevoerd wordt, in dat geval kan leiden tot beschadiging van de installatiekopie.

![Klassiek register upgraden wordt uitgevoerd in de gebruikersinterface van Azure portal][update-classic-03-updating]

Wanneer de migratie voltooid is, de **Inrichtingsstatus** geeft aan dat *geslaagd*, en u kunt de normale bewerkingen hervatten met het register.

![Klassiek register upgraden voltooiingsstatus in de gebruikersinterface van Azure portal][update-classic-04-updated]

## <a name="next-steps"></a>Volgende stappen

Wanneer u een klassiek register hebt bijgewerkt naar een beheerd register, gebruikt Azure niet meer de storage-account dat een back-Classic-register oorspronkelijk gemaakt. Als u wilt kosten reduceren, houd rekening met het verwijderen van het storage-account of de Blob-container binnen het account dat uw oude containerinstallatiekopieën bevat.

<!-- IMAGES -->
[update-classic-01-upgrade]: ./media/container-registry-upgrade/update-classic-01-upgrade.png
[update-classic-02-confirm]: ./media/container-registry-upgrade/update-classic-02-confirm.png
[update-classic-03-updating]: ./media/container-registry-upgrade/update-classic-03-updating.png
[update-classic-04-updated]: ./media/container-registry-upgrade/update-classic-04-updated.png

<!-- LINKS - internal -->
[az-acr-update]: /cli/azure/acr#az-acr-update
[azure-cli]: /cli/azure/install-azure-cli
[azure-portal]: https://portal.azure.com