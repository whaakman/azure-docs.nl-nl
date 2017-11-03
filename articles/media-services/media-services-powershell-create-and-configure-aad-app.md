---
title: PowerShell gebruiken voor het maken van een Azure AD-app voor toegang tot de Azure Media Services-API | Microsoft Docs
description: Informatie over het gebruik PowerShell voor het maken van een app met Azure Active Directory (Azure AD) en stel deze toegang tot de API van Azure Media Services.
services: media-services
documentationcenter: 
author: Juliako
manager: cfowler
editor: 
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/17/2017
ms.author: juliako
ms.openlocfilehash: eea0f3a03dd77ce56484f32b192299bd97c05300
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="use-powershell-to-create-an-azure-ad-app-to-use-with-the-azure-media-services-api"></a>PowerShell gebruiken voor het maken van een Azure AD-app voor gebruik met de Azure Media Services-API

Informatie over het gebruik van een PowerShell-script om een Azure Active Directory (Azure AD)-toepassing en service-principal te krijgen tot bronnen in Azure Media Services te maken.  

## <a name="prerequisites"></a>Vereisten

- Een Azure-account. Als u geen account hebt, beginnen met een [gratis proefversie van Azure](https://azure.microsoft.com/pricing/free-trial/). 
- Een Media Services-account. Zie voor meer informatie [een Azure Media Services-account maken in de Azure-portal](media-services-portal-create-account.md).
- Azure PowerShell-versie 0.8.8 of een latere versie. Zie voor meer informatie [het gebruik van Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).
- Azure Resource Manager-cmdlets.  

## <a name="create-an-azure-ad-app-by-using-powershell"></a>Een Azure AD-app maken met behulp van PowerShell  

```powershell
Login-AzureRmAccount
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

- [Azure PowerShell gebruiken om een service-principal te maken voor toegang tot resources](../azure-resource-manager/resource-group-authenticate-service-principal.md)
- [Toegangsbeheer op basis van rollen beheren met behulp van Azure PowerShell](../active-directory/role-based-access-control-manage-access-powershell.md)
- [Het handmatig daemon apps configureren met behulp van certificaten](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential/blob/master/Manual-Configuration-Steps.md#add-the-certificate-as-a-key-for-the-todolistdaemonwithcert-application-in-azure-ad)

## <a name="next-steps"></a>Volgende stappen

Aan de slag met [bestanden uploaden naar uw account](media-services-portal-upload-files.md).
