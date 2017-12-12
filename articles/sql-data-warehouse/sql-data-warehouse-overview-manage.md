---
title: Beheren van databases in Azure SQL Data Warehouse | Microsoft Docs
description: Overzicht van het beheren van databases in SQL Data Warehouse. Beheerhulpprogramma's voor dwu's en scale-out-prestaties queryprestaties tot stand brengen van goede beveiligingsbeleid en het herstellen van een database van beschadigde gegevens of van een regionale uitval probleemoplossing bevat.
services: sql-data-warehouse
documentationcenter: NA
author: kevinvngo
manager: jhubbard
editor: 
ms.assetid: 8576fdb3-71fe-4b3b-a4e0-5e8a7f148acf
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
ms.date: 10/31/2016
ms.author: kevin;barbkess
ms.openlocfilehash: d7b81c12c31fe7de40acca6baa8972e65c306ee0
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="manage-databases-in-azure-sql-data-warehouse"></a>Databases in Azure SQL Data Warehouse beheren
SQL Data Warehouse automatiseert veel aspecten van het beheer van uw databases. Bijvoorbeeld, als u wilt schalen prestaties hoeft u alleen te aanpassen en betaalt voor het juiste niveau van rekenresources en laat SQL Data Warehouse het werk van uitbreiden en terug te schalen.

Ongetwijfeld wilt bewaken van uw werkbelasting om te bepalen van uw prestatievereisten past, evenals een langlopende query's oplossen. U moet ook enkele beveiligingstaken voor het beheren van machtigingen voor gebruikers en aanmeldingen uitvoeren.

Dit overzicht bevat informatie over deze aspecten van het beheer van SQL Data Warehouse.

* Beheerhulpprogramma's
* Scale Compute
* Onderbreken en hervatten
* Aanbevolen procedures voor prestaties
* Bewaking van de query
* Beveiliging
* Back-ups en herstellen

## <a name="management-tools"></a>Beheerhulpprogramma's
U kunt verschillende hulpprogramma's gebruiken voor het beheren van databases in SQL Data Warehouse. Als u databases beheren, kunt u het hulpprogramma voorkeuren voor elk type taak die u moet uitvoeren kunt ontwikkelen.

### <a name="azure-portal"></a>Azure Portal
De [Azure-portal] [ Azure portal] is een web-portal kunt u maken, bijwerken, en verwijdert u de databases en databaseresources bewaken. Dit hulpprogramma is een goede is als u alleen aan de slag gaat met Azure, het beheren van een klein aantal datawarehouse-databases of snel een bewerking wilt uitvoeren.

Als u wilt beginnen met de Azure-portal, Zie [maken van een SQL Data Warehouse (Azure-portal)][Create a SQL Data Warehouse (Azure portal)].

### <a name="sql-server-data-tools-in-visual-studio"></a>SQL Server Data Tools in Visual Studio
[SQL Server Data Tools] [ SQL Server Data Tools] (SSDT) in Visual Studio kunt u verbinding wil maken, beheren en ontwikkelen van uw databases. Als u bent een toepassingsontwikkelaar bekend bent met Visual Studio of andere geïntegreerde ontwikkelomgevingen (IDE), probeert u SSDT in Visual Studio.

SSDT bevat de SQL Server Object Explorer waarmee u visualiseren, verbinding maken en uitvoeren van scripts in SQL Data Warehouse-databases. Voor snel verbinding met SQL Data Warehouse, klikt u gewoon op de **openen in Visual Studio** knop in de opdrachtbalk wanneer een details weergeven van de database in de Azure portal.  

Als u wilt beginnen met SSDT in Visual Studio, Zie [Query Azure SQL Data Warehouse met Visual Studio][Query Azure SQL Data Warehouse with Visual Studio].

### <a name="command-line-tools"></a>Opdrachtregelprogramma's
Opdrachtregelprogramma's zijn ideaal voor het automatiseren van uw werkbelastingen.  PowerShell en sqlcmd zijn goede kunt op twee manieren om uw processen te automatiseren.  U wordt aangeraden deze hulpprogramma's voor het beheren van een groot aantal logische servers en de wijzigingen van de resources in een productieomgeving implementeert, zoals de benodigde taken kunnen worden vastgelegd in een script en vervolgens automatisch.

### <a name="dynamic-management-views"></a>Dynamische beheerweergaven
DMV's zijn de boter brood en van het beheer van SQL Data Warehouse. Er is een vrijwel alle informatie in de portal waarmee afhankelijk van de DMV's. Zie voor een lijst met SQL Data Warehouse DMV's [SQL Data Warehouse systeemweergaven][SQL Data Warehouse system views].

Om te beginnen, Zie [Connect en query met sqlcmd][Connect and query with sqlcmd], en [maken van een database (PowerShell)][Create a database (PowerShell)].

## <a name="scale-compute"></a>Scale compute
In SQL Data Warehouse, kunt u snel de prestaties af of terug te vergroten of verkleinen van rekenresources van CPU, geheugen en i/o-bandbreedte te schalen. Als u wilt schalen prestaties, hoeft u is pas het aantal datawarehouse Units (dwu's) die SQL Data Warehouse worden toegewezen aan de database. SQL Data Warehouse snel de wijziging aanbrengt en alle onderliggende wijzigingen aan de hardware of software worden verwerkt.

Zie voor meer informatie over het schalen van dwu's, [prestaties schalen].

## <a name="pause-and-resume"></a>Onderbreken en hervatten
Als u wilt opslaan kosten, kunt u onderbreken en hervatten van compute bronnen op de aanvraag. Bijvoorbeeld als u niet de database's nachts en in het weekend gebruikt, kunt u tijdens de tijdstippen onderbreken en hervatten gedurende de dag. U geen afgeschreven voor dwu's terwijl de database is onderbroken.

Zie voor meer informatie [rekencapaciteit onderbreken][Pause compute], en [compute hervatten][Resume compute].

## <a name="performance-best-practices"></a>Aanbevolen procedures voor prestaties
Wanneer aan de slag met een nieuwe technologie, bespaart tips en trucs die best meteen vanaf het begin werken detecteren u veel tijd.  Vindt u aanbevolen procedures in de gehele veel van de onderwerpen over onze.

Zie voor veel een samenvatting van de meest belangrijke overwegingen bij het ontwikkelen van uw werkbelasting [aanbevolen procedures van SQL Data Warehouse][SQL Data Warehouse Best Practices].

## <a name="query-monitoring"></a>Bewaking van de query
Soms een query te lang wordt uitgevoerd, maar u niet weet welke is klikken. SQL Data Warehouse heeft dynamische beheerweergaven (DMV's) die u gebruiken kunt om te achterhalen welke query is te lang duurt.

Langlopende query's, Zie [bewaken van uw werkbelasting via DMV's][Monitor your workload using DMVs].

## <a name="security"></a>Beveiliging
Om te blijven van een beveiligd systeem, moet u zich op de waarschuwing en bescherming tegen onbevoegde toegang tot elk type. Er moet een beveiligingssysteem om ervoor te zorgen dat firewall-regels is voldaan, zodat alleen-geautoriseerd IP-adressen verbinding kunnen maken. Het moet de juiste verificatiegegevens van de gebruikersreferenties. Nadat een gebruiker is verbonden met de database, moet de gebruiker alleen machtigingen voor het uitvoeren van een minimum aantal acties hebben. U kunt versleuteling gebruiken om gegevens te beveiligen. Het is ook belangrijk om controle- en bij te houden, zodat u gebeurtenissen keren kunt als er verdachte activiteiten.

Voor meer informatie over het beheren van beveiliging, Ga naar de [beveiligingsoverzicht][Security overview].

## <a name="backup-and-restore"></a>Back-ups en herstellen
Met betrouwbare backps van uw gegevens is een essentieel onderdeel van een productiedatabase. SQL Data Warehouse houdt uw gegevens veilig door automatisch back-ups van uw actieve databases met regelmatige tussenpozen. Deze back-ups kunnen u scenario's waar hebt u uw gegevens beschadigd of per ongeluk verwijderd van uw gegevens of de database herstellen.  Zie voor de back-upschema gegevens bewaarbeleid en hoe u een database [herstellen vanuit een momentopname][Restore from snapshot].

## <a name="next-steps"></a>Volgende stappen
Met behulp van goed ontwerp principes maakt het eenvoudiger voor het beheren van uw databases in SQL Data Warehouse. Ga voor meer informatie naar de [overzicht voor ontwikkelaars][Development overview].

<!--Image references-->

<!--Article references-->
[Create a SQL Data Warehouse (Azure Portal)]: sql-data-warehouse-get-started-provision.md
[Create a database (PowerShell)]: sql-data-warehouse-get-started-provision-powershell.md
[connection]: sql-data-warehouse-develop-connections.md
[Query Azure SQL Data Warehouse with Visual Studio]: sql-data-warehouse-query-visual-studio.md
[Connect and query with sqlcmd]: sql-data-warehouse-get-started-connect-sqlcmd.md
[Development overview]: sql-data-warehouse-overview-develop.md
[Monitor your workload using DMVs]: sql-data-warehouse-manage-monitor.md
[Pause compute]: sql-data-warehouse-manage-compute-overview.md#pause-compute-bk
[Restore from snapshot]: sql-data-warehouse-restore-database-overview.md
[Resume compute]: sql-data-warehouse-manage-compute-overview.md#resume-compute-bk
[prestaties schalen]: sql-data-warehouse-manage-compute-overview.md#scale-compute
[Security overview]: sql-data-warehouse-overview-manage-security.md
[SQL Data Warehouse Best Practices]: sql-data-warehouse-best-practices.md
[SQL Data Warehouse system views]: sql-data-warehouse-reference-tsql-system-views.md

<!--MSDN references-->
[SQL Server Data Tools]: https://msdn.microsoft.com/library/mt204009.aspx

<!--Other web references-->
[Azure portal]: http://portal.azure.com/
