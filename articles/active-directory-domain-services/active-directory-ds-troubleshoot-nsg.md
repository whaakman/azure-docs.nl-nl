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
ms.date: 03/01/2018
ms.author: ergreenl
ms.openlocfilehash: b7010c2e8d5ca479411d101ce237709ad26d7bcc
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/02/2018
---
# <a name="troubleshoot-invalid-networking-configuration-for-your-managed-domain"></a>Ongeldige netwerkconfiguratie voor uw beheerde domein oplossen
In dit artikel helpt u problemen op te lossen netwerkgerelateerde configuratiefouten die in het volgende bericht weergegeven resulteren:

## <a name="alert-aadds104-network-error"></a>Waarschuwing AADDS104: Netwerkfout
**Waarschuwing:** *Microsoft kan niet tot de domeincontrollers voor dit beheerde domein. Dit kan gebeuren als een netwerkbeveiligingsgroep (NSG) geconfigureerd op uw virtuele netwerk blokkeert de toegang tot het beheerde domein. Een andere mogelijke reden is dat als er een gebruiker gedefinieerde route die inkomend verkeer van het internet wordt geblokkeerd.*

Ongeldige NSG-configuraties zijn de meest voorkomende oorzaak van netwerkfouten voor Azure AD Domain Services. De Netwerkbeveiligingsgroep (NSG) is geconfigureerd voor het virtuele netwerk toegang tot geven moet [bepaalde poorten](active-directory-ds-networking.md#ports-required-for-azure-ad-domain-services). Als deze poorten worden geblokkeerd, kan Microsoft controleren of bijwerken van uw beheerde domein. Bovendien wordt de synchronisatie tussen uw Azure AD-directory en uw beheerde domein beïnvloed. Tijdens het maken van uw NSG deze poorten open houden om te voorkomen in de service wordt onderbroken.


## <a name="sample-nsg"></a>voorbeeld NSG
De volgende tabel ziet u een voorbeeld van een NSG die uw beheerde domein beveiligde houdt terwijl Microsoft om te controleren, beheren en gegevens bijwerken.

![voorbeeld NSG](.\media\active-directory-domain-services-alerts\default-nsg.png)

>[!NOTE]
> Azure AD Domain Services vereist onbeperkte uitgaande toegang van het virtuele netwerk. We raden aan geen te maken van eventuele aanvullende NSG-regels die beperkt de uitgaande toegang tot het virtuele netwerk.

## <a name="add-a-rule-to-a-network-security-group-using-the-azure-portal"></a>Een regel toevoegen aan een Netwerkbeveiligingsgroep met de Azure portal
Als u niet gebruiken van PowerShell wilt, kunt u één regels handmatig toevoegen aan de nsg's met de Azure portal. Voor het maken van regels in de beveiligingsgroep van uw netwerk, moet u de volgende stappen uitvoeren:

1. Navigeer naar de [Netwerkbeveiligingsgroepen](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.Network%2FNetworkSecurityGroups) pagina in de Azure-portal
2. Kies uit de tabel de NSG die is gekoppeld aan het subnet waarin uw beheerde domein is ingeschakeld.
3. Onder **instellingen** in het deelvenster links, klikt u op **inkomende beveiligingsregels** of **uitgaande beveiligingsregels**.
4. De regel maken door te klikken op **toevoegen** en de gegevens in te vullen. Klik op **OK**.
5. Controleer of dat de regel is gemaakt door het zoeken in de regeltabel.


## <a name="create-an-nsg-for-azure-ad-domain-services-using-powershell"></a>Maken van een NSG voor Azure AD Domain Services met behulp van PowerShell
Deze NSG is geconfigureerd dat binnenkomend verkeer op de poorten die vereist zijn voor Azure AD Domain Services en andere ongewenste binnenkomende toegang weigert.

**Vereiste: Installeren en configureren van Azure PowerShell** Volg de instructies voor [Installeer de Azure PowerShell-module en maak verbinding met uw Azure-abonnement](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?toc=%2fazure%2factive-directory-domain-services%2ftoc.json).

>[!TIP]
> U wordt aangeraden de nieuwste versie van de Azure PowerShell-module. Als u al een oudere versie van de Azure PowerShell-module geïnstalleerd, bijgewerkt naar de nieuwste versie.
>

Gebruik de volgende stappen voor het maken van een nieuwe NSG met behulp van PowerShell.
1. Aanmelden bij uw Azure-abonnement.

  ```PowerShell
  # Log in to your Azure subscription.
  Login-AzureRmAccount
  ```

2. Maak een NSG met drie regels. Het volgende script worden drie regels gedefinieerd voor het NSG die toegang geven tot de poorten die nodig zijn voor het uitvoeren van Azure AD Domain Services. Het script maakt vervolgens een nieuwe NSG waarin deze regels. Gebruik dezelfde indeling om toe te voegen extra regels die andere binnenkomend verkeer toestaan indien vereist door de werkbelastingen die zijn geïmplementeerd in het virtuele netwerk.

  ```PowerShell
  # Allow inbound HTTPS traffic to enable synchronization to your managed domain.
  $SyncRule = New-AzureRmNetworkSecurityRuleConfig -Name AllowSyncWithAzureAD -Description "Allow synchronization with Azure AD" `
  -Access Allow -Protocol Tcp -Direction Inbound -Priority 101 `
  -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
  -DestinationPortRange 443

  # Allow management of your domain over port 5986 (PowerShell Remoting)
  $PSRemotingRule = New-AzureRmNetworkSecurityRuleConfig -Name AllowPSRemoting -Description "Allow management of domain through port 5986" `
  -Access Allow -Protocol Tcp -Direction Inbound -Priority 102 `
  -SourceAddressPrefix 52.180.183.8, 23.101.0.70, 52.225.184.198, 52.179.126.223, 13.74.249.156, 52.187.117.83, 52.161.13.95, 104.40.156.18, 104.40.87.209, 52.180.179.108, 52.175.18.134, 52.138.68.41, 104.41.159.212, 52.169.218.0, 52.187.120.237, 52.161.110.169, 52.174.189.149, 13.64.151.161 -SourcePortRange * -DestinationAddressPrefix * `
  -DestinationPortRange 5986

  #The following two rules are optional and needed only in certain situations.

  # Allow management of your domain over port 3389 (remote desktop).
  $RemoteDesktopRule = New-AzureRmNetworkSecurityRuleConfig -Name AllowRD -Description "Allow management of domain through port 3389" `
  -Access Allow -Protocol Tcp -Direction Inbound -Priority 103 `
  -SourceAddressPrefix 207.68.190.32/27, 13.106.78.32/27, 10.254.32.0/20, 10.97.136.0/22, 13.106.174.32/27, 13.106.4.96/27 -SourcePortRange * -DestinationAddressPrefix * `
  -DestinationPortRange 3389

  # Secure LDAP rule, it is recommended to change the source address prefix to include only the IP addresses
  $SecureLDAPRule = New-AzureRmNetworkSecurityRuleConfig -Name SecureLDAP -Description "Allow access through secure LDAP port" `
  -Access Allow -Protocol Tcp -Direction Inbound -Priority 104 `
  -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
  -DestinationPortRange 636

  # Create the NSG with the rules above (if you need the remote desktop rule and secure ldap rule, add it below)
  $nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $resourceGroup -Location westus `
  -Name "AADDomainServices-NSG" -SecurityRules $SyncRule, $PSRemotingRule
  ```

3. Ten slotte wordt het NSG koppelen met de vnet en het subnet van de keuze.

  ```PowerShell
  # Find vnet and subnet
  $Vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $ResourceGroup -Name $VnetName
  $Subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $Vnet -Name $SubnetName

  # Set the nsg to the subnet and save the changes
  $Subnet.NetworkSecurityGroup = $Nsg
  Set-AzureRmVirtualNetwork -VirtualNetwork $Vnet
  ```

## <a name="full-script-to-create-and-apply-an-nsg-for-azure-ad-domain-services"></a>Volledige script maken en toepassen van een NSG voor Azure AD Domain Services
>[!TIP]
> U wordt aangeraden de nieuwste versie van de Azure PowerShell-module. Als u al een oudere versie van de Azure PowerShell-module geïnstalleerd, bijgewerkt naar de nieuwste versie.
>

```PowerShell
# Change the following values to match your deployment
$ResourceGroup = "ResourceGroupName"
$Location = "westus"
$VnetName = "exampleVnet"
$SubnetName = "exampleSubnet"

# Log in to your Azure subscription.
Login-AzureRmAccount

# Allow inbound HTTPS traffic to enable synchronization to your managed domain.
$SyncRule = New-AzureRmNetworkSecurityRuleConfig -Name AllowSyncWithAzureAD -Description "Allow synchronization with Azure AD" `
-Access Allow -Protocol Tcp -Direction Inbound -Priority 101 `
-SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
-DestinationPortRange 443

# Allow management of your domain over port 5986 (PowerShell Remoting)
$PSRemotingRule = New-AzureRmNetworkSecurityRuleConfig -Name AllowPSRemoting -Description "Allow management of domain through port 5986" `
-Access Allow -Protocol Tcp -Direction Inbound -Priority 102 `
-SourceAddressPrefix 52.180.183.8, 23.101.0.70, 52.225.184.198, 52.179.126.223, 13.74.249.156, 52.187.117.83, 52.161.13.95, 104.40.156.18, 104.40.87.209, 52.180.179.108, 52.175.18.134, 52.138.68.41, 104.41.159.212, 52.169.218.0, 52.187.120.237, 52.161.110.169, 52.174.189.149, 13.64.151.161 -SourcePortRange * -DestinationAddressPrefix * `
-DestinationPortRange 5986

#The following two rules are optional and needed only in certain situations.

# Allow management of your domain over port 3389 (remote desktop).
$RemoteDesktopRule = New-AzureRmNetworkSecurityRuleConfig -Name AllowRD -Description "Allow management of domain through port 3389" `
-Access Allow -Protocol Tcp -Direction Inbound -Priority 103 `
-SourceAddressPrefix 207.68.190.32/27, 13.106.78.32/27, 10.254.32.0/20, 10.97.136.0/22, 13.106.174.32/27, 13.106.4.96/27 -SourcePortRange * -DestinationAddressPrefix * `
-DestinationPortRange 3389

# Secure LDAP rule, it is recommended to change the source address prefix to include only the IP addresses
$SecureLDAPRule = New-AzureRmNetworkSecurityRuleConfig -Name SecureLDAP -Description "Allow access through secure LDAP port" `
-Access Allow -Protocol Tcp -Direction Inbound -Priority 104 `
-SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
-DestinationPortRange 636

# Create the NSG with the rules above (if you need the remote desktop rule and secure ldap rule, add it below)
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $resourceGroup -Location westus `
-Name "AADDomainServices-NSG" -SecurityRules $SyncRule, $PSRemotingRule

# Find vnet and subnet
$Vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $ResourceGroup -Name $VnetName
$Subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $Vnet -Name $SubnetName

# Set the nsg to the subnet and save the changes
$Subnet.NetworkSecurityGroup = $Nsg
Set-AzureRmVirtualNetwork -VirtualNetwork $Vnet
```


## <a name="need-help"></a>Hulp nodig?
Neem contact op met de Azure Active Directory Domain Services-productteam voor [feedback delen of voor ondersteuning](active-directory-ds-contact-us.md).
