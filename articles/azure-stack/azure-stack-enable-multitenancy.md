---
title: Multitenancy in Azure Stack
description: Leer hoe u ondersteuning voor meerdere Azure Active Directory-mappen in Azure Stack
services: azure-stack
documentationcenter: ''
author: PatAltimore
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/6/2018
ms.author: patricka
ms.reviewer: bryanr
ms.openlocfilehash: 28bd314e2dd179d83d2880e3acbf39805b54d333
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/14/2019
ms.locfileid: "54263953"
---
# <a name="multi-tenancy-in-azure-stack"></a>Multitenancy in Azure Stack

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

U kunt Azure Stack ter ondersteuning van gebruikers van meerdere tenants van Azure Active Directory (Azure AD) om services te gebruiken in Azure Stack kunt configureren. Neem bijvoorbeeld het volgende scenario:

- U kunt de Service-beheerder van contoso.onmicrosoft.com, waarin Azure Stack is geïnstalleerd.
- Mary is de Directory-beheerder van fabrikam.onmicrosoft.com, waar gastgebruikers ook kunnen zich bevinden.
- Koos het bedrijf ontvangt IaaS en PaaS-services van uw bedrijf en moet toestaan dat gebruikers van de Gast-map (fabrikam.onmicrosoft.com) om te melden en Azure Stack-resources gebruiken in contoso.onmicrosoft.com.

Deze handleiding bevat de stappen die nodig zijn, in de context van dit scenario multitenancy configureren in Azure Stack. In dit scenario, moet u en Mary stappen voor het inschakelen van gebruikers van Fabrikam aanmelden en-services uit de Azure Stack-implementatie in Contoso gebruiken voltooien.  

## <a name="enable-multi-tenancy"></a>Multitenancy inschakelen

Er zijn enkele vereisten ter compensatie van voordat u multitenancy in Azure Stack configureert:
  
 - U en Mary moet coördineren met beheerdersrechten stappen in de map die Azure Stack is geïnstalleerd in (Contoso) en de Gast-map (Fabrikam).  
 - Zorg ervoor dat u hebt [geïnstalleerd](azure-stack-powershell-install.md) en [geconfigureerd](azure-stack-powershell-configure-admin.md) PowerShell voor Azure Stack.
 - [Download de Azure Stack-Tools](azure-stack-powershell-download.md), en de verbinding en identiteit-modules importeren:

    ````PowerShell  
    Import-Module .\Connect\AzureStack.Connect.psm1
    Import-Module .\Identity\AzureStack.Identity.psm1
    ````

### <a name="configure-azure-stack-directory"></a>Azure Stack-directory configureren

In deze sectie configureert u Azure Stack-aanmeldingen dat van Fabrikam Azure AD directory-tenants.

Onboard de Gast Directory-Tenant (Fabrikam) naar Azure Stack door het configureren van Azure Resource Manager om te accepteren van gebruikers en service-principals van de Gast directory-tenant.

De Service-beheerder van contoso.onmicrosoft.com wordt uitgevoerd de volgende opdrachten.

````PowerShell  
## The following Azure Resource Manager endpoint is for the ASDK. If you are in a multinode environment, contact your operator or service provider to get the endpoint.
$adminARMEndpoint = "https://adminmanagement.local.azurestack.external"

## Replace the value below with the Azure Stack directory
$azureStackDirectoryTenant = "contoso.onmicrosoft.com"

## Replace the value below with the guest tenant directory. 
$guestDirectoryTenantToBeOnboarded = "fabrikam.onmicrosoft.com"

## Replace the value below with the name of the resource group in which the directory tenant registration resource should be created (resource group must already exist).
$ResourceGroupName = "system.local"

## Replace the value below with the region location of the resource group. 
$location = "local"

Register-AzSGuestDirectoryTenant -AdminResourceManagerEndpoint $adminARMEndpoint `
 -DirectoryTenantName $azureStackDirectoryTenant `
 -GuestDirectoryTenantName $guestDirectoryTenantToBeOnboarded `
 -Location $location `
 -ResourceGroupName $ResourceGroupName
````

### <a name="configure-guest-directory"></a>Gastenlijst configureren

Zodra de beheerder van de Azure Stack / operator de Fabrikam-map moet worden gebruikt met Azure Stack is ingeschakeld, koos Azure Stack met directory-tenant van Fabrikam moet registreren.

#### <a name="registering-azure-stack-with-the-guest-directory"></a>Azure Stack registreren met de Gast-map

De Directory-beheerder van Fabrikam Mary wordt uitgevoerd de volgende opdrachten in de Gast directory fabrikam.onmicrosoft.com.

````PowerShell
## The following Azure Resource Manager endpoint is for the ASDK. If you are in a multinode environment, contact your operator or service provider to get the endpoint.
$tenantARMEndpoint = "https://management.local.azurestack.external"
    
## Replace the value below with the guest tenant directory. 
$guestDirectoryTenantName = "fabrikam.onmicrosoft.com"

Register-AzSWithMyDirectoryTenant `
 -TenantResourceManagerEndpoint $tenantARMEndpoint `
 -DirectoryTenantName $guestDirectoryTenantName `
 -Verbose 
````

> [!IMPORTANT]
> Als uw Azure Stack-beheerder in de toekomst nieuwe services of updates installeert, moet u mogelijk met dit script opnieuw uitvoeren.
>
> Voer dit script opnieuw uit op elk gewenst moment om te controleren of de status van de Azure Stack-toepassingen in uw directory.
>
> Als u problemen met het maken van virtuele machines in Managed Disks (geïntroduceerd bij de update 1808), een nieuwe opgevallen **schijf Resource Provider** is toegevoegd, die met dit script opnieuw te worden uitgevoerd.

### <a name="direct-users-to-sign-in"></a>Directe gebruikers zich aanmelden

Nu dat u en Mary de stappen voor onboarding Mary directory hebt voltooid, kan Fabrikam gebruikers zich aanmelden door Mary sturen.  Fabrikam-gebruikers (dat wil zeggen, de gebruikers met het achtervoegsel fabrikam.onmicrosoft.com) zich aanmelden door naar de pagina https://portal.local.azurestack.external.  

Mary leidt een [externe principals](../role-based-access-control/rbac-and-directory-admin-roles.md) in de map Fabrikam (dat wil zeggen, de gebruikers in de map Fabrikam zonder het achtervoegsel van fabrikam.onmicrosoft.com) aan te melden met https://portal.local.azurestack.external/fabrikam.onmicrosoft.com.  Als ze deze URL niet gebruikt, ze worden verzonden naar de standaardmap (Fabrikam) en een foutbericht weergegeven dat de beheerder nog niet is toegestaan.

## <a name="disable-multi-tenancy"></a>Multitenancy uitschakelen

Als u meerdere tenants in Azure Stack niet meer wilt, kunt u meerdere tenants uitschakelen door de volgende stappen in volgorde:

1. Als de beheerder van de Gast-map (Mary in dit scenario), voer *Unregister-AzsWithMyDirectoryTenant*. De cmdlet Hiermee verwijdert u alle Azure Stack-toepassingen van de nieuwe map.

    ``` PowerShell
    ## The following Azure Resource Manager endpoint is for the ASDK. If you are in a multinode environment, contact your operator or service provider to get the endpoint.
    $tenantARMEndpoint = "https://management.local.azurestack.external"
        
    ## Replace the value below with the guest tenant directory. 
    $guestDirectoryTenantName = "fabrikam.onmicrosoft.com"
    
    Unregister-AzsWithMyDirectoryTenant `
     -TenantResourceManagerEndpoint $tenantARMEndpoint `
     -DirectoryTenantName $guestDirectoryTenantName `
     -Verbose 
    ```

2. Als de servicebeheerder van Azure Stack (u in dit scenario), voer *Unregister-AzSGuestDirectoryTenant*. 

    ``` PowerShell  
    ## The following Azure Resource Manager endpoint is for the ASDK. If you are in a multinode environment, contact your operator or service provider to get the endpoint.
    $adminARMEndpoint = "https://adminmanagement.local.azurestack.external"
    
    ## Replace the value below with the Azure Stack directory
    $azureStackDirectoryTenant = "contoso.onmicrosoft.com"
    
    ## Replace the value below with the guest tenant directory. 
    $guestDirectoryTenantToBeDecommissioned = "fabrikam.onmicrosoft.com"
    
    ## Replace the value below with the name of the resource group in which the directory tenant registration resource should be created (resource group must already exist).
    $ResourceGroupName = "system.local"
    
    Unregister-AzSGuestDirectoryTenant -AdminResourceManagerEndpoint $adminARMEndpoint `
     -DirectoryTenantName $azureStackDirectoryTenant `
     -GuestDirectoryTenantName $guestDirectoryTenantToBeDecommissioned `
     -ResourceGroupName $ResourceGroupName
    ```

    > [!WARNING]
    > De stappen van de multitenancy uitschakelen moeten worden uitgevoerd in volgorde. Stap 1 # mislukt als stap #2 eerst is voltooid.

## <a name="next-steps"></a>Volgende stappen

- [Gedelegeerde providers beheren](azure-stack-delegated-provider.md)
- [Azure Stack-basisbegrippen](azure-stack-key-features.md)
