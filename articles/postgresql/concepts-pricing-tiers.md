---
title: Prijscategorieën voor Azure Database for PostgreSQL
description: Dit artikel beschrijft de Prijscategorieën voor Azure Database voor PostgreSQL.
author: jan-eng
ms.author: janeng
ms.service: postgresql
ms.topic: conceptual
ms.date: 01/15/2019
ms.openlocfilehash: 49c9d908970425165fcdd76171c20ab65454f42a
ms.sourcegitcommit: 3ba9bb78e35c3c3c3c8991b64282f5001fd0a67b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/15/2019
ms.locfileid: "54318573"
---
# <a name="azure-database-for-postgresql-pricing-tiers"></a>Azure Database voor PostgreSQL Prijscategorieën

U kunt een Azure Database for PostgreSQL-server maken in een van drie verschillende Prijscategorieën: Basic, Algemeen gebruik en Geoptimaliseerd voor geheugen. De Prijscategorieën worden onderscheiden op basis van de hoeveelheid Computing in vCores die kunnen worden ingericht, het geheugen per vCore en de technologie voor gegevensopslag gebruikt voor het opslaan van de gegevens. Alle resources zijn ingericht op het niveau van de PostgreSQL-server. Een server kan een of meer databases bevatten.

|    | **Basic** | **Algemeen gebruik** | **Geoptimaliseerd voor geheugen** |
|:---|:----------|:--------------------|:---------------------|
| Bewerking voor COMPUTE | Gen 4, Gen 5 | Gen 4, Gen 5 | Gen 5 |
| vCores | 1, 2 | 2, 4, 8, 16, 32, 64 |2, 4, 8, 16, 32 |
| Geheugen per vCore | 2 GB | 5 GB | 10 GB |
| Opslaggrootte | 5 GB tot 1 TB | 5 GB tot 4 TB | 5 GB tot 4 TB |
| Opslagtype | Azure Standard Storage | Azure Premium Storage | Azure Premium Storage |
| Database-back-up bewaarperiode | 7 tot 35 dagen | 7 tot 35 dagen | 7 tot 35 dagen |

Kies een prijscategorie, gebruik de volgende tabel als uitgangspunt.

| Prijscategorie | Beoogde workloads |
|:-------------|:-----------------|
| Basic | Workloads waarvoor lichte reken- en i/o-prestaties. Voorbeelden zijn onder meer servers die worden gebruikt voor ontwikkeling of tests of kleinschalige, onregelmatig gebruikte toepassingen. |
| Algemeen doel | Meeste zakelijke workloads die goede balans tussen Computing en geheugen met schaalbare i/o-doorvoer vereisen. Voorbeelden zijn servers voor het hosten van web en mobiele apps en andere zakelijke toepassingen.|
| Geoptimaliseerd geheugen | High-performance workloads van databases waarvoor de prestaties van het geheugen voor snellere transactieverwerking en hogere gelijktijdigheid. Voorbeelden zijn servers voor het verwerken van realtimegegevens en transactie- of analyseapps apps van hoge prestaties.|

Nadat u een server maakt, het aantal vCores, hardware genereren en prijzen laag (met uitzondering van en naar Basic) kan omhoog of omlaag worden gewijzigd binnen enkele seconden. U kunt ook afzonderlijk de hoeveelheid opslag van en de back-up bewaarperiode omhoog of omlaag met zonder downtime van toepassingen aanpassen. U kunt de back-upopslag-type niet wijzigen nadat een server is gemaakt. Zie voor meer informatie de [resources schalen](#scale-resources) sectie.


## <a name="compute-generations-and-vcores"></a>Bewerkingen voor COMPUTE en vCores

COMPUTE-resources worden verstrekt als vCores, die staan voor de logische CPU van de onderliggende hardware. Op dit moment, kunt u kiezen uit twee bewerkingen voor compute, Gen 4 en Gen 5. Gen 4 logische CPU's zijn gebaseerd op Intel E5-2673 v3 (Haswell)-processors voor 2,4 GHz. Gen 5 logische CPU's zijn gebaseerd op Intel E5-2673 v4 (Broadwell) 2,3 GHz-processors. Gen 4 en Gen 5 zijn beschikbaar in de volgende regio's ('X' geeft aan beschikbaar). 

> [!IMPORTANT]
> Vanaf 12 December 2018, is nieuwe klanten niet mogelijk om in te richten compute generatie 4 servers in Brazilië-Zuid, Canada-centraal, Canada-Oost, Zuidoost-Azië, VS-Oost 2, centraal-India, West-India, Japan-West, Noord-centraal VS, VS-West. Bewerking voor compute 4 servers worden gemigreerd om de bewerking 5 vanaf 1 februari 2019 in deze regio's voor compute eerder hebt gemaakt.

| **Azure-regio** | **Gen 4** | **Gen 5** |
|:---|:----------:|:--------------------:|
| US - centraal |  | X |
| US - oost |  | X |
| US - oost 2 | X | X |
| US - noord-centraal | X | X |
| US - zuid-centraal | X | X |
| US - west | X | X |
| US - west 2 |  | X |
| Brazilië - zuid | X | X |
| Canada - midden | X | X |
| Canada - oost | X | X |
| Europa - noord | X | X |
| Europa -west |  | X |
| Frankrijk - centraal |  | X |
| Verenigd Koninkrijk Zuid |  | X |
| Verenigd Koninkrijk West |  | X |
| Azië - oost | X | X |
| Azië - zuidoost | X | X |
| Australië - oost |  | X |
| Australië - centraal |  | X |
| Australië - centraal 2 |  | X |
| Australië - zuidoost |  | X |
| India - centraal | X | X |
| India - zuid |  | X |
| India - west | X | X |
| Japan - oost | X | X |
| Japan - west | X | X |
| Korea - centraal |  | X |
| Korea - zuid |  | X |
| China Oost 1 | X |  |
| China - oost 2 |  | X |
| China North 1 | X |  |
| China - noord 2 |  | X |
| Duitsland - centraal |  | X |
| US DoD - centraal  | X |  |
| US DoD - oost  | X |  |
| VS (overheid) - Arizona |  | X |
| VS (overheid) - Texas |  | X |
| VS (overheid) - Virginia |  | X |

## <a name="storage"></a>Storage

De opslag die u inricht is de hoeveelheid opslagcapaciteit beschikbaar zijn voor uw Azure Database for PostgreSQL-server. De opslag wordt gebruikt voor de databasebestanden, tijdelijke bestanden transactielogboeken en de PostgreSQL-server zich aanmeldt. De totale hoeveelheid opslag die u inricht definieert ook de i/o-capaciteit beschikbaar voor uw server.

|    | **Basic** | **Algemeen gebruik** | **Geoptimaliseerd voor geheugen** |
|:---|:----------|:--------------------|:---------------------|
| Opslagtype | Azure Standard Storage | Azure Premium Storage | Azure Premium Storage |
| Opslaggrootte | 5 GB tot 1 TB | 5 GB tot 4 TB | 5 GB tot 4 TB |
| Maximale grootte van de verhoging | 1 GB | 1 GB | 1 GB |
| IOPS | Variabele |3 IOPS/GB<br/>Min 100 IOPS<br/>Max. aantal 6000 IOP 's | 3 IOPS/GB<br/>Min 100 IOPS<br/>Max. aantal 6000 IOP 's |

U kunt extra opslagcapaciteit toevoegen tijdens en na het maken van de server. De Basic-laag biedt geen een garantie IOPS. In het algemeen gebruik en geoptimaliseerd voor geheugen Prijscategorieën, schalen de IOPS-waarde met de grootte van de ingerichte opslag in een verhouding van 3:1.

U kunt uw i/o-gebruik in Azure portal of met behulp van Azure CLI-opdrachten kunt bewaken. De relevante metrische gegevens voor het bewaken van zijn [-limiet voor opslag, opslagpercentage, opslag die wordt gebruikt en i/o-percentage](concepts-monitoring.md).

### <a name="reaching-the-storage-limit"></a>De opslaglimiet bereikt

De server wordt als alleen-lezen gemarkeerd wanneer de beschikbare opslagruimte kleiner is dan 5 GB of 5% van de ingerichte opslag, al naar gelang wat kleiner is. Bijvoorbeeld, als u 100 GB aan opslag hebt ingericht, en het gebruik van de werkelijke gaat via 95 GB, de server is gemarkeerd als alleen-lezen. Als u in een ander geval 5 GB aan opslagruimte hebt ingericht, wordt de server als alleen-lezen gemarkeerd indien de beschikbare opslagruimte minder dan 250 MB wordt.  

Wanneer de server is ingesteld op alleen-lezen, alle bestaande sessies worden beëindigd en niet-doorgevoerde transacties worden teruggedraaid. Alle daaropvolgende schrijfbewerkingen en de transactie wordt doorgevoerd mislukken. Alle volgende lezen query's werken ononderbroken.  

U kunt de ingerichte opslag met de server vergroten of een nieuwe sessie starten in de modus en -neerzetten gegevens lezen / schrijven vrije opslagruimte vrij te maken. Met `SET SESSION CHARACTERISTICS AS TRANSACTION READ WRITE;` Hiermee stelt u de huidige sessie om te lezen, schrijven-modus. Om te voorkomen dat een beschadiging van gegevens, Voer niet uit alle schrijfbewerkingen wanneer de server nog steeds in de status alleen-lezen is.

U wordt aangeraden dat u een waarschuwing instellen om u te waarschuwen als de opslag van uw server bijna de drempelwaarde is bereikt, zodat u kunt te voorkomen dat de status alleen-lezen. Zie voor meer informatie de documentatie over [over het instellen van een waarschuwing](howto-alert-on-metric.md).

## <a name="backup"></a>Backup

Voor de service worden automatisch back-ups van uw server. De minimale bewaarperiode voor back-ups is zeven dagen. U kunt een bewaarperiode van maximaal 35 dagen instellen. De bewaarperiode kan worden aangepast op elk gewenst moment tijdens de levensduur van de server. U kunt kiezen tussen lokaal redundant en geografisch redundante back-ups. Geografisch redundante back-ups worden ook opgeslagen in de [geokoppeling regio](https://docs.microsoft.com/azure/best-practices-availability-paired-regions) van de regio waarin uw server is gemaakt. Deze redundantie biedt een niveau van beveiliging in het geval van een noodgeval. Ook krijgt u de mogelijkheid om uw server te herstellen naar een andere Azure-regio waarin de service beschikbaar met geografisch redundante back-ups is. Het is niet mogelijk om te wisselen tussen de twee opties voor back-upopslag nadat de server is gemaakt.

## <a name="scale-resources"></a>Resources omhoog/omlaag schalen

Nadat u uw server maakt, kunt u onafhankelijk van elkaar wijzigen de vCores, de hardware-generatie en de prijscategorie (met uitzondering van en naar Basic), de hoeveelheid opslag en de bewaarperiode voor back-up. U kunt de back-upopslag-type niet wijzigen nadat een server is gemaakt. Het aantal vCores kunt omhoog of omlaag worden geschaald. De bewaarperiode voor back-up kan worden geschaald omhoog of omlaag van 7 tot 35 dagen. De grootte van de kan alleen worden verhoogd. Schalen van de resources die kan worden gedaan via de portal of Azure CLI. Zie voor een voorbeeld van schalen met behulp van Azure CLI, [bewaken en schalen van een Azure Database for PostgreSQL-server met behulp van Azure CLI](scripts/sample-scale-server-up-or-down.md).

Wanneer u het aantal vCores wijzigt, worden de hardware-generatie en de prijscategorie, een kopie van de oorspronkelijke server wordt gemaakt met de nieuwe compute-toewijzing. Nadat de nieuwe server actief en werkend is, worden verbindingen via overgeschakeld naar de nieuwe server. Tijdens het moment dat wanneer het systeem overgeschakeld naar de nieuwe server, kunnen er geen nieuwe verbindingen tot stand worden gebracht en alle niet-doorgevoerde transacties worden teruggedraaid. In dit venster varieert, maar in de meeste gevallen is minder dan een minuut.

Schalen van opslag en het wijzigen van de bewaarperiode voor back-up zijn ' True ' online bewerkingen. Er is geen downtime en uw toepassing wordt niet beïnvloed. IOP's met de grootte van de ingerichte opslag te schalen, kunt u de beschikbare IOPS verhogen met uw server met omhoog schalen van opslag.

## <a name="pricing"></a>Prijzen

Zie de service voor de meest recente prijsinformatie [pagina met prijzen](https://azure.microsoft.com/pricing/details/PostgreSQL/). Om te zien van de kosten voor de configuratie die u wilt, de [Azure-portal](https://portal.azure.com/#create/Microsoft.PostgreSQLServer) ziet u de maandelijkse kosten op de **prijscategorie** tabblad op basis van de opties die u selecteert. Als u geen Azure-abonnement hebt, kunt u de prijscalculator van Azure gebruiken om op te halen van een geschatte prijs. Op de [prijscalculator van Azure](https://azure.microsoft.com/pricing/calculator/) website, selecteer **items toevoegen**, vouw de **Databases** categorie, en kies **Azure Database for PostgreSQL** om aan te passen van de opties.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [een PostgreSQL-server maken in de portal](tutorial-design-database-using-azure-portal.md).
- Meer informatie over het [controleren en schalen van een Azure Database for PostgreSQL-server met behulp van Azure CLI](scripts/sample-scale-server-up-or-down.md).
- Meer informatie over de [service beperkingen](concepts-limits.md). 
