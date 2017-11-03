---
title: Ondersteunde gegevensbronnen in Azure Data Catalog | Microsoft Docs
description: Dit artikel worden de specificaties van de momenteel ondersteunde gegevensbronnen.
services: data-catalog
documentationcenter: 
author: steelanddata
manager: jstevens
editor: 
tags: 
ms.assetid: fd4345ca-2ed8-4c5e-9c4b-f954be2fc9f9
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 08/15/2017
ms.author: maroche
ms.openlocfilehash: d6867c73bc6ea3c238cceef8a68466d451f3365c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="supported-data-sources-in-azure-data-catalog"></a>Ondersteunde gegevensbronnen in Azure Data Catalog

U kunt metagegevens publiceren met behulp van een openbare API of een klik-eenmaal registratie hulpprogramma of door het handmatig invoeren gegevens rechtstreeks naar de Azure Data Catalog web-portal. De volgende tabel geeft een overzicht van alle gegevensbronnen die worden ondersteund door de catalogus vandaag de dag en de mogelijkheden voor publiceren voor elk. Ook vermeld, zijn de hulpprogramma's voor externe gegevens die elke gegevensbron uit onze ervaring portal 'open-in' kan starten. De tweede tabel bevat een meer technische specificatie van elke gegevensbron verbinding-eigenschap.


## <a name="list-of-supported-data-sources"></a>Lijst met ondersteunde gegevensbronnen

<table>
    <tr>
       <td><b>Gegevensbronobject</b></td>
       <td><b>API</b></td>
       <td><b>Handmatige invoer</b></td>
       <td><b>Hulpprogramma voor registratie</b></td>
       <td><b>Open in hulpprogramma 's</b></td>
       <td><b>Opmerkingen bij de</b></td>
    </tr>
    <tr>
      <td>Azure Data Lake Store-map</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Azure Data Lake Store-bestand</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Azure Blob Storage</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Power BI</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Azure-opslagmap</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Power BI</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Azure Storage-tabel</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td>
        <font size="2"></font>
      </td>
      <td>
        <font size="2"></font>
      </td>
    </tr>
    <tr>
      <td>HDFS-directory</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>HDFS-bestand</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Hive-tabel</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Hive-weergave</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>MySQL-tabel</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, Power BI</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>MySQL-weergave</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, Power BI</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Oracle-databasetabel</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, Power BI</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Weergave van de Oracle-Database</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, Power BI</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Andere (algemene actief)</td>
      <td>✓</td>
      <td>✓</td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Azure SQL Data Warehouse-tabel</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, Power BI, SQL Server data tools</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>SQL Data Warehouse weergeven</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, Power BI, SQL Server data tools</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services-dimensie</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, Power BI</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>SQL Server analyseservices KPI</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, Power BI</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services-meting</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, Power BI</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Tabel met SQL Server Analysis Services</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, Power BI</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>SQL Server Reporting Services-rapport</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Browser</font></td>
      <td><font size=2>Native modus servers. SharePoint-modus wordt niet ondersteund.</font></td>
    </tr>
    <tr>
      <td>SQL Server-tabel</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, Power BI, SQL Server data tools</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Weergave van SQL Server</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel, Power BI, SQL Server data tools</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Tabel voor Teradata</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Weergave voor Teradata</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Excel</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>SAP HANA-weergave</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2>Power BI</font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>DB2-tabel</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>DB2 weergeven</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Bestandssysteem bestand</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>FTP-map</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>FTP-bestand</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>HTTP-rapport</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>HTTP-eindpunt</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>HTTP-bestand</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>OData-entiteitset</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>OData-functie</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>PostgreSQL-tabel</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>PostgreSQL weergeven</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>SAP HANA-weergave</td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td> SalesForce-object</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>SharePoint-lijst </td>
      <td>✓</td>
      <td></td>
      <td></td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Azure DB Cosmos-verzameling</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Algemene ODBC-tabel</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Algemene ODBC-weergave</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Cassandra tabel</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2>Als een algemene ODBC-asset publiceren</font></td>
    </tr>
    <tr>
      <td>Cassandra weergeven</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2>Als een algemene ODBC-asset publiceren</font></td>
    </tr>
    <tr>
      <td>Sybase-tabel</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>Sybase weergeven</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2></font></td>
    </tr>
    <tr>
      <td>MongoDB-tabel</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2>Als een algemene ODBC-asset publiceren</font></td>
    </tr>
    <tr>
      <td>MongoDB weergeven</td>
      <td>✓</td>
      <td>✓</td>
      <td>✓</td>
      <td><font size=2></font></td>
      <td><font size=2>Als een algemene ODBC-asset publiceren</font></td>
    </tr>
</table>

Als u ondersteuning voor aanvullende bronnen nodig, een functie aanvraag indient bij de [Azure Data Catalog forum](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409).


## <a name="data-source-reference-specification"></a>Gegevensbron Verwijzingsspecificatie
> [!NOTE]
> De **DSL structuur** kolom in de volgende tabel toont alleen de verbindingseigenschappen van de eigenschappenverzameling 'adres' die worden gebruikt door Azure Data Catalog. Dat wil zeggen, mag 'adres' eigenschappenverzameling andere eigenschappen van de gegevensbron waarmee Azure Data Catalog zich blijft voordoen, maar maakt geen gebruik van de verbinding.

<table>
    <tr>
       <td><b>Type gegevensbron</b></td>
       <td><b>Activatype</b></td>
       <td><b>Objecttypen</b></td>
       <td><b>DSL-structuur<b></td>
    </tr>
    <tr>
      <td>Azure Data Lake Store</td>
      <td>Container</td>
      <td>Data Lake</td>
      <td>
        <font size=2>Protocol: webhdfs <br>Verificatie: {basis, oauth} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>Azure Data Lake Store</td>
      <td>Tabel</td>
      <td>Map, bestand</td>
      <td>
        <font size=2>Protocol: webhdfs <br>Verificatie: {basis, oauth} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>Azure Storage</td>
      <td>Container</td>
      <td>Container</td>
      <td>
        <font size=2>Protocol: azure-BLOB 's <br>Verificatie: {azure--toegangssleutel} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;domein <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;account <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;container</font>
      </td>
    </tr>
    <tr>
      <td>Azure Storage</td>
      <td>Tabel</td>
      <td>BLOB, directory</td>
      <td>
        <font size=2>Protocol: azure-BLOB 's <br>Verificatie: {azure--toegangssleutel} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;domein <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;account <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;container <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;naam</font>
      </td>
    </tr>
    <tr>
      <td>Azure Storage</td>
      <td>Container</td>
      <td>Container</td>
      <td>
        <font size=2>Protocol: azure-tabellen <br>Verificatie: {azure--toegangssleutel} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;domein <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;account</font>
      </td>
    </tr>
    <tr>
      <td>Azure Storage</td>
      <td>Tabel</td>
      <td>Tabel</td>
      <td>
        <font size=2>Protocol: azure-tabellen <br>Verificatie: {azure--toegangssleutel} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;domein <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;account <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;naam</font>
      </td>
    </tr>
    <tr>
      <td>Cosmos</td>
      <td>Container</td>
      <td>Virtueel cluster</td>
      <td>
        <font size=2>Protocol: cosmos <br>Verificatie: {basic, windows} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>Cosmos</td>
      <td>Tabel</td>
      <td>Stream, stream is ingesteld, weergeven</td>
      <td>
        <font size=2>Protocol: cosmos <br>Verificatie: {basic, windows} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>Datazen</td>
      <td>Container</td>
      <td>Site</td>
      <td>
        <font size=2>Protocol: http <br>Verificatie: {none, basic, windows, oauth} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>Datazen</td>
      <td>Rapport</td>
      <td>Rapport, dashboard</td>
      <td>
        <font size=2>Protocol: http <br>Verificatie: {none, basic, windows, oauth} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>DB2</td>
      <td>Container</td>
      <td>Database</td>
      <td>
        <font size=2>Protocol: db2 <br>Verificatie: {basic, windows} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;database</font>
      </td>
    </tr>
    <tr>
      <td>DB2</td>
      <td>Tabel</td>
      <td>Tabel, weergave</td>
      <td>
        <font size=2>Protocol: db2 <br>Verificatie: {basic, windows} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;object <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schema</font>
      </td>
    </tr>
    <tr>
      <td>Bestandssysteem</td>
      <td>Tabel</td>
      <td>File</td>
      <td>
        <font size=2>Protocol: bestand <br>Verificatie: {none, basic, windows} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;pad</font>
      </td>
    </tr>
    <tr>
      <td>FTP</td>
      <td>Tabel</td>
      <td>Map, bestand</td>
      <td>
        <font size=2>Protocol: ftp <br>Verificatie: {none, basic, windows} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>Hadoop Distributed File System</td>
      <td>Container</td>
      <td>Cluster</td>
      <td>
        <font size=2>Protocol: webhdfs <br>Verificatie: {basis, oauth} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>Hadoop Distributed File System</td>
      <td>Tabel</td>
      <td>Map, bestand</td>
      <td>
        <font size=2>Protocol: webhdfs <br>Verificatie: {basis, oauth} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>Hive</td>
      <td>Container</td>
      <td>Database</td>
      <td>
        <font size=2>Protocol: hive <br>Verificatie: {HDInsight, basic, username, geen} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;database <br>connectionProperties: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;serverProtocol: {hive2}</font>
      </td>
    </tr>
    <tr>
      <td>Hive</td>
      <td>Tabel</td>
      <td>Tabel, weergave</td>
      <td>
        <font size=2>Protocol: hive <br>Verificatie: {HDInsight, basic, username, geen} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;object <br>connectionProperties: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;serverProtocol: {hive2}</font>
      </td>
    </tr>
    <tr>
      <td>HTTP</td>
      <td>Container</td>
      <td>Site</td>
      <td>
        <font size=2>Protocol: http <br>Verificatie: {none, basic, windows, oauth} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>HTTP</td>
      <td>Rapport</td>
      <td>Rapport, dashboard</td>
      <td>
        <font size=2>Protocol: http <br>Verificatie: {none, basic, windows, oauth} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>HTTP</td>
      <td>Tabel</td>
      <td>Eindpunt, bestand</td>
      <td>
        <font size=2>Protocol: http <br>Verificatie: {none, basic, windows, oauth} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>MySQL</td>
      <td>Container</td>
      <td>Database</td>
      <td>
        <font size=2>Protocol: mysql <br>Verificatie: {protocol, windows} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;database</font>
      </td>
    </tr>
    <tr>
      <td>MySQL</td>
      <td>Tabel</td>
      <td>Tabel, weergave</td>
      <td>
        <font size=2>Protocol: mysql <br>Verificatie: {protocol, windows} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;object</font>
      </td>
    </tr>
    <tr>
      <td>OData</td>
      <td>Container</td>
      <td>Entiteitscontainer</td>
      <td>
        <font size=2>Protocol: odata <br>Verificatie: {none, basic, windows} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>OData</td>
      <td>Tabel</td>
      <td>Entiteitset, functie</td>
      <td>
        <font size=2>Protocol: odata <br>Verificatie: {none, basic, windows} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;resource</font>
      </td>
    </tr>
    <tr>
      <td>Oracle Database</td>
      <td>Container</td>
      <td>Database</td>
      <td>
        <font size=2>Protocol: oracle <br>Verificatie: {protocol, windows} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;database</font>
      </td>
    </tr>
    <tr>
      <td>Oracle Database</td>
      <td>Tabel</td>
      <td>Tabel, weergave</td>
      <td>
        <font size=2>Protocol: oracle <br>Verificatie: {protocol, windows} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schema <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;object</font>
      </td>
    </tr>
    <tr>
      <td>PostgreSQL</td>
      <td>Container</td>
      <td>Database</td>
      <td>
        <font size=2>Protocol: postgresql <br>Verificatie: {basic, windows} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;database</font>
      </td>
    </tr>
    <tr>
      <td>PostgreSQL</td>
      <td>Tabel</td>
      <td>Tabel, weergave</td>
      <td>
        <font size=2>Protocol: postgresql <br>Verificatie: {basic, windows} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schema <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;object</font>
      </td>
    </tr>
    <tr>
      <td>Power BI</td>
      <td>Container</td>
      <td>Site</td>
      <td>
        <font size=2>Protocol: http <br>Verificatie: {none, basic, windows, oauth} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>Power BI</td>
      <td>Rapport</td>
      <td>Rapport, dashboard</td>
      <td>
        <font size=2>Protocol: http <br>Verificatie: {none, basic, windows, oauth} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>Power Query</td>
      <td>Tabel</td>
      <td>Gegevens mashup</td>
      <td>
        <font size=2>Protocol: power-query <br>Verificatie: {oauth} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>SalesForce</td>
      <td>Tabel</td>
      <td>Object</td>
      <td>
        <font size=2>Protocol: salesforce-com <br>Verificatie: {basic, windows} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;loginServer <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;klasse <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;itemName</font>
      </td>
    </tr>
    <tr>
      <td>SAP HANA</td>
      <td>Container</td>
      <td>Server</td>
      <td>
        <font size=2>Protocol: sap hana-sql <br>Verificatie: {protocol, windows} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server</font>
      </td>
    </tr>
    <tr>
      <td>SAP HANA</td>
      <td>Tabel</td>
      <td>Weergave</td>
      <td>
        <font size=2>Protocol: sap hana-sql <br>Verificatie: {protocol, windows} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schema <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;object</font>
      </td>
    </tr>
    <tr>
      <td>SharePoint</td>
      <td>Tabel</td>
      <td>Lijst</td>
      <td>
        <font size=2>Protocollen: sharepoint-lijst <br>Verificatie: {basic, windows} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL</font>
      </td>
    </tr>
    <tr>
      <td>SQL Data Warehouse</td>
      <td>Opdracht</td>
      <td>Opgeslagen procedure</td>
      <td>
        <font size=2>Protocol: tds <br>Verificatie: {protocol, windows} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schema <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;object</font>
      </td>
    </tr>
    <tr>
      <td>SQL Data Warehouse</td>
      <td>TableValuedFunction</td>
      <td>Functie met tabelwaarden</td>
      <td>
        <font size=2>Protocol: tds <br>Verificatie: {protocol, windows} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schema <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;object</font>
      </td>
    </tr>
    <tr>
      <td>SQL Data Warehouse</td>
      <td>Container</td>
      <td>Database</td>
      <td>
        <font size=2>Protocol: tds <br>Verificatie: {protocol, windows} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;database</font>
      </td>
    </tr>
    <tr>
      <td>SQL Data Warehouse</td>
      <td>Tabel</td>
      <td>Tabel, weergave</td>
      <td>
        <font size=2>Protocol: tds <br>Verificatie: {protocol, windows} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schema <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;object</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server</td>
      <td>Opdracht</td>
      <td>Opgeslagen procedure</td>
      <td>
        <font size=2>Protocol: tds <br>Verificatie: {protocol, windows} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schema <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;object</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server</td>
      <td>TableValuedFunction</td>
      <td>Functie met tabelwaarden</td>
      <td>
        <font size=2>Protocol: tds <br>Verificatie: {protocol, windows} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schema <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;object</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server</td>
      <td>Container</td>
      <td>Database</td>
      <td>
        <font size=2>Protocol: tds <br>Verificatie: {protocol, windows} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;database</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server</td>
      <td>Tabel</td>
      <td>Tabel, weergave</td>
      <td>
        <font size=2>Protocol: tds <br>Verificatie: {protocol, windows} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schema <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;object</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services multidimensionale</td>
      <td>Container</td>
      <td>Model</td>
      <td>
        <font size=2>Protocol: analyseservices <br>Verificatie: {windows, basic, anoniem, geen} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;model</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services multidimensionale</td>
      <td>KPI</td>
      <td>KPI</td>
      <td>
        <font size=2>Protocol: analyseservices <br>Verificatie: {windows, basic, anoniem, geen} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;model <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;object <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objectType: {KPI}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services multidimensionale</td>
      <td>meting</td>
      <td>meting</td>
      <td>
        <font size=2>Protocol: analyseservices <br>Verificatie: {windows, basic, anoniem, geen} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;model <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;object <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objectType: {Measure}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services multidimensionale</td>
      <td>Tabel</td>
      <td>Dimensie</td>
      <td>
        <font size=2>Protocol: analyseservices <br>Verificatie: {windows, basic, anoniem, geen} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;model <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;object <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objectType: {dimensie}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services in tabelvorm</td>
      <td>Container</td>
      <td>Model</td>
      <td>
        <font size=2>Protocol: analyseservices <br>Verificatie: {windows, basic, anoniem, geen} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;model</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services in tabelvorm</td>
      <td>KPI</td>
      <td>KPI</td>
      <td>
        <font size=2>Protocol: analyseservices <br>Verificatie: {windows, basic, anoniem, geen} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;model <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;object <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objectType: {KPI}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services in tabelvorm</td>
      <td>meting</td>
      <td>meting</td>
      <td>
        <font size=2>Protocol: analyseservices <br>Verificatie: {windows, basic, anoniem, geen} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;model <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;object <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objectType: {Measure}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Analysis Services in tabelvorm</td>
      <td>Tabel</td>
      <td>Tabel</td>
      <td>
        <font size=2>Protocol: analyseservices <br>Verificatie: {windows, basic, anoniem, geen} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;model <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;object <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;objectType: {Table}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Reporting Services</td>
      <td>Container</td>
      <td>Server</td>
      <td>
        <font size=2>Protocol: reporting services <br>Verificatie: {windows} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;versie: {ReportingService2010}</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Reporting Services</td>
      <td>Rapport</td>
      <td>Rapport</td>
      <td>
        <font size=2>Protocol: reporting services <br>Verificatie: {windows} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;pad <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;versie: {ReportingService2010}</font>
      </td>
    </tr>
    <tr>
      <td>Teradata</td>
      <td>Container</td>
      <td>Database</td>
      <td>
        <font size=2>Protocol: teradata <br>Verificatie: {protocol, windows} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;database</font>
      </td>
    </tr>
    <tr>
      <td>Teradata</td>
      <td>Tabel</td>
      <td>Tabel, weergave</td>
      <td>
        <font size=2>Protocol: teradata <br>Verificatie: {protocol, windows} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;object</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Master Data Services</td>
      <td>Container</td>
      <td>Model</td>
      <td>
        <font size="2">Protocol: mssql-mds <br>Verificatie: {windows} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;model <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Versie</font>
      </td>
    </tr>
    <tr>
      <td>SQL Server Master Data Services</td>
      <td>Tabel</td>
      <td>Entiteit</td>
      <td>
        <font size="2">Protocol: mssql-mds <br>Verificatie: {windows} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;model <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Versie <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;entiteit</font>
      </td>
    </tr>
    <tr>
      <td>Azure Cosmos DB</td>
      <td>Container</td>
      <td>Database</td>
      <td>
        <font size=2>Protocol: document-db <br>Verificatie: {azure--toegangssleutel} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;database</font>
      </td>
    </tr>
    <tr>
      <td>Azure Cosmos DB</td>
      <td>Verzameling</td>
      <td>Verzameling</td>
      <td>
        <font size=2>Protocol: document-db <br>Verificatie: {azure--toegangssleutel} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;URL <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;verzameling</font>
      </td>
    </tr>
    <tr>
      <td>Algemene ODBC</td>
      <td>Container</td>
      <td>Database</td>
      <td>
        <font size=2>Protocol: odbc <br>Verificatie: {basic, windows} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Opties <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;database</font>
      </td>
    </tr>
    <tr>
      <td>Algemene ODBC</td>
      <td>Tabel</td>
      <td>Tabel, weergave</td>
      <td>
        <font size=2>Protocol: odbc <br>Verificatie: {basic, windows} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Opties <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;object <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schema</font>
      </td>
    </tr>
    <tr>
      <td>Sybase</td>
      <td>Container</td>
      <td>Database</td>
      <td>
        <font size=2>Protocol: sybase <br>Verificatie: {basic, windows} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;database</font>
      </td>
    </tr>
    <tr>
      <td>Sybase</td>
      <td>Tabel</td>
      <td>Tabel, weergave</td>
      <td>
        <font size=2>Protocol: sybase <br>Verificatie: {basic, windows} <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Server <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;database <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;schema <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;object</font>
      </td>
    </tr>
    <tr>
      <td>Andere (geen van de bovenstaande)</td>
      <td>\*</td>
      <td>\*</td>
      <td>
        <font size=2>Protocol: algemene-asset <br>Adres: <br>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;item-id hebt</font>
      </td>
    </tr>
</table>
