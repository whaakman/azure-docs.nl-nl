---
title: Key Vault in Azure Stack beheren met behulp van PowerShell | Microsoft Docs
description: Informatie over het beheren van Key Vault in Azure Stack met behulp van PowerShell
services: azure-stack
documentationcenter: ''
author: sethmanheim
manager: femila
editor: ''
ms.assetid: 22B62A3B-B5A9-4B8C-81C9-DA461838FAE5
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/05/2019
ms.author: sethm
ms.openlocfilehash: a7d49c29445b425caae2fbd1967a2408657b4610
ms.sourcegitcommit: d61faf71620a6a55dda014a665155f2a5dcd3fa2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54054119"
---
# <a name="manage-key-vault-in-azure-stack-using-powershell"></a>Key Vault in Azure Stack met behulp van PowerShell beheren

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

U kunt Key Vault in Azure Stack met behulp van PowerShell beheren. In dit artikel wordt beschreven hoe u Key Vault PowerShell-cmdlets te gebruiken:

* Een sleutelkluis maken.
* Store en cryptografische sleutels en geheimen beheren.
* Toestaan dat gebruikers of toepassingen voor het aanroepen van bewerkingen in de kluis.

>[!NOTE]
>De Key Vault PowerShell-cmdlets die in dit artikel vindt u in de Azure PowerShell SDK.

## <a name="prerequisites"></a>Vereisten

* U moet zich abonneren op een aanbieding met inbegrip van de Azure Key Vault-service.
* [Installeren van PowerShell voor Azure Stack](azure-stack-powershell-install.md).
* [PowerShell-omgeving van de Azure Stack-gebruiker configureren](azure-stack-powershell-configure-user.md).

## <a name="enable-your-tenant-subscription-for-key-vault-operations"></a>Schakel uw tenantabonnement voor Key Vault-bewerkingen

Voordat u alle bewerkingen op basis van een key vault uitvoert kunt, moet u ervoor zorgen dat uw tenant-abonnement is ingeschakeld voor vault-bewerkingen. Om te controleren dat vault-bewerkingen zijn ingeschakeld, moet u de volgende opdracht uitvoeren:

```PowerShell
Get-AzureRmResourceProvider -ProviderNamespace Microsoft.KeyVault | ft -Autosize
```

**Uitvoer**

Als uw abonnement is ingeschakeld voor bewerkingen van de kluis, in de uitvoer ziet **RegistrationState** is **geregistreerde** voor alle resourcetypen van een key vault.

![Status van de sleutelkluis-apparaatregistratie](media/azure-stack-kv-manage-powershell/image1.png)

Als de kluis bewerkingen zijn niet ingeschakeld, voert u de volgende opdracht om het registreren van de service Key Vault in uw abonnement:

```PowerShell
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.KeyVault
```

**Uitvoer**

Als de registratie geslaagd is, wordt de volgende uitvoer geretourneerd:

![Registreren](media/azure-stack-kv-manage-powershell/image2.png)

Wanneer u de Key Vault-opdrachten aanroepen, u mogelijk een foutbericht, zoals 'het abonnement is niet geregistreerd voor het gebruik van de naamruimte 'Microsoft.KeyVault'.' Als u een fout optreedt, moet u bevestigen dat u hebt [de Key Vault resourceprovider ingeschakeld](#enable-your-tenant-subscription-for-vault-operations) door de instructies die eerder zijn opgegeven.

## <a name="create-a-key-vault"></a>Een sleutelkluis maken

Voordat u een sleutelkluis maakt, een resourcegroep maken zodat alle resources met betrekking tot de key vault zich in een resourcegroep. Gebruik de volgende opdracht om een nieuwe resourcegroep te maken:

```PowerShell
New-AzureRmResourceGroup -Name "VaultRG" -Location local -verbose -Force
```

**Uitvoer**

![Nieuwe resourcegroep](media/azure-stack-kv-manage-powershell/image3.png)

Gebruik nu de **New-AzureRMKeyVault** cmdlet voor het maken van een key vault in de resourcegroep die u eerder hebt gemaakt. Met deze opdracht leest drie verplichte parameters: naam van de resourcegroep, key vault-naam en geografische locatie.

Voer de volgende opdracht om een sleutelkluis te maken:

```PowerShell
New-AzureRmKeyVault -VaultName "Vault01" -ResourceGroupName "VaultRG" -Location local -verbose
```

**Uitvoer**

![Nieuwe key vault](media/azure-stack-kv-manage-powershell/image4.png)

De uitvoer van deze opdracht worden de eigenschappen van de sleutelkluis die u hebt gemaakt. Wanneer een toepassing toegang heeft tot deze kluis, moet deze gebruiken de `Vault URI` eigenschap, die is `https://vault01.vault.local.azurestack.external` in dit voorbeeld.

### <a name="active-directory-federation-services-ad-fs-deployment"></a>Implementatie van Active Directory Federation Services (AD FS)

U kunt deze waarschuwing te krijgen in een AD FS-implementatie: "Toegangsbeleid is niet ingesteld. Er is geen gebruiker of toepassing heeft toegangsmachtigingen voor het gebruik van deze kluis." U lost dit probleem, stelt u een toegangsbeleid voor de kluis met behulp van de [Set-AzureRmKeyVaultAccessPolicy](azure-stack-kv-manage-powershell.md#authorize-an-application-to-use-a-key-or-secret) cmdlet:

```PowerShell
# Obtain the security identifier(SID) of the active directory user
$adUser = Get-ADUser -Filter "Name -eq '{Active directory user name}'"
$objectSID = $adUser.SID.Value

# Set the key vault access policy
Set-AzureRmKeyVaultAccessPolicy -VaultName "{key vault name}" -ResourceGroupName "{resource group name}" -ObjectId "{object SID}" -PermissionsToKeys {permissionsToKeys} -PermissionsToSecrets {permissionsToSecrets} -BypassObjectIdValidation
```

## <a name="manage-keys-and-secrets"></a>Sleutels en geheimen beheren

Nadat u een sleutelkluis gemaakt, gebruikt u de volgende stappen uit om te maken en beheren van sleutels en geheimen in de kluis.

### <a name="create-a-key"></a>Een sleutel maken

Gebruik de **Add-AzureKeyVaultKey** cmdlet te maken of importeren van een softwarematig beveiligde sleutel in een key vault.

```PowerShell
Add-AzureKeyVaultKey -VaultName "Vault01" -Name "Key01" -verbose -Destination Software
```

De `Destination` parameter wordt gebruikt om op te geven dat de sleutel software die zijn beveiligd is. Wanneer de sleutel is gemaakt, voert de opdracht de details van de zojuist gemaakte sleutel.

**Uitvoer**

![Nieuwe sleutel](media/azure-stack-kv-manage-powershell/image5.png)

U kunt nu verwijzen naar de zojuist gemaakte sleutel met behulp van de URI. Als u maken of importeren van een sleutel die dezelfde naam als een bestaande sleutel heeft, wordt de oorspronkelijke sleutel wordt bijgewerkt met de opgegeven waarden in de nieuwe sleutel. U kunt toegang tot de vorige versie met behulp van de URI van de specifieke versies van de sleutel. Bijvoorbeeld:

* Gebruik `https://vault10.vault.local.azurestack.external:443/keys/key01` naar altijd kunnen genieten van de huidige versie.
* Gebruik `https://vault010.vault.local.azurestack.external:443/keys/key01/d0b36ee2e3d14e9f967b8b6b1d38938a` voor deze specifieke versie.

### <a name="get-a-key"></a>Een sleutel ophalen

Gebruik de **Get-AzureKeyVaultKey** cmdlet om een sleutel en de details ervan te lezen.

```PowerShell
Get-AzureKeyVaultKey -VaultName "Vault01" -Name "Key01"
```

### <a name="create-a-secret"></a>Een geheim maken

Gebruik de **Set-AzureKeyVaultSecret** cmdlet maken of bijwerken van een geheim in een kluis. Een geheim is gemaakt als deze niet al bestaat. Een nieuwe versie van het geheim is gemaakt als deze al bestaat.

```PowerShell
$secretvalue = ConvertTo-SecureString "User@123" -AsPlainText -Force
Set-AzureKeyVaultSecret -VaultName "Vault01" -Name "Secret01" -SecretValue $secretvalue
```

**Uitvoer**

![Een geheim maken](media/azure-stack-kv-manage-powershell/image6.png)

### <a name="get-a-secret"></a>Een geheim ophalen

Gebruik de **Get-AzureKeyVaultSecret** cmdlet voor het lezen van een geheim in een key vault. Met deze opdracht kan al retourneren of een specifieke versie van een geheim.

```PowerShell
Get-AzureKeyVaultSecret -VaultName "Vault01" -Name "Secret01"
```

Nadat u de sleutels en geheimen hebt gemaakt, geeft u toestemming externe toepassingen om ze te gebruiken.

## <a name="authorize-an-application-to-use-a-key-or-secret"></a>Een toepassing machtigen voor het gebruik van een sleutel of geheim

Gebruik de **Set-AzureRmKeyVaultAccessPolicy** cmdlet om een toepassing voor toegang tot een sleutel of geheim in de key vault. In het volgende voorbeeld wordt de kluisnaam van de is `ContosoKeyVault`, en de toepassing die u wilt toestaan dat een client-ID van `8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed`. Voer de volgende opdracht voor het autoriseren van de toepassing. Desgewenst kunt u de `PermissionsToKeys` parameter machtigingen instellen voor een gebruiker, toepassing of een beveiligingsgroep.

```PowerShell
Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300b7b4ed -PermissionsToKeys decrypt,sign
```

Als u dat dezelfde toepassing machtigen voor het lezen van geheimen in uw kluis wilt, voert u de volgende cmdlet uit:

```PowerShell
Set-AzureRmKeyVaultAccessPolicy -VaultName 'ContosoKeyVault' -ServicePrincipalName 8f8c4bbd-485b-45fd-98f7-ec6300 -PermissionsToKeys Get
```

## <a name="next-steps"></a>Volgende stappen

* [Een virtuele machine implementeren met een wachtwoord opgeslagen in Key Vault](azure-stack-kv-deploy-vm-with-secret.md)
* [Een virtuele machine implementeren met een certificaat dat is opgeslagen in Key Vault](azure-stack-kv-push-secret-into-vm.md)
