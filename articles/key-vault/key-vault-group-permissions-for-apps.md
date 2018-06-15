---
title: Machtiging verlenen om te veel toepassingen voor toegang tot een Azure sleutelkluis | Microsoft Docs
description: Meer informatie over het machtiging verlenen om te veel toepassingen voor toegang tot een sleutelkluis
services: key-vault
documentationcenter: ''
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
ms.openlocfilehash: ddeaf184138bd48d324799ddb45248b0a0ee8eeb
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
ms.locfileid: "30174969"
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
Set-AzureRmKeyVaultAccessPolicy –VaultName ContosoVault –ObjectId $aadGroup.ObjectId -PermissionsToKeys all –PermissionsToSecrets all –PermissionsToCertificates all 
 
# Of course you can adjust the permissions as required 
```

Als u een andere set machtigingen aan een groep van toepassingen te verlenen moet, maakt u een afzonderlijke Azure Active Directory-beveiligingsgroep voor dergelijke toepassingen.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het [beveiligen van uw sleutelkluis](key-vault-secure-your-key-vault.md).
