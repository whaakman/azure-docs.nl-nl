---
title: Statische interne persoonlijke IP - Azure VM - klassiek
description: Inzicht krijgen in statische interne IP-adressen (Spanningsdips) en hoe deze te beheren
services: virtual-network
documentationcenter: na
author: genlin
manager: cshepard
editor: tysonn
ms.assetid: 93444c6f-af1b-41f8-a035-77f5c0302bf0
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/31/2018
ms.author: genli
ms.openlocfilehash: b83a6e2c81eac9993c481561e3cebbed681d2c4a
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58096041"
---
# <a name="how-to-set-a-static-internal-private-ip-address-using-powershell-classic"></a>Over het instellen van een statische interne persoonlijke IP-adres met behulp van PowerShell (klassiek)
In de meeste gevallen hoeft u niet een statische interne IP-adres voor uw virtuele machine opgeven. Virtuele machines in een virtueel netwerk ontvangt automatisch een intern IP-adres van een bereik dat u opgeeft. Maar in bepaalde gevallen een statisch IP-adres op te geven voor een bepaalde virtuele machine is logisch. Bijvoorbeeld, als uw virtuele machine wilt uitvoeren, DNS of wordt een domeincontroller. Een statische interne IP-adres blijft van toepassing op de virtuele machine, zelfs via een status stoppen/inrichting. 

> [!IMPORTANT]
> Azure heeft twee verschillende implementatiemodellen voor het maken van en werken met resources:  [Resource Manager en het klassieke model](../azure-resource-manager/resource-manager-deployment-model.md). Dit artikel gaat over het gebruik van het klassieke implementatiemodel. Microsoft raadt aan dat de meeste nieuwe implementaties gebruiken de [Resource Manager-implementatiemodel](virtual-networks-static-private-ip-arm-ps.md).
> 
> 
> ## <a name="install-the-azure-powershell-service-management-module"></a>De Azure PowerShell Service Management-module installeren

Voordat u de volgende opdrachten uitvoert, zorg ervoor dat de [Azure PowerShell Service Management-module](https://docs.microsoft.com/powershell/azure/servicemanagement/install-azure-ps?view=azuresmps-4.0.0
) op de computer is geïnstalleerd. Zie voor de versiegeschiedenis van Azure PowerShell Service Management-module, [Azure-module in PowerShell Gallery](https://www.powershellgallery.com/packages/Azure/5.3.0).

## <a name="how-to-verify-if-a-specific-ip-address-is-available"></a>Controleren of een specifiek IP-adres beschikbaar is
Om te controleren als het IP-adres *10.0.0.7* is beschikbaar in een vnet met de naam *TestVnet*, de volgende PowerShell-opdracht uit en controleer of de waarde voor *IsAvailable*.


    Test-AzureStaticVNetIP –VNetName TestVNet –IPAddress 10.0.0.7 

    IsAvailable          : True
    AvailableAddresses   : {}
    OperationDescription : Test-AzureStaticVNetIP
    OperationId          : fd3097e1-5f4b-9cac-8afa-bba1e3492609
    OperationStatus      : Succeeded

> [!NOTE]
> Als u wilt testen van de opdracht hierboven in een veilige omgeving Volg de richtlijnen in [een virtueel netwerk (klassiek) maken](virtual-networks-create-vnet-classic-pportal.md) te maken van een vnet met de naam *TestVnet* en zorg ervoor dat deze gebruikmaakt van de *10.0.0.0/8*  -adresruimte.
> 
> 

## <a name="how-to-specify-a-static-internal-ip-when-creating-a-vm"></a>Een statische interne IP-adres opgeven bij het maken van een virtuele machine
De onderstaande PowerShell-script maakt een nieuwe cloudservice met de naam *TestService*, klikt u vervolgens een afbeelding opgehaald uit Azure en vervolgens maakt u een virtuele machine met de naam *TestVM* in de nieuwe cloudservice met behulp van de opgehaalde, stelt u de Virtuele machine worden in een subnet met de naam *Subnet-1*, en stelt *10.0.0.7* als statische interne IP-adres voor de virtuele machine:

    New-AzureService -ServiceName TestService -Location "Central US"
    $image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}
    New-AzureVMConfig -Name TestVM -InstanceSize Small -ImageName $image.ImageName `
    | Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
    | Set-AzureSubnet –SubnetNames Subnet-1 `
    | Set-AzureStaticVNetIP -IPAddress 10.0.0.7 `
    | New-AzureVM -ServiceName "TestService" –VNetName TestVnet

## <a name="how-to-retrieve-static-internal-ip-information-for-a-vm"></a>Statische interne IP-informatie voor een virtuele machine ophalen
Voer de volgende PowerShell-opdracht als u wilt de statische interne IP-gegevens weergeven voor de virtuele machine met het bovenstaande script gemaakt, en houd rekening met de waarden voor *IpAddress*:

    Get-AzureVM -Name TestVM -ServiceName TestService

    DeploymentName              : TestService
    Name                        : TestVM
    Label                       : 
    VM                          : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.PersistentVM
    InstanceStatus              : Provisioning
    IpAddress                   : 10.0.0.7
    InstanceStateDetails        : Windows is preparing your computer for first use...
    PowerState                  : Started
    InstanceErrorCode           : 
    InstanceFaultDomain         : 0
    InstanceName                : TestVM
    InstanceUpgradeDomain       : 0
    InstanceSize                : Small
    HostName                    : rsR2-797
    AvailabilitySetName         : 
    DNSName                     : http://testservice000.cloudapp.net/
    Status                      : Provisioning
    GuestAgentStatus            : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.GuestAgentStatus
    ResourceExtensionStatusList : {Microsoft.Compute.BGInfo}
    PublicIPAddress             : 
    PublicIPName                : 
    NetworkInterfaces           : {}
    ServiceName                 : TestService
    OperationDescription        : Get-AzureVM
    OperationId                 : 34c1560a62f0901ab75cde4fed8e8bd1
    OperationStatus             : OK

## <a name="how-to-remove-a-static-internal-ip-from-a-vm"></a>Het verwijderen van een statische interne IP-adres van een virtuele machine
Als u wilt verwijderen van het statische interne IP-adres toegevoegd aan de virtuele machine in het bovenstaande script, moet u de volgende PowerShell-opdracht uitvoeren:

    Get-AzureVM -ServiceName TestService -Name TestVM `
    | Remove-AzureStaticVNetIP `
    | Update-AzureVM

## <a name="how-to-add-a-static-internal-ip-to-an-existing-vm"></a>Een statische interne IP-adres toevoegen aan een bestaande virtuele machine
Een statische interne IP-adres toevoegen aan de virtuele machine gemaakt met behulp van het bovenstaande script, moet u de volgende opdracht uitvoeren:

    Get-AzureVM -ServiceName TestService000 -Name TestVM `
    | Set-AzureStaticVNetIP -IPAddress 10.10.0.7 `
    | Update-AzureVM

## <a name="next-steps"></a>Volgende stappen
[Gereserveerde IP-adres](virtual-networks-reserved-public-ip.md)

[Instance-Level Public IP (ILPIP)](virtual-networks-instance-level-public-ip.md)

[Gereserveerde IP-adres REST-API 's](https://msdn.microsoft.com/library/azure/dn722420.aspx)

