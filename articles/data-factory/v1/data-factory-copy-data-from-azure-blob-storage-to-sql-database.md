---
title: "Gegevens kopiëren van Blob-opslag naar SQL Database - Azure | Microsoft Docs"
description: "Deze zelfstudie ziet u het gebruik van de Kopieeractiviteit in een Azure Data Factory-pijplijn om gegevens te kopiëren van Blob-opslag met SQL-database."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: e4035060-93bf-4e8d-bf35-35e2d15c51e0
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/22/2018
ms.author: spelluru
robots: noindex
ms.openlocfilehash: 41ead775c8ce1919d8c3813b58c8a4d648c8bc64
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="tutorial-copy-data-from-blob-storage-to-sql-database-using-data-factory"></a>Zelfstudie: Gegevens kopiëren van Blob Storage met SQL Database met behulp van de Data Factory
> [!div class="op_single_selector"]
> * [Overzicht en vereisten](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md)
> * [De wizard Kopiëren](data-factory-copy-data-wizard-tutorial.md)
> * [Azure Portal](data-factory-copy-activity-tutorial-using-azure-portal.md)
> * [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
> * [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
> * [Azure Resource Manager-sjabloon](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
> * [REST API](data-factory-copy-activity-tutorial-using-rest-api.md)
> * [.NET API](data-factory-copy-activity-tutorial-using-dotnet-api.md)

> [!NOTE]
> Dit artikel is van toepassing op versie 1 van Data Factory, die algemeen beschikbaar is. Als u versie 2 van de Data Factory-service gebruikt, welke nog een preview is, raadpleegt u de [zelfstudie kopieeractiviteit in de documentatie voor versie 2](../quickstart-create-data-factory-dot-net.md). 

In deze zelfstudie maakt u een gegevensfactory met een pijplijn om gegevens te kopiëren van Blob-opslag met SQL-database.

Met de kopieeractiviteit wordt de gegevensverplaatsing in Azure Data Factory uitgevoerd. De activiteit wordt mogelijk gemaakt door een wereldwijd beschikbare service waarmee gegevens veilig, betrouwbaar en schaalbaar kunnen worden gekopieerd tussen verschillende gegevensarchieven. Zie [Activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) voor meer informatie over de kopieeractiviteit.  

> [!NOTE]
> Zie voor een gedetailleerd overzicht van de Data Factory-service de [Inleiding tot Azure Data Factory](data-factory-introduction.md) artikel.
>
>

## <a name="prerequisites-for-the-tutorial"></a>Vereisten voor de zelfstudie
Voordat u met deze zelfstudie begint, moet u aan de volgende vereisten voldoen:

* **Azure-abonnement**.  Als u geen abonnement hebt, kunt u binnen een paar minuten een gratis proefaccount maken. Zie de [gratis proefversie](http://azure.microsoft.com/pricing/free-trial/) artikel voor meer informatie.
* **Azure Storage-Account**. Gebruik van de blob storage als een **bron** gegevens opslaan in deze zelfstudie. Als u geen Azure Storage-account hebt, raadpleegt u het artikel [Een opslagaccount maken](../../storage/common/storage-create-storage-account.md#create-a-storage-account) voor de stappen voor het maken van een account.
* **Azure SQL-database**. Gebruik van een Azure SQL database als een **bestemming** gegevens opslaan in deze zelfstudie. Als u een Azure SQL-database die u in de zelfstudie, Zie gebruiken kunt geen [maken en configureren van een Azure SQL Database](../../sql-database/sql-database-get-started.md) een maken.
* **SQL Server 2012/2014 or Visual Studio 2013**. U gebruikt de SQL Server Management Studio of Visual Studio een voorbeelddatabase te maken en de result-gegevens weergeven in de database.  

## <a name="collect-blob-storage-account-name-and-key"></a>Blob storage-accountnaam en sleutel verzamelen
U moet de accountnaam en accountsleutel van uw Azure storage-account te doen in deze zelfstudie. Noteer **accountnaam** en **accountsleutel** voor uw Azure storage-account.

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Klik op **meer services** op het menu aan de linkerkant en selecteer **Opslagaccounts**.

    ![Bladeren - Storage-accounts](media/data-factory-copy-data-from-azure-blob-storage-to-sql-database/browse-storage-accounts.png)
3. In de **Opslagaccounts** blade, selecteer de **Azure storage-account** die u wilt gebruiken in deze zelfstudie.
4. Selecteer **toegangssleutels** koppeling onder **instellingen**.
5. Klik op **kopie** (afbeelding)-knop naast **opslagaccountnaam** tekst vak en opslaan en ergens plakken (bijvoorbeeld: in een tekstbestand).
6. Herhaal de vorige stap om te kopiëren of noteer de **key1**.

    ![Toegangssleutel voor opslag](media/data-factory-copy-data-from-azure-blob-storage-to-sql-database/storage-access-key.png)
7. Alle blades te sluiten door te klikken op **X**.

## <a name="collect-sql-server-database-user-names"></a>SQL server, database, gebruikersnamen verzamelen
U moet de namen van Azure SQL-server, database en gebruiker in deze zelfstudie. Noteer de namen van **server**, **database**, en **gebruiker** voor uw Azure SQL database.

1. In de **Azure-portal**, klikt u op **meer services** op de linkerkant en selecteer **SQL-databases**.
2. In de **SQL databases blade**, selecteer de **database** die u wilt gebruiken in deze zelfstudie. Noteer de **databasenaam**.  
3. In de **SQL-database** blade, klikt u op **eigenschappen** onder **instellingen**.
4. Noteer de waarden voor **servernaam** en **AANMELDGEGEVENS van SERVERBEHEERDER**.
5. Alle blades te sluiten door te klikken op **X**.

## <a name="allow-azure-services-to-access-sql-server"></a>Azure-services voor toegang tot SQL server toestaan
Zorg ervoor dat **toegang tot Azure-services toestaan** instelling ingeschakeld **ON** voor uw Azure SQL-server zodat de Data Factory-service toegang heeft tot uw Azure SQL-server. Voer de volgende stappen uit om dit te controleren en de instelling in te schakelen:

1. Klik op de hub **Meer services** aan de linkerkant en klik op **SQL-servers**.
2. Selecteer uw server en klik op **Firewall** onder **INSTELLINGEN**.
3. In de blade **Firewallinstellingen**schakelt u **Toegang tot Azure-services toestaan** **in**.
4. Alle blades te sluiten door te klikken op **X**.

## <a name="prepare-blob-storage-and-sql-database"></a>Blob-opslag- en SQL-Database voorbereiden
Nu uw Azure blob storage en Azure SQL database voor de zelfstudie door bereid de volgende stappen uit te voeren:  

1. Start Kladblok. Kopieer de volgende tekst en sla het bestand als **emp.txt** naar **C:\ADFGetStarted** map op de harde schijf.

    ```
    John, Doe
    Jane, Doe
    ```
2. Gebruik hulpprogramma's zoals [Azure Opslagverkenner](http://storageexplorer.com/) om de container **adftutorial** te maken en om het bestand **emp.txt** te uploaden naar de container.

    ![Azure Storage Explorer. Gegevens kopiëren van Blob-opslag naar SQL-database](./media/data-factory-copy-data-from-azure-blob-storage-to-sql-database/getstarted-storage-explorer.png)
3. Gebruik het volgende SQL-script om de tabel **emp** te maken in uw Azure SQL Database.  

    ```SQL
    CREATE TABLE dbo.emp
    (
        ID int IDENTITY(1,1) NOT NULL,
        FirstName varchar(50),
        LastName varchar(50),
    )
    GO

    CREATE CLUSTERED INDEX IX_emp_ID ON dbo.emp (ID);
    ```

    **Als u SQL Server 2012-2014 op uw computer geïnstalleerd hebben:** Volg de instructies uit [beherende Azure SQL Database met SQL Server Management Studio](../../sql-database/sql-database-manage-azure-ssms.md) verbinding maken met uw Azure SQL-server en voer de SQL-script. 

    Als de client geen toegang heeft tot de Azure SQL-server, moet u de firewall configureren voor uw Azure SQL-server zodat toegang vanaf uw apparaat (IP-adres) wordt toegestaan. Raadpleeg [dit artikel](../../sql-database/sql-database-configure-firewall-settings.md) voor stappen waarmee u uw firewall kunt configureren voor uw Azure SQL-server.

## <a name="create-a-data-factory"></a>Een gegevensfactory maken
U kunt de vereisten hebt voltooid. U kunt een gegevensfactory met een van de volgende manieren maken. Klik op een van de opties in de vervolgkeuzelijst boven of de volgende koppelingen voor het uitvoeren van de zelfstudie.     

* [De wizard Kopiëren](data-factory-copy-data-wizard-tutorial.md)
* [Azure Portal](data-factory-copy-activity-tutorial-using-azure-portal.md)
* [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md)
* [PowerShell](data-factory-copy-activity-tutorial-using-powershell.md)
* [Azure Resource Manager-sjabloon](data-factory-copy-activity-tutorial-using-azure-resource-manager-template.md)
* [REST API](data-factory-copy-activity-tutorial-using-rest-api.md)
* [.NET API](data-factory-copy-activity-tutorial-using-dotnet-api.md)

> [!NOTE]
> In de gegevenspijplijn in deze zelfstudie worden gegevens van een brongegevensarchief gekopieerd naar een doelgegevensarchief. Er worden geen invoergegevens mee getransformeerd in uitvoergegevens. Zie [Zelfstudie: uw eerste pijplijn maken om gegevens te transformeren met een Hadoop-cluster](data-factory-build-your-first-pipeline.md) voor meer informatie over het transformeren van gegevens met Azure Data Factory.
> 
> U kunt twee activiteiten koppelen (de ene activiteit na de andere laten uitvoeren) door de uitvoergegevensset van één activiteit in te stellen als invoergegevensset voor een andere activiteit. Zie [Planning en uitvoering in Data Factory](data-factory-scheduling-and-execution.md) voor gedetailleerde informatie. 
