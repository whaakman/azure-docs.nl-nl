---
title: Veelgestelde vragen over Azure SQL datawarehouse | Microsoft Docs
description: In dit artikel worden uit de veelgestelde vragen over Azure SQL Data Warehouse van klanten en ontwikkelaars
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: johnmac
editor: 
ms.assetid: 812CA525-3BF3-49DF-8DF3-FB4342464F4F
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: overview
ms.date: 3/1/2017
ms.author: elbutter;barbkess
ms.openlocfilehash: 4c00710ecc0c91f8407eca81b78176075fcbd6ad
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="sql-data-warehouse-frequently-asked-questions"></a>SQL Data Warehouse Veelgestelde vragen over

## <a name="general"></a>Algemeen

Q. Wat biedt SQL DW voor beveiliging van gegevens?

A. SQL DW biedt verschillende oplossingen voor het beveiligen van gegevens, zoals TDE en controle. Zie voor meer informatie [beveiliging].

Q. Waar vind ik controleren welke standaarden wettelijke of zakelijke is SQL DW naleven?

A. Ga naar de [Microsoft Compliance] pagina voor verschillende producten voor naleving door product zoals SOC en ISO. Eerst kiezen door naleving titel vervolgens Azure Vouw in het Microsoft-scope services gedeelte aan de rechterkant van de pagina om te zien welke services zijn Azure services compatibel zijn.

Q. Kan ik verbinden met Power BI?

A. Ja. Hoewel Power BI ondersteunt de directe query met SQL DW, het niet bedoeld voor een groot aantal gebruikers of realtime-gegevens. Voor gebruik in productieomgevingen van Power BI, wordt u aangeraden PowerBI naast Azure Analysis Services of Analysis Service IaaS. 

Q. Wat zijn de capaciteitslimiet van SQL Data Warehouse?

A. Zie onze huidige [Capaciteitslimieten] pagina. 

Q. Waarom wordt mijn schaal/onderbreken/hervatten duurt te lang waardoor?

A. Een verscheidenheid aan factoren kan invloed hebben op de tijd voor de compute-beheerbewerkingen. Als u een algemeen case voor transactionele terugdraaien langlopende bewerkingen is. Wanneer een bewerking voor schaal of onderbreken is gestart, worden alle binnenkomende sessies geblokkeerd en query's zijn geleegd. Om het systeem in een stabiele status laat, worden transacties teruggedraaid terug voordat een bewerking kan beginnen. Langer zijn dan het getal en grotere grootte van het logboekbestand van transacties, hoe langer de bewerking wordt gestopt een stabiele status van het systeem herstellen.

## <a name="user-support"></a>Gebruikersondersteuning

Q. Ik heb een aanvraag functie waar kan ik verzenden?

A. Als u een aanvraag van de functie, dient u deze op onze [UserVoice] pagina

Q. Hoe kan ik doen x?

A. Voor hulp bij het ontwikkelen met SQL Data Warehouse, kunt u vragen stellen op onze [Stack Overflow] pagina. 

Q. Hoe verzend ik een ondersteuningsticket

A. [Ondersteuningstickets] kunnen worden ingediend via Azure portal.

## <a name="sql-languagefeature-support"></a>Ondersteuning voor SQL-taal of onderdeel 

Q. Welke gegevenstypen biedt ondersteuning voor SQL Data Warehouse?

A. SQL Data Warehouse Zie [gegevenstypen].

Q. Welke tabelfuncties ondersteund?

A. Terwijl de SQL Data Warehouse ondersteunt vele functies, sommige worden niet ondersteund en worden beschreven in [niet-ondersteunde functies van de tabel].

## <a name="tooling-and-administration"></a>Tools en beheer

Q. U bieden ondersteuning voor Database-projecten in Visual Studio.

A. We ondersteunen momenteel geen Database-projecten in Visual Studio voor SQL Data Warehouse. Als u converteren van een stem wilt ophalen van deze functie, gaat u naar onze User Voice [Database-projecten functie aanvraag].

Q. Ondersteunt SQL Data Warehouse REST-API's?

A. Ja. De meeste REST-functionaliteit die kan worden gebruikt met SQL Database is ook beschikbaar met SQL Data Warehouse. U vindt informatie in de documentatie voor REST API of [MSDN].


## <a name="loading"></a>Bezig met laden

Q. Welke clientstuurprogramma's ondersteund?

A. Ondersteuning voor stuurprogramma's voor DW vindt u op de [verbindingsreeksen] pagina

V: wat bestandsindelingen worden ondersteund door PolyBase met SQL Data Warehouse?

A: Orc, RC parketvloeren en platte tekst met scheidingstekens

V: wat kan ik verbinding maken met uit SQL DW met PolyBase? 

A: [Azure Data Lake Store] en [Azure Storage-Blobs]

V: is berekening naar beneden duwen mogelijk bij het verbinden met Azure Storage-Blobs of ADLS? 

A: SQL DW PolyBase communiceert Nee, alleen de opslagonderdelen. 

V: kan ik verbinding maken met HDI?

A: HDI kunt ADLS of WASB gebruiken als de HDFS-laag. Als u als uw HDFS-laag hebt, kunt u die gegevens laden in SQL DW. U kunt echter naar beneden duwen berekeningen met het HDI-exemplaar niet genereren. 

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over SQL Data Warehouse als geheel onze [overzicht] pagina.


<!-- Article references -->
[UserVoice]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[verbindingsreeksen]: ./sql-data-warehouse-connection-strings.md
[Stack Overflow]: http://stackoverflow.com/questions/tagged/azure-sqldw
[Ondersteuningstickets]: ./sql-data-warehouse-get-started-create-support-ticket.md
[beveiliging]: ./sql-data-warehouse-overview-manage-security.md
[Microsoft Compliance]: https://www.microsoft.com/en-us/trustcenter/compliance/complianceofferings
[Capaciteitslimieten]: ./sql-data-warehouse-service-capacity-limits.md
[gegevenstypen]: ./sql-data-warehouse-tables-data-types.md
[niet-ondersteunde functies van de tabel]: ./sql-data-warehouse-tables-overview.md#unsupported-table-features
[Azure Data Lake Store]: ./sql-data-warehouse-load-from-azure-data-lake-store.md
[Azure Storage-Blobs]: ./sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md
[Database-projecten functie aanvraag]: https://feedback.azure.com/forums/307516-sql-data-warehouse/suggestions/13313247-database-project-from-visual-studio-to-support-azu
[MSDN]: https://msdn.microsoft.com/en-us/library/azure/mt163685.aspx
[overzicht]: ./sql-data-warehouse-overview-faq.md