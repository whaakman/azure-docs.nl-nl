---
title: Een back-up herstellen voor beheerd exemplaar voor Azure SQL Database | Microsoft Docs
description: Herstel een back-up van de database voor een beheerd exemplaar voor Azure SQL Database met behulp van SSMS.
keywords: zelfstudie sql database, beheerd exemplaar sql database, back-up herstellen
services: sql-database
author: bonova
ms.reviewer: carlrab, srbozovi
ms.service: sql-database
ms.custom: managed instance
ms.topic: tutorial
ms.date: 07/06/2018
ms.author: bonova
manager: craigg
ms.openlocfilehash: 5dd8b1f662f1ae6d6502743c6d976db4b58e962f
ms.sourcegitcommit: 4ea0cea46d8b607acd7d128e1fd4a23454aa43ee
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/15/2018
ms.locfileid: "41920289"
---
# <a name="restore-a-database-backup-to-an-azure-sql-database-managed-instance"></a>Een back-up van de database herstellen voor een beheerd exemplaar voor Azure SQL Database

Deze zelfstudie laat zien hoe u een back-up van een database die is opgeslagen in Azure Blob Storage, kunt herstellen naar het beheerd exemplaar met behulp van het standaard back-upbestand van Wide World Importers. Deze methode vereist enige uitvaltijd. Lees het artikel [Managed Instance migration using DMS](../dms/tutorial-sql-server-to-managed-instance.md) (Migratie van een beheerd exemplaar via DMS) voor een zelfstudie over gebruik van de Azure Database Migration Service (DMS). Zie [Migratie van SQL Server-exemplaar naar beheerd exemplaar voor Azure SQL Database](sql-database-managed-instance-migrate.md) voor een bespreking van de diverse migratiemethoden.

> [!div class="checklist"]
> * Het standaard back-upbestand van Wide World Importers downloaden
> * Een Azure-opslagaccount maken en het back-upbestand uploaden
> * De Wide World Importers-database herstellen op basis van een back-upbestand

## <a name="prerequisites"></a>Vereisten

Deze zelfstudie gebruikt als uitgangspunt de resources die in deze zelfstudie zijn gemaakt: [Een beheerd exemplaar voor Azure SQL Database maken](sql-database-managed-instance-get-started.md).

## <a name="download-the-wide-world-importers---standard-backup-file"></a>Het standaard back-upbestand van Wide World Importers downloaden

Volg deze stappen om het standaard back-upbestand van Wide World Importers te downloaden.

Wanneer u Internet Explorer gebruikt, voer dan https://github.com/Microsoft/sql-server-samples/releases/download/wide-world-importers-v1.0/WideWorldImporters-Standard.bak in het URL-adresvak in en klik desgevraagd op **Opslaan** om dit bestand op te slaan in de map **Downloads**.

## <a name="log-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Meld u aan bij [Azure Portal](https://portal.azure.com/#create/Microsoft.SQLManagedInstance).

## <a name="create-azure-storage-account-and-upload-backup-file"></a>Een Azure-opslagaccount maken en het back-upbestand uploaden

1. Klik in de linkerbovenhoek van Azure Portal op **Een resource maken**.
2. Zoek naar **opslag** en klik vervolgens op **Opslagaccount** om het formulier voor opslagaccounts te openen.

   ![opslagaccount](./media/sql-database-managed-instance-tutorial/storage-account.png)

3. Vul het formulier voor het opslagaccount in met behulp van de gegevens in onderstaande tabel:

   | Instelling| Voorgestelde waarde | Beschrijving |
   | ------ | --------------- | ----------- |
   |**Naam**|Een geldige naam|Zie [Naming conventions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) (Naamgevingsconventies) voor geldige namen.|
   |**Implementatiemodel**|Resourcemodel||
   |**Type account**|Blob Storage||
   |**Prestaties**|Standard of Premium|Magnetische schijven of SSD's|
   |**Replicatie**|Lokaal redundante opslag||
   |\*\*Opslaglaag (standaard)|Statisch of dynamisch||
   |**Veilige overdracht vereist**|Uitgeschakeld||
   |**Abonnement**|Uw abonnement|Zie [Abonnementen](https://account.windowsazure.com/Subscriptions) voor meer informatie over uw abonnementen.|
   |**Resourcegroep**|De resourcegroep die u eerder hebt gemaakt|| 
   |**Locatie**|De locatie die u eerder hebt geselecteerd||
   |**Virtuele netwerken**|Uitgeschakeld||

4. Klik op **Create**.

   ![details opslagaccount](./media/sql-database-managed-instance-tutorial/storage-account-details.png)

5. Nadat de implementatie van het opslagaccount is voltooid, opent u het nieuwe opslagaccount.
6. Klik onder **Instellingen** op **Shared Access Signature** om het formulier voor Shared Access Signature (SAS) te openen.

   ![sas-formulier](./media/sql-database-managed-instance-tutorial/sas-form.png)

7. Op het SAS-formulier kunt u de standaardwaarden naar wens aanpassen. Houd er rekening mee dat de vervaldatum/-tijd standaard is ingesteld op slechts 8 uur.
8. Klik op **SAS genereren**.

   ![sas-formulier voltooid](./media/sql-database-managed-instance-tutorial/sas-generate.png)

9. Kopieer de **SAS-token** en de **SAS-URL van de Blob-server** en sla deze op.
10. Klik onder **Instellingen** op **Containers**.

    ![containers](./media/sql-database-managed-instance-tutorial/containers.png)

11. Klik op **+Container** om een container voor uw back-upbestand te maken.
12. Vul het formulier voor de container in met behulp van de gegevens in onderstaande tabel:

    | Instelling| Voorgestelde waarde | Beschrijving |
   | ------ | --------------- | ----------- |
   |**Naam**|Een geldige naam|Zie [Naming conventions](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions) (Naamgevingsconventies) voor geldige namen.|
   |**Openbaar toegangsniveau**|Privé (geen anonieme toegang)||

    ![containerdetails](./media/sql-database-managed-instance-tutorial/container-detail.png)

13. Klik op **OK**.
14. Nadat de container is gemaakt, opent u de container.

    ![container](./media/sql-database-managed-instance-tutorial/container.png)

15. Klik op **Containereigenschappen** en kopieer de URL vervolgens naar de container.

    ![URL voor container](./media/sql-database-managed-instance-tutorial/container-url.png)

16. Klik op **Uploaden** om het formulier **Blob uploaden** te openen

    ![uploaden](./media/sql-database-managed-instance-tutorial/upload.png)

17. Blader naar uw downloadmap en selecteer het bestand **WideWorldIimporters-Standard.bak**.

    ![uploaden](./media/sql-database-managed-instance-tutorial/upload-bak.png)

18. Klik op **Uploaden**.
19. Ga niet verder voordat het uploaden is voltooid.

    ![uploaden voltooid](./media/sql-database-managed-instance-tutorial/upload-complete.png)


## <a name="restore-the-wide-world-importers-database-from-a-backup-file"></a>De Wide World Importers-database herstellen op basis van een back-upbestand

Volg deze stappen om in SQL Server Management Studio de Wide World Importers-database te herstellen naar uw beheerde exemplaar op basis van het back-upbestand.

1. Open een nieuw queryvenster in SQL Server Management Studio.
2. Gebruik het volgende script om SAS-referenties te maken. Geef daarbij de URL voor de container van het opslagaccount en de SAS-sleutel op, zoals aangegeven.

   ```sql
   CREATE CREDENTIAL [https://<storage_account_name>.blob.core.windows.net/<container>] 
      WITH IDENTITY = 'SHARED ACCESS SIGNATURE'
      , SECRET = '<shared_access_signature_key_with_removed_first_?_symbol>' 
   ```

    ![referenties](./media/sql-database-managed-instance-tutorial/credential.png)

3. Gebruik het volgende script om de SAS-referenties en geldigheid van de back-up te controleren. Geef daarbij de URL voor de container met het back-upbestand op:

   ```sql
   RESTORE FILELISTONLY FROM URL = 
      'https://<storage_account_name>.blob.core.windows.net/<container>/WideWorldImporters-Standard.bak'
   ```

    ![lijst met bestanden](./media/sql-database-managed-instance-tutorial/file-list.png)

4. Gebruik het volgende script om de Wide World Importers-database te herstellen op basis van een back-upbestand. Geef daarbij de URL voor de container met het back-upbestand op:

   ```sql
   RESTORE DATABASE [Wide World Importers] FROM URL =
     'https://<storage_account_name>.blob.core.windows.net/<container>/WideWorldImporters-Standard.bak'`
   ```

    ![herstellen in uitvoering](./media/sql-database-managed-instance-tutorial/restore-executing.png)

5. Als u de status van het herstellen wilt bijhouden, voert u de volgende query uit in een nieuwe querysessie:

   ```sql
   SELECT session_id as SPID, command, a.text AS Query, start_time, percent_complete
      , dateadd(second,estimated_completion_time/1000, getdate()) as estimated_completion_time 
   FROM sys.dm_exec_requests r 
   CROSS APPLY sys.dm_exec_sql_text(r.sql_handle) a 
   WHERE r.command in ('BACKUP DATABASE','RESTORE DATABASE')`
   ```

    ![percentage voltooid van herstellen](./media/sql-database-managed-instance-tutorial/restore-percent-complete.png)

6. Wanneer het herstellen is voltooid, bekijkt u het herstel in Objectverkenner. 

    ![herstellen voltooid](./media/sql-database-managed-instance-tutorial/restore-complete.png)

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u gezien hoe u een back-up van een database die is opgeslagen in Azure Blob Storage, kunt herstellen naar het beheerd exemplaar met behulp van het standaard back-upbestand van Wide World Importers. U hebt geleerd hoe u: 

> [!div class="checklist"]
> * Het standaard back-upbestand van Wide World Importers downloaden
> * Een Azure-opslagaccount maken en het back-upbestand uploaden
> * De Wide World Importers-database herstellen op basis van een back-upbestand

Ga naar de volgende zelfstudie voor informatie over het migreren van SQL Server naar een beheerd exemplaar voor Azure SQL Database met behulp van DMS.

> [!div class="nextstepaction"]
>[SQL Server migreren naar beheerd exemplaar voor Azure SQL Database met behulp van DMS](../dms/tutorial-sql-server-to-managed-instance.md)
