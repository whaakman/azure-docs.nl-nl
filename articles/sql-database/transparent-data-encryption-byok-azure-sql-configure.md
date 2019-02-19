---
title: 'PowerShell en CLI: Schakel SQL TDE - met Azure Key Vault: Breng uw eigen sleutel - Azure SQL Database | Microsoft Docs'
description: Informatie over het configureren van een Azure SQL Database en datawarehouse voor het starten van transparante gegevensversleuteling (TDE) gebruiken voor versleuteling-at-rest met behulp van PowerShell of CLI.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: aliceku
ms.author: aliceku
ms.reviewer: vanto
manager: craigg
ms.date: 02/15/2019
ms.openlocfilehash: f4db9a3424400dcddde82bd55d6d5968e04be179
ms.sourcegitcommit: fcb674cc4e43ac5e4583e0098d06af7b398bd9a9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/18/2019
ms.locfileid: "56340260"
---
# <a name="powershell-and-cli-enable-transparent-data-encryption-with-customer-managed-key-from-azure-key-vault"></a>PowerShell en CLI: Transparent Data Encryption inschakelen met de klant beheerde sleutel uit Azure Key Vault

Dit artikel helpt bij het gebruik van een sleutel uit Azure Key Vault voor transparante gegevensversleuteling (TDE) op een SQL-Database of het datawarehouse. Voor meer informatie over de TDE met integratie van Azure Key Vault - ondersteuning voor Bring Your Own Key (BYOK), gaat u naar [TDE met de klant beheerde sleutels in Azure Key Vault](transparent-data-encryption-byok-azure-sql.md). 

## <a name="prerequisites-for-powershell"></a>Vereisten voor PowerShell

- U moet een Azure-abonnement hebt en een beheerder zijn op dat aan het abonnement.
- [Aanbevolen maar niet vereist] Een hardware security module (HSM) of de lokale sleutel opslaan voor het maken van een lokale kopie van het sleutelmateriaal TDE-beveiliging hebben.
- U moet Azure PowerShell versie 4.2.0 of hoger zijn geïnstalleerd en uitgevoerd. 
- Maak een Azure Key Vault en de sleutel moet worden gebruikt voor TDE.
   - [PowerShell-instructies uit Key Vault](../key-vault/key-vault-overview.md)
   - [Instructies voor het gebruik van een hardware security module (HSM) en Key Vault](../key-vault/key-vault-hsm-protected-keys.md)
 - De key vault moet beschikken over de volgende eigenschap moet worden gebruikt voor TDE:
   - [soft-delete](../key-vault/key-vault-ovw-soft-delete.md)
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

- Meer informatie over de TDE-beveiliging van een server om te voldoen aan de beveiligingsvereisten draaien: [De Transparent Data Encryption protector met behulp van PowerShell draaien](transparent-data-encryption-byok-azure-sql-key-rotation.md).
- Meer informatie over het verwijderen van een potentieel aangetast TDE-beveiliging in het geval van een veiligheidsrisico inhouden: [Verwijderen van een potentieel aangetast sleutel](transparent-data-encryption-byok-azure-sql-remove-tde-protector.md). 

## <a name="prerequisites-for-cli"></a>Vereisten voor de CLI

- U moet een Azure-abonnement hebt en een beheerder zijn op dat aan het abonnement.
- [Aanbevolen maar niet vereist] Een hardware security module (HSM) of de lokale sleutel opslaan voor het maken van een lokale kopie van het sleutelmateriaal TDE-beveiliging hebben.
- Opdrachtregelinterface versie 2.0 of hoger. Zie voor het installeren van de meest recente versie en verbinding maken met uw Azure-abonnement, [installeren en configureren van de Azure platformoverschrijdende opdrachtregelinterface 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest). 
- Maak een Azure Key Vault en de sleutel moet worden gebruikt voor TDE.
   - [Beheren van Key Vault met behulp van CLI 2.0](../key-vault/key-vault-manage-with-cli2.md)
   - [Instructies voor het gebruik van een hardware security module (HSM) en Key Vault](../key-vault/key-vault-hsm-protected-keys.md)
 - De key vault moet beschikken over de volgende eigenschap moet worden gebruikt voor TDE:
   - [soft-delete](../key-vault/key-vault-ovw-soft-delete.md)
   - [De Key Vault-functie voor voorlopig verwijderen gebruiken met CLI](../key-vault/key-vault-soft-delete-cli.md) 
- De sleutel moet de volgende kenmerken moet worden gebruikt voor TDE hebben:
   - Er is geen vervaldatum
   - Niet uitgeschakeld
   - Kan uitvoeren *ophalen*, *sleutel inpakken*, *sleutel uitpakken* bewerkingen
   
## <a name="step-1-create-a-server-with-an-azure-ad-identity"></a>Step 1. Een server maken met een Azure AD-identiteit
      cli
      # create server (with identity) and database
      az sql server create --name <servername> --resource-group <rgname>  --location <location> --admin-user <user> --admin-password <password> --assign-identity
      az sql db create --name <dbname> --server <servername> --resource-group <rgname>  
 
 
>[!Tip]
>Voorkomen dat de "principalID" maken van de server, is de object-id die is gebruikt voor het toewijzen van key vault-machtigingen in de volgende stap
>
 
## <a name="step-2-grant-key-vault-permissions-to-the-logical-sql-server"></a>Stap 2. Key Vault-machtigingen verlenen aan de logische sql-server
      cli
      # create key vault, key and grant permission
       az keyvault create --name <kvname> --resource-group <rgname> --location <location> --enable-soft-delete true
       az keyvault key create --name <keyname> --vault-name <kvname> --protection software
       az keyvault set-policy --name <kvname>  --object-id 60daa1f2-2776-4dcd-9f2f-d265aa0625c8  --resource-group <rgname> --key-permissions wrapKey unwrapKey get 


>[!Tip]
>Houd de key URI of keyID van de nieuwe sleutel voor de volgende stap, bijvoorbeeld: https://contosokeyvault.vault.azure.net/keys/Key1/1a1a2b2b3c3c4d4d5e5e6f6f7g7g8h8h
>
 
       
## <a name="step-3-add-the-key-vault-key-to-the-server-and-set-the-tde-protector"></a>Stap 3. De Key Vault-sleutel toevoegen aan de server en stel de TDE-beveiliging
  
     cli
     # add server key and update encryption protector
     az sql server key create --server <servername> --resource-group <rgname> --kid <keyID>
     az sql server tde-key set --server <servername> --server-key-type AzureKeyVault  --resource-group <rgname> --kid <keyID>

        
  > [!Note]
> De gecombineerde lengte voor de key vault-naam en de naam mag maximaal 94 tekens bevatten.
> 

  
## <a name="step-4-turn-on-tde"></a>Stap 4. TDE inschakelen 
      cli
      # enable encryption
      az sql db tde set --database <dbname> --server <servername> --resource-group <rgname> --status Enabled 
      

De database of het datawarehouse is nu TDE is ingeschakeld met een door de klant beheerde versleutelingssleutel in Azure Key Vault.

## <a name="step-5-check-the-encryption-state-and-encryption-activity"></a>Stap 5. Controleer de status van de versleuteling en versleuteling activiteit

     cli
      # get encryption scan progress
      az sql db tde list-activity --database <dbname> --server <servername> --resource-group <rgname>  

      # get whether encryption is on or off
      az sql db tde show --database <dbname> --server <servername> --resource-group <rgname> 

## <a name="sql-cli-references"></a>Verwijzingen van de SQL-CLI

https://docs.microsoft.com/cli/azure/sql?view=azure-cli-latest 

https://docs.microsoft.com/cli/azure/sql/server/key?view=azure-cli-latest 

https://docs.microsoft.com/cli/azure/sql/server/tde-key?view=azure-cli-latest 

https://docs.microsoft.com/cli/azure/sql/db/tde?view=azure-cli-latest 

