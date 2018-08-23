---
title: Gegevens kopiëren van Blob Storage naar SQL Database - Azure | Microsoft Docs
description: Deze zelfstudie leert u over het gebruik van de Kopieeractiviteit in een Azure Data Factory-pijplijn om gegevens te kopiëren van Blob-opslag naar SQL database.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: ''
editor: ''
ms.assetid: e4035060-93bf-4e8d-bf35-35e2d15c51e0
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/22/2018
ms.author: jingwang
robots: noindex
ms.openlocfilehash: a9f76b38139cccedb97c6026f0e0efa14d0dbc8c
ms.sourcegitcommit: 8ebcecb837bbfb989728e4667d74e42f7a3a9352
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/21/2018
ms.locfileid: "42054116"
---
# <a name="tutorial-copy-data-from-blob-storage-to-sql-database-using-data-factory"></a>Zelfstudie: Gegevens kopiëren van Blob-opslag naar SQL Database met behulp van Data Factory
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
> Dit artikel is van toepassing op versie 1 van Data Factory. Als u de huidige versie van de Data Factory-service gebruikt, raadpleegt u de [zelfstudie over kopieeractiviteiten](../quickstart-create-data-factory-dot-net.md). 

In deze zelfstudie maakt maken u een data factory met een pijplijn om gegevens te kopiëren van Blob-opslag naar SQL database.

Met de kopieeractiviteit wordt de gegevensverplaatsing in Azure Data Factory uitgevoerd. De activiteit wordt mogelijk gemaakt door een wereldwijd beschikbare service waarmee gegevens veilig, betrouwbaar en schaalbaar kunnen worden gekopieerd tussen verschillende gegevensarchieven. Zie [Activiteiten voor gegevensverplaatsing](data-factory-data-movement-activities.md) voor meer informatie over de kopieeractiviteit.  

> [!NOTE]
> Zie voor een gedetailleerd overzicht van de Data Factory-service, de [Inleiding tot Azure Data Factory](data-factory-introduction.md) artikel.
>
>

## <a name="prerequisites-for-the-tutorial"></a>Vereisten voor de zelfstudie
Voordat u met deze zelfstudie begint, moet u aan de volgende vereisten voldoen:

* **Azure-abonnement**.  Als u geen abonnement hebt, kunt u binnen een paar minuten een gratis proefaccount maken. Zie de [gratis proefversie](http://azure.microsoft.com/pricing/free-trial/) artikel voor meer informatie.
* **Azure Storage-Account**. U gebruikt de blobopslag als een **bron** gegevens opslaan in deze zelfstudie. Als u geen Azure Storage-account hebt, raadpleegt u het artikel [Een opslagaccount maken](../../storage/common/storage-quickstart-create-account.md) voor de stappen voor het maken van een account.
* **Azure SQL-database**. U gebruikt een Azure SQL-database als een **bestemming** gegevens opslaan in deze zelfstudie. Als u hebt een Azure SQL-database die u in de zelfstudie, Zie gebruiken kunt [maken en configureren van een Azure SQL Database](../../sql-database/sql-database-get-started.md) een te maken.
* **SQL Server 2012/2014 of Visual Studio 2013**. U SQL Server Management Studio of Visual Studio gebruiken om te maken van een voorbeelddatabase en om de resulterende gegevens in de database weer te geven.  

## <a name="collect-blob-storage-account-name-and-key"></a>Verzamelen van blob storage-accountnaam en -sleutel
U moet de accountnaam en accountsleutel van uw Azure storage-account te doen in deze zelfstudie. Noteer de **accountnaam** en **accountsleutel** voor uw Azure storage-account.

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Klik op **alle services** op het menu aan de linkerkant en selecteer **Opslagaccounts**.

    ![Bladeren - Storage-accounts](media/data-factory-copy-data-from-azure-blob-storage-to-sql-database/browse-storage-accounts.png)
3. In de **Opslagaccounts** blade, selecteer de **Azure storage-account** die u wilt gebruiken in deze zelfstudie.
4. Selecteer **toegangssleutels** koppeling onder **instellingen**.
5. Klik op **kopie** (afbeelding)-knop naast **opslagaccountnaam** tekst vak en opslaan en ergens plakken (bijvoorbeeld: in een tekstbestand).
6. Herhaal de vorige stap om te kopiëren of noteer de **key1**.

    ![Toegangssleutel voor opslag](media/data-factory-copy-data-from-azure-blob-storage-to-sql-database/storage-access-key.png)
7. Alle blades te sluiten door te klikken op **X**.

## <a name="collect-sql-server-database-user-names"></a>SQL server, database, gebruikersnamen verzamelen
Moet u de namen van de Azure SQL-server, database en gebruiker in deze zelfstudie. Noteer de namen van **server**, **database**, en **gebruiker** voor uw Azure SQL-database.

1. In de **Azure-portal**, klikt u op **alle services** aan de linkerkant en selecteer **SQL-databases**.
2. In de **blade SQL-databases**, selecteer de **database** die u wilt gebruiken in deze zelfstudie. Noteer de **databasenaam**.  
3. In de **SQL-database** blade, klikt u op **eigenschappen** onder **instellingen**.
4. Noteer de waarden voor **servernaam** en **SERVERBEHEERDER**.
5. Alle blades te sluiten door te klikken op **X**.

## <a name="allow-azure-services-to-access-sql-server"></a>Azure-services voor toegang tot SQL server toestaan
Zorg ervoor dat **toegang tot Azure-services toestaan** instelling ingeschakeld **ON** voor uw Azure SQL-server zodat de Data Factory-service toegang heeft tot uw Azure SQL-server. Voer de volgende stappen uit om dit te controleren en de instelling in te schakelen:

1. Klik op **alle services** hub aan de linkerkant en klikt u op **SQL-servers**.
2. Selecteer uw server en klik op **Firewall** onder **INSTELLINGEN**.
3. In de blade **Firewallinstellingen**schakelt u **Toegang tot Azure-services toestaan** **in**.
4. Alle blades te sluiten door te klikken op **X**.

## <a name="prepare-blob-storage-and-sql-database"></a>Blob-opslag en SQL-Database voorbereiden
Voorbereiden nu uw Azure-blobopslag en Azure SQL-database voor de zelfstudie door de volgende stappen uit:  

1. Start Kladblok. Kopieer de volgende tekst en sla het bestand als **emp.txt** naar **C:\ADFGetStarted** map op uw harde schijf.

    ```
    John, Doe
    Jane, Doe
    ```
2. Gebruik hulpprogramma's zoals [Azure Opslagverkenner](http://storageexplorer.com/) om de container **adftutorial** te maken en om het bestand **emp.txt** te uploaden naar de container.

    ![Azure Storage Explorer. Gegevens kopiëren van Blob storage naar SQL database](./media/data-factory-copy-data-from-azure-blob-storage-to-sql-database/getstarted-storage-explorer.png)
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

    **Als u SQL Server 2012/2014 op uw computer geïnstalleerd hebt:** Volg de instructies uit [beherende Azure SQL Database met behulp van SQL Server Management Studio](../../sql-database/sql-database-manage-azure-ssms.md) verbinding maken met uw Azure SQL-server en de SQL-script uitvoeren. 

    Als de client geen toegang heeft tot de Azure SQL-server, moet u de firewall configureren voor uw Azure SQL-server zodat toegang vanaf uw apparaat (IP-adres) wordt toegestaan. Raadpleeg [dit artikel](../../sql-database/sql-database-configure-firewall-settings.md) voor stappen waarmee u uw firewall kunt configureren voor uw Azure SQL-server.

## <a name="create-a-data-factory"></a>Een gegevensfactory maken
U kunt de vereisten hebt voltooid. U kunt een data factory maken met een van de volgende manieren. Klik op een van de opties in de vervolgkeuzelijst aan de bovenkant of de volgende koppelingen om uit te voeren van de zelfstudie.     

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
