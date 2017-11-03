---
title: Adressen van Azure instantieniveau openbare IP-adres (klassiek) | Microsoft Docs
description: Begrijpen exemplaar niveau openbare IP (ILPIP adressen) en hoe deze te beheren met behulp van PowerShell.
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
ms.assetid: 07eef6ec-7dfe-4c4d-a2c2-be0abfb48ec5
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/10/2016
ms.author: jdial
ms.openlocfilehash: 773043f2841ec7539b0d49357dec6bcb9f4f78a1
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="instance-level-public-ip-classic-overview"></a>Exemplaar niveau openbare IP (klassiek)-overzicht
Een instantie niveau openbare IP (ILPIP) is een openbaar IP-adres die u rechtstreeks naar een exemplaar van de rol virtuele machine of Cloud Services, in plaats van met de cloudservice die uw exemplaar van de virtuele machine of rol zich bevinden in kunt toewijzen. Een ILPIP ter niet vervanging van het virtuele IP (VIP) dat is toegewezen aan de cloudservice. Het is in plaats daarvan een extra IP-adres waarmee u kunt rechtstreeks verbinding maken met uw exemplaar van de virtuele machine of de rol.

> [!IMPORTANT]
> Azure heeft twee verschillende implementatiemodellen voor het maken van en werken met resources: [Resource Manager en het klassieke model](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Dit artikel gaat over het gebruik van het klassieke implementatiemodel. Microsoft raadt u aan voor het maken van virtuele machines via Resource Manager. Zorg dat u begrijpt hoe [IP-adressen](virtual-network-ip-addresses-overview-classic.md) werk in Azure.

![Verschil tussen ILPIP en VIP](./media/virtual-networks-instance-level-public-ip/Figure1.png)

Zoals in afbeelding 1, de cloudservice wordt geopend met een VIP, terwijl de afzonderlijke virtuele machines normaal gesproken worden geopend met behulp van VIP:&lt;poortnummer&gt;. Door een ILPIP toewijzen aan een specifieke virtuele machine, zijn die VM toegankelijk via dat IP-adres.

Wanneer u een cloudservice in Azure maakt, worden overeenkomende DNS A-records automatisch gemaakt voor toegang tot de service via een volledig gekwalificeerde domeinnaam (FQDN), in plaats van het werkelijke VIP. Hetzelfde proces gebeurt voor een ILPIP, toegang tot de VM of rol-exemplaar met FQDN-naam in plaats van de ILPIP toe te staan. Bijvoorbeeld, als u een cloudservice met de naam maakt *contosoadservice*, en configureert u een Webrol met de naam *contosoweb* met twee exemplaren Azure registreert de volgende A-records voor de exemplaren:

* contosoweb\_IN_0.contosoadservice.cloudapp.net
* contosoweb\_IN_1.contosoadservice.cloudapp.net 

> [!NOTE]
> U kunt slechts één ILPIP voor elk exemplaar van de virtuele machine of rol toewijzen. U kunt maximaal 5 ILPIPs per abonnement gebruiken. ILPIPs worden niet ondersteund voor virtuele machines van meerdere NIC's.
> 
> 

## <a name="why-would-i-request-an-ilpip"></a>Waarom zou ik een ILPIP aanvragen?
Als u wilt verbinding maken met uw exemplaar van de virtuele machine of rol door een IP-adres dat is toegewezen aan deze in plaats van de cloud service VIP:&lt;poortnummer&gt;, een ILPIP aanvragen voor uw virtuele machine of uw rolexemplaar.

* **Actieve FTP** -een ILPIP toewijst aan een VM, het verkeer op een willekeurige poort kan ontvangen. Eindpunten zijn niet vereist voor de virtuele machine verkeer ontvangt.  (Https://en.wikipedia.org/wiki/File_Transfer_Protocol#Protocol_overview) [FTP-Protocol Overview] Zie voor meer informatie over het FTP-protocol.
* **Uitgaande IP** - uitgaand verkeer dat afkomstig is van de virtuele machine is toegewezen aan de ILPIP als de bron en de ILPIP een unieke identificatie van de virtuele machine naar externe entiteiten.

> [!NOTE]
> In het verleden is een adres ILPIP aangeduid als een openbaar IP-(PIP)-adres.
> 

## <a name="manage-an-ilpip-for-a-vm"></a>Een ILPIP voor een virtuele machine beheren
De volgende taken kunnen u maakt, toewijst en ILPIPs verwijderen van virtuele machines:

### <a name="how-to-request-an-ilpip-during-vm-creation-using-powershell"></a>Het aanvragen van een ILPIP tijdens het maken van de virtuele machine met behulp van PowerShell
De volgende PowerShell-script maakt een cloudservice met de naam *FTPService*, haalt u een installatiekopie van Azure, maakt u een virtuele machine met de naam *FTPInstance* met behulp van de installatiekopie van het opgehaalde stelt u de VM voor gebruik van een ILPIP en worden toegevoegd de virtuele machine naar de nieuwe service:

```powershell
New-AzureService -ServiceName FTPService -Location "Central US"

$image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"} `
New-AzureVMConfig -Name FTPInstance -InstanceSize Small -ImageName $image.ImageName `
| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
| Set-AzurePublicIP -PublicIPName ftpip | New-AzureVM -ServiceName FTPService -Location "Central US"
```

### <a name="how-to-retrieve-ilpip-information-for-a-vm"></a>Hoe ILPIP-informatie voor een virtuele machine ophalen
Als u de ILPIP-informatie voor de virtuele machine met het vorige script gemaakt, voer de volgende PowerShell-opdracht en houd rekening met de waarden voor *PublicIPAddress* en *PublicIPName*:

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

### <a name="how-to-remove-an-ilpip-from-a-vm"></a>Een ILPIP van een virtuele machine verwijderen
Voer de volgende PowerShell-opdracht voor het verwijderen van de ILPIP toegevoegd aan de virtuele machine in het vorige script:

```powershell
Get-AzureVM -ServiceName FTPService -Name FTPInstance | Remove-AzurePublicIP | Update-AzureVM
```

### <a name="how-to-add-an-ilpip-to-an-existing-vm"></a>Een ILPIP toevoegen aan een bestaande virtuele machine
Als u wilt een ILPIP toevoegen aan de virtuele machine gemaakt met behulp van het vorige script, voer de volgende opdracht:

```powershell
Get-AzureVM -ServiceName FTPService -Name FTPInstance | Set-AzurePublicIP -PublicIPName ftpip2 | Update-AzureVM
```

## <a name="manage-an-ilpip-for-a-cloud-services-role-instance"></a>Een ILPIP voor een exemplaar van Cloud Services-functie beheren

Als u wilt een ILPIP toevoegen aan een exemplaar van Cloud Services-rol, moet u de volgende stappen uitvoeren:

1. Het cscfg-bestand voor de cloudservice downloaden via de stappen in de [Cloud-Services configureren hoe](../cloud-services/cloud-services-how-to-configure-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json#reconfigure-your-cscfg) artikel.
2. Het cscfg-bestand bijwerken door het toevoegen van de `InstanceAddress` element. Het volgende voorbeeld wordt een ILPIP met de naam toegevoegd *MyPublicIP* naar een rolexemplaar met de naam *WebRole1*: 

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
          <PublicIP name="MyPublicIP" domainNameLabel="MyPublicIP" />
            </PublicIPs>
          </InstanceAddress>
        </AddressAssignments>
      </NetworkConfiguration>
    </ServiceConfiguration>
    ```
3. Upload het cscfg-bestand voor de cloudservice via de stappen in de [Cloud-Services configureren hoe](../cloud-services/cloud-services-how-to-configure-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json#reconfigure-your-cscfg) artikel.

## <a name="next-steps"></a>Volgende stappen
* Begrijpen hoe [IP-adressering](virtual-network-ip-addresses-overview-classic.md) werkt in het klassieke implementatiemodel.
* Meer informatie over [gereserveerd IP-adressen](virtual-networks-reserved-public-ip.md).
