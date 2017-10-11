---
title: Laden van gegevens uit SQL Server in Azure SQL Data Warehouse (SSIS) | Microsoft Docs
description: Laat zien hoe u een pakket met SQL Server Integration Services (SSIS) om gegevens te verplaatsen van een groot aantal gegevensbronnen in SQL Data Warehouse maken.
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: jhubbard
editor: 
ms.assetid: e2c252e9-0828-47c2-a808-e3bea46c134a
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: loading
ms.date: 03/30/2017
ms.author: cakarst;douglasl;barbkess
ms.openlocfilehash: 6c9cebdd715b6997d0633bc725a3945ba9e0c357
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="load-data-from-sql-server-into-azure-sql-data-warehouse-ssis"></a>Laden van gegevens uit SQL Server in Azure SQL Data Warehouse (SSIS)
> [!div class="op_single_selector"]
> * [SSIS](sql-data-warehouse-load-from-sql-server-with-integration-services.md)
> * [PolyBase](sql-data-warehouse-load-from-sql-server-with-polybase.md)
> * [bcp](sql-data-warehouse-load-from-sql-server-with-bcp.md)
> 
> 

Maak een pakket met SQL Server Integration Services (SSIS) om gegevens uit SQL Server laden in Azure SQL Data Warehouse. U kunt eventueel herstructureren, transformeren en opschonen van de gegevens als deze de SSIS-gegevensstroom passeert.

In deze zelfstudie leert u het volgende:

* Maak een nieuwe Integration Services-project in Visual Studio.
* Verbinding maken met gegevensbronnen, met inbegrip van SQL Server (als een bron) en SQL Data Warehouse (als doel).
* Ontwerp een SSIS-pakket dat gegevens worden geladen van de bron naar doel.
* Voer de SSIS-pakket om de gegevens te laden.

Deze zelfstudie gebruikt SQL Server als de gegevensbron. SQL Server kan on-premises of in Azure een virtuele machine worden uitgevoerd.

## <a name="basic-concepts"></a>Basisconcepten
Het pakket is de werkeenheid in SSIS. Verwante pakketten worden gegroepeerd in projecten. U maken projecten en ontwerp pakketten in Visual Studio met SQL Server Data Tools. Het ontwerpproces is een visual proces waarin u slepen en neerzetten van onderdelen uit de werkset naar het ontwerpoppervlak, koppelt u deze en hun eigenschappen instellen. Nadat u het pakket, kunt u eventueel deze implementeren op SQL Server voor uitgebreide beheer, bewaking en beveiliging.

## <a name="options-for-loading-data-with-ssis"></a>Opties voor het laden van gegevens met SSIS
SQL Server Integration Services (SSIS) is een flexibele set hulpprogramma's waarmee een groot aantal opties voor verbinding met en gegevens in SQL Data Warehouse te laden.

1. Gebruik een ADO NET doel verbinding maken met SQL Data Warehouse. Deze zelfstudie wordt een ADO NET doel omdat deze het minste aantal configuratieopties.
2. Gebruik een OLE DB-doel verbinding maken met SQL Data Warehouse. Deze optie kan enigszins betere prestaties dan het doel van ADO NET bieden.
3. Gebruik de Azure-taak voor het uploaden van Blob om voor te bereiden van de gegevens in Azure Blob Storage. Gebruik vervolgens de SQL uitvoeren van SSIS-taak starten van een Polybase-script dat de gegevens worden geladen in SQL Data Warehouse. Deze optie biedt de beste prestaties van de drie opties die hier worden vermeld. Als u de taak Azure Blob uploaden, downloaden de [Microsoft SQL Server 2016 Integration Services Feature Pack voor Azure][Microsoft SQL Server 2016 Integration Services Feature Pack for Azure]. Zie voor meer informatie over Polybase, [PolyBase-handleiding][PolyBase Guide].

## <a name="before-you-start"></a>Voordat u begint
Voor deze zelfstudie hebt u het volgende nodig:

1. **SQL Server integratieservices (SSIS)**. SSIS is een onderdeel van SQL Server en een evaluatieversie of een gelicentieerde versie van SQL Server vereist. Als u een evaluatieversie van SQL Server 2016 Preview, Zie [SQL Server-evaluaties][SQL Server Evaluations].
2. **Visual Studio**. Als u de gratis Visual Studio Community Edition, Zie [Visual Studio Community][Visual Studio Community].
3. **SQL Server Data Tools voor Visual Studio (SSDT)**. Als u SQL Server Data Tools voor Visual Studio, Zie [Download SQL Server Data Tools (SSDT)][Download SQL Server Data Tools (SSDT)].
4. **Voorbeeldgegevens**. Deze zelfstudie wordt opgeslagen in SQL Server in de AdventureWorks voorbeelddatabase als de brongegevens voorbeeldgegevens worden geladen in SQL Data Warehouse. Als u de AdventureWorks voorbeelddatabase, Zie [AdventureWorks 2014 voorbeeld Databases][AdventureWorks 2014 Sample Databases].
5. **Een SQL Data Warehouse-database en machtigingen**. Deze zelfstudie maakt verbinding met een exemplaar van SQL Data Warehouse en gegevens worden geladen in de App. U moet gemachtigd zijn om een tabel te maken en om gegevens te laden.
6. **Een firewallregel**. U moet een firewallregel maken in SQL Data Warehouse met het IP-adres van uw lokale computer, voordat u gegevens naar de SQL Data Warehouse uploaden kunt.

## <a name="step-1-create-a-new-integration-services-project"></a>Stap 1: Maak een nieuw project voor de Integration Services
1. Visual Studio starten.
2. Op de **bestand** selecteert u **nieuw | Project**.
3. Navigeer naar de **geïnstalleerd | Sjablonen | Business Intelligence | Integratieservices** projecttypen.
4. Selecteer **Integration Services-Project**. Waarden opgeven voor **naam** en **locatie**, en selecteer vervolgens **OK**.

Visual Studio wordt geopend en wordt een nieuw project voor de Integration Services (SSIS) gemaakt. Visual Studio wordt geopend de ontwerpfunctie voor het één nieuwe SSIS-pakket (Package.dtsx) in het project. U ziet het volgende schermgebieden:

* Aan de linkerkant de **werkset** van SSIS-onderdelen.
* In het midden het ontwerpoppervlak met meerdere tabbladen. Doorgaans gebruikt u ten minste de **controlestroom** en de **gegevensstroom** tabbladen.
* Aan de rechterkant de **Solution Explorer** en de **eigenschappen** deelvensters.
  
    ![][01]

## <a name="step-2-create-the-basic-data-flow"></a>Stap 2: De stroom basisgegevens maken
1. Sleep een taak gegevens stromen vanuit de werkset naar het midden van het ontwerpoppervlak dat (op het **controlestroom** tabblad).
   
    ![][02]
2. Dubbelklik op de taak gegevens stromen overschakelen naar het tabblad gegevens stromen.
3. In de lijst van andere bronnen in de werkset, sleept u een ADO.NET-gegevensbron naar het ontwerpoppervlak. De naam ervan te wijzigen met de bron-adapter nog steeds is geselecteerd, **SQL Server-bron** in de **eigenschappen** deelvenster.
4. Sleep een ADO.NET-doel naar het ontwerpoppervlak onder de ADO.NET-bron in de lijst andere bestemmingen in de werkset. De naam ervan te wijzigen met de doeladapter nog steeds is geselecteerd, **SQL DW bestemming** in de **eigenschappen** deelvenster.
   
    ![][09]

## <a name="step-3-configure-the-source-adapter"></a>Stap 3: Configureer de bron-adapter
1. Dubbelklik op de adapter bron te openen de **ADO.NET bron Editor**.
   
    ![][03]
2. Op de **Verbindingsbeheer** tabblad van de **ADO.NET bron Editor**, klikt u op de **nieuw** naast de **ADO.NET Verbindingsbeheer** lijst openen van de **ADO.NET Verbindingsbeheer configureren** dialoogvenster en te maken van de verbindingsinstellingen voor de SQL Server-database waarin gegevens voor deze zelfstudie worden geladen.
   
    ![][04]
3. In de **ADO.NET Verbindingsbeheer configureren** in het dialoogvenster klikt u op de **nieuw** te openen de **Verbindingsbeheer** dialoogvenster vak en maak een nieuwe verbinding.
   
    ![][05]
4. In de **Verbindingsbeheer** dialoogvenster vak, het volgende doen.
   
   1. Voor **Provider**, selecteer de SqlClient-gegevensprovider.
   2. Voor **servernaam**, voer de naam van de SQL Server.
   3. In de **Meld u aan bij de server** sectie Selecteer of typ de verificatiegegevens.
   4. In de **verbinding maken met een database** sectie, selecteert u de AdventureWorks voorbeelddatabase.
   5. Klik op **verbinding testen**.
      
       ![][06]
   6. Klik in het dialoogvenster die de resultaten van de verbindingstest rapporteert op **OK** om terug te keren naar de **Verbindingsbeheer** in het dialoogvenster.
   7. In de **Verbindingsbeheer** in het dialoogvenster klikt u op **OK** om terug te keren naar de **ADO.NET Verbindingsbeheer configureren** in het dialoogvenster.
5. In de **ADO.NET Verbindingsbeheer configureren** in het dialoogvenster klikt u op **OK** om terug te keren naar de **ADO.NET bron Editor**.
6. In de **ADO.NET bron Editor**, in de **naam van de tabel of de weergave** lijst, selecteert de **Sales.SalesOrderDetail** tabel.
   
    ![][07]
7. Klik op **Preview** om te zien van de eerste 200 rijen van de gegevens in de brontabel in de **Preview queryresultaten** in het dialoogvenster.
   
    ![][08]
8. In de **Preview queryresultaten** in het dialoogvenster klikt u op **sluiten** om terug te keren naar de **ADO.NET bron Editor**.
9. In de **ADO.NET bron Editor**, klikt u op **OK** voltooid met het configureren van de gegevensbron.

## <a name="step-4-connect-the-source-adapter-to-the-destination-adapter"></a>Stap 4: Verbind de netwerkadapter van de bron met de doeladapter
1. Selecteer de bron-adapter op het ontwerpoppervlak.
2. Selecteer de blauwe pijl die van de bron-adapter loopt en sleep deze naar de doel-editor totdat deze wordt uitgelijnd naar de juiste plaats.
   
    ![][10]
   
    In een typische SSIS-pakket gebruikt u een aantal andere onderdelen uit de werkset SSIS tussen de bron en bestemming herstructureren, transformeren en gegevens te verwijderen als deze de SSIS-gegevensstroom passeert. Als u wilt behouden in dit voorbeeld zo eenvoudig mogelijk, verbinding we de bron rechtstreeks naar de bestemming.

## <a name="step-5-configure-the-destination-adapter"></a>Stap 5: Configureer de doeladapter
1. Dubbelklik op de doeladapter te openen de **ADO.NET bestemming Editor**.
   
    ![][11]
2. Op de **Verbindingsbeheer** tabblad van de **ADO.NET bestemming Editor**, klikt u op de **nieuw** naast de **Verbindingsbeheer** lijst Open de **ADO.NET Verbindingsbeheer configureren** dialoogvenster en te maken van de verbindingsinstellingen voor de Azure SQL Data Warehouse-database waarin gegevens voor deze zelfstudie worden geladen.
3. In de **ADO.NET Verbindingsbeheer configureren** in het dialoogvenster klikt u op de **nieuw** te openen de **Verbindingsbeheer** dialoogvenster vak en maak een nieuwe verbinding.
4. In de **Verbindingsbeheer** dialoogvenster vak, het volgende doen.
   1. Voor **Provider**, selecteer de SqlClient-gegevensprovider.
   2. Voor **servernaam**, voer de naam van de SQL Data Warehouse.
   3. In de **Meld u aan bij de server** sectie **Gebruik SQL Server-verificatie** en voer de verificatiegegevens.
   4. In de **verbinding maken met een database** sectie, selecteer een bestaande SQL Data Warehouse-database.
   5. Klik op **verbinding testen**.
   6. Klik in het dialoogvenster die de resultaten van de verbindingstest rapporteert op **OK** om terug te keren naar de **Verbindingsbeheer** in het dialoogvenster.
   7. In de **Verbindingsbeheer** in het dialoogvenster klikt u op **OK** om terug te keren naar de **ADO.NET Verbindingsbeheer configureren** in het dialoogvenster.
5. In de **ADO.NET Verbindingsbeheer configureren** in het dialoogvenster klikt u op **OK** om terug te keren naar de **ADO.NET bestemming Editor**.
6. In de **ADO.NET bestemming Editor**, klikt u op **nieuw** naast de **gebruik van een tabel of weergave** lijst openen de **Create Table** in het dialoogvenster voor het maken een nieuwe doeltabel met een kolomlijst die overeenkomt met de brontabel.
   
    ![][12a]
7. In de **Create Table** dialoogvenster vak, het volgende doen.
   
   1. Wijzig de naam van de doeltabel voor **verkooporderdetail**.
   2. Verwijder de **rowguid** kolom. De **uniqueidentifier** gegevenstype wordt niet ondersteund in SQL Data Warehouse.
   3. Wijzig het gegevenstype van de **LineTotal** kolom **geld**. De **decimale** gegevenstype wordt niet ondersteund in SQL Data Warehouse. Zie voor informatie over de ondersteunde gegevenstypen [CREATE TABLE (Azure SQL Data Warehouse, Parallel Data Warehouse)][CREATE TABLE (Azure SQL Data Warehouse, Parallel Data Warehouse)].
      
       ![][12b]
   4. Klik op **OK** voor het maken van de tabel en Ga terug naar de **ADO.NET bestemming Editor**.
8. In de **ADO.NET bestemming Editor**, selecteer de **toewijzingen** tabblad om te zien hoe de kolommen in de bron zijn toegewezen aan kolommen in de bestemming.
   
    ![][13]
9. Klik op **OK** voltooid met het configureren van de gegevensbron.

## <a name="step-6-run-the-package-to-load-the-data"></a>Stap 6: Het pakket om de gegevens te laden uitvoeren
Het pakket uitvoeren door te klikken op de **Start** knop op de werkbalk of door het selecteren van een van de **uitvoeren** opties op de **Debug** menu.

Als het pakket gestart wordt, ziet u gele draaiende wheels om aan te geven van de activiteit, evenals het aantal rijen verwerkt tot nu toe.

![][14]

Wanneer het pakket is voltooid, ziet u groen vinkje om aan te geven, zowel geslaagd als het totale aantal rijen met gegevens die worden geladen vanuit de bron naar de bestemming.

![][15]

Gefeliciteerd. U hebt de SQL Server Integration Services is gebruikt om gegevens te laden in Azure SQL Data Warehouse.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over de SSIS-gegevensstroom. Begin hier: [gegevensstroom][Data Flow].
* Informatie over het opsporen en oplossen van uw recht pakketten in de ontwerpomgeving. Begin hier: [hulpprogramma's voor het oplossen van problemen voor de ontwikkeling van pakket][Troubleshooting Tools for Package Development].
* Informatie over het implementeren van SSIS-projecten en pakketten op Integration Services-Server of naar een andere opslaglocatie. Begin hier: [van implementatieprojecten en pakketten][Deployment of Projects and Packages].

<!-- Image references -->
[01]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ssis-designer-01.png
[02]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ssis-data-flow-task-02.png
[03]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-source-03.png
[04]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-connection-manager-04.png
[05]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-connection-05.png
[06]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/test-connection-06.png
[07]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-source-07.png
[08]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/preview-data-08.png
[09]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/source-destination-09.png
[10]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/connect-source-destination-10.png
[11]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-destination-11.png
[12a]: ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/destination-query-before-12a.png
[12b]: ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/destination-query-after-12b.png
[13]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/column-mapping-13.png
[14]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/package-running-14.png
[15]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/package-success-15.png

<!-- Article references -->

<!-- MSDN references -->
[PolyBase Guide]: https://msdn.microsoft.com/library/mt143171.aspx
[Download SQL Server Data Tools (SSDT)]: https://msdn.microsoft.com/library/mt204009.aspx
[CREATE TABLE (Azure SQL Data Warehouse, Parallel Data Warehouse)]: https://msdn.microsoft.com/library/mt203953.aspx
[Data Flow]: https://msdn.microsoft.com/library/ms140080.aspx
[Troubleshooting Tools for Package Development]: https://msdn.microsoft.com/library/ms137625.aspx
[Deployment of Projects and Packages]: https://msdn.microsoft.com/library/hh213290.aspx

<!--Other Web references-->
[Microsoft SQL Server 2016 Integration Services Feature Pack for Azure]: http://go.microsoft.com/fwlink/?LinkID=626967
[SQL Server Evaluations]: https://www.microsoft.com/en-us/evalcenter/evaluate-sql-server-2016
[Visual Studio Community]: https://www.visualstudio.com/en-us/products/visual-studio-community-vs.aspx
[AdventureWorks 2014 Sample Databases]: https://msftdbprodsamples.codeplex.com/releases/view/125550
