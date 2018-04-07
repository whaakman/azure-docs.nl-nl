---
title: Ophalen van waarden voor verificatie in app - Azure SQL Database | Microsoft Docs
description: Een service-principal maken voor toegang tot SQL-Database vanuit code.
services: sql-database
author: stevestein
manager: jhubbard
ms.service: sql-database
ms.custom: develop apps
ms.topic: article
ms.date: 04/01/2018
ms.author: sstein
ms.openlocfilehash: 9127362ec806b7b3cbf3971488bb5393eb170eff
ms.sourcegitcommit: 3a4ebcb58192f5bf7969482393090cb356294399
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/06/2018
---
# <a name="get-the-required-values-for-authenticating-an-application-to-access-sql-database-from-code"></a>De vereiste waarden voor het verifiëren van een toepassing voor toegang tot SQL-Database vanuit code ophalen
Maken en beheren van SQL-Database van de code die u moet uw app registreren in het domein met Azure Active Directory (AAD) in het abonnement waar uw Azure-resources zijn gemaakt.

## <a name="create-a-service-principal-to-access-resources-from-an-application"></a>Een service-principal maken voor toegang tot bronnen van een toepassing
Moet u beschikken over de meest recente [Azure PowerShell](https://msdn.microsoft.com/library/mt619274.aspx) geïnstalleerd en wordt uitgevoerd. Zie voor gedetailleerde informatie [Installeren en configureren van Azure PowerShell](/powershell/azureps-cmdlets-docs).

Het volgende PowerShell-script maakt de Active Directory-toepassing (AD) en de service-principal die we nodig hebben om onze C#-app te verifiëren. Het script voert de waarden uit die we nodig hebben voor het voorgaande C#-voorbeeld. Zie [Use Azure PowerShell to create a service principal to access resources](../azure-resource-manager/resource-group-authenticate-service-principal.md) (Azure PowerShell gebruiken om een service-principal te maken voor toegang tot resources) voor gedetailleerde informatie.

    # Sign in to Azure.
    Add-AzureRmAccount

    # If you have multiple subscriptions, uncomment and set to the subscription you want to work with.
    #$subscriptionId = "{xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx}"
    #Set-AzureRmContext -SubscriptionId $subscriptionId

    # Provide these values for your new AAD app.
    # $appName is the display name for your app, must be unique in your directory.
    # $uri does not need to be a real uri.
    # $secret is a password you create.

    $appName = "{app-name}"
    $uri = "http://{app-name}"
    $secret = "{app-password}"

    # Create a AAD app
    $azureAdApplication = New-AzureRmADApplication -DisplayName $appName -HomePage $Uri -IdentifierUris $Uri -Password $secret

    # Create a Service Principal for the app
    $svcprincipal = New-AzureRmADServicePrincipal -ApplicationId $azureAdApplication.ApplicationId

    # To avoid a PrincipalNotFound error, I pause here for 15 seconds.
    Start-Sleep -s 15

    # If you still get a PrincipalNotFound error, then rerun the following until successful. 
    $roleassignment = New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $azureAdApplication.ApplicationId.Guid


    # Output the values we need for our C# application to successfully authenticate

    Write-Output "Copy these values into the C# sample app"

    Write-Output "_subscriptionId:" (Get-AzureRmContext).Subscription.SubscriptionId
    Write-Output "_tenantId:" (Get-AzureRmContext).Tenant.TenantId
    Write-Output "_applicationId:" $azureAdApplication.ApplicationId.Guid
    Write-Output "_applicationSecret:" $secret




## <a name="see-also"></a>Zie ook
* [Een SQL-database maken met C#](sql-database-get-started-csharp.md)
* [Verbinding maken met SQL-Database via Azure Active Directory-verificatie](sql-database-aad-authentication.md)

