---
title: Prijscategorieën voor Azure-Database voor PostgreSQL
description: In dit artikel beschrijft de Prijscategorieën voor Azure-Database voor PostgreSQL.
services: postgresql
author: jan-eng
ms.author: janeng
manager: kfile
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 03/20/2018
ms.openlocfilehash: 3ea7d09338d4d89030138b8c4dc4085a6cd8ccc5
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/05/2018
---
# <a name="azure-database-for-postgresql-pricing-tiers"></a>Azure Prijscategorieën PostgreSQL-Database

U kunt een Azure-Database voor PostgreSQL-server maken in een van drie verschillende Prijscategorieën: Basic, algemeen gebruik en geoptimaliseerd voor geheugen. De Prijscategorieën worden onderscheiden door de hoeveelheid compute in vCores die kunnen worden ingericht, geheugen per vCore en de opslagtechnologie gebruikt voor het opslaan van de gegevens. Alle resources worden ingericht op het niveau van de PostgreSQL-server. Een server kan een of meer databases hebben.

|    | **Basic** | **Voor algemene doeleinden** | **Geoptimaliseerd voor geheugen** |
|:---|:----------|:--------------------|:---------------------|
| COMPUTE generatie | Gen 4, 5 Gen | Gen 4, 5 Gen | Gen 5 |
| vCores | 1, 2 | 2, 4, 8, 16, 32 |2, 4, 8, 16 |
| Geheugen per vCore | Baseline | 2x Basic | 2 x voor algemene doeleinden |
| Opslaggrootte | 5 GB tot 1 TB | 5 GB tot 2 TB | 5 GB tot 2 TB |
| Opslagtype | Azure Standard Storage | Azure Premium Storage | Azure Premium Storage |
| De back-up bewaarperiode database | 7 tot 35 dagen | 7 tot 35 dagen | 7 tot 35 dagen |

Gebruik de volgende tabel als uitgangspunt om een prijscategorie te selecteren.

| Prijscategorie | Beoogde workloads |
|:-------------|:-----------------|
| Basic | Werklasten die lichte berekenings- en i/o-prestaties vereisen. Voorbeelden zijn onder meer servers die worden gebruikt voor ontwikkeling of tests of kleinschalige, onregelmatig gebruikte toepassingen. |
| Algemeen doel | De meeste zakelijke workloads waarvoor taakverdeling reken- en geheugencapaciteit met schaalbare i/o-doorvoer. Voorbeelden zijn onder meer servers voor het hosten van web- en mobiele apps en andere bedrijfstoepassingen.|
| Geoptimaliseerd geheugen | Hoge prestaties database werklasten die in het geheugen prestaties zijn vereist voor een snellere verwerking van transacties en hogere gelijktijdigheid. Voorbeelden zijn onder meer servers voor de verwerking van realtime gegevens en high-performance transactionele of analytische apps.|

Nadat u een server hebt gemaakt, kan het aantal vCores worden gewijzigd omhoog of omlaag binnen enkele seconden. U kunt ook afzonderlijk de hoeveelheid opslag van en de back-up bewaarperiode omhoog of omlaag zonder uitvaltijd voor de toepassing aanpassen. Zie de sectie 'Schalen' voor meer informatie.

## <a name="compute-generations-vcores-and-memory"></a>COMPUTE generaties, vCores en geheugen

COMPUTE resources worden geleverd als vCores die de logische CPU van de onderliggende hardware vertegenwoordigen. U kunt op dit moment kunt kiezen uit twee compute generaties Gen 4 en 5 Gen. Gen 4 logische CPU's zijn gebaseerd op Intel E5-2673 v3 (Haswell) 2,4 GHz-processors. Gen 5 logische CPU's zijn gebaseerd op Intel E5-2673 v4 (Broadwell) 2.3 GHz-processors. Gen 4 en 5 Gen zijn beschikbaar in de volgende regio's ('X' geeft beschikbaar). 

| **Azure-regio** | **Gen 4** | **Gen 5** |
|:---|:----------:|:--------------------:|
| VS - midden |  | X |
| VS - oost | X | X |
| VS - oost 2 | X | X |
| Noord-centraal VS | X |  |
| Zuid-centraal VS | X | X |
| VS - west | X | X |
| VS - west 2 |  | X |
| Canada - midden | X | X |
| Canada - oost | X | X |
| Brazilië - zuid | X |  |
| Noord-Europa | X | X |
| West-Europa | X | X |
| Verenigd Koninkrijk West |  | X |
| Verenigd Koninkrijk Zuid |  | X |
| Oost-Azië | X |  |
| Zuidoost-Azië | X |  |
| Australië - oost |  | X |
| Centraal-India | X |  |
| West-India | X |  |
| Japan - oost | X | X |
| Japan - west | X | X |
| Korea - zuid |  | X |

Elke vCore is afhankelijk van de prijscategorie ingericht met een specifieke hoeveelheid geheugen. Wanneer u vergroten of het aantal vCores voor uw server verkleinen, wordt het geheugen verhoogt of verlaagt proportioneel. De laag voor algemene doeleinden biedt tweemaal de hoeveelheid geheugen per vCore vergeleken met de basisstaffel. De laag geoptimaliseerd voor geheugen bevat dubbele de hoeveelheid geheugen ten opzichte van de laag voor algemene doeleinden.

## <a name="storage"></a>Storage

De opslag die u inricht is de hoeveelheid opslagcapaciteit beschikbaar is voor uw Azure-Database voor PostgreSQL-server. De opslag wordt gebruikt voor de databasebestanden, tijdelijke bestanden, transactielogboeken en de server PostgreSQL Logboeken. De totale hoeveelheid opslag die u inricht definieert ook de i/o-capaciteit die beschikbaar is op uw server.

|    | **Basic** | **Voor algemene doeleinden** | **Geoptimaliseerd voor geheugen** |
|:---|:----------|:--------------------|:---------------------|
| Opslagtype | Azure Standard Storage | Azure Premium Storage | Azure Premium Storage |
| Opslaggrootte | 5 GB tot 1 TB | 5 GB tot 2 TB | 5 GB tot 2 TB |
| Maximale grootte van de verhoging | 1 GB | 1 GB | 1 GB |
| IOPS | Variabele |3 IOPS/GB<br/>Min 100 IOP's | 3 IOPS/GB<br/>Min 100 IOP's |

U kunt extra opslagcapaciteit toevoegen tijdens en na het maken van de server. De laag Basic biedt geen garantie van een IOPS. In het algemeen en de geoptimaliseerd voor geheugen Prijscategorieën schalen het aantal IOPS dat met de opslaggrootte van de ingerichte in een ratio van 3:1.

U kunt uw i/o-verbruik in de Azure-portal of met behulp van Azure CLI-opdrachten kunt bewaken. De relevante meetgegevens voor het bewaken van zijn [opslaglimiet bereikt, opslagpercentage opslag gebruikt en i/o-procent](concepts-monitoring.md).

## <a name="backup"></a>Back-up maken

De service wordt automatisch een back-ups van uw server. De minimale bewaarperiode voor back-ups is zeven dagen. U kunt een bewaarperiode van maximaal 35 dagen instellen. De bewaarperiode kan worden aangepast op elk gewenst moment tijdens de levensduur van de server. U kunt kiezen tussen lokaal redundante en geografisch redundante back-ups. Geografisch redundante back-ups worden ook opgeslagen in de [regio geo gekoppeld](https://docs.microsoft.com/azure/best-practices-availability-paired-regions) van de regio waar uw server wordt gemaakt. Dit biedt een niveau van bescherming in het geval van een noodgeval. Ook krijgt u de mogelijkheid uw server naar een andere Azure-regio waarin de service beschikbaar met geografisch redundante back-ups is herstellen. Het is niet mogelijk om te wijzigen tussen de twee opties voor back-upopslag nadat de server is gemaakt.

## <a name="scale-resources"></a>Resources omhoog/omlaag schalen

Nadat u uw server maakt, kunt u de vCores, de hoeveelheid opslagruimte en de back-up bewaarperiode onafhankelijk wijzigen. U kunt de prijscategorie of het type back-upopslag niet wijzigen nadat een server is gemaakt. vCores en de back-up bewaarperiode kunnen worden geschaald omhoog of omlaag. De grootte van de opslagruimte kan alleen worden verhoogd. Schalen van de resources kan worden gedaan door middel van de portal of Azure CLI. Zie voor een voorbeeld van het gebruik van Azure CLI schalen [bewaken en schalen van een Azure-Database voor PostgreSQL-server met behulp van Azure CLI](scripts/sample-scale-server-up-or-down.md).

Wanneer u het aantal vCores wijzigt, wordt een kopie van de oorspronkelijke server gemaakt met de nieuwe compute-toewijzing. Nadat de nieuwe server actief is, worden verbindingen via overgeschakeld naar de nieuwe server. Tijdens het moment dat wanneer het systeem overgeschakeld naar de nieuwe server, kunnen er geen nieuwe verbindingen tot stand worden gebracht en alle niet-doorgevoerde transacties teruggedraaid. Dit venster verschilt, maar in de meeste gevallen is minder dan een minuut.

Schaalbaarheid van opslag en het wijzigen van de back-up bewaarperiode zijn true online bewerkingen. Er is geen uitvaltijd en uw toepassing wordt niet beïnvloed. Zoals IOPS met de grootte van de ingerichte opslag uitbreiden, kunt u het aantal IOPS dat beschikbaar is op uw server door te schalen opslag van verhogen.

## <a name="pricing"></a>Prijzen

Zie de service voor de meest actuele informatie over de prijzen, [pagina met prijzen](https://azure.microsoft.com/pricing/details/PostgreSQL/). De kosten voor de configuratie die u wilt, zien de [Azure-portal](https://portal.azure.com/#create/Microsoft.PostgreSQLServer) ziet u de maandelijkse kosten op de **prijscategorie** tabblad op basis van de opties die u selecteert. Als u geen Azure-abonnement hebt, kunt u de Azure prijscategorie Rekenmachine een geschatte prijs ophalen. Op de [Azure prijscategorie Rekenmachine](https://azure.microsoft.com/pricing/calculator/) website, selecteer **items toevoegen**, vouw de **Databases** categorie, en kies **Azure Database voor PostgreSQL** voor het aanpassen van de opties.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over hoe [een PostgreSQL-server maken in de portal](tutorial-design-database-using-azure-portal.md).
- Meer informatie over hoe [bewaken en schalen van een Azure-Database voor PostgreSQL-server met behulp van Azure CLI](scripts/sample-scale-server-up-or-down.md).
- Meer informatie over de [service beperkingen](concepts-limits.md).
