---
title: Ondersteunde gegevensbronnen in Azure Analysis Services | Microsoft Docs
description: Beschrijving van gegevensbronnen die worden ondersteund voor gegevensmodellen in Azure Analysis Services.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 01/09/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: e1a001a60151136be6bde9de38f971807cf0c288
ms.sourcegitcommit: 63b996e9dc7cade181e83e13046a5006b275638d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2019
ms.locfileid: "54188399"
---
# <a name="data-sources-supported-in-azure-analysis-services"></a>Ondersteunde gegevensbronnen in Azure Analysis Services

Gegevensbronnen en connectors die wordt weergegeven in gegevens ophalen of in de Wizard importeren in Visual Studio worden weergegeven voor zowel Azure Analysis Services en SQL Server Analysis Services. Echter niet alle gegevensbronnen en connectors die worden weergegeven in Azure Analysis Services worden ondersteund. De soorten gegevensbronnen die u kunt verbinding maken met zijn afhankelijk van veel factoren af zoals model compatibiliteitsniveau, beschikbare gegevensconnectors verificatietype, providers en ondersteuning voor On-premises data gateway. 

## <a name="azure-data-sources"></a>Azure-gegevensbronnen

|Gegevensbron  |In het geheugen  |DirectQuery  |
|---------|---------|---------|
|Azure SQL Database     |   Ja      |    Ja      |
|Azure SQL Data Warehouse     |   Ja      |   Ja       |
|Azure Blob Storage *     |   Ja       |    Nee      |
|Azure Table Storage *    |   Ja       |    Nee      |
|Azure Cosmos DB*     |  Ja        |  Nee        |
|Azure Data Lake Store *     |   Ja       |    Nee      |
|Azure HDInsight HDFS*     |     Ja     |   Nee       |
|Azure HDInsight Spark *     |   Ja       |   Nee       |
||||

\* Tabellaire modellen met compatibiliteitsniveau 1400 alleen.

**Provider**   
In-memory en DirectQuery-modellen verbinding te maken met Azure-gegevensbronnen gebruikt .NET Framework Data Provider voor SQL Server.

## <a name="on-premises-data-sources"></a>On-premises gegevensbronnen

Verbinding maken met on-premises gegevensbronnen van en Azure-AS-server moet een On-premises gateway. Bij het gebruik van een gateway is 64-bits-providers zijn vereist.

### <a name="in-memory-and-directquery"></a>In-memory en DirectQuery

|Gegevensbron | In-memory-provider | DirectQuery-provider |
|  --- | --- | --- |
| SQL Server |SQL Server Native Client 11.0, Microsoft OLE DB-Provider voor SQL Server, .NET Framework Data Provider voor SQL Server | .NET framework Data Provider voor SQL Server |
| SQL Server datawarehouse |SQL Server Native Client 11.0, Microsoft OLE DB-Provider voor SQL Server, .NET Framework Data Provider voor SQL Server | .NET framework Data Provider voor SQL Server |
| Oracle |Microsoft OLE DB-Provider voor Oracle, Oracle-gegevensprovider voor .NET |Oracle-gegevensprovider voor .NET | |
| Teradata |OLE DB-Provider voor Teradata, Teradata-gegevensprovider voor .NET |Teradata-gegevensprovider voor .NET | |
| | | |

### <a name="in-memory-only"></a>In het geheugen alleen

|Gegevensbron  |  
|---------|---------|
|Access-Database     |  
|Active Directory *     |  
|Analysis Services     |  
|Analytics Platform System     |  
|Dynamics CRM *     |  
|Excel-werkmap     |  
|Exchange *     |  
|Map *     |
|IBM Informix * (bèta) |
|JSON-document *     |  
|Regels van binaire *     | 
|MySQL-database     | 
|OData-Feed *     |  
|ODBC-query     | 
|OLE DB     |   
|Postgre SQL-Database *    | 
|SalesForce-objecten * |  
|SalesForce-rapporten * |
|SAP HANA *    |  
|SAP Business Warehouse *    |  
|SharePoint*     |   
|Sybase-database     |  
|XML-tabel *    |  
|||
 
\* Tabellaire modellen met compatibiliteitsniveau 1400 alleen.

## <a name="specifying-a-different-provider"></a>Een andere provider op te geven

Gegevensmodellen in Azure Analysis Services mogelijk verschillende gegevensproviders bij het verbinden met bepaalde gegevensbronnen. In sommige gevallen mogelijk modellen in tabelvorm verbinding maken met gegevensbronnen met behulp van systeemeigen providers, zoals SQL Server Native Client (SQLNCLI11) een fout geretourneerd. Als u systeemeigen providers dan SQLOLEDB, ziet u mogelijk een foutbericht weergegeven: **De provider 'SQLNCLI11.1' is niet geregistreerd**. Of, als u een DirectQuery-model verbinding te maken met on-premises gegevensbronnen hebt en u systeemeigen providers gebruikt, ziet u mogelijk foutbericht weergegeven: **Fout bij het maken van OLE DB-rij instellen. Onjuiste syntaxis bij 'LIMIET'**.

Bij het migreren van een on-premises SQL Server Analysis Services tabellair model naar Azure Analysis Services, is het mogelijk nodig is om te wijzigen van de provider.

**Om op te geven van een provider**

1. In SSDT > **Tabular Model Explorer** > **gegevensbronnen**, met de rechtermuisknop op een verbinding met de gegevensbron en klik vervolgens op **gegevensbron bewerken**.
2. In **verbinding bewerken**, klikt u op **Geavanceerd** om de geavanceerde eigenschappen-venster te openen.
3. In **geavanceerde eigenschappen instellen** > **Providers**, selecteer vervolgens de juiste provider.

## <a name="impersonation"></a>Imitatie
In sommige gevallen is het mogelijk dat het nodig om op te geven van een verschillende imitatieaccount. Imitatieaccount kan worden opgegeven in Visual Studio (SSDT) of in SSMS.

Voor on-premises gegevensbronnen:

* Als u SQL-verificatie gebruikt, moet de imitatie-serviceaccount.
* Als Windows-verificatie, stelt u de Windows-gebruiker en wachtwoord. Voor SQL Server, wordt Windows-verificatie met een specifieke imitatieaccount alleen ondersteund voor modellen in-memory-gegevens.

Voor cloud-gegevensbronnen:

* Als u SQL-verificatie gebruikt, moet de imitatie-serviceaccount.

## <a name="next-steps"></a>Volgende stappen
[On-premises gateway](analysis-services-gateway.md)   
[Uw server beheren](analysis-services-manage.md)   

