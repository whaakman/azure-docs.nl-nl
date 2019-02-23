---
title: PowerShell gebruiken voor het maken van een Azure AD-app voor toegang tot de API van Azure Media Services | Microsoft Docs
description: Leer hoe u PowerShell gebruiken voor een Azure Active Directory (Azure AD)-app maken en instellen voor het toegang tot de Azure Media Services-API.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/10/2019
ms.author: juliako
ms.openlocfilehash: 13e4b99da6de7d9c1fb08edc80605d38ed07c6f5
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2019
ms.locfileid: "56727193"
---
# <a name="use-powershell-to-create-an-azure-ad-app-to-use-with-the-azure-media-services-api"></a>PowerShell gebruiken voor het maken van een Azure AD-app voor gebruik met de Azure Media Services-API

Leer hoe u een PowerShell-script gebruiken om te maken van een Azure Active Directory (Azure AD)-toepassing en service-principal toegang krijgen tot bronnen van Azure Media Services.  

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

- Een Azure-account. Als u geen account hebt, beginnen met een [gratis proefversie van Azure](https://azure.microsoft.com/pricing/free-trial/). 
- Een Media Services-account. Zie voor meer informatie, [een Azure Media Services-account maken in Azure portal](media-services-portal-create-account.md).

- Azure PowerShell. Zie voor meer informatie, [hoe u Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview).

## <a name="create-an-azure-ad-app-by-using-powershell"></a>Een Azure AD-app maken met behulp van PowerShell  

```powershell
Connect-AzAccount
Import-Module Az.Resources
Set-AzContext -SubscriptionId $SubscriptionId
$ServicePrincipal = New-AzADServicePrincipal -DisplayName $ApplicationDisplayName -Password $Password

Get-AzADServicePrincipal -ObjectId $ServicePrincipal.Id 
$NewRole = $null
$Scope = "/subscriptions/your subscription id/resourceGroups/userresourcegroup/providers/microsoft.media/mediaservices/your media account"

$Retries = 0;While ($NewRole -eq $null -and $Retries -le 6)
{
    # Sleep here for a few seconds to allow the service principal application to become active (usually, it will take only a couple of seconds)
    Sleep 15
    New-AzRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $ServicePrincipal.ApplicationId -Scope $Scope | Write-Verbose -ErrorAction SilentlyContinue
    $NewRole = Get-AzRoleAssignment -ServicePrincipalName $ServicePrincipal.ApplicationId -ErrorAction SilentlyContinue
    $Retries++;
}
```

Raadpleeg voor meer informatie de volgende artikelen:

- [Azure PowerShell gebruiken om een service-principal te maken voor toegang tot resources](../../active-directory/develop/howto-authenticate-service-principal-powershell.md)
- [Toegangsbeheer op basis van rollen beheren met behulp van Azure PowerShell](../../role-based-access-control/role-assignments-powershell.md)
- [Het handmatig daemon-apps configureren met behulp van certificaten](https://github.com/Azure-Samples/active-directory-dotnet-daemon-certificate-credential/blob/master/Manual-Configuration-Steps.md#add-the-certificate-as-a-key-for-the-todolistdaemonwithcert-application-in-azure-ad)

## <a name="next-steps"></a>Volgende stappen

Aan de slag met [bestanden uploaden naar uw account](media-services-portal-upload-files.md).
