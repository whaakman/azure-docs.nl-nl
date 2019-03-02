---
title: Auditing in Azure SQL datawarehouse | Microsoft Docs
description: Meer informatie over controle en over het instellen van de controle voor Azure SQL Data Warehouse.
services: sql-data-warehouse
author: KavithaJonnakuti
manager: craigg
ms.service: sql-data-warehouse
ms.topic: conceptual
ms.subservice: manage
ms.date: 04/11/2018
ms.author: kavithaj
ms.reviewer: igorstan
ms.openlocfilehash: d2093c5b4c07e6e62df4d1f52a7fbe6e12a91ea0
ms.sourcegitcommit: c712cb5c80bed4b5801be214788770b66bf7a009
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/01/2019
ms.locfileid: "57217050"
---
# <a name="auditing-in-azure-sql-data-warehouse"></a>Auditing in Azure SQL datawarehouse

Meer informatie over controle en over het instellen van de controle voor Azure SQL Data Warehouse.

## <a name="what-is-auditing"></a>Wat is controleren?
Controle van SQL Data Warehouse kunt u gebeurtenissen in uw database naar een auditlogboek in uw Azure Storage-account aanmelden record. Controles, kunt u naleving van regelgeving, inzicht in de databaseactiviteiten en inzicht krijgen in discrepanties en afwijkingen die kunnen wijzen op problemen voor het bedrijf of vermoedelijke beveiligingsschendingen.

Controleprogramma's inschakelen en voldoet aan de standaarden voor compliance vergemakkelijken, maar niet garanderen dat. Zie voor meer informatie over Azure-programma's die ondersteuning voor standaarden naleving, de [Azure Trust Center](https://azure.microsoft.com/support/trust-center/compliance/).

## <a id="subheading-1"></a>Grondbeginselen van controle
Controle van SQL Data Warehouse-database, kunt u:

* **Behouden** een audittrail van de geselecteerde gebeurtenissen. Categorieën van de databaseacties moeten worden gecontroleerd, kunt u definiëren.
* **Rapport** op database-activiteit. U kunt vooraf geconfigureerde rapporten en een dashboard snel aan de slag met de activiteit en rapportage.
* **Analyseren** rapporten. U kunt verdachte gebeurtenissen, ongebruikelijke activiteiten en trends vinden.

Auditlogboeken worden opgeslagen in uw Azure storage-account. U kunt een bewaarperiode van audit log definiëren.


## <a id="subheading-4"></a>Definiëren op serverniveau versus controlebeleid op databaseniveau

Een controlebeleid kan worden gedefinieerd voor een specifieke database of als een standaardbeleid voor server:

* Een serverbeleid **is van toepassing op alle bestaande en nieuwe databases** op de server.

* Als *controle is ingeschakeld*, het *altijd van toepassing op de database*. De database wordt gecontroleerd, ongeacht de database controle-instellingen.

* Inschakelen van controle op de database, naast het inschakelen van deze op de server heeft *niet* overschrijven of wijzigen van de instellingen van de controlefunctie voor de server. Beide controles wordt naast elkaar bestaan. Met andere woorden, wordt de database gecontroleerd tweemaal in parallelle; eenmaal door het serverbeleid en eenmaal door het beleid van de database.

> [!NOTE]
> Het wordt aangeraden dat u inschakelt **controlefunctie voor blobs alleen op serverniveau** en laat u de controle op databaseniveau uitgeschakeld voor alle databases.
> Vermijd het inschakelen van servercontrole en het Databasecontrole samen, tenzij:
> * U wilt gebruiken een andere *opslagaccount* of *bewaarperiode* voor een specifieke database.
> * U wilt controleren gebeurtenistypen of categorieën voor een specifieke database die van de rest van de databases op de server afwijken. Bijvoorbeeld, mogelijk hebt tabel ingevoegd die moeten worden gecontroleerd alleen voor een specifieke database.
> * U wilt gebruiken, detectie van bedreigingen momenteel is alleen ondersteund met de controle op databaseniveau.

> [!IMPORTANT]
>Inschakelen van controle op een Azure SQL Data Warehouse, of op een server met een Azure SQL Data Warehouse, **zal leiden tot het datawarehouse wordt hervat**, zelfs in het geval waarin het programma eerder is onderbroken. **Zorg ervoor dat u de datawarehouse onderbreken opnieuw na het inschakelen van controle**.

## <a id="subheading-5"></a>Op serverniveau voor alle databases controle instellen

Een serverbeleid voor controle is van toepassing op **alle bestaande en nieuwe databases** op de server.

Het volgende gedeelte bevat de configuratie van de controle met Azure portal.

1. Ga naar de [Azure Portal](https://portal.azure.com).
2. Ga naar de **SQL server** die u wilt controleren (belangrijk is, zorg ervoor dat u de SQL-server, niet een bepaalde database/DW bekijkt). In de **Security** in het menu **controle en detectie van bedreigingen**.

    ![Navigatievenster][6]
4. In de *controle en detectie van bedreigingen* blade voor **controle** Selecteer **ON**. Deze controle beleid wordt toegepast op alle bestaande en nieuwe databases op deze server.

    ![Navigatievenster][7]
5. Om te openen de **Audit Logs opslag** Selecteer **opslaggegevens**. Selecteer of maak de Azure storage-account waar de logboeken worden opgeslagen en selecteer vervolgens de bewaarperiode (oude logboeken worden verwijderd). Klik vervolgens op **OK**.

    ![Navigatievenster][8]

    > [!IMPORTANT]
    > Auditlogboeken worden geschreven naar **toevoeg-Blobs** in Azure Blob-opslag in uw Azure-abonnement.
    >
    > - Alle soorten opslag (v1, v2, blob) worden ondersteund.
    > - Replicatie van alle opslagconfiguraties worden ondersteund.
    > - **Premium-opslag** is momenteel **niet ondersteund**.
    > - **Opslag in VNet** is momenteel **niet ondersteund**.
    > - **De opslag achter een Firewall** is momenteel **niet ondersteund**

8. Klik op **Opslaan**.



## <a id="subheading-2"></a>Controle op databaseniveau voor één database instellen

U kunt een controlebeleid voor een specifieke database of als een server standaardbeleid definiëren.

Het is raadzaam om te gebruiken op serverniveau controle en geen controle op databaseniveau, zoals beschreven in [definiëren op serverniveau versus controlebeleid op databaseniveau](#subheading-4)

Voordat het instellen van audit, auditing selectievakje in als u een ['Downlevel Client'](sql-data-warehouse-auditing-downlevel-clients.md).


1. Start de [Azure-portal](https://portal.azure.com).
2. Ga naar **instellingen** voor de SQL Data Warehouse die u wilt controleren. Selecteer **controle en detectie van bedreigingen**.

    ![][1]
3. Schakel vervolgens controleren door te klikken op de **ON** knop.

    ![][3]
4. Selecteer in het deelvenster controle configuratie **OPSLAGGEGEVENS** om het deelvenster Audit Logs opslag te openen. Selecteer het Azure storage-account voor de logboeken en de bewaarperiode.
    >[!TIP]
    >Gebruik hetzelfde opslagaccount voor alle gecontroleerde databases om het meeste uit de vooraf geconfigureerde rapporten sjablonen.

    ![][4]

5. Klik op de **OK** om op te slaan van de opslagconfiguratie voor meer informatie.
6. Onder **LOGBOEKREGISTRATIE door GEBEURTENIS**, klikt u op **SUCCES** en **fout** aan te melden van alle gebeurtenissen of afzonderlijke gebeurteniscategorieën kiezen.
7. Als u controle voor een database configureert, moet u mogelijk om te wijzigen van de verbindingsreeks van uw client om te controleren of de controle van de gegevens correct wordt vastgelegd. Controleer de [FQDN van Server wijzigen in de connection string](sql-data-warehouse-auditing-downlevel-clients.md) onderwerp voor downlevel-clientverbindingen.
8. Klik op **OK**.

## <a id="subheading-3"></a>Analyseren van controlelogboeken en -rapporten

### <a name="server-level-policy-audit-logs"></a>De logboeken voor controle op serverniveau beleid
De logboeken voor controle op serverniveau worden geschreven naar **toevoeg-Blobs** in een Azure Blob-opslag in uw Azure-abonnement. Deze worden opgeslagen als een verzameling van blob-bestanden in een container met de naam **sqldbauditlogs**.

Zie voor meer informatie over de hiërarchie van de map storage naamgevingsregels en logboekindeling, de [Blobverwijzing Audit Log indeling](https://go.microsoft.com/fwlink/?linkid=829599).

Er zijn verschillende methoden die u gebruiken kunt om blob auditing logboeken weer te geven:

* Gebruik **controlebestanden samenvoegen** in SQL Server Management Studio (te beginnen met SSMS 17):
    1. Selecteer in het menu SSMS **bestand** > **Open** > **controlebestanden samenvoegen**.

    2. De **controlebestanden toevoegen** in het dialoogvenster wordt geopend. Selecteer een van de **toevoegen** opties te kiezen of u wilt samenvoegen auditbestanden van een lokale schijf of ze importeren uit Azure Storage. U moet uw Azure Storage-gegevens en de accountsleutel.

    3. Nadat alle bestanden samenvoegen zijn toegevoegd, klikt u op **OK** om de samenvoegbewerking te voltooien.

    4. Het samengevoegde bestand wordt geopend in SSMS, waar u kunt weergeven en analyseren, evenals exporteren naar een xel-bestand of een CSV-bestand of een tabel.

* Gebruik de [toepassing synchroniseren](https://github.com/Microsoft/Azure-SQL-DB-auditing-OMS-integration) die we hebben gemaakt. Het wordt uitgevoerd in Azure en maakt gebruik van logboekanalyse openbare API's om de push-SQL-controlelogboeken in Azure Monitor-Logboeken. De synchronisatie-toepassing pushes SQL-controlelogboeken in Azure Monitor-logboeken voor gebruik via de log analytics-dashboard.

* Power BI gebruiken. U kunt bekijken en analyseren van logboekgegevens audit in Power BI. Meer informatie over [Power BI en toegang tot een sjabloon downloaden](https://blogs.msdn.microsoft.com/azuresqldbsupport/2017/05/26/sql-azure-blob-auditing-basic-power-bi-dashboard/).

* Downloaden van de logboekbestanden van uw Azure Storage blob-container via de portal of met behulp van een hulpprogramma zoals [Azure Storage Explorer](http://storageexplorer.com/).
    * Nadat u een logboekbestand lokaal hebt gedownload, kunt u dubbelklikken op het bestand te openen, weergeven en analyseren van de logboeken in SSMS.
    * U kunt ook meerdere bestanden tegelijkertijd via Azure Storage Explorer downloaden. Met de rechtermuisknop op een specifieke submap en selecteer **opslaan als** om op te slaan in een lokale map.

* Aanvullende methoden:
   * Nadat u hebt meerdere bestanden of een submap met de logboekbestanden gedownload, kunt u deze lokaal zoals beschreven in de controlebestanden SSMS-samenvoegen-instructies die eerder zijn beschreven samenvoegen.

   * Controlefunctie voor blobs weergeven logboeken via een programma:

     * Gebruik de [uitgebreide gebeurtenissen-lezer](https://blogs.msdn.microsoft.com/extended_events/2011/07/20/introducing-the-extended-events-reader/) C#-bibliotheek.
     * [Uitgebreide gebeurtenissen querybestanden](https://sqlscope.wordpress.com/2014/11/15/reading-extended-event-files-using-client-side-tools-only/) met behulp van PowerShell.



<br>
### <a name="database-level-policy-audit-logs"></a>De logboeken voor controle op databaseniveau beleid
De logboeken voor controle op databaseniveau worden samengevoegd in een verzameling van Store tabellen met een **SQLDBAuditLogs** voorvoegsel in de Azure storage-account dat u hebt gekozen tijdens de installatie. U kunt met behulp van een hulpprogramma zoals logboekbestanden weergeven [Azure Storage Explorer](http://azurestorageexplorer.codeplex.com).

Een rapportsjabloon vooraf geconfigureerde dashboard is beschikbaar als een [downloadbare Excel-werkblad](https://go.microsoft.com/fwlink/?LinkId=403540) waarmee u snel analyseren logboekgegevens. Als u de sjabloon op uw auditlogboeken, moet u Excel 2013 of later en Power Query, die u kunt [download hier](https://www.microsoft.com/download/details.aspx?id=39379).

De sjabloon bevat fictieve voorbeeldgegevens, en kunt u Power Query voor het importeren van uw auditlogboek rechtstreeks vanuit uw Azure storage-account instellen.

## <a id="subheading-4"></a>Toegangssleutel voor opslag
In de productieomgeving bent u waarschijnlijk uw opslagsleutels periodiek te vernieuwen. Bij het vernieuwen van uw sleutels, moet u het beleid op te slaan. Het proces is als volgt:

1. Wijzig in het Configuratiescherm, die wordt beschreven in de voorgaande sectie controle-instellingen, controle de **toegangssleutel voor opslag** van *primaire* naar *secundaire* en  **Sla**.

   ![][4]
2. Ga naar het deelvenster van de configuratie van opslag en **opnieuw genereren** de *primaire toegangssleutel*.
3. Ga terug naar het deelvenster Controle configureren
4. Ga de **toegangssleutel voor opslag** van *secundaire* naar *primaire* en druk op **opslaan**.
4. Ga terug naar de gebruikersinterface van de opslag en **opnieuw genereren** de *secundaire toegangssleutel* (als voorbereiding op de volgende sleutels vernieuwen cyclus.

## <a id="subheading-5"></a>Automation (PowerShell/REST API)
U kunt ook configureren om controle in Azure SQL Data Warehouse met behulp van de volgende hulpprogramma's voor automation:

* **PowerShell-cmdlets**:

   * [Get-AzureRMSqlDatabaseAuditingPolicy](/powershell/module/azurerm.sql/get-azurermsqldatabaseauditingpolicy)
   * [Get-AzureRMSqlServerAuditingPolicy](/powershell/module/azurerm.sql/Get-AzureRMSqlServerAuditingPolicy)
   * [Remove-AzureRMSqlDatabaseAuditing](/powershell/module/azurerm.sql/Remove-AzureRMSqlDatabaseAuditing)
   * [Remove-AzureRMSqlServerAuditing](/powershell/module/azurerm.sql/Remove-AzureRMSqlServerAuditing)
   * [Set-AzureRMSqlDatabaseAuditingPolicy](/powershell/module/azurerm.sql/Set-AzureRMSqlDatabaseAuditingPolicy)
   * [Set-AzureRMSqlServerAuditingPolicy](/powershell/module/azurerm.sql/Set-AzureRMSqlServerAuditingPolicy)
   * [Use-AzureRMSqlServerAuditingPolicy](/powershell/module/azurerm.sql/Use-AzureRMSqlServerAuditingPolicy)


## <a name="downlevel-clients-support-for-auditing-and-dynamic-data-masking"></a>Ondersteuning voor downlevel-clients voor controle en dynamische gegevensmaskering
Controle werkt met SQL-clients die ondersteuning bieden voor TDS-omleiding.

Een willekeurige client die wordt geïmplementeerd TDS 7.4 moet ook ondersteuning bieden voor omleiden. Uitzonderingen op deze omvatten JDBC 4.0 waarin de functie voor omleiding wordt niet volledig ondersteund en Tedious voor Node.JS in waarvoor omleiding is niet geïmplementeerd.

Voor 'Downlevel-clients' die ondersteuning voor TDS-versie 7.3 en hieronder, wijzigt de FQDN-naam van de server in de verbindingsreeks als volgt:

- Oorspronkelijke server FQDN-naam in de connection string: <*servernaam*>. database.windows.net
- Gewijzigde server FQDN-naam in de connection string: <*servernaam*> .database. **beveiligde**. windows.net

Een gedeeltelijke lijst van 'Downlevel-clients' bevat:

* .NET 4.0 en lager,
* ODBC-10.0 en lager.
* JDBC (Hoewel JDBC TDS 7.4 ondersteunt, de functie voor TDS-omleiding wordt niet volledig ondersteund)
* Tedious (voor Node.JS)

**Opmerking:** De voorgaande wijziging van de FQDN-server zijn mogelijk nuttig zijn ook voor het toepassen van een beleid voor controle van SQL Server op zonder dat nodig is voor een configuratiestap in elke database (tijdelijke risicobeperking).     




<!--Anchors-->
[Database Auditing basics]: #subheading-1
[Set up auditing for your database]: #subheading-2
[Analyze audit logs and reports]: #subheading-3
[Define server-level vs. database-level auditing policy]: #subheading-4
[Set up server-level auditing for all databases]: #subheading-5

<!--Image references-->
[1]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing.png
[2]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-inherit.png
[3]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-enable.png
[4]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-storage-account.png
[5]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-dashboard.png
[6]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-server_1_overview.png
[7]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-server_2_enable.png
[8]: ./media/sql-data-warehouse-auditing-overview/sql-data-warehouse-auditing-server_3_storage.png
