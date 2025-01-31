---
title: Privé-IP-adressen configureren voor virtuele machines (klassiek) - Azure PowerShell | Microsoft Docs
description: Informatie over het configureren van privé IP-adressen voor virtuele machines (klassiek) met behulp van PowerShell.
services: virtual-network
documentationcenter: na
author: genlin
manager: cshepard
editor: tysonn
tags: azure-service-management
ms.assetid: 60c7b489-46ae-48af-a453-2b429a474afd
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2016
ms.author: genli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a1be2966cf568fc5101fb0d887f0d55a8a1876d0
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65952498"
---
# <a name="configure-private-ip-addresses-for-a-virtual-machine-classic-using-powershell"></a>Configureer persoonlijke IP-adressen voor een virtuele machine (klassiek) met behulp van PowerShell

[!INCLUDE [virtual-networks-static-private-ip-selectors-classic-include](../../includes/virtual-networks-static-private-ip-selectors-classic-include.md)]

[!INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Dit artikel is van toepassing op het klassieke implementatiemodel. U kunt ook [beheren van een statisch privé IP-adres in het Resource Manager-implementatiemodel](virtual-networks-static-private-ip-arm-ps.md).

[!INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

Het voorbeeld onderstaande PowerShell-opdrachten kunt verwachten van een eenvoudige omgeving al gemaakt. Als u wilt dat de opdrachten uitvoeren zoals ze worden weergegeven in dit document, eerst samenstellen van de testomgeving die wordt beschreven [een VNet maken](virtual-networks-create-vnet-classic-netcfg-ps.md).

## <a name="how-to-verify-if-a-specific-ip-address-is-available"></a>Controleren of een specifiek IP-adres beschikbaar is
Om te controleren als het IP-adres *192.168.1.101* is beschikbaar in een VNet met de naam *TestVNet*, de volgende PowerShell-opdracht uit en controleer of de waarde voor *IsAvailable*:

    Test-AzureStaticVNetIP –VNetName TestVNet –IPAddress 192.168.1.101 

Verwachte uitvoer:

    IsAvailable          : True
    AvailableAddresses   : {}
    OperationDescription : Test-AzureStaticVNetIP
    OperationId          : fd3097e1-5f4b-9cac-8afa-bba1e3492609
    OperationStatus      : Succeeded

## <a name="how-to-specify-a-static-private-ip-address-when-creating-a-vm"></a>Een statisch privé IP-adres opgeven bij het maken van een virtuele machine
De onderstaande PowerShell-script maakt een nieuwe cloudservice met de naam *TestService*, haalt u vervolgens een installatiekopie van Azure, maakt u een virtuele machine met de naam *DNS01* in de nieuwe service in de cloud de virtuele machine met behulp van de opgehaalde, ingesteld op worden in een subnet met de naam *FrontEnd*, en stelt *192.168.1.7* als een statisch privé IP-adres voor de virtuele machine:

    New-AzureService -ServiceName TestService -Location "Central US"
    $image = Get-AzureVMImage | where {$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}
    New-AzureVMConfig -Name DNS01 -InstanceSize Small -ImageName $image.ImageName |
      Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! |
      Set-AzureSubnet –SubnetNames FrontEnd |
      Set-AzureStaticVNetIP -IPAddress 192.168.1.7 |
      New-AzureVM -ServiceName TestService –VNetName TestVNet

Verwachte uitvoer:

    WARNING: No deployment found in service: 'TestService'.
    OperationDescription OperationId                          OperationStatus
    -------------------- -----------                          ---------------
    New-AzureService     fcf705f1-d902-011c-95c7-b690735e7412 Succeeded      
    New-AzureVM          3b99a86d-84f8-04e5-888e-b6fc3c73c4b9 Succeeded  

## <a name="how-to-retrieve-static-private-ip-address-information-for-a-vm"></a>Het statische privé IP-adresgegevens voor een virtuele machine ophalen
Voer de volgende PowerShell-opdracht om de weergave met het statische privé IP-adresgegevens voor de virtuele machine met het bovenstaande script gemaakt, en houd rekening met de waarden voor *IpAddress*:

    Get-AzureVM -Name DNS01 -ServiceName TestService

Verwachte uitvoer:

    DeploymentName              : TestService
    Name                        : DNS01
    Label                       : 
    VM                          : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.PersistentVM
    InstanceStatus              : Provisioning
    IpAddress                   : 192.168.1.7
    InstanceStateDetails        : Windows is preparing your computer for first use...
    PowerState                  : Started
    InstanceErrorCode           : 
    InstanceFaultDomain         : 0
    InstanceName                : DNS01
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

## <a name="how-to-remove-a-static-private-ip-address-from-a-vm"></a>Het verwijderen van een statisch privé IP-adres van een virtuele machine
Toegevoegd aan de virtuele machine in het bovenstaande script de volgende PowerShell-opdracht uitvoeren om te verwijderen van het statische privé IP-adres:

    Get-AzureVM -ServiceName TestService -Name DNS01 |
      Remove-AzureStaticVNetIP |
      Update-AzureVM

Verwachte uitvoer:

    OperationDescription OperationId                          OperationStatus
    -------------------- -----------                          ---------------
    Update-AzureVM       052fa6f6-1483-0ede-a7bf-14f91f805483 Succeeded

## <a name="how-to-add-a-static-private-ip-address-to-an-existing-vm"></a>Een statisch privé IP-adres toevoegen aan een bestaande virtuele machine
Een statisch privé IP-adres toevoegen aan de virtuele machine gemaakt met behulp van het bovenstaande script, moet u de volgende opdracht uitvoeren:

    Get-AzureVM -ServiceName TestService -Name DNS01 |
      Set-AzureStaticVNetIP -IPAddress 192.168.1.7 |
      Update-AzureVM

Verwachte uitvoer:

    OperationDescription OperationId                          OperationStatus
    -------------------- -----------                          ---------------
    Update-AzureVM       77d8cae2-87e6-0ead-9738-7c7dae9810cb Succeeded 

## <a name="set-ip-addresses-within-the-operating-system"></a>IP-adressen binnen het besturingssysteem instellen

Het verdient aanbeveling dat u geen statisch het privé IP-adres toegewezen aan de virtuele machine van Azure binnen het besturingssysteem van een virtuele machine, toewijst tenzij die nodig zijn. Als u het particuliere IP-adres binnen het besturingssysteem handmatig instelt, zorg ervoor dat deze hetzelfde adres als de privé IP-adres toegewezen aan de Azure-VM of verliest u connectiviteit met de virtuele machine. U moet het openbare IP-adres toegewezen aan een virtuele machine van Azure binnen het besturingssysteem van de virtuele machine nooit handmatig toewijzen.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [gereserveerde openbare IP-adres](virtual-networks-reserved-public-ip.md) adressen.
* Meer informatie over [instantieniveau openbare IP (ILPIP)](virtual-networks-instance-level-public-ip.md) adressen.
* Raadpleeg de [gereserveerd IP-REST-API's](https://msdn.microsoft.com/library/azure/dn722420.aspx).

