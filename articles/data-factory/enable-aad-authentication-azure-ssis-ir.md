---
title: Azure Active Directory-verificatie inschakelen voor de integratie van Azure SSIS runtime | Microsoft Docs
description: Dit artikel wordt beschreven hoe u configureert de runtime Azure SSIS-integratie voor verbindingen die gebruikmaken van Azure Active Directory-verificatie inschakelen.
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
ms.openlocfilehash: 93d3e25957fb1f04400fa78423a5658d32f7d5fd
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/25/2018
ms.locfileid: "36749715"
---
# <a name="enable-azure-active-directory-authentication-for-the-azure-ssis-integration-runtime"></a>Azure Active Directory-verificatie inschakelen voor de integratie van Azure SSIS runtime

In dit artikel leest u hoe een Azure-SSIS-IR maken met Azure Data Factory-service-identiteit. Verificatie van Azure Active Directory (Azure AD) met de beheerde Service identiteit (MSI) voor de runtime Azure SSIS-integratie kunt u de Data Factory-MSI in plaats van de SQL-verificatie voor het maken van een Azure-SSIS-integratie-runtime gebruiken.

Zie voor meer informatie over de Data Factory-MSI [Azure Data Factory-service-identiteit](https://docs.microsoft.com/en-us/azure/data-factory/data-factory-service-identity).

> [!NOTE]
> Als u al een runtime Azure SSIS-integratie met SQL-verificatie hebt gemaakt, kunt u de IR voor het gebruik van Azure AD-verificatie met PowerShell op dit moment niet opnieuw configureren.

## <a name="create-a-group-in-azure-ad-and-make-the-data-factory-msi-a-member-of-the-group"></a>Een groep maken in Azure AD en maakt u de Data Factory-MSI lid van de groep

U kunt een bestaande Azure AD-groep of maak een nieuwe Azure AD PowerShell gebruiken.

1.  Installeer de [Azure AD PowerShell](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2) module.

2.  Meld u aan met `Connect-AzureAD`, en voer de volgende opdracht om de groep te maken en opslaan in een variabele:

    ```powershell
    $Group = New-AzureADGroup -DisplayName "SSISIrGroup" `
                              -MailEnabled $false `
                              -SecurityEnabled $true `
                              -MailNickName "NotSet"
    ```

    De uitvoer ziet er het volgende voorbeeld, dat ook de waarde van de variabele moet worden gecontroleerd:

    ```powershell
    $Group

    ObjectId DisplayName Description
    -------- ----------- -----------
    6de75f3c-8b2f-4bf4-b9f8-78cc60a18050 SSISIrGroup
    ```

3.  De Data Factory-MSI toevoegen aan de groep. U kunt volgen [Azure Data Factory-service-identiteit](https://docs.microsoft.com/en-us/azure/data-factory/data-factory-service-identity) ophalen van de principal-IDENTITY-ID (bijvoorbeeld 765ad4ab-XXXX-XXXX-XXXX-51ed985819dc, maar gebruik geen SERVICE-identiteit TOEPASSINGS-ID voor dit doel).

    ```powershell
    Add-AzureAdGroupMember -ObjectId $Group.ObjectId -RefObjectId 765ad4ab-XXXX-XXXX-XXXX-51ed985819dc
    ```

    Ook kunt u bestuderen het groepslidmaatschap later.

    ```powershell
    Get-AzureAdGroupMember -ObjectId $Group.ObjectId
    ```

## <a name="enable-azure-ad-on-azure-sql-database"></a>Azure AD op Azure SQL Database inschakelen

Azure SQL Database ondersteunt het maken van een database met een Azure AD-gebruiker. Als gevolg hiervan kunt u een Azure AD-gebruiker instellen als de Active Directory-beheerder en vervolgens weer aanmelden bij SSMS met behulp van de Azure AD-gebruiker. Vervolgens kunt u een contained-gebruiker voor de Azure AD-groep zodat de IR voor het maken van de SQL Server Integration Services (SSIS)-catalogus op de server maken.

### <a name="enable-azure-ad-authentication-for-the-azure-sql-database"></a>Azure AD-verificatie inschakelen voor de Azure SQL Database

U kunt [Azure AD authentication configureren voor de SQL-Database](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-aad-authentication-configure) met behulp van de volgende stappen uit:

1.  Selecteer in de Azure-portal **alle services** -> **SQL-servers** van de linkernavigatiebalk.

2.  Selecteer de SQL-Database worden ingeschakeld voor Azure AD-verificatie.

3.  In de **instellingen** sectie van de blade Selecteer **Active Directory-beheerder**.

4.  Selecteer in de opdrachtbalk **beheerder instellen**.

5.  Selecteer een Azure AD-gebruikersaccount moet een beheerder van de server worden gemaakt en selecteer vervolgens **selecteren.**

6.  Selecteer in de opdrachtbalk **opslaan.**

### <a name="create-a-contained-user-in-the-database-that-represents-the-azure-ad-group"></a>Maak een contained-gebruiker in de database met de Azure AD-groep

Voor deze stap, moet u [Microsoft SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) (SSMS).

1.  Start SQL Server Management Studio.

2.  In de **verbinding maken met Server** dialoogvenster, voer de naam van uw SQL-server in de **servernaam** veld.

3.  In de **verificatie** optie **Active Directory - Universal met ondersteuning voor MFA**. (U kunt ook andere twee typen van de Active Directory-verificatie gebruiken. Zie [configureren en beheren van Azure Active Directory-verificatie met SQL-Database beheerd exemplaar](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-aad-authentication-configure).)

4.  In de **gebruikersnaam** en voer de naam van de Azure AD-account die u hebt ingesteld als de beheerder van de server - bijvoorbeeld testuser@xxxonline.com.

5.  Selecteer **Connect**. Voltooi de aanmelding.

6.  In de **Objectverkenner**, vouw de **Databases** -> systeemdatabases map.

7.  Rechts selecteren op **master** database en selecteer **nieuwe query**.

8.  Voer de volgende regel in het queryvenster en selecteer **Execute** op de werkbalk:

    ```sql
    CREATE USER [SSISIrGroup] FROM EXTERNAL PROVIDER
    ```

    De opdracht is voltooid, de gebruiker van de opgenomen voor de groep maken.

9.  De queryvenster gewist, voer de volgende regel en selecteer **Execute** op de werkbalk:

    ```sql
    ALTER ROLE dbmanager ADD MEMBER [SSISIrGroup]
    ```

    De opdracht is voltooid, de mogelijkheid om database te maken van de contained-gebruiker verlenen.

## <a name="enable-azure-ad-on-azure-sql-database-managed-instance"></a>Azure AD op beheerde Azure SQL Database-exemplaar inschakelen

Azure SQL Database beheerd-instantie biedt geen ondersteuning voor het maken van een database met een Azure AD-gebruiker dan AD-beheerder. Daardoor hebt u de Azure AD-groep instellen als de Active Directory-beheerder. U hoeft niet te maken van de gebruiker opgenomen.

U kunt [Azure AD authentication configureren voor de beheerde exemplaar van SQL Database-server](https://docs.microsoft.com/en-us/azure/sql-database/sql-database-aad-authentication-configure) met behulp van de volgende stappen uit:

7.  Selecteer in de Azure-portal **alle services** -> **SQL-servers** van de linkernavigatiebalk.

8.  Selecteer de SQL-server worden ingeschakeld voor Azure AD-verificatie.

9.  In de **instellingen** sectie van de blade Selecteer **Active Directory-beheerder**.

10. Selecteer in de opdrachtbalk **beheerder instellen**.

11. Zoek en selecteer de Azure AD-groep (bijvoorbeeld SSISIrGroup) en selecteer **selecteren.**

12. Selecteer in de opdrachtbalk **opslaan.**

## <a name="provision-the-azure-ssis-ir-in-the-portal"></a>Inrichten van de Azure-SSIS-IR in de portal

Wanneer u uw Azure-SSIS-IR met de Azure-portal te richten op de **SQL-instellingen** pagina controle van de ' Gebruik AAD-verificatie met uw ADF MSI ' optie. (De volgende schermafbeelding ziet u de instellingen voor IR met Azure SQL Database. Voor de IR met beheerde exemplaar is de eigenschap 'Laag catalogus Database Service' niet beschikbaar. andere instellingen zijn hetzelfde.)

Zie voor meer informatie over het maken van een Azure-SSIS-integratie runtime [maken van een Azure-SSIS-integratie runtime in Azure Data Factory](https://docs.microsoft.com/en-us/azure/data-factory/create-azure-ssis-integration-runtime).

![Instellingen voor de integratie van Azure SSIS runtime](media/enable-aad-authentication-azure-ssis-ir/enable-aad-authentication.png)

## <a name="provision-the-azure-ssis-ir-with-powershell"></a>Inrichten van de Azure-SSIS-IR met PowerShell

Voor het inrichten van uw Azure-SSIS-IR met PowerShell het volgende doen:

1.  Installeer de [Azure PowerShell](https://github.com/Azure/azure-powershell/releases/tag/v5.5.0-March2018) module.

2.  In het script, stel de *CatalogAdminCredential* parameter. Bijvoorbeeld:

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
