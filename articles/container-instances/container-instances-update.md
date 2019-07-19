---
title: Containers in Azure Container Instances bijwerken
description: Meer informatie over het bijwerken van actieve containers in uw Azure Container Instances-container groepen.
services: container-instances
author: dlepow
manager: gwallace
ms.service: container-instances
ms.topic: article
ms.date: 08/01/2018
ms.author: danlep
ms.openlocfilehash: d555ba6b8c2b32fc6ec56d6c51dda9626b6f0cb0
ms.sourcegitcommit: 4b431e86e47b6feb8ac6b61487f910c17a55d121
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/18/2019
ms.locfileid: "68325550"
---
# <a name="update-containers-in-azure-container-instances"></a>Containers in Azure Container Instances bijwerken

Tijdens de normale werking van uw container instanties is het wellicht nood zakelijk dat u de containers in een container groep bijwerkt. U kunt bijvoorbeeld de versie van de installatie kopie bijwerken, een DNS-naam wijzigen, omgevings variabelen bijwerken of de status van een container vernieuwen waarvan de toepassing is gecrasht.

## <a name="update-a-container-group"></a>Een container groep bijwerken

Werk de containers in een container groep bij door een bestaande groep opnieuw te implementeren met ten minste één gewijzigde eigenschap. Wanneer u een container groep bijwerkt, worden alle actieve containers in de groep op locatie opnieuw gestart.

Implementeer een bestaande container groep opnieuw door de opdracht Create uit te geven (of gebruik de Azure Portal) en geef de naam van een bestaande groep op. Wijzig ten minste één geldige eigenschap van de groep wanneer u de Create-opdracht geeft om de herimplementatie te activeren. Niet alle eigenschappen van de container groep zijn geldig voor opnieuw implementeren. Zie [eigenschappen waarvoor](#properties-that-require-container-delete) een lijst met niet-ondersteunde eigenschappen moet worden verwijderd.

In het volgende voor beeld van Azure CLI wordt een container groep bijgewerkt met een nieuwe DNS-naam label. Omdat de eigenschap van het DNS-naam label van de groep is gewijzigd, wordt de container groep opnieuw geïmplementeerd en opnieuw gestart.

Eerste implementatie met DNS-naam label *mijn toepassing-staging*:

```azurecli-interactive
# Create container group
az container create --resource-group myResourceGroup --name mycontainer \
    --image nginx:alpine --dns-name-label myapplication-staging
```

De container groep bijwerken met een nieuwe DNS-naam label, *mijn toepassing*:

```azurecli-interactive
# Update container group (restarts container)
az container create --resource-group myResourceGroup --name mycontainer \
    --image nginx:alpine --dns-name-label myapplication
```

## <a name="update-benefits"></a>Voor delen bijwerken

Het belangrijkste voor deel van het bijwerken van een bestaande container groep is een snellere implementatie. Wanneer u een bestaande container groep opnieuw implementeert, worden de bijbehorende container afbeeldings lagen opgehaald uit de caches van de vorige implementatie. In plaats van alle afbeeldings lagen uit het REGI ster te halen, zoals wordt uitgevoerd met nieuwe implementaties, worden alleen gewijzigde lagen (indien aanwezig) opgehaald.

Toepassingen die zijn gebaseerd op grotere container installatie kopieën, zoals Windows Server Core, kunnen aanzienlijke verbetering van de implementatie snelheid zien wanneer u een update in plaats van verwijderen en nieuwe implementeren.

## <a name="limitations"></a>Beperkingen

Niet alle eigenschappen van een container groep ondersteunen updates. Als u bepaalde eigenschappen van een container groep wilt wijzigen, moet u eerst de groep verwijderen en vervolgens opnieuw implementeren. Zie [eigenschappen waarvoor container verwijderen is vereist](#properties-that-require-container-delete)voor meer informatie.

Alle containers in een container groep worden opnieuw gestart wanneer u de container groep bijwerkt. Het is niet mogelijk om een update uit te voeren of een specifieke container in een groep met meerdere containers opnieuw op te starten.

Het IP-adres van een container verandert doorgaans niet tussen updates, maar het is niet gegarandeerd dat deze hetzelfde blijft. Zolang de container groep is geïmplementeerd op dezelfde onderliggende host, behoudt de container groep zijn IP-adres. Hoewel zeldzame, en terwijl Azure Container Instances elke moeite om op dezelfde host opnieuw te implementeren, zijn er enkele Azure-interne gebeurtenissen die de implementatie naar een andere host kunnen veroorzaken. Gebruik altijd een DNS-naam label voor uw container instanties om dit probleem te verhelpen.

Beëindigde of verwijderde container groepen kunnen niet worden bijgewerkt. Zodra een container groep is gestopt (de status is *beëindigd* ) of is verwijderd, wordt de groep geïmplementeerd als nieuw.

## <a name="properties-that-require-container-delete"></a>Eigenschappen waarvoor container verwijderen is vereist

Zoals eerder vermeld, kunnen niet alle eigenschappen van de container groep worden bijgewerkt. Als u bijvoorbeeld de poorten of het beleid voor het opnieuw opstarten van een container wilt wijzigen, moet u eerst de container groep verwijderen en vervolgens opnieuw maken.

Voor deze eigenschappen moet de container groep worden verwijderd voordat deze opnieuw kan worden geïmplementeerd:

* Type besturings systeem
* CPU
* Geheugen
* Beleid voor opnieuw opstarten
* Poorten

Wanneer u een container groep verwijdert en opnieuw maakt, wordt deze niet opnieuw geïmplementeerd, maar is er nieuwe gemaakt. Alle afbeeldings lagen worden vernieuwd uit het REGI ster, niet uit de caches van een vorige implementatie. Het IP-adres van de container kan ook worden gewijzigd omdat deze is geïmplementeerd op een andere onderliggende host.

## <a name="next-steps"></a>Volgende stappen

Dit artikel bevat meerdere keren de **container groep**. Elke container in Azure Container Instances wordt geïmplementeerd in een container groep en container groepen kunnen meer dan één container bevatten.

[Containergroepen in Azure Container Instances](container-instances-container-groups.md)

[Een groep met meerdere containers implementeren](container-instances-multi-container-group.md)

<!-- LINKS - External -->

<!-- LINKS - Internal -->
[az-container-create]: /cli/azure/container?view=azure-cli-latest#az-container-create
[az-container-logs]: /cli/azure/container?view=azure-cli-latest#az-container-logs
[az-container-show]: /cli/azure/container?view=azure-cli-latest#az-container-show
[azure-cli-install]: /cli/azure/install-azure-cli
