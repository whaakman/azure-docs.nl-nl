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
ms.date: 06/21/2018
ms.author: douglasl
ms.openlocfilehash: 234fb5af55565602d283539c63076adebad1ed25
ms.sourcegitcommit: 609c85e433150e7c27abd3b373d56ee9cf95179a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/03/2018
ms.locfileid: "48248972"
---
# <a name="enable-azure-active-directory-authentication-for-the-azure-ssis-integration-runtime"></a>Azure Active Directory-verificatie inschakelen voor de Azure-SSIS integratieruntime

In dit artikel wordt beschreven hoe u een Azure-SSIS IR maken met Azure Data Factory-service-identiteit. Verificatie van Azure Active Directory (Azure AD) met de beheerde identiteit voor Azure-resources voor de Azure-SSIS integratieruntime kunt u het Data Factory-MSI-bestand in plaats van SQL-verificatie gebruiken voor het maken van een Azure-SSIS integratieruntime.

Zie voor meer informatie over de Data Factory-MSI [Azure Data Factory-service-identiteit](https://docs.microsoft.com/azure/data-factory/data-factory-service-identity).

> [!NOTE]
> Als u al een Azure-SSIS integratieruntime met SQL-verificatie hebt gemaakt, kunt u de IR voor het gebruik van Azure AD-verificatie met PowerShell op dit moment niet opnieuw configureren.

## <a name="create-a-group-in-azure-ad-and-make-the-data-factory-msi-a-member-of-the-group"></a>Een groep maken in Azure AD en het Data Factory-MSI-bestand een lid van de groep maken

U kunt een bestaande Azure AD-groep gebruiken of een nieuwe groep maken met behulp van Azure AD PowerShell.

1.  Installeer de [Azure AD PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2) module.

2.  Meld u aan met behulp van `Connect-AzureAD`, en voer de volgende opdracht uit om de groep te maken en opslaan in een variabele:

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

3.  Het Data Factory-MSI-bestand toevoegen aan de groep. U kunt volgen [Azure Data Factory-service-identiteit](https://docs.microsoft.com/azure/data-factory/data-factory-service-identity) om op te halen van de principal-SERVICE-identiteit-ID (bijvoorbeeld 765ad4ab-XXXX-XXXX-XXXX-51ed985819dc, maar gebruik geen SERVICE-identiteit TOEPASSINGS-ID voor dit doel).

    ```powershell
    Add-AzureAdGroupMember -ObjectId $Group.ObjectId -RefObjectId 765ad4ab-XXXX-XXXX-XXXX-51ed985819dc
    ```

    Ook kunt u bestuderen het groepslidmaatschap later.

    ```powershell
    Get-AzureAdGroupMember -ObjectId $Group.ObjectId
    ```

## <a name="enable-azure-ad-on-azure-sql-database"></a>Azure AD in Azure SQL Database inschakelen

Azure SQL Database ondersteunt het maken van een database met een Azure AD-gebruiker. Als gevolg hiervan kunt u een Azure AD-gebruiker instellen als de Active Directory-beheerder en vervolgens Meld u aan bij SSMS met behulp van de Azure AD-gebruiker. Vervolgens maakt u een contained-gebruiker voor de Azure AD-groep om in te schakelen van de IR te maken van de catalogus van SQL Server Integration Services (SSIS) op de server.

### <a name="enable-azure-ad-authentication-for-the-azure-sql-database"></a>Azure AD-verificatie inschakelen voor de Azure SQL Database

U kunt [Azure AD-verificatie voor de SQL-Database configureren](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication-configure) met behulp van de volgende stappen uit:

1.  Selecteer in de Azure portal, **alle services** -> **SQL-servers** in de navigatie aan de linkerkant.

2.  Selecteer de SQL-Database wordt ingeschakeld voor Azure AD-verificatie.

3.  In de **instellingen** sectie van de blade, selecteer **Active Directory-beheerder**.

4.  Selecteer in de opdrachtbalk **beheerder instellen**.

5.  Selecteer een Azure AD-gebruikersaccount moet een beheerder van de server worden gemaakt en selecteer vervolgens **selecteren.**

6.  Selecteer in de opdrachtbalk **opslaan.**

### <a name="create-a-contained-user-in-the-database-that-represents-the-azure-ad-group"></a>Een ingesloten gebruiker maken in de database die staat voor de Azure AD-groep

Voor deze stap moet u [Microsoft SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) (SSMS).

1.  Start SQL Server Management Studio.

2.  In de **verbinding maken met Server** dialoogvenster, geef de naam van uw SQL-server in de **servernaam** veld.

3.  Selecteer in het veld **Verificatie** **Active Directory - Universeel met ondersteuning voor MFA**. (U kunt ook andere twee typen van de Active Directory-verificatie gebruiken. Zie [configureren en beheren van Azure Active Directory-verificatie met SQL Database Managed Instance](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication-configure).)

4.  In de **gebruikersnaam** en voer de naam van de Azure AD-account die u hebt ingesteld als de beheerder van de server - bijvoorbeeld testuser@xxxonline.com.

5.  Selecteer **verbinding**. Voltooi het aanmeldingsproces.

6.  In de **Objectverkenner**, vouw de **Databases** -> folder systeemdatabases.

7.  Selecteer met de rechtermuisknop op **master** database en selecteer **nieuwe query**.

8.  Voer in het queryvenster de volgende regel en selecteer **Execute** in de werkbalk:

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

Azure SQL Database Managed Instance biedt geen ondersteuning voor het maken van een database met een Azure AD-gebruiker dan AD-beheerder. Als gevolg hiervan, hebt u het instellen van de Azure AD-groep als de Active Directory-beheerder. U hoeft te maken van de contained-gebruiker.

U kunt [Azure AD-verificatie voor de SQL Database Managed Instance-server configureren](https://docs.microsoft.com/azure/sql-database/sql-database-aad-authentication-configure) met behulp van de volgende stappen uit:

7.  Selecteer in de Azure portal, **alle services** -> **SQL-servers** in de navigatie aan de linkerkant.

8.  Selecteer de SQL-server worden ingeschakeld voor Azure AD-verificatie.

9.  In de **instellingen** sectie van de blade, selecteer **Active Directory-beheerder**.

10. Selecteer in de opdrachtbalk **beheerder instellen**.

11. Zoek en selecteer de Azure AD-groep (bijvoorbeeld SSISIrGroup) en selecteer **selecteren.**

12. Selecteer in de opdrachtbalk **opslaan.**

## <a name="provision-the-azure-ssis-ir-in-the-portal"></a>Inrichten van de Azure-SSIS-IR in de portal

Wanneer u uw Azure-SSIS IR met de Azure-portal te richten op de **SQL-instellingen** pagina, Controleer de ' Gebruik AAD-verificatie met uw MSI ADF "optie. (De volgende schermafbeelding ziet u de instellingen voor IR met Azure SQL Database. Voor de IR met Managed Instance is de eigenschap 'Catalog Database-servicelaag' niet beschikbaar. andere instellingen zijn hetzelfde.)

Zie voor meer informatie over het maken van een Azure-SSIS integratieruntime [maken van een Azure-SSIS integratieruntime in Azure Data Factory](https://docs.microsoft.com/azure/data-factory/create-azure-ssis-integration-runtime).

![Instellingen voor de Azure-SSIS integratieruntime](media/enable-aad-authentication-azure-ssis-ir/enable-aad-authentication.png)

## <a name="provision-the-azure-ssis-ir-with-powershell"></a>Inrichten van de Azure-SSIS IR met PowerShell

Voor het inrichten van uw Azure-SSIS IR met PowerShell, moet u de volgende dingen doen:

1.  Installeer de [Azure PowerShell](https://github.com/Azure/azure-powershell/releases/tag/v5.5.0-March2018) module.

2.  In het script niet instelt de *CatalogAdminCredential* parameter. Bijvoorbeeld:

    ```powershell
    Set-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                               -DataFactoryName $DataFactoryName `
                                               -Name $AzureSSISName `
                                               -Type Managed `
                                               -CatalogServerEndpoint $SSISDBServerEndpoint `
                                               -CatalogPricingTier $SSISDBPricingTier `
                                               -Description $AzureSSISDescription `
                                               -Edition $AzureSSISEdition `
                                               -Location $AzureSSISLocation `
                                               -NodeSize $AzureSSISNodeSize `
                                               -NodeCount $AzureSSISNodeNumber `
                                               -MaxParallelExecutionsPerNode $AzureSSISMaxParallelExecutionsPerNode `
                                               -SetupScriptContainerSasUri $SetupScriptContainerSasUri

    Start-AzureRmDataFactoryV2IntegrationRuntime -ResourceGroupName $ResourceGroupName `
                                                 -DataFactoryName $DataFactoryName `
                                                 -Name $AzureSSISName
   ```
