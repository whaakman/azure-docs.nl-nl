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
ms.reviewer: carlrab, jovanpop, sachinp
manager: craigg
ms.date: 12/03/2018
ms.openlocfilehash: 1512098c29c8916a0486ed66b438654ba29f0601
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52968225"
---
# <a name="overview-azure-sql-database-managed-instance-resource-limits"></a>Overzicht van Azure SQL Database Managed Instance-resourcebeperkingen

In dit artikel biedt een overzicht van de resourcelimieten voor Azure SQL Database Managed Instance en bevat informatie over het maken van de aanvraag om regionale abonnement standaardlimieten te verhogen.

> [!NOTE]
> Zie voor andere beperkingen in de Managed Instance [vCore gebaseerde aankoopmodel](sql-database-managed-instance.md#vcore-based-purchasing-model) en [Managed Instance-Servicelagen](sql-database-managed-instance.md#managed-instance-service-tiers). Zie voor verschillen in ondersteunde functies en T-SQL statements [Functieverschillen](sql-database-features.md) en [ondersteuning voor T-SQL-instructie](sql-database-managed-instance-transact-sql-information.md).

## <a name="instance-level-resource-limits"></a>Bronlimieten op exemplaarniveau

Beheerd exemplaar bevat kenmerken en resourcelimieten die afhankelijk zijn van de onderliggende infrastructuur en -architectuur. Limieten, is afhankelijk van hardware genereren en service-laag.

### <a name="hardware-generation-characteristics"></a>Hardwarekenmerken genereren

Azure SQL Database Managed Instance kunnen worden geïmplementeerd op twee hardware generatie (Gen4 als Gen5). Hardwaregeneraties hebben andere kenmerken die worden beschreven in de volgende tabel:

|   | **Gen 4** | **Gen 5** |
| --- | --- | --- |
| Hardware | Intel E5-2673 v3-processors 2,4 GHz (Haswell) gekoppeld SSD vCore = 1 PP (fysieke kernen) | Intel E5-2673 v4-processors 2,3 GHz (Broadwell) snel eNVM SSD, vCore = 1 LP (hyper-thread) |
| Compute | 8, 16, 24 vCores | 8, 16, 24 uur per dag, 32, 40, 64, 80 vCores |
| Geheugen | 7 GB per vCore | 5.1 GB per vCore |
| Maximale opslag (Business-kritische) | 1 TB | 1 TB, 2 TB of 4 TB, afhankelijk van het aantal kernen |

### <a name="service-tier-characteristics"></a>Service tier kenmerken

Beheerd exemplaar heeft twee Servicelagen: algemeen gebruik en bedrijfskritiek. Deze versies bieden verschillende mogelijkheden, zoals beschreven in de onderstaande tabel:

| **Functie** | **Algemeen gebruik** | **Bedrijfskritiek** |
| --- | --- | --- |
| Het aantal vCores\* | Gen4: 8, 16, 24 uur per dag<br/>Gen5: 8, 16, 24 uur per dag, 32, 40, 64, 80 | Gen4: 8, 16, 24 uur per dag, 32 <br/> Gen5: 8, 16, 24 uur per dag, 32, 40, 64, 80 |
| Geheugen | Gen4: 56GB - 156GB<br/>Gen5: 44GB - 440GB<br/>\*In verhouding staan tot het aantal vCores | Gen4: 56GB - 156GB <br/> Gen5: 44GB - 440GB<br/>\*In verhouding staan tot het aantal vCores |
| Maximumgrootte van opslag | 8 TB | Gen 4: 1 TB <br/> Gen 5: <br/>-1 TB voor 8, 16 vcores uitvoert<br/>-2 TB voor 24 vCores<br/>-4 TB voor 32, 40, 64, 80 vCores |
| Maximale opslagruimte per database | Bepaald door de maximale opslagruimte per exemplaar | Bepaald door de maximale opslagruimte per exemplaar |
| Maximumaantal databases per exemplaar | 100 | 100 |
| Maximum aantal bestanden per exemplaar | Maximaal 280 | 32.767 bestanden per database |
| IOPS (bij benadering) | 500-7500 per bestand<br/>\*[Afhankelijk van de bestandsgrootte](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage-performance#premium-storage-disk-sizes) | 11K - 110K (1375 per vCore) |
| I/o-latentie (bij benadering) | 5-10 ms | 1-2 ms |
| Maximumgrootte van tempDB | 192 1920 GB (24 GB per vCore) | Bepaald door de maximale opslagruimte per exemplaar |

- Gebruikers- en systeemdatabases zijn opgenomen in de grootte van de instantie die wordt vergeleken met de maximale grootte opslaglimiet. Gebruik <a href="https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-master-files-transact-sql">sys.master_files</a> door het systeemweergave om te bepalen het totale aantal gebruikte ruimte door databases. Foutenlogboeken zijn niet permanent en worden niet opgenomen in de grootte. Back-ups zijn niet opgenomen in de opslagruimte.

## <a name="supported-regions"></a>Ondersteunde regio’s

Beheerde Instanced kan alleen worden gemaakt in [ondersteunde regio's](https://azure.microsoft.com/global-infrastructure/services/?products=sql-database&regions=all). Als u maken van een beheerd exemplaar in de regio die wordt momenteel niet ondersteund wilt, kunt u [verzoek om ondersteuning te verzenden via Azure portal](#obtaining-a-larger-quota-for-sql-managed-instance).

## <a name="supported-subscription-types"></a>Ondersteunde abonnementstypen

Beheerd exemplaar ondersteunt momenteel implementatie alleen op de volgende typen abonnementen:

- [Enterprise Agreement (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/)
- [Betalen per gebruik](https://azure.microsoft.com/offers/ms-azr-0003p/)
- [Cloud serviceprovider (CSP)](https://docs.microsoft.com/partner-center/csp-documents-and-learning-resources)

> [!NOTE]
> Deze beperking is tijdelijk. Typen voor nieuwe abonnementen worden in de toekomst ingeschakeld.

## <a name="regional-resource-limitations"></a>Regionale resourcebeperkingen

Ondersteunde abonnementstypen mag een beperkt aantal resources per regio. Beheerd exemplaar heeft twee standaardlimieten per Azure-regio, afhankelijk van een type van het abonnementstype:

- **Subnet limiet**: het maximum aantal subnetten waarop beheerde exemplaren zijn geïmplementeerd in een enkele regio.
- **Maximumaantal exemplaar**: het maximum aantal instanties dat kan worden geïmplementeerd in een enkele regio.

In de volgende tabel worden weergegeven regionale standaardlimieten voor ondersteunde abonnementen:

|Abonnementstype| Maximumaantal Managed Instance-subnetten | Maximumaantal exemplaren |Maximumaantal GP beheerde exemplaren *|Maximumaantal BC beheerde exemplaren *|
| :---| :--- | :--- |:--- |:--- |
|Betalen naar gebruik|1 *|4 *|4 *|1 *|
|CSP |1 *|4 *|4 *|1 *|
|EA|3 **|12 **|12 **|3 **|

\* U kunt ofwel 1 BC of 4 GP-exemplaren in één subnet implementeren, zodat het totaal aantal 'exemplaar eenheden' in het subnet nooit meer dan 4.

** Maximum aantal exemplaren in een servicelaag van toepassing als er geen exemplaren in een andere servicelaag zijn. Als u van plan bent om GP- en BC-instanties binnen hetzelfde subnet bevinden, maken gebruik van de volgende sectie als uitgangspunt voor toegestane combinaties. Als een eenvoudige regel, het totale aantal subnetten niet langer zijn dan 3 en het totale aantal eenheden van de sessie mag niet meer dan 12.

Deze limieten kunnen worden verhoogd door het maken van speciale [verzoek tot ondersteuning in Azure portal](#obtaining-a-larger-quota-for-sql-managed-instance) als u meer beheerde instanties in de huidige regio. Als alternatief, kunt u nieuwe beheerde instanties in een andere Azure-regio maken zonder te verzenden aanvragen voor ondersteuning.

> [!IMPORTANT]
> Wanneer u uw implementatie plant, houd rekening met dat een kritieke zakelijke (BC)-exemplaar (als gevolg van toegevoegde redundantie) in het algemeen 4 x meer capaciteit dan een algemeen doel (GP)-exemplaar worden verbruikt. Ja, voor de berekeningen, 1 exemplaar van de GP = 1 exemplaar eenheid en 1 BC instantie = 4 eenheden van het exemplaar. Ter vereenvoudiging van uw verbruik analyse op basis van de standaardlimieten, een overzicht maken van de exemplaar-eenheden via alle subnetten in de regio waar beheerde instanties zijn geïmplementeerd en vergelijk de resultaten met de exemplaar-eenheid-limieten voor uw abonnementstype.

## <a name="strategies-for-deploying-mixed-general-purpose-and-business-critical-instances"></a>Strategieën voor het implementeren van gemengde exemplaren voor algemeen gebruik en bedrijfskritiek

[Enterprise Agreement (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/) abonnementen kunnen hebben combinaties van BC- en GP-instanties. Er zijn echter enkele beperkingen met betrekking tot de plaatsing van de exemplaren in de subnetten.

> [!Note]
> [Betalen per gebruik](https://azure.microsoft.com/offers/ms-azr-0003p/) en [Cloud Service Provider (CSP)](https://docs.microsoft.com/partner-center/csp-documents-and-learning-resources) abonnementstypen kunnen hebben beide één bedrijfskritiek of omhoog naar 4 exemplaren voor algemeen gebruik.

De volgende voorbeelden betrekking op implementatie gevallen met niet-lege subnetten en GP- en BC gemengde service-lagen.

|Aantal subnetten|Subnet 1|Subnet 2|Subnet 3|
|:---|:---|:---|:---|
|1|BC 1 en maximaal 8 GP<br>BC 2 en maximaal 4 GP|N/A| N/A|
|2|0 BC, tot 4 GP|1 BC, tot 4 GP<br>2 BC, 0 GP|N/A|
|2|BC 1, 0 GP|0 BC, tot maximaal 8 GP<br>1 BC, tot 4 GP|N/A|
|2|2 BC, 0 GP|0 BC, tot 4 GP|N/A|
|3|BC 1, 0 GP|BC 1, 0 GP|0 BC, tot 4 GP|
|3|BC 1, 0 GP|0 BC, tot 4 GP|0 BC, tot 4 GP|

## <a name="obtaining-a-larger-quota-for-sql-managed-instance"></a>Het ophalen van een grotere quotum voor beheerd exemplaar van SQL

Als u meer beheerde instanties in uw huidige regio's, kunt u de ondersteuningsaanvraag om uit te breiden het quotum met behulp van Azure portal verzenden.
Het proces voor het verkrijgen van een grotere quotum initiëren:

1. Open **Help en ondersteuning**, en klikt u op **nieuwe ondersteuningsaanvraag**.

   ![Help en ondersteuning](media/sql-database-managed-instance-resource-limits/help-and-support.png)
2. Op het tabblad basisbeginselen voor het nieuwe ondersteuningsaanvraag:
   - Voor **type probleem**, selecteer **limieten voor Service en -abonnement (quota)**.
   - Bij **Abonnement** selecteert u uw abonnement.
   - Voor **quotumtype**, selecteer **SQL Database Managed Instance**.
   - Voor **ondersteuningsplan**, selecteert u uw ondersteuningsplan.

     ![Probleem type quotum](media/sql-database-managed-instance-resource-limits/issue-type-quota.png)

3. Klik op **Volgende**.
4. Op het tabblad probleem voor de nieuwe ondersteuningsaanvraag:
   - Voor **ernst**, selecteert u de ernst van het probleem.
   - Voor **Details**, bieden aanvullende informatie over uw probleem, met inbegrip van foutberichten.
   - Voor **uploaden van het bestand**, koppelt u een bestand met meer informatie (maximaal 4 MB).

     ![Probleemdetails](media/sql-database-managed-instance-resource-limits/problem-details.png)

     > [!IMPORTANT]
     > Er moet een geldige aanvraag omvatten:
     > - Regio in welk abonnement u limiet moet worden verhoogd
     > - Vereiste aantal exemplaren per servicelaag in bestaande subnetten na het quotum (als een van de bestaande subnetten moet worden uitgebreid verhogen
     > - Aantal nieuwe subnetten en totaal aantal exemplaren per servicelaag in de nieuwe subnetten vereist (als u implementeren van beheerde exemplaren in de nieuwe subnetten wilt).

5. Klik op **Volgende**.
6. Voer op het tabblad contact opnemen met informatie voor de nieuwe ondersteuningsaanvraag contactwijze (bijvoorbeeld e-mail of telefoon) en de gegevens van de contactpersoon.
7. Klik op **Create**.

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over Managed Instance [wat is een beheerd exemplaar?](sql-database-managed-instance.md).
- Zie voor informatie over de prijzen [prijzen van SQL Database Managed Instance](https://azure.microsoft.com/pricing/details/sql-database/managed/).
- Zie voor meer informatie over het maken van uw eerste Managed Instance, [snelstartgids](sql-database-managed-instance-get-started.md).
