---
title: Azure SQL-Database beheerd exemplaar controle | Microsoft Docs
description: Meer informatie over het aan de slag met Azure SQL Database beheerd exemplaar Auditing met T-SQL
services: sql-database
author: giladm
manager: craigg
ms.reviewer: carlrab
ms.service: sql-database
ms.custom: security
ms.topic: article
ms.date: 03/19/2018
ms.author: giladm
ms.openlocfilehash: 3d5a4ad3f4046dfdfe6eb3f7ddd931ccb240b1a9
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
# <a name="get-started-with-azure-sql-database-managed-instance-auditing"></a>Aan de slag met Azure SQL Database beheerd exemplaar Auditing

[Azure SQL Database beheerd-instantie](sql-database-managed-instance.md) controle houdt database gebeurtenissen en schrijft deze naar een auditlogboek Meld u bij uw Azure storage-account. Ook controleren:
- Helpt u naleving van regelgeving onderhouden, de activiteit in een database begrijpen en meer inzicht krijgen in discrepanties en afwijkingen die kunnen wijzen op problemen voor het bedrijf of vermoedelijke beveiligingsschendingen.
- Hiermee wordt en vereenvoudigt voldoen aan de nalevingsstandaards, hoewel het geen garantie naleving. Zie voor meer informatie over Azure programma's die nalevingsscan standaarden, de [Azure Vertrouwenscentrum](https://azure.microsoft.com/support/trust-center/compliance/).


## <a name="set-up-auditing-for-your-server"></a>Controle voor uw server instellen

De volgende sectie beschrijft de configuratie van de controle op uw beheerde exemplaar.
1. Ga naar de [Azure Portal](https://portal.azure.com).
2. De volgende stappen maakt u een Azure Storage **container** waar controlelogboeken worden opgeslagen.

   - Navigeer naar de Azure-opslag waar u wilt uw controlelogboeken opslaan.

     > [!IMPORTANT]
     > Een opslagaccount in dezelfde regio bevinden als de server beheerd exemplaar gebruiken om te voorkomen dat de regio-overschrijdende leest/schrijft.

   - In de storage-account, gaat u naar **overzicht** en klik op **Blobs**.

     ![Navigatievenster][1]

   - Klik in het menu bovenaan op **+ Container** voor het maken van een nieuwe container.

     ![Navigatievenster][2]

   - Geef een container **naam**, openbare toegang instellen voor **persoonlijke**, en klik vervolgens op **OK**.

     ![Navigatievenster][3]

   - Klik op de nieuwe container in de lijst containers en klik vervolgens op **eigenschappen van Container**.

     ![Navigatievenster][4]

   - Kopieer de URL van de container door te klikken op het pictogram kopiëren en opslaan van de URL (bijvoorbeeld in Kladblok) voor toekomstig gebruik. De container-URL-indeling moet `https://<StorageName>.blob.core.windows.net/<ContainerName>`

     ![Navigatievenster][5]

3. De volgende stappen uit een Azure Storage genereren **SAS-Token** gebruikt voor het verlenen van controle-exemplaar beheerd rechten voor het opslagaccount.

   - Navigeer naar de Azure Storage-account waarop u de container in de vorige stap hebt gemaakt.

   - Klik op **Shared access signature** in het menu instellingen voor de opslag.

     ![Navigatievenster][6]

   - De SA's als volgt configureren:
     - **Services toegestaan**: Blob
     - **Begindatum**: om te voorkomen dat problemen met de tijdzone, verdient het de datum van gisteren gebruiken.
     - **Einddatum**: Kies de datum waarop deze SAS-Token verloopt. 

       > [!NOTE]
       > Verleng het token na het verstrijken ter voorkoming van fouten audit.

     - Klik op **SAS genereren**.

       ![Navigatievenster][7]

   - Nadat de gebruiker op een SAS genereren, de SAS-Token wordt weergegeven onder. Kopieer het token door te klikken op het pictogram kopiëren en opslaan (bijvoorbeeld in Kladblok) voor toekomstig gebruik.

     > [!IMPORTANT]
     > Verwijder het vraagteken ('? ') tekens vanaf het begin van het token.

     ![Navigatievenster][8]

4. Verbinding maken met uw beheerde exemplaar via SQL Server Management Studio (SSMS).

5. Voer de volgende T-SQL-instructie naar **een nieuwe referentie maken** met behulp van de Container-URL en de SAS-Token die u hebt gemaakt in de vorige stappen:

    ```SQL
    CREATE CREDENTIAL [<container_url>]
    WITH IDENTITY='SHARED ACCESS SIGNATURE',
    SECRET = '<SAS KEY>'
    GO
    ```

6. Voer de volgende T-SQL-instructie voor het maken van een nieuwe Server Audit (Kies de naam van uw eigen audit, gebruikt u de Container-URL die u hebt gemaakt in de vorige stappen):

    ```SQL
    CREATE SERVER AUDIT [<your_audit_name>]
    TO URL ( PATH ='<container_url>' [, RETENTION_DAYS =  integer ])
    GO
    ```

    Als niet wordt opgegeven, `RETENTION_DAYS` standaardwaarde is 0 (onbeperkt bewaren).

    Voor meer informatie:
    - [Controle van de verschillen tussen beheerde exemplaar, Azure SQL-database en SQL Server](#subheading-3)
    - [CONTROLE VAN DE SERVER MAKEN](https://docs.microsoft.com/sql/t-sql/statements/create-server-audit-transact-sql)
    - [ALTER SERVER AUDIT](https://docs.microsoft.com/sql/t-sql/statements/alter-server-audit-transact-sql)

7. Een Server Audit Specification of Auditspecificatie maken zoals u zou voor SQL Server doen:
    - [Handleiding voor Server audit specification T-SQL maken](https://docs.microsoft.com/ sql/t-sql/statements/create-server-audit-specification-transact-sql)
    - [Maken van Database audit specification T-SQL-handleiding](https://docs.microsoft.com/ sql/t-sql/statements/create-database-audit-specification-transact-sql)

8. Hiermee schakelt u de controle van server die u hebt gemaakt in stap 6:

    ```SQL
    ALTER SERVER AUDIT [<your_audit_name>]
    WITH (STATE=ON);
    GO
    ```

## <a name="analyze-audit-logs"></a>Controlelogboeken analyseren
Er zijn verschillende methoden die u gebruiken kunt om blob controlelogboeken weer te geven.

- Gebruik de systeemfunctie `sys.fn_get_audit_file` (T-SQL) te retourneren van de logboekgegevens audit in tabelvorm. Zie voor meer informatie over het gebruik van deze functie de [sys.fn_get_audit_file documentatie](https://docs.microsoft.com/sql/relational-databases/system-functions/sys-fn-get-audit-file-transact-sql).

- Voor een volledige lijst van audit log verbruikmethoden, raadpleegt u de [aan de slag met SQL database auditing](https://docs.microsoft.com/ azure/sql-database/sql-database-auditing).

> [!IMPORTANT]
> De methode voor het weergeven van AuditRecords vanuit de Azure-portal (deelvenster "Audit records") is momenteel niet beschikbaar voor beheerde exemplaar.

## <a name="auditing-differences-between-managed-instance-azure-sql-database-and-sql-server"></a>Controle van de verschillen tussen beheerde exemplaar, Azure SQL Database en SQL Server

De belangrijkste verschillen tussen de SQL-Audit op beheerde exemplaar, Azure SQL Database en SQL Server on-premises zijn:

- Beheerde exemplaar SQL Audit werkt op het serverniveau en de winkels `.xel` -logboekbestanden op Azure blob storage-account.
- In Azure SQL Database werkt SQL Audit op databaseniveau.
- Op SQL Server on-premises / virtuele niveau machines, SQL Audit werkt op de server, maar winkels gebeurtenissen op bestanden system/windows-gebeurtenislogboeken.

XEvent-controle in beheerde exemplaar ondersteunt Azure blob storage-doelen. Bestands- en windows logboeken zijn **niet ondersteund**.

De sleutel verschillen de `CREATE AUDIT` syntaxis voor controle naar Azure blob storage zijn:
- Een nieuwe syntaxis `TO URL` wordt aangeboden en kunt u de URL van de Azure blob Storage-container opgeven waar de `.xel` bestanden worden geplaatst.
- De syntaxis van de `TO FILE` is **niet ondersteund** omdat beheerd exemplaar heeft geen toegang Windows-bestandsshares tot.
- Optie voor afsluiten is **niet ondersteund**.
- `queue_delay` 0 is **niet ondersteund**.


## <a name="next-steps"></a>Volgende stappen

- Voor een volledige lijst van audit log verbruikmethoden, raadpleegt u de [aan de slag met SQL database auditing](https://docs.microsoft.com/ azure/sql-database/sql-database-auditing).
- Zie voor meer informatie over Azure programma's die nalevingsscan standaarden, de [Azure Vertrouwenscentrum](https://azure.microsoft.com/support/trust-center/compliance/).


<!--Anchors-->
[Set up auditing for your server]: #subheading-1
[Analyze audit logs]: #subheading-2
[Auditing differences between Managed Instance, Azure SQL DB and SQL Server]: #subheading-3

<!--Image references-->
[1]: ./media/sql-managed-instance-auditing/1_blobs_widget.png
[2]: ./media/sql-managed-instance-auditing/2_create_container_button.png
[3]: ./media/sql-managed-instance-auditing/3_create_container_config.png
[4]: ./media/sql-managed-instance-auditing/4_container_properties_button.png
[5]: ./media/sql-managed-instance-auditing/5_container_copy_name.png
[6]: ./media/sql-managed-instance-auditing/6_storage_settings_menu.png
[7]: ./media/sql-managed-instance-auditing/7_sas_configure.png
[8]: ./media/sql-managed-instance-auditing/8_sas_copy.png
