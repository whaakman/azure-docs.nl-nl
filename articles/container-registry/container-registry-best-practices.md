---
title: Aanbevolen procedures in Azure Container register
description: Informatie over het gebruik van uw Azure-container register effectief door deze aanbevolen procedures te volgen.
services: container-registry
documentationcenter: 
author: mmacy
manager: timlt
editor: mmacy
tags: 
keywords: 
ms.assetid: 
ms.service: container-registry
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/05/2017
ms.author: marsma
ms.custom: 
ms.openlocfilehash: 9ec5573082dbf9de1288e1511f5041f8c20b416e
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/02/2017
---
# <a name="best-practices-for-azure-container-registry"></a>Aanbevolen procedures voor het Azure Container register

Door deze aanbevolen procedures te volgen, kunt u de prestaties en rendabele gebruik van uw persoonlijke Docker-register in Azure te maximaliseren.

## <a name="network-close-deployment"></a>Implementatie van het netwerk wordt gesloten

Het register van de container maken in de dezelfde Azure-regio waarin u containers implementeren. Als u het register in een regio die het netwerk wordt gesloten voor de container hosts kunnen helpen, latentie en de kosten te verlagen.

Netwerk sluiten implementatie is een van de belangrijkste redenen voor het gebruik van een privé-container-register. Docker afbeeldingen hebben een geweldige [gelaagdheid constructie](https://docs.docker.com/engine/userguide/storagedriver/imagesandcontainers/) waarmee voor incrementele implementaties. Nieuwe knooppunten moeten echter voor het ophalen van alle lagen die nodig zijn voor een opgegeven installatiekopie. Deze initiële `docker pull` snel tot meerdere gigabytes kunt toevoegen. Met een persoonlijke register dicht bij uw implementatie minimaliseert de netwerklatentie.
Bovendien implementeren alle openbare clouds is opgenomen, Azure network-kosten voor uitgaande gegevens. Binnenhalen van installatiekopieën van een datacenter naar de andere, voegt de kosten van het netwerk uitgaande, naast de latentie.

## <a name="geo-replicate-multi-region-deployments"></a>Geo-replicatie meerdere landen/regio-implementaties

Gebruik Azure Container register [geo-replicatie](container-registry-geo-replication.md) functie als u containers naar meerdere regio's implementeert. Of u bent voor de globale klanten uit lokale datacenters of uw ontwikkelteam bevindt zich in verschillende locaties, kunt u het registerbeheer vereenvoudigen en Latentie minimaliseren door geo-replicatie met het register. Deze functie is momenteel beschikbaar met Preview-versie, [Premium](container-registry-skus.md#premium) registers.

Voor informatie over het gebruik van geo-replicatie, raadpleegt de zelfstudie driedelige [Geo-replicatie in Azure Container register](container-registry-tutorial-prepare-registry.md).

## <a name="repository-namespaces"></a>Opslagplaats naamruimten

Dankzij het gebruik van de opslagplaats naamruimten, kunt u een enkele register delen tussen meerdere groepen binnen uw organisatie. Registers kunnen worden gedeeld door implementaties en teams. Azure Container register ondersteunt geneste naamruimten, waardoor de isolatie van de groep.

Neem bijvoorbeeld de volgende container installatiekopie labels. Afbeeldingen die worden gebruikt hele bedrijf, zoals `aspnetcore`, terwijl de installatiekopieën van de container die eigendom van de productie- en -groepen elk gebruik hun eigen naamruimten zijn in de hoofdnaamruimte worden geplaatst.

```
contoso.azurecr.io/aspnetcore:2.0
contoso.azurecr.io/products/widget/web:1
contoso.azurecr.io/products/bettermousetrap/refundapi:12.3
contoso.azurecr.io/marketing/2017-fall/concertpromotions/campaign:218.42
```

## <a name="dedicated-resource-group"></a>Toegewezen resourcegroep

Omdat container registers resources die worden gebruikt op meerdere hosts van de container, moet een eigen resourcegroep bevinden zich een register.

Hoewel u met een specifieke host-type, zoals Azure Containerexemplaren experimenteren mogelijk, wilt u waarschijnlijk de instantie van de container verwijderen wanneer u klaar bent. U kunt echter ook om de verzameling van installatiekopieën die u in Azure Container register gepusht te bewaren. Door het plaatsen van het register in een eigen resourcegroep bevinden, verkleint u het risico van het per ongeluk verwijderen van de verzameling van afbeeldingen in het register wanneer u de resourcegroep van de container-exemplaar verwijdert.

## <a name="authentication"></a>Authentication

Bij het verifiëren met een Azure container registry, er zijn twee primaire scenario's: afzonderlijke verificatie en verificatie service (of 'headless'). De volgende tabel bevat een kort overzicht van deze scenario's en de aanbevolen methode verificatie voor elk.

| Type | Voorbeeldscenario 's | Aanbevolen methode |
|---|---|---|
| Afzonderlijke identiteit | Een ontwikkelaar binnenhalen van afbeeldingen of pushen van installatiekopieën van hun ontwikkelcomputer. | [AZ acr aanmelding](/cli/azure/acr?view=azure-cli-latest#az_acr_login) |
| Headless-/ service-identiteit | Build- en pijplijnen waar de gebruiker is niet direct is betrokken. | [Service-principal](container-registry-authentication.md#service-principal) |

Zie voor gedetailleerde informatie over verificatie met Azure Container register [verifiëren met een Azure container registry](container-registry-authentication.md).

## <a name="next-steps"></a>Volgende stappen

Azure Container register is beschikbaar in verschillende lagen, aangeroepen SKU's, die elk verschillende mogelijkheden bieden. Zie voor meer informatie over de beschikbare SKU's [Azure Container register SKU's](container-registry-skus.md).