---
title: Aanbevolen procedures voor Azure Container Registry
description: Leer hoe u Azure Container Registry effectief gebruikt door deze aanbevolen procedures te volgen.
services: container-registry
author: mmacy
manager: timlt
ms.service: container-registry
ms.topic: quickstart
ms.date: 11/05/2017
ms.author: marsma
ms.openlocfilehash: 5ccbb3022dc38f13eed9b5aa24beb14dfdb3b5b6
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/05/2017
---
# <a name="best-practices-for-azure-container-registry"></a>Aanbevolen procedures voor Azure Container Registry

Door deze aanbevolen procedures te volgen, kunt u de prestaties en het rendabele gebruik van uw privé-Docker-register in Azure maximaliseren.

## <a name="network-close-deployment"></a>Implementatie dichtbij het netwerk

Maak uw containerregister in dezelfde Azure-regio waar u containers implementeert. Door uw register te plaatsen in een regio die zich dichtbij het netwerk bevindt, kunnen de hosts van uw container uw helpen zowel de latentie als de kosten te verlagen.

Implementatie dichtbij het netwerk is een van de belangrijkste redenen om een privé containerregister te gebruiken. Docker-installatiekopieën hebben een uitstekende [gelaagde constructie](https://docs.docker.com/engine/userguide/storagedriver/imagesandcontainers/) die incrementele implementaties mogelijk maken. Nieuwe knooppunten moeten echter alle lagen ophalen die nodig zijn voor een bepaalde installatiekopie. Deze initiële `docker pull` kan al gauw meerdere gigabytes bedragen. Met een privéregister dichtbij uw implementatie wordt de netwerklatentie tot een minimum teruggebracht.
Bovendien brengen alle openbare clouds, waaronder Azure, netwerkkosten voor uitgaande gegevens in rekening. Het binnenhalen van installatiekopieën van het ene datacenter in het andere, verhoogt behalve de latentie ook de netwerkkosten voor uitgaande gegevens.

## <a name="geo-replicate-multi-region-deployments"></a>Geo-replicatie voor implementaties in meerdere regio’s

Gebruik de functie voor [geo-replicatie](container-registry-geo-replication.md) in Azure Container Registry om containers naar meerdere regio's te implementeren. Of u nu internationale klanten vanuit lokale datacentra helpt of uw dat uw ontwikkelteam zich op verschillende locaties bevindt, u kunt uw registerbeheer vereenvoudigen en latentie minimaliseren door geo-replicatie op uw register toe te passen. Een preview-versie van deze functie is momenteel beschikbaar voor [Premium](container-registry-skus.md#premium)-registers.

Zie de driedelige zelfstudie [Geo-replicatie in Azure Container Registry](container-registry-tutorial-prepare-registry.md) voor informatie over het gebruik van geo-replicatie.

## <a name="repository-namespaces"></a>Opslagplaatsnaamruimten

Dankzij het gebruik van opslagplaatsnaamruimten kunt u toestaan dat een enkel register tussen meerdere groepen binnen uw organisatie kan worden gedeeld. Registers kunnen worden gedeeld tussen implementaties en teams. Azure Container Registry biedt ondersteuning voor geneste naamruimten, waardoor met geïsoleerde groepen kan worden gewerkt.

Neem bijvoorbeeld de volgende container installatiekopielabels in overweging. Installatiekopieën die in het hele bedrijf worden gebruikt, zoals `aspnetcore`, worden in de hoofdnaamruimte geplaatst, terwijl containerinstallatiekopieën die eigendom zijn van de productie- of marketinggroepen hun eigen naamruimten gebruiken.

```
contoso.azurecr.io/aspnetcore:2.0
contoso.azurecr.io/products/widget/web:1
contoso.azurecr.io/products/bettermousetrap/refundapi:12.3
contoso.azurecr.io/marketing/2017-fall/concertpromotions/campaign:218.42
```

## <a name="dedicated-resource-group"></a>Toegewezen resourcegroep

Omdat containerregisters resources zijn die op meerdere containerhosts worden gebruikt, moet een resourcegroep een eigen register bevatten.

Hoewel u mogelijk aan het experimenteren bent met een specifiek type host, zoals Azure Container Instances, wilt u de containerinstantie waarschijnlijk verwijderen wanneer u klaar bent. Maar misschien wilt u de verzameling installatiekopieën die u naar Azure Container Registry hebt gepusht, wel houden. Door uw register in een eigen resourcegroep te plaatsen, verkleint u het risico dat u de verzameling installatiekopieën in het register per ongeluk verwijdert als u de resourcegroep van de containerinstantie verwijdert.

## <a name="authentication"></a>Authentication

Voor de verificatie van een Azure-containerregister bestaan er twee primaire scenario's: afzonderlijke verificatie en serviceverificatie (ook wel een 'headless'-verificatie genoemd). De volgende tabel bevat een kort overzicht van deze scenario's en de verificatiemethode die voor elk ervan wordt aanbevolen.

| Type | Voorbeeldscenario | Aanbevolen methode |
|---|---|---|
| Afzonderlijke identiteit | Een ontwikkelaar die installatiekopieën binnenhaalt op of pusht vanaf zijn ontwikkelcomputer. | [az acr login](/cli/azure/acr?view=azure-cli-latest#az_acr_login) |
| Headless/service-identiteit | Bouw en implementeer pijplijnen waarbij de gebruiker niet direct is betrokken. | [Service/principal](container-registry-authentication.md#service-principal) |

Zie [Verifiëren met een Azure containerregister](container-registry-authentication.md) voor gedetailleerde informatie over verificatie met Azure Container Registry.

## <a name="next-steps"></a>Volgende stappen

Azure Container Registry is beschikbaar in verschillende categorieën, ook wel SKU's genoemd, die elk verschillende mogelijkheden bieden. Zie [Azure Container Registry-SKU's](container-registry-skus.md) voor meer informatie over de beschikbare SKU's.