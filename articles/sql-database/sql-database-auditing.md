---
title: Aan de slag met Azure SQL database auditing | Microsoft Docs
description: Met Azure SQL database auditing kunt bijhouden van databasegebeurtenissen naar een controlelogboek.
services: sql-database
documentationcenter: 
author: giladm
manager: jhubbard
editor: giladm
ms.assetid: 89c2a155-c2fb-4b67-bc19-9b4e03c6d3bc
ms.service: sql-database
ms.custom: security
ms.workload: On Demand
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/07/2017
ms.author: giladm
ms.openlocfilehash: c97a9d96dbe6d9bc9eaa189384acad7579365e82
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2017
---
# <a name="get-started-with-sql-database-auditing"></a>Aan de slag met SQL Database Auditing
Azure SQL database auditing houdt databasegebeurtenissen en schrijft die deze naar een auditlogboek Meld u bij uw Azure storage-account. Ook controleren:

* Helpt u naleving van regelgeving onderhouden, de activiteit in een database begrijpen en meer inzicht krijgen in discrepanties en afwijkingen die kunnen wijzen op problemen voor het bedrijf of vermoedelijke beveiligingsschendingen.

* Hiermee wordt en vereenvoudigt voldoen aan de nalevingsstandaards, hoewel het geen garantie naleving. Zie voor meer informatie over Azure programma's die nalevingsscan standaarden, de [Azure Vertrouwenscentrum](https://azure.microsoft.com/support/trust-center/compliance/).


## <a id="subheading-1"></a>Azure SQL-database controle-overzicht
U kunt gebruiken om SQL-database in op:


* **Behouden** een audittrail van de geselecteerde gebeurtenissen. Categorieën van databaseacties moeten worden gecontroleerd, kunt u definiëren.
* **Rapport** op database-activiteit. U kunt vooraf geconfigureerde rapporten en een dashboard snel aan de slag met de activiteit en rapportage.
* **Analyseren** rapporten. U kunt verdachte gebeurtenissen, ongebruikelijke activiteiten en trends vinden.

Configureer controle voor verschillende soorten gebeurteniscategorieën, zoals wordt beschreven in de [controle instelt voor uw database](#subheading-2) sectie.

Controlelogboeken worden geschreven naar Azure Blob-opslag op uw Azure-abonnement.


## <a id="subheading-8"></a>Serverniveau versus databaseniveau controlebeleid definiëren

Een controlebeleid kan worden gedefinieerd voor een specifieke database of als een standaardbeleid voor server:

* Een serverbeleid geldt voor alle bestaande en nieuwe databases op de server.

* Als *server blob-controle is ingeschakeld*, het *altijd van toepassing is op de database*. De database worden gecontroleerd, ongeacht de database controle-instellingen.

* Blob controle op de database is ingeschakeld, naast het inschakelen van deze op de server heeft *niet* overschrijven of wijzigen van de instellingen van de server auditingfunctie voor blobs. Beide audits, naast elkaar bestaan. Met andere woorden, wordt de database gecontroleerd tweemaal in parallel; eenmaal door de serverbeleid en de tweede maal door het beleid van de database.

   > [!NOTE]
   > Vermijd het inschakelen van de server blob controle en het blob Databasecontrole samen tenzij:
    > * U wilt gebruiken een andere *opslagaccount* of *bewaarperiode* voor een specifieke database.
    > * U wilt controleren gebeurtenistypen of categorieën voor een specifieke database die van de rest van de databases op de server afwijken. Bijvoorbeeld wellicht tabel invoegen die moeten worden gecontroleerd alleen voor een specifieke database.
   >
   > Anders, raden wij u alleen serverniveau blob controle in te schakelen en laat de databaseniveau controle uitgeschakeld voor alle databases.


## <a id="subheading-2"></a>Controle voor uw database instellen
De volgende sectie beschrijft de configuratie van controlebeleid met de Azure portal.

1. Ga naar de [Azure Portal](https://portal.azure.com).
2. Ga naar de **instellingen** blade van de SQL database/SQL-server die u wilt controleren. In de **instellingen** blade Selecteer **controle en detectie van bedreigingen**.

    <a id="auditing-screenshot"></a>![Navigatiedeelvenster][1]
3. Als u liever een controlebeleid server instellen, kunt u de **serverinstellingen weergeven** koppeling in de databaseblade voor controle. U kunt vervolgens weergeven of wijzigen van de server controle-instellingen. Controlebeleid van de server van toepassing op alle bestaande en nieuwe databases op deze server.

    ![Navigatiedeelvenster][2]
4. Als u liever blob controle inschakelen op het databaseniveau van de voor **controle**, selecteer **ON**, en voor **type controle**, selecteer **Blob**.

    Als server auditingfunctie voor blobs is ingeschakeld, wordt de controle van de database geconfigureerd bestaan naast de blob-controle-server.

    ![Navigatiedeelvenster][3]
5. Openen van de **Audit logboeken opslag** blade, selecteer **opslaggroep**. Selecteer de Azure-opslagaccount waarin de logboeken worden opgeslagen en selecteer vervolgens de bewaarperiode. De oude logboeken worden verwijderd. Klik vervolgens op **OK**.
   >[!TIP]
   >Als u de meest buiten de controle rapporten sjablonen, gebruikt u hetzelfde opslagaccount voor alle gecontroleerde databases.

    <a id="storage-screenshot"></a>![Navigatiedeelvenster][4]
6. Als u aanpassen van de gecontroleerde gebeurtenissen wilt, kunt u dit doen via PowerShell of de REST-API. 
7. Nadat u de controle-instellingen hebt geconfigureerd, kunt u de mogelijkheid voor het detecteren van nieuwe threat inschakelen en configureren van e-mailberichten voor het ontvangen van beveiligingsberichten. Wanneer u detectie van dreigingen gebruikt, ontvangt u proactieve waarschuwingen voor afwijkende databaseactiviteiten die kunnen duiden op beveiligingsdreigingen. Zie voor meer informatie [aan de slag met detectie van dreigingen](sql-database-threat-detection-get-started.md).
8. Klik op **Opslaan**.





## <a id="subheading-3"></a>Controlelogboeken en -rapporten analyseren
Controlelogboeken worden samengevoegd in de Azure storage-account dat u hebt gekozen tijdens de installatie. U kunt de controlelogboeken verkennen met een hulpprogramma zoals [Azure Opslagverkenner](http://storageexplorer.com/).

BLOB controle logboeken worden opgeslagen als een verzameling van blob-bestanden in een container met de naam **sqldbauditlogs**.

Zie voor meer informatie over de hiërarchie van de opslagmap naamconventies en logboekbestandsindeling de [Blobverwijzing Audit Log indeling](https://go.microsoft.com/fwlink/?linkid=829599).

Er zijn verschillende methoden die u gebruiken kunt om blob controlelogboeken weer te geven:

* Gebruik de [Azure-portal](https://portal.azure.com).  Open de betreffende database. Aan de bovenkant van de database **controle en detectie van bedreigingen** blade, klikt u op **weergave controlelogboeken**.

    ![Navigatiedeelvenster][7]

    Een **controleren records** blade wordt geopend, waarin u zult kunnen de logboeken weergeven.

    - U kunt specifieke datums weergeven door te klikken op **Filter** boven aan de **controleren records** blade.
    - U kunt schakelen tussen controlerecords die zijn gemaakt met een server beleid of database beleid audit.

       ![Navigatiedeelvenster][8]

* Gebruik de systeemfunctie **sys.fn_get_audit_file** (T-SQL) te retourneren van de logboekgegevens audit in tabelvorm. Zie voor meer informatie over het gebruik van deze functie de [sys.fn_get_audit_file documentatie](https://docs.microsoft.com/sql/relational-databases/system-functions/sys-fn-get-audit-file-transact-sql).


* Gebruik **controlebestanden samenvoegen** in SQL Server Management Studio (SSMS 17 vanaf):
    1. Selecteer in het menu SSMS **bestand** > **Open** > **controlebestanden samenvoegen**.

        ![Navigatiedeelvenster][9]
    2. De **controlebestanden toevoegen** dialoogvenster wordt geopend. Selecteer een van de **toevoegen** opties te kiezen of auditbestanden van een lokale schijf samen, of ze importeren uit Azure Storage. U moet uw Azure Storage details en de accountsleutel opgeven.

    3. Nadat alle bestanden samenvoegen zijn toegevoegd, klikt u op **OK** de samenvoegen uit te voeren.

    4. Het samengevoegde bestand wordt geopend in SSMS, waar u kunt weergeven en analyseren, evenals exporteren naar een xel-bestand of de CSV-bestand of een tabel.

* Gebruik de [toepassing synchroniseren](https://github.com/Microsoft/Azure-SQL-DB-auditing-OMS-integration) die we hebben gemaakt. Het wordt uitgevoerd in Azure en maakt gebruik van Operations Management Suite (OMS) logboekanalyse openbare API's voor de push-SQL-controlelogboeken in OMS. De synchronisatie-toepassing duwt SQL controlelogboeken in OMS Log Analytics voor consumptie via het dashboard OMS-logboekanalyse.

* Power BI gebruiken. U kunt weergeven en analyseren van gegevens van de audit log in Power BI. Meer informatie over [Power BI en toegang tot een sjabloon downloaden](https://blogs.msdn.microsoft.com/azuresqldbsupport/2017/05/26/sql-azure-blob-auditing-basic-power-bi-dashboard/).

* Downloaden van de logboekbestanden van uw Azure Storage blob-container via de portal of met een hulpprogramma zoals [Azure Opslagverkenner](http://storageexplorer.com/).
    * Nadat u lokaal een logboekbestand hebt gedownload, kunt u het bestand te openen, weergeven en analyseren van de logboeken in SSMS dubbelklikken.
    * U kunt ook meerdere bestanden tegelijkertijd via Azure Opslagverkenner downloaden. Met de rechtermuisknop op een specifieke submap en selecteer **opslaan als** om op te slaan in een lokale map.

* Extra methoden:
   * Na het downloaden van meerdere bestanden of een submap met logboekbestanden, kunt u deze lokaal zoals beschreven in de controlebestanden van SSMS Merge-instructies eerder beschreven samenvoegen.

   * Weergave auditingfunctie voor blobs registreert programmatisch:

     * Gebruik de [uitgebreide gebeurtenissen lezer](https://blogs.msdn.microsoft.com/extended_events/2011/07/20/introducing-the-extended-events-reader/) C#-bibliotheek.
     * [Uitgebreide gebeurtenissen bestanden query](https://sqlscope.wordpress.com/2014/11/15/reading-extended-event-files-using-client-side-tools-only/) met behulp van PowerShell.




## <a id="subheading-5"></a>Procedures voor productie
<!--The description in this section refers to preceding screen captures.-->

### <a id="subheading-6">Geogerepliceerde databases controle</a>
Met geogerepliceerde databases, wanneer u controle in te schakelen op de primaire database hebben de secundaire database een identieke controlebeleid. Het is ook mogelijk om in te stellen op de secundaire database controleren door het inschakelen van controle op de **secundaire server**, onafhankelijk van de primaire database.

* Niveau van de server (**aanbevolen**): controle op zowel de **primaire server** , evenals de **secundaire server** -de primaire en secundaire databases elk worden gecontroleerd onafhankelijk op basis van hun respectieve serverniveau-beleid.

* Database-niveau: Databaseniveau controle voor secundaire databases kan alleen worden geconfigureerd uit de primaire database controle-instellingen.
   * Controle van de BLOB moet worden ingeschakeld op de *primaire database zelf*, niet op de server.
   * Nadat de blob-controle is ingeschakeld op de primaire database, wordt deze ook ingeschakeld op de secundaire database.

     >[!IMPORTANT]
     >Als databaseniveau controle, zal de opslaginstellingen voor de secundaire database zijn identiek aan die van de primaire database waardoor cross-regionale verkeer. U wordt aangeraden alleen serverniveau controle in te schakelen en laat de databaseniveau controle uitgeschakeld voor alle databases.
<br>

### <a id="subheading-6">Opslag van sessiesleutels</a>
In productie bent u waarschijnlijk uw opslagsleutels periodiek te vernieuwen. Bij het vernieuwen van uw sleutels, moet u het beveiligingsbeleid opnieuw op te slaan. Het proces is als volgt:

1. Open de **opslaggroep** blade. In de **toegangssleutel voor opslag** de optie **secundaire**, en klik op **OK**. Klik vervolgens op **opslaan** aan de bovenkant van de controlemogelijkheden configuratie-blade.

    ![Navigatiedeelvenster][5]
2. Ga naar de blade van de configuratie van opslag en opnieuw genereren van de primaire toegangssleutel.

    ![Navigatiedeelvenster][6]
3. Ga terug naar de blade controle configuratie schakelt de toegangssleutel voor opslag van secundaire op primaire en klik vervolgens op **OK**. Klik vervolgens op **opslaan** aan de bovenkant van de controlemogelijkheden configuratie-blade.
4. Ga terug naar de blade van de configuratie van opslag en opnieuw genereren van de secundaire toegangssleutel (in voorbereiding op de volgende sleutel vernieuwingscyclus).

## <a name="manage-sql-database-auditing-using-azure-powershell"></a>Controle van SQL-database met Azure PowerShell beheren


* **PowerShell-cmdlets**:

   * [Get-AzureRMSqlDatabaseAuditing][101]
   * [Get-AzureRMSqlServerAuditing][102]
   * [Verwijder AzureRMSqlDatabaseAuditing][103]
   * [Verwijder AzureRMSqlServerAuditing][104]
   * [Set-AzureRMSqlDatabaseAuditing][105]
   * [Set-AzureRMSqlServerAuditing][106]

   Zie voor een scriptvoorbeeld van een, [configureren van controle en detectie van bedreigingen met behulp van PowerShell](scripts/sql-database-auditing-and-threat-detection-powershell.md).

## <a name="manage-sql-database-auditing-using-rest-api"></a>SQL database auditing met REST API beheren

* **REST-API - Auditingfunctie voor blobs**:

   * [Maken of bijwerken van de Database Blob controlebeleid](https://msdn.microsoft.com/library/azure/mt695939.aspx)
   * [Maken of bijwerken van de Server Blob controlebeleid](https://msdn.microsoft.com/library/azure/mt771861.aspx)
   * [Database-Blob controlebeleid ophalen](https://msdn.microsoft.com/library/azure/mt695938.aspx)
   * [Ophalen van Server Blob controlebeleid](https://msdn.microsoft.com/library/azure/mt771860.aspx)
   * [Ophalen van Server Blob die het resultaat van controle](https://msdn.microsoft.com/library/azure/mt771862.aspx)


<!--Anchors-->
[Azure SQL Database Auditing overview]: #subheading-1
[Set up auditing for your database]: #subheading-2
[Analyze audit logs and reports]: #subheading-3
[Practices for usage in production]: #subheading-5
[Storage Key Regeneration]: #subheading-6
[Automation (PowerShell / REST API)]: #subheading-7
[Blob/Table differences in Server auditing policy inheritance]: (#subheading-8)

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
