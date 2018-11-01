---
title: Een Azure-netwerk (klassiek) maken met meerdere subnetten | Microsoft Docs
description: Leer hoe u een virtueel netwerk (klassiek) maken met meerdere subnetten in Azure.
services: virtual-network
documentationcenter: ''
author: genlin
manager: cshepard
editor: ''
tags: azure-service-management
ms.assetid: ''
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/31/2018
ms.author: genli
ms.custom: ''
ms.openlocfilehash: e40648ef47b108050486d43eefdb1564786c053e
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2018
ms.locfileid: "50421387"
---
# <a name="create-a-virtual-network-classic-with-multiple-subnets"></a>Een virtueel netwerk (klassiek) maken met meerdere subnetten

> [!IMPORTANT]
> Azure heeft twee [verschillende implementatiemodellen](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) voor het maken van en werken met resources: Resource Manager en klassiek. Dit artikel gaat over het gebruik van het klassieke implementatiemodel. Microsoft raadt aan om het maken van de meeste nieuwe virtuele netwerken via de [Resource Manager](quick-create-portal.md) implementatiemodel.

In deze zelfstudie leert u hoe u een eenvoudige Azure-netwerk maken (klassiek) met afzonderlijke openbare en particuliere subnetten. U kunt Azure-resources, zoals virtuele machines en cloudservices in een subnet maken. Resources die zijn gemaakt in virtuele netwerken (klassiek) kunnen communiceren met elkaar en met resources in andere netwerken die zijn verbonden met een virtueel netwerk.

Meer informatie over alle [virtueel netwerk](manage-virtual-network.md) en [subnet](virtual-network-manage-subnet.md) instellingen.

> [!WARNING]
> Virtuele netwerken (klassiek) worden onmiddellijk verwijderd door Azure wanneer een [abonnement is uitgeschakeld](../billing/billing-subscription-become-disable.md?toc=%2fazure%2fvirtual-network%2ftoc.json#you-reached-your-spending-limit). Virtuele netwerken (klassiek) zijn verwijderd, ongeacht of de resources in het virtuele netwerk bestaat. Als u later het abonnement opnieuw inschakelt, kan de resources die aanwezig in het virtuele netwerk was moeten worden gemaakt.

U een virtueel netwerk (klassiek) kunt maken met behulp van de [Azure-portal](#portal), wordt de [Azure-opdrachtregelinterface (CLI) 1.0](#azure-cli), of [PowerShell](#powershell).

## <a name="portal"></a>Portal

1. Ga in een internetbrowser naar de [Azure-portal](https://portal.azure.com). Meld u aan met uw [Azure-account](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account). Als u nog geen Azure-account hebt, kunt u zich registreren voor een [gratis proefversie](https://azure.microsoft.com/offers/ms-azr-0044p).
2. Klik op **een resource maken** in de portal.
3. Voer *virtueel netwerk* in de **Marketplace doorzoeken** vak aan de bovenkant van de **nieuw** deelvenster dat wordt weergegeven. Klik op **virtueel netwerk** wanneer deze wordt weergegeven in de lijst met zoekresultaten.
4. Selecteer **klassieke** in de **een implementatiemodel selecteren** vak de **Virtueelnetwerk** deelvenster dat verschijnt, klikt u vervolgens op **maken**. 
5. Voer de volgende waarden op de **virtueel netwerk maken (klassiek)** deelvenster en klik vervolgens op **maken**:

    |Instelling|Waarde|
    |---|---|
    |Naam|myVnet|
    |Adresruimte|10.0.0.0/16|
    |Subnetnaam|Public|
    |Subnetadresbereik|10.0.0.0/24|
    |Resourcegroep|Laat **nieuw** geselecteerd en voer vervolgens **myResourceGroup**.|
    |Abonnement en de locatie|Selecteer uw abonnement en de locatie.

    Als u bekend bent met Azure bent, meer informatie over [resourcegroepen](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group), [abonnementen](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription), en [locaties](https://azure.microsoft.com/regions) (ook wel *regio's*).
4. In de portal, kunt u slechts één subnet maken wanneer u een virtueel netwerk maken. In deze zelfstudie maakt u een tweede subnet nadat u het virtuele netwerk hebt gemaakt. U kunt later maken via Internet toegankelijke resources in de **openbare** subnet. U kunt resources die niet toegankelijk is vanaf Internet in ook maken de **persoonlijke** subnet. Voer voor het maken van het tweede subnet **myVnet** in de **zoeken naar resources** vak aan de bovenkant van de pagina. Klik op **myVnet** wanneer deze wordt weergegeven in de lijst met zoekresultaten.
5. Klik op **subnetten** (in de **instellingen** sectie) op de **virtueel netwerk maken (klassiek)** deelvenster dat wordt weergegeven.
6. Klik op **+ toevoegen** op de **myVnet - subnetten** deelvenster dat wordt weergegeven.
7. Voer **persoonlijke** voor **naam** op de **subnet toevoegen** deelvenster. Voer **10.0.1.0/24** voor **adresbereik**.  Klik op **OK**.
8. Op de **myVnet - subnetten** deelvenster ziet u de **openbare** en **persoonlijke** subnetten die u hebt gemaakt.
9. **Optionele**: wanneer u klaar bent met deze zelfstudie, kunt u de resources verwijderen die u hebt gemaakt, zodat u hiervoor geen kosten voor het gebruik:
    - Klik op **overzicht** op de **myVnet** deelvenster.
    - Klik op de **verwijderen** pictogram op de **myVnet** deelvenster.
    - Als u wilt bevestigen, klikt u op **Ja** in de **virtueel netwerk verwijderen** vak.

## <a name="azure-cli"></a>Azure-CLI

1. U kunt ofwel [installeren en configureren van de Azure CLI](../cli-install-nodejs.md?toc=%2fazure%2fvirtual-network%2ftoc.json), of de CLI in de Azure Cloud Shell gebruiken. De Azure Cloud Shell is een gratis Bash-shell die u rechtstreeks in Azure Portal kunt uitvoeren. In deze shell is de Azure CLI vooraf geïnstalleerd en geconfigureerd voor gebruik met uw account. Voor hulp bij CLI-opdrachten, typt u `azure <command> --help`. 
2. In een CLI-sessie, meld u aan bij Azure met de opdracht die volgt. Als u klikt op **uitproberen** in het onderstaande vak een Cloud Shell wordt geopend. U kunt zich aanmeldt bij uw Azure-abonnement, zonder in te voeren van de volgende opdracht uit:

    ```azurecli-interactive
    azure login
    ```

3. Om ervoor te zorgen de CLI in Service Management-modus, voer de volgende opdracht:

    ```azurecli-interactive
    azure config mode asm
    ```

4. Een virtueel netwerk maken met een privé-subnet:

    ```azurecli-interactive
    azure network vnet create --vnet myVnet --address-space 10.0.0.0 --cidr 16  --subnet-name Private --subnet-start-ip 10.0.0.0 --subnet-cidr 24 --location "East US"
    ```

5. Maak een openbaar subnet binnen het virtuele netwerk:

    ```azurecli-interactive
    azure network vnet subnet create --name Public --vnet-name myVnet --address-prefix 10.0.1.0/24
    ```    
    
6. Lees het virtuele netwerk en subnetten:

    ```azurecli-interactive
    azure network vnet show --vnet myVnet
    ```

7. **Optionele**: kunt u de resources verwijderen die u hebt gemaakt wanneer u klaar bent met deze zelfstudie, zodat u hiervoor geen kosten voor het gebruik:

    ```azurecli-interactive
    azure network vnet delete --vnet myVnet --quiet
    ```

> [!NOTE]
> Hoewel u een resourcegroep voor het maken van een virtueel netwerk (klassiek) in met behulp van de CLI kan niet opgeeft, maakt Azure het virtuele netwerk in een resourcegroep met de naam *standaard-netwerk*.

## <a name="powershell"></a>PowerShell

1. Installeer de nieuwste versie van de PowerShell [Azure](https://www.powershellgallery.com/packages/Azure) module. Zie [Overzicht van Azure PowerShell](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json) als u nog geen ervaring hebt met Azure PowerShell.
2. Start een PowerShell-sessie.
3. Meld u in PowerShell aan bij Azure door het commando `Add-AzureAccount` in te voeren.
4. Het volgende pad en bestandsnaam, waar nodig, wijzigen en vervolgens uw bestaande netwerkconfiguratiebestand exporteert:

    ```powershell
    Get-AzureVNetConfig -ExportToFile c:\azure\NetworkConfig.xml
    ```

5. Gebruik voor het maken van een virtueel netwerk met openbare en particuliere subnetten, een teksteditor om toe te voegen de **VirtualNetworkSite** element zijn dat op het netwerkconfiguratiebestand volgt.

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

    Bekijk de volledige [schema van het netwerkconfiguratiebestand](https://msdn.microsoft.com/library/azure/jj157100.aspx).

6. Importeer het netwerkconfiguratiebestand:

    ```powershell
    Set-AzureVNetConfig -ConfigurationPath c:\azure\NetworkConfig.xml
    ```

    > [!WARNING]
    > Een gewijzigde netwerkconfiguratiebestand importeren kan leiden tot wijzigingen in bestaande virtuele netwerken (klassiek) in uw abonnement. Zorg ervoor dat u alleen de vorige virtueel netwerk toevoegen en u niet wijzigen of verwijderen van een bestaande virtuele netwerken van uw abonnement. 

7. Lees het virtuele netwerk en subnetten:

    ```powershell
    Get-AzureVNetSite -VNetName "myVnet"
    ```

8. **Optionele**: kunt u de resources verwijderen die u hebt gemaakt wanneer u klaar bent met deze zelfstudie, zodat u hiervoor geen kosten voor het gebruik. Als u wilt verwijderen van het virtuele netwerk, volledige stappen 4-6 nogmaals, het verwijderen van deze tijd de **VirtualNetworkSite** element dat u in stap 5 hebt toegevoegd.
 
> [!NOTE]
> Hoewel u een resourcegroep voor het maken van een virtueel netwerk (klassiek) in met behulp van PowerShell kan niet opgeeft, maakt Azure het virtuele netwerk in een resourcegroep met de naam *standaard-netwerk*.

---

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over alle virtuele netwerk en subnet-instellingen, [virtuele netwerken beheren](manage-virtual-network.md) en [subnetten van het virtuele netwerk beheren](virtual-network-manage-subnet.md). U hebt verschillende opties voor het gebruik van virtuele netwerken en subnetten in een productieomgeving om te voldoen aan verschillende vereisten.
- Maak een [Windows](../virtual-machines/windows/classic/createportal-classic.md?toc=%2fazure%2fvirtual-network%2ftoc.json) of een [Linux](../virtual-machines/linux/classic/createportal-classic.md?toc=%2fazure%2fvirtual-network%2ftoc.json) virtuele machine en verbindt dit vervolgens met een bestaand virtueel netwerk.
- Als u wilt verbinding maken met twee virtuele netwerken in dezelfde Azure-locatie, maak een [peering op virtueel netwerk](create-peering-different-deployment-models.md) tussen de virtuele netwerken. U kunt een virtueel netwerk (Resource Manager) koppelen aan een virtueel netwerk (klassiek), maar u kunt geen maken een peering tussen twee virtuele netwerken (klassiek).
- Met het virtuele netwerk een on-premises netwerk te maken met een [VPN-Gateway](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) of [Azure ExpressRoute](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md?toc=%2fazure%2fvirtual-network%2ftoc.json) circuit.
