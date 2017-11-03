---
title: Een Azure-netwerk (klassiek) - configuratiebestand netwerk configureren | Microsoft Docs
description: Informatie over het maken en wijzigen van virtuele netwerken (klassiek) te exporteren en importeren van een configuratiebestand netwerk wijzigen.
services: virtual-network
documentationcenter: 
author: jimdial
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: c29b9059-22b0-444e-bbfe-3e35f83cde2f
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/23/2017
ms.author: jdial
ms.custom: 
ms.openlocfilehash: f1e3ae26b6525f2235a6b0d53546b334dc027b94
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="configure-a-virtual-network-classic-using-a-network-configuration-file"></a>Een virtueel netwerk (klassiek) met een configuratiebestand netwerk configureren
> [!IMPORTANT]
> Azure heeft twee verschillende implementatiemodellen voor het maken en werken met resources: [Resource Manager en classic](../resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Dit artikel gaat over het gebruik van het klassieke implementatiemodel. Microsoft raadt aan dat de meeste nieuwe implementaties het Resource Manager-implementatiemodel gebruiken.

U kunt maken en configureren van een virtueel netwerk (klassiek) met een netwerk-configuratiebestand met de Azure-opdrachtregelinterface (CLI) 1.0 of Azure PowerShell. U kunt maken of wijzigen van een virtueel netwerk via het Azure Resource Manager-implementatiemodel met een netwerk configuratiebestand. U niet de Azure portal maken of wijzigen van een virtueel netwerk (klassiek), met een configuratiebestand netwerk, maar u kunt de Azure portal een virtueel netwerk (klassiek) maken zonder gebruik van een configuratiebestand netwerk gebruiken.

Maken en configureren van een virtueel netwerk (klassiek) met een netwerk-configuratiebestand moet exporteren en importeren van het bestand wijzigen.

## <a name="export"></a>Een netwerk-configuratiebestand exporteren

U kunt PowerShell of Azure CLI gebruiken voor het exporteren van een netwerk-configuratiebestand. PowerShell exporteert een XML-bestand, terwijl de Azure CLI een json-bestand exporteert.

### <a name="powershell"></a>PowerShell
 
1. [Azure PowerShell installeren en aanmelden bij Azure](/powershell/azure/install-azure-ps?toc=%2fazure%2fvirtual-network%2ftoc.json).
2. Wijzig de directory (en zorg ervoor dat deze bestaat) en de bestandsnaam in de volgende opdracht naar wens, voer de opdracht voor het exporteren van het configuratiebestand van het netwerk:

    ```powershell
    Get-AzureVNetConfig -ExportToFile c:\azure\networkconfig.xml
    ```

### <a name="azure-cli-10"></a>Azure CLI 1.0

1. [Installeer de Azure CLI 1.0](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json). De resterende stappen vanaf een opdrachtprompt Azure CLI 1.0.
2. Meld u aan bij Azure door te voeren de `azure login` opdracht.
3. Zorg ervoor dat u zich in de asm-modus door te voeren de `azure config mode asm` opdracht.
4. Wijzig de directory (en zorg ervoor dat deze bestaat) en de bestandsnaam in de volgende opdracht naar wens, voer de opdracht voor het exporteren van het configuratiebestand van het netwerk:
    
    ```azurecli
    azure network export c:\azure\networkconfig.json
    ```

## <a name="create-or-modify-a-network-configuration-file"></a>Maken of een configuratiebestand netwerk wijzigen

Een configuratiebestand netwerk is een XML-bestand (als u PowerShell) of een json-bestand (als u de Azure CLI). U kunt het bestand in een tekst- of XML-/ json-editor kunt bewerken. De [netwerk-instellingen voor het configuratiebestand schema](https://msdn.microsoft.com/library/azure/jj157100.aspx) artikel bevat informatie voor alle instellingen. Zie voor aanvullende uitleg van de instellingen [virtuele netwerken en instellingen weergeven](virtual-network-manage-network.md#view-vnet). De wijzigingen die u in het bestand aanbrengt:

- Moet voldoen aan het schema of het importeren van die het configuratiebestand van het netwerk mislukt.
- Overschrijf de bestaande netwerkinstellingen voor uw abonnement, dus Wees daarom uiterst voorzichtig als u wijzigingen aanbrengt. Verwijs bijvoorbeeld naar de configuratiebestanden van de voorbeeld-netwerk die volgen. Stel het oorspronkelijke bestand bevat twee **VirtualNetworkSite** exemplaren en u gewijzigd, zoals wordt weergegeven in de voorbeelden. Wanneer u het bestand importeert, Azure worden verwijderd van het virtuele netwerk voor de **VirtualNetworkSite** exemplaar dat u in het bestand verwijderd. Dit scenario vereenvoudigde wordt ervan uitgegaan dat er zijn geen bronnen in het virtuele netwerk alsof er, het virtuele netwerk kan niet worden verwijderd en het importeren mislukt.

> [!IMPORTANT]
> Azure overweegt een subnet met iets geïmplementeerd als **gebruikt**. Wanneer een subnet gebruikt wordt, kan niet worden gewijzigd. Voordat u informatie over het subnet in een configuratiebestand netwerk wijzigt, verplaatst u alles wat u hebt geïmplementeerd op het subnet moet een ander subnet dat is niet wordt gewijzigd. Zie [een virtuele machine of Rolinstantie verplaatsen naar een ander Subnet](virtual-networks-move-vm-role-to-subnet.md) voor meer informatie.

### <a name="example-xml-for-use-with-powershell"></a>XML-voorbeeld voor gebruik met PowerShell

Het volgende configuratiebestand van de voorbeeld-netwerk maakt een virtueel netwerk met de naam *myVirtualNetwork* met een adresruimte van *10.0.0.0/16* in de *VS-Oost* Azure de regio. Het virtuele netwerk bevat één subnet met de naam *mySubnet* met een adresvoorvoegsel van *10.0.0.0/24*.

```xml
<?xml version="1.0" encoding="utf-8"?>
<NetworkConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
  <VirtualNetworkConfiguration>
    <Dns />
    <VirtualNetworkSites>
      <VirtualNetworkSite name="myVirtualNetwork" Location="East US">
        <AddressSpace>
          <AddressPrefix>10.0.0.0/16</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="mySubnet">
            <AddressPrefix>10.0.0.0/24</AddressPrefix>
          </Subnet>
        </Subnets>
      </VirtualNetworkSite>
    </VirtualNetworkSites>
  </VirtualNetworkConfiguration>
</NetworkConfiguration>
```

Als het netwerk-configuratiebestand dat u hebt geëxporteerd geen inhoud bevat, kunt u de XML in het vorige voorbeeld kopieert en plakt u deze in een nieuw bestand.

### <a name="example-json-for-use-with-the-azure-cli-10"></a>Voorbeeld JSON voor gebruik met de Azure CLI 1.0

Het volgende configuratiebestand van de voorbeeld-netwerk maakt een virtueel netwerk met de naam *myVirtualNetwork* met een adresruimte van *10.0.0.0/16* in de *VS-Oost* Azure de regio. Het virtuele netwerk bevat één subnet met de naam *mySubnet* met een adresvoorvoegsel van *10.0.0.0/24*.

```json
{
   "VirtualNetworkConfiguration" : {
      "Dns" : "",
      "VirtualNetworkSites" : [
         {
            "AddressSpace" : [ "10.0.0.0/16" ],
            "Location" : "East US",
            "Name" : "myVirtualNetwork",
            "Subnets" : [
               {
                  "AddressPrefix" : "10.0.0.0/24",
                  "Name" : "mySubnet"
               }
            ]
         }
      ]
   }
}
```

Als het netwerk-configuratiebestand dat u hebt geëxporteerd geen inhoud bevat, kunt u kopiëren van de json in het vorige voorbeeld en plak deze in een nieuw bestand.

## <a name="import"></a>Een configuratiebestand netwerk importeren

U kunt PowerShell of Azure CLI gebruiken voor het importeren van een netwerk-configuratiebestand. PowerShell importeert een XML-bestand, terwijl de Azure CLI een json-bestand importeert. Als het importeren is mislukt, moet u bevestigen dat het bestand voldoet aan de [network-configuratieschema](https://msdn.microsoft.com/library/azure/jj157100.aspx). 

### <a name="powershell"></a>PowerShell
 
1. [Azure PowerShell installeren en aanmelden bij Azure](/powershell/azure/install-azure-ps?toc=%2fazure%2fvirtual-network%2ftoc.json).
2. Wijzig de map en bestandsnaam in de volgende opdracht als nodig Voer vervolgens de opdracht voor het importeren van het configuratiebestand van het netwerk:
 
    ```powershell
    Set-AzureVNetConfig  -ConfigurationPath c:\azure\networkconfig.xml
    ```

### <a name="azure-cli-10"></a>Azure CLI 1.0

1. [Installeer de Azure CLI 1.0](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json). De resterende stappen vanaf een opdrachtprompt Azure CLI 1.0.
2. Meld u aan bij Azure door te voeren de `azure login` opdracht.
3. Zorg ervoor dat u zich in de asm-modus door te voeren de `azure config mode asm` opdracht.
4. Wijzig de map en bestandsnaam in de volgende opdracht als nodig Voer vervolgens de opdracht voor het importeren van het configuratiebestand van het netwerk:

    ```azurecli
    azure network import c:\azure\networkconfig.json
    ```
