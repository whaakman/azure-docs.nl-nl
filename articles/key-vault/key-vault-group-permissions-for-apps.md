---
title: Veel toepassingen toegang verlenen tot een Azure-sleutel kluis-Azure Key Vault | Microsoft Docs
description: Meer informatie over het verlenen van machtigingen aan veel toepassingen om toegang te krijgen tot een sleutel kluis
services: key-vault
author: amitbapat
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.topic: tutorial
ms.date: 08/12/2019
ms.author: ambapat
ms.openlocfilehash: 07ee544057ffeb0a5859cc771b124523ec79c9c0
ms.sourcegitcommit: 62bd5acd62418518d5991b73a16dca61d7430634
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/13/2019
ms.locfileid: "68976406"
---
# <a name="grant-several-applications-access-to-a-key-vault"></a>Verschillende toepassingen toegang verlenen tot een sleutel kluis

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Toegangs beheer beleid kan worden gebruikt om verschillende toepassingen toegang te verlenen tot een sleutel kluis. Een toegangscontrole beleid kan Maxi maal 1024 toepassingen ondersteunen en wordt als volgt geconfigureerd:

1. Een Azure Active Directory beveiligings groep maken. 
2. Voeg alle bijbehorende service-principals van de toepassing toe aan de beveiligings groep.
3. Verleen de beveiligings groep toegang tot uw Key Vault.

## <a name="prerequisites"></a>Vereisten

Dit zijn de vereisten:
* [Installeer Azure PowerShell](/powershell/azure/overview).
* [Installeer de Azure Active Directory v2 Power shell-module](https://www.powershellgallery.com/packages/AzureAD).
* Machtigingen voor het maken/bewerken van groepen in de Azure Active Directory Tenant. Als u geen machtigingen hebt, moet u mogelijk contact opnemen met uw Azure Active Directory-beheerder. Zie [informatie over Azure Key Vault sleutels, geheimen en certificaten](about-keys-secrets-and-certificates.md) voor meer informatie over Key Vault machtigingen voor toegangs beleid.

## <a name="granting-key-vault-access-to-applications"></a>Key Vault toegang verlenen tot toepassingen

Voer de volgende opdrachten uit in Power shell:

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
Set-AzKeyVaultAccessPolicy –VaultName ContosoVault –ObjectId $aadGroup.ObjectId `
-PermissionsToKeys decrypt,encrypt,unwrapKey,wrapKey,verify,sign,get,list,update,create,import,delete,backup,restore,recover,purge `
–PermissionsToSecrets get,list,set,delete,backup,restore,recover,purge `
–PermissionsToCertificates get,list,delete,create,import,update,managecontacts,getissuers,listissuers,setissuers,deleteissuers,manageissuers,recover,purge,backup,restore `
-PermissionsToStorage get,list,delete,set,update,regeneratekey,getsas,listsas,deletesas,setsas,recover,backup,restore,purge 
 
# Of course you can adjust the permissions as required 
```

Als u een andere set machtigingen voor een groep toepassingen wilt verlenen, maakt u een afzonderlijke Azure Active Directory beveiligings groep voor dergelijke toepassingen.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over hoe u [uw sleutel kluis kunt beveiligen](key-vault-secure-your-key-vault.md).
