---
title: Aan de slag met Azure SQL database auditing | Microsoft Docs
description: Gebruik Azure SQL database auditing voor het bijhouden van databasegebeurtenissen in een logboek.
services: sql-database
author: giladmit
manager: craigg
ms.service: sql-database
ms.custom: security
ms.topic: conceptual
ms.date: 09/10/2018
ms.author: giladm
ms.reviewer: vanto
ms.openlocfilehash: 935baf791d9244f2fa4f5be9c02d4778244754de
ms.sourcegitcommit: f983187566d165bc8540fdec5650edcc51a6350a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/13/2018
ms.locfileid: "45543743"
---
# <a name="get-started-with-sql-database-auditing"></a>Aan de slag met SQL Database Auditing
Met Azure SQL database auditing houdt databasegebeurtenissen en geschreven naar een auditlogboek in uw Azure storage-account. Ook controle:

* Helpt u naleving van regelgeving, inzicht in de databaseactiviteiten en inzicht krijgen in discrepanties en afwijkingen die kunnen wijzen op problemen voor het bedrijf of vermoedelijke beveiligingsschendingen.

* Hiermee wordt en vergemakkelijkt de naleving van standaarden voor compliance, hoewel het nalevingsbeleid geen garantie. Zie voor meer informatie over Azure-programma's die ondersteuning voor standaarden naleving, de [Azure Trust Center](https://azure.microsoft.com/support/trust-center/compliance/).


## <a id="subheading-1"></a>Azure SQL database auditing-overzicht
U kunt gebruiken om SQL database auditing aan:


* **Behouden** een audittrail van de geselecteerde gebeurtenissen. Categorieën van de databaseacties moeten worden gecontroleerd, kunt u definiëren.
* **Rapport** op database-activiteit. U kunt vooraf geconfigureerde rapporten en een dashboard snel aan de slag met de activiteit en rapportage.
* **Analyseren** rapporten. U kunt verdachte gebeurtenissen, ongebruikelijke activiteiten en trends vinden.

Configureren voor verschillende soorten gebeurteniscategorieën, controle, zoals wordt beschreven de [controle voor uw database instellen](#subheading-2) sectie.

> [!IMPORTANT]
> Auditlogboeken worden geschreven naar **toevoeg-Blobs** in een Azure Blob-opslag in uw Azure-abonnement.
>
> * **Premium Storage** is momenteel **niet ondersteund** door toevoeg-Blobs.
> * **Opslag in VNet** is momenteel **niet ondersteund**.

## <a id="subheading-8"></a>Definiëren op serverniveau versus controlebeleid op databaseniveau

Een controlebeleid kan worden gedefinieerd voor een specifieke database of als een standaardbeleid voor server:

* Een serverbeleid is van toepassing op alle bestaande en nieuwe databases op de server.

* Als *controle is ingeschakeld*, het *altijd van toepassing op de database*. De database wordt gecontroleerd, ongeacht de database controle-instellingen.

* Controlefunctie voor blobs in de database is ingeschakeld, naast het inschakelen van deze op de server heeft *niet* overschrijven of wijzigen van de instellingen van de controlefunctie voor de server. Beide controles wordt naast elkaar bestaan. Met andere woorden, wordt de database gecontroleerd tweemaal in parallelle; eenmaal door het serverbeleid en eenmaal door het beleid van de database.

   > [!NOTE]
   > Vermijd het inschakelen van serverblobs zowel blob Databasecontrole samen, tenzij:
    > * U wilt gebruiken een andere *opslagaccount* of *bewaarperiode* voor een specifieke database.
    > * U wilt controleren gebeurtenistypen of categorieën voor een specifieke database die van de rest van de databases op de server afwijken. Bijvoorbeeld, mogelijk hebt tabel ingevoegd die moeten worden gecontroleerd alleen voor een specifieke database.
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

5. **Nieuwe** -u hebt nu meerdere opties voor het configureren van waarnaar de auditlogboeken worden geschreven. U kunt Logboeken schrijven naar een Azure storage-account, aan een OMS-werkruimte voor gebruik door Log Analytics of naar event hub voor gebruik met behulp van event hub. U kunt een willekeurige combinatie van deze opties configureren en auditlogboeken worden geschreven naar elk.

    ![Opties voor opslag](./media/sql-database-auditing-get-started/auditing-select-destination.png)

6. Logboeken om te schrijven controle configureren naar een opslagaccount, selecteer **opslag** en open **opslaggegevens**. Selecteer het Azure storage-account waar de logboeken worden opgeslagen en selecteer vervolgens de bewaarperiode. De oude logboeken worden verwijderd. Klik vervolgens op **OK**.

    ![opslagaccount](./media/sql-database-auditing-get-started/auditing_select_storage.png)

7. Om te schrijven controle configureren meldt u zich bij een OMS-werkruimte en selecteer **Log Analytics (Preview)** en open **Log Analytics-gegevens**. Selecteer of maak de OMS-werkruimte waar logboeken worden geschreven en klik vervolgens op **OK**.

    ![OMS](./media/sql-database-auditing-get-started/auditing_select_oms.png)

8. Logboeken om te schrijven controle configureren naar een event hub, selecteer **Event Hub (Preview)** en open **details van Event Hub**. Selecteer de event hub waar logboeken worden geschreven en klik vervolgens op **OK**. Zorg ervoor dat de event hub in dezelfde regio als uw database en de server.

    ![Event Hub](./media/sql-database-auditing-get-started/auditing_select_event_hub.png)

9. Klik op **Opslaan**.
10. Als u aanpassen van de gecontroleerde gebeurtenissen wilt, u kunt dit doen via [PowerShell-cmdlets](#subheading-7) of de [REST-API](#subheading-9).
11. Nadat u de controle-instellingen hebt geconfigureerd, kunt u de nieuwe functie voor de detectie van bedreigingen inschakelen en configureren van e-mailberichten voor het ontvangen van beveiligingswaarschuwingen. Wanneer u detectie van bedreigingen, ontvangt u proactieve waarschuwingen voor afwijkende activiteiten die op potentiële beveiligingsrisico's duiden kunnen. Zie voor meer informatie, [aan de slag met detectie van bedreigingen](sql-database-threat-detection-get-started.md). 

## <a id="subheading-3"></a>Analyseren van controlelogboeken en -rapporten
Als u auditlogboeken schrijven naar een Azure storage-account, zijn er verschillende methoden die u gebruiken kunt om de logboeken weer te geven:
- Auditlogboeken worden samengevoegd in het account dat u hebt gekozen tijdens de installatie. U kunt de logboeken voor controle verkennen met behulp van een hulpprogramma zoals [Azure Storage Explorer](http://storageexplorer.com/). In Azure storage controleren logboeken worden opgeslagen als een verzameling van blob-bestanden in een container met de naam **sqldbauditlogs**. Zie voor meer informatie over de hiërarchie van de map storage naamgevingsregels en logboekindeling, de [Blobverwijzing Audit Log indeling](https://go.microsoft.com/fwlink/?linkid=829599).

- Gebruik de [Azure-portal](https://portal.azure.com).  Open de betreffende database. Aan de bovenkant van de database **controle en detectie van bedreigingen** pagina, klikt u op **auditlogboeken weergeven**.

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

- Power BI gebruiken. U kunt bekijken en analyseren van logboekgegevens audit in Power BI. Zie voor meer informatie en voor toegang tot een sjabloon downloaden [Analyzie audit log-gegevens in Power BI](https://blogs.msdn.microsoft.com/azuresqldbsupport/2017/05/26/sql-azure-blob-auditing-basic-power-bi-dashboard/).
- Downloaden van de logboekbestanden van uw Azure Storage blob-container via de portal of met behulp van een hulpprogramma zoals [Azure Storage Explorer](http://storageexplorer.com/).
    * Nadat u een logboekbestand lokaal hebt gedownload, dubbelklikt u op het bestand te openen, weergeven en analyseren van de logboeken in SSMS.
    * U kunt ook meerdere bestanden tegelijkertijd via Azure Storage Explorer downloaden. Om dit te doen met de rechtermuisknop op een specifieke submap en selecteer **opslaan als** om op te slaan in een lokale map.

* Aanvullende methoden:
   * Nadat u hebt meerdere bestanden of een submap met de logboekbestanden gedownload, kunt u deze lokaal zoals beschreven in de controlebestanden SSMS-samenvoegen-instructies die eerder zijn beschreven samenvoegen.
   * Controlefunctie voor blobs weergeven logboeken via een programma:

     * Gebruik de [uitgebreide gebeurtenissen-lezer](https://blogs.msdn.microsoft.com/extended_events/2011/07/20/introducing-the-extended-events-reader/) C#-bibliotheek.
     * [Uitgebreide gebeurtenissen querybestanden](https://sqlscope.wordpress.com/2014/11/15/reading-extended-event-files-using-client-side-tools-only/) met behulp van PowerShell.

Als u ervoor hebt gekozen auditlogboeken schrijven naar Log Analytics:
- Voor auditlogboeken in Log Analytics, opent u uw Log Analytics-werkruimte en klik vervolgens onder **zoeken in Logboeken en analyseren**, klikt u op **logboeken bekijken**. In de weergave zoeken in Logboeken, kunt u beginnen door te klikken op **alle verzamelde gegevens**.  

    ![OMS voor zoeken in Logboeken](./media/sql-database-auditing-get-started/oms_log_search.png)

   Hier kunt u [Operations Management Suite (OMS) Log Analytics](../log-analytics/log-analytics-log-search.md) geavanceerde zoekopdrachten uitvoeren op uw logboekgegevens audit. Log Analytics biedt u realtime operationele inzichten met behulp van geïntegreerde Zoek- en aangepaste dashboards voor het analyseren van gemakkelijk miljoenen records voor al uw workloads en servers. Zie voor meer nuttige informatie over opdrachten en OMS Log Analytics-zoektaal, [Log Analytics zoeken verwijzing](../log-analytics/log-analytics-log-search.md).

Als u ervoor hebt gekozen auditlogboeken naar Event Hub schrijven:
- Als u wilt controleren Logboeken gegevens uit Event Hub gebruiken, moet u voor het instellen van een stroom gebeurtenissen gebruiken en te schrijven naar een doel. Zie voor meer informatie, [documentatie over Azure Event Hubs](https://docs.microsoft.com/azure/event-hubs/).

## <a id="subheading-5"></a>Procedures voor productie
<!--The description in this section refers to preceding screen captures.-->

### <a id="subheading-6">Controle van databases geo-replicatie</a>
Met geo-replicatie databases, wanneer u de controle op de primaire database inschakelen heeft de secundaire database een identieke controlebeleid. Het is ook mogelijk om in te stellen de controle op de secundaire database door het inschakelen van controle op de **secundaire server**, onafhankelijk van de primaire database.

* Op serverniveau (**aanbevolen**): Schakel controle op zowel de **primaire server** , evenals de **secundaire server** -de primaire en secundaire databases wordt elk worden gecontroleerd onafhankelijk van elkaar op basis van hun respectieve op serverniveau beleid.

* Op databaseniveau: Op databaseniveau voor secundaire databases controle kan alleen worden geconfigureerd in de primaire database controle-instellingen.
   * Controle moet zijn ingeschakeld op de *primaire database zelf*, niet op de server.
   * Nadat de controle is ingeschakeld op de primaire database, wordt deze ook worden ingeschakeld op de secundaire database.

    >[!IMPORTANT]
    >Met de controle op databaseniveau is de opslaginstellingen voor de secundaire database identiek aan die van de primaire database, waardoor regio-overschrijdend-verkeer. U wordt aangeraden dat u alleen op serverniveau controle inschakelen en laat u de controle op databaseniveau uitgeschakeld voor alle databases.
<br>

### <a id="subheading-6">Toegangssleutel voor opslag</a>
In de productieomgeving bent u waarschijnlijk uw opslagsleutels periodiek te vernieuwen. Bij het schrijven van de logboeken voor controle met Azure storage, moet u het beveiligingsbeleid opnieuw op te slaan bij het vernieuwen van uw sleutels. Het proces is als volgt:

1. Open **opslaggegevens**. In de **toegangssleutel voor opslag** Schakel **secundaire**, en klikt u op **OK**. Klik vervolgens op **opslaan** aan de bovenkant van de configuratiepagina van de controle.

    ![Navigatievenster][5]
2. Ga naar de configuratiepagina van de opslag en de primaire toegangssleutel opnieuw genereren.

    ![Navigatievenster][6]
3. Ga terug naar de configuratiepagina van controle, schakelen de toegangssleutel voor opslag van de secundaire naar primaire en klik vervolgens op **OK**. Klik vervolgens op **opslaan** aan de bovenkant van de configuratiepagina van de controle.
4. Ga terug naar de configuratiepagina van de opslag en genereer de secundaire toegangssleutel (ter voorbereiding op de volgende sleutel vernieuwingscyclus) opnieuw.

## <a name="additional-information"></a>Aanvullende gegevens

* Voor meer informatie over het logboek-indeling, hiërarchie van de map storage en naamconventies, Zie de [Blobverwijzing Audit Log indeling](https://go.microsoft.com/fwlink/?linkid=829599).

    > [!IMPORTANT]
    > Azure SQL Database controleren worden 4000 tekens van de gegevens voor teken velden in een controlerecord opgeslagen. Wanneer de **instructie** of de **data_sensitivity_information** waarden geretourneerd door een controleerbare actie meer dan 4000 tekens bevatten, kunnen geen gegevens buiten de eerste 4000 tekens,  **afgekapt en niet gecontroleerd**.

* Auditlogboeken worden geschreven naar **toevoeg-Blobs** in een Azure Blob-opslag in uw Azure-abonnement:
    * **Premium Storage** is momenteel **niet ondersteund** door toevoeg-Blobs.
    * **Opslag in VNet** is momenteel **niet ondersteund**.

* Het standaardbeleid voor controle bevat alle acties en de volgende set met actiegroepen die audit uit op alle query's en opgeslagen procedures die worden uitgevoerd op de database, evenals de geslaagde en mislukte aanmeldingen:

    BATCH_COMPLETED_GROUP<br>
    SUCCESSFUL_DATABASE_AUTHENTICATION_GROUP<br>
    FAILED_DATABASE_AUTHENTICATION_GROUP

    U kunt configureren voor verschillende soorten acties en actiegroepen met behulp van PowerShell, controle, zoals beschreven in de [beheren SQL database auditing met behulp van Azure PowerShell](#subheading-7) sectie.

## <a id="subheading-7"></a>SQL database auditing met behulp van Azure PowerShell beheren

**PowerShell-cmdlets**:

* [Maken of bijwerken van de Database Blob controlebeleid (Set-AzureRMSqlDatabaseAuditing)][105]
* [Maken of bijwerken van de Server Blob controlebeleid (Set-AzureRMSqlServerAuditing)][106]
* [Database controlebeleid ophalen (Get-AzureRMSqlDatabaseAuditing)][101]
* [Server Blob controlebeleid ophalen (Get-AzureRMSqlServerAuditing)][102]

Zie voor een voorbeeldscript [controle en detectie van bedreigingen met behulp van PowerShell configureren](scripts/sql-database-auditing-and-threat-detection-powershell.md).

## <a id="subheading-9"></a>SQL database auditing met REST API's beheren

**REST-API - controlefunctie voor blobs**:

* [Maken of bijwerken van de Database Blob controlebeleid](https://docs.microsoft.com/en-us/rest/api/sql/database%20auditing%20settings/createorupdate)
* [Maken of bijwerken van de Server Blob controlebeleid](https://docs.microsoft.com/en-us/rest/api/sql/server%20auditing%20settings/createorupdate)
* [Database-Blob controlebeleid ophalen](https://docs.microsoft.com/en-us/rest/api/sql/database%20auditing%20settings/get)
* [Ophalen van Server Blob controlebeleid](https://docs.microsoft.com/en-us/rest/api/sql/server%20auditing%20settings/get)

Uitgebreide beleid met waar component ondersteuning voor aanvullende filters:
* [Maken of bijwerken van de Database *uitgebreid* controlebeleid voor Blob](https://docs.microsoft.com/en-us/rest/api/sql/database%20extended%20auditing%20settings/createorupdate)
* [Maken of bijwerken van de Server *uitgebreid* controlebeleid voor Blob](https://docs.microsoft.com/en-us/rest/api/sql/server%20extended%20auditing%20settings/createorupdate)
* [Database ophalen *uitgebreid* controlebeleid voor Blob](https://docs.microsoft.com/en-us/rest/api/sql/database%20extended%20auditing%20settings/get)
* [Ophalen van Server *uitgebreid* controlebeleid voor Blob](https://docs.microsoft.com/en-us/rest/api/sql/server%20extended%20auditing%20settings/get)

<!--Anchors-->
[Azure SQL Database Auditing overview]: #subheading-1
[Set up auditing for your database]: #subheading-2
[Analyze audit logs and reports]: #subheading-3
[Practices for usage in production]: #subheading-5
[Storage Key Regeneration]: #subheading-6
[Manage SQL database auditing using Azure PowerShell]: #subheading-7
[Blob/Table differences in Server auditing policy inheritance]: (#subheading-8)
[Manage SQL database auditing using REST API]: #subheading-9

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

[101]: /powershell/module/azurerm.sql/get-azurermsqldatabaseauditing
[102]: /powershell/module/azurerm.sql/Get-AzureRMSqlServerAuditing
[103]: /powershell/module/azurerm.sql/Remove-AzureRMSqlDatabaseAuditing
[104]: /powershell/module/azurerm.sql/Remove-AzureRMSqlServerAuditing
[105]: /powershell/module/azurerm.sql/Set-AzureRMSqlDatabaseAuditing
[106]: /powershell/module/azurerm.sql/Set-AzureRMSqlServerAuditing
