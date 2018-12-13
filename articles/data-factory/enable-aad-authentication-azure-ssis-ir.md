---
title: Azure Active Directory-verificatie inschakelen voor de Azure-SSIS integratieruntime | Microsoft Docs
description: In dit artikel wordt beschreven hoe u de Azure-SSIS integratieruntime om in te schakelen verbindingen die gebruikmaken van Azure Active Directory-verificatie configureren.
services: data-factory
documentationcenter: ''
author: douglaslMS
manager: craigg
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: ''
ms.devlang: powershell
ms.topic: conceptual
ms.date: 12/11/2018
ms.author: douglasl
ms.openlocfilehash: d2000e626166304e92556e3c965df175a27046ad
ms.sourcegitcommit: e37fa6e4eb6dbf8d60178c877d135a63ac449076
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2018
ms.locfileid: "53321064"
---
# <a name="enable-azure-active-directory-authentication-for-the-azure-ssis-integration-runtime"></a>Azure Active Directory-verificatie inschakelen voor de Azure-SSIS integratieruntime

In dit artikel wordt beschreven hoe u een Azure-SSIS IR maken met Azure Data Factory-service-identiteit. U kunt verificatie van Azure Active Directory (Azure AD) met de beheerde identiteit voor uw Azure Data Factory in plaats van SQL-verificatie gebruiken om te maken van een Azure-SSIS integratieruntime.

Zie voor meer informatie over de beheerde identiteit voor uw data factory [Azure Data Factory-service-identiteit](https://docs.microsoft.com/azure/data-factory/data-factory-service-identity).

> [!NOTE]
> Als u al een Azure-SSIS integratieruntime met SQL-verificatie hebt gemaakt, kunt u de IR voor het gebruik van Azure AD-verificatie met PowerShell op dit moment niet opnieuw configureren.

## <a name="enable-azure-ad-on-azure-sql-database"></a>Azure AD in Azure SQL Database inschakelen

Azure SQL Database ondersteunt het maken van een database met een Azure AD-gebruiker. Als gevolg hiervan kunt u een Azure AD-gebruiker instellen als de Active Directory-beheerder en vervolgens Meld u aan SQL Server Management Studio (SSMS) hebt met behulp van de Azure AD-gebruiker. Vervolgens maakt u een contained-gebruiker voor de Azure AD-groep om in te schakelen uw IR te maken van de catalogus van SQL Server Integration Services (SSIS) op de server.

### <a name="create-a-group-in-azure-ad-and-make-the-managed-identity-for-your-data-factory-a-member-of-the-group"></a>Een groep maken in Azure AD en een lid van de groep voor de beheerde identiteit voor uw data factory maken

U kunt een bestaande Azure AD-groep gebruiken of een nieuwe groep maken met behulp van Azure AD PowerShell.

1.  Installeer de [Azure AD PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2) module.

2.  Meld u aan met behulp van `Connect-AzureAD`, en voer de volgende opdracht uit om de groep te maken en opslaan in een variabele:

    ```powershell
    $Group = New-AzureADGroup -DisplayName "SSISIrGroup" `
                              -MailEnabled $false `
                              -SecurityEnabled $true `
                              -MailNickName "NotSet"
    ```

    De uitvoer ziet er uit als in het volgende voorbeeld, waarin ook de waarde van de variabele:

    ```powershell
    $Group

    ObjectId DisplayName Description
    -------- ----------- -----------
    6de75f3c-8b2f-4bf4-b9f8-78cc60a18050 SSISIrGroup
    ```

3.  De beheerde identiteit voor uw data factory toevoegen aan de groep. U kunt volgen [Azure Data Factory-service-identiteit](https://docs.microsoft.com/azure/data-factory/data-factory-service-identity) om op te halen van de principal-SERVICE-identiteit-ID (bijvoorbeeld 765ad4ab-XXXX-XXXX-XXXX-51ed985819dc, maar gebruik geen SERVICE-identiteit TOEPASSINGS-ID voor dit doel).

    ```powershell
    Add-AzureAdGroupMember -ObjectId $Group.ObjectId -RefObjectId 765ad4ab-XXXX-XXXX-XXXX-51ed985819dc
    ```

    Ook kunt u bestuderen het groepslidmaatschap later.

    ```powershell
    Get-AzureAdGroupMember -ObjectId $Group.ObjectId
    ```

### <a name="enable-azure-ad-authentication-for-the-azure-sql-database"></a>Azure AD-verificatie inschakelen voor de Azure SQL Database

U kunt [Azure AD-verificatie voor de SQL-Database configureren](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication-configure) met behulp van de volgende stappen uit:

1.  Selecteer in de Azure portal, **alle services** -> **SQL-servers** in de navigatie aan de linkerkant.

2.  Selecteer de SQL-Database wordt ingeschakeld voor Azure AD-verificatie.

3.  In de **instellingen** sectie van de blade, selecteer **Active Directory-beheerder**.

4.  Selecteer in de opdrachtbalk **beheerder instellen**.

5.  Selecteer een Azure AD-gebruikersaccount moet een beheerder van de server worden gemaakt en selecteer vervolgens **selecteren.**

6.  Selecteer in de opdrachtbalk **opslaan.**

### <a name="create-a-contained-user-in-the-database-that-represents-the-azure-ad-group"></a>Een ingesloten gebruiker maken in de database die staat voor de Azure AD-groep

Voor deze stap moet u [Microsoft SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) (SSMS).

1.  Start SQL Server Management Studio.

2.  In de **verbinding maken met Server** dialoogvenster, geef de naam van uw SQL-server in de **servernaam** veld.

3.  In de **verificatie** veld **Active Directory - Universal met ondersteuning voor MFA**. (U kunt ook andere twee typen van de Active Directory-verificatie gebruiken. Zie [configureren en beheren van Azure Active Directory-verificatie met SQL Database Managed Instance](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication-configure).)

4.  In de **gebruikersnaam** en voer de naam van de Azure AD-account die u hebt ingesteld als de beheerder van de server - bijvoorbeeld testuser@xxxonline.com.

5.  Selecteer **Connect**. Voltooi het aanmeldingsproces.

6.  In de **Objectverkenner**, vouw de **Databases** -> folder systeemdatabases.

7.  Selecteer met de rechtermuisknop op **master** database en selecteer **nieuwe query**.

8.  Voer in het queryvenster de volgende regel en selecteer **Execute** in de werkbalk:

    ```sql
    CREATE USER [SSISIrGroup] FROM EXTERNAL PROVIDER
    ```

    De opdracht voor het maken van de ingesloten gebruiker voor de groep wordt uitgevoerd.

9.  Schakelt u het queryvenster, voer de volgende regel in en selecteer **Execute** in de werkbalk:

    ```sql
    ALTER ROLE dbmanager ADD MEMBER [SSISIrGroup]
    ```

    De opdracht voltooid is, verleent de ingesloten gebruikers de mogelijkheid om database te maken.

## <a name="enable-azure-ad-on-azure-sql-database-managed-instance"></a>Azure AD op beheerd exemplaar voor Azure SQL Database inschakelen

Azure SQL Database Managed Instance biedt ondersteuning voor het maken van een database met MSI-bestand rechtstreeks. U hoeft niet te gegevensfactory MSI-bestand toevoegen aan een AD-groep of de contained-gebruiker maken in MI.

### <a name="enable-azure-ad-authentication-for-the-azure-sql-database-managed-instance"></a>Azure AD-verificatie inschakelen voor de Azure SQL Database Managed Instance

1.   Selecteer in de Azure portal, **alle services** -> **SQL-servers** in de navigatie aan de linkerkant.

1.   Selecteer de SQL-server worden ingeschakeld voor Azure AD-verificatie.

1.   In de **instellingen** sectie van de blade, selecteer **Active Directory-beheerder**.

1.   Selecteer in de opdrachtbalk **beheerder instellen**.

1.   Selecteer een Azure AD-gebruikersaccount moet een beheerder van de server worden gemaakt en selecteer vervolgens **Selecteer**.

1.   Selecteer in de opdrachtbalk **opslaan**.

### <a name="add-data-factory-msi-as-a-user-to-the-azure-sql-database-managed-instance"></a>Data factory MSI-bestand als een gebruiker toevoegen aan de Azure SQL Database Managed Instance

1.  Start SQL Server Management Studio.

2.  Aanmelden met een SQL-beheerdersaccount of een Active Directory-beheerdersaccount.

3.  Vouw in Object Explorer de Databases -> map voor systeemdatabases.

4.  Met de rechtermuisknop op de hoofddatabase en selecteer **nieuwe query**.

5.  U kunt het artikel volgen [Azure Data Factory-service-identiteit](data-factory-service-identity.md) om op te halen van de principal-SERVICE-identiteit TOEPASSINGS-ID. (Gebruik niet de ID van de SERVICE-identiteit voor dit doel.)

6.  Voer het volgende script om te converteren van de SERVICE-identiteit TOEPASSINGS-ID in binaire type in het queryvenster:

    ```sql
    DECLARE @applicationId uniqueidentifier = {your service identity application id}
    select CAST(@applicationId AS varbinary)
    ```

7.  U krijgt de waarde van het resultatenvenster.

8.  Schakel het query-venster en voer het volgende script:

    ```sql
    CREATE LOGIN [{MSI name}] FROM EXTERNAL PROVIDER with SID ={your service identity application id in binary type}, TYPE = E
    ALTER SERVER ROLE [dbcreator] ADD MEMBER [{MSI name}]
    ALTER SERVER ROLE [securityadmin] ADD MEMBER [{MSI name}]
    ```

9.  De opdracht is voltooid.

## <a name="provision-the-azure-ssis-ir-in-the-portal"></a>Inrichten van de Azure-SSIS-IR in de portal

Wanneer u uw Azure-SSIS IR met de Azure-portal te richten op de **SQL-instellingen** pagina, Controleer de ' Gebruik AAD-verificatie met de beheerde identiteit voor uw ADF "optie. (De volgende schermafbeelding ziet u de instellingen voor IR met Azure SQL Database. Voor de IR met Managed Instance is de eigenschap 'Catalog Database-servicelaag' niet beschikbaar. andere instellingen zijn hetzelfde.)

Zie voor meer informatie over het maken van een Azure-SSIS integratieruntime [maken van een Azure-SSIS integratieruntime in Azure Data Factory](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime).

![Instellingen voor de Azure-SSIS integratieruntime](media/enable-aad-authentication-azure-ssis-ir/enable-aad-authentication.png)

## <a name="provision-the-azure-ssis-ir-with-powershell"></a>Inrichten van de Azure-SSIS IR met PowerShell

Voor het inrichten van uw Azure-SSIS IR met PowerShell, moet u de volgende dingen doen:

1.  Installeer de [Azure PowerShell](https://github.com/Azure/azure-powershell/releases/tag/v5.5.0-March2018) module.

2.  In het script niet instelt de *CatalogAdminCredential* parameter. Bijvoorbeeld:

    ```powershell
    Set-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                               -DataFactoryName $DataFactoryName `
                                               -Name $AzureSSISName `
                                               -Description $AzureSSISDescription `
                                               -Type Managed `
                                               -Location $AzureSSISLocation `
                                               -NodeSize $AzureSSISNodeSize `
                                               -NodeCount $AzureSSISNodeNumber `
                                               -Edition $AzureSSISEdition `
                                               -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode `
                                               -CatalogServerEndpoint $SSISDBServerEndpoint `
                                               -CatalogPricingTier $SSISDBPricingTier

    Start-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                                 -DataFactoryName $DataFactoryName `
                                                 -Name $AzureSSISName
   ```
