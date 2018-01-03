---
title: Upgrade van een klassieke Azure-container-register
description: Profiteren van de uitgevouwen functieset van Basic, Standard en Premium container registers beheerd door uw niet-beheerde klassieke container register upgraden.
services: container-registry
author: mmacy
manager: timlt
ms.service: container-registry
ms.topic: article
ms.date: 12/20/2017
ms.author: marsma
ms.openlocfilehash: 19090bb69d7165c1e904450dc93b925e23e44782
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="upgrade-a-classic-container-registry"></a>Upgrade van een container klassieke register

Azure Container register (ACR) is beschikbaar in verschillende Servicelagen, [bekend als SKU's](container-registry-skus.md). De eerste release van ACR aangeboden een enkele SKU klassieke die beschikt niet over de verschillende functies die deel uitmaken van de Basic, Standard en Premium-SKU's (gezamenlijk bekend als *beheerd* registers). In dit artikel wordt het niet-beheerde klassieke register migreren naar een van de beheerde SKU's zodat u van hun verbeterde functieset profiteren kunt.

## <a name="why-upgrade"></a>Waarom upgraden?

U wordt aangeraden dat alle klassieke registers worden bijgewerkt naar Basic, Standard of Premium beheerde registers vanwege de beperkte mogelijkheden van registers klassieke zonder begeleiding. Het register deze op een hoger niveau SKU's meer diep geïntegreerd in de mogelijkheden van Azure.

Beheerde registers bieden:

* Azure Active Directory-integratie voor [afzonderlijke aanmelding](container-registry-authentication.md#individual-login-with-azure-ad)
* Ondersteuning voor image en label verwijderen
* [Geo-replicatie](container-registry-geo-replication.md)
* [Webhooks.](container-registry-webhook.md)

Meest, een klassiek register hangt af van het opslagaccount dat Azure automatisch ingericht in uw Azure-abonnement bij het maken van het register. Daarentegen te profiteren van de Basic, Standard en Premium-SKU's *storage beheerd*. Dat wil zeggen beheerd Azure transparant de opslag van uw afbeeldingen voor u--is niet een afzonderlijke opslagaccount gemaakt in uw eigen abonnement.

Beheerde register opslag biedt de volgende voordelen:

* Container afbeeldingen zijn [in rust versleuteld](../storage/common/storage-service-encryption.md).
* Installatiekopieën worden opgeslagen met behulp van [geografisch redundante opslag](../storage/common/storage-redundancy.md#geo-redundant-storage), zodat de zeker back-up van uw afbeeldingen met meerdere landen/regio-replicatie.
* Mogelijkheid om te vrijelijk [verplaatsen tussen SKU's](container-registry-skus.md#changing-skus), hogere doorvoer inschakelen wanneer u ervoor een hogere SKU kiest. Met elke SKU ACR kunt doorvoer aan uw eisen voldoen als verhogen voor uw behoeften.
* Geïntegreerde beveiligingsmodel voor het register en bijbehorende opslag biedt vereenvoudigde rights management. U beheren machtigingen alleen voor het register container zonder dat ook om machtigingen te beheren voor een afzonderlijke opslagaccount.

## <a name="migration-considerations"></a>Overwegingen bij migratie

Wanneer u een klassiek register met een beheerde register wijzigt, moet Azure alle bestaande container installatiekopieën van het opslagaccount ACR gemaakt in uw abonnement kopiëren naar een beheerd door Azure storage-account. Afhankelijk van de grootte van het register kan dit proces enkele minuten tot enkele uren duren.

Tijdens het conversieproces alle `docker push` bewerkingen worden geblokkeerd, terwijl `docker pull` blijft werken.

Verwijder of wijzig de inhoud van het opslagaccount back-ups van het register klassieke tijdens de conversie niet. In dat geval kan leiden tot beschadiging van de container afbeeldingen.

Zodra de migratie voltooid is, wordt het opslagaccount in uw abonnement die oorspronkelijk ondersteund uw register klassieke meer gebruikt door ACR. Nadat u hebt geverifieerd dat de migratie geslaagd is, moet u overwegen verwijderen van opslagaccount te helpen minimaliseren kosten.

>[!IMPORTANT]
> Upgraden van klassiek naar een van de beheerde SKU's is een **onomkeerbaar**. Nadat u hebt een klassiek register in op Basic, Standard of Premium geconverteerd, kunt u niet terugkeren naar de klassieke. U kunt echter vrijelijk verplaatsen tussen beheerde SKU's met voldoende capaciteit voor het register.

## <a name="how-to-upgrade"></a>Bijwerken

U kunt een niet-beheerde klassieke registry upgraden naar een van de beheerde SKU's op verschillende manieren. In de volgende secties beschrijven we de procedure voor het gebruik van de [Azure CLI] [ azure-cli] en de [Azure-portal][azure-portal].

## <a name="upgrade-in-azure-cli"></a>Upgrade uitvoeren in Azure CLI

Uitvoeren als u wilt upgraden een klassiek register in de Azure CLI, de [az acr update] [ az-acr-update] opdracht en geeft u de nieuwe SKU van het register. In het volgende voorbeeld wordt een klassiek register met de naam *myclassicregistry* is bijgewerkt naar de Premium-SKU:

```azurecli-interactive
az acr update --name myclassicregistry --sku Premium
```

Wanneer de migratie voltooid is, ziet u uitvoer ziet er als volgt. U ziet dat de `sku` ' Premium ' en de `storageAccount` ' null ' waarmee wordt aangegeven dat Azure nu de installatiekopie van opslag voor deze registersleutel beheert.

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

Als u een beheerde register SKU waarvan maximale capaciteit kleiner dan de grootte van het register klassieke is opgeeft, ontvangt u een vergelijkbaar met het volgende foutbericht weergegeven.

`Cannot update the registry SKU due to reason: Registry size 12936251113 bytes exceeds the quota value 10737418240 bytes for SKU Basic. The suggested SKU is Standard.`

Als u een vergelijkbare foutbericht ontvangt, voert u de [az acr update] [ az-acr-update] opdracht opnieuw en geef de voorgestelde SKU de SKU van de volgende hoogste niveau die kan worden aangepast aan uw afbeeldingen.

## <a name="upgrade-in-azure-portal"></a>Upgrade uitvoeren in Azure-portal

Wanneer u een upgrade uitvoert van een klassiek register met behulp van de Azure-portal, selecteert Azure automatisch het laagste niveau SKU die kan worden aangepast aan uw afbeeldingen. Bijvoorbeeld, als het register 12 GiB in afbeeldingen bevat, Azure automatisch geselecteerd en het register klassieke converteert naar Standard (maximaal van 100 GiB).

Als u wilt uw register klassieke upgraden met behulp van de Azure-portal, gaat u naar het register van de container **overzicht** en selecteer **Upgrade naar beheerde register**.

![Klassieke register upgrade knop in de gebruikersinterface van de Azure portal][update-classic-01-upgrade]

Selecteer **OK** om te bevestigen dat u wilt upgraden naar een beheerde register.

![Klassieke register upgrade bevestiging in de gebruikersinterface van de Azure portal][update-classic-02-confirm]

Tijdens de migratie de portal aan te geven dat het register **Inrichtingsstatus** is *Updating*. Zoals eerder gezegd `docker push` zijn uitgeschakeld tijdens de migratie en moet u niet verwijderen of update het storage-account door het register klassieke gebruikt terwijl de migratie uitgevoerd wordt--in dat geval kan leiden tot beschadiging van de installatiekopie.

![Klassieke register voortgang in de gebruikersinterface van de Azure portal bijwerken][update-classic-03-updating]

Als de migratie voltooid is, de **Inrichtingsstatus** geeft *geslaagd*, en kunt u nog een keer `docker push` toe aan het register.

![Klassieke register upgrade voltooiingsstatus in de gebruikersinterface van de Azure portal][update-classic-04-updated]

## <a name="next-steps"></a>Volgende stappen

Zodra u een klassiek register in op Basic, Standard of Premium hebt bijgewerkt, maakt gebruik van Azure niet langer de storage-account dat het register klassieke oorspronkelijk ondersteund. Als u kosten, kunt u eventueel het opslagaccount of de Blob-container binnen het account dat uw oude installatiekopieën van de container bevat verwijderen.

<!-- IMAGES -->
[update-classic-01-upgrade]: ./media/container-registry-upgrade\update-classic-01-upgrade.png
[update-classic-02-confirm]: ./media/container-registry-upgrade\update-classic-02-confirm.png
[update-classic-03-updating]: ./media/container-registry-upgrade\update-classic-03-updating.png
[update-classic-04-updated]: ./media/container-registry-upgrade\update-classic-04-updated.png

<!-- LINKS - internal -->
[az-acr-update]: /cli/azure/acr#az_acr_update
[azure-cli]: /cli/azure/install-azure-cli
[azure-portal]: https://portal.azure.com