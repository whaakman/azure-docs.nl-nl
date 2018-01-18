---
title: 'Azure Active Directory Domain Services: Configuratie Netwerkbeveiligingsgroep probleemoplossing | Microsoft Docs'
description: Het NSG-configuratie voor Azure AD Domain Services oplossen
services: active-directory-ds
documentationcenter: 
author: eringreenlee
manager: 
editor: 
ms.assetid: 95f970a7-5867-4108-a87e-471fa0910b8c
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2018
ms.author: ergreenl
ms.openlocfilehash: 447f9119ea379e278be77d8699c7dcb751ea3085
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="azure-ad-domain-services---troubleshooting-network-security-group-configuration"></a>Azure AD Domain Services - configuratie voor het oplossen van Netwerkbeveiligingsgroep



## <a name="aadds104-network-error"></a>AADDS104: Netwerkfout

**Waarschuwing:** *Microsoft kan niet tot de domeincontrollers voor dit beheerde domein. Dit kan gebeuren als een netwerkbeveiligingsgroep (NSG) geconfigureerd op uw virtuele netwerk blokkeert de toegang tot het beheerde domein. Een andere mogelijke oorzaak is als er een gebruiker gedefinieerde route blokken binnenkomend verkeer van het internet.*

De meest voorkomende oorzaak van netwerkfouten voor Azure AD Domain Services kan worden herleid op onjuiste NSG-configuraties. Om ervoor te zorgen dat Microsoft kunt service en onderhouden u beheerd domein, moet u een Netwerkbeveiligingsgroep (NSG) gebruiken voor toegang tot [bepaalde poorten](active-directory-ds-networking.md#ports-required-for-azure-ad-domain-services) binnen het subnet. Als deze poorten worden geblokkeerd, Microsoft geen toegang tot de resources die nodig is, die mogelijk schadelijk zijn voor uw service. Tijdens het maken van uw NSG, laat u deze poorten open zijn om ervoor te zorgen zonder onderbreking in de service.

## <a name="sample-nsg"></a>Voorbeeld NSG
De volgende tabel ziet u een voorbeeld van een NSG die uw beheerde domein beveiligde houdt terwijl Microsoft om te controleren, beheren en gegevens bijwerken.

![voorbeeld NSG](.\media\active-directory-domain-services-alerts\default-nsg.png)

>[!NOTE]
> Azure AD Domain Services vereist onbeperkte uitgaande toegang. Aangeraden een extra outboud-regel voor uw nsg's niet maken.

## <a name="adding-a-rule-to-a-network-security-group-using-the-azure-portal"></a>Een regel toe te voegen aan de beveiligingsgroep van een netwerk met de Azure portal
Als u niet gebruiken van PowerShell wilt, kunt u één regels handmatig toevoegen aan de nsg's met de Azure portal. Volg deze stappen voor het maken van regels in de beveiligingsgroep van uw netwerk.

1. Navigeer naar de [Netwerkbeveiligingsgroepen](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.Network%2FNetworkSecurityGroups) pagina in de Azure-portal
2. Kies het NSG die is gekoppeld aan uw domein uit de tabel.
3. Klik onder de instellingen in de linkernavigatiebalk **inkomende beveiligingsregels** of **uitgaande beveiligingsregels**.
4. De regel maken door te klikken op **toevoegen** en de gegevens in te vullen. Klik op **OK**.
5. Controleer of dat de regel is gemaakt door het zoeken in de regeltabel.


## <a name="create-a-default-nsg-using-powershell"></a>Maken van een standaard NSG met behulp van PowerShell

Voorafgaand aan zijn stappen die u gebruiken kunt voor het maken van een nieuwe NSG met behulp van PowerShell waarmee alle poorten die nodig zijn om uit te voeren van Azure AD Domain Services openen blijft terwijl de andere ongewenste toegang wordt geweigerd.


Deze oplossing vereist installeren en uitvoeren van [Azure AD Powershell](https://docs.microsoft.com/en-us/powershell/azure/active-directory/install-adv2?toc=%2Fazure%2Factive-directory-domain-services%2Ftoc.json&view=azureadps-2.0).

1. Verbinding maken met uw Azure AD-directory.

  ```PowerShell
  # Connect to your Azure AD directory.
  Connect-AzureAD
  ```
2. Aanmelden bij uw Azure-abonnement.

  ```PowerShell
  # Log in to your Azure subscription.
  Login-AzureRmAccount
  ```

3. Maak een NSG met drie regels. Het volgende script worden drie regels gedefinieerd voor het NSG die toegang geven tot de poorten die nodig zijn voor het uitvoeren van Azure AD Domain Services. Het script maakt vervolgens een nieuwe NSG waarin deze regels. U bent Welkom bij het toevoegen van extra regels naar wens met behulp van dezelfde indeling.

  ```PowerShell
  # Create the rules needed
  $rule1 = New-AzureRmNetworkSecurityRuleConfig -Name https-rule -Description "Allow HTTP" `
  -Access Allow -Protocol Tcp -Direction Inbound -Priority 101 `
  -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
  -DestinationPortRange 443
  $rule2 = New-AzureRmNetworkSecurityRuleConfig -Name manage-3389 -Description "Manage domain through port 3389" `
  -Access Allow -Protocol Tcp -Direction Inbound -Priority 102 `
  -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
  -DestinationPortRange 3389
  $rule3 = New-AzureRmNetworkSecurityRuleConfig -Name manage-5986 -Description "Manage domain through port 5986" `
  -Access Allow -Protocol Tcp -Direction Inbound -Priority 103 `
  -SourceAddressPrefix $serviceIPs -SourcePortRange * -DestinationAddressPrefix * `
  -DestinationPortRange 5986
  # Create the NSG with the 3 rules above
  $nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $resourceGroup -Location westus `
  -Name "AADDS-NSG" -SecurityRules $rule1,$rule2,$rule3
  ```

4. Het script wordt het NSG ten slotte wordt gekoppeld aan de vnet en het subnet van keuze.

  ```PowerShell
  # Find vnet and subnet
  $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $resourceGroup -Name $vnetName
  $subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name $subnetName
  # Set the nsg to the subnet and save the changes
  $subnet.NetworkSecurityGroup = $nsg
  Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
  ```

### <a name="full-script"></a>Volledige script

```PowerShell
#Change the following values to match your deployment
$resourceGroup = "ResourceGroupName"
$vnetName = "exampleVnet"
$subnetName = "exampleSubnet"

$serviceIPs = "52.180.183.8, 23.101.0.70, 52.225.184.198, 52.179.126.223, 13.74.249.156, 52.187.117.83, 52.161.13.95, 104.40.156.18, 104.40.87.209, 52.180.179.108, 52.175.18.134, 52.138.68.41, 104.41.159.212, 52.169.218.0, 52.187.120.237, 52.161.110.169, 52.174.189.149, 13.64.151.161"

# Create the rules needed
$rule1 = New-AzureRmNetworkSecurityRuleConfig -Name https-rule -Description "Allow HTTP" `
-Access Allow -Protocol Tcp -Direction Inbound -Priority 101 `
-SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
-DestinationPortRange 443

$rule2 = New-AzureRmNetworkSecurityRuleConfig -Name manage-3389 -Description "Manage domain through port 3389" `
-Access Allow -Protocol Tcp -Direction Inbound -Priority 102 `
-SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
-DestinationPortRange 3389

$rule3 = New-AzureRmNetworkSecurityRuleConfig -Name manage-5986 -Description "Manage domain through port 5986" `
-Access Allow -Protocol Tcp -Direction Inbound -Priority 103 `
-SourceAddressPrefix $serviceIPs -SourcePortRange * -DestinationAddressPrefix * `
-DestinationPortRange 3389


# Connect to your Azure AD directory.
Connect-AzureAD

# Log in to your Azure subscription.
Login-AzureRmAccount

# Create the NSG with the 3 rules above
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $resourceGroup -Location westus `
-Name "NSG-Default" -SecurityRules $rule1,$rule2,$rule3

# Find vnet and subnet
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $resourceGroup -Name $vnetName
$subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name $subnetName

# Set the nsg to the subnet and save the changes
$subnet.NetworkSecurityGroup = $nsg
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```

> [!NOTE]
>Deze standaardinstelling NSG wordt de toegang tot de poort die wordt gebruikt voor beveiligde LDAP niet vergrendeld. Als u wilt weten hoe u een regel maken voor deze poort, gaat u naar [in dit artikel](active-directory-ds-troubleshoot-ldaps.md).
>

## <a name="contact-us"></a>Contact opnemen
Neem contact op met de Azure Active Directory Domain Services-productteam voor [feedback delen of voor ondersteuning](active-directory-ds-contact-us.md).
