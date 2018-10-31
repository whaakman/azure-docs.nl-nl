---
title: Inschakelen van Azure Active Directory Domain Services met behulp van PowerShell | Microsoft Docs
description: Azure Active Directory Domain Services met behulp van PowerShell inschakelen
services: active-directory-ds
documentationcenter: ''
author: eringreenlee
manager: mtillman
editor: curtand
ms.assetid: d4bc5583-6537-4cd9-bc4b-7712fdd9272a
ms.service: active-directory
ms.component: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 12/06/2017
ms.author: ergreenl
ms.openlocfilehash: b58df5ebf5332688424ac6ed2eeb9679487bcdc4
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50240253"
---
# <a name="enable-azure-active-directory-domain-services-using-powershell"></a>Azure Active Directory Domain Services met behulp van PowerShell inschakelen
Dit artikel laat u het inschakelen van Azure Active Directory (AD) Domain Services met behulp van PowerShell.

## <a name="task-1-install-the-required-powershell-modules"></a>Taak 1: De vereiste PowerShell-modules installeren

### <a name="install-and-configure-azure-ad-powershell"></a>Azure AD PowerShell installeren en configureren
Volg de instructies in het artikel [installeren van de Azure AD PowerShell-module en maak verbinding met Azure AD](https://docs.microsoft.com/powershell/azure/active-directory/install-adv2?toc=%2fazure%2factive-directory-domain-services%2ftoc.json).

### <a name="install-and-configure-azure-powershell"></a>Azure PowerShell installeren en configureren
Volg de instructies in het artikel [installeren van de Azure PowerShell-module en maak verbinding met uw Azure-abonnement](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?toc=%2fazure%2factive-directory-domain-services%2ftoc.json).


## <a name="task-2-create-the-required-service-principal-in-your-azure-ad-directory"></a>Taak 2: De vereiste service-principal in uw Azure AD-directory maken
Typ de volgende PowerShell-opdracht voor het maken van de service-principal die is vereist voor Azure AD Domain Services in uw Azure AD-directory.
```powershell
# Create the service principal for Azure AD Domain Services.
New-AzureADServicePrincipal -AppId "2565bd9d-da50-47d4-8b85-4c97f669dc36"
```

## <a name="task-3-create-and-configure-the-aad-dc-administrators-group"></a>Taak 3: Maken en configureren van de groep 'AAD DC Administrators'
De volgende taak is het maken van de groep Administrators die wordt gebruikt voor het delegeren van beheertaken in uw beheerde domein.
```powershell
# Create the delegated administration group for AAD Domain Services.
New-AzureADGroup -DisplayName "AAD DC Administrators" `
  -Description "Delegated group to administer Azure AD Domain Services" `
  -SecurityEnabled $true -MailEnabled $false `
  -MailNickName "AADDCAdministrators"
```

Nu dat u de groep hebt gemaakt, moet u een aantal gebruikers toevoegen aan de groep.
```powershell
# First, retrieve the object ID of the newly created 'AAD DC Administrators' group.
$GroupObjectId = Get-AzureADGroup `
  -Filter "DisplayName eq 'AAD DC Administrators'" | `
  Select-Object ObjectId

# Now, retrieve the object ID of the user you'd like to add to the group.
$UserObjectId = Get-AzureADUser `
  -Filter "UserPrincipalName eq 'admin@contoso100.onmicrosoft.com'" | `
  Select-Object ObjectId

# Add the user to the 'AAD DC Administrators' group.
Add-AzureADGroupMember -ObjectId $GroupObjectId.ObjectId -RefObjectId $UserObjectId.ObjectId
```

## <a name="task-4-register-the-azure-ad-domain-services-resource-provider"></a>Taak 4: Registreer de resourceprovider van Azure AD Domain Services
Typ de volgende PowerShell-opdracht om de resourceprovider voor Azure AD Domain Services te registreren:
```powershell
# Register the resource provider for Azure AD Domain Services with Resource Manager.
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.AAD
```

## <a name="task-5-create-a-resource-group"></a>Taak 5: Een resourcegroep maken
Typ de volgende PowerShell-opdracht om een resourcegroep te maken:
```powershell
$ResourceGroupName = "ContosoAaddsRg"
$AzureLocation = "westus"

# Create the resource group.
New-AzureRmResourceGroup `
  -Name $ResourceGroupName `
  -Location $AzureLocation
```

U kunt het virtuele netwerk en het Azure AD Domain Services beheerde domein maken in deze resourcegroep.


## <a name="task-6-create-and-configure-the-virtual-network"></a>Taak 6: Maken en configureren van het virtuele netwerk
Maak nu het virtuele netwerk waarin u Azure AD Domain Services inschakelen. Zorg ervoor dat u een toegewezen subnet voor Azure AD Domain Services maken. Implementeer niet werkbelasting van virtuele machines in dit subnet toegewezen.

Typ de volgende PowerShell-opdrachten om te maken van een virtueel netwerk met een toegewezen subnet voor Azure AD Domain Services.

```powershell
$ResourceGroupName = "ContosoAaddsRg"
$VnetName = "DomainServicesVNet_WUS"

# Create the dedicated subnet for AAD Domain Services.
$AaddsSubnet = New-AzureRmVirtualNetworkSubnetConfig `
  -Name DomainServices `
  -AddressPrefix 10.0.0.0/24

$WorkloadSubnet = New-AzureRmVirtualNetworkSubnetConfig `
  -Name Workloads `
  -AddressPrefix 10.0.1.0/24

# Create the virtual network in which you will enable Azure AD Domain Services.
$Vnet=New-AzureRmVirtualNetwork `
  -ResourceGroupName $ResourceGroupName `
  -Location westus `
  -Name $VnetName `
  -AddressPrefix 10.0.0.0/16 `
  -Subnet $AaddsSubnet,$WorkloadSubnet
```


## <a name="task-7-provision-the-azure-ad-domain-services-managed-domain"></a>Taak 7: Richt het Azure AD Domain Services beheerde domein
Typ de volgende PowerShell-opdracht uit om Azure AD Domain Services voor uw directory:

```powershell
$AzureSubscriptionId = "YOUR_AZURE_SUBSCRIPTION_ID"
$ManagedDomainName = "contoso100.com"
$ResourceGroupName = "ContosoAaddsRg"
$VnetName = "DomainServicesVNet_WUS"
$AzureLocation = "westus"

# Enable Azure AD Domain Services for the directory.
New-AzureRmResource -ResourceId "/subscriptions/$AzureSubscriptionId/resourceGroups/$ResourceGroupName/providers/Microsoft.AAD/DomainServices/$ManagedDomainName" `
  -Location $AzureLocation `
  -Properties @{"DomainName"=$ManagedDomainName; `
    "SubnetId"="/subscriptions/$AzureSubscriptionId/resourceGroups/$ResourceGroupName/providers/Microsoft.Network/virtualNetworks/$VnetName/subnets/DomainServices"} `
  -ApiVersion 2017-06-01 -Force -Verbose
```

> [!WARNING]
> **Vergeet niet de aanvullende configuratiestappen na het inrichten van uw beheerde domein.**
> Nadat uw beheerde domein is ingericht, moet u nog steeds de volgende taken uitvoeren:
> * **[DNS-instellingen bijwerken](active-directory-ds-getting-started-dns.md)**  voor het virtuele netwerk, zodat virtuele machines het beheerde domein voor het lid van domein of -verificatie vinden kan.
* **[Wachtwoordsynchronisatie met Azure AD Domain Services inschakelen](active-directory-ds-getting-started-password-sync.md)**, zodat eindgebruikers kunt aanmelden bij het beheerde domein met hun bedrijfsreferenties.
>


## <a name="powershell-script"></a>PowerShell-script
Het PowerShell-script gebruikt voor het uitvoeren van alle taken die worden vermeld in dit artikel is lager dan. Kopieer het script en sla deze op een bestand met de extensie '.ps1'. Voer het script in PowerShell of met behulp van PowerShell Integrated Scripting Environment (ISE).

> [!NOTE]
> **Vereiste machtigingen voor het uitvoeren van dit script** voor Azure AD Domain Services hebt ingeschakeld, moet u de globale beheerder voor de Azure AD-directory. Daarnaast moet u ten minste bevoegdheden "Bijdrager" op het virtuele netwerk waarin Azure AD Domain Services inschakelen.
>

```powershell
# Change the following values to match your deployment.
$AaddsAdminUserUpn = "admin@contoso100.onmicrosoft.com"
$AzureSubscriptionId = "YOUR_AZURE_SUBSCRIPTION_ID"
$ManagedDomainName = "contoso100.com"
$ResourceGroupName = "ContosoAaddsRg"
$VnetName = "DomainServicesVNet_WUS"
$AzureLocation = "westus"

# Connect to your Azure AD directory.
Connect-AzureAD

# Login to your Azure subscription.
Connect-AzureRmAccount

# Create the service principal for Azure AD Domain Services.
New-AzureADServicePrincipal -AppId "2565bd9d-da50-47d4-8b85-4c97f669dc36"

# Create the delegated administration group for AAD Domain Services.
New-AzureADGroup -DisplayName "AAD DC Administrators" `
  -Description "Delegated group to administer Azure AD Domain Services" `
  -SecurityEnabled $true -MailEnabled $false `
  -MailNickName "AADDCAdministrators"

# First, retrieve the object ID of the newly created 'AAD DC Administrators' group.
$GroupObjectId = Get-AzureADGroup `
  -Filter "DisplayName eq 'AAD DC Administrators'" | `
  Select-Object ObjectId

# Now, retrieve the object ID of the user you'd like to add to the group.
$UserObjectId = Get-AzureADUser `
  -Filter "UserPrincipalName eq '$AaddsAdminUserUpn'" | `
  Select-Object ObjectId

# Add the user to the 'AAD DC Administrators' group.
Add-AzureADGroupMember -ObjectId $GroupObjectId.ObjectId -RefObjectId $UserObjectId.ObjectId

# Register the resource provider for Azure AD Domain Services with Resource Manager.
Register-AzureRmResourceProvider -ProviderNamespace Microsoft.AAD

# Create the resource group.
New-AzureRmResourceGroup `
  -Name $ResourceGroupName `
  -Location $AzureLocation

# Create the dedicated subnet for AAD Domain Services.
$AaddsSubnet = New-AzureRmVirtualNetworkSubnetConfig `
  -Name DomainServices `
  -AddressPrefix 10.0.0.0/24

$WorkloadSubnet = New-AzureRmVirtualNetworkSubnetConfig `
  -Name Workloads `
  -AddressPrefix 10.0.1.0/24

# Create the virtual network in which you will enable Azure AD Domain Services.
$Vnet=New-AzureRmVirtualNetwork `
  -ResourceGroupName $ResourceGroupName `
  -Location $AzureLocation `
  -Name $VnetName `
  -AddressPrefix 10.0.0.0/16 `
  -Subnet $AaddsSubnet,$WorkloadSubnet

# Enable Azure AD Domain Services for the directory.
New-AzureRmResource -ResourceId "/subscriptions/$AzureSubscriptionId/resourceGroups/$ResourceGroupName/providers/Microsoft.AAD/DomainServices/$ManagedDomainName" `
  -Location $AzureLocation `
  -Properties @{"DomainName"=$ManagedDomainName; `
    "SubnetId"="/subscriptions/$AzureSubscriptionId/resourceGroups/$ResourceGroupName/providers/Microsoft.Network/virtualNetworks/$VnetName/subnets/DomainServices"} `
  -ApiVersion 2017-06-01 -Force -Verbose
```

> [!WARNING]
> **Vergeet niet de aanvullende configuratiestappen na het inrichten van uw beheerde domein.**
> Nadat uw beheerde domein is ingericht, moet u nog steeds de volgende taken uitvoeren:
> * Werk de DNS-instellingen voor het virtuele netwerk, zodat virtuele machines het beheerde domein voor het lid van domein of verificatie vindt.
* Wachtwoordsynchronisatie met Azure AD Domain Services inschakelen, zodat eindgebruikers kan aanmelden bij het beheerde domein met hun bedrijfsreferenties.
>

## <a name="next-steps"></a>Volgende stappen
Nadat uw beheerde domein is gemaakt, de volgende configuratietaken uitvoeren, zodat u het beheerde domein kan gebruiken:

* [De DNS-serverinstellingen voor het virtuele netwerk om te verwijzen naar uw beheerde domein bijwerken](active-directory-ds-getting-started-dns.md)
* [Wachtwoordsynchronisatie inschakelen voor uw beheerde domein](active-directory-ds-getting-started-password-sync.md)
