---
title: Prijscategorieën voor Azure-Database voor MySQL
description: In dit artikel beschrijft de Prijscategorieën voor Azure-Database voor MySQL.
services: mysql
author: jan-eng
ms.author: janeng
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.topic: article
ms.date: 06/21/2018
ms.openlocfilehash: 93665ab075de539f98eebe34c2229870bd5a1a6e
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/25/2018
ms.locfileid: "36750235"
---
# <a name="azure-database-for-mysql-pricing-tiers"></a>Azure Prijscategorieën MySQL-Database

U kunt een Azure-Database voor de MySQL-server maken in een van drie verschillende Prijscategorieën: Basic, algemeen gebruik en geoptimaliseerd voor geheugen. De Prijscategorieën worden onderscheiden door de hoeveelheid compute in vCores die kunnen worden ingericht, geheugen per vCore en de opslagtechnologie gebruikt voor het opslaan van de gegevens. Alle resources worden ingericht op het niveau van de MySQL-server. Een server kan een of meer databases hebben.

|    | **Basic** | **Voor algemene doeleinden** | **Geoptimaliseerd voor geheugen** |
|:---|:----------|:--------------------|:---------------------|
| COMPUTE generatie | Gen 4, 5 Gen | Gen 4, 5 Gen | Gen 5 |
| vCores | 1, 2 | 2, 4, 8, 16, 32 |2, 4, 8, 16 |
| Geheugen per vCore | 2 GB | 5 GB | 10 GB |
| Opslaggrootte | 5 GB tot 1 TB | 5 GB tot 4 TB | 5 GB tot 4 TB |
| Opslagtype | Azure Standard Storage | Azure Premium Storage | Azure Premium Storage |
| De back-up bewaarperiode database | 7 tot 35 dagen | 7 tot 35 dagen | 7 tot 35 dagen |

Gebruik de volgende tabel als uitgangspunt om een prijscategorie te selecteren.

| Prijscategorie | Beoogde workloads |
|:-------------|:-----------------|
| Basic | Werklasten die lichte berekenings- en i/o-prestaties vereisen. Voorbeelden zijn onder meer servers die worden gebruikt voor ontwikkeling of tests of kleinschalige, onregelmatig gebruikte toepassingen. |
| Algemeen doel | De meeste zakelijke workloads waarvoor taakverdeling reken- en geheugencapaciteit met schaalbare i/o-doorvoer. Voorbeelden zijn onder meer servers voor het hosten van web- en mobiele apps en andere bedrijfstoepassingen.|
| Geoptimaliseerd geheugen | Hoge prestaties database werklasten die in het geheugen prestaties zijn vereist voor een snellere verwerking van transacties en hogere gelijktijdigheid. Voorbeelden zijn onder meer servers voor de verwerking van realtime gegevens en high-performance transactionele of analytische apps.|

Nadat u een server hebt gemaakt, het aantal vCores, hardware generatie en prijzen servicetier (met uitzondering van en naar Basic) kan worden gewijzigd omhoog of omlaag binnen enkele seconden. U kunt ook afzonderlijk de hoeveelheid opslag van en de back-up bewaarperiode omhoog of omlaag zonder uitvaltijd voor de toepassing aanpassen. U kunt het back-upopslag-type niet wijzigen nadat een server is gemaakt. Zie voor meer informatie de [schalen](#scale-resources) sectie.

## <a name="compute-generations-and-vcores"></a>COMPUTE generaties en vCores

COMPUTE resources worden geleverd als vCores die de logische CPU van de onderliggende hardware vertegenwoordigen. U kunt op dit moment kunt kiezen uit twee compute generaties Gen 4 en 5 Gen. Gen 4 logische CPU's zijn gebaseerd op Intel E5-2673 v3 (Haswell) 2,4 GHz-processors. Gen 5 logische CPU's zijn gebaseerd op Intel E5-2673 v4 (Broadwell) 2.3 GHz-processors. Gen 4 en 5 Gen zijn beschikbaar in de volgende regio's ('X' geeft beschikbaar).

| **Azure-regio** | **Gen 4** | **Gen 5** |
|:---|:----------:|:--------------------:|
| VS - centraal | X |  |
| VS - oost | X | X |
| VS - oost 2 | X | X |
| VS - noord/centraal | X | X |
| VS - zuid/centraal | X | X |
| VS - west | X | X |
| VS - west 2 |  | X |
| Canada - centraal | X | X |
| Canada - oost | X | X |
| Brazilië - Zuid | X | X |
| Noord-Europa | X | X |
| West-Europa |  | X |
| VK - west |  | X |
| VK - zuid |  | X |
| Oost-Azië | X | X |
| Zuidoost-Azië | X | X |
| Australië - oost |  | X |
| Australië - zuidoost |  | X |
| India - centraal | X | X |
| India - west | X | X |
| India - zuid |  | X |
| Japan - Oost | X | X |
| Japan - West | X | X |
| Korea - centraal |  | X |
| Korea - zuid |  | X |

## <a name="storage"></a>Storage

De opslag die u inricht is de hoeveelheid opslagcapaciteit beschikbaar is voor uw Azure-Database voor MySQL-server. De opslag wordt gebruikt voor de databasebestanden, tijdelijke bestanden, transactielogboeken en de server MySQL Logboeken. De totale hoeveelheid opslag die u inricht definieert ook de i/o-capaciteit die beschikbaar is op uw server.

|    | **Basic** | **Voor algemene doeleinden** | **Geoptimaliseerd voor geheugen** |
|:---|:----------|:--------------------|:---------------------|
| Opslagtype | Azure Standard Storage | Azure Premium Storage | Azure Premium Storage |
| Opslaggrootte | 5 GB tot 1 TB | 5 GB tot 4 TB | 5 GB tot 4 TB |
| Maximale grootte van de verhoging | 1 GB | 1 GB | 1 GB |
| IOPS | Variabele |3 IOPS/GB<br/>Min 100 IOP's<br/>Max. 6000 IOP 's | 3 IOPS/GB<br/>Min 100 IOP's<br/>Max. 6000 IOP 's |

U kunt extra opslagcapaciteit toevoegen tijdens en na het maken van de server. De laag Basic biedt geen garantie van een IOPS. In het algemeen en de geoptimaliseerd voor geheugen Prijscategorieën schalen het aantal IOPS dat met de opslaggrootte van de ingerichte in een ratio van 3:1.

U kunt uw i/o-verbruik in de Azure-portal of met behulp van Azure CLI-opdrachten kunt bewaken. De relevante meetgegevens voor het bewaken van zijn [opslaglimiet bereikt, opslagpercentage opslag gebruikt en i/o-procent](concepts-monitoring.md).

### <a name="reaching-the-storage-limit"></a>De opslaglimiet bereikt

De server wordt gemarkeerd als alleen-lezen wanneer de hoeveelheid vrije opslagruimte op minder dan 5 GB of ingerichte opslag maar 5% bereikt, afhankelijk van wat is. Bijvoorbeeld, als u hebt ingericht, 100 GB aan opslagruimte en het werkelijke gebruik gaat over 95 GB, de server is gemarkeerd als alleen-lezen. U kunt ook als u 5 GB aan opslagruimte hebt ingericht, is de server gemarkeerd alleen-lezen wanneer de vrije opslagruimte op minder dan 250 MB bereikt.  

Terwijl de service probeert te maken van de server alleen-lezen, alle nieuwe schrijftransactieaanvragen worden geblokkeerd en bestaande actieve transacties blijven uitvoeren. Wanneer de server is ingesteld op alleen-lezen, doorvoeren alle daaropvolgende schrijfbewerkingen en transactie is mislukt. Alleen query's blijven doorwerken. Nadat u de ingerichte opslag verhoogt, moet de server klaar om te schrijven transacties opnieuw accepteren.

## <a name="backup"></a>Backup

De service wordt automatisch een back-ups van uw server. De minimale bewaarperiode voor back-ups is zeven dagen. U kunt een bewaarperiode van maximaal 35 dagen instellen. De bewaarperiode kan worden aangepast op elk gewenst moment tijdens de levensduur van de server. U kunt kiezen tussen lokaal redundante en geografisch redundante back-ups. Geografisch redundante back-ups worden ook opgeslagen in de [regio geo gekoppeld](https://docs.microsoft.com/azure/best-practices-availability-paired-regions) van de regio waar uw server wordt gemaakt. Dit biedt een niveau van bescherming in het geval van een noodgeval. Ook krijgt u de mogelijkheid uw server naar een andere Azure-regio waarin de service beschikbaar met geografisch redundante back-ups is herstellen. Het is niet mogelijk om te wijzigen tussen de twee opties voor back-upopslag nadat de server is gemaakt.

## <a name="scale-resources"></a>Resources omhoog/omlaag schalen

Nadat u uw server maakt, kunt u onafhankelijk wijzigen de vCores, het genereren van de hardware, de prijscategorie (behalve naar en van Basic), de hoeveelheid opslag en de back-up bewaarperiode. U kunt het back-upopslag-type niet wijzigen nadat een server is gemaakt. Het aantal vCores kan worden geschaald omhoog of omlaag. De back-up bewaarperiode kan worden geschaald omhoog of omlaag uit 7 tot 35 dagen. De grootte van de opslagruimte kan alleen worden verhoogd. Schalen van de resources kan worden gedaan door middel van de portal of Azure CLI. Zie voor een voorbeeld van schalen met Azure CLI, [bewaken en schalen van een Azure-Database voor de MySQL-server met behulp van Azure CLI](scripts/sample-scale-server.md).

Wanneer u het aantal vCores wijzigt, wordt het genereren van de hardware of de prijscategorie, een kopie van de oorspronkelijke server gemaakt met de nieuwe compute-toewijzing. Nadat de nieuwe server actief is, worden verbindingen via overgeschakeld naar de nieuwe server. Tijdens het moment dat wanneer het systeem overgeschakeld naar de nieuwe server, kunnen er geen nieuwe verbindingen tot stand worden gebracht en alle niet-doorgevoerde transacties teruggedraaid. Dit venster verschilt, maar in de meeste gevallen is minder dan een minuut.

Schaalbaarheid van opslag en het wijzigen van de back-up bewaarperiode zijn true online bewerkingen. Er is geen uitvaltijd en uw toepassing wordt niet beïnvloed. Zoals IOPS met de grootte van de ingerichte opslag uitbreiden, kunt u het aantal IOPS dat beschikbaar is op uw server door te schalen opslag van verhogen.

## <a name="pricing"></a>Prijzen

Zie de service voor de meest actuele informatie over de prijzen, [pagina met prijzen](https://azure.microsoft.com/pricing/details/mysql/). De kosten voor de configuratie die u wilt, zien de [Azure-portal](https://portal.azure.com/#create/Microsoft.MySQLServer) ziet u de maandelijkse kosten op de **prijscategorie** tabblad op basis van de opties die u selecteert. Als u geen Azure-abonnement hebt, kunt u de Azure prijscategorie Rekenmachine een geschatte prijs ophalen. Op de [Azure prijscategorie Rekenmachine](https://azure.microsoft.com/pricing/calculator/) website, selecteer **items toevoegen**, vouw de **Databases** categorie, en kies **Azure Database voor MySQL**voor het aanpassen van de opties.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over hoe [een MySQL-server maken in de portal](howto-create-manage-server-portal.md).
- Meer informatie over hoe [bewaken en schalen van een Azure-Database voor de MySQL-server met behulp van Azure CLI](scripts/sample-scale-server.md).
- Meer informatie over de [service beperkingen](concepts-limits.md).
