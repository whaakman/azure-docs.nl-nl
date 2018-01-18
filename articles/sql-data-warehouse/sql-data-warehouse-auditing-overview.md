---
title: Controle van Azure SQL datawarehouse | Microsoft Docs
description: Aan de slag met Azure SQL Data Warehouse controle
services: sql-data-warehouse
documentationcenter: 
author: ronortloff
manager: jhubbard
editor: 
ms.assetid: 0e6af148-b218-4b43-bb5f-907917d20330
ms.service: sql-data-warehouse
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: security
ms.date: 01/16/2018
ms.author: rortloff;barbkess
ms.openlocfilehash: 5400f29d8c7579809ef7b2a084115473df7baa85
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2018
---
# <a name="auditing-in-azure-sql-data-warehouse"></a>Controle van Azure SQL datawarehouse

Controle van SQL Data Warehouse kunt u record gebeurtenissen in de database naar een auditlogboek in uw Azure Storage-account worden geregistreerd. Controle, kunt u de naleving van regelgeving onderhouden, de activiteit in een database begrijpen en meer inzicht krijgen in discrepanties en afwijkingen die kunnen wijzen op problemen voor het bedrijf of vermoedelijke beveiligingsschendingen. Controle van SQL Data Warehouse ook worden geïntegreerd met Microsoft Power BI voor rapportage en analyse.

Controleprogramma's inschakelen en voldoen aan de nalevingsstandaards vergemakkelijken, maar niet garanderen dat. Zie voor meer informatie over Azure programma's die nalevingsscan standaarden, de <a href="http://azure.microsoft.com/support/trust-center/compliance/" target="_blank">Azure Vertrouwenscentrum</a>.

## <a id="subheading-1"></a>Controle-basisbeginselen
Controle van SQL Data Warehouse-database, kunt u:

* **Behouden** een audittrail van de geselecteerde gebeurtenissen. Categorieën van databaseacties moeten worden gecontroleerd, kunt u definiëren.
* **Rapport** op database-activiteit. U kunt vooraf geconfigureerde rapporten en een dashboard snel aan de slag met de activiteit en rapportage.
* **Analyseren** rapporten. U kunt verdachte gebeurtenissen, ongebruikelijke activiteiten en trends vinden.

U kunt configureren om controle-instellingen voor de volgende gebeurteniscategorieën:

**Gewone SQL** en **geparametriseerde SQL** waarvoor de controlelogboeken van de verzamelde zijn geclassificeerd als  

* **Toegang tot gegevens**
* **Wijzigingen in het schema (DDL)**
* **Gegevenswijzigingen (DML)**
* **Accounts, rollen en machtigingen (DCL)**
* **Opgeslagen Procedure**, **aanmelding** en, **transactiebeheer**.

Voor elke gebeurteniscategorie controle van **geslaagd** en **fout** bewerkingen afzonderlijk geconfigureerd.

Zie voor meer informatie over de activiteiten en gebeurtenissen die worden gecontroleerd, de <a href="http://go.microsoft.com/fwlink/?LinkId=506733" target="_blank">indeling verwijzing naar controlelogboek (doc-bestand downloaden)</a>.

Auditlogboeken worden opgeslagen in uw Azure storage-account. U kunt een bewaarperiode van audit log definiëren.

U kunt een controlebeleid voor een specifieke database of als een server standaardbeleid definiëren. Een controle standaardbeleid voor server geldt voor alle databases op een server waarop geen een specifieke database controle beleid gedefinieerd.

Voordat u kunt instellen audit controle selectievakje in als u een ['Downlevel Client'.](sql-data-warehouse-auditing-downlevel-clients.md)

## <a id="subheading-2"></a>Controle voor uw database instellen
1. Start de <a href="https://portal.azure.com" target="_blank">Azure-portal</a>.
2. Ga naar **instellingen** voor de SQL Data Warehouse die u wilt controleren. Selecteer **controle en detectie van bedreigingen**.
   
    ![][1]
3. Schakel vervolgens de controle door te klikken op de **ON** knop.
   
    ![][3]
4. Selecteer in het deelvenster controle configuratie **OPSLAGGROEP** om het deelvenster Audit logboeken opslag te openen. Selecteer de Azure storage-account voor de logboeken en de bewaarperiode. 
>[!TIP]
>Gebruik hetzelfde opslagaccount voor alle gecontroleerde databases optimaal buiten de vooraf geconfigureerde rapporten sjablonen.
   
    ![][4]
5. Klik op de **OK** om op te slaan de opslagconfiguratie voor meer informatie.
6. Onder **LOGBOEKREGISTRATIE door GEBEURTENIS**, klikt u op **geslaagd** en **fout** met alle gebeurtenissen of kies een afzonderlijke gebeurteniscategorieën.
7. Als u controle voor een database configureren wilt, moet u mogelijk voor het wijzigen van de verbindingsreeks van de client om controle van de gegevens correct wordt vastgelegd. Controleer de [Server FDQN wijzigen in de verbindingsreeks](sql-data-warehouse-auditing-downlevel-clients.md) onderwerp voor downlevel-clientverbindingen.
8. Klik op **OK**.

## <a id="subheading-3"></a>Controlelogboeken en -rapporten analyseren
Controlelogboeken worden geaggregeerd in een verzameling van Store tabellen met een **SQLDBAuditLogs** -voorvoegsel op in de Azure storage-account dat u hebt gekozen tijdens de installatie. U kunt met een hulpprogramma zoals logboekbestanden weergeven <a href="http://azurestorageexplorer.codeplex.com/" target="_blank">Azure Opslagverkenner</a>.

Een vooraf geconfigureerde dashboard rapportsjabloon is beschikbaar als een <a href="http://go.microsoft.com/fwlink/?LinkId=403540" target="_blank">downloadbare Excel-spreadsheet</a> waarmee u kunt snel analyseren logboekgegevens. De sjabloon wilt gebruiken op uw controlelogboeken, moet u Excel 2013 of later en Power Query die u kunt downloaden <a href="http://www.microsoft.com/download/details.aspx?id=39379">hier</a>.

De sjabloon met fictieve voorbeeldgegevens erin en kunt u Power Query voor het importeren van het controlelogboek rechtstreeks vanuit uw Azure storage-account instellen.

## <a id="subheading-4"></a>Opslag van sessiesleutels
In productie bent u waarschijnlijk uw opslagsleutels periodiek te vernieuwen. Wanneer uw sleutels vernieuwen, moet u het beleid op te slaan. Het proces is als volgt:

1. Wijzig in het Configuratiescherm, die wordt beschreven in de voorgaande sectie controle-instellingen, controle de **toegangssleutel voor opslag** van *primaire* naar *secundaire* en  **Sla**.

   ![][4]
2. Ga naar het deelvenster van de configuratie van opslag en **genereren** de *primaire toegangssleutel*.
3. Ga terug naar het deelvenster controle configuratie 
4. schakelt de **toegangssleutel voor opslag** van *secundaire* naar *primaire* en druk op **opslaan**.
4. Ga terug naar de opslag gebruikersinterface en **genereren** de *secundaire toegangssleutel* (als voorbereiding op de volgende sleutels vernieuwen cyclus.

## <a id="subheading-5"></a>Automation (PowerShell/REST API)
U kunt ook configureren om controle in Azure SQL Data Warehouse met behulp van de volgende automation-hulpprogramma's:

* **PowerShell-cmdlets**:

   * [Get-AzureRMSqlDatabaseAuditingPolicy](/powershell/module/azurerm.sql/get-azurermsqldatabaseauditingpolicy)
   * [Get-AzureRMSqlServerAuditingPolicy](/powershell/module/azurerm.sql/Get-AzureRMSqlServerAuditingPolicy)
   * [Remove-AzureRMSqlDatabaseAuditing](/powershell/module/azurerm.sql/Remove-AzureRMSqlDatabaseAuditing)
   * [Remove-AzureRMSqlServerAuditing](/powershell/module/azurerm.sql/Remove-AzureRMSqlServerAuditing)
   * [Set-AzureRMSqlDatabaseAuditingPolicy](/powershell/module/azurerm.sql/Set-AzureRMSqlDatabaseAuditingPolicy)
   * [Set-AzureRMSqlServerAuditingPolicy](/powershell/module/azurerm.sql/Set-AzureRMSqlServerAuditingPolicy)
   * [Use-AzureRMSqlServerAuditingPolicy](/powershell/module/azurerm.sql/Use-AzureRMSqlServerAuditingPolicy)


## <a name="downlevel-clients-support-for-auditing-and-dynamic-data-masking"></a>Ondersteuning voor downlevel-clients voor controle en dynamische gegevensmaskering
Controle werkt met SQL-clients die ondersteuning bieden voor omleiden van TDS.

Een client die wordt geïmplementeerd TDS 7.4 ook ondersteunen omleiding. Uitzonderingen op deze omvatten JDBC 4.0 waarin de functie voor omleiding wordt niet volledig ondersteund en Tedious voor Node.JS in welke omleiding is niet geïmplementeerd.

Voor 'Downlevel-clients' die TDS-versie 7.3 ondersteunen en hieronder, wijzigt de FQDN-naam van de server in de verbindingstekenreeks als volgt:

- Oorspronkelijke server FQDN-naam in de verbindingsreeks: <*servernaam*>. database.windows.net
- Gewijzigde FQDN van de server in de verbindingsreeks: <*servernaam*> .database. **beveiligde**. windows.net

Een gedeeltelijke lijst met 'Downlevel-clients' omvat:

* .NET 4.0 en lager,
* ODBC-10.0 en lager.
* JDBC (Hoewel JDBC TDS 7.4 ondersteunt, de TDS-omleiding-functie is niet volledig ondersteund)
* Omslachtig (voor Node.JS)

**Opmerking:** de voorgaande server FDQN wijziging mogelijk handig ook voor het toepassen van een beleid voor controle van SQL Server op zonder behoefte aan een configuratie stap in elke database (tijdelijke risicobeperking).     




<!--Anchors-->
[Database Auditing basics]: #subheading-1
[Set up auditing for your database]: #subheading-2
[Analyze audit logs and reports]: #subheading-3


<!--Image references-->
[1]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing.png
[2]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-inherit.png
[3]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-enable.png
[4]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-storage-account.png
[5]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-dashboard.png


