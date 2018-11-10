---
title: Beheren Azure gereserveerde IP-adressen (klassiek) | Microsoft Docs
description: Informatie over gereserveerde IP-adressen (klassiek) en hoe u ze kunt beheren met behulp van Azure PowerShell en Azure CLI.
services: virtual-network
documentationcenter: na
author: genlin
manager: cshepard
editor: tysonn
ms.assetid: 34652a55-3ab8-4c2d-8fb2-43684033b191
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/12/2018
ms.author: genli
ms.openlocfilehash: bd2b28a7f8d0a765e10ffa58b5a72b4bd5bc47b0
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51228178"
---
# <a name="reserved-ip-addresses-classic-deployment"></a>Gereserveerde IP-adressen (klassieke implementatie)

 IP-adressen in Azure kunnen worden onderverdeeld in twee categorieën: dynamische en gereserveerd. Openbare IP-adressen die worden beheerd door Azure worden standaard dynamisch. Dat betekent dat het IP-adres gebruikt voor een bepaalde cloud-service (VIP) of voor toegang tot een virtuele machine of rechtstreeks rolinstantie (ILPIP) van tijd tot tijd wijzigen kan wanneer resources worden afgesloten of gestopt (toewijzing opgeheven).

Als u wilt voorkomen dat IP-adressen wijzigen, kunt u een IP-adres reserveren. Gereserveerde IP-adressen kan alleen als een VIP-adres, ervoor te zorgen dat het IP-adres voor de cloudservice hetzelfde blijft, zelfs als het resources worden afgesloten of gestopt (toewijzing opgeheven) worden gebruikt. Bovendien kunt u als een VIP-adres aan een gereserveerde IP-adres gebruikt bestaande dynamische IP-adressen converteren.

> [!IMPORTANT]
> Azure heeft twee verschillende implementatiemodellen voor het maken van en werken met resources: [Resource Manager en het klassieke model](../azure-resource-manager/resource-manager-deployment-model.md). Dit artikel gaat over het gebruik van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken. Meer informatie over het reserveren van een statische openbare IP-adres met de [Resource Manager-implementatiemodel](virtual-network-ip-addresses-overview-arm.md).

Lees voor meer informatie over IP-adressen in Azure, de [IP-adressen](virtual-network-ip-addresses-overview-classic.md) artikel.

## <a name="when-do-i-need-a-reserved-ip"></a>Wanneer moet ik een gereserveerde IP-adres?
* **U wilt ervoor zorgen dat het IP-adres wordt gereserveerd in uw abonnement**. Als u reserveren van een IP-adres dat niet is vrijgegeven uit uw abonnement onder alle omstandigheden wilt, moet u een gereserveerde openbare IP-adres gebruiken.  
* **U wilt dat uw IP-adres om te blijven met uw service in de cloud, zelfs meerdere gestopt of de toewijzing ongedaan gemaakt status (VM's)**. Als u wilt dat uw service worden geopend met behulp van een IP-adres wijzigen dat niet, zelfs wanneer virtuele machines in de cloudservice worden afgesloten of stoppen (toewijzing opgeheven).
* **U wilt ervoor zorgen dat het uitgaande verkeer van Azure gebruikmaakt van een voorspelbare IP-adres**. Mogelijk hebt u uw on-premises firewall geconfigureerd, zodat alleen verkeer van specifieke IP-adressen. Door het reserveren van een IP-adres, kunt u kent het bron-IP-adres en niet nodig hebt om bij te werken van uw firewall-regels vanwege een wijziging in de IP.

## <a name="faqs"></a>Veelgestelde vragen
- Kan ik een gereserveerde IP-adres gebruiken voor alle Azure-services?
    Nee. Gereserveerde IP-adressen kan alleen worden gebruikt voor virtuele machines en cloud service-exemplaar rollen die toegankelijk is via een VIP-adres.
- Het aantal gereserveerde IP-adressen kan ik hebben?
    Zie voor meer informatie, de [Azure-limieten](../azure-subscription-service-limits.md#networking-limits) artikel.
- Is er een toeslag voor gereserveerde IP-adressen?
    Soms. Zie voor details over de prijzen, de [gereserveerde IP-adres prijsinformatie over](https://go.microsoft.com/fwlink/?LinkID=398482) pagina.
- Hoe ik een IP-adres reserveren?
    Kunt u PowerShell, de [Azure Management REST API](https://msdn.microsoft.com/library/azure/dn722420.aspx), of de [Azure-portal](https://portal.azure.com) te reserveren IP-adres in een Azure-regio. Een gereserveerde IP-adres is gekoppeld aan uw abonnement.
- Kan ik een gereserveerde IP-adres met VNets op basis van een affiniteitsgroep gebruiken?
    Nee. Gereserveerde IP-adressen worden alleen ondersteund in regionale VNets. Gereserveerde IP-adressen worden niet ondersteund voor VNets die zijn gekoppeld aan affiniteitsgroepen. Zie voor meer informatie over het koppelen van een VNet met een regio of affiniteitsgroep de [over regionale vnet's en Affiniteitsgroepen](virtual-networks-migrate-to-regional-vnet.md) artikel.

## <a name="manage-reserved-vips"></a>Gereserveerde VIP's beheren

### <a name="using-azure-powershell-classic"></a>Met behulp van Azure PowerShell (klassiek)

Voordat u gereserveerde IP-adressen gebruiken kunt, moet u deze toevoegen aan uw abonnement. Maken van een gereserveerde IP-adres uit de groep van openbare IP-adressen beschikbaar in de *VS-midden* locatie als volgt te werk:

> [!NOTE]
> Voor het klassieke implementatiemodel, moet u de Service Management-versie van Azure PowerShell installeren. Zie [Installing the Azure PowerShell Service Management module](https://docs.microsoft.com/powershell/azure/servicemanagement/install-azure-ps?view=azuresmps-4.0.0) (De Azure PowerShell Service Management-module installeren) voor meer informatie. 

  ```powershell
    New-AzureReservedIP –ReservedIPName MyReservedIP –Location "Central US"
  ```
U ziet, maar u niet opgeven welke IP wordt gereserveerd. Als u wilt weergeven welke IP-adressen zijn gereserveerd in uw abonnement, de volgende PowerShell-opdracht uitvoeren en u ziet de waarden voor *ReservedIPName* en *adres*:

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
>Wanneer u een gereserveerd IP-adres met PowerShell maken, kunt u een resourcegroep voor het maken van het gereserveerde IP-adres in niet opgeven. Azure plaatsen deze in een resourcegroep met de naam *standaard-netwerk* automatisch. Als u de gereserveerde IP-via maakt de [Azure-portal](http://portal.azure.com), kunt u een resourcegroep die u kiest. Als u het gereserveerde IP-adres in een resourcegroep gemaakt met andere dan maken *standaard-netwerk* echter wanneer u verwijst naar de gereserveerde IP-adres met opdrachten zoals `Get-AzureReservedIP` en `Remove-AzureReservedIP`, u moet verwijzen naar de naam van de  *Resource-group-name gereserveerd-ip-naam van de groep*.  Bijvoorbeeld, als u een gereserveerde IP-adres met de naam *myReservedIP* in een resourcegroep met de naam *myResourceGroup*, u moet verwijzen naar de naam van het gereserveerde IP-adres als *groep myResourceGroup myReservedIP*.   


Nadat een IP-adres is gereserveerd, blijft deze gekoppeld aan uw abonnement totdat u deze verwijdert. Een gereserveerde IP-adres als volgt verwijderen:

```powershell
Remove-AzureReservedIP -ReservedIPName "MyReservedIP"
```

### <a name="using-azure-cli-classic"></a>Met behulp van Azure CLI (klassiek)
Maken van een gereserveerde IP-adres uit de groep van openbare IP-adressen beschikbaar in de *VS-midden* locatie als met behulp van Azure classic CLI volgt:

> [!NOTE]
> Voor klassieke implementatie, moet u Azure klassieke CLI. Zie voor meer informatie over het installeren van Azure CLI voor klassieke [de klassieke Azure-CLI installeren](https://docs.microsoft.com/cli/azure/install-classic-cli?view=azure-cli-latest)
  
 Opdracht:
 
```azurecli
azure network reserved-ip create <name> <location>
```
Voorbeeld:
 ```azurecli
 azure network reserved-ip create MyReservedIP centralus
 ```

U kunt bekijken welke IP-adressen zijn gereserveerd in uw abonnement met behulp van Azure CLI als volgt: 

Opdracht:
```azurecli
azure network reserved-ip list
```
Nadat een IP-adres is gereserveerd, blijft deze gekoppeld aan uw abonnement totdat u deze verwijdert. Een gereserveerde IP-adres als volgt verwijderen:

Opdracht:

 ```azurecli
 azure network reserved-ip delete <name>
 ```
  Voorbeeld:  
 ```azurecli
 azure network reserved-ip delete MyReservedIP
 ```
## <a name="reserve-the-ip-address-of-an-existing-cloud-service"></a>De IP-adres van een bestaande cloudservice reserveren
U kunt het IP-adres van een bestaande cloudservice reserveren door toe te voegen de `-ServiceName` parameter. De IP-adres van een cloudservice reserveren *TestService* in de *VS-midden* locatie als volgt te werk:

- Met behulp van Azure PowerShell (klassiek):

  ```powershell
  New-AzureReservedIP –ReservedIPName MyReservedIP –Location "Central US" -ServiceName TestService
  ```

- Met behulp van Azure CLI (klassiek):
  
    Opdracht:

    ```azurecli
     azure network reserved-ip create <name> <location> -r <service-name> -d <deployment-name>
    ```
    Voorbeeld:

    ```azurecli
      azure network reserved-ip create MyReservedIP centralus -r TestService -d asmtest8942
    ```

## <a name="associate-a-reserved-ip-to-a-new-cloud-service"></a>Een gereserveerde IP-adres aan een nieuwe cloudservice koppelen
Het volgende script maakt u een nieuw gereserveerd IP-adres en koppelt het aan een nieuwe cloudservice met de naam *TestService*.

### <a name="using-azure-powershell-classic"></a>Met behulp van Azure PowerShell (klassiek)
```powershell
New-AzureReservedIP –ReservedIPName MyReservedIP –Location "Central US"

$image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}

New-AzureVMConfig -Name TestVM -InstanceSize Small -ImageName $image.ImageName `
| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
| New-AzureVM -ServiceName TestService -ReservedIPName MyReservedIP -Location "Central US"
```
> [!NOTE]
> Wanneer u een gereserveerde IP-adres te gebruiken met een cloudservice maakt, u nog steeds verwijzen naar de virtuele machine met behulp van *VIP:&lt;poortnummer >* voor binnenkomende communicatie. Een IP-adres reserveren betekent niet dat u direct verbinding kunt maken met de virtuele machine. Het gereserveerde IP-adres is toegewezen aan de cloudservice die de virtuele machine is geïmplementeerd op. Als u rechtstreeks verbinding maken met een virtuele machine door IP wilt, hebt u een openbaar IP op exemplaarniveau configureren. Een openbaar IP op exemplaarniveau is een openbaar IP-adres (een ILPIP genoemd) die rechtstreeks aan uw virtuele machine is toegewezen. Het kan niet worden gereserveerd. Lees voor meer informatie de [Instance-level Public IP (ILPIP)](virtual-networks-instance-level-public-ip.md) artikel.
> 

## <a name="remove-a-reserved-ip-from-a-running-deployment"></a>Een gereserveerde IP-adres uit een actieve implementatie verwijderen

Een gereserveerde IP-adres toegevoegd aan een nieuwe cloudservice als volgt verwijderen: 
### <a name="using-azure-powershell-classic"></a>Met behulp van Azure PowerShell (klassiek)

```powershell
Remove-AzureReservedIPAssociation -ReservedIPName MyReservedIP -ServiceName TestService
```

### <a name="using-azure-cli-classic"></a>Met behulp van Azure CLI (klassiek)
Opdracht:

```azurecli
azure network reserved-ip disassociate <name> <service-name> <deployment-name>
```

Voorbeeld:

```azurecli
azure network reserved-ip disassociate MyReservedIP TestService asmtest8942
```

> [!NOTE]
> Verwijderen van een gereserveerde IP-adres uit een actieve implementatie, wordt de reservering niet verwijderd uit uw abonnement. Het IP-adres moet worden gebruikt door een andere resource in uw abonnement te kunnen.
> 

Als u wilt een gereserveerde IP-adres volledig verwijderen van een abonnement, moet u de volgende opdracht uitvoeren:

Opdracht:

```azurecli
azure network reserved-ip delete <name>
```
Voorbeeld:

```azurecli
azure network reserved-ip delete MyReservedIP
```

## <a name="associate-a-reserved-ip-to-a-running-deployment"></a>Een gereserveerde IP-adres aan een actieve implementatie koppelen

### <a name="using-azure-powershell-classic"></a>Met behulp van Azure PowerShell (klassiek)

De volgende opdrachten maakt u een cloudservice met de naam *TestService2* met een nieuwe virtuele machine met de naam *TestVM2*. De bestaande gereserveerd IP met de naam *MyReservedIP* wordt vervolgens die is gekoppeld aan de cloudservice.

```powershell
$image = Get-AzureVMImage|?{$_.ImageName -like "*RightImage-Windows-2012R2-x64*"}

New-AzureVMConfig -Name TestVM2 -InstanceSize Small -ImageName $image.ImageName `
| Add-AzureProvisioningConfig -Windows -AdminUsername adminuser -Password MyP@ssw0rd!! `
| New-AzureVM -ServiceName TestService2 -Location "Central US"

Set-AzureReservedIPAssociation -ReservedIPName MyReservedIP -ServiceName TestService2
```

### <a name="using-azure-cli-classic"></a>Met behulp van Azure CLI (klassiek)
U kunt een nieuw gereserveerd IP-adres koppelen aan uw actieve cloud service-implementatie met behulp van Azure CLI als volgt:

Opdracht:
```azurecli
azure network reserved-ip associate <name> <service-name> <deployment-name>
```
Voorbeeld:
```azurecli
azure network reserved-ip associate MyReservedIP TestService asmtest8942
```
## <a name="associate-a-reserved-ip-to-a-cloud-service-by-using-a-service-configuration-file"></a>Een gereserveerde IP-adres aan een cloudservice koppelen met behulp van een service-configuratiebestand
U kunt ook een gereserveerde IP-adres aan een cloudservice koppelen met behulp van een serviceconfiguratiebestand (CSCFG). De xml van het volgende voorbeeld laat zien hoe het configureren van een cloudservice voor het gebruik van een gereserveerde VIP-adres met de naam *MyReservedIP*:
```
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
```
## <a name="next-steps"></a>Volgende stappen
* Begrijpen hoe [IP-adressering](virtual-network-ip-addresses-overview-classic.md) werkt in het klassieke implementatiemodel.
* Meer informatie over [privé IP-adressen gereserveerd](virtual-networks-reserved-private-ip.md).
* Meer informatie over [Instance Level Public IP (ILPIP)-adressen](virtual-networks-instance-level-public-ip.md).

