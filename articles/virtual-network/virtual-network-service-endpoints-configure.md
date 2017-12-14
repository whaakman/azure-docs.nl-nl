---
title: Service-eindpunten voor virtuele netwerken van Azure configureren | Microsoft Docs
description: Meer informatie over het in- en uitschakelen van service-eindpunten voor uw virtuele netwerk
services: virtual-network
documentationcenter: na
author: anithaa
manager: narayan
editor: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/15/2017
ms.author: anithaa
ms.custom: 
ms.openlocfilehash: c9c23462f80533a224c3c2ac3658b9630f1798f9
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="configure-virtual-network-service-endpoints"></a>Service-eindpunten voor virtuele netwerken configureren

Met service-eindpunten voor virtuele netwerken (VNet) kunt u Azure-serviceresources naar uw Azure Virtual Network beveiligen doordat de internettoegang tot deze resources wordt verwijderd. Service-eindpunten bieden een rechtstreekse verbinding van uw virtuele netwerk naar een Azure-service, zodat u het privé-adres van uw VNet kunt gebruiken voor toegang tot de Azure-services. Verkeer dat is bestemd voor Azure-services via service-eindpunten blijft altijd in het Microsoft Azure backbone-netwerk. Meer informatie over[Service-eindpunten voor virtuele netwerken](virtual-network-service-endpoints-overview.md)

Dit artikel bevat stappen voor het in- en uitschakelen van service-eindpunten. Als de eindpunten zijn ingeschakeld op een subnet naar een Azure-service, kunt u specifieke serviceresources naar een virtueel netwerk beveiligen.

Service-eindpunten kunnen worden geconfigureerd met [Azure Portal](#azure-portal), [Azure PowerShell](#azure-powershell), [Azure-opdrachtregelinterface](#azure-cli) of een Azure Resource Manager[-sjabloon](#resource-manager-template).

>[!NOTE]
Tijdens de previewperiode worden service-eindpunten voor vnets voor bepaalde regio's ondersteund. Zie de pagina [Azure Virtual Network-updates](https://azure.microsoft.com/updates/?product=virtual-network) voor een lijst met ondersteunde regio's.

## <a name="service-endpoint-configuration-overview"></a>Overzicht van de configuratie van service-eindpunten

- Service-eindpunten kunnen alleen worden geconfigureerd op VNets die zijn geïmplementeerd via het Azure Resource Manager-implementatiemodel.

- Service-eindpunten worden ingesteld op elk subnet van een VNet.

- Voor een subnet kunt u slechts één service-eindpunt naar een service configureren. U kunt meerdere service-eindpunten naar verschillende services (bijvoorbeeld Azure Storage, Azure SQL) configureren.

- U kunt de eindpunten in een nieuw of bestaand subnet inschakelen.

- De locatie voor een eindpunt wordt automatisch geconfigureerd. Service-eindpunten worden standaard geconfigureerd naar de regio van het VNet. Om voor Azure Storage regionale failover-scenario's te ondersteunen, worden eindpunten automatisch geconfigureerd naar [gekoppelde Azure-regio's](https://docs.microsoft.com/azure/best-practices-availability-paired-regions#what-are-paired-regions)

  >[!NOTE]
  Afhankelijk van de grootte van het VNet/subnet kan het inschakelen van het service-eindpunt enige tijd duren. Zorg ervoor dat er geen kritieke taken worden uitgevoerd als u service-eindpunten inschakelt. Service-eindpunten wijzigen routes op elke NIC in uw subnet. Geopende TCP-verbindingen worden mogelijk beëindigd. 

- De aanroep van een service-eindpunt retourneert 'geslaagd' nadat de verkeersstroom naar de service op alle NIC's in het subnet zijn gewijzigd in privé-IP-adressen van het VNet.

- Effectieve routes om de configuratie van eindpunten te valideren:

   Om te valideren of een service-eindpunt juist is geconfigureerd, geeft 'effectieve routes' op een NIC in het subnet een nieuwe standaardroute weer met nextHopType: VirtualNetworkServiceEndpoint per service en regio. Meer informatie over [het oplossen van problemen met effectieve routes](https://docs.microsoft.com/azure/virtual-network/virtual-network-routes-troubleshoot-portal#using-effective-routes-to-troubleshoot-vm-traffic-flow)

   >[!NOTE]
   Effectieve routes kunnen alleen worden weergegeven als u een of meer netwerkinterfaces (NIC's) hebt geconfigureerd en gekoppeld aan een actieve virtuele machine in het subnet.

## <a name="azure-portal"></a>Azure Portal

### <a name="setting-up-service-endpoint-on-a-subnet-during-vnet-create"></a>Service-eindpunten in een subnet instellen tijdens VNet Create

1. Open [Azure Portal](https://portal.azure.com/).
Meld u aan bij Azure met uw Azure-account. Als u nog geen Azure-account hebt, kunt u zich registreren voor een gratis proefversie. Het account moet beschikken over de benodigde [machtigingen](#provisioning) voor het maken van een virtueel netwerk en een service-eindpunt.
2. Klik op + Nieuw > Netwerken > Virtueel netwerk > +Toevoegen.
3. Voer op Virtueel netwerk maken de volgende waarden in en klik vervolgens op Maken:

Instelling | Waarde
------- | -----
Naam    | myVnet
Adresruimte | 10.0.0.0/16
Subnetnaam|mySubnet
Subnetadresbereik|10.0.0.0/24
Resourcegroep|Zorg dat het vakje Nieuwe maken geselecteerd is en voer een naam in.
Locatie|Elke ondersteunde regio, bijvoorbeeld Australië - oost
Abonnement|Selecteer uw abonnement.
__ServiceEndpoints__|Ingeschakeld
__Services__ | Selecteer een of alle beschikbare services. In de previewperiode zijn dit de ondersteunde services: __"Microsoft.Storage", "Microsoft.Sql"__.

Selecteer services voor eindpunten: ![Services voor service-eindpunt selecteren](media/virtual-network-service-endpoints-portal/vnet-create-flow-services.png)

4. Controleer of alle instellingen juist zijn en klik op Maken.

![Service-eindpunt instellen](media/virtual-network-service-endpoints-portal/create-vnet-flow.png)

5. Als u de beveiliging van Azure-serviceresources naar uw VNet wilt voltooien, klikt u op de documentatie over de service in [Volgende stappen](#Next%20Steps).


### <a name="validating-service-endpoint-configuration"></a>De configuratie van service-eindpunten valideren

U kunt controleren of de service-eindpunten zijn geconfigureerd door de volgende stappen uit te voeren:

- Klik in Resources op Virtuele netwerken. Zoek het VNet.
- Klik op de naam van het VNet en navigeer naar Service-eindpunten
- Geconfigureerde eindpunten worden als Geslaagd weergegeven. Automatisch geconfigureerde locaties kunnen ook worden weergegeven

![De configuratie van service-eindpunten bevestigen](media/virtual-network-service-endpoints-portal/vnet-validate-settings.png
)

### <a name="effective-routes-to-validate-endpoint-configuration"></a>Effectieve routes om de configuratie van eindpunten te valideren

Als u effectieve routes op een netwerkinterface (NIC) in het subnet wilt weergeven, klikt u op een NIC in dat subnet. Klik onder Ondersteuning en probleemoplossing op Effectieve routes. Als het eindpunt is geconfigureerd, ziet u een nieuwe standaardroute met adresvoorvoegsels van de service als de bestemming en nextHopType als VirtualNetworkServiceEndpoint.

![Effectieve routes voor service-eindpunten](media/virtual-network-service-endpoints-portal/effective-routes.png)

### <a name="setting-up-service-endpoints-for-existing-subnets-in-a-vnet"></a>Service-eindpunten instellen voor bestaande subnetten in een VNet

1. Klik in Resources op Virtuele netwerken zoek een bestaand VNet
2. Klik op de naam van het VNet en navigeer naar Service-eindpunten
3. Klik op Toevoegen. Selecteer Service. U kunt een eindpunt naar slechts één service tegelijk maken. 
4. Selecteer alle subnetten waarop u het eindpunt wilt toepassen. Klik op Toevoegen

![Configuratie van service-eindpunten voor subnetten](media/virtual-network-service-endpoints-portal/existing-subnet.png)

### <a name="deleting-service-endpoints"></a>Service-eindpunten verwijderen
1. Klik in Resources op Virtuele netwerken. Zoek een bestaand VNet door te filteren op de VNet-naam.
2. Klik op de naam van het VNet en navigeer naar Service-eindpunten
3. Klik op de servicenaam en klik met de rechtermuisknop op de vermelding van het service-eindpunt
4. Selecteer Verwijderen

![Service-eindpunt verwijderen](media/virtual-network-service-endpoints-portal/delete-endpoint.png)

## <a name="azure-powershell"></a>Azure Powershell

Vereisten voor de installatie:

- Installeer de meest recente versie van de PowerShell [AzureRm](https://www.powershellgallery.com/packages/AzureRM/)-module. Zie [Overzicht van Azure PowerShell](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json) als u nog geen ervaring hebt met Azure PowerShell.
- Als u een PowerShell-sessie wilt starten, gaat u naar **Start**, voert u **powershell** in en klikt u vervolgens op **PowerShell**.
- Meld u in PowerShell aan bij Azure door het commando `login-azurermaccount` in te voeren. Het account moet beschikken over de benodigde [machtigingen](#provisioning) voor het maken van een virtueel netwerk en een service-eindpunt.

### <a name="get-available-service-endpoints-for-azure-region"></a>Beschikbare service-eindpunten voor een Azure-regio ophalen

Gebruik de onderstaande opdracht om de lijst met ondersteunde services voor eindpunten op te halen voor een Azure-regio.
 ```powershell
Get-AzureRmVirtualNetworkAvailableEndpointService -location eastus
```

Uitvoer: 
Naam | Id | Type
-----|----|-------
Microsoft.Storage|/subscriptions/xxxx-xxx-xxx/providers/Microsoft.Network/virtualNetworkEndpointServices/Microsoft.Storage|Microsoft.Network/virtualNetworkEndpointServices
Microsoft.Sql|/subscriptions/xxxx-xxx-xxx/providers/Microsoft.Network/virtualNetworkEndpointServices/Microsoft.Sql|Microsoft.Network/virtualNetworkEndpointServices

### <a name="add-azure-storage-service-endpoint-to-a-subnet-mysubnet-while-creating-the-virtual-network-myvnet"></a>Azure Storage-service-eindpunt toevoegen aan een subnet *mySubnet* tijdens het maken van het virtuele netwerk *myVNet*

```powershell
$subnet = New-AzureRmVirtualNetworkSubnetConfig -Name "mySubnet" -AddressPrefix "10.0.1.0/24" -ServiceEndpoint “Microsoft.Storage”

New-AzureRmVirtualNetwork -Name "myVNet" -AddressPrefix "10.0.0.0/16" -Subnet $subnet -ResourceGroupName "myRG" -Location "WestUS"
```
U kunt meerdere services inschakelen met behulp van de door komma's gescheiden lijst met servicenamen.
Bijvoorbeeld: Microsoft.Storage, Microsoft.Sql

Verwachte uitvoer:
```
Subnets : [
            {
            "Name": "mySubnet",
             ...
            "ServiceEndpoints": [
              {
                   "ProvisioningState": "Succeeded",
                    "Service": "Microsoft.Storage",
                    "Locations": [
                        "westus",
                        "eastus"
                                 ]
               }
                                ],
            "ProvisioningState": "Succeeded"
            }
          ]
```

Als u de beveiliging van Azure-serviceresources naar uw VNet wilt voltooien, klikt u op de documentatie over de service in [Volgende stappen](#Next%20Steps).

### <a name="add-multiple-service-endpoints-to-an-existing-subnet"></a>Meerdere service-eindpunten aan een bestaand subnet toevoegen

```powershell
Get-AzureRmVirtualNetwork -ResourceGroupName "myRG" -Name "myVNet" | Set-AzureRmVirtualNetworkSubnetConfig -Name "mySubnet"  -AddressPrefix "10.0.1.0/24" -ServiceEndpoint "Microsoft.Storage", "Microsoft.Sql" | Set-AzureRmVirtualNetwork
```

Verwachte uitvoer: 
```
Subnets : [
            {
                "Name": "mySubnet",
                 ...
                "ServiceEndpoints": [
                {
                    "ProvisioningState": "Succeeded",
                    "Service": "Microsoft.Storage",
                    "Locations": [
                        "eastus",
                        "westus"
                                 ]
                },
                {
                    "ProvisioningState": "Succeeded",
                    "Service": "Microsoft.Sql",
                    "Locations": [
                        "eastus"
                                 ]
                }
                ],
               "ProvisioningState": "Succeeded"
            }
         ]
```

### <a name="view-service-endpoints-configured-on-a-subnet"></a>Service-eindpunten weergeven die zijn geconfigureerd op een subnet

```powershell
$subnet=Get-AzureRmVirtualNetwork -ResourceGroupName "myRG" -Name "myVNet" | Get-AzureRmVirtualNetworkSubnetConfig -Name "mySubnet"
$subnet.ServiceEndpoints
```
Uitvoer:
```
ProvisioningState Service           Locations
----------------- -------           ---------
Succeeded         Microsoft.Storage {eastus, westus}
Succeeded         Microsoft.Sql     {eastus}
```

### <a name="delete-service-endpoints-on-a-subnet"></a>Service-eindpunten op een subnet verwijderen
```powershell
Get-AzureRmVirtualNetwork -ResourceGroupName "myRG" -Name "myVNet" | Set-AzureRmVirtualNetworkSubnetConfig -Name "mySubnet"  -AddressPrefix "10.0.1.0/24" -ServiceEndpoint $null | Set-AzureRmVirtualNetwork
```

## <a name="azure-cli"></a>Azure CLI

Vereisten voor de installatie:
- Meld u aan bij uw Azure-abonnement met de opdracht [az login](/cli/azure/#login) en volg de instructies op het scherm. Zie [Aan de slag met Azure CLI 2.0](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest) voor meer informatie over aanmelden.
 - Het account moet beschikken over de benodigde [machtigingen](#provisioning) voor het maken van een virtueel netwerk en een service-eindpunt.

 Zie [Azure CLI Virtual Network commands (Opdrachten voor Azure CLI virtueel netwerk)](https://docs.microsoft.com/cli/azure/network/vnet?view=azure-cli-latest) voor een volledige lijst met opdrachten voor virtuele netwerken

### <a name="get-available-service-endpoints-for-azure-region"></a>Beschikbare service-eindpunten voor een Azure-regio ophalen

Gebruik de onderstaande opdracht om de lijst met ondersteunde services voor eindpunten op te halen voor een Azure-regio, bijvoorbeeld VS - oost.
```azure-cli
az network vnet list-endpoint-services -l eastus
```
Uitvoer:
```
    {
    "id": "/subscriptions/xxxx-xxxx-xxxx/providers/Microsoft.Network/virtualNetworkEndpointServices/Microsoft.Storage",
    "name": "Microsoft.Storage",
    "type": "Microsoft.Network/virtualNetworkEndpointServices"
     },
     {
     "id": "/subscriptions/xxxx-xxxx-xxxx/providers/Microsoft.Network/virtualNetworkEndpointServices/Microsoft.Sql",
     "name": "Microsoft.Sql",
     "type":   "Microsoft.Network/virtualNetworkEndpointServices"
     }
```

### <a name="add-azure-storage-service-endpoint-to-a-subnet-mysubnet-while-creating-the-virtual-network-myvnet"></a>Azure Storage-service-eindpunt toevoegen aan een subnet *mySubnet* tijdens het maken van het virtuele netwerk *myVNet*

```azure-cli
az network vnet create -g myRG -n myVNet --address-prefixes 10.0.0.0/16 -l eastUS

az network vnet subnet create -g myRG -n mySubnet --vnet-name myVNet --address-prefix 10.0.1.0/24 --service-endpoints Microsoft.Storage
```

Meerdere eindpunten toevoegen:--service-eindpunten Microsoft.Storage Microsoft.Sql

Uitvoer:
```
{
  "addressPrefix": "10.0.1.0/24",
  ...
  "name": "mySubnet",
  "networkSecurityGroup": null,
  "provisioningState": "Succeeded",
  "resourceGroup": "myRG",
  "resourceNavigationLinks": null,
  "routeTable": null,
  "serviceEndpoints": [
    {
      "locations": [
        "eastus",
        "westus"
      ],
      "provisioningState": "Succeeded",
      "service": "Microsoft.Storage"
    }
  ]
}
```

Als u de beveiliging van Azure-serviceresources naar uw VNet wilt voltooien, klikt u op de documentatie over de service in [Volgende stappen](#Next%20Steps).

### <a name="add-multiple-service-endpoints-to-an-existing-subnet"></a>Meerdere service-eindpunten aan een bestaand subnet toevoegen

```azure-cli
az network vnet subnet update -g myRG -n mySubnet2 --vnet-name myVNet --service-endpoints Microsoft.Storage Microsoft.Sql
```

Verwachte uitvoer:
```
{
  "addressPrefix": "10.0.2.0/24",
  ...
  "name": "mySubnet2",
  ...
  "serviceEndpoints": [
    {
      "locations": [
        "eastus",
        "westus"
      ],
      "provisioningState": "Succeeded",
      "service": "Microsoft.Storage"
    },
    {
      "locations": [
        "eastus"
      ],
      "provisioningState": "Succeeded",
      "service": "Microsoft.Sql"
    }
  ]
}
```

### <a name="view-service-endpoints-configured-on-a-subnet"></a>Service-eindpunten weergeven die zijn geconfigureerd op een subnet

```azure-cli
az network vnet subnet show -g myRG -n mySubnet --vnet-name myVNet
```

### <a name="delete-service-endpoints-on-a-subnet"></a>Service-eindpunten op een subnet verwijderen
```azure-cli
az network vnet subnet update -g myRG -n mySubnet --vnet-name myVNet --service-endpoints ""
```

Uitvoer: 
```
{
  "addressPrefix": "10.0.1.0/24",
  ...
  "name": "mySubnet",
  "networkSecurityGroup": null,
  "provisioningState": "Succeeded",
  "resourceGroup": "myRG",
  "resourceNavigationLinks": null,
  "routeTable": null,
  "serviceEndpoints": null
}
```

## <a name="resource-manager-template"></a>Resource Manager-sjabloon

### <a name="securing-azure-service-resources-to-vnets"></a>Azure-serviceresources naar VNets beveiligen

U kunt specifieke Azure-resources naar uw virtuele netwerk beveiligen via service-eindpunten.

Download het voorbeeld [Resource Manager-sjabloon](https://azure.microsoft.com/resources/templates/201-vnet-2subnets-service-endpoints-storage-integration) voor het beveiligen van een opslagaccount naar een subnet in een VNet.

De sjabloon maakt een VNet met 2 subnetten, een virtuele machine met een NIC in elk subnet. Hiermee schakelt u een eindpunt op een subnet in en beveiligt u een opslagaccount naar dat subnet.

U kunt de sjabloon downloaden en aanpassen aan uw scenario.

Bij de sjabloon worden instructies geleverd voor de implementatie van de sjabloon via Azure Portal, PowerShell of Azure CLI. Zorg ervoor dat u over de vereiste [machtigingen](#provisioning) beschikt om het eindpunt in te stellen en het account te beveiligen.

Voor het beveiligen van Azure-resources naar een subnet geldt:

- er moet een service-eindpunt worden geconfigureerd op dat subnet.
- de resource moet worden beveiligd naar het vnet door een regel voor het virtuele netwerk aan de resource toe te voegen.

### <a name="deleting-service-endpoints-with-resources-secured-to-the-subnet"></a>Service-eindpunten verwijderen met beveiligde resources naar het subnet
Als Azure-serviceresources naar het subnet zijn beveiligd en het service-eindpunt wordt verwijderd, hebt u geen toegang meer tot de resource vanuit het subnet.
U kunt de toegang tot beveiligde resources naar het subnet niet herstellen door het eindpunt opnieuw in te schakelen.

Als u de serviceresource naar dit subnet opnieuw wilt beveiligen, moet u:

- het eindpunt opnieuw inschakelen
- de oude vnet-regel op de resource verwijderen
- een nieuwe regel toevoegen voor de beveiliging van de resource naar het subnet

## <a name="provisioning"></a>Inrichten

Service-eindpunten kunnen afzonderlijk op virtuele netwerken worden geconfigureerd door een gebruiker met schrijftoegang tot het virtuele netwerk.

Als u Azure-serviceresources naar een VNet wilt beveiligen, moet u machtigingen hebben voor Microsoft.Network/JoinServicetoaSubnet voor de subnetten die worden toegevoegd. Deze machtiging is standaard opgenomen in de ingebouwde service-beheerdersrollen en kan worden gewijzigd door aangepaste rollen te maken.

Meer informatie over [ingebouwde rollen](https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles) en het toewijzen van specifieke machtigingen voor [aangepaste rollen](https://docs.microsoft.com/azure/active-directory/role-based-access-control-custom-roles).

VNets en Azure-serviceresources kunnen in hetzelfde of in verschillend abonnementen zitten. Als ze in verschillende abonnementen zitten, moeten de resources tijdens deze previewperiode onder dezelfde Active Directory-tenant (AD) zitten.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer instructies voor het beveiligen van resources naar VNets de onderstaande koppelingen:

[Azure Storage-accounts naar virtuele netwerken beveiligen](https://docs.microsoft.com/azure/storage/common/storage-network-security)

[Azure SQL naar virtuele netwerken beveiligen](https://docs.microsoft.com/azure/sql-database/sql-database-vnet-service-endpoint-rule-overview)
