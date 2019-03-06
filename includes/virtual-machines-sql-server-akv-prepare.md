---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines-windows
author: rothja
manager: craigg
tags: azure-service-management
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: include
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 04/30/2018
ms.author: jroth
ms.custom: include file
ms.openlocfilehash: 4ff99cab168abeb0bbeeb8b29de0ea0285c255b0
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57457959"
---
## <a name="prepare-for-akv-integration"></a>Voorbereiden voor Azure Sleutelkluis-integratie
Als u Azure Key Vault-integratie wilt configureren van uw SQL Server-machine, zijn er verschillende vereisten: 

1. [Azure Powershell installeren](#install)
2. [Maak een Azure Active Directory](#register)
3. [Een sleutelkluis maken](#createkeyvault)

De volgende secties beschrijven deze vereisten en de informatie die u verzamelen moet om de PowerShell-cmdlets later worden uitgevoerd.

### <a id="install"></a> Azure PowerShell installeren
Zorg ervoor dat u de nieuwste Azure PowerShell SDK hebt geïnstalleerd. Zie [Azure PowerShell installeren en configureren](/powershell/azureps-cmdlets-docs) voor meer informatie.

### <a id="register"></a> Een toepassing registreren in uw Azure Active Directory

Eerst moet u hebt een [Azure Active Directory](https://azure.microsoft.com/trial/get-started-active-directory/) (AAD) in uw abonnement. Veel voordelen kunt hiermee u toestemming te verlenen voor uw key vault voor bepaalde gebruikers en toepassingen.

Vervolgens moet u een toepassing registreren met AAD. Hiermee geeft u een Service-Principal-account dat toegang heeft tot uw key vault, die uw virtuele machine moet. In het artikel Azure Key Vault vindt u de stappen in de [een toepassing registreren met Azure Active Directory](../articles/key-vault/key-vault-manage-with-cli2.md#registering-an-application-with-azure-active-directory) sectie, of u kunt zien welke stappen met schermafbeeldingen in de **een identiteit voor de sectie van de toepassing ophalen**  van [dit blogbericht](http://blogs.technet.com/b/kv/archive/2015/01/09/azure-key-vault-step-by-step.aspx). Voordat u deze stappen uitvoert, moet u voor het verzamelen van de volgende informatie tijdens deze registratie dat hoger is vereist wanneer u Azure Key Vault-integratie op de SQL-VM inschakelen.

* Nadat de toepassing is toegevoegd, vinden de **toepassings-ID** op de **geregistreerde app** blade.
    De toepassings-ID is toegewezen, later naar de **$spName** (Service Principal name)-parameter in de PowerShell-script voor het inschakelen van Azure Key Vault-integratie.

   ![Toepassings-id](./media/virtual-machines-sql-server-akv-prepare/aad-application-id.png)

* Tijdens deze stappen wanneer u uw sleutel maakt, het geheim te kopiëren voor uw sleutel zoals wordt weergegeven in de volgende schermafbeelding. Deze sleutels geheim krijgt later naar de **$spSecret** (Service-Principal-geheim)-parameter in het PowerShell-script.

   ![AAD-geheim](./media/virtual-machines-sql-server-akv-prepare/aad-sp-secret.png)

* De toepassings-ID en het geheim wordt ook gebruikt voor het maken van een referentie in SQL Server.

* Verleent u deze nieuwe client-ID hebben de volgende toegangsmachtigingen: **ophalen**, **wrapKey**, **unwrapKey**. Dit wordt gedaan met de [Set AzKeyVaultAccessPolicy](https://docs.microsoft.com/powershell/module/azurerm.keyvault/set-azurermkeyvaultaccesspolicy) cmdlet. Zie voor meer informatie, [overzicht van Azure Key Vault](../articles/key-vault/key-vault-overview.md).

### <a id="createkeyvault"></a> Een sleutelkluis maken
Azure Key Vault gebruiken voor het opslaan van de sleutels die u voor versleuteling in uw virtuele machine gebruikt, moet u toegang tot een key vault. Als u uw key vault nog niet hebt ingesteld, maakt u volgt u de stappen in de [aan de slag met Azure Key Vault](../articles/key-vault/key-vault-overview.md) artikel. Voordat u deze stappen uitvoert, moet u enkele gegevens u verzamelen tijdens deze set wilt die is ook later nodig wanneer u Azure Key Vault-integratie op de SQL-VM inschakelen.

    New-AzKeyVault -VaultName 'ContosoKeyVault' -ResourceGroupName 'ContosoResourceGroup' -Location 'East Asia'

Wanneer u op de maken een stap van de sleutelkluis, houd er rekening mee de geretourneerde **vaultUri** eigenschap, dit de URL voor key vault is. In het voorbeeld in deze stap, hieronder wordt weergegeven, de naam van de sleutelkluis is ContosoKeyVault, dus de key vault URL zou zijn https://contosokeyvault.vault.azure.net/.

De URL voor key vault is toegewezen, later naar de **$akvURL** parameter in de PowerShell-script voor het inschakelen van Azure Key Vault-integratie.

Nadat de key vault is gemaakt, moeten we een sleutel toevoegen aan de sleutelkluis, deze sleutel wordt verwezen als maken we maken een asymmetrische sleutel later in SQL Server.
