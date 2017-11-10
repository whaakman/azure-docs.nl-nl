---
title: Gegevens verplaatsen naar een Azure SQL Database voor Azure Machine Learning | Microsoft Docs
description: SQL-tabel en gegevens laden naar SQL-tabel maken
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 50f8b862-4d32-44b2-a1e2-4fbc8024acaa
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/04/2017
ms.author: bradsev
ms.openlocfilehash: 323861d078e9beeb197333dc7e2d0314014dfdb0
ms.sourcegitcommit: 93902ffcb7c8550dcb65a2a5e711919bd1d09df9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/09/2017
---
# <a name="move-data-to-an-azure-sql-database-for-azure-machine-learning"></a>Gegevens voor Azure Machine Learning verplaatsen naar een Azure SQL-database
In dit onderwerp bevat een overzicht van de opties voor het verplaatsen van gegevens uit platte bestanden (TSV- of CSV-indeling) of van de gegevens die zijn opgeslagen in een lokale SQL Server naar een Azure SQL-database. Deze taken voor het verplaatsen van gegevens naar de cloud maken deel uit van het Team gegevens wetenschap proces.

Zie voor in dit onderwerp bevat een overzicht van de opties voor het verplaatsen van gegevens naar een lokale SQL Server voor Machine Learning, [gegevens verplaatsen naar SQL Server op een virtuele machine van Azure](move-sql-server-virtual-machine.md).

De volgende **menu** koppelingen naar onderwerpen waarin wordt beschreven hoe u opnemen van gegevens in de doel-omgevingen waarin de gegevens kunnen worden opgeslagen en verwerkt tijdens het Team gegevens wetenschap proces (TDSP).

[!INCLUDE [cap-ingest-data-selector](../../../includes/cap-ingest-data-selector.md)]

De volgende tabel geeft een overzicht van de opties voor het verplaatsen van gegevens naar een Azure SQL Database.

| <b>BRON</b> | <b>BESTEMMING: Azure SQL Database</b> |
| --- | --- |
| <b>Plat bestand (CSV- of geformatteerd TSV)</b> |<a href="#bulk-insert-sql-query">Bulksgewijs invoegen SQL-Query |
| <b>On-premises SQL Server</b> |1. <a href="#export-flat-file">Exporteren naar platte bestanden<br> 2. <a href="#insert-tables-bcp">Wizard voor migratie van SQL-Database<br> 3. <a href="#db-migration">Back-up van database maken en terugzetten<br> 4. <a href="#adf">Azure Data Factory |

## <a name="prereqs"></a>Vereisten
De procedures die hier wordt beschreven, vereisen dat u hebt:

* Een **Azure-abonnement**. Als u geen abonnement hebt, kunt u zich aanmelden voor een [gratis proefversie](https://azure.microsoft.com/pricing/free-trial/).
* Een **Azure storage-account**. U kunt een Azure storage-account gebruiken voor het opslaan van de gegevens in deze zelfstudie. Zie het artikel [Een opslagaccount maken](../../storage/common/storage-create-storage-account.md#create-a-storage-account) als u geen account Azure-opslagaccount hebt. Nadat u het opslagaccount hebt gemaakt, moet u de accountsleutel ophalen die wordt gebruikt voor toegang tot de opslag. Zie [beheren van uw toegangssleutels voor opslag](../../storage/common/storage-create-storage-account.md#manage-your-storage-access-keys).
* Toegang tot een **Azure SQL Database**. Als u een Azure SQL-Database moet ingesteld [aan de slag met Microsoft Azure SQL Database](../../sql-database/sql-database-get-started.md) bevat informatie over het inrichten van een nieuw exemplaar van een Azure SQL Database.
* Geïnstalleerd en geconfigureerd **Azure PowerShell** lokaal. Zie voor instructies [installeren en configureren van Azure PowerShell](/powershell/azure/overview).

**Gegevens**: de migratie-processen worden gedemonstreerd met behulp van de [NYC Taxi gegevensset](http://chriswhong.com/open-data/foil_nyc_taxi/). De gegevensset NYC Taxi bevat informatie over reis gegevens en beurzen en is beschikbaar op Azure blob-opslag: [NYC Taxi gegevens](http://www.andresmh.com/nyctaxitrips/). Een beschrijving van deze bestanden en voorbeelden vindt u in [NYC Taxi reizen gegevensset beschrijving](sql-walkthrough.md#dataset).

U kunt aanpassen van de procedures die hier wordt beschreven op een reeks uw eigen gegevens of de stappen zoals beschreven met behulp van de NYC Taxi gegevensset. Als u wilt de gegevensset NYC Taxi uploaden naar uw lokale SQL Server-database, volgt u de procedure beschreven in [gegevens voor bulksgewijs importeren in SQL Server-Database](sql-walkthrough.md#dbload). Deze instructies zijn voor een SQL-Server op een virtuele Machine van Azure, maar de procedure voor het uploaden naar de lokale SQL Server is hetzelfde.

## <a name="file-to-azure-sql-database"></a>Verplaatsen van gegevens van een plat bestand-bron naar een Azure SQL database
Gegevens in platte bestanden (CSV-bestand of TSV geformatteerd) kunnen worden verplaatst naar een Azure SQL database met behulp van een Bulk Insert SQL-Query.

### <a name="bulk-insert-sql-query"></a>Bulksgewijs invoegen SQL-Query
De stappen voor de procedure met de Bulk Insert SQL-Query zijn vergelijkbaar met die in de secties voor het verplaatsen van gegevens van een plat bestand-bron met SQL Server op een Azure VM besproken. Zie voor meer informatie [Bulk Insert SQL-Query](move-sql-server-virtual-machine.md#insert-tables-bulkquery).

## <a name="sql-on-prem-to-sazure-sql-database"></a>Verplaatsen van gegevens van on-premises SQL-Server naar een Azure SQL database
Als de brongegevens is opgeslagen in een lokale SQL Server, zijn er verschillende mogelijkheden voor het verplaatsen van de gegevens naar een Azure SQL database:

1. [Exporteren naar platte bestanden](#export-flat-file)
2. [Wizard voor migratie van SQL-Database](#insert-tables-bcp)
3. [Back-up van database maken en terugzetten](#db-migration)
4. [Azure Data Factory](#adf)

De stappen voor de eerste drie zijn vergelijkbaar met deze gedeelten in [gegevens verplaatsen naar SQL Server op een virtuele machine van Azure](move-sql-server-virtual-machine.md) dat deze dezelfde procedures beslaan. Koppelingen naar de toepasselijke rubrieken in dit onderwerp vindt u in de volgende instructies.

### <a name="export-flat-file"></a>Exporteren naar platte bestanden
De stappen om deze te exporteren naar een plat bestand zijn vergelijkbaar met die behandeld in [exporteren naar platte bestanden](move-sql-server-virtual-machine.md#export-flat-file).

### <a name="insert-tables-bcp"></a>Wizard voor migratie van SQL-Database
De stappen voor het gebruik van de Wizard migratie van SQL-Database zijn vergelijkbaar met die behandeld in [SQL Database Migration Wizard](move-sql-server-virtual-machine.md#sql-migration).

### <a name="db-migration"></a>Back-up van database maken en terugzetten
De stappen voor het gebruik van de database back-up en herstel zijn vergelijkbaar met die behandeld in [Database back-en herstel](move-sql-server-virtual-machine.md#sql-backup).

### <a name="adf"></a>Azure Data Factory
De procedure voor het verplaatsen van gegevens naar een Azure SQL database met Azure Data Factory (ADF) is opgegeven in het onderwerp [verplaatsen van gegevens van een lokale SQL server naar SQL Azure met Azure Data Factory](move-sql-azure-adf.md). Dit onderwerp leest hoe u gegevens uit een on-premises SQL Server database verplaatsen naar een Azure SQL database via Azure Blob Storage met ADF.

Overweeg het gebruik van ADF wanneer gegevens moeten voortdurend worden gemigreerd in een hybride scenario die toegang heeft tot zowel on-premises en cloudresources en wanneer de gegevens is transactionele of moet worden gewijzigd of dat er zakelijke logica toegevoegd wanneer wordt gemigreerd. ADF kunt u de planning en bewaking van taken met behulp van eenvoudige JSON-scripts die de verplaatsing van gegevens op periodieke basis beheren. ADF heeft ook andere mogelijkheden, zoals ondersteuning voor complexe bewerkingen.
