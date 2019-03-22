---
title: Waarden ophalen voor app-verificatie - Azure SQL Database | Microsoft Docs
description: Een service-principal maken voor toegang tot SQL Database van code.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
manager: craigg
ms.date: 03/12/2019
ms.openlocfilehash: 1d60e875b12f02c957ebd6259eb0e7267f23ee51
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57897602"
---
# <a name="get-the-required-values-for-authenticating-an-application-to-access-sql-database-from-code"></a>De vereiste waarden voor het verifiëren van toegang tot SQL Database van de code van een toepassing ophalen

Als u wilt maken en beheren van SQL-Database vanuit code moet u uw app registreren in de Azure Active Directory (AAD)-domein in het abonnement waar uw Azure-resources zijn gemaakt.

## <a name="create-a-service-principal-to-access-resources-from-an-application"></a>Een service-principal maken voor toegang tot resources van een toepassing

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> De PowerShell Azure Resource Manager-module nog steeds wordt ondersteund door Azure SQL Database, maar alle toekomstige ontwikkeling is voor de module Az.Sql. Zie voor deze cmdlets [AzureRM.Sql](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). De argumenten voor de opdrachten in de Az-module en de AzureRm-modules zijn vrijwel identiek zijn.

Het volgende PowerShell-script maakt de Active Directory-toepassing (AD) en de service-principal die we nodig hebben om onze C#-app te verifiëren. Het script voert de waarden uit die we nodig hebben voor het voorgaande C#-voorbeeld. Zie [Use Azure PowerShell to create a service principal to access resources](../active-directory/develop/howto-authenticate-service-principal-powershell.md) (Azure PowerShell gebruiken om een service-principal te maken voor toegang tot resources) voor gedetailleerde informatie.

    # Sign in to Azure.
    Connect-AzAccount

    # If you have multiple subscriptions, uncomment and set to the subscription you want to work with.
    #$subscriptionId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}"
    #Set-AzContext -SubscriptionId $subscriptionId

    # Provide these values for your new AAD app.
    # $appName is the display name for your app, must be unique in your directory.
    # $uri does not need to be a real uri.
    # $secret is a password you create.

    $appName = "{app-name}"
    $uri = "http://{app-name}"
    $secret = "{app-password}"

    # Create an AAD app
    $azureAdApplication = New-AzADApplication -DisplayName $appName -HomePage $Uri -IdentifierUris $Uri -Password $secret

    # Create a Service Principal for the app
    $svcprincipal = New-AzADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId

    # To avoid a PrincipalNotFound error, I pause here for 15 seconds.
    Start-Sleep -s 15

    # If you still get a PrincipalNotFound error, then rerun the following until successful. 
    $roleassignment = New-AzRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $azureAdApplication.ApplicationId.Guid


    # Output the values we need for our C# application to successfully authenticate

    Write-Output "Copy these values into the C# sample app"

    Write-Output "_subscriptionId:" (Get-AzContext).Subscription.SubscriptionId
    Write-Output "_tenantId:" (Get-AzContext).Tenant.TenantId
    Write-Output "_applicationId:" $azureAdApplication.ApplicationId.Guid
    Write-Output "_applicationSecret:" $secret




## <a name="see-also"></a>Zie ook
* [Een SQL-database maken met C#](sql-database-get-started-csharp.md)
* [Verbinding maken met SQL Database met behulp van Azure Active Directory-verificatie](sql-database-aad-authentication.md)

