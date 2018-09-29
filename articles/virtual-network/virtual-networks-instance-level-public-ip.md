---
title: Adressen van Azure Instance-level openbaar IP-adres (klassiek) | Microsoft Docs
description: Instance-level public IP (ILPIP)-adressen en hoe u ze kunt beheren met behulp van PowerShell.
services: virtual-network
documentationcenter: na
author: genlin
manager: cshepard
editor: tysonn
ms.assetid: 07eef6ec-7dfe-4c4d-a2c2-be0abfb48ec5
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/03/2018
ms.author: genli
ms.openlocfilehash: 7d8325ce04a9fa7853fb622062022a6938375f96
ms.sourcegitcommit: 7c4fd6fe267f79e760dc9aa8b432caa03d34615d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/28/2018
ms.locfileid: "47430978"
---
# <a name="instance-level-public-ip-classic-overview"></a>Openbare IP (klassiek) overzicht's op exemplaarniveau
Een instantie level public IP (ILPIP) is een openbare IP-adres dat u rechtstreeks naar een exemplaar van de rol virtuele machine of Cloud Services, in plaats van met de cloudservice die uw rol of VM-exemplaar zich bevinden in kunt toewijzen. Een ILPIP ter niet vervanging van het virtuele IP (VIP) die is toegewezen aan uw cloudservice. Het is in plaats daarvan een extra IP-adres dat u kunt rechtstreeks verbinding maken met uw rol of VM-exemplaar.

> [!IMPORTANT]
> Azure heeft twee verschillende implementatiemodellen voor het maken van en werken met resources: [Resource Manager en het klassieke model](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Dit artikel gaat over het gebruik van het klassieke implementatiemodel. Microsoft raadt aan het maken van virtuele machines via Resource Manager. Zorg ervoor dat u begrijpt hoe [IP-adressen](virtual-network-ip-addresses-overview-classic.md) werk in Azure.

![Verschil tussen ILPIP en VIP](./media/virtual-networks-instance-level-public-ip/Figure1.png)

Zoals weergegeven in afbeelding 1, de cloudservice wordt geopend met behulp van een VIP-adres, terwijl de afzonderlijke virtuele machines normaal gesproken worden geopend met behulp van VIP:&lt;poortnummer&gt;. Door een ILPIP toewijzen aan een specifieke virtuele machine, is die virtuele machine toegankelijk via dit IP-adres.

Wanneer u een cloudservice in Azure maakt, worden bijbehorende DNS A-records automatisch gemaakt voor toegang tot de service via een volledig gekwalificeerde domeinnaam (FQDN), in plaats van de werkelijke VIP. Hetzelfde proces gebeurt voor een ILPIP, zodat toegang tot de rol of VM-exemplaar met FQDN-naam in plaats van de ILPIP. Bijvoorbeeld, als u een cloudservice met de naam *contosoadservice*, en het configureren van een Webrol die met de naam *contosoweb* met twee exemplaren en in cscfg `domainNameLabel` is ingesteld op  *WebPublicIP*, Azure registreert de volgende A-records voor de exemplaren:


* WebPublicIP.0.contosoadservice.cloudapp.net
* WebPublicIP.1.contosoadservice.cloudapp.net
* ...


> [!NOTE]
> U kunt slechts één ILPIP voor elk exemplaar van de virtuele machine of rol kunt toewijzen. U kunt maximaal 5 ILPIPs per abonnement gebruiken. ILPIPs worden niet ondersteund voor virtuele machines meerdere NIC's.
> 
> 

## <a name="why-would-i-request-an-ilpip"></a>Waarom zou ik een ILPIP vragen?
Als u kunnen verbinding maken met uw rol of VM-exemplaar met een IP-adres toegewezen om er rechtstreeks wilt, in plaats van de cloud service VIP:&lt;poortnummer&gt;, een ILPIP aanvragen voor uw virtuele machine of uw rolinstantie.

* **Actieve FTP** -door een ILPIP toewijzen aan een virtuele machine, kan ontvangen verkeer op een willekeurige poort. Eindpunten zijn niet vereist voor de virtuele machine om verkeer te ontvangen.  Zie [FTP-Protocol Overview] (https://en.wikipedia.org/wiki/File_Transfer_Protocol#Protocol_overview) voor meer informatie over het FTP-protocol.
* **Uitgaande IP** : uitgaand verkeer dat afkomstig is van de virtuele machine is toegewezen aan de ILPIP als de bron en de ILPIP unieke identificatie van de virtuele machine naar externe entiteiten.

> [!NOTE]
> In het verleden is een ILPIP-adres aangeduid als een openbaar IP-(PIP)-adres.
> 

## <a name="manage-an-ilpip-for-a-vm"></a>Een ILPIP voor een virtuele machine beheren
De volgende taken kunnen u maken, toewijzen en ILPIPs verwijderen van virtuele machines:

### <a name="how-to-request-an-ilpip-during-vm-creation-using-powershell"></a>Het aanvragen van een ILPIP VM wordt gemaakt met behulp van PowerShell
De volgende PowerShell-script maakt een cloudservice met de naam *FTPService*, haalt u een installatiekopie van Azure, maakt u een virtuele machine met de naam *FTPInstance* met behulp van de opgehaalde, stelt u de virtuele machine te gebruiken een ILPIP en wordt toegevoegd de virtuele machine naar de nieuwe service:

```powershell
New-AzureService -ServiceName FTPService -Location "Central US"

$image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}

#Set "current" storage account for the subscription. It will be used as the location of new VM disk

Set-AzureSubscription -SubscriptionName <SubName> -CurrentStorageAccountName <StorageAccountName>

#Create a new VM configuration object

New-AzureVMConfig -Name FTPInstance -InstanceSize Small -ImageName $image.ImageName `
| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
| Set-AzurePublicIP -PublicIPName ftpip | New-AzureVM -ServiceName FTPService -Location "Central US"

```
Als u een ander opslagaccount opgeven als de locatie van de nieuwe VM-schijf wilt, kunt u **MediaLocation** parameter:

```powershell
    New-AzureVMConfig -Name FTPInstance -InstanceSize Small -ImageName $image.ImageName `
     -MediaLocation https://management.core.windows.net/<SubscriptionID>/services/storageservices/<StorageAccountName> `
    | Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
    | Set-AzurePublicIP -PublicIPName ftpip | New-AzureVM -ServiceName FTPService -Location "Central US"
```

### <a name="how-to-retrieve-ilpip-information-for-a-vm"></a>ILPIP-informatie voor een virtuele machine ophalen
Voer de volgende PowerShell-opdracht als u de ILPIP-informatie voor de virtuele machine met het vorige script gemaakt, en houd rekening met de waarden voor *PublicIPAddress* en *PublicIPName*:

```powershell
Get-AzureVM -Name FTPInstance -ServiceName FTPService
```

Verwachte uitvoer:
 
    DeploymentName              : FTPService
    Name                        : FTPInstance
    Label                       : 
    VM                          : Microsoft.WindowsAzure.Commands.ServiceManagement.Model.PersistentVM
    InstanceStatus              : ReadyRole
    IpAddress                   : 100.74.118.91
    InstanceStateDetails        : 
    PowerState                  : Started
    InstanceErrorCode           : 
    InstanceFaultDomain         : 0
    InstanceName                : FTPInstance
    InstanceUpgradeDomain       : 0
    InstanceSize                : Small
    HostName                    : FTPInstance
    AvailabilitySetName         : 
    DNSName                     : http://ftpservice888.cloudapp.net/
    Status                      : ReadyRole
    GuestAgentStatus            :   Microsoft.WindowsAzure.Commands.ServiceManagement.Model.GuestAgentStatus
    ResourceExtensionStatusList : {Microsoft.Compute.BGInfo}
    PublicIPAddress             : 104.43.142.188
    PublicIPName                : ftpip
    NetworkInterfaces           : {}
    ServiceName                 : FTPService
    OperationDescription        : Get-AzureVM
    OperationId                 : 568d88d2be7c98f4bbb875e4d823718e
    OperationStatus             : OK

### <a name="how-to-remove-an-ilpip-from-a-vm"></a>Het verwijderen van een ILPIP van een virtuele machine
Als u wilt verwijderen van de ILPIP toegevoegd aan de virtuele machine in het vorige script, voer de volgende PowerShell-opdracht:

```powershell
Get-AzureVM -ServiceName FTPService -Name FTPInstance | Remove-AzurePublicIP | Update-AzureVM
```

### <a name="how-to-add-an-ilpip-to-an-existing-vm"></a>Een ILPIP toevoegen aan een bestaande virtuele machine
Een ILPIP toevoegen aan de virtuele machine gemaakt met behulp van het vorige script, moet u de volgende opdracht uitvoeren:

```powershell
Get-AzureVM -ServiceName FTPService -Name FTPInstance | Set-AzurePublicIP -PublicIPName ftpip2 | Update-AzureVM
```

## <a name="manage-an-ilpip-for-a-cloud-services-role-instance"></a>Een ILPIP voor een exemplaar van Cloud Services beheren

Een ILPIP toevoegen aan een exemplaar van Cloud Services, voert u de volgende stappen uit:

1. Download het cscfg-bestand voor de cloudservice via de stappen in de [over het configureren van Cloud Services](../cloud-services/cloud-services-how-to-configure-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json#reconfigure-your-cscfg) artikel.
2. Het cscfg-bestand bijwerken door toe te voegen de `InstanceAddress` element. Het volgende voorbeeld wordt een met de naam ILPIP *MyPublicIP* naar een rolexemplaar met de naam *WebRole1*: 

    ```xml
    <?xml version="1.0" encoding="utf-8"?>
    <ServiceConfiguration serviceName="ILPIPSample" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="4" osVersion="*" schemaVersion="2014-01.2.3">
      <Role name="WebRole1">
        <Instances count="1" />
          <ConfigurationSettings>
        <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true" />
          </ConfigurationSettings>
      </Role>
      <NetworkConfiguration>
        <AddressAssignments>
          <InstanceAddress roleName="WebRole1">
        <PublicIPs>
          <PublicIP name="MyPublicIP" domainNameLabel="WebPublicIP" />
            </PublicIPs>
          </InstanceAddress>
        </AddressAssignments>
      </NetworkConfiguration>
    </ServiceConfiguration>
    ```
3. Uploaden van het cscfg-bestand voor de cloudservice via de stappen in de [over het configureren van Cloud Services](../cloud-services/cloud-services-how-to-configure-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json#reconfigure-your-cscfg) artikel.

### <a name="how-to-retrieve-ilpip-information-for-a-cloud-service"></a>Het ophalen van gegevens voor een Cloudservice ILPIP
Als u de informatie ILPIP per rolexemplaar, de volgende PowerShell-opdracht uit en bekijk de waarden voor *PublicIPAddress*, *PublicIPName*, *PublicIPDomainNameLabel* en *PublicIPFqdns*:

```powershell
Add-AzureAccount

$roles = Get-AzureRole -ServiceName <Cloud Service Name> -Slot Production -RoleName WebRole1 -InstanceDetails

$roles[0].PublicIPAddress
$roles[1].PublicIPAddress
```

U kunt ook `nslookup` de record het subdomein query:

```batch
nslookup WebPublicIP.0.<Cloud Service Name>.cloudapp.net
``` 

## <a name="next-steps"></a>Volgende stappen
* Begrijpen hoe [IP-adressering](virtual-network-ip-addresses-overview-classic.md) werkt in het klassieke implementatiemodel.
* Meer informatie over [gereserveerd IP-adressen](virtual-networks-reserved-public-ip.md).
