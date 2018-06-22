---
title: Gegevensbronnen die worden ondersteund in Azure Analysis Services | Microsoft Docs
description: Beschrijft de gegevensbronnen die worden ondersteund voor gegevensmodellen in Azure Analysis Services.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 06/21/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 60b6769dd3c15bbe628f60318cc08fcac78357ea
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/21/2018
ms.locfileid: "36307956"
---
# <a name="data-sources-supported-in-azure-analysis-services"></a>Gegevensbronnen die worden ondersteund in Azure Analysis Services

Gegevensbronnen en weergegeven in de gegevens opvragen of de Wizard importeren in Visual Studio-connectors worden weergegeven voor zowel Azure Analysis Services en SQL Server Analysis Services. Echter niet alle gegevensbronnen en weergegeven connectors worden ondersteund in Azure Analysis Services. De soorten gegevensbronnen die kunt u afhankelijk van veel factoren, zoals het model compatibiliteitsniveau, beschikbare gegevens connectors verificatietype, providers en On-premises gateway gegevensondersteuning. 

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

\* 1400-modellen in tabelvorm alleen.

**Provider**   
In het geheugen en DirectQuery-modellen die verbinding maken met Azure-gegevensbronnen gebruiken .NET Framework Data Provider voor SQL Server.

## <a name="on-premises-data-sources"></a>On-premises gegevensbronnen

Verbinding maken met lokale gegevensbronnen uit en AS Azure-server moet een On-premises gateway. Wanneer u een gateway, zijn 64-bits providers vereist.

### <a name="in-memory-and-directquery"></a>In het geheugen- en DirectQuery

|Gegevensbron | In het geheugen-provider | DirectQuery-provider |
|  --- | --- | --- |
| SQL Server |SQL Server Native Client 11.0, Microsoft OLE DB-Provider voor SQL Server, .NET Framework Data Provider voor SQL Server | .NET framework Data Provider voor SQL Server |
| SQL Server-datawarehouse |SQL Server Native Client 11.0, Microsoft OLE DB-Provider voor SQL Server, .NET Framework Data Provider voor SQL Server | .NET framework Data Provider voor SQL Server |
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
|Regels uit binair *     | 
|MySQL-database     | 
|OData-Feed *     |  
|ODBC-query     | 
|OLE DB     |   
|Postgre SQL Database *    | 
|SalesForce objecten * |  
|SalesForce-rapporten * |
|SAP HANA *    |  
|SAP Business Warehouse *    |  
|SharePoint*     |   
|Sybase-database     |  
|XML-tabel *    |  
|||
 
\* 1400-modellen in tabelvorm alleen.

## <a name="specifying-a-different-provider"></a>Geef een andere provider

Gegevensmodellen in Azure Analysis Services moet mogelijk verschillende gegevensproviders bij het verbinden met bepaalde gegevensbronnen. In sommige gevallen kunnen verbinding maken met gegevensbronnen met behulp van systeemeigen providers zoals SQL Server Native Client (SQLNCLI11) modellen in tabelvorm een fout geretourneerd. Als u native providers dan SQLOLEDB, ziet u mogelijk foutbericht: **de provider 'SQLNCLI11.1' is niet geregistreerd**. Of, als u een verbinding te maken met on-premises gegevensbronnen DirectQuery-model hebt en u systeemeigen providers gebruiken, ziet u mogelijk foutbericht: **fout bij het maken van de OLE DB-rijenset. Onjuiste syntaxis bij 'LIMIET'**.

Wanneer een lokale SQL Server Analysis Services model in tabelvorm migreert naar Azure Analysis Services, is het mogelijk nodig om te wijzigen van de provider.

**Om op te geven van een provider**

1. In SSDT > **Tabellaire Model Explorer** > **gegevensbronnen**, met de rechtermuisknop op een gegevensbronverbinding en klik vervolgens op **gegevensbron bewerken**.
2. In **verbinding bewerken**, klikt u op **Geavanceerd** om de geavanceerde eigenschappen-venster te openen.
3. In **geavanceerde eigenschappen instellen** > **Providers**, selecteer vervolgens de juiste provider.

## <a name="impersonation"></a>Imitatie
In sommige gevallen is het mogelijk nodig om op te geven van een andere imitatieaccount. Imitatieaccount kan worden opgegeven in Visual Studio (SSDT) of SSMS.

Voor on-premises gegevensbronnen:

* Als SQL-verificatie wordt gebruikt, moet de imitatie-serviceaccount.
* Als Windows-verificatie wordt gebruikt, moet u Windows-gebruiker/wachtwoord instellen. Voor SQL Server, wordt Windows-verificatie met een specifieke imitatie-account alleen ondersteund voor modellen van gegevens in het geheugen.

Voor cloud-gegevensbronnen:

* Als SQL-verificatie wordt gebruikt, moet de imitatie-serviceaccount.

## <a name="next-steps"></a>Volgende stappen
[Lokale gateway](analysis-services-gateway.md)   
[Beheren van uw server](analysis-services-manage.md)   

