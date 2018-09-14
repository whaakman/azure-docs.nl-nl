---
title: Ondersteunde gegevensbronnen in Azure Data Catalog
description: In dit artikel geeft een lijst met specificaties van de momenteel ondersteunde gegevensbronnen.
services: data-catalog
author: steelanddata
ms.author: maroche
ms.assetid: fd4345ca-2ed8-4c5e-9c4b-f954be2fc9f9
ms.service: data-catalog
ms.topic: conceptual
ms.date: 01/18/2018
ms.openlocfilehash: 7b3f403ac479e0a6a8a4ae46c2a5ab448bd8ecc2
ms.sourcegitcommit: f983187566d165bc8540fdec5650edcc51a6350a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/13/2018
ms.locfileid: "45544127"
---
# <a name="supported-data-sources-in-azure-data-catalog"></a>Ondersteunde gegevensbronnen in Azure Data Catalog

U kunt metagegevens publiceren met behulp van een openbare API of een klik-eenmaal registratie hulpprogramma of door het handmatig invoeren van gegevens rechtstreeks naar de Azure Data Catalog-webportal. De volgende tabel geeft een overzicht van alle gegevensbronnen die worden ondersteund door de catalogus vandaag en de mogelijkheden voor publiceren voor elk. Ook vermeld, zijn de hulpprogramma's voor externe gegevens die elke gegevensbron vanuit de portal 'open-in'-ervaring starten kunt. De tweede tabel bevat een meer technische specificatie van elke gegevensbron verbinding-eigenschap.


## <a name="list-of-supported-data-sources"></a>Lijst met ondersteunde gegevensbronnen

<table>
    <tr>
       <td><b>Gegevensbronobject</b></td>
       <td><b>API</b></td>
       <td><b>Handmatige invoer</b></td>
       <td><b>Hulpprogramma voor registratie</b></td>
       <td><b>Openen in hulpprogramma 's</b></td>
       <td><b>Opmerkingen</b></td>
    </tr>
    <tr>
      <td>Azure Data Lake Store-map</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>Azure Data Lake Store-bestand</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>Azure Blob Storage</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td>Power BI Desktop</td>
      <td></td>
    </tr>
    <tr>
      <td>Azure Storage-directory</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td>Power BI Desktop</td>
      <td></td>
    </tr>
    <tr>
      <td>Azure Storage-tabel</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td>
        <font size="2">
      </td>
      <td>
        <font size="2">
      </td>
    </tr>
    <tr>
      <td>HDFS-map</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>HDFS-bestand</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>Hive-tabel</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td>Excel</td>
      <td></td>
    </tr>
    <tr>
      <td>Hive-weergave</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td>Excel</td>
      <td></td>
    </tr>
    <tr>
      <td>MySQL-tabel</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td>Excel, Power BI Desktop</td>
      <td></td>
    </tr>
    <tr>
      <td>MySQL-weergave</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td>Excel, Power BI Desktop</td>
      <td></td>
    </tr>
    <tr>
      <td>Oracle Database-tabel</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td>Excel, Power BI Desktop</td>
      <td></td>
    </tr>
    <tr>
      <td>Oracle Database-weergave</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td>Excel, Power BI Desktop</td>
      <td></td>
    </tr>
    <tr>
      <td>Overige (algemene actief)</td>
      <td>✓</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>Azure SQL Data Warehouse-tabel</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td>Excel, Power BI Desktop, SQL Server data tools</td>
      <td></td>
    </tr>
    <tr>
      <td>SQL Data Warehouse-weergave</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td>Excel, Power BI Desktop, SQL Server data tools</td>
      <td></td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services-dimensie</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td>Excel, Power BI Desktop</td>
      <td></td>
    </tr>
    <tr>
      <td>SQL Server analyseservices KPI</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td>Excel, Power BI Desktop</td>
      <td></td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services-meting</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td>Excel, Power BI Desktop</td>
      <td></td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services-tabel</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td>Excel, Power BI Desktop</td>
      <td></td>
    </tr>
    <tr>
      <td>SQL Server Reporting Services-rapport</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td>Browser</td>
      <td>Native modus servers. SharePoint-modus wordt niet ondersteund.</td>
    </tr>
    <tr>
      <td>SQL Server-tabel</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td>Excel, Power BI Desktop, SQL Server data tools</td>
      <td></td>
    </tr>
    <tr>
      <td>SQL Server-weergave</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td>Excel, Power BI Desktop, SQL Server data tools</td>
      <td></td>
    </tr>
    <tr>
      <td>Teradata-tabel</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td>Excel</td>
      <td></td>
    </tr>
    <tr>
      <td>Teradata-weergave</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td>Excel</td>
      <td></td>
    </tr>
    <tr>
      <td>SAP HANA-weergave</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td>Power BI Desktop</td>
      <td></td>
    </tr>
    <tr>
      <td>DB2-tabel</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>DB2-weergave</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>Systeembestand</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>FTP-map</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>FTP-bestand</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>HTTP-rapport</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>HTTP-eindpunt</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>HTTP-bestand</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>OData-entiteitsset</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>OData-functie</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>PostgreSQL-tabel</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>PostgreSQL-weergave</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>SAP HANA-weergave</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td> SalesForce-object</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>SharePoint-lijst </td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>Azure Cosmos DB-verzameling</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td></td>
      <td>Alleen verouderde verzamelingen van Azure DocumentDB en verzamelingen in Azure Cosmos DB SQL API zijn compatibel. Nieuwere Cosmos DB-API's zijn nog niet ondersteund. Kies Azure DocumentDB in de lijst van de gegevensbron.</td>
    </tr>
    <tr>
      <td>Algemene ODBC-tabel</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>Algemene ODBC-weergave</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>Cassandra-tabel</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td></td>
      <td>Publiceren als een algemene ODBC-asset</td>
    </tr>
    <tr>
      <td>Cassandra-weergave</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td></td>
      <td>Publiceren als een algemene ODBC-asset</td>
    </tr>
    <tr>
      <td>Sybase-tabel</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>Sybase-weergave</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td></td>
      <td></td>
    </tr>
    <tr>
      <td>MongoDB-tabel</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td></td>
      <td>Publiceren als een algemene ODBC-asset</td>
    </tr>
    <tr>
      <td>MongoDB-weergave</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td></td>
      <td>Publiceren als een algemene ODBC-asset</td>
    </tr>
</table>

Als u wilt weergeven van een specifieke gegevensbron wordt ondersteund, suggestie (of het ondersteuningsteam van uw stem als deze al is voorgesteld) door te gaan naar de [gegevenscatalogus op de Azure-Forums voor Feedback](https://feedback.azure.com/forums/906052-data-catalog).


## <a name="data-source-reference-specification"></a>Referentie-specificatie van gegevensbron
> [!NOTE]
> De **DSL-structuur** kolom in de volgende tabel bevat alleen de verbindingseigenschappen voor de eigenschappenverzameling "adres" die worden gebruikt door Azure Data Catalog. Dat wil zeggen, mag "adres" eigenschappenverzameling andere eigenschappen van de verbinding van de gegevensbron die Azure Data Catalog zich blijft voordoen, maar maakt geen gebruik.

<table>
    <tr>
       <td><b>Gegevensbrontype</b></td>
       <td><b>Activatype</b></td>
       <td><b>Objecttypen</b></td>
       <td><b>DSL-structuur<b></td>
    </tr>
    <tr>
      <td>Azure Data Lake Store</td>
      <td>Container</td>
      <td>Data Lake</td>
      <td>
        
            Protocol: webhdfs
            <br>Authentication: {basic, oauth}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url
        
      </td>
    </tr>
    <tr>
      <td>Azure Data Lake Store</td>
      <td>Table</td>
      <td>Directory, file</td>
      <td>
        
            Protocol: webhdfs
            <br>Authentication: {basic, oauth}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url
        
      </td>
    </tr>
    <tr>
      <td>Azure Storage</td>
      <td>Container</td>
      <td>Container</td>
      <td>
        
            Protocol: azure-blobs
            <br>Authentication: {azure-access-key}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; domain
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; account
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; container
        
      </td>
    </tr>
    <tr>
      <td>Azure Storage</td>
      <td>Table</td>
      <td>Blob, directory</td>
      <td>
        
            Protocol: azure-blobs
            <br>Authentication: {azure-access-key}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; domain
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; account
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; container
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; name
        
      </td>
    </tr>
    <tr>
      <td>Azure Storage</td>
      <td>Container</td>
      <td>Container</td>
      <td>
        
            Protocol: azure-tables
            <br>Authentication: {azure-access-key}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; domain
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; account
        
      </td>
    </tr>
    <tr>
      <td>Azure Storage</td>
      <td>Table</td>
      <td>Table</td>
      <td>
        
            Protocol: azure-tables
            <br>Authentication: {azure-access-key}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; domain
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; account
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; name
        
      </td>
    </tr>
    <tr>
      <td>Datazen</td>
      <td>Container</td>
      <td>Site</td>
      <td>
        
            Protocol: http
            <br>Authentication: {none, basic, windows, oauth}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url
        
      </td>
    </tr>
    <tr>
      <td>Datazen</td>
      <td>Report</td>
      <td>Report, dashboard</td>
      <td>
        
            Protocol: http
            <br>Authentication: {none, basic, windows, oauth}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url
        
      </td>
    </tr>
    <tr>
      <td>DB2</td>
      <td>Container</td>
      <td>Database</td>
      <td>
        
            Protocol: db2
            <br>Authentication: {basic, windows}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database
        
      </td>
    </tr>
    <tr>
      <td>DB2</td>
      <td>Table</td>
      <td>Table, view</td>
      <td>
        
            Protocol: db2
            <br>Authentication: {basic, windows}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema
        
      </td>
    </tr>
    <tr>
      <td>File system</td>
      <td>Table</td>
      <td>File</td>
      <td>
        
            Protocol: file
            <br>Authentication: {none, basic, windows}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; path
        
      </td>
    </tr>
    <tr>
      <td>FTP</td>
      <td>Table</td>
      <td>Directory, file</td>
      <td>
        
            Protocol: ftp
            <br>Authentication: {none, basic, windows}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url
        
      </td>
    </tr>
    <tr>
      <td>Hadoop Distributed File System</td>
      <td>Container</td>
      <td>Cluster</td>
      <td>
        
            Protocol: webhdfs
            <br>Authentication: {basic, oauth}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url
        
      </td>
    </tr>
    <tr>
      <td>Hadoop Distributed File System</td>
      <td>Table</td>
      <td>Directory, file</td>
      <td>
        
            Protocol: webhdfs
            <br>Authentication: {basic, oauth}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url
        
      </td>
    </tr>
    <tr>
      <td>Hive</td>
      <td>Container</td>
      <td>Database</td>
      <td>
        
            Protocol: hive
            <br>Authentication: {HDInsight, basic, username, none}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database
            <br>connectionProperties:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; serverProtocol: {hive2}
        
      </td>
    </tr>
    <tr>
      <td>Hive</td>
      <td>Table</td>
      <td>Table, view</td>
      <td>
        
            Protocol: hive
            <br>Authentication: {HDInsight, basic, username, none}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object
            <br>connectionProperties:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; serverProtocol: {hive2}
        
      </td>
    </tr>
    <tr>
      <td>HTTP</td>
      <td>Container</td>
      <td>Site</td>
      <td>
        
            Protocol: http
            <br>Authentication: {none, basic, windows, oauth}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url
        
      </td>
    </tr>
    <tr>
      <td>HTTP</td>
      <td>Report</td>
      <td>Report, dashboard</td>
      <td>
        
            Protocol: http
            <br>Authentication: {none, basic, windows, oauth}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url
        
      </td>
    </tr>
    <tr>
      <td>HTTP</td>
      <td>Table</td>
      <td>Endpoint, file</td>
      <td>
        
            Protocol: http
            <br>Authentication: {none, basic, windows, oauth}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url
        
      </td>
    </tr>
    <tr>
      <td>MySQL</td>
      <td>Container</td>
      <td>Database</td>
      <td>
        
            Protocol: mysql
            <br>Authentication: {protocol, windows}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database
        
      </td>
    </tr>
    <tr>
      <td>MySQL</td>
      <td>Table</td>
      <td>Table, view</td>
      <td>
        
            Protocol: mysql
            <br>Authentication: {protocol, windows}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object
        
      </td>
    </tr>
    <tr>
      <td>OData</td>
      <td>Container</td>
      <td>Entity container</td>
      <td>
        
            Protocol: odata
            <br>Authentication: {none, basic, windows}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url
        
      </td>
    </tr>
    <tr>
      <td>OData</td>
      <td>Table</td>
      <td>Entity set, function</td>
      <td>
        
            Protocol: odata
            <br>Authentication: {none, basic, windows}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; resource
        
      </td>
    </tr>
    <tr>
      <td>Oracle Database</td>
      <td>Container</td>
      <td>Database</td>
      <td>
        
            Protocol: oracle
            <br>Authentication: {protocol, windows}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database
        
      </td>
    </tr>
    <tr>
      <td>Oracle Database</td>
      <td>Table</td>
      <td>Table, view</td>
      <td>
        
            Protocol: oracle
            <br>Authentication: {protocol, windows}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object
        
      </td>
    </tr>
    <tr>
      <td>PostgreSQL</td>
      <td>Container</td>
      <td>Database</td>
      <td>
        
            Protocol: postgresql
            <br>Authentication: {basic, windows}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database
        
      </td>
    </tr>
    <tr>
      <td>PostgreSQL</td>
      <td>Table</td>
      <td>Table, view</td>
      <td>
        
            Protocol: postgresql
            <br>Authentication: {basic, windows}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object
        
      </td>
    </tr>
    <tr>
      <td>Power BI Desktop</td>
      <td>Container</td>
      <td>Site</td>
      <td>
        
            Protocol: http
            <br>Authentication: {none, basic, windows, oauth}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url
        
      </td>
    </tr>
    <tr>
      <td>Power BI Desktop</td>
      <td>Report</td>
      <td>Report, dashboard</td>
      <td>
        
            Protocol: http
            <br>Authentication: {none, basic, windows, oauth}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url
        
      </td>
    </tr>
    <tr>
      <td>Power Query</td>
      <td>Table</td>
      <td>Data mashup</td>
      <td>
        
            Protocol: power-query
            <br>Authentication: {oauth}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url
        
      </td>
    </tr>
    <tr>
      <td>Salesforce</td>
      <td>Table</td>
      <td>Object</td>
      <td>
        
            Protocol: salesforce-com
            <br>Authentication: {basic, windows}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; loginServer
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; class
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; itemName
        
      </td>
    </tr>
    <tr>
      <td>SAP HANA</td>
      <td>Container</td>
      <td>Server</td>
      <td>
        
            Protocol: sap-hana-sql
            <br>Authentication: {protocol, windows}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server
        
      </td>
    </tr>
    <tr>
      <td>SAP HANA</td>
      <td>Table</td>
      <td>View</td>
      <td>
        
            Protocol: sap-hana-sql
            <br>Authentication: {protocol, windows}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object
        
      </td>
    </tr>
    <tr>
      <td>SharePoint</td>
      <td>Table</td>
      <td>List</td>
      <td>
        
            Protocol: sharepoint-list
            <br>Authentication: {basic, windows}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url
        
      </td>
    </tr>
    <tr>
      <td>SQL Data Warehouse</td>
      <td>Command</td>
      <td>Stored procedure</td>
      <td>
        
            Protocol: tds
            <br>Authentication: {protocol, windows}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object
        
      </td>
    </tr>
    <tr>
      <td>SQL Data Warehouse</td>
      <td>TableValuedFunction</td>
      <td>Table-valued function</td>
      <td>
        
            Protocol: tds
            <br>Authentication: {protocol, windows}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object
        
      </td>
    </tr>
    <tr>
      <td>SQL Data Warehouse</td>
      <td>Container</td>
      <td>Database</td>
      <td>
        
            Protocol: tds
          <br>Authentication: {protocol, windows}
          <br>Address:
          <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server
          <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database
        
      </td>
    </tr>
    <tr>
      <td>SQL Data Warehouse</td>
      <td>Table</td>
      <td>Table, view</td>
      <td>
        
            Protocol: tds
            <br>Authentication: {protocol, windows}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object
        
      </td>
    </tr>
    <tr>
      <td>SQL Server</td>
      <td>Command</td>
      <td>Stored procedure</td>
      <td>
        
            Protocol: tds
            <br>Authentication: {protocol, windows}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object
        
      </td>
    </tr>
    <tr>
      <td>SQL Server</td>
      <td>TableValuedFunction</td>
      <td>Table-valued function</td>
      <td>
        
            Protocol: tds
            <br>Authentication: {protocol, windows}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object
        
      </td>
    </tr>
    <tr>
      <td>SQL Server</td>
      <td>Container</td>
      <td>Database</td>
      <td>
        
            Protocol: tds
            <br>Authentication: {protocol, windows}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database
        
      </td>
    </tr>
    <tr>
      <td>SQL Server</td>
      <td>Table</td>
      <td>Table, view</td>
      <td>
        
            Protocol: tds
            <br>Authentication: {protocol, windows}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object
        
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services multidimensional</td>
      <td>Container</td>
      <td>Model</td>
      <td>
        
            Protocol: analysis-services
            <br>Authentication: {windows, basic, anonymous, none}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model
        
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services multidimensional</td>
      <td>KPI</td>
      <td>KPI</td>
      <td>
        
            Protocol: analysis-services
            <br>Authentication: {windows, basic, anonymous, none}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; objectType: {KPI}
        
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services multidimensional</td>
      <td>Measure</td>
      <td>Measure</td>
      <td>
        
            Protocol: analysis-services
            <br>Authentication: {windows, basic, anonymous, none}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; objectType: {Measure}
        
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services multidimensional</td>
      <td>Table</td>
      <td>Dimension</td>
      <td>
        
            Protocol: analysis-services
            <br>Authentication: {windows, basic, anonymous, none}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; objectType: {Dimension}
        
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services tabular</td>
      <td>Container</td>
      <td>Model</td>
      <td>
        
            Protocol: analysis-services
            <br>Authentication: {windows, basic, anonymous, none}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model
        
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services tabular</td>
      <td>KPI</td>
      <td>KPI</td>
      <td>
        
            Protocol: analysis-services
            <br>Authentication: {windows, basic, anonymous, none}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; objectType: {KPI}
        
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services tabular</td>
      <td>Measure</td>
      <td>Measure</td>
      <td>
        
            Protocol: analysis-services
            <br>Authentication: {windows, basic, anonymous, none}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; objectType: {Measure}
        
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services tabular</td>
      <td>Table</td>
      <td>Table</td>
      <td>
        
            Protocol: analysis-services
            <br>Authentication: {windows, basic, anonymous, none}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; objectType: {Table}
        
      </td>
    </tr>
    <tr>
      <td>SQL Server Reporting Services</td>
      <td>Container</td>
      <td>Server</td>
      <td>
        
            Protocol: reporting-services
            <br>Authentication: {windows}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; version: {ReportingService2010}
        
      </td>
    </tr>
    <tr>
      <td>SQL Server Reporting Services</td>
      <td>Report</td>
      <td>Report</td>
      <td>
        
            Protocol: reporting-services
            <br>Authentication: {windows}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; path
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; version: {ReportingService2010}
        
      </td>
    </tr>
    <tr>
      <td>Teradata</td>
      <td>Container</td>
      <td>Database</td>
      <td>
        
            Protocol: teradata
            <br>Authentication: {protocol, windows}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database
        
      </td>
    </tr>
    <tr>
      <td>Teradata</td>
      <td>Table</td>
      <td>Table, view</td>
      <td>
        
            Protocol: teradata
            <br>Authentication: {protocol, windows}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object
        
      </td>
    </tr>
    <tr>
      <td>SQL Server Master Data Services</td>
      <td>Container</td>
      <td>Model</td>
      <td>
        <font size="2">
          Protocol: mssql-mds
          <br>Authentication: {windows}
          <br>Address:
          <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url
          <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model
          <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; version
        
      </td>
    </tr>
    <tr>
      <td>SQL Server Master Data Services</td>
      <td>Table</td>
      <td>Entity</td>
      <td>
        <font size="2">
          Protocol: mssql-mds
          <br>Authentication: {windows}
          <br>Address:
          <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url
          <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; model
          <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; version
          <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; entity
        
      </td>
    </tr>
    <tr>
      <td>Azure Cosmos DB</td>
      <td>Container</td>
      <td>Database</td>
      <td>
        
            Protocol: document-db
            <br>Authentication: {azure-access-key}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database
        
      </td>
    </tr>
    <tr>
      <td>Azure Cosmos DB</td>
      <td>Collection</td>
      <td>Collection</td>
      <td>
        
            Protocol: document-db
            <br>Authentication: {azure-access-key}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; url
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; collection
        
      </td>
    </tr>
    <tr>
      <td>Generic ODBC</td>
      <td>Container</td>
      <td>Database</td>
      <td>
        
            Protocol: odbc
            <br>Authentication: {basic, windows}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; options
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database
        
      </td>
    </tr>
    <tr>
      <td>Generic ODBC</td>
      <td>Table</td>
      <td>Table, View</td>
      <td>
        
            Protocol: odbc
            <br>Authentication: {basic, windows}
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; options
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema
        
      </td>
    </tr>
    <tr>
      <td>Sybase</td>
      <td>Container</td>
      <td>Database</td>
      <td>
        
            protocol: sybase
            <br>authentication: {basic, windows}
            <br>address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database
        
      </td>
    </tr>
    <tr>
      <td>Sybase</td>
      <td>Table</td>
      <td>Table, View</td>
      <td>
        
            protocol: sybase
            <br>authentication: {basic, windows}
            <br>address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; server
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; database
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; schema
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; object
        
      </td>
    </tr>
    <tr>
      <td>Other (none of the above)</td>
      <td>&#10033;</td>
      <td>&#10033;</td>
      <td>
        
            Protocol: generic-asset
            <br>Address:
            <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; assetId
        
      </td>
    </tr>
</table>
