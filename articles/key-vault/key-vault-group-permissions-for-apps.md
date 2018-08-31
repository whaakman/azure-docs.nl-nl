---
title: Veel toepassingen om een Azure-sleutelkluis toegang te verlenen tot | Microsoft Docs
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
ms.devlang: na
ms.topic: article
ms.date: 12/01/2016
ms.author: ambapat
ms.openlocfilehash: 14e6e8bb723eb236f8fb315454b8697a3bd947ef
ms.sourcegitcommit: f94f84b870035140722e70cab29562e7990d35a3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/30/2018
ms.locfileid: "43286424"
---
# <a name="grant-permission-to-many-applications-to-access-a-key-vault"></a>Machtiging verlenen aan veel toepassingen toegang tot een key vault

## <a name="q-i-have-several-applications-that-need-to-access-a-key-vault-how-can-i-give-these-applications-up-to-1024-access-to-key-vault"></a>V: ik heb verschillende toepassingen die toegang moeten krijgen tot een key vault, hoe kan ik deze toepassingen (maximaal 1024) geen toegang verlenen tot Key Vault?

Toegangsbeleid voor Key Vault ondersteunt maximaal 1024 vermeldingen. U kunt echter een Azure Active Directory-beveiligingsgroep maken. Alle gekoppelde service-principals toevoegen aan deze beveiligingsgroep en vervolgens toegang verlenen aan deze beveiligingsgroep aan Key Vault.

Hier volgen de vereisten:
* [Azure Active Directory V2 PowerShell-module installeren](https://www.powershellgallery.com/packages/AzureAD).
* [Installeer Azure PowerShell](/powershell/azure/overview).
* Als u wilt de volgende opdrachten uitvoeren, moet u machtigingen voor groepen in de Azure Active Directory-tenant maken/bewerken. Als u geen machtigingen hebt, moet u mogelijk contact op met uw Azure Active Directory-beheerder.

Voer nu de volgende opdrachten uit in PowerShell.

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

Als u een andere set machtigingen aan een groep van toepassingen te verlenen wilt, maakt u een afzonderlijke Azure Active Directory-beveiligingsgroep voor deze toepassingen.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het [uw key vault beveiligen](key-vault-secure-your-key-vault.md).
