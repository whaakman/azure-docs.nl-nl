---
title: Een Azure-netwerk (klassiek) maken met meerdere subnetten | Microsoft Docs
description: Ontdek hoe u een virtueel netwerk (klassiek) maken met meerdere subnetten in Azure.
services: virtual-network
documentationcenter: 
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: jdial
ms.custom: 
ms.openlocfilehash: 099e3c259f0b63e4376847727eb8e185aeb37380
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="create-a-virtual-network-classic-with-multiple-subnets"></a>Een virtueel netwerk (klassiek) maken met meerdere subnetten

> [!IMPORTANT]
> Azure heeft twee [verschillende implementatiemodellen](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) voor het maken en werken met resources: Resource Manager en classic. Dit artikel gaat over het gebruik van het klassieke implementatiemodel. Microsoft raadt u aan het maken van de meeste nieuwe virtuele netwerken via de [Resource Manager](virtual-networks-create-vnet-arm-pportal.md) implementatiemodel.

Informatie over het maken van een eenvoudige Azure-netwerk (klassiek) met afzonderlijke openbare en particuliere subnetten in deze zelfstudie. U kunt Azure-resources, zoals virtuele machines en cloudservices in een subnet maken. Resources die zijn gemaakt in virtuele netwerken (klassiek) kunnen communiceren met elkaar en met resources in andere netwerken die zijn verbonden met een virtueel netwerk.

Meer informatie over alle [virtueel netwerk](virtual-network-manage-network.md) en [subnet](virtual-network-manage-subnet.md) instellingen.

> [!WARNING]
> Virtuele netwerken (klassiek), worden onmiddellijk verwijderd door Azure wanneer een [abonnement is uitgeschakeld](../billing/billing-subscription-become-disable.md?toc=%2fazure%2fvirtual-network%2ftoc.json#you-reached-your-spending-limit). Virtuele netwerken (klassiek) verwijderd, ongeacht of de bronnen in het virtuele netwerk bestaan. Als u later het abonnement opnieuw inschakelt, resources die beschikbaar in het virtuele netwerk waren moeten opnieuw worden gemaakt.

U een virtueel netwerk (klassiek) kunt maken met behulp van de [Azure-portal](#portal), wordt de [Azure-opdrachtregelinterface (CLI) 1.0](#azure-cli), of [PowerShell](#powershell).

## <a name="portal"></a>Portal

1. In een internetbrowser, gaat u naar de [Azure-portal](https://portal.azure.com). Meld u aan met uw [Azure-account](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account). Als u nog geen Azure-account hebt, kunt u zich registreren voor een [gratis proefversie](https://azure.microsoft.com/offers/ms-azr-0044p).
2. Klik op **+ nieuw** in de portal.
3. Voer *virtueel netwerk* in de **zoeken van de Marketplace** vak aan de bovenkant van de **nieuw** blade die wordt weergegeven.  Klik op **virtueel netwerk** wanneer deze wordt weergegeven in de zoekresultaten.
4. Selecteer **klassieke** in de **een implementatiemodel selecteren** vak de **virtueel netwerk** blade die wordt weergegeven, klikt u vervolgens op **maken**. 
5. Voer de volgende waarden op de **virtueel netwerk maken (klassiek)** blade en klik vervolgens op **maken**:

    |Instelling|Waarde|
    |---|---|
    |Naam|myVnet|
    |Adresruimte|10.0.0.0/16|
    |Subnetnaam|Openbaar|
    |Subnetadresbereik|10.0.0.0/24|
    |Resourcegroep|Laat **nieuw** geselecteerd en voer vervolgens **myResourceGroup**.|
    |Abonnement en de locatie|Selecteer uw abonnement en locatie.

    Als u geen ervaring met Azure, meer informatie over [resourcegroepen](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group), [abonnementen](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription), en [locaties](https://azure.microsoft.com/regions) (ook wel *regio's*).
4. U kunt slechts één subnet in de portal maken wanneer u een virtueel netwerk maken. In deze zelfstudie maakt u een tweede subnet nadat u het virtuele netwerk hebt gemaakt. U kunt later maken met Internet toegankelijke bronnen in de **openbare** subnet. U bronnen die niet toegankelijk is vanaf het Internet in ook mogelijk maakt de **persoonlijke** subnet. Voer voor het maken van het tweede subnet **myVnet** in de **zoeken bronnen** vak aan de bovenkant van de pagina. Klik op **myVnet** wanneer deze wordt weergegeven in de zoekresultaten.
5. Klik op **subnetten** (in de **instellingen** sectie) op de **virtueel netwerk maken (klassiek)** blade die wordt weergegeven.
6. Klik op **+ toevoegen** op de **myVnet - subnetten** blade die wordt weergegeven.
7. Voer **persoonlijke** voor **naam** op de **subnet toevoegen** blade. Voer **10.0.1.0/24** voor **-adresbereik**.  Klik op **OK**.
8. Op de **myVnet - subnetten** , ziet u de **openbare** en **persoonlijke** subnetten die u hebt gemaakt.
9. **Optionele**: wanneer u deze zelfstudie hebt voltooid, wilt u mogelijk de resources verwijderen die u hebt gemaakt, zodat u geen gebruik kosten:
    - Klik op **overzicht** op de **myVnet** blade.
    - Klik op de **verwijderen** pictogram op de **myVnet** blade.
    - Het verwijderen te bevestigen, klikt u op **Ja** in de **virtuele netwerk verwijderen** vak.

## <a name="azure-cli"></a>Azure-CLI

1. U kunt ofwel [installeren en configureren van de Azure CLI](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json), of gebruik de CLI vanuit de Azure-Cloud-Shell. De Azure Cloud Shell is een gratis Bash-shell die u rechtstreeks in Azure Portal kunt uitvoeren. In deze shell is de Azure CLI vooraf geïnstalleerd en geconfigureerd voor gebruik met uw account. Typ voor hulp bij CLI-opdrachten, `azure <command> --help`. 
2. In een sessie CLI aanmelden bij Azure met de volgende opdracht. Als u op **Try it** in het onderstaande vak een Cloud-Shell wordt geopend. U kunt aanmelden bij uw Azure-abonnement zonder invoeren van de volgende opdracht:

    ```azurecli-interactive
    azure login
    ```

3. Om ervoor te zorgen de CLI in Service Management-modus, voer de volgende opdracht:

    ```azurecli-interactive
    azure config mode asm
    ```

4. Een virtueel netwerk maken met een particulier subnet:

    ```azurecli-interactive
    azure network vnet create --vnet myVnet --address-space 10.0.0.0 --cidr 16  --subnet-name Private --subnet-start-ip 10.0.0.0 --subnet-cidr 24 --location "East US"
    ```

5. Maak een openbare subnet binnen het virtuele netwerk:

    ```azurecli-interactive
    azure network vnet subnet create --name Public --vnet-name myVnet --address-prefix 10.0.1.0/24
    ```    
    
6. Controleer het virtuele netwerk en subnetten:

    ```azurecli-interactive
    azure network vnet show --vnet myVnet
    ```

7. **Optionele**: U kunt de resources die u hebt gemaakt wanneer u klaar bent met deze zelfstudie, zodat u geen gebruik kosten verwijderen:

    ```azurecli-interactive
    azure network vnet delete --vnet myVnet --quiet
    ```

> [!NOTE]
> Hoewel u een resourcegroep voor het maken van een virtueel netwerk (klassiek) in de CLI niet opgeven, Azure maakt van het virtuele netwerk in een resourcegroep met de naam *standaard netwerken*.

## <a name="powershell"></a>PowerShell

1. Installeer de nieuwste versie van de PowerShell [Azure](https://www.powershellgallery.com/packages/Azure) module. Zie [Overzicht van Azure PowerShell](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json) als u nog geen ervaring hebt met Azure PowerShell.
2. Een PowerShell-sessie starten.
3. Meld u in PowerShell aan bij Azure door het commando `Add-AzureAccount` in te voeren.
4. Wijzig het volgende pad en bestandsnaam naar gelang van toepassing, en uw bestaande netwerk-configuratiebestand exporteren:

    ```powershell
    Get-AzureVNetConfig -ExportToFile c:\azure\NetworkConfig.xml
    ```

5. Gebruik een virtueel netwerk maakt met openbare en particuliere subnetten, een teksteditor om toe te voegen de **VirtualNetworkSite** element zijn dat naar het configuratiebestand van het netwerk volgt.

    ```xml
    <VirtualNetworkSite name="myVnet" Location="East US">
        <AddressSpace>
          <AddressPrefix>10.0.0.0/16</AddressPrefix>
        </AddressSpace>
        <Subnets>
          <Subnet name="Private">
            <AddressPrefix>10.0.0.0/24</AddressPrefix>
          </Subnet>
          <Subnet name="Public">
            <AddressPrefix>10.0.1.0/24</AddressPrefix>
          </Subnet>
        </Subnets>
      </VirtualNetworkSite>
    ```

    Bekijk de volledige [network-configuratieschema bestand](https://msdn.microsoft.com/library/azure/jj157100.aspx).

6. Importeer het configuratiebestand van het netwerk:

    ```powershell
    Set-AzureVNetConfig -ConfigurationPath c:\azure\NetworkConfig.xml
    ```

    > [!WARNING]
    > Een configuratiebestand gewijzigde netwerk importeren kan leiden tot wijzigingen in bestaande virtuele netwerken (klassiek) in uw abonnement. Zorg ervoor dat u alleen de vorige virtueel netwerk toevoegen en u niet wijzigen of verwijderen van een bestaande virtuele netwerken uit uw abonnement. 

7. Controleer het virtuele netwerk en subnetten:

    ```powershell
    Get-AzureVNetSite -VNetName "myVnet"
    ```

8. **Optionele**: U kunt de resources die u hebt gemaakt wanneer u klaar bent met deze zelfstudie, zodat u geen gebruik kosten verwijderen. Voor het verwijderen van het virtuele netwerk volledige stap 4-6 opnieuw verwijderen van deze tijd de **VirtualNetworkSite** element dat u in stap 5 hebt toegevoegd.
 
> [!NOTE]
> Hoewel u een resourcegroep voor het maken van een virtueel netwerk (klassiek) in met behulp van PowerShell kan niet opgeeft, Azure maakt van het virtuele netwerk in een resourcegroep met de naam *standaard netwerken*.

---

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over alle virtueel netwerk en subnetinstellingen, [virtuele netwerken beheren](virtual-network-manage-network.md) en [beheren van virtueel netwerksubnetten](virtual-network-manage-subnet.md). U beschikt over verschillende opties voor het gebruik van virtuele netwerken en subnetten in een productieomgeving om te voldoen aan verschillende vereisten.
- Van binnenkomende en uitgaande subnetverkeer filteren, maken en toepassen [netwerkbeveiligingsgroepen](virtual-networks-nsg.md) aan subnetten.
- Maak een [Windows](../virtual-machines/windows/classic/createportal-classic.md?toc=%2fazure%2fvirtual-network%2ftoc.json) of een [Linux](../virtual-machines/linux/classic/createportal-classic.md?toc=%2fazure%2fvirtual-network%2ftoc.json) virtuele machine en maak verbinding met een bestaand virtueel netwerk.
- Voor verbinding twee virtuele netwerken in dezelfde Azure-locatie, maak een [virtueel netwerk peering](create-peering-different-deployment-models.md) tussen de virtuele netwerken. U kunt een virtueel netwerk (Resource Manager) peer met een virtueel netwerk (klassiek), maar u kunt geen maken een peering tussen twee virtuele netwerken (klassiek).
- Het virtuele netwerk verbinding met een on-premises netwerk via een [VPN-Gateway](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) of [Azure ExpressRoute](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md?toc=%2fazure%2fvirtual-network%2ftoc.json) circuit.
