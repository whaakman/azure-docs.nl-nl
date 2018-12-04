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
ms.openlocfilehash: ba79d2d5ed4350960af1b92ee863595e3540a1d2
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52843924"
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

## <a name="improve-redis-cache-performance-and-reliability"></a>Redis-Cache-prestaties en betrouwbaarheid verbeteren

Advisor identificeert waarbij prestaties kan nadelig worden beïnvloed door hoog geheugengebruik, serverbelasting, bandbreedte van het netwerk of een groot aantal clientverbindingen Redis Cache-exemplaren. Advisor biedt ook aanbevolen procedures voor aanbevelingen om te voorkomen van potentiële problemen. Zie voor meer informatie over aanbevelingen voor de Redis-Cache [Redis Cache Advisor](https://azure.microsoft.com/documentation/articles/cache-configure/#redis-cache-advisor).


## <a name="improve-app-service-performance-and-reliability"></a>App Service-prestaties en betrouwbaarheid verbeteren

Azure Advisor kan worden geïntegreerd met aanbevelingen voor het verbeteren van de ervaring van uw App Services en relevante platformmogelijkheden detecteren. Voorbeelden van aanbevelingen voor de App-Services zijn:
* Detectie van exemplaren waar geheugen of CPU-resources worden uitgeput door app-runtimes met opties voor risicobeperking.
* Detectie van exemplaren waar collocating resources, zoals web-apps en -databases kan verbeteren, prestaties en lagere kosten. 

Zie voor meer informatie over aanbevelingen voor de App Services [Best Practices voor Azure App Service](https://azure.microsoft.com/documentation/articles/app-service-best-practices/).

## <a name="remove-data-skew-on-your-sql-data-warehouse-table-to-increase-query-performance"></a>Gegevensverschil op uw SQL datawarehouse-tabel te verhogen van de prestaties van query's verwijderen

Gegevensverschil kan leiden tot onnodige gegevens knelpunten in verkeer of resource biedt bij het uitvoeren van uw workload. Advisor detecteert distributiegegevens scheeftrekken groter is dan 15% is en aangeraden dat u uw gegevens distribueren en terugkeren naar uw tabel distributie sleutel selecties. Zie voor meer informatie over het identificeren en verwijderen van scheeftrekken [probleemoplossing scheeftrekken](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-distribute#how-to-tell-if-your-distribution-column-is-a-good-choice).

## <a name="create-or-update-outdated-table-statistics-on-your-sql-data-warehouse-table-to-increase-query-performance"></a>Maken of bijwerken van verouderde tabelstatistieken op uw SQL datawarehouse-tabel voor betere queryprestaties

Advisor identificeert de tabellen die u geen recente hebt [tabelstatistieken](https://docs.microsoft.com/azure/sql-data-warehouse/sql-data-warehouse-tables-statistics) en de gebruiker wordt aanbevolen maken of bijwerken van statistieken tabel. Query's optimaliseren up-to-date statische waarden gebruikt om te schatten van de kardinaliteit of het aantal rijen in het queryresultaat waarmee het queryoptimalisatieprogramma om een hoge kwaliteit queryplan voor de snelste prestaties te maken voor de SQL data warehouse.

## <a name="migrate-your-storage-account-to-azure-resource-manager-to-get-all-of-the-latest-azure-features"></a>Uw Storage-Account migreren naar Azure Resource Manager om de nieuwste functies van Azure

Migreer uw Storage-Account-implementatiemodel naar Azure Resource Manager (ARM) om te profiteren van sjabloonimplementaties, extra beveiligingsopties en de mogelijkheid om te upgraden naar een GPv2-account voor het gebruik van de nieuwste functies van Azure Storage. Advisor identificeert een zelfstandige storage-accounts die van het klassieke implementatiemodel gebruikmaken en beveelt migreren naar het ARM-implementatiemodel. 

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

