---
title: Azure gereserveerde IP-adressen beheren (klassiek) - PowerShell | Microsoft Docs
description: Overzicht van gereserveerde IP-adressen (klassiek) en hoe deze te beheren met behulp van PowerShell.
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
ms.assetid: 34652a55-3ab8-4c2d-8fb2-43684033b191
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/10/2016
ms.author: jdial
ms.openlocfilehash: 5e9c83cebec96c6bc8afd53b0c637d7af899746f
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="reserved-ip-addresses-classic"></a>Gereserveerde IP-adressen (klassiek)

> [!div class="op_single_selector"]
> * [Azure Portal](virtual-network-deploy-static-pip-arm-portal.md)
> * [PowerShell](virtual-network-deploy-static-pip-arm-ps.md)
> * [Azure-CLI](virtual-network-deploy-static-pip-arm-cli.md)
> * [Sjabloon](virtual-network-deploy-static-pip-arm-template.md)
> * [PowerShell (klassiek)](virtual-networks-reserved-public-ip.md)

IP-adressen in Azure worden onderverdeeld in twee categorieën: dynamische en gereserveerd. Openbare IP-adressen die worden beheerd door Azure worden standaard dynamisch. Dat betekent dat het IP-adres wordt gebruikt voor een bepaalde cloud-service (VIP) of voor toegang tot een virtuele machine of rechtstreeks rolinstantie (ILPIP) van tijd tot tijd veranderen kan wanneer resources worden afgesloten of gestopt (toewijzing ongedaan gemaakt).

Als u wilt voorkomen dat IP-adressen wijzigen, kunt u een IP-adres reserveren. Gereserveerd IP-adressen kan alleen worden gebruikt als een VIP, waarbij u ervoor zorgt dat het IP-adres voor de cloudservice gelijk blijft, zelfs wanneer resources worden afgesloten of gestopt (toewijzing ongedaan gemaakt). Bovendien kunt u als een VIP-adres op een gereserveerde IP-adres gebruikt bestaande dynamische IP-adressen converteren.

> [!IMPORTANT]
> Azure heeft twee verschillende implementatiemodellen voor het maken van en werken met resources: [Resource Manager en het klassieke model](../azure-resource-manager/resource-manager-deployment-model.md). Dit artikel gaat over het gebruik van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken. Meer informatie over het reserveren van een statische openbare IP-adres met de [Resource Manager-implementatiemodel](virtual-network-ip-addresses-overview-arm.md).

Lees voor meer informatie over IP-adressen in Azure, de [IP-adressen](virtual-network-ip-addresses-overview-classic.md) artikel.

## <a name="when-do-i-need-a-reserved-ip"></a>Wanneer moet ik een gereserveerde IP-adres?
* **U wilt zorgen dat het IP-adres is gereserveerd in uw abonnement**. Als u reserveren van een IP-adres dat niet uit uw abonnement in elk geval wordt uitgebracht wilt, moet u een gereserveerde openbare IP-adres gebruiken.  
* **U wilt dat uw IP-om te blijven met de cloudservice zelfs meerdere gestopt of toewijzing opgeheven status (VM's)**. Als u wilt dat uw service toegankelijk via een IP-adres verandert dat niet, zelfs wanneer virtuele machines in de cloudservice worden afgesloten of gestopt (toewijzing ongedaan gemaakt).
* **U wilt zorgen dat uitgaande verkeer vanuit Azure gebruikmaakt van een voorspelbare IP-adres**. Mogelijk hebt u de firewall van uw lokale geconfigureerd, zodat alleen verkeer van bepaalde IP-adressen. Door een IP-adres reserveren, weet van het IP-bronadres en hoeft niet bijwerken van de firewallregels vanwege een wijziging in de IP.

## <a name="faq"></a>Veelgestelde vragen
1. Kan ik een gereserveerde IP-adres gebruiken voor alle Azure-services? <br>
    Nee. Gereserveerd IP-adressen kan alleen worden gebruikt voor virtuele machines en cloud service-exemplaar rollen weergegeven via een VIP-adres.
2. Het aantal gereserveerde IP's kan ik hebben? <br>
    Zie voor meer informatie de [Azure beperkt](../azure-subscription-service-limits.md#networking-limits) artikel.
3. Is er kosten met zich mee voor gereserveerde IP's? <br>
    Soms. Zie voor prijsinformatie, de [gereserveerde IP-adres prijsinformatie](http://go.microsoft.com/fwlink/?LinkID=398482) pagina.
4. Hoe ik een IP-adres reserveren? <br>
    PowerShell, kunt u de [Management REST API van Azure](https://msdn.microsoft.com/library/azure/dn722420.aspx), of de [Azure-portal](https://portal.azure.com) te reserveren van een IP-adres in een Azure-regio. Een gereserveerd IP-adres is gekoppeld aan uw abonnement.
5. Kan ik een gereserveerde IP-adres gebruiken met VNets op basis van een affiniteitsgroep <br>
    Nee. Gereserveerd IP-adressen worden alleen ondersteund in regionaal vnet's. Gereserveerd IP-adressen worden niet ondersteund voor VNets die zijn gekoppeld aan affiniteitsgroepen. Zie voor meer informatie over het koppelen van een VNet met een regio of affiniteitsgroep de [over regionaal vnet's en Affiniteitsgroepen](virtual-networks-migrate-to-regional-vnet.md) artikel.

## <a name="manage-reserved-vips"></a>Gereserveerde VIP's beheren

Zorg ervoor dat u hebt geïnstalleerd en geconfigureerd PowerShell via de stappen in de [installeren en configureren van PowerShell](/powershell/azure/overview) artikel. 

Voordat u gereserveerde IP's gebruiken kunt, moet u deze toevoegen aan uw abonnement. Maken van een gereserveerd IP-adres van de groep met openbare IP-adressen beschikbaar zijn in de *VS-midden* locatie, voer de volgende opdracht:

```powershell
New-AzureReservedIP –ReservedIPName MyReservedIP –Location "Central US"
```

U ziet, echter, u niet opgeven wat IP wordt gereserveerd. Voer de volgende PowerShell-opdracht om weer te geven welke IP-adressen zijn gereserveerd in uw abonnement, en Let op de waarden voor *ReservedIPName* en *adres*:

```powershell
Get-AzureReservedIP
```

Verwachte uitvoer:

    ReservedIPName       : MyReservedIP
    Address              : 23.101.114.211
    Id                   : d73be9dd-db12-4b5e-98c8-bc62e7c42041
    Label                :
    Location             : Central US
    State                : Created
    InUse                : False
    ServiceName          :
    DeploymentName       :
    OperationDescription : Get-AzureReservedIP
    OperationId          : 55e4f245-82e4-9c66-9bd8-273e815ce30a
    OperationStatus      : Succeeded

>[!NOTE]
>Wanneer u een gereserveerd IP-adres met PowerShell maakt, kunt u een resourcegroep voor het maken van de gereserveerde IP-adres in niet opgeven. Azure plaatsen in een resourcegroep met de naam *standaard netwerken* automatisch. Als u de gereserveerde IP-met maakt de [Azure-portal](http://portal.azure.com), kunt u een resourcegroep die u kiest. Als u de gereserveerde IP-adres dan in een resourcegroep maken *standaard netwerken* echter, wanneer u verwijst naar de gereserveerde IP-adres met opdrachten zoals `Get-AzureReservedIP` en `Remove-AzureReservedIP`, moet u de naam verwijzen naar *Resourcegroepnaam gereserveerd-ip-naam van de groep*.  Bijvoorbeeld, als u een gereserveerde IP-adres met de naam *myReservedIP* in een resourcegroep met de naam *myResourceGroup*, moet u de naam van het gereserveerde IP-adres als verwijzen naar *groep myResourceGroup myReservedIP*.   

Nadat een IP-adres is gereserveerd, blijft deze gekoppeld aan uw abonnement totdat u het verwijdert. Voer de volgende PowerShell-opdracht voor het verwijderen van een gereserveerd IP-adres:

```powershell
Remove-AzureReservedIP -ReservedIPName "MyReservedIP"
```

## <a name="reserve-the-ip-address-of-an-existing-cloud-service"></a>Het IP-adres van een bestaande cloudservice reserveren
U kunt het IP-adres van een bestaande cloudservice reserveren door toe te voegen de `-ServiceName` parameter. Het IP-adres van een cloudservice reserveren *TestService* in de *VS-midden* locatie, voer de volgende PowerShell-opdracht:

```powershell
New-AzureReservedIP –ReservedIPName MyReservedIP –Location "Central US" -ServiceName TestService
```

## <a name="associate-a-reserved-ip-to-a-new-cloud-service"></a>Koppelen van een gereserveerd IP-adres op een nieuwe cloudservice
Het volgende script maakt een nieuw gereserveerd IP-adres en vervolgens koppelt u deze naar een nieuwe cloudservice met de naam *TestService*.

```powershell
New-AzureReservedIP –ReservedIPName MyReservedIP –Location "Central US"

$image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}

New-AzureVMConfig -Name TestVM -InstanceSize Small -ImageName $image.ImageName `
| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
| New-AzureVM -ServiceName TestService -ReservedIPName MyReservedIP -Location "Central US"
```

> [!NOTE]
> Wanneer u een gereserveerde IP-adres voor gebruik met een cloudservice maakt, u nog steeds verwijzen naar de virtuele machine met behulp van *VIP:&lt;poortnummer >* voor binnenkomende communicatie. Een IP-adres reserveren betekent niet dat u rechtstreeks verbinding kan maken met de virtuele machine. De gereserveerde IP-adres is toegewezen aan de cloudservice die de virtuele machine is geïmplementeerd op. Als u een virtuele machine door IP rechtstreeks verbinding wilt maken, hebt u een openbaar IP op exemplaarniveau configureren. Een openbaar IP op exemplaarniveau is een openbaar IP-adres (een ILPIP genoemd) die rechtstreeks aan uw virtuele machine is toegewezen. Deze kan niet worden gereserveerd. Lees voor meer informatie de [instantieniveau openbare IP (ILPIP)](virtual-networks-instance-level-public-ip.md) artikel.
> 

## <a name="remove-a-reserved-ip-from-a-running-deployment"></a>Een gereserveerd IP-adres van een actieve implementatie verwijderen
Voer de volgende PowerShell-opdracht voor het verwijderen van een gereserveerd IP-adres toegevoegd aan een nieuwe cloudservice:

```powershell
Remove-AzureReservedIPAssociation -ReservedIPName MyReservedIP -ServiceName TestService
```

> [!NOTE]
> Een gereserveerd IP-adres worden van een actieve implementatie niet verwijderd de reservering van uw abonnement. Deze vrij eenvoudig, het IP-adres moet worden gebruikt door een andere resource in uw abonnement.
> 

## <a name="associate-a-reserved-ip-to-a-running-deployment"></a>Koppelen van een gereserveerd IP-adres aan een actieve implementatie
De volgende opdrachten maakt een cloudservice met de naam *TestService2* met een nieuwe virtuele machine met de naam *TestVM2*. De bestaande gereserveerde IP-adres met de naam *MyReservedIP* wordt vervolgens naar de cloudservice is gekoppeld.

```powershell
$image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}

New-AzureVMConfig -Name TestVM2 -InstanceSize Small -ImageName $image.ImageName `
| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
| New-AzureVM -ServiceName TestService2 -Location "Central US"

Set-AzureReservedIPAssociation -ReservedIPName MyReservedIP -ServiceName TestService2
```

## <a name="associate-a-reserved-ip-to-a-cloud-service-by-using-a-service-configuration-file"></a>Koppelen van een gereserveerd IP-adres op een cloudservice met behulp van een service-configuratiebestand
U kunt ook een gereserveerde IP-adres op een cloudservice met behulp van een service-configuratiebestand (CSCFG) koppelen. De xml van het volgende voorbeeld toont hoe u configureert een cloudservice voor het gebruik van een gereserveerd VIP-adres met de naam *MyReservedIP*:

    <?xml version="1.0" encoding="utf-8"?>
    <ServiceConfiguration serviceName="ReservedIPSample" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="4" osVersion="*" schemaVersion="2014-01.2.3">
      <Role name="WebRole1">
        <Instances count="1" />
        <ConfigurationSettings>
          <Setting name="Microsoft.WindowsAzure.Plugins.Diagnostics.ConnectionString" value="UseDevelopmentStorage=true" />
        </ConfigurationSettings>
      </Role>
      <NetworkConfiguration>
        <AddressAssignments>
          <ReservedIPs>
           <ReservedIP name="MyReservedIP"/>
          </ReservedIPs>
        </AddressAssignments>
      </NetworkConfiguration>
    </ServiceConfiguration>

## <a name="next-steps"></a>Volgende stappen
* Begrijpen hoe [IP-adressering](virtual-network-ip-addresses-overview-classic.md) werkt in het klassieke implementatiemodel.
* Meer informatie over [privé IP-adressen gereserveerd](virtual-networks-reserved-private-ip.md).
* Meer informatie over [Instance Level Public IP (ILPIP) adressen](virtual-networks-instance-level-public-ip.md).

