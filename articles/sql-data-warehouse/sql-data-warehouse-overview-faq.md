---
title: Veelgestelde vragen over Azure SQL datawarehouse | Microsoft Docs
description: In dit artikel geeft een lijst van veelgestelde vragen over Azure SQL Data Warehouse van klanten en -ontwikkelaars
services: sql-data-warehouse
author: acomet
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.component: design
ms.date: 04/17/2018
ms.author: acomet
ms.reviewer: igorstan
ms.openlocfilehash: 35252b4d262833daef330a89f1874cc0835c480a
ms.sourcegitcommit: 1fb353cfca800e741678b200f23af6f31bd03e87
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/30/2018
ms.locfileid: "43306737"
---
# <a name="sql-data-warehouse-frequently-asked-questions"></a>SQL Data Warehouse Veelgestelde vragen over

## <a name="general"></a>Algemeen

V. Wat biedt SQL DW voor beveiliging van gegevens?

A. SQL DW biedt verschillende oplossingen voor het beveiligen van gegevens, zoals TDE en controle. Zie voor meer informatie, [Beveiliging].

V. Waar vind ik welke standaarden juridische of zakelijke SQL DW is naleven?

A. Ga naar de [Microsoft-naleving] -pagina voor verschillende compliance-aanbiedingen per product, zoals SOC en ISO. Eerst op naleving titel kiezen uit en vouw vervolgens Azure in Microsoft binnen de regeling vallen cloud services sectie aan de rechterkant van de pagina om te zien welke services zijn Azure-services compatibel zijn.

V. Kan ik Power BI verbinding maken?

A. Ja. Hoewel Power BI biedt ondersteuning voor directquery met SQL DW, het niet bedoeld voor een groot aantal gebruikers of realtime-gegevens. Voor gebruik in productieomgevingen van Power BI, wordt u aangeraden Power BI naast Azure Analysis Services of Analysis Service IaaS. 

V. Wat zijn de capaciteitslimieten van SQL Data Warehouse?

A. Zie onze huidige [Capaciteitslimieten] pagina. 

V. Waarom wordt mijn schaal/onderbreken/hervatten duurt zo lang?

A. Een verscheidenheid aan factoren kan invloed hebben op de tijd voor bewerkingen voor compute. Een algemene aanvraag voor het terugdraaien van de transactionele langlopende bewerkingen is. Wanneer een bewerking voor schalen of onderbreken wordt gestart, wordt alle binnenkomende sessies worden geblokkeerd en query's zijn geleegd. Als u wilt het systeem in een stabiele status laat, worden transacties teruggedraaid terug voordat een bewerking kan beginnen. Hoe groter het nummer en grotere grootte van het logboekbestand van transacties, hoe langer de bewerking wordt gestopt het systeem herstellen naar een stabiele status.

## <a name="user-support"></a>Gebruikersondersteuning

V. Ik heb een functieaanvraag waar dien ik deze?

A. Als u een functie-aanvraag hebt, dient u dit op onze [UserVoice] pagina

V. Hoe kan ik x?

A. Voor hulp bij het ontwikkelen met SQL Data Warehouse, kunt u vragen stellen op onze [Stack Overflow] pagina. 

V. Hoe dien ik een ondersteuningsticket?

A. [Ondersteuningstickets] kunnen worden ingediend via Azure portal.

## <a name="sql-languagefeature-support"></a>Ondersteuning voor SQL-taal/functie 

V. Welke gegevenstypen biedt ondersteuning voor SQL Data Warehouse?

A. SQL Data Warehouse Zie [gegevenstypen].

V. Welke tabelfuncties ondersteund?

A. Hoewel SQL Data Warehouse veel functies ondersteunt, sommige worden niet ondersteund en worden beschreven in [Niet-ondersteunde tabelfuncties].

## <a name="tooling-and-administration"></a>Hulpprogramma's en -beheer

V. U bieden ondersteuning voor Database-projecten in Visual Studio.

A. We ondersteunen op dit moment geen Database-projecten in Visual Studio voor SQL Data Warehouse. Als u een stem om op te halen van deze functie cast-conversie uitvoeren wilt, gaat u naar onze Uservoice [Verzoek om de functie van de database-projecten].

V. SQL Data Warehouse biedt ondersteuning voor REST-API's?

A. Ja. De meeste REST-functionaliteit die kan worden gebruikt met SQL Database is ook beschikbaar met SQL Data Warehouse. Vindt u informatie in de documentatie van de REST API of [MSDN].


## <a name="loading"></a>Bezig met laden

V. Welke-clientstuurprogramma's ondersteund?

A. Ondersteuning voor stuurprogramma's voor DW is te vinden in de [verbindingsreeksen] pagina

V: voor welke indelingen worden ondersteund door PolyBase met SQL Data Warehouse?

A: Orc, RC, Parquet en platte tekst met scheidingstekens

V: wat kan ik verbinding maken met van SQL DW met PolyBase? 

A: [Azure Data Lake Store] en [Azure Storage-Blobs]

V: is berekening pushdown mogelijk bij het verbinden met Azure Storage-Blobs of ADLS? 

A: SQL DW PolyBase communiceert Nee, alleen de opslagonderdelen. 

V: kan ik verbinding maken met HDI?

A: HDI kunt ADLS of WASB gebruiken als de HDFS-laag. Als u een als uw HDFS-laag, kunt u die gegevens laden in SQL DW. U kunt echter pushdown berekening met het HDI-exemplaar niet genereren. 

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over SQL Data Warehouse als geheel, onze [overzicht] pagina.


<!-- Article references -->
[UserVoice]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[Verbindingsreeksen]: ./sql-data-warehouse-connection-strings.md
[Stack Overflow]: http://stackoverflow.com/questions/tagged/azure-sqldw
[Ondersteuningstickets]: ./sql-data-warehouse-get-started-create-support-ticket.md
[Beveiliging]: ./sql-data-warehouse-overview-manage-security.md
[Microsoft-naleving]: https://www.microsoft.com/en-us/trustcenter/compliance/complianceofferings
[Capaciteitslimieten]: ./sql-data-warehouse-service-capacity-limits.md
[gegevenstypen]: ./sql-data-warehouse-tables-data-types.md
[Niet-ondersteunde tabelfuncties]: ./sql-data-warehouse-tables-overview.md#unsupported-table-features
[Azure Data Lake Store]: ./sql-data-warehouse-load-from-azure-data-lake-store.md
[Azure Storage-Blobs]: ./sql-data-warehouse-load-from-azure-blob-storage-with-polybase.md
[Verzoek om de functie van de database-projecten]: https://feedback.azure.com/forums/307516-sql-data-warehouse/suggestions/13313247-database-project-from-visual-studio-to-support-azu
[MSDN]: https://msdn.microsoft.com/library/azure/mt163685.aspx
[Overzicht]: ./sql-data-warehouse-overview-faq.md
