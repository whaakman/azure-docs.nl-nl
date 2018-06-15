---
title: Sleutelkluis in Azure-Stack beheren met behulp van PowerShell | Microsoft Docs
description: Informatie over het beheren van Sleutelkluis in Azure-Stack met behulp van PowerShell
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 22B62A3B-B5A9-4B8C-81C9-DA461838FAE5
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/10/2018
ms.author: mabrigg
ms.openlocfilehash: 5e9de401f64a835c286c226bfac88caf5168b96e
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/12/2018
ms.locfileid: "34075762"
---
# <a name="manage-key-vault-in-azure-stack-using-powershell"></a>Sleutelkluis in Azure-Stack met behulp van PowerShell beheren

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

U kunt de Sleutelkluis in Azure-Stack met behulp van PowerShell beheren. Meer informatie over Sleutelkluis PowerShell-cmdlets te gebruiken:

* Een sleutelkluis maken.
* Opgeslagen en beheerd cryptografiesleutels en geheimen.
* Het autoriseren van gebruikers of toepassingen aan te roepen bewerkingen in de kluis.

>[!NOTE]
>De Sleutelkluis PowerShell-cmdlets beschreven in dit artikel vindt u in de Azure PowerShell SDK.

## <a name="prerequisites"></a>Vereisten

* U moet zich abonneren op een aanbieding met de Azure Sleutelkluis-service.
* [Installeer PowerShell voor Azure Stack](azure-stack-powershell-install.md).
* [Configureren van de gebruiker van de Stack van Azure PowerShell-omgeving](azure-stack-powershell-configure-user.md).

## <a name="enable-your-tenant-subscription-for-key-vault-operations"></a>Uw tenantabonnement voor Sleutelkluis bewerkingen inschakelen

Voordat u geen bewerkingen uit op basis van een sleutelkluis verzendt kunt, moet u ervoor zorgen dat uw tenant-abonnement is ingeschakeld voor bewerkingen van de kluis. Om te bevestigen dat de kluis bewerkingen zijn ingeschakeld, moet u de volgende opdracht uitvoeren:

```PowerShell
Get-AzureRmResourceProvider -ProviderNamespace Microsoft.KeyVault | ft -Autosize
```

**Uitvoer**

Als uw abonnement is ingeschakeld voor bewerkingen van de kluis, ziet u de uitvoer 'RegistrationState' is 'geregistreerd' voor alle resourcetypen van een sleutelkluis.

![Status van de sleutelkluis-registratie](media/azure-stack-kv-manage-powershell/image1.png)

Als de kluis niet is ingeschakeld, roepen de volgende opdracht voor het registreren van de Sleutelkluis-service in uw abonnement:

```PowerShell
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.KeyVault
```

**Uitvoer**

Als de registratie gelukt is, wordt de volgende uitvoer geretourneerd:

![Registreren](media/azure-stack-kv-manage-powershell/image2.png) als u de sleutelkluis-opdrachten aanroept, mogelijk, krijgt u een fout optreedt, zoals 'het abonnement is niet geregistreerd voor gebruik van de naamruimte 'Microsoft.KeyVault'.' Als u een fout optreedt, moet u bevestigen dat u hebt [ingeschakeld de Sleutelkluis-resourceprovider](#enable-your-tenant-subscription-for-vault-operations) volgens de instructies die eerder werden vermeld.

## <a name="create-a-key-vault"></a>Een sleutelkluis maken

Voordat u een sleutelkluis maken, een resourcegroep maken zodat alle bronnen die betrekking hebben op de sleutelkluis in een resourcegroep bestaan. Gebruik de volgende opdracht om een nieuwe resourcegroep te maken:

```PowerShell
New-AzureRmResourceGroup -Name “VaultRG” -Location local -verbose -Force

```

**Uitvoer**

![nieuwe resourcegroep](media/azure-stack-kv-manage-powershell/image3.png)

Nu gebruiken de **New-AzureRMKeyVault** opdracht een sleutelkluis maken in de resourcegroep die u eerder hebt gemaakt. Met deze opdracht leest drie verplichte parameters: naam van een resourcegroep, sleutelkluisnaam en geografische locatie.

Voer de volgende opdracht om een sleutelkluis te maken:

```PowerShell
New-AzureRmKeyVault -VaultName “Vault01” -ResourceGroupName “VaultRG” -Location local -verbose
```

**Uitvoer**

![Nieuwe sleutelkluis](media/azure-stack-kv-manage-powershell/image4.png)

De uitvoer van deze opdracht worden de eigenschappen van de sleutelkluis die u hebt gemaakt. Wanneer een toepassing deze kluis opent, moet hiervoor de **Vault URI** eigenschap, die 'https://vault01.vault.local.azurestack.external' in dit voorbeeld.

### <a name="active-directory-federation-services-ad-fs-deployment"></a>Implementatie van Active Directory Federation Services (AD FS)

In een AD FS-implementatie, kunt u deze waarschuwing krijgen: '-beleid is niet ingesteld. Er is geen gebruiker of toepassing heeft toegangsmachtigingen voor het gebruik van deze kluis." Dit probleem oplossen door een toegangsbeleid voor de kluis in te stellen via de [Set-AzureRmKeyVaultAccessPolicy](azure-stack-kv-manage-powershell.md#authorize-an-application-to-use-a-key-or-secret) opdracht:

```PowerShell
# Obtain the security identifier(SID) of the active directory user
$adUser = Get-ADUser -Filter "Name -eq '{Active directory user name}'"
$objectSID = $adUser.SID.Value

# Set the key vault access policy
Set-AzureRmKeyVaultAccessPolicy -VaultName "{key vault name}" -ResourceGroupName "{resource group name}" -ObjectId "{object SID}" -PermissionsToKeys {permissionsToKeys} -PermissionsToSecrets {permissionsToSecrets} -BypassObjectIdValidation
```

## <a name="manage-keys-and-secrets"></a>Sleutels en geheimen beheren

Nadat u een kluis hebt gemaakt, gebruik de volgende stappen voor het maken en beheren van sleutels en geheimen in de kluis.

### <a name="create-a-key"></a>Een sleutel maken

Gebruik de **Add-AzureKeyVaultKey** opdracht te maken of importeren van een softwarematig beveiligde sleutel in een sleutelkluis.

```PowerShell
Add-AzureKeyVaultKey -VaultName “Vault01” -Name “Key01” -verbose -Destination Software
```

De **bestemming** parameter wordt gebruikt om op te geven dat de sleutel software die zijn beveiligd is. Als de sleutel is gemaakt, wordt de opdracht de details van de gemaakte sleutel.

**Uitvoer**

![Nieuwe sleutel](media/azure-stack-kv-manage-powershell/image5.png)

U kunt nu verwijzen naar de gemaakte sleutel met behulp van de URI. Als u maken of importeren van een sleutel die dezelfde naam als een bestaande sleutel heeft, wordt de oorspronkelijke sleutel bijgewerkt met de opgegeven waarden in de nieuwe sleutel. U kunt toegang tot de vorige versie met behulp van de versie-specifieke-URI van de sleutel. Bijvoorbeeld:

* Gebruik 'https://vault10.vault.local.azurestack.external:443/keys/key01' voor altijd de huidige versie.
* Gebruik 'https://vault010.vault.local.azurestack.external:443/keys/key01/d0b36ee2e3d14e9f967b8b6b1d38938a' voor deze specifieke versie.

### <a name="get-a-key"></a>Een sleutel ophalen

Gebruik de **Get-AzureKeyVaultKey** opdracht om een sleutel en de details ervan te lezen.

```PowerShell
Get-AzureKeyVaultKey -VaultName “Vault01” -Name “Key01”
```

### <a name="create-a-secret"></a>Een geheim maken

Gebruik de **Set AzureKeyVaultSecret** opdracht maken of bijwerken van een geheim in een kluis. Een geheim wordt gemaakt als een nog niet bestaat. Een nieuwe versie van het geheim wordt gemaakt als deze al bestaat.

```PowerShell
$secretvalue = ConvertTo-SecureString “User@123” -AsPlainText -Force
Set-AzureKeyVaultSecret -VaultName “Vault01” -Name “Secret01” -SecretValue $secretvalue
```

**Uitvoer**

![Een geheim maken](media/azure-stack-kv-manage-powershell/image6.png)

### <a name="get-a-secret"></a>Een geheim ophalen

Gebruik de **Get-AzureKeyVaultSecret** opdracht een geheim in een sleutelkluis te lezen. Met deze opdracht kunt terugkeren alle of specifieke versies van een geheim.

```PowerShell
Get-AzureKeyVaultSecret -VaultName “Vault01” -Name “Secret01”
```

Nadat u de sleutels en geheimen hebt gemaakt, geeft u toestemming externe toepassingen worden gebruikt.

## <a name="authorize-an-application-to-use-a-key-or-secret"></a>Een toepassing voor het gebruik van een sleutel of geheim autoriseren

Gebruik de **Set-AzureRmKeyVaultAccessPolicy** opdracht om een toepassing te krijgen tot een sleutel of geheim in de sleutelkluis.
In het volgende voorbeeld wordt de kluisnaam van de is *ContosoKeyVault* en de toepassing die u wilt machtigen client-ID heeft *8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed*. Voer de volgende opdracht voor het autoriseren van de toepassing. Desgewenst kunt u de **PermissionsToKeys** parameter machtigingen instellen voor een gebruiker, toepassing of een beveiligingsgroep.

```PowerShell
Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed -PermissionsToKeys decrypt,sign
```

Als u autoriseren van dezelfde toepassing wilt te lezen van geheimen in uw kluis, voert u de volgende cmdlet:

```PowerShell
Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300 -PermissionsToKeys Get
```

## <a name="next-steps"></a>Volgende stappen

* [Een virtuele machine implementeren met een wachtwoord opgeslagen in de Sleutelkluis](azure-stack-kv-deploy-vm-with-secret.md)
* [Een virtuele machine implementeren met een certificaat dat is opgeslagen in de Sleutelkluis](azure-stack-kv-push-secret-into-vm.md)
