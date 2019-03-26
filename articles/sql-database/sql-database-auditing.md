---
title: Aan de slag met Azure SQL database auditing | Microsoft Docs
description: Gebruik Azure SQL database auditing voor het bijhouden van databasegebeurtenissen in een logboek.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: vainolo
ms.author: arib
ms.reviewer: vanto
manager: craigg
ms.date: 02/07/2019
ms.openlocfilehash: a98ec18f2ed38b290d04c3fdc36d9a6ff80a675a
ms.sourcegitcommit: 280d9348b53b16e068cf8615a15b958fccad366a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/25/2019
ms.locfileid: "58407348"
---
# <a name="get-started-with-sql-database-auditing"></a>Aan de slag met SQL Database Auditing

Controle-instellingen voor Azure [SQL-Database](sql-database-technical-overview.md) en [SQL Data Warehouse](../sql-data-warehouse/sql-data-warehouse-overview-what-is.md) databasegebeurtenissen bijgehouden en geschreven naar een auditlogboek in uw Azure storage-account, OMS-werkruimte of Eventhubs. Ook controle:

- Helpt u naleving van regelgeving, inzicht in de databaseactiviteiten en inzicht krijgen in discrepanties en afwijkingen die kunnen wijzen op problemen voor het bedrijf of vermoedelijke beveiligingsschendingen.

- Hiermee wordt en vergemakkelijkt de naleving van standaarden voor compliance, hoewel het nalevingsbeleid geen garantie. Zie voor meer informatie over Azure-programma's die ondersteuning voor standaarden naleving, de [Azure Trust Center](https://azure.microsoft.com/support/trust-center/compliance/).


> [!NOTE] 
> Dit onderwerp is van toepassing op Azure SQL-servers en op SQL Database- en SQL Data Warehouse-databases die op deze Azure SQL-servers worden gemaakt. Voor het gemak wordt de term 'SQL Database' gebruikt wanneer er wordt verwezen naar zowel SQL Database als SQL Data Warehouse.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]


## <a id="subheading-1"></a>Azure SQL database auditing-overzicht

U kunt gebruiken om SQL database auditing aan:

- **Behouden** een audittrail van de geselecteerde gebeurtenissen. Categorieën van de databaseacties moeten worden gecontroleerd, kunt u definiëren.
- **Rapport** op database-activiteit. U kunt vooraf geconfigureerde rapporten en een dashboard snel aan de slag met de activiteit en rapportage.
- **Analyseren** rapporten. U kunt verdachte gebeurtenissen, ongebruikelijke activiteiten en trends vinden.

> [!IMPORTANT]
> Auditlogboeken worden geschreven naar **toevoeg-Blobs** in Azure Blob-opslag in uw Azure-abonnement.
>
> - Alle soorten opslag (v1, v2, blob) worden ondersteund.
> - Replicatie van alle opslagconfiguraties worden ondersteund.
> - **Premium-opslag** is momenteel **niet ondersteund**.
> - **Opslag in VNet** is momenteel **niet ondersteund**.
> - **De opslag achter een Firewall** is momenteel **niet ondersteund**

## <a id="subheading-8"></a>Definiëren op serverniveau versus controlebeleid op databaseniveau

Een controlebeleid kan worden gedefinieerd voor een specifieke database of als een standaardbeleid voor server:

- Een serverbeleid is van toepassing op alle bestaande en nieuwe databases op de server.

- Als *controle is ingeschakeld*, het *altijd van toepassing op de database*. De database wordt gecontroleerd, ongeacht de database controle-instellingen.

- Controlefunctie voor blobs in de database of het datawarehouse is ingeschakeld, naast het inschakelen van deze op de server heeft *niet* overschrijven of wijzigen van de instellingen van de controlefunctie voor de server. Beide controles wordt naast elkaar bestaan. Met andere woorden, wordt de database gecontroleerd tweemaal in parallelle; eenmaal door het serverbeleid en eenmaal door het beleid van de database.

   > [!NOTE]
   > Vermijd het inschakelen van serverblobs zowel blob Databasecontrole samen, tenzij:
    > - U wilt gebruiken een andere *opslagaccount* of *bewaarperiode* voor een specifieke database.
    > - U wilt controleren gebeurtenistypen of categorieën voor een specifieke database die van de rest van de databases op de server afwijken. Bijvoorbeeld, mogelijk hebt tabel ingevoegd die moeten worden gecontroleerd alleen voor een specifieke database.
   >
   > Anders wordt aangeraden dat u alleen op serverniveau blob-controle inschakelen en laat u de controle op databaseniveau uitgeschakeld voor alle databases.

## <a id="subheading-2"></a>Controle voor uw database instellen

Het volgende gedeelte bevat de configuratie van de controle met Azure portal.

1. Ga naar de [Azure Portal](https://portal.azure.com).
2. Navigeer naar **controle** onder de kop van de beveiliging in uw SQL database-/ server-venster.

    <a id="auditing-screenshot"></a> ![Navigatiedeelvenster][1]

3. Als u liever het instellen van een controlebeleid server, kunt u de **serverinstellingen weergeven** koppeling op de pagina voor het controleren van database. U kunt vervolgens weergeven of wijzigen van de server controle-instellingen. Server controlebeleid van toepassing op alle bestaande en nieuwe databases op deze server.

    ![Navigatievenster][2]

4. Als u liever controle op databaseniveau inschakelen, schakelt u over **controle** naar **ON**.

    Als servercontrole is ingeschakeld, wordt de database is geconfigureerd-controle side-by-side met de controle van de server bestaat.

    ![Navigatievenster][3]

5. **Nieuwe** -u hebt nu meerdere opties voor het configureren van waarnaar de auditlogboeken worden geschreven. U kunt Logboeken schrijven naar een Azure storage-account, een Log Analytics-werkruimte voor gebruik door Azure Monitor-Logboeken of naar event hub voor gebruik met behulp van event hub. U kunt een willekeurige combinatie van deze opties configureren en auditlogboeken worden geschreven naar elk.

    ![Opties voor opslag](./media/sql-database-auditing-get-started/auditing-select-destination.png)

6. Logboeken om te schrijven controle configureren naar een opslagaccount, selecteer **opslag** en open **opslaggegevens**. Selecteer het Azure storage-account waar de logboeken worden opgeslagen en selecteer vervolgens de bewaarperiode. De oude logboeken worden verwijderd. Klik vervolgens op **OK**.

    ![opslagaccount](./media/sql-database-auditing-get-started/auditing_select_storage.png)

7. Schrijven controle configureren vastgelegd in een Log Analytics-werkruimte, selecteer **Log Analytics (Preview)** en open **Log Analytics-gegevens**. Selecteer of maak de Log Analytics-werkruimte waar logboeken worden geschreven en klik vervolgens op **OK**.

    ![Log Analytics-werkruimte](./media/sql-database-auditing-get-started/auditing_select_oms.png)

8. Logboeken om te schrijven controle configureren naar een event hub, selecteer **Event Hub (Preview)** en open **details van Event Hub**. Selecteer de event hub waar logboeken worden geschreven en klik vervolgens op **OK**. Zorg ervoor dat de event hub in dezelfde regio als uw database en de server.

    ![Event Hub](./media/sql-database-auditing-get-started/auditing_select_event_hub.png)

9. Klik op **Opslaan**.
10. Als u aanpassen van de gecontroleerde gebeurtenissen wilt, u kunt dit doen via [PowerShell-cmdlets](#subheading-7) of de [REST-API](#subheading-9).
11. Nadat u de controle-instellingen hebt geconfigureerd, kunt u de nieuwe functie voor de detectie van bedreigingen inschakelen en configureren van e-mailberichten voor het ontvangen van beveiligingswaarschuwingen. Wanneer u detectie van bedreigingen, ontvangt u proactieve waarschuwingen voor afwijkende activiteiten die op potentiële beveiligingsrisico's duiden kunnen. Zie voor meer informatie, [aan de slag met detectie van bedreigingen](sql-database-threat-detection-get-started.md).


> [!IMPORTANT]
>Inschakelen van controle op een Azure SQL Data Warehouse, of op een server met een Azure SQL Data Warehouse, **zal leiden tot het datawarehouse wordt hervat**, zelfs in het geval waarin het programma eerder is onderbroken. **Zorg ervoor dat u de datawarehouse onderbreken opnieuw na het inschakelen van controle**.'


## <a id="subheading-3"></a>Analyseren van controlelogboeken en -rapporten

Als u ervoor hebt gekozen auditlogboeken schrijven naar Azure Monitor-Logboeken:

- Gebruik de [Azure-portal](https://portal.azure.com).  Open de betreffende database. Aan de bovenkant van de database **controle** pagina, klikt u op **auditlogboeken weergeven**.

    ![Auditlogboeken weergeven](./media/sql-database-auditing-get-started/7_auditing_get_started_blob_view_audit_logs.png)

- Vervolgens te klikken op **openen in OMS** aan de bovenkant van de **controlerecords** pagina te openen de weergave van de logboeken in Log Analytics, waar u het tijdsbereik en de zoekopdracht kunt aanpassen.

    ![Open in Log Analytics](./media/sql-database-auditing-get-started/auditing_open_in_oms.png)

- U kunt ook u kunt ook toegang tot de auditlogboeken van Log Analytics-blade. Open uw Log Analytics-werkruimte en klik vervolgens onder **algemene** sectie, klikt u op **logboeken**. U kunt beginnen met een eenvoudige query, zoals: *zoeken naar "SQLSecurityAuditEvents"* om weer te geven van de audit-Logboeken.
    Hier kunt u ook kunt gebruiken [logboeken van Azure Monitor](../log-analytics/log-analytics-log-search.md) geavanceerde zoekopdrachten uitvoeren op uw logboekgegevens audit. Logboeken in Azure Monitor kunt u realtime operationele inzichten met behulp van geïntegreerde Zoek- en aangepaste dashboards voor het analyseren van miljoenen records gemakkelijk in uw werkbelastingen en servers. Zie voor meer nuttige informatie over opdrachten en Azure Monitor logboeken zoektaal, [Azure Monitor-verwijzing naar de logboeken](../log-analytics/log-analytics-log-search.md).

Als u ervoor hebt gekozen auditlogboeken naar Event Hub schrijven:

- Als u wilt controleren Logboeken gegevens uit Event Hub gebruiken, moet u voor het instellen van een stroom gebeurtenissen gebruiken en te schrijven naar een doel. Zie voor meer informatie, [documentatie over Azure Event Hubs](https://docs.microsoft.com/azure/event-hubs/).
- Auditlogboeken in Event Hub worden vastgelegd in de hoofdtekst van [Apache Avro](https://avro.apache.org/) gebeurtenissen en opgeslagen met behulp van JSON opmaak met UTF-8-codering. De om controlelogboeken te lezen, kunt u [Avro-hulpprogramma's](https://docs.microsoft.com/azure/event-hubs/event-hubs-capture-overview#use-avro-tools) of een vergelijkbaar hulpprogramma's waarmee deze indeling worden verwerkt.

Als u auditlogboeken schrijven naar een Azure storage-account, zijn er verschillende methoden die u gebruiken kunt om de logboeken weer te geven:

- Auditlogboeken worden samengevoegd in het account dat u hebt gekozen tijdens de installatie. U kunt de logboeken voor controle verkennen met behulp van een hulpprogramma zoals [Azure Storage Explorer](https://storageexplorer.com/). In Azure storage controleren logboeken worden opgeslagen als een verzameling van blob-bestanden in een container met de naam **sqldbauditlogs**. Zie voor meer informatie over de hiërarchie van de map storage naamgevingsregels en logboekindeling, de [Blobverwijzing Audit Log indeling](https://go.microsoft.com/fwlink/?linkid=829599).

- Gebruik de [Azure-portal](https://portal.azure.com).  Open de betreffende database. Aan de bovenkant van de database **controle** pagina, klikt u op **auditlogboeken weergeven**.

    ![Navigatievenster][7]

    **Controlerecords** wordt geopend, waarin zal het mogelijk om de logboeken weer te geven.

  - U kunt specifieke datums weergeven door te klikken op **Filter** aan de bovenkant van de **controlerecords** pagina.
  - U kunt schakelen tussen controlerecords die zijn gemaakt door de *server controlebeleid* en de *database controlebeleid* door het omschakelen van **bron controleren**.
  - Vindt u alleen SQL-injectie controlerecords door het controleren van gerelateerde **tonen alleen controlerecords voor SQL-injecties** selectievakje.

       ![Navigatievenster][8]

- Gebruik de systeemfunctie **sys.fn_get_audit_file** (T-SQL) om te retourneren van de audit log-gegevens in tabelvorm. Zie voor meer informatie over het gebruik van deze functie [sys.fn_get_audit_file](https://docs.microsoft.com/sql/relational-databases/system-functions/sys-fn-get-audit-file-transact-sql).

- Gebruik **controlebestanden samenvoegen** in SQL Server Management Studio (te beginnen met SSMS 17):
    1. Selecteer in het menu SSMS **bestand** > **Open** > **controlebestanden samenvoegen**.

        ![Navigatievenster][9]
    2. De **controlebestanden toevoegen** in het dialoogvenster wordt geopend. Selecteer een van de **toevoegen** opties te kiezen of u wilt samenvoegen auditbestanden van een lokale schijf of ze importeren uit Azure Storage. U moet uw Azure Storage-gegevens en de accountsleutel.

    3. Nadat alle bestanden samenvoegen zijn toegevoegd, klikt u op **OK** om de samenvoegbewerking te voltooien.

    4. Het samengevoegde bestand wordt geopend in SSMS, waar u kunt weergeven en analyseren, evenals het exporteren naar een xel-bestand of een CSV-bestand, of naar een tabel.

- Power BI gebruiken. U kunt bekijken en analyseren van logboekgegevens audit in Power BI. Zie voor meer informatie en voor toegang tot een sjabloon downloaden [analyseren audit log-gegevens in Power BI](https://blogs.msdn.microsoft.com/azuresqldbsupport/20../../sql-azure-blob-auditing-basic-power-bi-dashboard/).
- Downloaden van de logboekbestanden van uw Azure Storage blob-container via de portal of met behulp van een hulpprogramma zoals [Azure Storage Explorer](https://storageexplorer.com/).
  - Nadat u een logboekbestand lokaal hebt gedownload, dubbelklikt u op het bestand te openen, weergeven en analyseren van de logboeken in SSMS.
  - U kunt ook meerdere bestanden tegelijkertijd via Azure Storage Explorer downloaden. Om dit te doen met de rechtermuisknop op een specifieke submap en selecteer **opslaan als** om op te slaan in een lokale map.

- Aanvullende methoden:

  - Nadat u hebt meerdere bestanden of een submap met de logboekbestanden gedownload, kunt u deze lokaal zoals beschreven in de controlebestanden SSMS-samenvoegen-instructies die eerder zijn beschreven samenvoegen.
  - Controlefunctie voor blobs weergeven logboeken via een programma:

    - [Uitgebreide gebeurtenissen querybestanden](https://sqlscope.wordpress.com/20../../reading-extended-event-files-using-client-side-tools-only/) met behulp van PowerShell.

## <a id="subheading-5"></a>Procedures voor productie

<!--The description in this section refers to preceding screen captures.-->

### <a id="subheading-6">Controle van databases geo-replicatie</a>

Met geo-replicatie databases, wanneer u de controle op de primaire database inschakelen heeft de secundaire database een identieke controlebeleid. Het is ook mogelijk om in te stellen de controle op de secundaire database door het inschakelen van controle op de **secundaire server**, onafhankelijk van de primaire database.

- Op serverniveau (**aanbevolen**): Schakel controle op zowel de **primaire server** , evenals de **secundaire server** -de primaire en secundaire databases wordt elk gecontroleerd afzonderlijk op basis van hun respectieve op serverniveau-beleid.
- Database-level: Controle op databaseniveau voor secundaire databases kan alleen worden geconfigureerd in de primaire database controle-instellingen.
  - Controle moet zijn ingeschakeld op de *primaire database zelf*, niet op de server.
  - Nadat de controle is ingeschakeld op de primaire database, wordt deze ook worden ingeschakeld op de secundaire database.

    >[!IMPORTANT]
    >Met de controle op databaseniveau is de opslaginstellingen voor de secundaire database identiek aan die van de primaire database, waardoor regio-overschrijdend-verkeer. U wordt aangeraden dat u alleen op serverniveau controle inschakelen en laat u de controle op databaseniveau uitgeschakeld voor alle databases.
    > [!WARNING]
    > Met behulp van event hub of Azure Monitor-Logboeken als doelen voor de logboeken voor controle op serverniveau is momenteel niet ondersteund voor secundaire geo-replicatie-databases.

### <a id="subheading-6">Toegangssleutel voor opslag</a>

In de productieomgeving bent u waarschijnlijk uw opslagsleutels periodiek te vernieuwen. Bij het schrijven van de logboeken voor controle met Azure storage, moet u het beveiligingsbeleid opnieuw op te slaan bij het vernieuwen van uw sleutels. Het proces is als volgt:

1. Open **opslaggegevens**. In de **toegangssleutel voor opslag** Schakel **secundaire**, en klikt u op **OK**. Klik vervolgens op **opslaan** aan de bovenkant van de configuratiepagina van de controle.

    ![Navigatievenster][5]
2. Ga naar de configuratiepagina van de opslag en de primaire toegangssleutel opnieuw genereren.

    ![Navigatievenster][6]
3. Ga terug naar de configuratiepagina van controle, schakelen de toegangssleutel voor opslag van de secundaire naar primaire en klik vervolgens op **OK**. Klik vervolgens op **opslaan** aan de bovenkant van de configuratiepagina van de controle.
4. Ga terug naar de configuratiepagina van de opslag en genereer de secundaire toegangssleutel (ter voorbereiding op de volgende sleutel vernieuwingscyclus) opnieuw.

## <a name="additional-information"></a>Aanvullende informatie

- Voor meer informatie over het logboek-indeling, hiërarchie van de map storage en naamconventies, Zie de [Blobverwijzing Audit Log indeling](https://go.microsoft.com/fwlink/?linkid=829599).

    > [!IMPORTANT]
    > Azure SQL Database controleren worden 4000 tekens van de gegevens voor teken velden in een controlerecord opgeslagen. Wanneer de **instructie** of de **data_sensitivity_information** waarden geretourneerd door een controleerbare actie meer dan 4000 tekens bevatten, kunnen geen gegevens buiten de eerste 4000 tekens,  **afgekapt en niet gecontroleerd**.

- Auditlogboeken worden geschreven naar **toevoeg-Blobs** in een Azure Blob-opslag in uw Azure-abonnement:
  - **Premium Storage** is momenteel **niet ondersteund** door toevoeg-Blobs.
  - **Opslag in VNet** is momenteel **niet ondersteund**.

- Het standaardbeleid voor controle bevat alle acties en de volgende set met actiegroepen die audit uit op alle query's en opgeslagen procedures die worden uitgevoerd op de database, evenals de geslaagde en mislukte aanmeldingen:

    BATCH_COMPLETED_GROUP<br>
    SUCCESSFUL_DATABASE_AUTHENTICATION_GROUP<br>
    FAILED_DATABASE_AUTHENTICATION_GROUP

    U kunt configureren voor verschillende soorten acties en actiegroepen met behulp van PowerShell, controle, zoals beschreven in de [beheren SQL database auditing met behulp van Azure PowerShell](#subheading-7) sectie.

- Wanneer u AAD-verificatie gebruikt, kan de aanmeldingen records wordt niet *niet* weergegeven in de SQL-auditlogboek. Mislukte aanmeldingen AuditRecords wilt weergeven, moet u gaat u naar de [Azure Active Directory-portal]( ../active-directory/reports-monitoring/reference-sign-ins-error-codes.md), waarvan de details van deze gebeurtenissen zich aanmeldt.


## <a id="subheading-7"></a>SQL database auditing met behulp van Azure PowerShell beheren

**PowerShell-cmdlets (met inbegrip van WHERE-component ondersteuning voor het filteren van aanvullende)**:

- [Maken of bijwerken van de Database controlebeleid (Set-AzSqlDatabaseAuditing)](https://docs.microsoft.com/powershell/module/az.sql/set-azsqldatabaseauditing)
- [Maken of bijwerken van controlebeleid van Server (Set-AzSqlServerAuditing)](https://docs.microsoft.com/powershell/module/az.sql/set-azsqlserverauditing)
- [Database controlebeleid ophalen (Get-AzSqlDatabaseAuditing)](https://docs.microsoft.com/powershell/module/az.sql/get-azsqldatabaseauditing)
- [Get Server Auditing Policy (Get-AzSqlServerAuditing)](https://docs.microsoft.com/powershell/module/az.sql/get-azsqlserverauditing)

Zie voor een voorbeeldscript [controle en detectie van bedreigingen met behulp van PowerShell configureren](scripts/sql-database-auditing-and-threat-detection-powershell.md).

## <a id="subheading-9"></a>SQL database auditing met REST API's beheren

**REST API**:

- [Maken of bijwerken van de Database controlebeleid](https://docs.microsoft.com/rest/api/sql/database%20auditing%20settings/createorupdate)
- [Maken of bijwerken van de Server controlebeleid](https://docs.microsoft.com/rest/api/sql/server%20auditing%20settings/createorupdate)
- [Controlebeleid Database ophalen](https://docs.microsoft.com/rest/api/sql/database%20auditing%20settings/get)
- [Van controlebeleid van Server ophalen](https://docs.microsoft.com/rest/api/sql/server%20auditing%20settings/get)

Uitgebreide beleid met waar component ondersteuning voor aanvullende filters:

- [Maken of bijwerken van de Database *uitgebreid* controlebeleid](https://docs.microsoft.com/rest/api/sql/database%20extended%20auditing%20settings/createorupdate)
- [Maken of bijwerken van de Server *uitgebreid* controlebeleid](https://docs.microsoft.com/rest/api/sql/server%20auditing%20settings/createorupdate)
- [Database ophalen *uitgebreid* controlebeleid](https://docs.microsoft.com/rest/api/sql/database%20extended%20auditing%20settings/get)
- [Ophalen van Server *uitgebreid* controlebeleid](https://docs.microsoft.com/rest/api/sql/server%20auditing%20settings/get)

## <a id="subheading-10"></a>SQL database auditing met behulp van ARM-sjablonen beheren

U kunt beheren met Azure SQL database auditing met behulp van [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) sjablonen, zoals wordt weergegeven in deze voorbeelden:

- [Een Azure SQL-Server implementeren met controle ingeschakeld auditlogboeken schrijven naar Azure Blob storage-account](https://github.com/Azure/azure-quickstart-templates/tree/master/201-sql-auditing-server-policy-to-blob-storage)
- [Een Azure SQL-Server met controle ingeschakeld auditlogboeken schrijven naar Log Analytics implementeren](https://github.com/Azure/azure-quickstart-templates/tree/master/201-sql-auditing-server-policy-to-oms)
- [Een Azure SQL-Server implementeren met controle ingeschakeld auditlogboeken schrijven naar Event Hubs](https://github.com/Azure/azure-quickstart-templates/tree/master/201-sql-auditing-server-policy-to-eventhub)

> [!NOTE]
> De gekoppelde voorbeelden bevinden zich op een externe openbare opslagplaats en worden geleverd 'as is' zonder garantie, en worden niet ondersteund onder een Microsoft-ondersteuning programma/service.

<!--Anchors-->
[Azure SQL Database Auditing overview]: #subheading-1
[Set up auditing for your database]: #subheading-2
[Analyze audit logs and reports]: #subheading-3
[Practices for usage in production]: #subheading-5
[Storage Key Regeneration]: #subheading-6
[Manage SQL database auditing using Azure PowerShell]: #subheading-7
[Blob/Table differences in Server auditing policy inheritance]: (#subheading-8)
[Manage SQL database auditing using REST API]: #subheading-9
[Manage SQL database auditing using ARM templates]: #subheading-10

<!--Image references-->
[1]: ./media/sql-database-auditing-get-started/1_auditing_get_started_settings.png
[2]: ./media/sql-database-auditing-get-started/2_auditing_get_started_server_inherit.png
[3]: ./media/sql-database-auditing-get-started/3_auditing_get_started_turn_on.png
[4]: ./media/sql-database-auditing-get-started/4_auditing_get_started_storage_details.png
[5]: ./media/sql-database-auditing-get-started/5_auditing_get_started_storage_key_regeneration.png
[6]: ./media/sql-database-auditing-get-started/6_auditing_get_started_regenerate_key.png
[7]: ./media/sql-database-auditing-get-started/7_auditing_get_started_blob_view_audit_logs.png
[8]: ./media/sql-database-auditing-get-started/8_auditing_get_started_blob_audit_records.png
[9]: ./media/sql-database-auditing-get-started/9_auditing_get_started_ssms_1.png
[10]: ./media/sql-database-auditing-get-started/10_auditing_get_started_ssms_2.png