---
title: PowerShell - draaien TDE-beveiliging - Azure SQL Database | Microsoft Docs
description: Informatie over het draaien van de sleutelbeveiliging transparante gegevensversleuteling (TDE) voor een Azure SQL-server.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: aliceku
ms.author: aliceku
ms.reviewer: vanto
manager: jhubbard
ms.date: 12/06/2018
ms.openlocfilehash: 45cd4e884530836d515e0c6cce8a6fc9be109d88
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/11/2019
ms.locfileid: "55992004"
---
# <a name="rotate-the-transparent-data-encryption-tde-protector-using-powershell"></a>De transparante gegevensversleuteling (TDE)-beveiliging met behulp van PowerShell draaien

Dit artikel wordt beschreven rouleren van de sleutel voor een Azure SQL-server met behulp van een TDE-beveiliging van Azure Key Vault. Een Azure SQL-server TDE-beveiliging betekent overschakelen naar een nieuwe asymmetrische sleutel die worden beveiligd met de databases op de server draaien. Rouleren van de sleutel is een online bewerking en duurt slechts een paar seconden voltooid, omdat dit alleen wordt ontsleuteld en versleutelt de gegevensversleutelingssleutel van de database, niet de gehele database opnieuw.

Deze handleiding wordt beschreven twee opties voor het draaien van de TDE-beveiliging op de server.

> [!NOTE]
> Een onderbroken SQL Data Warehouse moet voordat sleutelrotaties worden hervat.
>

> [!IMPORTANT]
> **Kan niet worden verwijderd** eerdere versies van de sleutel na een rollover.  Wanneer sleutels worden meegenomen, sommige gegevens nog steeds versleuteld met de vorige sleutels, zoals oudere databaseback-ups. 
>

## <a name="prerequisites"></a>Vereisten

- In deze gebruiksaanwijzing wordt ervan uitgegaan dat u al van een sleutel uit Azure Key Vault als de TDE-beveiliging voor een Azure SQL Database of het datawarehouse gebruikmaakt. Zie [Transparent Data Encryption met Azure Key Vault-integratie - BYOK-ondersteuning](transparent-data-encryption-byok-azure-sql.md).
- U moet Azure PowerShell versie 3.7.0 of hoger zijn geïnstalleerd en uitgevoerd. 
- [Aanbevolen maar niet vereist] Maken van het sleutelmateriaal voor de TDE-beveiliging in een hardware security module (HSM) of lokale sleutel opslaan van de eerste en het sleutelmateriaal importeren naar Azure Key Vault. Ga als volgt de [instructies voor het gebruik van een hardware security module (HSM) en Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-get-started) voor meer informatie.

## <a name="manual-key-rotation"></a>Handmatige sleutelroulatie

Handmatige sleutelroulatie maakt gebruik van de [Add-AzureKeyVaultKey](/powershell/module/azurerm.keyvault/add-azurekeyvaultkey), [toevoegen AzureRmSqlServerKeyVaultKey](/powershell/module/azurerm.sql/add-azurermsqlserverkeyvaultkey), en [Set-azurermsqlservertransparentdataencryptionprotector geeft](/powershell/module/azurerm.sql/set-azurermsqlservertransparentdataencryptionprotector) cmdlets voor het toevoegen van een volledig nieuwe sleutel, die onder de naam van een nieuwe sleutel of zelfs een andere sleutelkluis worden kan. Met deze aanpak biedt ondersteuning voor dezelfde sleutel aan verschillende sleutelkluizen ter ondersteuning van hoge beschikbaarheid en geo-dr's toe te voegen.

>[!NOTE]
>De gecombineerde lengte voor de key vault-naam en de naam mag maximaal 94 tekens bevatten.
>

   ```powershell
   # Add a new key to Key Vault
   Add-AzureKeyVaultKey `
   -VaultName <KeyVaultName> `
   -Name <KeyVaultKeyName> `
   -Destination <HardwareOrSoftware>

   # Add the new key from Key Vault to the server
   Add-AzureRmSqlServerKeyVaultKey `
   -KeyId <KeyVaultKeyId> `
   -ServerName <LogicalServerName> `
   -ResourceGroup <SQLDatabaseResourceGroupName>   
  
   <# Set the key as the TDE protector for all resources 
   under the server #>
   Set-AzureRmSqlServerTransparentDataEncryptionProtector `
   -Type AzureKeyVault `
   -KeyId <KeyVaultKeyId> `
   -ServerName <LogicalServerName> `
   -ResourceGroup <SQLDatabaseResourceGroupName>
   ```
  
## <a name="other-useful-powershell-cmdlets"></a>Andere nuttige PowerShell-cmdlets

- Als u wilt overschakelen van de TDE-beveiliging van Microsoft beheerde naar de BYOK-modus, gebruikt u de [Set-azurermsqlservertransparentdataencryptionprotector geeft](/powershell/module/azurerm.sql/set-azurermsqlservertransparentdataencryptionprotector) cmdlet.

   ```powershell
   Set-AzureRmSqlServerTransparentDataEncryptionProtector `
   -Type AzureKeyVault `
   -KeyId <KeyVaultKeyId> `
   -ServerName <LogicalServerName> `
   -ResourceGroup <SQLDatabaseResourceGroupName>
   ```

- Als u wilt overschakelen van de TDE-beveiliging van de BYOK-modus naar Microsoft worden beheerd, gebruiken de [Set-azurermsqlservertransparentdataencryptionprotector geeft](/powershell/module/azurerm.sql/set-azurermsqlservertransparentdataencryptionprotector) cmdlet.

   ```powershell
   Set-AzureRmSqlServerTransparentDataEncryptionProtector `
   -Type ServiceManaged `
   -ServerName <LogicalServerName> `
   -ResourceGroup <SQLDatabaseResourceGroupName> 
   ``` 

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het verwijderen van een potentieel aangetast TDE-beveiliging in het geval van een veiligheidsrisico inhouden: [Een potentieel aangetast sleutel verwijderen](transparent-data-encryption-byok-azure-sql-remove-tde-protector.md) 

- Aan de slag met Azure Key Vault-integratie en Bring Your Own Key-ondersteuning voor TDE: [TDE met behulp van uw eigen sleutel uit Key Vault met behulp van PowerShell inschakelen](transparent-data-encryption-byok-azure-sql-configure.md)
