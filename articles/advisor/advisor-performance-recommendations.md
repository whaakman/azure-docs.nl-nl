---
title: Azure Advisor prestatieaanbevelingen | Microsoft Docs
description: Advisor gebruiken om de prestaties van uw Azure-implementaties te optimaliseren.
services: advisor
documentationcenter: NA
author: kasparks
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: advisor
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/16/2016
ms.author: kasparks
ms.openlocfilehash: 3caf838fec3a5c0ab847ded85b269df7a66859e0
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/14/2019
ms.locfileid: "54266742"
---
# <a name="advisor-performance-recommendations"></a>Advisor-aanbevelingen voor prestaties

Azure Advisor-aanbevelingen voor prestaties te verbeteren en de reactiesnelheid van uw bedrijfskritische toepassingen. U kunt ook aanbevelingen voor prestaties van de Advisor krijgen bij de **prestaties** van de Advisor-dashboard.

## <a name="reduce-dns-time-to-live-on-your-traffic-manager-profile-to-fail-over-to-healthy-endpoints-faster"></a>Time to live van uw Traffic Manager-profiel sneller failover naar de eindpunten in orde van DNS-beperken

[Tijd naar Live (TTL) instellingen](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-performance-considerations) op uw Traffic Manager-profiel kunt u opgeven hoe snel om over te schakelen eindpunten als een bepaald eindpunt niet meer reageert op query's. Vermindering van de TTL-waarden, betekent dat clients worden doorgestuurd naar een functionerende eindpunten sneller.

Azure Advisor identificeert Traffic Manager-profielen met een langer TTL geconfigureerd en aanbevolen configuratie van de TTL-waarde 20 seconden of 60 seconden, afhankelijk van of u het profiel is geconfigureerd voor [Fast Failover](https://azure.microsoft.com/roadmap/fast-failover-and-tcp-probing-in-azure-traffic-manager/).

## <a name="improve-database-performance-with-sql-db-advisor"></a>De prestaties van de database verbeteren met SQL DB Advisor

Advisor biedt u een consistente, geconsolideerde weergave van de aanbevelingen voor al uw Azure-resources. Het is geïntegreerd met SQL Database Advisor voor aanbevelingen voor het verbeteren van de prestaties van uw SQL Azure-database. SQL Database Advisor beoordeelt de prestaties van uw SQL Azure-databases door het analyseren van uw gebruiksgeschiedenis. Vervolgens biedt aanbevelingen die bij uitstek geschikt zijn voor het uitvoeren van de typische werkbelasting van de database.

> [!NOTE]
> Om aanbevelingen te krijgen, moet een database over een week van het gebruik van, en in de afgelopen week moet er activiteit zijn consistent. SQL Database Advisor kunt eenvoudiger voor consistente querypatronen dan voor willekeurige pieken van activiteit optimaliseren.

Zie voor meer informatie over SQL Database Advisor [SQL Database Advisor](https://azure.microsoft.com/documentation/articles/sql-database-advisor/).

## <a name="improve-app-service-performance-and-reliability"></a>App Service-prestaties en betrouwbaarheid verbeteren

Azure Advisor kan worden geïntegreerd met aanbevelingen voor het verbeteren van de ervaring van uw App Services en relevante platformmogelijkheden detecteren. Voorbeelden van aanbevelingen voor de App-Services zijn:
* Detectie van exemplaren waar geheugen of CPU-resources worden uitgeput door app-runtimes met opties voor risicobeperking.
* Detectie van exemplaren waar collocating resources, zoals web-apps en -databases kan verbeteren, prestaties en lagere kosten.

Zie voor meer informatie over aanbevelingen voor de App Services [Best Practices voor Azure App Service](https://azure.microsoft.com/documentation/articles/app-service-best-practices/).

## <a name="use-managed-disks-to-prevent-disk-io-throttling"></a>Managed Disks gebruiken om te voorkomen dat de-i/o-schijfbeperking

Advisor identificeert virtuele machines die deel uitmaken van een storage-account dat de schaalbaarheidsdoel bereikt. Dit maakt het vatbaar voor i/o-beperking. Advisor wordt aangeraden deze virtuele machines Managed Disks te gebruiken om te voorkomen dat de systeemprestaties.

## <a name="improve-the-performance-and-reliability-of-virtual-machine-disks-by-using-premium-storage"></a>De prestaties en betrouwbaarheid van virtuele-machineschijven verbeteren met behulp van Premium Storage

Advisor identificeert virtuele machines met standard-schijven waarvoor een groot aantal transacties in uw storage-account en wordt aanbevolen een upgrade naar premium-schijven. 

Azure Premium Storage voorziet in ondersteuning voor hoge prestaties en lage latentie schijven voor virtuele machines die I/O-intensieve workloads uitvoeren. VM-schijven die gebruikmaken van premium storage-accounts worden gegevens opgeslagen op SSD-schijven (SSD's). Voor de beste prestaties voor uw toepassing, wordt u aangeraden dat u alle schijven van virtuele machines waarvoor hoge IOPS naar premium storage gemigreerd.

## <a name="remove-data-skew-on-your-sql-data-warehouse-table-to-increase-query-performance"></a>Gegevensverschil op uw SQL datawarehouse-tabel te verhogen van de prestaties van query's verwijderen

Gegevensverschil kan leiden tot onnodige gegevens knelpunten in verkeer of resource biedt bij het uitvoeren van uw workload. Advisor detecteert distributiegegevens scheeftrekken groter is dan 15% is en aangeraden dat u uw gegevens distribueren en terugkeren naar uw tabel distributie sleutel selecties. Zie voor meer informatie over het identificeren en verwijderen van scheeftrekken [probleemoplossing scheeftrekken](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-distribute#how-to-tell-if-your-distribution-column-is-a-good-choice).

## <a name="create-or-update-outdated-table-statistics-on-your-sql-data-warehouse-table-to-increase-query-performance"></a>Maken of bijwerken van verouderde tabelstatistieken op uw SQL datawarehouse-tabel voor betere queryprestaties

Advisor identificeert de tabellen die u geen recente hebt [tabelstatistieken](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-statistics) en de gebruiker wordt aanbevolen maken of bijwerken van statistieken tabel. Query's optimaliseren up-to-date statische waarden gebruikt om te schatten van de kardinaliteit of het aantal rijen in het queryresultaat waarmee het queryoptimalisatieprogramma om een hoge kwaliteit queryplan voor de snelste prestaties te maken voor de SQL data warehouse.

## <a name="scale-up-to-optimize-cache-utilization-on-your-sql-data-warehouse-tables-to-increase-query-performance"></a>Opschalen naar het Optimaliseer het gebruik van de cache voor uw SQL Data Warehouse-tabellen voor betere queryprestaties

Azure Advisor detecteert als uw SQL Data Warehouse beschikt over hoge cache percentage gebruikt en een lage percentage bereikt. Hiermee wordt aangegeven hoge cache verwijdering die kan invloed hebben op de prestaties van uw SQL Data Warehouse. Advisor kan erop wijzen dat u omhoog schalen van uw SQL Data Warehouse om ervoor te zorgen voldoende capaciteit cache voor uw werkbelasting die u toewijst.

## <a name="convert-sql-data-warehouse-tables-to-replicated-tables-to-increase-query-performance"></a>SQL Data Warehouse-tabellen omzetten in gerepliceerde tabellen voor betere queryprestaties

Advisor identificeert de tabellen die geen gerepliceerde tabellen zijn, maar veel voordeel hebben van het converteren van en stelt deze tabellen te converteren. Aanbevelingen zijn gebaseerd op de grootte van gerepliceerde tabel, het aantal kolommen, tabel Distributietype en het aantal partities van de SQL Data Warehouse-tabel. Aanvullende methodiek kan worden opgegeven in de aanbeveling voor context. Zie voor meer informatie over hoe deze aanbeveling wordt bepaald, [aanbevelingen voor SQL Data Warehouse](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-concept-recommendations#replicate-tables). 

## <a name="migrate-your-storage-account-to-azure-resource-manager-to-get-all-of-the-latest-azure-features"></a>Uw Storage-Account migreren naar Azure Resource Manager om de nieuwste functies van Azure

Migreer uw Storage-Account-implementatiemodel naar Azure Resource Manager (ARM) om te profiteren van sjabloonimplementaties, extra beveiligingsopties en de mogelijkheid om te upgraden naar een GPv2-account voor het gebruik van de nieuwste functies van Azure Storage. Advisor identificeert een zelfstandige storage-accounts die van het klassieke implementatiemodel gebruikmaken en beveelt migreren naar het ARM-implementatiemodel.

> [!NOTE]
> Klassieke waarschuwingen in Azure Monitor is aangekondigd buiten gebruik stellen in juni 2019, het wordt aanbevolen dat u uw klassieke storage-account upgraden naar ARM wilt behouden waarschuwingen functionaliteit met het nieuwe platform. Zie voor meer informatie, [buiten gebruik stellen met klassieke waarschuwingen](https://azure.microsoft.com/updates/classic-alerting-monitoring-retirement/).

## <a name="how-to-access-performance-recommendations-in-advisor"></a>Toegang tot de aanbevelingen voor prestaties in Advisor

1. Aanmelden bij de [Azure-portal](https://portal.azure.com), en open vervolgens [Advisor](https://aka.ms/azureadvisordashboard).

2.  Klik op de Advisor-dashboard op de **prestaties** tabblad.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over aanbevelingen van Advisor:

* [Inleiding tot Advisor](advisor-overview.md)
* [Aan de slag met Advisor](advisor-get-started.md)
* [Aanbevelingen van Advisor-kosten](advisor-performance-recommendations.md)
* [Advisor-aanbevelingen voor hoge beschikbaarheid](advisor-high-availability-recommendations.md)
* [Advisor-aanbevelingen voor beveiliging](advisor-security-recommendations.md)
