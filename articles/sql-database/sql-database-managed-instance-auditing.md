---
title: Azure SQL Database Managed Instance-controle | Microsoft Docs
description: Leer hoe u aan de slag met Azure SQL Database Managed Instance Auditing met T-SQL
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
f1_keywords:
- mi.azure.sqlaudit.general.f1
author: vainolo
ms.author: vainolo
ms.reviewer: vanto
manager: craigg
ms.date: 09/20/2018
ms.openlocfilehash: 045314980d0051e8b5ef71bdf95023084eff1880
ms.sourcegitcommit: 3ab534773c4decd755c1e433b89a15f7634e088a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/07/2019
ms.locfileid: "54063865"
---
# <a name="get-started-with-azure-sql-database-managed-instance-auditing"></a>Aan de slag met Azure SQL Database Managed Instance Auditing

[Azure SQL Database Managed Instance](sql-database-managed-instance.md) databasegebeurtenissen controleren en geschreven naar een auditlogboek in uw Azure storage-account. Ook controle:

- Helpt u naleving van regelgeving, inzicht in de databaseactiviteiten en inzicht krijgen in discrepanties en afwijkingen die kunnen wijzen op problemen voor het bedrijf of vermoedelijke beveiligingsschendingen.
- Hiermee wordt en vergemakkelijkt de naleving van standaarden voor compliance, hoewel het nalevingsbeleid geen garantie. Zie voor meer informatie over Azure-programma's die ondersteuning voor standaarden naleving, de [Azure Trust Center](https://azure.microsoft.com/support/trust-center/compliance/).

## <a name="set-up-auditing-for-your-server-to-azure-storage"></a>Instellen van de controle voor uw server naar Azure Storage 

Het volgende gedeelte bevat de configuratie van de controle op uw beheerde exemplaar.

1. Ga naar de [Azure Portal](https://portal.azure.com).
2. De volgende stappen maakt u een Azure Storage **container** waar de logboeken voor controle worden opgeslagen.

   - Navigeer naar de Azure-opslag waar u wilt voor het opslaan van uw auditlogboeken.

     > [!IMPORTANT]
     > Een opslagaccount in dezelfde regio als de Managed Instance-server gebruiken om te voorkomen dat de regio-overschrijdende lees-/ schrijfbewerkingen.

   - In de storage-account, gaat u naar **overzicht** en klikt u op **Blobs**.

     ![Navigatievenster][1]

   - In het bovenste menu, klikt u op **+ Container** om een nieuwe container te maken.

     ![Navigatievenster][2]

   - Geef een container **naam**, openbare toegang instellen voor **persoonlijke**, en klik vervolgens op **OK**.

     ![Navigatievenster][3]

   - In de lijst met containers, klikt u op de nieuwe container en klik vervolgens op **containereigenschappen**.

     ![Navigatievenster][4]

   - Kopieer de URL van de container door te klikken op het pictogram voor kopiëren en sla de URL (bijvoorbeeld in Kladblok) op voor toekomstig gebruik. De container-URL-notatie moet zijn `https://<StorageName>.blob.core.windows.net/<ContainerName>`

     ![Navigatievenster][5]

3. De volgende stappen uit een Azure Storage genereren **SAS-Token** gebruikt voor het verlenen van beheerd exemplaar controle rechten voor het opslagaccount.

   - Navigeer naar de Azure Storage-account waar u de container in de vorige stap hebt gemaakt.

   - Klik op **handtekening voor gedeelde toegang** in het menu instellingen voor de opslag.

     ![Navigatievenster][6]

   - De SAS is als volgt configureren:
     - **Toegestane services**: Blob
     - **Begindatum**: om te voorkomen dat problemen met de tijdzone, het verdient aanbeveling om de datum van gisteren te gebruiken.
     - **Einddatum**: Kies de datum waarop deze SAS-Token is verlopen. 

       > [!NOTE]
       > Verleng het token na het verstrijken ter voorkoming van fouten voor controle.

     - Klik op **SAS genereren**.

       ![Navigatievenster][7]

   - Nadat u hebt geklikt op SAS genereren, de SAS-Token wordt weergegeven aan de onderkant. Kopieer het token door te klikken op het pictogram voor kopiëren en sla het op (bijvoorbeeld in Kladblok) voor toekomstig gebruik.

     > [!IMPORTANT]
     > Verwijder het vraagteken ("?") tekens vanaf het begin van het token.

     ![Navigatievenster][8]

4. Verbinding maken met uw beheerde exemplaar via SQL Server Management Studio (SSMS).

5. Voer de volgende T-SQL-instructie op **een nieuwe referentie maken** met behulp van de Container-URL en de SAS-Token die u hebt gemaakt in de vorige stappen:

    ```SQL
    CREATE CREDENTIAL [<container_url>]
    WITH IDENTITY='SHARED ACCESS SIGNATURE',
    SECRET = '<SAS KEY>'
    GO
    ```

6. Voer de volgende T-SQL-instructie voor het maken van een nieuwe Server Audit (Kies de naam van uw eigen controleren, gebruikt u de Container-URL die u hebt gemaakt in de vorige stappen):

    ```SQL
    CREATE SERVER AUDIT [<your_audit_name>]
    TO URL ( PATH ='<container_url>' [, RETENTION_DAYS =  integer ])
    GO
    ```

    Indien niet opgegeven, `RETENTION_DAYS` standaardwaarde is 0 (onbeperkte bewaarperiode).

    Meer informatie:
    - [Controle van de verschillen tussen de Managed Instance, Azure SQL-database en SQL Server](#auditing-differences-between-managed-instance-azure-sql-database-and-sql-server)
    - [SERVERAUDIT MAKEN](https://docs.microsoft.com/sql/t-sql/statements/create-server-audit-transact-sql)
    - [ALTER SERVERAUDIT](https://docs.microsoft.com/sql/t-sql/statements/alter-server-audit-transact-sql)

7. Maak een Auditspecificatie Server of Database Audit Specification zoals u zou voor SQL Server doen:
    - [Handleiding voor Server audit specification T-SQL maken](https://docs.microsoft.com/sql/t-sql/statements/create-server-audit-specification-transact-sql)
    - [Maken van Database audit specification T-SQL-handleiding](https://docs.microsoft.com/sql/t-sql/statements/create-database-audit-specification-transact-sql)

8. Schakel de controle van de server die u hebt gemaakt in stap 6:

    ```SQL
    ALTER SERVER AUDIT [<your_audit_name>]
    WITH (STATE=ON);
    GO
    ```

## <a name="set-up-auditing-for-your-server-to-event-hub-or-log-analytics"></a>Controle voor uw server voor het Event Hub of Log Analytics instellen

Controlelogboeken van een beheerd exemplaar kunnen worden verzonden naar zelfs Hubs of Log Analytics met Azure Monitor. Deze sectie wordt beschreven hoe u dit wilt configureren:

1. Navigeer de [Azure-Portal](https://portal.azure.com/) SQL beheerd exemplaar.

2. Klik op **diagnostische instellingen**.

3. Klik op **diagnostische gegevens inschakelen**. Als diagnostische gegevens al is ingeschakeld de *+ diagnostische instelling toevoegen* in plaats daarvan wordt weergegeven.

4. Selecteer **SQLSecurityAuditEvents** in de lijst van Logboeken.

5. Selecteer een doel voor de controlegebeurtenissen - Event Hub, de Log Analytics of beide. Configureren voor elk doel van de vereiste parameters (bijvoorbeeld Log Analytics-werkruimte).

6. Klik op **Opslaan**.

  ![Navigatievenster][9]

7. Verbinding maken met de Managed Instance via **SQL Server Management Studio (SSMS)** of een andere ondersteunde client.

8. Voer de volgende T-SQL-instructie voor het maken van een serveraudit:

    ```SQL
    CREATE SERVER AUDIT [<your_audit_name>] TO EXTERNAL_MONITOR;
    GO
    ```

9. Maak een auditspecificatie server of een auditspecificatie zoals u zou voor SQL Server doen:

   - [Handleiding voor Server audit specification T-SQL maken](https://docs.microsoft.com/sql/t-sql/statements/create-server-audit-specification-transact-sql)
   - [Maken van Database audit specification T-SQL-handleiding](https://docs.microsoft.com/sql/t-sql/statements/create-database-audit-specification-transact-sql)

10. Schakel de controle van de server hebt gemaakt in stap 7:
 
    ```SQL
    ALTER SERVER AUDIT [<your_audit_name>] WITH (STATE=ON);
    GO
    ```

## <a name="consume-audit-logs"></a>Auditlogboeken

### <a name="consume-logs-stored-in-azure-storage"></a>Logboeken die zijn opgeslagen in Azure Storage gebruiken

Er zijn verschillende methoden die u gebruiken kunt om blob auditing logboeken weer te geven.

- Gebruik de systeemfunctie `sys.fn_get_audit_file` (T-SQL) om te retourneren van de audit log-gegevens in tabelvorm. Zie voor meer informatie over het gebruik van deze functie, de [sys.fn_get_audit_file documentatie](https://docs.microsoft.com/sql/relational-databases/system-functions/sys-fn-get-audit-file-transact-sql).

- U kunt de logboeken voor controle verkennen met behulp van een hulpprogramma zoals Azure Storage Explorer. In Azure storage worden controleren logboeken opgeslagen als een verzameling van blob-bestanden in een container met de naam sqldbauditlogs. Zie voor meer informatie over de hiërarchie van de map storage naming conventions en logboekindeling, de Blobverwijzing Audit Log-indeling.

- Voor een volledige lijst van audit log verbruikmethoden, raadpleegt u de [aan de slag met SQL database auditing](https://docs.microsoft.com/ azure/sql-database/sql-database-auditing).

> [!IMPORTANT]
> Voor het weergeven van controlerecords in Azure portal (in het deelvenster 'Controlerecords') is momenteel niet beschikbaar voor Managed Instance.

### <a name="consume-logs-stored-in-event-hub"></a>Logboeken die zijn opgeslagen in de Event Hub gebruiken

Als u wilt controleren Logboeken gegevens uit Event Hub gebruiken, moet u voor het instellen van een stroom gebeurtenissen gebruiken en te schrijven naar een doel. Zie voor meer informatie de documentatie over Azure Event Hubs.

### <a name="consume-and-analyze-logs-stored-in-log-analytics"></a>Verbruiken en die zijn opgeslagen in Log Analytics-logboekbestanden analyseren

Als u auditlogboeken naar Log Analytics worden geschreven, zijn ze beschikbaar in de Log Analytics-werkruimte waar u geavanceerde zoekopdrachten op de controlegegevens uitvoeren kunt. Als uitgangspunt, gaat u naar de Log Analytics en klik vervolgens onder *algemene* sectie Klik *logboeken* en voer een eenvoudige query, zoals: `search "SQLSecurityAuditEvents"` om weer te geven van de audit-Logboeken.  

Log Analytics biedt u realtime operationele inzichten met behulp van geïntegreerde Zoek- en aangepaste dashboards voor het analyseren van miljoenen records gemakkelijk in uw werkbelastingen en servers. Zie voor meer nuttige informatie over opdrachten en Log Analytics-zoektaal, [Log Analytics zoeken verwijzing](https://docs.microsoft.com/azure/azure-monitor/log-query/log-query-overview).

## <a name="auditing-differences-between-managed-instance-azure-sql-database-and-sql-server"></a>Controle van de verschillen tussen de Managed Instance, Azure SQL Database en SQL Server

De belangrijkste verschillen tussen de SQL-controle in de Managed Instance, Azure SQL Database en SQL Server on-premises zijn:

- In het beheerde exemplaar SQL Audit werkt op het serverniveau en de winkels `.xel` -logboekbestanden op Azure blob storage-account.
- In Azure SQL Database werkt SQL-controle op het databaseniveau van de.
- In on-premises SQL Server / virtuele niveau machines, SQL-controle werkt op de server, maar winkels gebeurtenissen op bestanden system/windows-gebeurtenislogboeken.

XEvent-controle in het beheerde exemplaar biedt ondersteuning voor Azure blob storage-doelen. Bestands- en windows logboeken zijn **niet ondersteund**.

De sleutel verschillen de `CREATE AUDIT` syntaxis voor controle naar Azure blob-opslag zijn:

- Een nieuwe syntaxis `TO URL` wordt geleverd en kunt u de URL van de Azure blob Storage-container op te geven waar de `.xel` bestanden worden geplaatst.
- Een nieuwe syntaxis `TO EXTERNAL MONITOR` is opgegeven voor het inschakelen van zelfs Hub en de Log Analytics-doelen.
- De syntaxis van de `TO FILE` is **niet ondersteund** omdat het beheerde exemplaar heeft geen toegang tot Windows-bestandsshares.
- Optie voor afsluiten is **niet ondersteund**.
- `queue_delay` 0 is **niet ondersteund**.

## <a name="next-steps"></a>Volgende stappen

- Voor een volledige lijst van audit log verbruikmethoden, raadpleegt u de [aan de slag met SQL database auditing](https://docs.microsoft.com/azure/sql-database/sql-database-auditing).
- Zie voor meer informatie over Azure-programma's die ondersteuning voor standaarden naleving, de [Azure Trust Center](https://azure.microsoft.com/support/trust-center/compliance/).

<!--Image references-->
[1]: ./media/sql-managed-instance-auditing/1_blobs_widget.png
[2]: ./media/sql-managed-instance-auditing/2_create_container_button.png
[3]: ./media/sql-managed-instance-auditing/3_create_container_config.png
[4]: ./media/sql-managed-instance-auditing/4_container_properties_button.png
[5]: ./media/sql-managed-instance-auditing/5_container_copy_name.png
[6]: ./media/sql-managed-instance-auditing/6_storage_settings_menu.png
[7]: ./media/sql-managed-instance-auditing/7_sas_configure.png
[8]: ./media/sql-managed-instance-auditing/8_sas_copy.png
[9]: ./media/sql-managed-instance-auditing/9_mi_configure_diagnostics.png
