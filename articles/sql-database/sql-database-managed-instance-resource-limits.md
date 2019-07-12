---
title: Azure SQL Database-resourcebeperkingen - beheerd exemplaar | Microsoft Docs
description: In dit artikel biedt een overzicht van de Azure SQL Database-resourcebeperkingen voor beheerde exemplaren.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: bonova
ms.author: bonova
ms.reviewer: carlrab, jovanpop, sachinp, sstein
manager: craigg
ms.date: 06/26/2019
ms.openlocfilehash: f4e19b916553912e36f2c3beee3f6a518b244e4d
ms.sourcegitcommit: c105ccb7cfae6ee87f50f099a1c035623a2e239b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2019
ms.locfileid: "67707010"
---
# <a name="overview-azure-sql-database-managed-instance-resource-limits"></a>Overzicht van Azure SQL-Database beheerd exemplaar resourcebeperkingen

Dit artikel biedt een overzicht van de resourcelimieten voor het beheerde exemplaar van Azure SQL Database, en bevat informatie over hoe u een verhoging naar deze limieten.

> [!NOTE]
> Zie voor verschillen in ondersteunde functies en T-SQL statements [Functieverschillen](sql-database-features.md) en [ondersteuning voor T-SQL-instructie](sql-database-managed-instance-transact-sql-information.md).

## <a name="instance-level-resource-limits"></a>Bronlimieten op exemplaarniveau

Beheerd exemplaar bevat kenmerken en resourcelimieten die afhankelijk van de onderliggende infrastructuur en -architectuur zijn. Limieten, is afhankelijk van hardware genereren en service-laag.

### <a name="hardware-generation-characteristics"></a>Hardwarekenmerken genereren

Beheerd exemplaar voor Azure SQL-Database kan worden geïmplementeerd op twee hardwaregeneraties van: Gen4 als Gen5. Hardwaregeneraties hebben andere kenmerken, zoals beschreven in de volgende tabel:

|   | **Gen4** | **Gen5** |
| --- | --- | --- |
| Hardware | Intel E5-2673 v3-processors 2,4 GHz (Haswell) gekoppeld SSD vCore = 1 PP (fysieke kernen) | Intel E5-2673 v4-processors 2,3 GHz (Broadwell) snel NVMe SSD, vCore = 1 LP (hyper-thread) |
| Het aantal vCores | 8, 16, 24 vCores | 4, 8, 16, 24, 32, 40, 64, 80 vCores |
| Maximale geheugen (geheugen/core-verhouding) | 7 GB per vCore<br/>Voeg meer vCores om op te halen van meer geheugen. | 5.1 GB per vCore<br/>Voeg meer vCores om op te halen van meer geheugen. |
| Maximum aantal In-Memory OLTP-geheugen | Limiet voor exemplaar: 3 GB per vCore<br/>Limieten voor databases:<br/> -8-core: 8 GB per database<br/> -16-core: 20 GB per database<br/> -24-core: 36 GB per database | Limiet voor exemplaar: 2,5 GB per vCore<br/>Limieten voor databases:<br/> -8-core: 13 GB per database<br/> -16-core: 32 GB per database |
| Maximumaantal exemplaren gereserveerd storage (Algemeen) |  8 TB | 8 TB |
| Maximumaantal exemplaren gereserveerd opslag (Business-kritische) | 1 TB | 1 TB, 2 TB of 4 TB, afhankelijk van het aantal kernen |

> [!IMPORTANT]
> Nieuwe Gen4 databases worden niet meer ondersteund in de regio AustraliaEast.

### <a name="service-tier-characteristics"></a>Service tier kenmerken

Beheerd exemplaar heeft twee Servicelagen: Algemeen gebruik en bedrijfskritiek. Deze versies bieden verschillende mogelijkheden, zoals beschreven in de onderstaande tabel:

| **Functie** | **Algemeen gebruik** | **Bedrijfskritiek** |
| --- | --- | --- |
| Het aantal vCores\* | Gen4: 8, 16, 24<br/>Gen5: 4, 8, 16, 24, 32, 40, 64, 80 | Gen4: 8, 16, 24, 32 <br/> Gen5: 4, 8, 16, 24, 32, 40, 64, 80 |
| Maximaal geheugen | Gen4: 56 GB - 168 GB (7GB/vCore)<br/>Gen5: 40.8 GB - 408 GB (5.1 GB/vCore)<br/>Voeg meer vCores om op te halen van meer geheugen. | Gen4: 56 GB - 168 GB (7GB/vCore)<br/>Gen5: 40.8 GB - 408 GB (5.1 GB/vCore)<br/>Voeg meer vCores om op te halen van meer geheugen. |
| Maximumaantal exemplaren gereserveerd opslaggrootte | -2 TB voor 4 vCores (alleen Gen5)<br/>-8 TB voor andere grootten | Gen4: 1 TB <br/> Gen5: <br/>-1 TB voor 4, 8, 16 vCores<br/>-2 TB voor 24 vCores<br/>-4 TB voor 32, 40, 64, 80 vCores |
| Maximale databasegrootte | Bepaald door de maximale opslagruimte per exemplaar | Bepaald door de maximale opslagruimte per exemplaar |
| Maximumaantal databases per exemplaar | 100 | 100 |
| Maximumaantal bestanden per exemplaar | Maximaal 280 | 32.767 bestanden per database |
| Gegevens/Log IOPS (bij benadering) | 500 - 7500 per bestand<br/>\*[De bestandsgrootte vergroten om op te halen meer IOPS](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage-performance#premium-storage-disk-sizes)| 11 K - 110 K (1375/vCore)<br/>Voeg meer vCores voor betere i/o-prestaties. |
| Logboek schrijven doorvoerlimiet | 3 MB/s per vCore<br/>Max 22 MB/s per exemplaar | 4 MB/s per vCore<br/>Max 48 MB/s per exemplaar|
| Doorvoer van gegevens (bij benadering) | 100 - 250 MB/s per bestand<br/>\*[Verhoog de bestandsgrootte voor betere i/o-prestaties](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage-performance#premium-storage-disk-sizes) | N/A |
| Latentie voor i/o-opslag (bij benadering) | 5-10 ms | 1-2 ms |
| Maximumgrootte van tempDB | 192 - 1,920 GB (24 GB per vCore)<br/>Voeg meer vCores als u wilt meer TempDB-ruimte. | Beperkt door de grootte van de max-exemplaar. Grootte van TempDB logboekbestand is momenteel beperkt tot 24GB/vCore. |
| Maximum aantal sessies | 30000 | 30000 |

> [!NOTE]
> - Gegevens- en logboekbestanden bestandsgrootte in de gebruiker en de systeemdatabases zijn opgenomen in de grootte van de instantie die wordt vergeleken met de maximale grootte opslaglimiet. Gebruik <a href="https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-master-files-transact-sql">sys.master_files</a> door het systeemweergave om te bepalen het totale aantal gebruikte ruimte door databases. Foutenlogboeken zijn niet permanent en worden niet opgenomen in de grootte. Back-ups zijn niet opgenomen in de opslagruimte.
> - Doorvoer en IOPS ook afhankelijk van het formaat van de pagina die niet expliciet is beperkt door een beheerd exemplaar.

## <a name="supported-regions"></a>Ondersteunde regio’s

Beheerde exemplaren kunnen alleen worden gemaakt in [ondersteunde regio's](https://azure.microsoft.com/global-infrastructure/services/?products=sql-database&regions=all). Voor het maken van een beheerd exemplaar in een regio die wordt momenteel niet ondersteund, kunt u [verzenden van een ondersteuningsaanvraag via Azure portal](#obtaining-a-larger-quota-for-sql-managed-instance).

## <a name="supported-subscription-types"></a>Ondersteunde abonnementstypen

Beheerd exemplaar ondersteunt momenteel implementatie alleen op de volgende typen abonnementen:

- [Enterprise Agreement (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/)
- [Pay-as-you-go](https://azure.microsoft.com/offers/ms-azr-0003p/)
- [Cloud serviceprovider (CSP)](https://docs.microsoft.com/partner-center/csp-documents-and-learning-resources)
- [Enterprise Dev/Test](https://azure.microsoft.com/offers/ms-azr-0148p/)
- [Pay-As-You-Go Dev/Test](https://azure.microsoft.com/offers/ms-azr-0023p/)
- [Abonnementen met maandelijkse Azure-tegoed voor Visual Studio-abonnees](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/)

## <a name="regional-resource-limitations"></a>Regionale resourcebeperkingen

Ondersteunde abonnementstypen mag een beperkt aantal resources per regio. Beheerd exemplaar heeft twee standaardlimieten per Azure-regio, afhankelijk van een type van het abonnementstype:

- **Subnet limiet**: Het maximale aantal subnetten waarop beheerde exemplaren zijn geïmplementeerd in een enkele regio.
- **vCore limiet**: Het maximale aantal vCores op dat kan worden geïmplementeerd voor alle instanties in één regio.

> [!Note]
> Deze limieten zijn de standaardinstellingen en geen technische beperkingen. De limieten meer op aanvraag kunnen worden door het maken van een speciale [verzoek tot ondersteuning in Azure portal](#obtaining-a-larger-quota-for-sql-managed-instance) als u meer beheerde exemplaren in de huidige regio. Als alternatief kunt u nieuwe beheerde exemplaren in een andere Azure-regio maken zonder te verzenden aanvragen voor ondersteuning.

De volgende tabel ziet u de regionale standaardlimieten voor ondersteunde abonnementen:

|Abonnementstype| Maximumaantal beheerd exemplaar subnetten | Maximumaantal eenheden vCore * |
| :---| :--- | :--- |
|Betalen naar gebruik|3|320|
|CSP |8 (en met 15 in sommige regio's **)|960 (1440 in sommige regio's **)|
|Pay-as-you-go Dev/Test|3|320|
|Enterprise Dev/Test|3|320|
|EA|8 (en met 15 in sommige regio's **)|960 (1440 in sommige regio's **)|
|Visual Studio Enterprise|2 |64|
|Visual Studio Professional en MSDN Platforms|2|32|

\* Wanneer u uw implementatie plant, houd rekening met dat een vCore Business kritieke (BC) (als gevolg van toegevoegde redundantie) 4 x meer capaciteit dan een vCore voor algemeen gebruik (GP) in beslag neemt. Ja, voor de berekeningen, 1 GP vCore = 1 vCore-eenheid en BC 1 vCore = 4 vCore-eenheden. Ter vereenvoudiging van uw verbruik analyse op basis van de standaardlimieten vatten het vCore-eenheden via alle subnetten in de regio waar beheerde exemplaren zijn geïmplementeerd en de resultaten vergelijken met de exemplaar-eenheid-limieten voor uw abonnementstype. **Maximumaantal eenheden vCore** geldt een limiet aan elk abonnement in een regio. Er is geen limiet per afzonderlijke subnetten, behalve dat de som van alle vCores geïmplementeerd in meerdere subnetten moet kleiner of gelijk aan **maximum aantal eenheden vCore**.

\*\* Groter subnet en vCore-limieten zijn beschikbaar in de volgende regio's: Australië-Oost, VS-Oost, VS-Oost 2, Noord-Europa, Zuid-centraal VS, Zuidoost-Azië, UK-Zuid, West-Europa, VS-West 2.

## <a name="obtaining-a-larger-quota-for-sql-managed-instance"></a>Het ophalen van een grotere quotum voor SQL beheerd exemplaar

Als u meer beheerde exemplaren in uw huidige regio's, verzendt u een ondersteuningsaanvraag om uit te breiden het quotum met behulp van de Azure portal.
Het proces voor het verkrijgen van een grotere quotum initiëren:

1. Open **Help en ondersteuning**, en klikt u op **nieuwe ondersteuningsaanvraag**.

   ![Help en ondersteuning](media/sql-database-managed-instance-resource-limits/help-and-support.png)
2. Op het tabblad basisbeginselen voor het nieuwe ondersteuningsaanvraag:
   - Voor **type probleem**, selecteer **limieten voor Service en -abonnement (quota)** .
   - Bij **Abonnement** selecteert u uw abonnement.
   - Voor **quotumtype**, selecteer **SQL Database Managed Instance**.
   - Voor **ondersteuningsplan**, selecteert u uw ondersteuningsplan.

     ![Probleem type quotum](media/sql-database-managed-instance-resource-limits/issue-type-quota.png)

3. Klik op **Volgende**.
4. Op de **probleem tabblad** voor de nieuwe ondersteuningsaanvraag:
   - Voor **ernst**, selecteert u de ernst van het probleem.
   - Voor **Details**, bieden aanvullende informatie over uw probleem, met inbegrip van foutberichten.
   - Voor **uploaden van het bestand**, koppelt u een bestand met meer informatie (maximaal 4 MB).

     ![Details van probleem](media/sql-database-managed-instance-resource-limits/problem-details.png)

     > [!IMPORTANT]
     > Er moet een geldige aanvraag omvatten:
     > - De regio in welk abonnement u limiet moet worden verhoogd.
     > - Vereiste aantal vCores per servicelaag in bestaande subnetten na het quotum verhogen (als een van de bestaande subnetten moet worden uitgebreid.
     > - Aantal nieuwe subnetten en totaal aantal vCores per servicelaag in de nieuwe subnetten vereist (als u implementeren van beheerde exemplaren in de nieuwe subnetten wilt).

5. Klik op **Volgende**.
6. Voer op het tabblad contact opnemen met informatie voor de nieuwe ondersteuningsaanvraag contactwijze (bijvoorbeeld e-mail of telefoon) en de gegevens van de contactpersoon.
7. Klik op **Create**.

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over het beheerde exemplaar [wat is er een beheerd exemplaar?](sql-database-managed-instance.md).
- Zie voor informatie over de prijzen [SQL-Database beheerd exemplaar prijzen](https://azure.microsoft.com/pricing/details/sql-database/managed/).
- Zie voor meer informatie over het maken van uw eerste beheerd exemplaar, [de snelstartgids](sql-database-managed-instance-get-started.md).
