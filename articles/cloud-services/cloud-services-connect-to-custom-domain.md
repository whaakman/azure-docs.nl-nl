---
title: Een Cloudservice verbinden met een aangepaste domeincontroller | Microsoft Docs
description: Leer hoe u uw web-/ werkrollen verbinden met een aangepaste AD-domein met behulp van PowerShell en AD-domein-extensie
services: cloud-services
documentationcenter: ''
author: jpconnock
manager: timlt
editor: ''
ms.assetid: 1e2d7c87-d254-4e7a-a832-67f84411ec95
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: jeconnoc
ms.openlocfilehash: 8bee2e2038ee39c777e1ca09994ad21872d2029a
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57530055"
---
# <a name="connecting-azure-cloud-services-roles-to-a-custom-ad-domain-controller-hosted-in-azure"></a>Azure Cloud Services-rollen verbinding te maken met een aangepaste AD-domeincontroller die wordt gehost in Azure
Er wordt eerst een Virtueelnetwerk (VNet) instellen in Azure. Er wordt een Active Directory-domeincontroller (die worden gehost op een Azure-Machine) toevoegen aan het VNet. Vervolgens wordt er bestaande cloudservicerollen toevoegen aan de vooraf gemaakte VNet en verbind deze met de domeincontroller.

Voordat we aan de slag, enkele van de zaken waarmee u rekening moet houden:

1. In deze zelfstudie maakt gebruik van PowerShell, dus zorg ervoor dat u hebt Azure PowerShell is geïnstalleerd en klaar voor gebruik. Zie voor hulp bij het instellen van Azure PowerShell, [hoe u Azure PowerShell installeren en configureren](/powershell/azure/overview).
2. De AD-domeincontroller en Web-/ Werkrol-instanties moeten zich in het VNet.

Volg deze stapsgewijze handleiding en als u problemen ondervindt, laat u ons een opmerking aan het einde van het artikel. Iemand anders wordt teruggaan naar u (Ja, doen we opmerkingen lezen).

Het netwerk waarnaar wordt verwezen door de cloudservice moet een **klassiek virtueel netwerk**.

## <a name="create-a-virtual-network"></a>Een virtueel netwerk maken
U kunt een Virtueelnetwerk maken in Azure met behulp van de Azure portal of PowerShell. Voor deze zelfstudie, wordt PowerShell gebruikt. Zie voor het maken van een virtueel netwerk met behulp van de Azure portal, [maken van een virtueel netwerk](../virtual-network/quick-create-portal.md). Het artikel bevat informatie over het maken van een virtueel netwerk (Resource Manager), maar moet u een virtueel netwerk (klassiek) voor cloudservices maken. Om dit te doen, in de portal, selecteert u **een resource maken**, type *virtueel netwerk* in de **zoeken** vak en druk vervolgens op **Enter**. In de lijst met zoekresultaten onder **Alles**, selecteer **virtueel netwerk**. Onder **een implementatiemodel selecteren**, selecteer **klassieke**en selecteer vervolgens **maken**. U kunt vervolgens de stappen in het artikel.

```powershell
#Create Virtual Network

$vnetStr =
@"<?xml version="1.0" encoding="utf-8"?>
<NetworkConfiguration xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
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
Nadat u het instellen van het Virtueelnetwerk hebt voltooid, moet u een AD-domeincontroller maken. Voor deze zelfstudie instellen we van een AD-domeincontroller op een Azure-Machine.

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

## <a name="promote-your-virtual-machine-to-a-domain-controller"></a>Uw virtuele Machine naar een domeincontroller promoveren
Voor het configureren van de virtuele Machine als een AD-domeincontroller, moet u zich aanmeldt bij de virtuele machine en configureer deze.

Als u wilt zich aanmeldt bij de virtuele machine, kunt u de RDP-bestand ophalen via PowerShell, gebruikt u de volgende opdrachten:

```powershell
# Get RDP file
Get-AzureRemoteDesktopFile -ServiceName $vmsvc1 -Name $vm1 -LocalPath <rdp-file-path>
```

Nadat u bent aangemeld bij de virtuele machine, instellen van uw virtuele Machine als een AD-domeincontroller door de stapsgewijze handleiding volgen op [over het instellen van uw klant AD-domeincontroller](https://social.technet.microsoft.com/wiki/contents/articles/12370.windows-server-2012-set-up-your-first-domain-controller-step-by-step.aspx).

## <a name="add-your-cloud-service-to-the-virtual-network"></a>Uw Cloudservice toevoegen aan het virtuele netwerk
Vervolgens moet u uw cloud service-implementatie toevoegen aan de nieuwe VNet. U doet dit door uw cloud service cscfg te wijzigen door de relevante secties toe te voegen aan uw cscfg met behulp van Visual Studio of de editor van uw keuze.

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

Vervolgens uw cloud services-project te bouwen en implementeren in Azure. Zie voor hulp bij de implementatie van uw cloud services-pakket naar Azure, [maken en implementeren van een Service in de Cloud](cloud-services-how-to-create-deploy-portal.md)

## <a name="connect-your-webworker-roles-to-the-domain"></a>Verbinding maken met uw web-/ werkrollen aan het domein
Zodra uw cloudserviceproject is geïmplementeerd op Azure, verbinding maken met uw rolinstanties aan de aangepaste AD-domein met de extensie van de AD-domein. Als u wilt de AD-domein-extensie toevoegen aan uw bestaande implementatie van cloud services en deelnemen aan het aangepaste domein, voert u de volgende opdrachten in PowerShell:

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

Uw cloudservices moeten worden toegevoegd aan uw aangepaste domein-controller. Als u meer informatie over de verschillende opties voor het configureren van de extensie van de AD-domein wilt, gebruikt u de PowerShell-help. Een aantal voorbeelden volgt:

```powershell
help Set-AzureServiceADDomainExtension
help New-AzureServiceADDomainExtensionConfig
```
