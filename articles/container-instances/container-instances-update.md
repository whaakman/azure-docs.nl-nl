---
title: Update-containers in Azure Container Instances
description: Informatie over het bijwerken van de actieve containers in uw Azure Container Instances-container-groepen.
services: container-instances
author: mmacy
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 08/01/2018
ms.author: marsma
ms.openlocfilehash: 5a42b0983b0f754b119fa304317e758a976fb4f6
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39432613"
---
# <a name="update-containers-in-azure-container-instances"></a>Update-containers in Azure Container Instances

Tijdens normale werking van uw containerinstanties merkt u misschien het die nodig zijn voor het bijwerken van de containers in een containergroep. U wilt mogelijk bijwerken van versie van de installatiekopie, een DNS-naam wilt wijzigen, omgevingsvariabelen bijwerken of vernieuw de status van een container waarvan toepassing is gecrasht.

## <a name="update-a-container-group"></a>Bijwerken van een containergroep

De containers in een containergroep bijwerken door een bestaande groep met ten minste één aangepaste eigenschap opnieuw te implementeren. Wanneer u een containergroep bijwerkt, alle actieve containers in de groep opnieuw worden gestart in-place.

Opnieuw implementeren van een bestaande containergroep met behulp van de opdracht create (of gebruik de Azure-portal) en geef de naam van een bestaande groep. Ten minste één geldige eigenschap van de groep wijzigen wanneer u de opdracht maken voor het activeren van het opnieuw implementeren. Niet alle eigenschappen van container-groep zijn geldig voor opnieuw implementeren. Zie [eigenschappen waarvoor verwijderen](#properties-that-require-delete) voor een lijst van niet-ondersteunde eigenschappen.

De volgende Azure CLI-voorbeeld werkt bij een containergroep met een nieuwe DNS-naamlabel. Omdat de labeleigenschap van DNS-naam van de groep is gewijzigd, de containergroep is geïmplementeerd en de containers opnieuw gestart.

Voor de eerste implementatie met DNS-naamlabel *myapplication fasering*:

```azurecli-interactive
# Create container group
az container create --resource-group myResourceGroup --name mycontainer \
    --image nginx:alpine --dns-name-label myapplication-staging
```

Bijwerken van de containergroep met een nieuwe DNS-naamlabel *myapplication*:

```azurecli-interactive
# Update container group (restarts container)
az container create --resource-group myResourceGroup --name mycontainer \
    --image nginx:alpine --dns-name-label myapplication
```

## <a name="update-benefits"></a>Voordelen van de update

Het belangrijkste voordeel van het bijwerken van een bestaande containergroep is snellere implementatie. Wanneer u een bestaande containergroep implementeren, wordt de container-installatiekopie-lagen worden opgehaald uit die in cache opgeslagen door de vorige implementatie. In plaats van het binnenhalen van alle lagen van de afbeelding uit het register nieuwe zoals met nieuwe implementaties wordt uitgevoerd, worden alleen gewijzigde lagen (indien aanwezig) opgehaald.

Toepassingen op basis van grotere containerinstallatiekopieën, zoals Windows Server Core aanzienlijke verbetering van de snelheid van de implementatie zien kan wanneer u in plaats van bijwerken, verwijderen en nieuwe implementeren.

## <a name="limitations"></a>Beperkingen

Niet alle eigenschappen van een containergroep ondersteuning voor updates. Als u wilt wijzigen van bepaalde eigenschappen van een containergroep, moet u eerst verwijderen en opnieuw implementeren van de groep. Zie voor meer informatie, [eigenschappen waarvoor container verwijderen](#properties-that-require-container-delete).

Alle containers in een containergroep opnieuw opgestart tijdens het bijwerken van de containergroep. U kunt een update of een in-place opnieuw starten van een specifieke container niet uitvoeren in een groep met meerdere containers.

Het IP-adres van een container meestal tussen de updates niet gewijzigd, maar het is niet noodzakelijkerwijs gelijk blijven. Zolang de containergroep wordt geïmplementeerd naar dezelfde onderliggende host, wordt het IP-adres in de containergroep bewaard. Hoewel zeldzaam, en hoewel Azure Container Instances spant zich opnieuw te implementeren naar dezelfde host, zijn er enkele Azure-interne gebeurtenissen die leiden opnieuw implementeren naar een andere host tot kunnen. Risico's te beperken, moet u altijd een DNS-naamlabel gebruiken voor uw container instances.

Beëindigde of verwijderde containergroepen kunnen niet worden bijgewerkt. Zodra een containergroep is gestopt (is in de *beëindigd* staat) of is verwijderd, de groep wordt geïmplementeerd als nieuwe.

## <a name="properties-that-require-container-delete"></a>Eigenschappen waarvoor container verwijderen

Zoals eerder vermeld, kunnen niet alle groepseigenschappen van de container worden bijgewerkt. Bijvoorbeeld, als u wilt wijzigen van de poorten of beleid van een container voor opnieuw opstarten, moet u eerst de containergroep verwijderen en vervolgens opnieuw maken.

Deze eigenschappen vereisen container verwijderen voordat u opnieuw implementeren:

* Type besturingssysteem
* CPU
* Geheugen
* Beleid voor opnieuw opstarten
* Poorten

Wanneer u een containergroep verwijderen en opnieuw maken, is het niet "opnieuw geïmplementeerd", maar nieuwe gemaakt. Alle lagen van de afbeelding worden opgehaald als nieuwe uit het register, niet van deze cache door een eerdere implementatie. Het IP-adres van de container mogelijk ook wijzigen door op een andere onderliggende host worden geïmplementeerd.

## <a name="next-steps"></a>Volgende stappen

Vermeld meerdere keren in dit artikel is de **containergroep**. Elke container in Azure Container Instances is geïmplementeerd in een containergroep en containergroepen kunnen meer dan één container bevatten.

[Containergroepen in Azure Container Instances](container-instances-container-groups.md)

[Een groep met meerdere containers implementeren](container-instances-multi-container-group.md)

<!-- LINKS - External -->

<!-- LINKS - Internal -->
[az-container-create]: /cli/azure/container?view=azure-cli-latest#az-container-create
[az-container-logs]: /cli/azure/container?view=azure-cli-latest#az-container-logs
[az-container-show]: /cli/azure/container?view=azure-cli-latest#az-container-show
[azure-cli-install]: /cli/azure/install-azure-cli
