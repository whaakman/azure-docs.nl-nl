---
title: Configureren van een Azure-netwerk (klassiek) - netwerkconfiguratiebestand | Microsoft Docs
description: Informatie over het maken en aanpassen van virtuele netwerken (klassiek) met exporteren en importeren van een netwerkconfiguratiebestand wijzigen.
services: virtual-network
documentationcenter: ''
author: genlin
manager: cshepard
editor: ''
tags: azure-service-management
ms.assetid: c29b9059-22b0-444e-bbfe-3e35f83cde2f
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/23/2017
ms.author: genli
ms.custom: ''
ms.openlocfilehash: 57ad5541bb7b61f8d26002168bb069fad3058965
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/31/2018
ms.locfileid: "43339071"
---
# <a name="configure-a-virtual-network-classic-using-a-network-configuration-file"></a>Een virtueel netwerk (klassiek) met behulp van een netwerkconfiguratiebestand configureren
> [!IMPORTANT]
> Azure heeft twee verschillende implementatiemodellen voor het maken van en werken met resources: [Resource Manager en klassieke](../resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Dit artikel gaat over het gebruik van het klassieke implementatiemodel. Microsoft raadt aan dat de meeste nieuwe implementaties het Resource Manager-implementatiemodel gebruiken.

U kunt maken en configureren van een virtueel netwerk (klassiek) met een netwerkconfiguratiebestand met behulp van de Azure-opdrachtregelinterface (CLI) 1.0 of Azure PowerShell. U kunt maken of wijzigen van een virtueel netwerk via het Azure Resource Manager-implementatiemodel met behulp van een netwerkconfiguratiebestand. U niet de Azure-portal gebruiken voor het maken of wijzigen van een virtueel netwerk (klassiek) met behulp van een netwerkconfiguratiebestand, maar u de Azure-portal gebruiken kunt om te maken van een virtueel netwerk (klassiek) zonder een netwerkconfiguratiebestand.

Het maken en configureren van een virtueel netwerk (klassiek) met een netwerkconfiguratiebestand moet exporteren en importeren van het bestand wijzigen.

## <a name="export"></a>Exporteren van een netwerkconfiguratiebestand

U kunt PowerShell of Azure CLI gebruiken voor het exporteren van een netwerkconfiguratiebestand. PowerShell exporteert een XML-bestand, terwijl de Azure CLI een json-bestand exporteert.

### <a name="powershell"></a>PowerShell
 
1. [Azure PowerShell installeren en aanmelden bij Azure](https://docs.microsoft.com/azure/azure-stack/azure-stack-powershell-install).
2. Wijzig de map (en zorg ervoor dat deze bestaat) en de bestandsnaam in de volgende opdracht naar wens, voer de opdracht voor het exporteren van het netwerkconfiguratiebestand:

    ```powershell
    Get-AzureVNetConfig -ExportToFile c:\azure\networkconfig.xml
    ```

### <a name="azure-cli-10"></a>Azure CLI 1.0

1. [De Azure CLI 1.0 installeren](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Voltooi de resterende stappen van de Azure CLI 1.0-prompt.
2. Meld u aan bij Azure door te voeren de `azure login` opdracht.
3. Zorg ervoor dat u bent in asm-modus door op te geven de `azure config mode asm` opdracht.
4. Wijzig de map (en zorg ervoor dat deze bestaat) en de bestandsnaam in de volgende opdracht naar wens, voer de opdracht voor het exporteren van het netwerkconfiguratiebestand:
    
    ```azurecli
    azure network export c:\azure\networkconfig.json
    ```

## <a name="create-or-modify-a-network-configuration-file"></a>Maken of wijzigen van een netwerkconfiguratiebestand

Een netwerkconfiguratiebestand is een XML-bestand (bij gebruik van PowerShell) of een json-bestand (als u de Azure CLI). U kunt het bestand in een tekst of een XML-/ json-editor kunt bewerken. De [netwerk-instellingen voor het configuratiebestand schema](https://msdn.microsoft.com/library/azure/jj157100.aspx) artikel bevat informatie voor alle instellingen. Zie voor aanvullende informatie over van de instellingen, [virtuele netwerken en instellingen weergeven](manage-virtual-network.md#view-virtual-networks-and-settings). De wijzigingen die u in het bestand aanbrengt:

- Moet voldoen aan het schema of het importeren van die het netwerkconfiguratiebestand mislukken.
- Overschrijven van een bestaande netwerkinstellingen voor uw abonnement, dus moet u bijzonder voorzichtig wanneer u wijzigingen aanbrengt. Verwijs bijvoorbeeld naar de configuratiebestanden van de voorbeeld-netwerk die volgen. Stel dat het oorspronkelijke bestand bevat twee **VirtualNetworkSite** instanties, en u gewijzigd, zoals wordt weergegeven in de voorbeelden. Wanneer u het bestand importeert, Azure Hiermee verwijdert u het virtuele netwerk voor de **VirtualNetworkSite** instantie die u in het bestand verwijderd. Dit eenvoudige scenario wordt ervan uitgegaan dat er zijn geen bronnen in het virtuele netwerk, als er waren, het virtuele netwerk kan niet worden verwijderd en het importeren mislukt.

> [!IMPORTANT]
> Azure rekening gehouden met een subnet dat er iets geïmplementeerd als **gebruikt**. Wanneer een subnet gebruikt wordt, kan niet worden gewijzigd. Voordat u informatie over het subnet in een netwerkconfiguratiebestand wijzigt, verplaatst u alles wat u hebt geïmplementeerd op het subnet naar een ander subnet die is niet wordt gewijzigd. Zie [een virtuele machine of Rolinstantie verplaatsen naar een ander Subnet](virtual-networks-move-vm-role-to-subnet.md) voor meer informatie.

### <a name="example-xml-for-use-with-powershell"></a>XML-voorbeeld voor gebruik met PowerShell

Het volgende voorbeeld van het netwerkconfiguratiebestand maakt een virtueel netwerk met de naam *myVirtualNetwork* met een adresruimte van *10.0.0.0/16* in de *VS-Oost* Azure de regio. Het virtuele netwerk bevat één subnet met de naam *mySubnet* met een adresvoorvoegsel van *10.0.0.0/24*.

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

Als het netwerkconfiguratiebestand die u hebt geëxporteerd geen inhoud bevat, kunt u het XML-bestand in het vorige voorbeeld kopiëren en plak deze in een nieuw bestand.

### <a name="example-json-for-use-with-the-azure-cli-10"></a>Voorbeeld van JSON voor gebruik met de Azure CLI 1.0

Het volgende voorbeeld van het netwerkconfiguratiebestand maakt een virtueel netwerk met de naam *myVirtualNetwork* met een adresruimte van *10.0.0.0/16* in de *VS-Oost* Azure de regio. Het virtuele netwerk bevat één subnet met de naam *mySubnet* met een adresvoorvoegsel van *10.0.0.0/24*.

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

Als het netwerkconfiguratiebestand die u hebt geëxporteerd geen inhoud bevat, kunt u de json kopiëren in het vorige voorbeeld, en plak deze in een nieuw bestand.

## <a name="import"></a>Importeren van een netwerkconfiguratiebestand

U kunt PowerShell of Azure CLI gebruiken voor het importeren van een netwerkconfiguratiebestand. PowerShell importeert een XML-bestand, terwijl de Azure CLI de invoer een json-bestand. Als het importeren is mislukt, controleert u of dat het bestand voldoet aan de [network-configuratieschema](https://msdn.microsoft.com/library/azure/jj157100.aspx). 

### <a name="powershell"></a>PowerShell
 
1. [Azure PowerShell installeren en aanmelden bij Azure](https://docs.microsoft.com/azure/azure-stack/azure-stack-powershell-install).
2. Wijzig de map en bestandsnaam in de volgende opdracht uit als nodig is, voert u de opdracht voor het importeren van het netwerkconfiguratiebestand:
 
    ```powershell
    Set-AzureVNetConfig  -ConfigurationPath c:\azure\networkconfig.xml
    ```

### <a name="azure-cli-10"></a>Azure CLI 1.0

1. [De Azure CLI 1.0 installeren](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Voltooi de resterende stappen van de Azure CLI 1.0-prompt.
2. Meld u aan bij Azure door te voeren de `azure login` opdracht.
3. Zorg ervoor dat u bent in asm-modus door op te geven de `azure config mode asm` opdracht.
4. Wijzig de map en bestandsnaam in de volgende opdracht uit als nodig is, voert u de opdracht voor het importeren van het netwerkconfiguratiebestand:

    ```azurecli
    azure network import c:\azure\networkconfig.json
    ```
