---
title: 'PowerShell en CLI: SQL TDE - uw sleutel - Azure SQL Database inschakelen | Microsoft Docs'
description: Informatie over het configureren van een Azure SQL Database en datawarehouse voor het starten van transparante gegevensversleuteling (TDE) gebruiken voor versleuteling-at-rest met behulp van PowerShell of CLI.
services: sql-database
keywords: ''
documentationcenter: ''
author: aliceku
manager: craigg
editor: ''
ms.prod: ''
ms.reviewer: vanto
ms.suite: sql
ms.prod_service: sql-database, sql-data-warehouse
ms.service: sql-database
ms.tgt_pltfrm: ''
ms.devlang: azurecli, powershell
ms.topic: conceptual
ms.date: 09/20/2018
ms.author: aliceku
monikerRange: = azuresqldb-current || = azure-sqldw-latest || = sqlallproducts-allversions
ms.openlocfilehash: 65f9384bb0ceedd6c2bde8ef513040e198ccc4d2
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47036368"
---
# <a name="powershell-and-cli-enable-transparent-data-encryption-using-your-own-key-from-azure-key-vault"></a>PowerShell en CLI: Transparent Data Encryption met behulp van uw eigen sleutel uit Azure Key Vault inschakelen

Dit artikel helpt bij het gebruik van een sleutel uit Azure Key Vault voor transparante gegevensversleuteling (TDE) op een SQL-Database of het datawarehouse. Voor meer informatie over de TDE met Bring Your Own Key (BYOK) ondersteuning, gaat u naar [TDE Bring Your Own Key to Azure SQL](transparent-data-encryption-byok-azure-sql.md). 

## <a name="prerequisites-for-powershell"></a>Vereisten voor PowerShell

- U moet een Azure-abonnement hebt en een beheerder zijn op dat aan het abonnement.
- [Aanbevolen maar niet vereist] Een hardware security module (HSM) of de lokale sleutel opslaan voor het maken van een lokale kopie van het sleutelmateriaal TDE-beveiliging hebben.
- U moet Azure PowerShell versie 4.2.0 of hoger zijn geïnstalleerd en uitgevoerd. 
- Maak een Azure Key Vault en de sleutel moet worden gebruikt voor TDE.
   - [PowerShell-instructies uit Key Vault](../key-vault/key-vault-get-started.md)
   - [Instructies voor het gebruik van een hardware security module (HSM) en Key Vault](../key-vault/key-vault-get-started.md#HSM)
 - De key vault moet beschikken over de volgende eigenschap moet worden gebruikt voor TDE:
   - [voorlopig verwijderen](../key-vault/key-vault-ovw-soft-delete.md)
   - [De Key Vault-functie voor voorlopig verwijderen gebruiken met PowerShell](../key-vault/key-vault-soft-delete-powershell.md) 
- De sleutel moet de volgende kenmerken moet worden gebruikt voor TDE hebben:
   - Er is geen vervaldatum
   - Niet uitgeschakeld
   - Kan uitvoeren *ophalen*, *sleutel inpakken*, *sleutel uitpakken* bewerkingen

## <a name="step-1-assign-an-azure-ad-identity-to-your-server"></a>Step 1. Een Azure AD-identiteit toewijzen aan uw server 

Als u een bestaande server hebt, gebruikt u de volgende in een Azure AD-identiteit toevoegen aan uw server:

   ```powershell
   $server = Set-AzureRmSqlServer `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> `
   -AssignIdentity
   ```

Als u een server maakt, gebruikt u de [New-AzureRmSqlServer](/powershell/module/azurerm.sql/new-azurermsqlserver) cmdlet met de tag-identiteit van een Azure AD-identiteit toevoegen tijdens het maken van de server:

   ```powershell
   $server = New-AzureRmSqlServer `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -Location <RegionName> `
   -ServerName <LogicalServerName> `
   -ServerVersion "12.0" `
   -SqlAdministratorCredentials <PSCredential> `
   -AssignIdentity 
   ```

## <a name="step-2-grant-key-vault-permissions-to-your-server"></a>Stap 2. Key Vault-machtigingen verlenen aan uw server

Gebruik de [Set-AzureRmKeyVaultAccessPolicy](/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy) cmdlet uw servertoegang verlenen tot de sleutel voordat u een sleutel van het voor TDE-kluis.

   ```powershell
   Set-AzureRmKeyVaultAccessPolicy  `
   -VaultName <KeyVaultName> `
   -ObjectId $server.Identity.PrincipalId `
   -PermissionsToKeys get, wrapKey, unwrapKey
   ```

## <a name="step-3-add-the-key-vault-key-to-the-server-and-set-the-tde-protector"></a>Stap 3. De Key Vault-sleutel toevoegen aan de server en stel de TDE-beveiliging

- Gebruik de [toevoegen AzureRmSqlServerKeyVaultKey](/powershell/module/azurerm.sql/add-azurermsqlserverkeyvaultkey) cmdlet om toe te voegen van de sleutel van de Key Vault met de server.
- Gebruik de [Set-azurermsqlservertransparentdataencryptionprotector geeft](/powershell/module/azurerm.sql/set-azurermsqlservertransparentdataencryptionprotector) cmdlet om in te stellen van de sleutel als de TDE-beveiliging voor alle serverresources.
- Gebruik de [Get-azurermsqlservertransparentdataencryptionprotector geeft](/powershell/module/azurerm.sql/get-azurermsqlservertransparentdataencryptionprotector) cmdlet om te bevestigen dat de TDE-beveiliging is geconfigureerd zoals bedoeld.

> [!Note]
> De gecombineerde lengte voor de key vault-naam en de naam mag maximaal 94 tekens bevatten.
> 

>[!Tip]
>Een voorbeeld KeyId uit Key Vault: https://contosokeyvault.vault.azure.net/keys/Key1/1a1a2b2b3c3c4d4d5e5e6f6f7g7g8h8h
>

   ```powershell
   <# Add the key from Key Vault to the server #>
   Add-AzureRmSqlServerKeyVaultKey `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> `
   -KeyId <KeyVaultKeyId>

   <# Set the key as the TDE protector for all resources under the server #>
   Set-AzureRmSqlServerTransparentDataEncryptionProtector `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> `
   -Type AzureKeyVault `
   -KeyId <KeyVaultKeyId> 

   <# To confirm that the TDE protector was configured as intended: #>
   Get-AzureRmSqlServerTransparentDataEncryptionProtector `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> 
   ```

## <a name="step-4-turn-on-tde"></a>Stap 4. TDE inschakelen 

Gebruik de [Set AzureRMSqlDatabaseTransparentDataEncryption](/powershell/module/azurerm.sql/set-azurermsqldatabasetransparentdataencryption) cmdlet TDE inschakelen.

   ```powershell
   Set-AzureRMSqlDatabaseTransparentDataEncryption `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> `
   -DatabaseName <DatabaseName> `
   -State "Enabled"
   ```

De database of het datawarehouse is nu TDE is ingeschakeld met een versleutelingssleutel in Key Vault.

## <a name="step-5-check-the-encryption-state-and-encryption-activity"></a>Stap 5. Controleer de status van de versleuteling en versleuteling activiteit

Gebruik de [Get-AzureRMSqlDatabaseTransparentDataEncryption](/powershell/module/azurerm.sql/get-azurermsqldatabasetransparentdataencryption) om op te halen van de status van de versleuteling en de [Get-AzureRMSqlDatabaseTransparentDataEncryptionActivity](/powershell/module/azurerm.sql/get-azurermsqldatabasetransparentdataencryptionactivity) om te controleren of de voortgang van de versleuteling voor een database of het datawarehouse.

   ```powershell
   # Get the encryption state
   Get-AzureRMSqlDatabaseTransparentDataEncryption `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> `
   -DatabaseName <DatabaseName> `

   <# Check the encryption progress for a database or data warehouse #>
   Get-AzureRMSqlDatabaseTransparentDataEncryptionActivity `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -ServerName <LogicalServerName> `
   -DatabaseName <DatabaseName>  
   ```

## <a name="other-useful-powershell-cmdlets"></a>Andere nuttige PowerShell-cmdlets

- Gebruik de [Set AzureRMSqlDatabaseTransparentDataEncryption](/powershell/module/azurerm.sql/set-azurermsqldatabasetransparentdataencryption) cmdlet TDE uitschakelen.

   ```powershell
   Set-AzureRMSqlDatabaseTransparentDataEncryption `
   -ServerName <LogicalServerName> `
   -ResourceGroupName <SQLDatabaseResourceGroupName> `
   -DatabaseName <DatabaseName> `
   -State "Disabled”
   ```
 
- Gebruik de [Get-AzureRmSqlServerKeyVaultKey](/powershell/module/azurerm.sql/get-azurermsqlserverkeyvaultkey) cmdlet om te retourneren van de lijst met Key Vault sleutels die zijn toegevoegd aan de server.

   ```powershell
   <# KeyId is an optional parameter, to return a specific key version #>
   Get-AzureRmSqlServerKeyVaultKey `
   -ServerName <LogicalServerName> `
   -ResourceGroupName <SQLDatabaseResourceGroupName>
   ```
 
- Gebruik de [Remove-AzureRmSqlServerKeyVaultKey](/powershell/module/azurerm.sql/remove-azurermsqlserverkeyvaultkey) te verwijderen van een Key Vault-sleutel van de server.

   ```powershell
   <# The key set as the TDE Protector cannot be removed. #>
   Remove-AzureRmSqlServerKeyVaultKey `
   -KeyId <KeyVaultKeyId> `
   -ServerName <LogicalServerName> `
   -ResourceGroupName <SQLDatabaseResourceGroupName>   
   ```
 
## <a name="troubleshooting"></a>Problemen oplossen

Controleer het volgende als er een probleem optreedt:
- Als de key vault kan niet worden gevonden, controleert u of u bent in het juiste abonnement met de [Get-AzureRmSubscription](/powershell/module/azurerm.profile/get-azurermsubscription) cmdlet.

   ```powershell
   Get-AzureRmSubscription `
   -SubscriptionId <SubscriptionId>
   ```

- Als de nieuwe sleutel kan niet worden toegevoegd aan de server of de nieuwe sleutel als de TDE-beveiliging kan niet worden bijgewerkt, controleert u het volgende:
   - De sleutel mag geen hebben een vervaldatum
   - De sleutel moet de *ophalen*, *sleutel inpakken*, en *sleutel uitpakken* bewerkingen die zijn ingeschakeld.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het draaien van de TDE-beveiliging van een server om te voldoen aan de beveiligingsvereisten: [draaien de Transparent Data Encryption protector met behulp van PowerShell](transparent-data-encryption-byok-azure-sql-key-rotation.md).
- In het geval van een veiligheidsrisico inhouden, informatie over het verwijderen van een potentieel aangetast TDE-beveiliging: [een potentieel aangetast sleutel verwijderen](transparent-data-encryption-byok-azure-sql-remove-tde-protector.md). 

## <a name="prerequisites-for-cli"></a>Vereisten voor de CLI

- U moet een Azure-abonnement hebt en een beheerder zijn op dat aan het abonnement.
- [Aanbevolen maar niet vereist] Een hardware security module (HSM) of de lokale sleutel opslaan voor het maken van een lokale kopie van het sleutelmateriaal TDE-beveiliging hebben.
- Opdrachtregelinterface versie 2.0 of hoger. Zie voor het installeren van de meest recente versie en verbinding maken met uw Azure-abonnement, [installeren en configureren van de Azure platformoverschrijdende opdrachtregelinterface 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). 
- Maak een Azure Key Vault en de sleutel moet worden gebruikt voor TDE.
   - [Beheren van Key Vault met behulp van CLI 2.0](../key-vault/key-vault-manage-with-cli2.md)
   - [Instructies voor het gebruik van een hardware security module (HSM) en Key Vault](../key-vault/key-vault-get-started.md#HSM)
 - De key vault moet beschikken over de volgende eigenschap moet worden gebruikt voor TDE:
   - [voorlopig verwijderen](../key-vault/key-vault-ovw-soft-delete.md)
   - [De Key Vault-functie voor voorlopig verwijderen gebruiken met CLI](../key-vault/key-vault-soft-delete-cli.md) 
- De sleutel moet de volgende kenmerken moet worden gebruikt voor TDE hebben:
   - Er is geen vervaldatum
   - Niet uitgeschakeld
   - Kan uitvoeren *ophalen*, *sleutel inpakken*, *sleutel uitpakken* bewerkingen
   
## <a name="step-1-create-a-server-and-assign-an-azure-ad-identity-to-your-server"></a>Step 1. Een server maken en toewijzen van een Azure AD-identiteit aan uw server
      cli
      # create server (with identity) and database
      az sql server create -n "ServerName" -g "ResourceGroupName" -l "westus" -u "cloudsa" -p "YourFavoritePassWord99@34" -I 
      az sql db create -n "DatabaseName" -g "ResourceGroupName" -s "ServerName" 
      

 
## <a name="step-2-grant-key-vault-permissions-to-your-server"></a>Stap 2. Key Vault-machtigingen verlenen aan uw server
      cli
      # create key vault, key and grant permission
      az keyvault create -n "VaultName" -g "ResourceGroupName" 
      az keyvault key create -n myKey -p software --vault-name "VaultName" 
      az keyvault set-policy -n "VaultName" --object-id "ServerIdentityObjectId" -g "ResourceGroupName" --key-permissions wrapKey unwrapKey get list 
      

 
## <a name="step-3-add-the-key-vault-key-to-the-server-and-set-the-tde-protector"></a>Stap 3. De Key Vault-sleutel toevoegen aan de server en stel de TDE-beveiliging
  
     cli
     # add server key and update encryption protector
      az sql server key create -g "ResourceGroupName" -s "ServerName" -t "AzureKeyVault" -u "FullVersionedKeyUri 
      az sql server tde-key update -g "ResourceGroupName" -s "ServerName" -t AzureKeyVault -u "FullVersionedKeyUri" 
      
  
  > [!Note]
> De gecombineerde lengte voor de key vault-naam en de naam mag maximaal 94 tekens bevatten.
> 

>[!Tip]
>Een voorbeeld KeyId uit Key Vault: https://contosokeyvault.vault.azure.net/keys/Key1/1a1a2b2b3c3c4d4d5e5e6f6f7g7g8h8h
>
  
## <a name="step-4-turn-on-tde"></a>Stap 4. TDE inschakelen 
      cli
      # enable encryption
      az sql db tde create -n "DatabaseName" -g "ResourceGroupName" -s "ServerName" --status Enabled 
      

De database of het datawarehouse is nu TDE is ingeschakeld met een versleutelingssleutel in Key Vault.

## <a name="step-5-check-the-encryption-state-and-encryption-activity"></a>Stap 5. Controleer de status van de versleuteling en versleuteling activiteit

     cli
      # get encryption scan progress
      az sql db tde show-activity -n "DatabaseName" -g "ResourceGroupName" -s "ServerName" 

      # get whether encryption is on or off
      az sql db tde show-configuration -n "DatabaseName" -g "ResourceGroupName" -s "ServerName" 

## <a name="sql-cli-references"></a>Verwijzingen van de SQL-CLI

https://docs.microsoft.com/cli/azure/sql?view=azure-cli-latest 

https://docs.microsoft.com/cli/azure/sql/server/key?view=azure-cli-latest 

https://docs.microsoft.com/cli/azure/sql/server/tde-key?view=azure-cli-latest 

https://docs.microsoft.com/cli/azure/sql/db/tde?view=azure-cli-latest 

