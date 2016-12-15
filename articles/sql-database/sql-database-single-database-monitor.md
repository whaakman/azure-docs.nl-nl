---
title: Databaseprestaties bewaken in Azure SQL Database | Microsoft Docs
description: Lees meer over de opties voor het bewaken van uw database met Azure-hulpprogramma&quot;s en dynamische beheerweergave.
keywords: database bewaken, prestaties van clouddatabase
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: a2e47475-c955-4a8d-a65c-cbef9a6d9b9f
ms.service: sql-database
ms.custom: monitor and tune
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 09/27/2016
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 5cda0c5b491f71999da0b7bd70156256ab2d5908


---
# <a name="monitoring-database-performance-in-azure-sql-database"></a>Databaseprestaties bewaken in Azure SQL Database
Het bewaken van de prestaties van een SQL-database in Azure begint met het bewaken van het resourcegebruik ten opzichte van het gekozen niveau van databaseprestaties. Bewaking helpt u om te bepalen of de database te veel capaciteit heeft of juist problemen heeft omdat resources volledig worden benut. Vervolgens kunt u beslissen of u het prestatieniveau en de [servicelaag](sql-database-service-tiers.md) van uw database moet aanpassen. U kunt een database bewaken met grafische hulpprogramma's in de [Azure-portal](https://portal.azure.com) of met [dynamische beheerweergaven](https://msdn.microsoft.com/library/ms188754.aspx) van SQL.

## <a name="monitor-databases-using-the-azure-portal"></a>Databases bewaken via de Azure-portal
In de [Azure-portal](https://portal.azure.com/) kunt u het verbruik van een individuele database bewaken door de database te selecteren en op de grafiek **Bewaking** te klikken. Nu wordt een venster **Metrische gegevens** geopend, dat u kunt wijzigen door op de knop **Grafiek bewerken** te klikken. Voeg de volgende metrische gegevens toe:

* CPU-percentage
* DTU-percentage
* Gegevens-I/O-percentage
* Databaseomvangpercentage

Wanneer u deze metrische gegevens hebt toegevoegd, kunt u ze in de grafiek **Bewaking** gedetailleerder bekijken in het venster **Metrische gegevens**. De vier metrische gegevens tonen het gemiddelde gebruikspercentage ten opzichte van de **DTU** van uw database. Raadpleeg het artikel over [servicelagen](sql-database-service-tiers.md) voor meer informatie over DTU's.

![Servicelaagbewaking van databaseprestaties.](./media/sql-database-service-tiers/sqldb_service_tier_monitoring.png)

U kunt ook meldingen configureren voor prestatiewaarden. Klik op de knop **Melding toevoegen** in het venster **Metrische gegevens**. Volg de wizard om de melding te configureren. U hebt de keuze om een melding weer te geven als de metrische gegevens een bepaalde drempelwaarde overschrijden of als het meetpunt onder een bepaalde drempelwaarde komt.

Als u bijvoorbeeld verwacht dat de workload van de database zal toenemen, kunt u configureren dat er een e-mailmelding wordt verstuurd wanneer de database 80% van een van de prestatiewaarden heeft bereikt. U kunt dit gebruiken als een aankondiging dat u misschien moet overschakelen naar een hoger prestatieniveau.

Aan de hand van de prestatiewaarden kunt u ook bepalen of u misschien moet downgraden naar een lager prestatieniveau. Stel dat u een Standard S2-database gebruikt en alle prestatiewaarden aangeven dat de database gemiddeld nooit meer dan 10% gebruikt. Het is dan waarschijnlijk dat de database in Standard S1 goed werkt. Houd echter wel rekening met workloads die pieken of fluctueren, voordat u besluit om over te stappen op een lager prestatieniveau.

## <a name="monitor-databases-using-dmvs"></a>Databases bewaken via DMV's
Dezelfde metrische gegevens die in de portal worden weergegeven, zijn ook beschikbaar via systeemweergaven: [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx) in de logische **hoofd**database van de server en [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) in de gebruikersdatabase. Gebruik **sys.resource_stats** als u voor een langere periode minder gedetailleerde gegevens wilt bewaken. Gebruik **sys.dm_db_resource_stats** als u gedurende kortere tijd meer gedetailleerde gegevens wilt bewaken. Zie [Richtlijnen voor Azure SQL Database-prestaties](sql-database-performance-guidance.md#monitor-resource-use) voor meer informatie.

> [!NOTE]
> **sys.dm_db_resource_stats** retourneert een lege resultatenset als deze wordt gebruikt voor de buiten gebruik gestelde Web en Business Edition-databases.
>
>

Voor pools voor elastische database kunt u afzonderlijke databases in de pool bewaken met de technieken die in deze sectie zijn beschreven. U kunt echter ook de groep als geheel bewaken. Zie [Een pool voor elastische database bewaken en beheren](sql-database-elastic-pool-manage-portal.md) voor meer informatie.



<!--HONumber=Dec16_HO1-->


