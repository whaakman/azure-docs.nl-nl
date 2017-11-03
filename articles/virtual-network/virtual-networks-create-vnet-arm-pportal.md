---
title: Een Azure-netwerk maken met meerdere subnetten | Microsoft Docs
description: Informatie over het maken van een virtueel netwerk met meerdere subnetten in Azure.
services: virtual-network
documentationcenter: 
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 4ad679a4-a959-4e48-a317-d9f5655a442b
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/26/2017
ms.author: jdial
ms.custom: 
ms.openlocfilehash: f82a95ec9543b2d53ef28bf7f15315e23cf4893a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-virtual-network-with-multiple-subnets"></a>Een virtueel netwerk maken met meerdere subnetten

Informatie over het maken van een eenvoudige Azure-netwerk met afzonderlijke openbare en particuliere subnetten in deze zelfstudie. Resources in virtuele netwerken kunnen communiceren met elkaar en met resources in andere netwerken die zijn verbonden met een virtueel netwerk. U kunt Azure-resources, zoals virtuele machines, App Service-omgevingen, virtuele-machineschaalsets, Azure HDInsight en cloudservices in de dezelfde of verschillende subnetten binnen een virtueel netwerk maken. Maken van resources in verschillende subnetten kunt u om netwerkverkeer te filteren en afmelden van subnetten onafhankelijk van elkaar met [netwerkbeveiligingsgroepen](virtual-networks-create-nsg-arm-pportal.md), en zo de [routeren van verkeer tussen subnetten](virtual-network-create-udr-arm-ps.md) via netwerk virtuele apparaten, zoals een firewall, als u wilt. 

De volgende secties vindt u stappen die u nemen kunt voor een virtueel netwerk maken met behulp van de [Azure-portal](#portal), de Azure-opdrachtregelinterface ([Azure CLI](#azure-cli)), [Azure PowerShell](#powershell), en een [Azure Resource Manager-sjabloon](#resource-manager-template). Het resultaat is hetzelfde, ongeacht welk hulpmiddel u gebruikt voor het maken van het virtuele netwerk. Klik op de koppeling van een hulpprogramma naar die sectie van de zelfstudie. Meer informatie over alle [virtueel netwerk](virtual-network-manage-network.md) en [subnet](virtual-network-manage-subnet.md) instellingen.

Dit artikel bevat stappen voor het maken van een virtueel netwerk via het Resource Manager-implementatiemodel het implementatiemodel dat wordt u aangeraden is bij het maken van nieuwe virtuele netwerken. Als u een virtueel netwerk (klassiek) maken moet, Zie [een virtueel netwerk (klassiek) maken](create-virtual-network-classic.md). Als u niet bekend met Azure implementatiemodellen bent, Zie [begrijpen Azure-implementatiemodellen](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

## <a name="portal"></a>Azure-portal

1. In een internetbrowser, gaat u naar de [Azure-portal](https://portal.azure.com). Meld u aan met uw [Azure-account](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account). Als u nog geen Azure-account hebt, kunt u zich registreren voor een [gratis proefversie](https://azure.microsoft.com/offers/ms-azr-0044p).
2. Klik in de portal op **+ nieuw** > **Networking** > **virtueel netwerk**.
3. Op de **virtueel netwerk maken** blade, voer de volgende waarden en klik vervolgens op **maken**:

    |Instelling|Waarde|
    |---|---|
    |Naam|myVnet|
    |Adresruimte|10.0.0.0/16|
    |Subnetnaam|Openbaar|
    |Subnetadresbereik|10.0.0.0/24|
    |Resourcegroep|Laat **nieuw** geselecteerd en voer vervolgens **myResourceGroup**.|
    |Abonnement en de locatie|Selecteer uw abonnement en locatie.

    Als u geen ervaring met Azure, meer informatie over [resourcegroepen](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group), [abonnementen](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription), en [locaties](https://azure.microsoft.com/regions) (ook wel *regio's*).
4. U kunt slechts één subnet in de portal maken wanneer u een virtueel netwerk maken. In deze zelfstudie maakt u een tweede subnet nadat u het virtuele netwerk hebt gemaakt. U kunt later maken met Internet toegankelijke bronnen in de **openbare** subnet. U bronnen die niet toegankelijk is vanaf het Internet in ook mogelijk maakt de **persoonlijke** subnet. Het tweede subnet maken in de **zoeken bronnen** vak boven aan de pagina **myVnet**. Klik in de zoekresultaten op **myVnet**. Als u meerdere virtuele netwerken met dezelfde naam in uw abonnement hebt, controleert u de brongroepen die worden vermeld in elk virtueel netwerk. Zorg ervoor dat u klikt u op de **myVnet** zoekresultaat die de resourcegroep **myResourceGroup**.
5. Op de **myVnet** blade onder **instellingen**, klikt u op **subnetten**.
6. Op de **myVnet - subnetten** blade, klikt u op **+ Subnet**.
7. Op de **subnet toevoegen** blade voor **naam**, voer **persoonlijke**. Voor **-adresbereik**, voer **10.0.1.0/24**.  Klik op **OK**.
8. Op de **myVnet - subnetten** blade bekijken van de subnetten. U ziet de **openbare** en **persoonlijke** subnetten die u hebt gemaakt.
9. **Optioneel:** Voltooi aanvullende zelfstudies die worden vermeld onder [Vervolgstappen](#next-steps) om netwerkverkeer te filteren en afmelden van elk subnet met netwerkbeveiligingsgroepen om verkeer te leiden tussen subnetten door middel van een virtueel netwerkapparaat , of het virtuele netwerk verbinding met andere virtuele netwerken of on-premises netwerken.
10. **Optioneel:** verwijderen van de resources die u in deze zelfstudie maakt via de stappen in [resources verwijderen](#delete-portal).

## <a name="azure-cli"></a>Azure CLI

Azure CLI-opdrachten zijn hetzelfde, ongeacht of u de opdrachten vanaf Windows, Linux of Mac OS uitvoeren. Er zijn echter scripting verschillen tussen de houders van het besturingssysteem. Het script in de volgende stappen worden uitgevoerd in een Bash-shell. 

1. [Installeren en configureren van de Azure CLI](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Zorg ervoor dat u hebt de meest recente versie van de Azure CLI geïnstalleerd. Typ voor hulp bij CLI-opdrachten, `az <command> --help`. In plaats van de CLI en de vereisten installeert, kunt u de Azure-Cloud-Shell. De Azure Cloud Shell is een gratis Bash-shell die u rechtstreeks in Azure Portal kunt uitvoeren. De Shell Cloud heeft de Azure CLI vooraf is geïnstalleerd en geconfigureerd voor gebruik met uw account. Met de Cloud-Shell, klikt u op de Cloud-Shell (**> _**) knop aan de bovenkant van de [portal](https://portal.azure.com) of klik op de *Try it* knop in de stappen volgen. 
2. Als de CLI lokaal uitgevoerd, meld u aan bij Azure met de `az login` opdracht. Als de Cloud-Shell, bent u al aangemeld.
3. Bekijk het volgende script en bijbehorende opmerkingen. Kopieer het script in uw browser en plak deze in de CLI-sessie:

    ```azurecli-interactive
    #!/bin/bash
    
    # Create a resource group.
    az group create \
      --name myResourceGroup \
      --location eastus
    
    # Create a virtual network with one subnet named Public.
    az network vnet create \
      --name myVnet \
      --resource-group myResourceGroup \
      --subnet-name Public
    
    # Create an additional subnet named Private in the virtual network.
    az network vnet subnet create \
      --name Private \
      --address-prefix 10.0.1.0/24 \
      --vnet-name myVnet \
      --resource-group myResourceGroup
    ```
    
4. Wanneer het script is voltooid wordt uitgevoerd, Controleer de subnetten voor het virtuele netwerk. Kopieer de volgende opdracht en plak deze in de CLI-sessie:

    ```azurecli
    az network vnet subnet list --resource-group myResourceGroup --vnet-name myVnet --output table
    ```

5. **Optioneel:** Voltooi aanvullende zelfstudies die worden vermeld onder [Vervolgstappen](#next-steps) om netwerkverkeer te filteren en afmelden van elk subnet met netwerkbeveiligingsgroepen om verkeer te leiden tussen subnetten door middel van een virtueel netwerkapparaat , of het virtuele netwerk verbinding met andere virtuele netwerken of on-premises netwerken.
6. **Optionele**: verwijderen van de resources die u in deze zelfstudie maakt via de stappen in [resources verwijderen](#delete-cli).

## <a name="powershell"></a>PowerShell

1. Installeer de meest recente versie van de PowerShell [AzureRm](https://www.powershellgallery.com/packages/AzureRM/)-module. Zie [Overzicht van Azure PowerShell](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json) als u nog geen ervaring hebt met Azure PowerShell.
2. In een PowerShell-sessie, moet u zich aanmelden bij Azure met uw [Azure-account](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account) met behulp van de `login-azurermaccount` opdracht.

3. Bekijk het volgende script en bijbehorende opmerkingen. Kopieer het script in uw browser en plak deze in uw PowerShell-sessie:

    ```powershell
    # Create a resource group.
    New-AzureRmResourceGroup `
      -Name myResourceGroup `
      -Location eastus
    
    # Create the public and private subnets.
    $Subnet1 = New-AzureRmVirtualNetworkSubnetConfig `
      -Name Public `
      -AddressPrefix 10.0.0.0/24
    $Subnet2 = New-AzureRmVirtualNetworkSubnetConfig `
      -Name Private `
      -AddressPrefix 10.0.1.0/24
    
    # Create a virtual network.
    $Vnet=New-AzureRmVirtualNetwork `
      -ResourceGroupName myResourceGroup `
      -Location eastus `
      -Name myVnet `
      -AddressPrefix 10.0.0.0/16 `
      -Subnet $Subnet1,$Subnet2
    ```

4. Kopieer de volgende opdracht om te controleren van de subnetten voor het virtuele netwerk, en plak deze in uw PowerShell-sessie:

    ```powershell
    $Vnet.subnets | Format-Table Name, AddressPrefix
    ```

5. **Optioneel:** Voltooi aanvullende zelfstudies die worden vermeld onder [Vervolgstappen](#next-steps) om netwerkverkeer te filteren en afmelden van elk subnet met netwerkbeveiligingsgroepen om verkeer te leiden tussen subnetten door middel van een virtueel netwerkapparaat , of het virtuele netwerk verbinding met andere virtuele netwerken of on-premises netwerken.
6. **Optionele**: verwijderen van de resources die u in deze zelfstudie maakt via de stappen in [resources verwijderen](#delete-powershell).

## <a name="resource-manager-template"></a>Resource Manager-sjabloon

U kunt een virtueel netwerk implementeren met behulp van een Azure Resource Manager-sjabloon. Zie voor meer informatie over sjablonen, [wat is er Resource Manager](../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#template-deployment). Zie voor toegang tot de sjabloon en voor meer informatie over de parameters de [een virtueel netwerk maken met twee subnetten](https://azure.microsoft.com/resources/templates/101-vnet-two-subnets/) sjabloon. U kunt de sjabloon implementeren met behulp van de [portal](#template-portal), [Azure CLI](#template-cli), of [PowerShell](#template-powershell).

Optionele stappen nadat u de sjabloon implementeert:

1. Voltooi aanvullende zelfstudies die worden vermeld onder [Vervolgstappen](#next-steps) voor het filteren van netwerkverkeer naar en vanuit elk subnet met netwerkbeveiligingsgroepen om verkeer te leiden tussen subnetten via een virtueel netwerkapparaat of de virtuele verbinding netwerk met andere virtuele netwerken of on-premises netwerken.
2. Verwijderen van de resources die u in deze zelfstudie maakt via de stappen in een subsecties van [resources verwijderen](#delete).

### <a name="template-portal"></a>Azure-portal

1. Open in uw browser de [sjabloonpagina](https://azure.microsoft.com/resources/templates/101-vnet-two-subnets).
2. Klik op de **implementeren in Azure** knop. Als u niet al bent aangemeld bij Azure, moet u zich aanmelden op het scherm Azure portal-aanmelding die wordt weergegeven.
3. Aanmelden bij de portal met behulp van uw [Azure-account](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account). Als u nog geen Azure-account hebt, kunt u zich registreren voor een [gratis proefversie](https://azure.microsoft.com/offers/ms-azr-0044p).
4. Voer de volgende waarden voor de parameters:

    |Parameter|Waarde|
    |---|---|
    |Abonnement|Selecteer uw abonnement|
    |Resourcegroep|myResourceGroup|
    |Locatie|Een locatie selecteren|
    |Vnet-naam|myVnet|
    |Vnet-adresvoorvoegsel|10.0.0.0/16|
    |Subnet1Prefix|10.0.0.0/24|
    |Subnet1Name|Openbaar|
    |Subnet2Prefix|10.0.1.0/24|
    |Subnet2Name|Privé|

5. Ga akkoord met de voorwaarden en bepalingen en klik vervolgens op **aankoop** voor het implementeren van het virtuele netwerk.

### <a name="template-cli"></a>Azure CLI

1. [Installeren en configureren van de Azure CLI](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Zorg ervoor dat u hebt de meest recente versie van de Azure CLI geïnstalleerd. Typ voor hulp bij CLI-opdrachten, `az <command> --help`. In plaats van de CLI en de vereisten installeert, kunt u de Azure-Cloud-Shell. De Azure Cloud Shell is een gratis Bash-shell die u rechtstreeks in Azure Portal kunt uitvoeren. De Shell Cloud heeft de Azure CLI vooraf is geïnstalleerd en geconfigureerd voor gebruik met uw account. Met de Cloud-Shell, klikt u op de Shell Cloud **> _** knop aan de bovenkant van de [portal](https://portal.azure.com), of klik op de **Try it** knop in de stappen volgen. 
2. Als de CLI lokaal uitgevoerd, meld u aan bij Azure met de `az login` opdracht. Als de Cloud-Shell, bent u al aangemeld.
3. Kopieer de volgende opdracht en plak deze in uw sessie CLI voor het maken van een resourcegroep voor het virtuele netwerk:

    ```azurecli-interactive
    az group create --name myResourceGroup --location eastus
    ```
    
4. U kunt de sjabloon implementeren met behulp van een van de volgende parameters opties:
    - **Standaard parameterwaarden**. Voer de volgende opdracht in:
    
        ```azurecli-interactive
        az group deployment create --resource-group myResourceGroup --name VnetTutorial --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vnet-two-subnets/azuredeploy.json`
        ```
    - **Aangepaste parameterwaarden**. Download en de sjabloon aanpassen voordat u de sjabloon implementeert. Ook kunt de sjabloon implementeren met behulp van de parameters op de opdrachtregel of de sjabloon met een afzonderlijke parameterbestand implementeren. De sjabloon en parameters om bestanden te downloaden, klikt u op de **bladeren op GitHub** knop op de [een virtueel netwerk maken met twee subnetten](https://azure.microsoft.com/resources/templates/101-vnet-two-subnets/) sjabloonpagina. Klik in GitHub op de **azuredeploy.parameters.json** of **azuredeploy.json** bestand. Klik vervolgens op de **Raw** knop om het bestand weer te geven. Kopieer de inhoud van het bestand in uw browser. De inhoud opslaan naar een bestand op uw computer. U kunt de parameterwaarden in de sjabloon wijzigt of de sjabloon met een afzonderlijke parameterbestand implementeren.  

    Typ voor meer informatie over het implementeren van sjablonen met behulp van deze methoden `az group deployment create --help`.

### <a name="template-powershell"></a>PowerShell

1. Installeer de meest recente versie van de PowerShell [AzureRm](https://www.powershellgallery.com/packages/AzureRM/)-module. Zie [Overzicht van Azure PowerShell](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json) als u nog geen ervaring hebt met Azure PowerShell.
2. In een PowerShell-sessie aan te melden met uw [Azure-account](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account), voer `login-azurermaccount`.
3. Voer de volgende opdracht voor het maken van een resourcegroep voor het virtuele netwerk:

    ```powershell
    New-AzureRmResourceGroup -Name myResourceGroup -Location eastus
    ```
    
4. U kunt de sjabloon implementeren met behulp van een van de volgende parameters opties:
    - **Standaard parameterwaarden**. Voer de volgende opdracht in:
    
        ```powershell
        New-AzureRmResourceGroupDeployment -Name VnetTutorial -ResourceGroupName myResourceGroup -TemplateUri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vnet-two-subnets/azuredeploy.json
        ```
        
    - **Aangepaste parameterwaarden**. Download en de sjabloon aanpassen voordat u deze implementeert. Ook kunt de sjabloon implementeren met behulp van de parameters op de opdrachtregel of de sjabloon met een afzonderlijke parameterbestand implementeren. De sjabloon en parameters om bestanden te downloaden, klikt u op de **bladeren op GitHub** knop op de [een virtueel netwerk maken met twee subnetten](https://azure.microsoft.com/resources/templates/101-vnet-two-subnets/) sjabloonpagina. Klik in GitHub op de **azuredeploy.parameters.json** of **azuredeploy.json** bestand. Klik vervolgens op de **Raw** knop om het bestand weer te geven. Kopieer de inhoud van het bestand in uw browser. De inhoud opslaan naar een bestand op uw computer. U kunt de parameterwaarden in de sjabloon wijzigt of de sjabloon met een afzonderlijke parameterbestand implementeren.  

    Typ voor meer informatie over het implementeren van sjablonen met behulp van deze methoden `Get-Help New-AzureRmResourceGroupDeployment`. 

## <a name="delete"></a>Resources verwijderen

Als u deze zelfstudie hebt voltooid, is het raadzaam de resources verwijderen die u hebt gemaakt, zodat u geen gebruik kosten. Verwijderen van een resourcegroep, verwijdert tevens alle bronnen die zich in de resourcegroep.

### <a name="delete-portal"></a>Azure-portal

1. Voer in het zoekvak portal **myResourceGroup**. Klik in de zoekresultaten op **myResourceGroup**.
2. Op de **myResourceGroup** blade, klikt u op de **verwijderen** pictogram.
3. Het verwijderen te bevestigen, in de **TYPE de naam van een RESOURCEGROEP** Voer **myResourceGroup**, en klik vervolgens op **verwijderen**.

### <a name="delete-cli"></a>Azure CLI

Voer de volgende opdracht in een sessie CLI:

```azurecli-interactive
az group delete --name myResourceGroup --yes
```

### <a name="delete-powershell"></a>PowerShell

Voer de volgende opdracht in een PowerShell-sessie:

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over alle virtueel netwerk en subnetinstellingen, [virtuele netwerken beheren](virtual-network-manage-network.md#view-vnet) en [beheren van virtueel netwerksubnetten](virtual-network-manage-subnet.md#create-subnet). U beschikt over verschillende opties voor het gebruik van virtuele netwerken en subnetten in een productieomgeving om te voldoen aan verschillende vereisten.
- Van binnenkomende en uitgaande subnetverkeer filteren op maken en toepassen van [netwerkbeveiligingsgroepen](virtual-networks-nsg.md) aan subnetten.
- Verkeer tussen subnetten door middel van een virtueel netwerkapparaat, door het maken van routeren [gebruiker gedefinieerde routes](virtual-network-create-udr-arm-ps.md) en de routes van toepassing op elk subnet.
- Maak een [Windows](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-network%2ftoc.json) of een [Linux](../virtual-machines/linux/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) virtuele machine in een bestaand virtueel netwerk.
- Verbinding maken met twee virtuele netwerken door het maken van een [virtueel netwerk peering](virtual-network-peering-overview.md) tussen de virtuele netwerken.
- Het virtuele netwerk verbinding met een on-premises netwerk via een [VPN-Gateway](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) of [Azure ExpressRoute](../expressroute/expressroute-howto-linkvnet-portal-resource-manager.md?toc=%2fazure%2fvirtual-network%2ftoc.json) circuit.
