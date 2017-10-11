---
title: Een virtueel netwerk maken | Azure Resource Manager-sjabloon | Microsoft Docs
description: Informatie over het maken van een virtueel netwerk met een Azure Resource Manager-sjabloon.
services: virtual-network
documentationcenter: 
author: jimdial
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 69530861-2f97-4a6e-b336-a7baf2690044
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 81602766848a91331c8d811ea1c8ec3ffae44b96
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-virtual-network-using-an-azure-resource-manager-template"></a>Een virtueel netwerk met een Azure Resource Manager-sjabloon maken

[!INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnet-intro-include.md)]

Azure heeft twee implementatiemodellen: Azure Resource Manager en klassiek. Microsoft raadt aan resources te maken via het Resource Manager-implementatiemodel. Lees het artikel [Azure-implementatiemodellen begrijpen](../azure-resource-manager/resource-manager-deployment-model.md) voor meer informatie over de verschillen tussen de twee modellen.
 
Dit artikel wordt uitgelegd hoe u een VNet met het implementatiemodel van Resource Manager met een Azure Resource Manager-sjabloon maakt. U kunt via Resource Manager ook een VNet maken met andere hulpprogramma's. Bovendien kunt u een VNet maken via het klassieke implementatiemodel door in de volgende lijst een andere optie te selecteren:

> [!div class="op_single_selector"]
- [Portal](virtual-networks-create-vnet-arm-pportal.md)
- [PowerShell](virtual-networks-create-vnet-arm-ps.md)
- [CLI](virtual-networks-create-vnet-arm-cli.md)
- [Sjabloon](virtual-networks-create-vnet-arm-template-click.md)
- [Portal (klassiek)](virtual-networks-create-vnet-classic-pportal.md)
- [PowerShell (klassiek)](virtual-networks-create-vnet-classic-netcfg-ps.md)
- [CLI (klassiek)](virtual-networks-create-vnet-classic-cli.md)

U ziet hoe u een bestaande ARM-sjabloon kunt downloaden en wijzigen vanuit GitHub, en de sjabloon vervolgens kunt implementeren met GitHub, PowerShell en Azure CLI.

Als u de ARM-sjabloon rechtstreeks vanuit GitHub wilt implementeren zonder deze te wijzigen, gaat u naar [Een sjabloon implementeren vanuit GitHub](#deploy-the-arm-template-by-using-click-to-deploy).

[!INCLUDE [virtual-networks-create-vnet-scenario-include](../../includes/virtual-networks-create-vnet-scenario-include.md)]

## <a name="download-and-understand-the-azure-resource-manager-template"></a>De Azure Resource Manager-sjabloon downloaden en begrijpen
U kunt de bestaande sjabloon voor het maken van een VNet en twee subnets vanuit GitHub downloaden, breng eventuele wijzigingen die u mogelijk wilt gebruiken en het opnieuw gebruiken. Voer de volgende stappen uit om dit te doen:

1. Navigeer naar [de sjabloon-voorbeeldpagina](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vnet-two-subnets).
2. Klik op **azuredeploy.json** en vervolgens op **RAW**.
3. Sla het bestand op in een lokale map op uw computer.
4. Als u bekend met sjablonen bent, gaat u verder met stap 7.
5. Open het bestand dat u zojuist hebt opgeslagen en bekijk de inhoud onder **parameters** in regel 5. ARM-sjabloonparameters bieden een tijdelijke aanduiding voor waarden die kunnen worden ingevuld tijdens de implementatie.
   
   | Parameter | Beschrijving |
   | --- | --- |
   | **location** |Azure-regio waar het VNet wordt aangemaakt |
   | **vnetName** |Naam voor het nieuwe VNet |
   | **addressPrefix** |Adresruimte voor het VNet, in CIDR-indeling |
   | **subnet1Name** |Naam voor het eerste VNet |
   | **subnet1Prefix** |CIDR-blokkering voor het eerste subnet |
   | **subnet2Name** |Naam voor het tweede VNet |
   | **subnet2Prefix** |CIDR-blokkering voor het tweede subnet |
   
   > [!IMPORTANT]
   > Azure Resource Manager-sjablonen die in GitHub worden bewaard, kunnen in de loop van de tijd veranderen. Zorg ervoor dat u de sjabloon controleert, voordat u deze gebruikt.
   > 
   > 
6. Controleer de inhoud onder **Resources** en let op het volgende:
   
   * **type**. Het type resource dat door de sjabloon wordt aangemaakt. In dit geval **Microsoft.Network/virtualNetworks**, die een VNet vertegenwoordigen.
   * **Naam**. Naam voor de resource. Let op het gebruik van **[parameters('vnetName')]**. Deze geeft aan of de naam wordt geleverd als invoer door de gebruiker of een parameterbestand tijdens implementatie.
   * **Eigenschappen**. Lijst met eigenschappen voor de resource. Deze sjabloon maakt gebruik van de adresruimte en de subneteigenschappen tijdens het aanmaken van het VNet.
7. Ga naar de [pagina Voorbeeldsjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vnet-two-subnets).
8. Klik op **azuredeploy-paremeters.json** en vervolgens op **RAW**.
9. Sla het bestand op in een lokale map op uw computer.
10. Open het bestand dat u zojuist hebt opgeslagen en bewerk de waarden voor de parameters. Gebruik de volgende waarden op om het VNet dat is beschreven in het scenario te implementeren:

    ```json
        {
          "location": {
            "value": "Central US"
          },
          "vnetName": {
              "value": "TestVNet"
          },
          "addressPrefix": {
              "value": "192.168.0.0/16"
          },
          "subnet1Name": {
              "value": "FrontEnd"
          },
          "subnet1Prefix": {
            "value": "192.168.1.0/24"
          },
          "subnet2Name": {
              "value": "BackEnd"
          },
          "subnet2Prefix": {
              "value": "192.168.2.0/24"
          }
        }
    ```

11. Sla het bestand op.


## <a name="deploy-the-template-using-powershell"></a>De sjabloon die met behulp van PowerShell implementeren

De volgende stappen voor het implementeren van de sjabloon die u hebt gedownload met behulp van PowerShell:

1. Installeren en configureren van Azure PowerShell via de stappen in de [installeren en configureren van Azure PowerShell](/powershell/azure/overview) artikel.
2. Voer de volgende opdracht uit om een nieuwe resourcegroep te maken:

    ```powershell
    New-AzureRmResourceGroup -Name TestRG -Location centralus
    ```

    De opdracht maakt u een resourcegroep met de naam *TestRG* in de *VS-midden* azure-regio. Zie [Azure Resource Manager Overview](../azure-resource-manager/resource-group-overview.md) (Overzicht van Azure Resource Manager) voor meer informatie over resourcegroepen.

    Verwachte uitvoer:

        ResourceGroupName : TestRG
        Location          : centralus
        ProvisioningState : Succeeded
        Tags              :
        Permissions       :
                            Actions  NotActions
                            =======  ==========
                            *
        ResourceId        : /subscriptions/[Id]/resourceGroups/TestRG

3. Voer de volgende opdracht voor het implementeren van de nieuwe VNet met behulp van de sjabloon en de parameterbestanden bestanden u hebt gedownload en hierboven zijn gewijzigd:

    ```powershell
    New-AzureRmResourceGroupDeployment -Name TestVNetDeployment -ResourceGroupName TestRG `
    -TemplateFile C:\ARM\azuredeploy.json -TemplateParameterFile C:\ARM\azuredeploy-parameters.json
    ```

    Verwachte uitvoer:
   
        DeploymentName    : TestVNetDeployment
        ResourceGroupName : TestRG
        ProvisioningState : Succeeded
        Timestamp         : [Date and time]
        Mode              : Incremental
        TemplateLink      :
        Parameters        :
                            Name             Type                       Value
                            ===============  =========================  ==========
                            location         String                     Central US
                            vnetName         String                     TestVNet
                            addressPrefix    String                     192.168.0.0/16
                            subnet1Prefix    String                     192.168.1.0/24
                            subnet1Name      String                     FrontEnd
                            subnet2Prefix    String                     192.168.2.0/24
                            subnet2Name      String                     BackEnd
   
        Outputs           :
4. Voer de volgende opdracht om de eigenschappen van de nieuwe VNet te bekijken:

    ```powershell
    Get-AzureRmVirtualNetwork -ResourceGroupName TestRG -Name TestVNet
    ```

    Verwachte uitvoer:

        Name              : TestVNet
        ResourceGroupName : TestRG
        Location          : centralus
        Id                : /subscriptions/[Id]/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet
        Etag              : W/"[Id]"
        ProvisioningState : Succeeded
        Tags              :
        AddressSpace      : {
                              "AddressPrefixes": [
                                "192.168.0.0/16"
                              ]
                            }
        DhcpOptions       : {
                              "DnsServers": null
                            }
        NetworkInterfaces : null
        Subnets           : [
                              {
                                "Name": "FrontEnd",
                                "Etag": "W/\"[Id]\"",
                                "Id": "/subscriptions/[Id]/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/FrontEnd",
                                "AddressPrefix": "192.168.1.0/24",
                                "IpConfigurations": [],
                                "NetworkSecurityGroup": null,
                                "RouteTable": null,
                                "ProvisioningState": "Succeeded"
                              },
                              {
                                "Name": "BackEnd",
                                "Etag": "W/\"[Id]\"",
                                "Id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/TestRG/providers/Microsoft.Network/virtualNetworks/TestVNet/subnets/BackEnd",
                                "AddressPrefix": "192.168.2.0/24",
                                "IpConfigurations": [],
                                "NetworkSecurityGroup": null,
                                "RouteTable": null,
                                "ProvisioningState": "Succeeded"
                              }
                            ]

## <a name="deploy-the-template-using-click-to-deploy"></a>De sjabloon met click-to-deploy implementeren

U kunt hergebruiken van vooraf gedefinieerde Azure Resource Manager-sjablonen geüpload naar een door Microsoft beheerde GitHub-opslagplaats en toegankelijk voor de community. Deze sjablonen kunnen rechtstreeks uit de GitHub geïmplementeerd of gedownload en gewijzigd naar wens aanpassen. Voor het implementeren van een sjabloon die een VNet met twee subnetten maakt, moet u de volgende stappen uitvoeren:

1. Vanuit een browser gaat u naar [https://github.com/Azure/azure-quickstart-templates](https://github.com/Azure/azure-quickstart-templates).
2. Scroll omlaag in de lijst met sjablonen en klik op **101-vnet-two-subnets**. Controleer het **README.md** -bestand, zoals hieronder weergegeven.

    ![READEME.md-bestand in github](./media/virtual-networks-create-vnet-arm-template-click-include/figure1.png)

3. Klik op **Implementeren in Azure**. Voer indien nodig uw Azure-aanmeldingsreferenties in. 
4. Voer de waarden die u wilt gebruiken om uw nieuwe VNet te maken in de **Parameters**-blade en klik vervolgens op **OK**. De volgende afbeelding ziet de waarden voor het scenario:
   
    ![ARM-sjabloonparameters](./media/virtual-networks-create-vnet-arm-template-click-include/figure2.png)

5. Klik op **Resourcegroep** en selecteer een resourcegroep om toe te voegen aan het VNet of klik op **Nieuwe aanmaken** om het VNet toe te voegen aan een nieuwe resourcegroep. De volgende afbeelding ziet u de resource groepsinstellingen voor een nieuwe resourcegroep aangeroepen **TestRG**:

    ![Resourcegroep](./media/virtual-networks-create-vnet-arm-template-click-include/figure3.png)

6. Wijzig zo nodig de instellingen van het **Abonnement** en de **Locatie** voor uw VNet.
7. Als u VNet niet als tegel wilt laten weergeven in het **Startboard** schakelt u **Vastmaken aan Startboard** uit.
8. Klik op **juridische voorwaarden**, lees de voorwaarden en klik op **kopen** accepteren. 
9. Klik op **Maken** om het VNet te maken.
   
    ![Tegel implementatie in Preview Portal verzenden](./media/virtual-networks-create-vnet-arm-template-click-include/figure4.png)

10. Zodra de implementatie is voltooid in de Azure portal op **meer services**, type *virtuele netwerken* in het vak van het filter dat wordt weergegeven, klik vervolgens op virtuele netwerken om te zien van de virtuele netwerken-blade. Klik op de blade *TestVNet*. In de *TestVNet* blade, klikt u op **subnetten** om te zien van de subnetten gemaakt, zoals wordt weergegeven in de volgende afbeelding:
    
     ![VNet maken in de Preview Portal](./media/virtual-networks-create-vnet-arm-template-click-include/figure5.png)

## <a name="next-steps"></a>Volgende stappen

Leer hoe u de volgende verbindingen maakt:

- Verbinding van een virtuele machine (VM) met een virtueel netwerk: lees de artikelen [Een Windows VM maken](../virtual-machines/virtual-machines-windows-hero-tutorial.md) of [Een Linux-VM maken](../virtual-machines/linux/quick-create-portal.md). In plaats van een VNet en subnet te maken via de stappen die in de artikelen worden beschreven, kunt u ook een bestaand VNet en subnet selecteren waarmee u een VM wilt verbinden.
- Verbinding van het virtuele netwerk met andere virtuele netwerken: lees het artikel [VNets verbinden](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md).
- Verbinding van het virtuele netwerk met een on-premises netwerk via een site-naar-site virtueel particulier netwerk (VPN) of een ExpressRoute-circuit. Lees hiervoor de artikelen [Een VNet verbinden met een on-premises netwerk via een site-naar-site VPN](../vpn-gateway/vpn-gateway-howto-multi-site-to-site-resource-manager-portal.md) en [Een VNet koppelen aan een ExpressRoute-circuit](../expressroute/expressroute-howto-linkvnet-arm.md).
