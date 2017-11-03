---
title: Machtiging verlenen om te veel toepassingen voor toegang tot een Azure sleutelkluis | Microsoft Docs
description: Meer informatie over het machtiging verlenen om te veel toepassingen voor toegang tot een sleutelkluis
services: key-vault
documentationcenter: 
author: amitbapat
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 785d4e40-fb7b-485a-8cbc-d9c8c87708e6
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/01/2016
ms.author: ambapat
ms.openlocfilehash: 14da9256def60d678ef5cae795fef1c373914b5c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="grant-permission-to-many-applications-to-access-a-key-vault"></a>Machtiging verlenen om te veel toepassingen voor toegang tot een sleutelkluis

## <a name="q-i-have-several-over-16-applications-that-need-to-access-a-key-vault-since-key-vault-only-allows-16-access-control-entries-how-can-i-achieve-that"></a>V: ik heb meerdere (meer dan 16) toepassingen die toegang moeten krijgen tot een sleutelkluis. Hoe doe ik dit, aangezien in Key Vault maximaal 16 vermeldingen voor toegangsbeheer zijn toegestaan?

Beleid voor toegangsbeheer Sleutelkluis biedt alleen ondersteuning voor 16 vermeldingen. U kunt echter een Azure Active Directory-beveiligingsgroep maken. Alle gekoppelde service-principals toevoegen aan deze beveiligingsgroep en vervolgens toegang verlenen aan deze beveiligingsgroep voor Sleutelkluis.

Hier volgen de vereisten:
* [Installeer Azure Active Directory V2 PowerShell-module](https://www.powershellgallery.com/packages/AzureAD).
* [Installeer Azure PowerShell](/powershell/azure/overview).
* Als u wilt de volgende opdrachten uitvoeren, moet u machtigingen voor groepen in de Azure Active Directory-tenant maken/bewerken. Als u geen machtigingen hebt, moet u mogelijk contact opnemen met uw Azure Active Directory-beheerder.

Voer nu de volgende opdrachten in PowerShell.

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
Set-AzureRmKeyVaultAccessPolicy –VaultName ContosoVault –ObjectId $aadGroup.ObjectId -PermissionToKeys all –PermissionToSecrets all –PermissionToCertificates all 
 
# Of course you can adjust the permissions as required 
```

Als u een andere set machtigingen aan een groep van toepassingen te verlenen moet, maakt u een afzonderlijke Azure Active Directory-beveiligingsgroep voor dergelijke toepassingen.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het [beveiligen van uw sleutelkluis](key-vault-secure-your-key-vault.md).
