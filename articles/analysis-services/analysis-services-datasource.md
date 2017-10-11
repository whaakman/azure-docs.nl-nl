---
title: Gegevensbronnen die worden ondersteund in Azure Analysis Services | Microsoft Docs
description: Beschrijft de gegevensbronnen die worden ondersteund voor gegevensmodellen in Azure Analysis Services.
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 6ec63319-ff9b-4b01-a1cd-274481dc8995
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/15/2017
ms.author: owend
ms.openlocfilehash: 8bd6c3b5a923ce2f3cd0f60af82e59c5cc27cbb4
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="data-sources-supported-in-azure-analysis-services"></a>Gegevensbronnen die worden ondersteund in Azure Analysis Services
Azure Analysis Services-servers ondersteunen verbinding te maken met gegevensbronnen in de cloud en on-premises in uw organisatie. Extra ondersteunde gegevensbronnen worden altijd toegevoegd. Terugkomen vaak. 

De volgende gegevensbronnen worden momenteel ondersteund:

| Cloud  |
|---|
| Azure Blob Storage *  |
| Azure SQL Database  |
| Azure datawarehouse |


| On-premises  |   |   |   |
|---|---|---|---|
| Access-Database  | Map * | Oracle Database  | Teradata Database |
| Active Directory *  | JSON-document *  | Postgre SQL Database *  |XML-tabel * |
| Analysis Services  | Regels uit binair *  | SAP HANA *  |
| Analytics Platform System  | MySQL-database  | SAP Business Warehouse *  | |
| Dynamics CRM *  | OData-Feed *  | SharePoint *  |
| Excel-werkmap  | ODBC-query  | SQL Database  |
| Exchange *  | OLE DB  | Sybase-Database  |

\*1400-modellen in tabelvorm alleen. 

> [!IMPORTANT]
> Verbinding maken met on-premises gegevensbronnen vereisen een [On-premises gegevensgateway](analysis-services-gateway.md) geïnstalleerd op een computer in uw omgeving.

## <a name="data-providers"></a>Gegevensproviders

Gegevensmodellen in Azure Analysis Services moet mogelijk verschillende gegevensproviders bij het verbinden met bepaalde gegevensbronnen. In sommige gevallen kunnen verbinding maken met gegevensbronnen met behulp van systeemeigen providers zoals SQL Server Native Client (SQLNCLI11) modellen in tabelvorm een fout geretourneerd.

Gegevensbron voor gegevensmodellen die verbinding met een cloudgegevens maken zoals Azure SQL Database, als u native providers dan SQLOLEDB gebruikt, ziet u mogelijk foutbericht: **'Voor de provider 'SQLNCLI11.1' is niet geregistreerd.'** Of, als u een verbinding te maken met on-premises gegevensbronnen, DirectQuery-model hebt als u native providers u foutbericht weergegeven: **"Fout bij het maken van de OLE DB-rijenset. Onjuiste syntaxis bij 'LIMIET' '**.

De volgende datasource-providers worden ondersteund voor in het geheugen of de DirectQuery-modellen voor gegevens bij het verbinden met gegevensbronnen in de cloud of on-premises:

### <a name="cloud"></a>Cloud
| **Gegevensbron** | **In het geheugen** | **DirectQuery** |
|  --- | --- | --- |
| Azure SQL Data Warehouse |.NET framework Data Provider voor SQL Server |.NET framework Data Provider voor SQL Server |
| Azure SQL Database |.NET framework Data Provider voor SQL Server |.NET framework Data Provider voor SQL Server | |

### <a name="on-premises-via-gateway"></a>On-premises (via de Gateway)
|**Gegevensbron** | **In het geheugen** | **DirectQuery** |
|  --- | --- | --- |
| SQL Server |SQL Server Native Client 11.0 |.NET framework Data Provider voor SQL Server |
| SQL Server |Microsoft OLE DB-Provider voor SQL Server |.NET framework Data Provider voor SQL Server | |
| SQL Server |.NET framework Data Provider voor SQL Server |.NET framework Data Provider voor SQL Server | |
| Oracle |Microsoft OLE DB-Provider voor Oracle |Oracle-gegevensprovider voor .NET | |
| Oracle |Oracle-gegevensprovider voor .NET |Oracle-gegevensprovider voor .NET | |
| Teradata |OLE DB-Provider voor Teradata |Teradata-gegevensprovider voor .NET | |
| Teradata |Teradata-gegevensprovider voor .NET |Teradata-gegevensprovider voor .NET | |
| Analytics Platform System |.NET framework Data Provider voor SQL Server |.NET framework Data Provider voor SQL Server | |

> [!NOTE]
> Zorg ervoor dat op 64-bits providers zijn geïnstalleerd bij gebruik van On-premises gateway.
> 
> 

Wanneer een lokale SQL Server Analysis Services model in tabelvorm migreert naar Azure Analysis Services, is het mogelijk nodig om te wijzigen van de provider.

**De provider van een gegevensbron opgeven**

1. In SSDT > **Tabellaire Model Explorer** > **gegevensbronnen**, met de rechtermuisknop op een gegevensbronverbinding en klik vervolgens op **gegevensbron bewerken**.
2. In **verbinding bewerken**, klikt u op **Geavanceerd** om de geavanceerde eigenschappen-venster te openen.
3. In **geavanceerde eigenschappen instellen** > **Providers**, selecteer vervolgens de juiste provider.

## <a name="impersonation"></a>Imitatie
In sommige gevallen is het mogelijk nodig om op te geven van een andere imitatieaccount. Imitatieaccount kan worden opgegeven in SSDT of SSMS.

Voor on-premises gegevensbronnen:

* Als SQL-verificatie wordt gebruikt, moet de imitatie-serviceaccount.
* Als Windows-verificatie wordt gebruikt, moet u Windows-gebruiker/wachtwoord instellen. Voor SQL Server, wordt Windows-verificatie met een specifieke imitatie-account alleen ondersteund voor modellen van gegevens in het geheugen.

Voor cloud-gegevensbronnen:

* Als SQL-verificatie wordt gebruikt, moet de imitatie-serviceaccount.

## <a name="next-steps"></a>Volgende stappen
Als u de on-premises gegevensbronnen hebt, moet u voor het installeren van de [On-premises gateway](analysis-services-gateway.md).   
Zie voor meer informatie over het beheren van uw server in SSDT of SSMS, [beheren van uw server](analysis-services-manage.md).

