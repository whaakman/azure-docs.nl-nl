---
title: Veel toepassingen om een Azure-sleutelkluis - Azure Key Vault toegang te verlenen tot | Microsoft Docs
description: Leer hoe u toestemming te verlenen voor veel toepassingen toegang tot een key vault
services: key-vault
documentationcenter: ''
author: amitbapat
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 785d4e40-fb7b-485a-8cbc-d9c8c87708e6
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/07/2019
ms.author: ambapat
ms.openlocfilehash: cd680f24eafe61bc73fa6eb91df4b4dfa5f5399b
ms.sourcegitcommit: fbf0124ae39fa526fc7e7768952efe32093e3591
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2019
ms.locfileid: "54073421"
---
# <a name="grant-several-applications-access-to-a-key-vault"></a>Verschillende toepassingen toegang verlenen tot een key vault

Beleid voor toegangsbeheer kan worden gebruikt om verschillende toepassingen toegang verlenen tot een key vault. Een beleid voor toegangsbeheer kan maximaal 1024 toepassingen ondersteunen en is als volgt geconfigureerd:

1. Een Azure Active Directory-beveiligingsgroep maken. 
2. Voeg alle van de toepassingen die de service-principals aan de beveiligingsgroep die is gekoppeld.
3. De beveiliging groepstoegang verlenen tot uw Key Vault.

Hier volgen de vereisten:
* [Azure Active Directory V2 PowerShell-module installeren](https://www.powershellgallery.com/packages/AzureAD).
* [Installeer Azure PowerShell](/powershell/azure/overview).
* Als u wilt de volgende opdrachten uitvoeren, moet u machtigingen voor groepen in de Azure Active Directory-tenant maken/bewerken. Als u geen machtigingen hebt, moet u mogelijk contact op met uw Azure Active Directory-beheerder. Zie [over Azure Key Vault sleutels, geheimen en certificaten](about-keys-secrets-and-certificates.md) voor meer informatie over Key Vault-beleid toegangsmachtigingen.

Voer nu de volgende opdrachten uit in PowerShell:

```powershell
# Connect to Azure AD 
Connect-AzureAD 
 
# Create Azure Active Directory Security Group 
$aadGroup = New-AzureADGroup -Description "Contoso App Group" -DisplayName "ContosoAppGroup" -MailEnabled 0 -MailNickName none -SecurityEnabled 1 
 
# Find and add your applications (ServicePrincipal ObjectID) as members to this group 
$spn = Get-AzureADServicePrincipal –SearchString "ContosoApp1" 
Add-AzureADGroupMember –ObjectId $aadGroup.ObjectId -RefObjectId $spn.ObjectId 
 
# You can add several members to this group, in this fashion. 
 
# Set the Key Vault ACLs 
Set-AzureRmKeyVaultAccessPolicy –VaultName ContosoVault –ObjectId $aadGroup.ObjectId `
-PermissionsToKeys decrypt,encrypt,unwrapKey,wrapKey,verify,sign,get,list,update,create,import,delete,backup,restore,recover,purge `
–PermissionsToSecrets get,list,set,delete,backup,restore,recover,purge `
–PermissionsToCertificates get,list,delete,create,import,update,managecontacts,getissuers,listissuers,setissuers,deleteissuers,manageissuers,recover,purge,backup,restore `
-PermissionsToStorage get,list,delete,set,update,regeneratekey,getsas,listsas,deletesas,setsas,recover,backup,restore,purge 
 
# Of course you can adjust the permissions as required 
```

Als u een andere set machtigingen aan een groep van toepassingen te verlenen wilt, maakt u een afzonderlijke Azure Active Directory-beveiligingsgroep voor deze toepassingen.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het [uw key vault beveiligen](key-vault-secure-your-key-vault.md).
