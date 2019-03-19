---
title: Ondersteunde gegevensbronnen in Azure Analysis Services | Microsoft Docs
description: Beschrijving van gegevensbronnen die worden ondersteund voor gegevensmodellen in Azure Analysis Services.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 02/07/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: eeb3b2098cf477004ba7ad85a903f1a8e2267392
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58117826"
---
# <a name="data-sources-supported-in-azure-analysis-services"></a>Ondersteunde gegevensbronnen in Azure Analysis Services

Gegevensbronnen en connectors die wordt weergegeven in gegevens ophalen of in de Wizard importeren in Visual Studio worden weergegeven voor zowel Azure Analysis Services en SQL Server Analysis Services. Echter niet alle gegevensbronnen en connectors die worden weergegeven in Azure Analysis Services worden ondersteund. De soorten gegevensbronnen die u kunt verbinding maken met zijn afhankelijk van veel factoren af zoals model compatibiliteitsniveau, beschikbare gegevensconnectors verificatietype, providers en ondersteuning voor On-premises data gateway. 

## <a name="azure-data-sources"></a>Azure-gegevensbronnen

|Gegevensbron  |In het geheugen  |DirectQuery  |
|---------|---------|---------|
|Azure SQL Database<sup>[2](#azsqlmanaged)</sup>     |   Ja      |    Ja      |
|Azure SQL Data Warehouse     |   Ja      |   Ja       |
|Azure Blob Storage<sup>[1](#tab1400a)</sup>     |   Ja       |    Nee      |
|Azure Table Storage<sup>[1](#tab1400a)</sup>    |   Ja       |    Nee      |
|Azure Cosmos DB<sup>[1](#tab1400a)</sup>     |  Ja        |  Nee        |
|Azure Data Lake Store (Gen1)<sup>[1](#tab1400a)</sup>, <sup>[4](#gen2)</sup>      |   Ja       |    Nee      |
|Azure HDInsight HDFS<sup>[1](#tab1400a)</sup>     |     Ja     |   Nee       |
|Azure HDInsight Spark<sup>[1](#tab1400a)</sup>, <sup>[3](#databricks)</sup>     |   Ja       |   Nee       |
||||

<a name="tab1400a">1</a> -tabular 1400 en alleen hogere modellen.   
<a name="azsqlmanaged">2</a> -azure SQL Database Managed Instance wordt ondersteund. Een beheerd exemplaar wordt uitgevoerd in Azure VNet met een privé IP-adres, is een On-premises gegevensgateway vereist.   
<a name="databricks">3</a> - azure Databricks met behulp van de Spark-connector wordt momenteel niet ondersteund.   
<a name="gen2">4</a> -ADLS Gen2 wordt momenteel niet ondersteund.


**Provider**   
In-memory en DirectQuery-modellen verbinding te maken met Azure-gegevensbronnen gebruikt .NET Framework Data Provider voor SQL Server.

## <a name="on-premises-data-sources"></a>On-premises gegevensbronnen

Verbinding maken met on-premises gegevensbronnen van en Azure-AS-server moet een On-premises gateway. Bij het gebruik van een gateway is 64-bits-providers zijn vereist.

### <a name="in-memory-and-directquery"></a>In-memory en DirectQuery

|Gegevensbron | In-memory-provider | DirectQuery-provider |
|  --- | --- | --- |
| SQL Server |SQL Server Native Client 11.0, Microsoft OLE DB-Provider voor SQL Server, .NET Framework Data Provider voor SQL Server | .NET framework Data Provider voor SQL Server |
| SQL Server datawarehouse |SQL Server Native Client 11.0, Microsoft OLE DB-Provider voor SQL Server, .NET Framework Data Provider voor SQL Server | .NET framework Data Provider voor SQL Server |
| Oracle |Microsoft OLE DB-Provider voor Oracle, Oracle-gegevensprovider voor .NET |Oracle-gegevensprovider voor .NET |
| Teradata |OLE DB-Provider voor Teradata, Teradata-gegevensprovider voor .NET |Teradata-gegevensprovider voor .NET |
| | | |

### <a name="in-memory-only"></a>In het geheugen alleen

|Gegevensbron  |  
|---------|
|Access-Database     |  
|Active Directory<sup>[1](#tab1400b)</sup>     |  
|Analysis Services     |  
|Analytics Platform System     |  
|Dynamics CRM<sup>[1](#tab1400b)</sup>     |  
|Excel-werkmap     |  
|Exchange<sup>[1](#tab1400b)</sup>     |  
|Map<sup>[1](#tab1400b)</sup>     |
|IBM Informix<sup>[1](#tab1400b) </sup> (bèta) |
|JSON-document<sup>[1](#tab1400b)</sup>     |  
|Regels van binair<sup>[1](#tab1400b)</sup>     | 
|MySQL-database     | 
|OData-Feed<sup>[1](#tab1400b)</sup>     |  
|ODBC query     | 
|OLE DB     |   
|Postgre SQL-Database<sup>[1](#tab1400b)</sup>    | 
|SalesForce-objecten<sup>[1](#tab1400b)</sup> |  
|SalesForce-rapporten<sup>[1](#tab1400b)</sup> |
|SAP HANA<sup>[1](#tab1400b)</sup>    |  
|SAP Business Warehouse<sup>[1](#tab1400b)</sup>    |  
|SharePoint<sup>[1](#tab1400b)</sup>     |   
|Sybase-database     |  
|XML-tabel<sup>[1](#tab1400b)</sup>    |  
||
 
<a name="tab1400b">1</a> tabular 1400 en alleen hogere modellen.

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

