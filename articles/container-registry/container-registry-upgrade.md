---
title: Een klassiek Azure container Registry bijwerken
description: Profiteer van de uitgebreide functieset basis, standaard en Premium beheerde container registers door uw onbeheerde klassieke container Registry te upgraden.
services: container-registry
author: dlepow
manager: gwallace
ms.service: container-registry
ms.topic: article
ms.date: 03/26/2019
ms.author: danlep
ms.openlocfilehash: 05c227e7de078c6bb371049f16e191598b9ca4e5
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68310383"
---
# <a name="upgrade-a-classic-container-registry"></a>Een klassiek container register bijwerken

Azure Container Registry (ACR) is beschikbaar in verschillende service lagen, [ook wel sku's genoemd](container-registry-skus.md). De eerste versie van ACR bood een enkele SKU, klassiek, die geen verschillende functies bevat die inherent zijn aan de Basic-, Standard-en Premium-Sku's (gezamenlijk bekend als *beheerde* registers).

De klassieke SKU wordt afgeschaft en is na 2019 april niet meer beschikbaar. In dit artikel wordt beschreven hoe u uw onbeheerde klassieke REGI ster kunt migreren naar een van de beheerde Sku's zodat u de uitgebreide functies van de set optimaal benut.

## <a name="why-upgrade"></a>Waarom bijwerken?

De klassieke register-SKU wordt **afgeschaft**en zal na **2019 april**niet meer beschikbaar zijn. Alle bestaande klassieke registers moeten worden bijgewerkt vóór 2019 april. De functies voor portal beheer van klassieke registers worden in de fase uitgeoefend. Het maken van nieuwe klassieke registers wordt uitgeschakeld na 2019 april.

Vanwege de geplande afschaffing en beperkte mogelijkheden van klassieke onbeheerde registers moeten alle klassieke registers worden bijgewerkt naar beheerde registers (Basic, Standard of Premium). Deze hogere Sku's hebben meer dieper het REGI ster integreren in de mogelijkheden van Azure. Zie [container Registry sku's](container-registry-skus.md)voor meer informatie over de prijzen en mogelijkheden van de verschillende service lagen.

Het klassieke REGI ster is afhankelijk van het opslag account dat door Azure automatisch wordt ingericht in uw Azure-abonnement wanneer u het REGI ster maakt. Daarentegen profiteren de basis-, standaard-en Premium-Sku's van de [Geavanceerde opslag functies](container-registry-storage.md) van Azure door de opslag van uw installatie kopieën transparant te verwerken. Er is geen afzonderlijk opslag account gemaakt in uw eigen abonnement.

Beheerde register opslag biedt de volgende voor delen:

* Container installatie kopieën worden [op rest](container-registry-storage.md#encryption-at-rest)versleuteld.
* Installatie kopieën worden opgeslagen met [geo-redundante opslag](container-registry-storage.md#geo-redundant-storage), zodat er een back-up van uw installatie kopieën wordt gemaakt met replicatie met meerdere regio's (alleen Premium-SKU).
* Mogelijkheid om vrij te [scha kelen tussen sku's](container-registry-skus.md#changing-skus), waardoor een hogere door Voer wordt ingeschakeld wanneer u een SKU van een hoger niveau kiest. Met elke SKU kunt ACR voldoen aan uw doorvoer vereisten naarmate uw behoeften toenemen.
* Het uniforme beveiligings model voor het REGI ster en de opslag biedt vereenvoudigd Rights Management. U beheert alleen de machtigingen voor het container register, zonder dat u ook machtigingen voor een afzonderlijk opslag account hoeft te beheren.

Zie [opslag van container installatie kopieën in azure container Registry](container-registry-storage.md)voor meer informatie over installatie kopie opslag in ACR.

## <a name="migration-considerations"></a>Overwegingen bij migraties

Wanneer u een klassiek REGI ster bijwerkt naar een beheerd REGI ster, moet Azure alle bestaande container installatie kopieën van het met ACR gemaakte opslag account in uw abonnement kopiëren naar een opslag account dat wordt beheerd door Azure. Afhankelijk van de grootte van uw REGI ster kan dit proces enkele minuten tot enkele uren duren. Voor schattingen wordt een migratie tijd van ongeveer 0,5 GiB per minuut verwacht.

Tijdens het conversie proces `docker push` worden bewerkingen uitgeschakeld tijdens de laatste 10% van de migratie. `docker pull`blijft normaal functioneren.

Verwijder of wijzig de inhoud van het opslag account dat tijdens het conversie proces een back-up maakt van uw klassieke REGI ster niet. Dit kan leiden tot beschadiging van de container installatie kopieën.

Zodra de migratie is voltooid, wordt het opslag account in uw abonnement dat oorspronkelijk is ondersteund voor uw klassieke REGI ster, niet meer gebruikt door Azure Container Registry. Nadat u hebt gecontroleerd of de migratie is geslaagd, kunt u het opslag account verwijderen om de kosten te minimaliseren.

>[!IMPORTANT]
> Een upgrade uitvoeren van klassiek naar een van de beheerde Sku's is een **eenrichtings proces**. Wanneer u een klassiek REGI ster hebt geconverteerd naar Basic, Standard of Premium, kunt u niet terugkeren naar klassiek. U kunt echter vrij wisselen tussen beheerde Sku's met voldoende capaciteit voor het REGI ster.

## <a name="how-to-upgrade"></a>Een upgrade uitvoeren

U kunt op verschillende manieren een onbeheerd klassiek REGI ster upgraden naar een van de beheerde Sku's. In de volgende secties beschrijven we het proces voor het gebruik van de [Azure cli][azure-cli] en de [Azure Portal][azure-portal].

## <a name="upgrade-in-azure-cli"></a>Upgrade uitvoeren in azure CLI

Als u een klassiek REGI ster in de Azure CLI wilt bijwerken, voert u de opdracht [AZ ACR update][az-acr-update] uit en geeft u de nieuwe SKU voor het REGI ster op. In het volgende voor beeld wordt een klassiek REGI ster met de naam *myclassicregistry* bijgewerkt naar de Premium-SKU:

```azurecli-interactive
az acr update --name myclassicregistry --sku Premium
```

Wanneer de migratie is voltooid, ziet de uitvoer er ongeveer als volgt uit. U ziet dat `sku` de is ' Premium ' en `storageAccount` de `null`' is ', wat aangeeft dat Azure de installatie kopie opslag voor dit REGI ster nu beheert.

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

Als u een beheerde register-SKU opgeeft waarvan de maximum capaciteit kleiner is dan de grootte van het klassieke REGI ster, wordt een fout bericht van de volgende strekking weer gegeven.

`Cannot update the registry SKU due to reason: Registry size 12936251113 bytes exceeds the quota value 10737418240 bytes for SKU Basic. The suggested SKU is Standard.`

Als er een vergelijk bare fout wordt weer gegeven, voert u de opdracht [AZ ACR update][az-acr-update] opnieuw uit en geeft u de voorgestelde SKU op. Dit is de Next-Level-SKU die uw installatie kopieën kan bevatten.

## <a name="upgrade-in-azure-portal"></a>Upgrade in Azure Portal

Wanneer u een klassiek REGI ster bijwerkt met behulp van de Azure Portal, selecteert Azure automatisch de standaard-of Premium-SKU, afhankelijk van welke SKU uw installatie kopieën kan bevatten. Als uw REGI ster bijvoorbeeld minder dan 100 GiB in installatie kopieën bevat, wordt het klassieke REGI ster automatisch door Azure geselecteerd en omgezet naar standaard (100 GiB maximum).

Als u uw klassieke REGI ster wilt bijwerken met behulp van de Azure Portal, gaat u naar het **overzicht** van het container register en selecteert **u upgraden naar beheerd REGI ster**.

![De knop klassieke register upgrade in de gebruikers interface van Azure Portal][update-classic-01-upgrade]

Selecteer **OK** om te bevestigen dat u wilt upgraden naar een beheerd REGI ster.

Tijdens de migratie geeft de portal aan dat de inrichtings **status** van het REGI ster wordt *bijgewerkt*. Zoals eerder vermeld, `docker push` worden bewerkingen uitgeschakeld tijdens de laatste 10% van de migratie. Verwijder of werk het opslag account dat wordt gebruikt door het klassieke REGI ster niet tijdens het uitvoeren van de migratie. Dit kan leiden tot beschadiging van de installatie kopie.

![Voortgang van de upgrade van het klassieke REGI ster in de Azure Portal-gebruikers interface][update-classic-03-updating]

Wanneer de migratie is voltooid, wordt de inrichtings **status** *geslaagd*en kunt u de normale bewerkingen hervatten met het REGI ster.

![Voltooiings status van de klassieke register upgrade in de gebruikers interface van Azure Portal][update-classic-04-updated]

## <a name="next-steps"></a>Volgende stappen

Wanneer u een klassiek REGI ster hebt bijgewerkt naar een beheerd REGI ster, gebruikt Azure niet langer het opslag account dat oorspronkelijk een back-up heeft gemaakt van het klassieke REGI ster. U kunt de kosten verlagen door het opslag account of de BLOB-container te verwijderen binnen het account dat uw oude container installatie kopieën bevat.

<!-- IMAGES -->
[update-classic-01-upgrade]: ./media/container-registry-upgrade/update-classic-01-upgrade.png
[update-classic-02-confirm]: ./media/container-registry-upgrade/update-classic-02-confirm.png
[update-classic-03-updating]: ./media/container-registry-upgrade/update-classic-03-updating.png
[update-classic-04-updated]: ./media/container-registry-upgrade/update-classic-04-updated.png

<!-- LINKS - internal -->
[az-acr-update]: /cli/azure/acr#az-acr-update
[azure-cli]: /cli/azure/install-azure-cli
[azure-portal]: https://portal.azure.com