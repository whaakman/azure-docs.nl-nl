---
title: Redgate gebruiken om gegevens in uw Azure datawarehouse te laden | Microsoft Docs
description: Leer hoe u Data Platform Studio van Redgate kunt gebruiken voor datawarehousingscenario's.
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: jhubbard
editor: 
ms.assetid: 670aef98-31f7-4436-86c0-cc989a39fe7f
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: loading
ms.date: 10/31/2016
ms.author: cakarst;barbkess
ms.openlocfilehash: a38b237d5bfc0450c1ca79b53a5784dbb9bf8602
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="load-data-with-redgate-data-platform-studio"></a>Gegevens laden met behulp van Data Platform Studio van Redgate
> [!div class="op_single_selector"]
> * [Redgate](sql-data-warehouse-load-with-redgate.md)
> * [Data Factory](sql-data-warehouse-get-started-load-with-azure-data-factory.md)
> * [PolyBase](sql-data-warehouse-get-started-load-with-polybase.md)
> * [BCP](sql-data-warehouse-load-with-bcp.md)
> 
> 

In deze zelfstudie ziet u hoe u [DPS van Redgate](http://www.red-gate.com/products/azure-development/data-platform-studio/) (Data Platform Studio) kunt gebruiken om gegevens te verplaatsen van een on-premises SQL-server naar Azure SQL Data Warehouse. Met DPS worden de meest geschikte compatibiliteitscorrecties en optimalisaties toegepast. Daarom is het de snelste manier om aan de slag te gaan met SQL Data Warehouse.

> [!NOTE]
> [Redgate](http://www.red-gate.com) is al lange tijd een Microsoft-partner en levert verschillende SQL Server-hulpprogramma's. Deze functie in DPS is gratis beschikbaar voor commercieel en niet-commercieel gebruik.
> 
> 

## <a name="before-you-begin"></a>Voordat u begint
### <a name="create-or-identify-resources"></a>Resources maken of identificeren
Voordat u met deze zelfstudie begint, moet u beschikken over:

* **Een on-premises SQL Server-database**: de gegevens die u wilt importeren naar SQL Data Warehouse, moeten afkomstig zijn van een on-premises SQL-server (versie 2008R2 of hoger). Met DPS kunnen gegevens niet rechtstreeks worden geïmporteerd uit een Azure SQL-database of uit tekstbestanden.
* **Azure-opslagaccount**: met DPS worden de gegevens klaargezet in de Azure Blob-opslag voordat ze in SQL Data Warehouse worden geladen. Het opslagaccount moet het standaardimplementatiemodel Resource Manager gebruiken in plaats van het klassieke implementatiemodel. Als u geen opslagaccount hebt, leert u hoe u er een kunt maken. 
* **SQL Data Warehouse**: in deze zelfstudie wordt getoond hoe gegevens worden verplaatst van een on-premises SQL-server naar SQL Data Warehouse. U hebt daarom een onlinedatawarehouse nodig. Als u nog geen datawarehouse hebt, leert u hoe u er een moet inrichten om een SQL-datawarehouse te maken.

> [!NOTE]
> Als het opslagaccount en het datawarehouse in dezelfde regio worden gemaakt, verbeteren de prestaties.
> 
> 

## <a name="step-1-sign-in-to-data-platform-studio-with-your-azure-account"></a>Stap 1: Aanmelden bij DPS met uw Azure-account
Open de webbrowser en navigeer naar de website van [Data Platform Studio](https://www.dataplatformstudio.com/). Meld u aan met hetzelfde Azure-account dat u hebt gebruikt om het opslagaccount en het datawarehouse te maken. Als uw e-mailadres is gekoppeld aan een werk- of schoolaccount en aan een Microsoft-account, kiest u het account waarmee u toegang hebt tot uw resources.

> [!NOTE]
> Als dit de eerste keer is dat u DPS gebruikt, wordt u gevraagd om de toepassing toestemming te geven om de Azure-resources te beheren.
> 
> 

## <a name="step-2-start-the-import-wizard"></a>Stap 2: Wizard Importeren starten
Selecteer in het hoofdvenster van DPS de koppeling Importeren naar Azure SQL Data Warehouse om de wizard Importeren te starten.

![][1]

## <a name="step-3-install-the-data-platform-studio-gateway"></a>Stap 3: DPS-gateway installeren
Als u verbinding wilt maken met uw on-premises SQL Server-database, moet u de DPS-gateway installeren. De gateway is een clientagent voor toegang tot de on-premises omgeving, waarmee gegevens worden uitgepakt en geüpload naar uw opslagaccount. De gegevens worden niet verwerkt op de servers van Redgate. De gateway installeren:

1. Klik op de koppeling **Gateway maken**
2. Download en installeer de gateway met behulp van het geleverde installatieprogramma

![][2]

> [!NOTE]
> De gateway kan worden geïnstalleerd op elke computer met netwerktoegang tot de SQL Server-brondatabase. De gateway heeft toegang tot de SQL Server-database via Windows-verificatie met de referenties van de huidige gebruiker.
> 
> 

Zodra de gateway is geïnstalleerd, wordt de status ervan gewijzigd naar Verbonden en kunt u Volgende selecteren.

## <a name="step-4-identify-the-source-database"></a>Stap 4: De brondatabase identificeren
Voer in het tekstvak *Servernaam invoeren* de naam in van de server die de database host, en selecteer **Volgende**. Selecteer vervolgens in de vervolgkeuzelijst de database waaruit u gegevens wilt importeren.

![][3]

De geselecteerde database wordt met DPS gecontroleerd op tabellen die kunnen worden geïmporteerd. Met DPS worden standaard alle tabellen in de database geïmporteerd. U kunt tabellen selecteren of de selectie van tabellen opheffen door de koppeling Alle tabellen uit te vouwen. Selecteer de knop Volgende om door te gaan.

## <a name="step-5-choose-a-storage-account-to-stage-the-data"></a>Stap 5: Een opslagaccount kiezen om de gegevens in klaar te zetten
DPS vraagt u om een locatie om de gegevens in klaar te zetten. Kies een bestaand opslagaccount in uw abonnement en selecteer **Volgende**.

> [!NOTE]
> Met DPS wordt een nieuwe blobcontainer gemaakt in het gekozen opslagaccount. Voor elke import wordt een afzonderlijke map gebruikt.
> 
> 

![][4]

## <a name="step-6-select-a-data-warehouse"></a>Stap 6: Een datawarehouse selecteren
Vervolgens selecteert u [Azure SQL-datawarehouse](http://aka.ms/sqldw) om de gegevens in te importeren. Nadat u de database hebt geselecteerd, moet u de referenties invoeren om verbinding te maken met de database. Selecteer vervolgens **Volgende**.

![][5]

> [!NOTE]
> De doelgegevenstabellen worden samengevoegd in het datawarehouse. U wordt gewaarschuwd als de tabelnaam vereist dat bestaande tabellen in het datawarehouse worden overschreven. U kunt ervoor kiezen om bestaande objecten in het datawarehouse te verwijderen. Dit doet u door te tikken op Alle bestaande objecten verwijderen voor het importeren.
> 
> 

## <a name="step-7-import-the-data"></a>Stap 7: De gegevens importeren
In DPS wordt bevestigd dat u de gegevens wilt importeren. Klik op de knop Importeren starten om te beginnen met het importeren van gegevens.

![][6]

Er wordt een visualisatie weergegeven waarin de voortgang van het uitpakken en uploaden van de gegevens van de on-premises SQL-server wordt weergegeven, evenals de voortgang van het importeren in SQL Data Warehouse.

![][7]

Als het importeren is voltooid, wordt er in DPS een overzicht weergegeven van de gegevensimport en een wijzigingsrapport van de compatibiliteitsoplossingen die zijn uitgevoerd.

![][8]

## <a name="next-steps"></a>Volgende stappen
Als u de gegevens in SQL Data Warehouse wilt verkennen, begint u met het bekijken van:

* [Query’s uitvoeren bij Azure SQL Data Warehouse (Visual Studio)][Query Azure SQL Data Warehouse (Visual Studio)]
* [Gegevens visualiseren met Power BI][Visualize data with Power BI]

Voor meer informatie over Data Platform Studio van Redgate:

* [Ga naar de DPS-startpagina](http://www.dataplatformstudio.com/)
* [Bekijk een demonstratie van DPS op Channel 9](https://channel9.msdn.com/Blogs/cloud-with-a-silver-lining/Loading-data-into-Azure-SQL-Datawarehouse-with-Redgate-Data-Platform-Studio)

Zie voor een overzicht van andere manieren om te migreren en uw gegevens in SQL Data Warehouse te laden:

* [Uw oplossing migreren naar SQL Data Warehouse][Migrate your solution to SQL Data Warehouse]
* [Load data into Azure SQL Data Warehouse](sql-data-warehouse-overview-load.md) (Gegevens laden in Azure SQL Data Warehouse)

Zie het [Overzicht van SQL Data Warehouse voor ontwikkelaars](sql-data-warehouse-overview-develop.md) voor meer tips voor ontwikkelaars.

<!--Image references-->
[1]: media/sql-data-warehouse-redgate/2016-10-05_15-59-56.png
[2]: media/sql-data-warehouse-redgate/2016-10-05_11-16-07.png
[3]: media/sql-data-warehouse-redgate/2016-10-05_11-17-46.png
[4]: media/sql-data-warehouse-redgate/2016-10-05_11-20-41.png
[5]: media/sql-data-warehouse-redgate/2016-10-05_11-31-24.png
[6]: media/sql-data-warehouse-redgate/2016-10-05_11-32-20.png
[7]: media/sql-data-warehouse-redgate/2016-10-05_11-49-53.png
[8]: media/sql-data-warehouse-redgate/2016-10-05_12-57-10.png

<!--Article references-->
[Query Azure SQL Data Warehouse (Visual Studio)]: ./sql-data-warehouse-query-visual-studio.md
[Visualize data with Power BI]: ./sql-data-warehouse-get-started-visualize-with-power-bi.md
[Migrate your solution to SQL Data Warehouse]: ./sql-data-warehouse-overview-migrate.md
[Load data into Azure SQL Data Warehouse]: ./sql-data-warehouse-overview-load.md
[SQL Data Warehouse development overview]: ./sql-data-warehouse-overview-develop.md
