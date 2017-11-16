---
title: Verbinding maken met een Cloudservice met een aangepaste domeincontroller | Microsoft Docs
description: Informatie over het verbinden van uw web/worker rollen aan een aangepast AD-domein met behulp van PowerShell en uitbreiding van AD-domein
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 1e2d7c87-d254-4e7a-a832-67f84411ec95
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: adegeo
ms.openlocfilehash: e2aadf6a103e92a4fbb11223a449280a36dea6b4
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/16/2017
---
# <a name="connecting-azure-cloud-services-roles-to-a-custom-ad-domain-controller-hosted-in-azure"></a>Verbinding maken met Azure Cloud Services-functies van een aangepaste gehost in Azure AD Domain-Controller
Er wordt eerst een virtueel netwerk (VNet) instellen in Azure. Er wordt een Active Directory-domeincontroller (gehost op een virtuele Machine van Azure) toegevoegd aan het VNet. Er wordt vervolgens bestaande cloud service rollen toevoegen aan de vooraf gemaakte VNet en verbind ze met de domeincontroller.

Voordat we aan de slag, enkele dingen rekening moet houden:

1. Deze zelfstudie maakt gebruik van PowerShell, dus zorg ervoor dat u hebt Azure PowerShell is geïnstalleerd en klaar voor gebruik. Zie voor hulp bij het instellen van Azure PowerShell, [installeren en configureren van Azure PowerShell](/powershell/azure/overview).
2. Uw AD-domeincontroller en de functie Web/Worker-exemplaren moeten in het VNet.

Volg deze stapsgewijze handleiding en als u problemen ondervindt uitvoert, laat ons een opmerking aan het einde van het artikel. Iemand anders wordt u zo snel (Ja, we opmerkingen lezen).

Het netwerk waarnaar wordt verwezen door de cloudservice moet een **klassiek virtueel netwerk**.

## <a name="create-a-virtual-network"></a>Een virtueel netwerk maken
U kunt een virtueel netwerk maken in Azure met behulp van de Azure-portal of PowerShell. Voor deze zelfstudie gebruiken we PowerShell. Zie het maken van een virtueel netwerk met de Azure-portal [virtueel netwerk maken](../virtual-network/virtual-networks-create-vnet-arm-pportal.md).

```powershell
#Create Virtual Network

$vnetStr =
@"<?xml version="1.0" encoding="utf-8"?>
<NetworkConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
    <VirtualNetworkConfiguration>
    <VirtualNetworkSites>
        <VirtualNetworkSite name="[your-vnet-name]" Location="West US">
        <AddressSpace>
            <AddressPrefix>[your-address-prefix]</AddressPrefix>
        </AddressSpace>
        <Subnets>
            <Subnet name="[your-subnet-name]">
            <AddressPrefix>[your-subnet-range]</AddressPrefix>
            </Subnet>
        </Subnets>
        </VirtualNetworkSite>
    </VirtualNetworkSites>
    </VirtualNetworkConfiguration>
</NetworkConfiguration>
"@;

$vnetConfigPath = "<path-to-vnet-config>"
Set-AzureVNetConfig -ConfigurationPath $vnetConfigPath
```

## <a name="create-a-virtual-machine"></a>Een virtuele machine maken
Als u klaar bent met het instellen van het virtuele netwerk, moet u een AD-domeincontroller maken. Voor deze zelfstudie stellen we van een AD-domeincontroller op een virtuele Machine van Azure.

Om dit te doen, maakt u een virtuele machine via PowerShell met de volgende opdrachten:

```powershell
# Initialize variables
# VNet and subnet must be classic virtual network resources, not Azure Resource Manager resources.

$vnetname = '<your-vnet-name>'
$subnetname = '<your-subnet-name>'
$vmsvc1 = '<your-hosted-service>'
$vm1 = '<your-vm-name>'
$username = '<your-username>'
$password = '<your-password>'
$affgrp = '<your- affgrp>'

# Create a VM and add it to the Virtual Network

New-AzureQuickVM -Windows -ServiceName $vmsvc1 -Name $vm1 -ImageName $imgname -AdminUsername $username -Password $password -AffinityGroup $affgrp -SubnetNames $subnetname -VNetName $vnetname
```

## <a name="promote-your-virtual-machine-to-a-domain-controller"></a>De virtuele Machine naar een domeincontroller promoveren
Voor het configureren van de virtuele Machine als een AD-domeincontroller, moet u aanmelden bij de virtuele machine en configureert.

Meld u bij naar de virtuele machine, kunt u het RDP-bestand downloaden via PowerShell, gebruikt u de volgende opdrachten:

```powershell
# Get RDP file
Get-AzureRemoteDesktopFile -ServiceName $vmsvc1 -Name $vm1 -LocalPath <rdp-file-path>
```

Nadat u bent aangemeld bij de virtuele machine, instellen van uw virtuele Machine als AD-domeincontroller door de stapsgewijze handleiding volgen op [het instellen van uw klant AD-domeincontroller](http://social.technet.microsoft.com/wiki/contents/articles/12370.windows-server-2012-set-up-your-first-domain-controller-step-by-step.aspx).

## <a name="add-your-cloud-service-to-the-virtual-network"></a>Uw Cloudservice toevoegen aan het virtuele netwerk
Vervolgens moet u uw cloud service-implementatie toevoegen aan de nieuwe VNet. U doet dit door uw cscfg cloud-service te wijzigen door de relevante secties toe te voegen aan uw cscfg met Visual Studio of de editor van uw keuze.

```xml
<ServiceConfiguration serviceName="[hosted-service-name]" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="[os-family]" osVersion="*">
    <Role name="[role-name]">
    <Instances count="[number-of-instances]" />
    </Role>
    <NetworkConfiguration>

    <!--optional-->
    <Dns>
        <DnsServers><DnsServer name="[dns-server-name]" IPAddress="[ip-address]" /></DnsServers>
    </Dns>
    <!--optional-->

    <!--VNet settings
        VNet and subnet must be classic virtual network resources, not Azure Resource Manager resources.-->
    <VirtualNetworkSite name="[virtual-network-name]" />
    <AddressAssignments>
        <InstanceAddress roleName="[role-name]">
        <Subnets>
            <Subnet name="[subnet-name]" />
        </Subnets>
        </InstanceAddress>
    </AddressAssignments>
    <!--VNet settings-->

    </NetworkConfiguration>
</ServiceConfiguration>
```

Naast uw cloud services-project te bouwen en deze implementeren in Azure. Als u hulp bij het implementeren van uw cloud services-pakket naar Azure, Zie [maken en implementeren van een Cloud-Service](cloud-services-how-to-create-deploy-portal.md)

## <a name="connect-your-webworker-roles-to-the-domain"></a>Uw web-/ werkrollen verbindt met het domein
Zodra uw cloudserviceproject is geïmplementeerd in Azure, moet u uw rolinstanties verbinding met het aangepaste AD-domein met de extensie van de AD-domein. Voor de uitbreiding van AD-domein toevoegen aan uw bestaande implementatie van de cloud-services en deelnemen aan het aangepaste domein, voert u de volgende opdrachten uit in PowerShell:

```powershell
# Initialize domain variables

$domain = '<your-domain-name>'
$dmuser = '$domain\<your-username>'
$dmpswd = '<your-domain-password>'
$dmspwd = ConvertTo-SecureString $dmpswd -AsPlainText -Force
$dmcred = New-Object System.Management.Automation.PSCredential ($dmuser, $dmspwd)

# Add AD Domain Extension to the cloud service roles

Set-AzureServiceADDomainExtension -Service <your-cloud-service-hosted-service-name> -Role <your-role-name> -Slot <staging-or-production> -DomainName $domain -Credential $dmcred -JoinOption 35
```

En dat is alles.

Uw cloudservices moeten worden toegevoegd aan uw aangepaste domeincontroller. Als u meer informatie over de verschillende opties voor het configureren van de uitbreiding van AD-domein wilt, gebruikt u de PowerShell-help. Een aantal voorbeelden volgt:

```powershell
help Set-AzureServiceADDomainExtension
help New-AzureServiceADDomainExtensionConfig
```
