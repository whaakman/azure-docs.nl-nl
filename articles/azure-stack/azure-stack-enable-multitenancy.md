---
title: Multi-tenancymodus in Azure-Stack inschakelen | Microsoft Docs
description: Meer informatie over het ter ondersteuning van meerdere directory's van Azure Active Directory in Azure-Stack
services: azure-stack
documentationcenter: 
author: HeathL17
manager: byronr
editor: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: helaw
ms.openlocfilehash: a2cba85a553f20040d2fb118b35859b05870e361
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="enable-multi-tenancy-in-azure-stack"></a>Multi-tenancymodus in Azure-Stack inschakelen

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

U kunt Azure-Stack ter ondersteuning van gebruikers van meerdere tenants van Azure Active Directory (Azure AD) om services te gebruiken in Azure-Stack configureren. Neem bijvoorbeeld het volgende scenario:

 - Bent u de Service-beheerder van contoso.onmicrosoft.com, waarbij Azure-Stack is geïnstalleerd.
 - Mary is de Directory-beheerder van fabrikam.onmicrosoft.com, waarin gastgebruikers bevinden. 
 - Bedrijf koos van IaaS en PaaS-services van uw bedrijf ontvangt en moet toestaan dat gebruikers van de Gast-map (fabrikam.onmicrosoft.com) aanmelden en resources voor Azure-Stack in contoso.onmicrosoft.com gebruiken.

Deze handleiding bevat de stappen vereist, in de context van dit scenario voor het configureren van multi-tenancymodus in Azure-Stack.  In dit scenario moet u en koos stappen om gebruikers van Fabrikam aanmelden en services van de implementatie van de Azure-Stack in Contoso verbruiken voltooien.  

## <a name="before-you-begin"></a>Voordat u begint
Er zijn enkele vereisten ter compensatie van voordat u multi-tenancymodus in Azure-Stack configureren:
  
 - U en koos moet coördineren beheerdersrechten stappen in de map die Azure-Stack is geïnstalleerd in (Contoso) en de Gast-map (Fabrikam).  
 - Zorg ervoor dat u hebt [geïnstalleerd](azure-stack-powershell-install.md) en [geconfigureerd](azure-stack-powershell-configure-admin.md) PowerShell voor Azure-Stack.
 - [Download de Azure-Stack-Tools](azure-stack-powershell-download.md), en de modules Connect en identiteit importeren:

    ````PowerShell
        Import-Module .\Connect\AzureStack.Connect.psm1
        Import-Module .\Identity\AzureStack.Identity.psm1
    ```` 
 - Mary moet [VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn) toegang tot Azure-Stack. 

## <a name="configure-azure-stack-directory"></a>Azure-Stack directory configureren
In deze sectie configureert u Azure-Stack zodat aanmeldingen van Fabrikam Azure AD-directory-tenants.

### <a name="onboard-guest-directory-tenant"></a>Directory-tenant vrijgeven Gast
Next, vrijgeven de Gast Directory-Tenant (Fabrikam) naar Azure-Stack.  Deze stap configureert u Azure Resource Manager voor het accepteren van gebruikers en service-principals van de Gast directory-tenant.

````PowerShell
$adminARMEndpoint = "https://adminmanagement.local.azurestack.external"

## Replace the value below with the Azure Stack directory
$azureStackDirectoryTenant = "contoso.onmicrosoft.com"

## Replace the value below with the guest tenant directory. 
$guestDirectoryTenantToBeOnboarded = "fabrikam.onmicrosoft.com"

Register-AzSGuestDirectoryTenant -AdminResourceManagerEndpoint $adminARMEndpoint `
 -DirectoryTenantName $azureStackDirectoryTenant `
 -GuestDirectoryTenantName $guestDirectoryTenantToBeOnboarded `
 -Location "local"
````



## <a name="configure-guest-directory"></a>Gast directory configureren
Nadat u de stappen in de map Azure Stack hebt voltooid, moet koos toestemming verlenen aan Azure-Stack toegang tot de Gast-directory en Azure Stack registreren bij de directory van de Gast. 

### <a name="registering-azure-stack-with-the-guest-directory"></a>Azure-Stack registreren door de Gast-directory
Nadat de Gast directory-beheerder hebt gekregen toestemming voor Azure-Stack voor toegang tot de map van Fabrikam, moeten deze Azure-Stack registreren bij Fabrikam van directory-tenant.

````PowerShell
$tenantARMEndpoint = "https://management.local.azurestack.external"
    
## Replace the value below with the guest tenant directory. 
$guestDirectoryTenantName = "fabrikam.onmicrosoft.com"

Register-AzSWithMyDirectoryTenant `
 -TenantResourceManagerEndpoint $tenantARMEndpoint `
 -DirectoryTenantName $guestDirectoryTenantName `
 -Verbose 
````
## <a name="direct-users-to-sign-in"></a>Directe gebruikers zich aanmelden in
Nu dat u en koos hebt voltooid voor de stappen voor het vrijgeven koos directory, kunt koos Fabrikam gebruikers zich aanmelden in doorsturen.  Fabrikam gebruikers (dat wil zeggen, de gebruikers met het achtervoegsel fabrikam.onmicrosoft.com) aanmelden https://portal.local.azurestack.external bezoeken.  

Mary leidt een [afwijkende beveiligings-principals](../active-directory/active-directory-understanding-resource-access.md) in de map Fabrikam (dat wil zeggen, de gebruikers in de map Fabrikam zonder het achtervoegsel van fabrikam.onmicrosoft.com) aan te melden met https://portal.local.azurestack.external/fabrikam.onmicrosoft.com.  Als ze deze URL niet gebruikt, moet deze worden verzonden naar de standaardmap (Fabrikam) en een foutbericht gemeld dat er dat niet door de beheerder heeft ingestemd.

## <a name="next-steps"></a>Volgende stappen

- [Gedelegeerde providers beheren](azure-stack-delegated-provider.md)
- [Belangrijkste concepten van Azure Stack](azure-stack-key-features.md)
