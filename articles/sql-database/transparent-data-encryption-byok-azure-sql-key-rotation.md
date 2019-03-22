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
ms.date: 03/12/2019
ms.openlocfilehash: 760b292e75b4cc64b85eaf51ffad0521b721dabf
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57895648"
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

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> De PowerShell Azure Resource Manager-module nog steeds wordt ondersteund door Azure SQL Database, maar alle toekomstige ontwikkeling is voor de module Az.Sql. Zie voor deze cmdlets [AzureRM.Sql](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). De argumenten voor de opdrachten in de Az-module en de AzureRm-modules zijn vrijwel identiek zijn.

- In deze gebruiksaanwijzing wordt ervan uitgegaan dat u al van een sleutel uit Azure Key Vault als de TDE-beveiliging voor een Azure SQL Database of het datawarehouse gebruikmaakt. Zie [Transparent Data Encryption met BYOK-ondersteuning](transparent-data-encryption-byok-azure-sql.md).
- Azure PowerShell installeren en uitvoeren, moet u hebben.
- [Aanbevolen maar niet vereist] Maken van het sleutelmateriaal voor de TDE-beveiliging in een hardware security module (HSM) of lokale sleutel opslaan van de eerste en het sleutelmateriaal importeren naar Azure Key Vault. Ga als volgt de [instructies voor het gebruik van een hardware security module (HSM) en Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-get-started) voor meer informatie.

## <a name="manual-key-rotation"></a>Handmatige sleutelroulatie

Handmatige sleutelroulatie maakt gebruik van de [toevoegen AzKeyVaultKey](/powershell/module/az.keyvault/Add-AzKeyVaultKey), [toevoegen AzSqlServerKeyVaultKey](/powershell/module/az.sql/add-azsqlserverkeyvaultkey), en [Set AzSqlServerTransparentDataEncryptionProtector](/powershell/module/az.sql/set-azsqlservertransparentdataencryptionprotector) cmdlets om toe te voegen een volledig nieuwe sleutel, die onder de naam van een nieuwe sleutel of zelfs een andere sleutelkluis worden kan. Met deze aanpak biedt ondersteuning voor dezelfde sleutel aan verschillende sleutelkluizen ter ondersteuning van hoge beschikbaarheid en geo-dr's toe te voegen.

>[!NOTE]
>De gecombineerde lengte voor de key vault-naam en de naam mag maximaal 94 tekens bevatten.

   ```powershell
   # Add a new key to Key Vault
   Add-AzKeyVaultKey `
   -VaultName <KeyVaultName> `
   -Name <KeyVaultKeyName> `
   -Destination <HardwareOrSoftware>

   # Add the new key from Key Vault to the server
   Add-AzSqlServerKeyVaultKey `
   -KeyId <KeyVaultKeyId> `
   -ServerName <LogicalServerName> `
   -ResourceGroup <SQLDatabaseResourceGroupName>
  
   <# Set the key as the TDE protector for all resources under the server #>
   Set-AzSqlServerTransparentDataEncryptionProtector `
   -Type AzureKeyVault `
   -KeyId <KeyVaultKeyId> `
   -ServerName <LogicalServerName> `
   -ResourceGroup <SQLDatabaseResourceGroupName>
   ```

## <a name="option-2-manual-rotation"></a>Optie 2: Handmatige draaien

De optie gebruikt de [toevoegen AzKeyVaultKey](/powershell/module/az.keyvault/add-azkeyvaultkey), [toevoegen AzSqlServerKeyVaultKey](/powershell/module/az.sql/add-azsqlserverkeyvaultkey), en [Set AzSqlServerTransparentDataEncryptionProtector](/powershell/module/az.sql/set-azsqlservertransparentdataencryptionprotector) cmdlets om toe te voegen een volledig nieuwe sleutel, die onder de naam van een nieuwe sleutel of zelfs een andere sleutelkluis worden kan. 

>[!NOTE]
>De gecombineerde lengte voor de key vault-naam en de naam mag maximaal 94 tekens bevatten.
>

   ```powershell
   # Add a new key to Key Vault
   Add-AzKeyVaultKey `
   -VaultName <KeyVaultName> `
   -Name <KeyVaultKeyName> `
   -Destination <HardwareOrSoftware>

   # Add the new key from Key Vault to the server
   Add-AzSqlServerKeyVaultKey `
   -KeyId <KeyVaultKeyId> `
   -ServerName <LogicalServerName> `
   -ResourceGroup <SQLDatabaseResourceGroupName>   
  
   <# Set the key as the TDE protector for all resources 
   under the server #>
   Set-AzSqlServerTransparentDataEncryptionProtector `
   -Type AzureKeyVault `
   -KeyId <KeyVaultKeyId> `
   -ServerName <LogicalServerName> `
   -ResourceGroup <SQLDatabaseResourceGroupName>
   ```
  
## <a name="other-useful-powershell-cmdlets"></a>Andere nuttige PowerShell-cmdlets

- Als u wilt overschakelen van de TDE-beveiliging van Microsoft beheerde naar de BYOK-modus, gebruikt u de [Set AzSqlServerTransparentDataEncryptionProtector](/powershell/module/az.sql/set-azsqlservertransparentdataencryptionprotector) cmdlet.

   ```powershell
   Set-AzSqlServerTransparentDataEncryptionProtector `
   -Type AzureKeyVault `
   -KeyId <KeyVaultKeyId> `
   -ServerName <LogicalServerName> `
   -ResourceGroup <SQLDatabaseResourceGroupName>
   ```

- Als u wilt overschakelen van de TDE-beveiliging van de BYOK-modus naar Microsoft worden beheerd, gebruiken de [Set AzSqlServerTransparentDataEncryptionProtector](/powershell/module/az.sql/set-azsqlservertransparentdataencryptionprotector) cmdlet.

   ```powershell
   Set-AzSqlServerTransparentDataEncryptionProtector `
   -Type ServiceManaged `
   -ServerName <LogicalServerName> `
   -ResourceGroup <SQLDatabaseResourceGroupName> 
   ``` 

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het verwijderen van een potentieel aangetast TDE-beveiliging in het geval van een veiligheidsrisico inhouden: [Een potentieel aangetast sleutel verwijderen](transparent-data-encryption-byok-azure-sql-remove-tde-protector.md) 

- Aan de slag met Azure Key Vault-integratie en Bring Your Own Key-ondersteuning voor TDE: [TDE met behulp van uw eigen sleutel uit Key Vault met behulp van PowerShell inschakelen](transparent-data-encryption-byok-azure-sql-configure.md)
