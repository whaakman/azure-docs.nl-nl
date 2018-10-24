---
title: PowerShell gebruiken voor het maken van een Azure AD-app voor toegang tot de API van Azure Media Services | Microsoft Docs
description: Leer hoe u PowerShell gebruiken voor een Azure Active Directory (Azure AD)-app maken en instellen voor het toegang tot de Azure Media Services-API.
services: media-services
documentationcenter: ''
author: Juliako
manager: cfowler
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: juliako
ms.openlocfilehash: 2aa777cf0c2068a5ee2382a9e75fc65db8a27207
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/24/2018
ms.locfileid: "49957298"
---
# <a name="use-powershell-to-create-an-azure-ad-app-to-use-with-the-azure-media-services-api"></a>PowerShell gebruiken voor het maken van een Azure AD-app voor gebruik met de Azure Media Services-API

Leer hoe u een PowerShell-script gebruiken om te maken van een Azure Active Directory (Azure AD)-toepassing en service-principal toegang krijgen tot bronnen van Azure Media Services.  

## <a name="prerequisites"></a>Vereisten

- Een Azure-account. Als u geen account hebt, beginnen met een [gratis proefversie van Azure](https://azure.microsoft.com/pricing/free-trial/). 
- Een Media Services-account. Zie voor meer informatie, [een Azure Media Services-account maken in Azure portal](media-services-portal-create-account.md).
- Azure PowerShell-versie 0.8.8 of een latere versie. Zie voor meer informatie, [hoe u Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).
- Azure Resource Manager-cmdlets.  

## <a name="create-an-azure-ad-app-by-using-powershell"></a>Een Azure AD-app maken met behulp van PowerShell  

```powershell
Connect-AzureRmAccount
Import-Module AzureRM.Resources
Set-AzureRmContext -SubscriptionId $SubscriptionId
$ServicePrincipal = New-AzureRMADServicePrincipal -DisplayName $ApplicationDisplayName -Password $Password

Get-AzureRmADServicePrincipal -ObjectId $ServicePrincipal.Id 
$NewRole = $null
$Scope = "/subscriptions/your subscription id/resourceGroups/userresourcegroup/providers/microsoft.media/mediaservices/your media account"

$Retries = 0;While ($NewRole -eq $null -and $Retries -le 6)
{
    # Sleep here for a few seconds to allow the service principal application to become active (usually, it will take only a couple of seconds)
    Sleep 15
    New-AzureRMRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $ServicePrincipal.ApplicationId -Scope $Scope | Write-Verbose -ErrorAction SilentlyContinue
    $NewRole = Get-AzureRMRoleAssignment -ServicePrincipalName $ServicePrincipal.ApplicationId -ErrorAction SilentlyContinue
    $Retries++;
}
```

Raadpleeg voor meer informatie de volgende artikelen:

- [Azure PowerShell gebruiken om een service-principal te maken voor toegang tot resources](../../active-directory/develop/howto-authenticate-service-principal-powershell.md)
- [Toegangsbeheer op basis van rollen beheren met behulp van Azure PowerShell](../../role-based-access-control/role-assignments-powershell.md)
- [Het handmatig daemon-apps configureren met behulp van certificaten](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential/blob/master/Manual-Configuration-Steps.md#add-the-certificate-as-a-key-for-the-todolistdaemonwithcert-application-in-azure-ad)

## <a name="next-steps"></a>Volgende stappen

Aan de slag met [bestanden uploaden naar uw account](media-services-portal-upload-files.md).
