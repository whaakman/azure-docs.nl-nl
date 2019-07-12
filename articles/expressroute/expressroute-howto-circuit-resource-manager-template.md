---
title: 'Maak een ExpressRoute-circuit - Resource Manager-sjabloon: Azure | Microsoft Docs'
description: Maken, inrichten, verwijderen en de inrichting van een ExpressRoute-circuit ongedaan maken.
services: expressroute;azure-resource-manager
author: cherylmc
ms.service: expressroute
ms.topic: article
ms.date: 07/05/2019
ms.author: cherylmc;ganesr
ms.openlocfilehash: bf56145d0a8cd3b01d0d74fcaf3348c1916cee5a
ms.sourcegitcommit: cf438e4b4e351b64fd0320bf17cc02489e61406a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/08/2019
ms.locfileid: "67659682"
---
# <a name="create-an-expressroute-circuit-by-using-azure-resource-manager-template"></a>Een ExpressRoute-circuit met behulp van Azure Resource Manager-sjabloon maken

> [!div class="op_single_selector"]
> * [Azure-portal](expressroute-howto-circuit-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-circuit-arm.md)
> * [Azure-CLI](howto-circuit-cli.md)
> * [Azure Resource Manager-sjabloon](expressroute-howto-circuit-resource-manager-template.md)
> * [Video - Azure portal](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit)
> * [PowerShell (klassiek)](expressroute-howto-circuit-classic.md)
>

Meer informatie over het maken van een ExpressRoute-circuit met een Azure Resource Manager-sjabloon implementeren met behulp van Azure PowerShell. Zie voor meer informatie over het ontwikkelen van Resource Manager-sjablonen [Resource Manager-documentatie](/azure/azure-resource-manager/) en de [sjabloonverwijzing](/azure/templates/microsoft.network/expressroutecircuits).

## <a name="before-you-begin"></a>Voordat u begint

* Controleer de [vereisten](expressroute-prerequisites.md) en [werkstromen](expressroute-workflows.md) voordat u begint met de configuratie.
* Zorg ervoor dat u gemachtigd bent om te maken van nieuwe netwerkresources. Neem contact op met uw accountbeheerder als u nog niet de juiste machtigingen hebt.
* U kunt [een video bekijken](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit) voordat u begint om beter te begrijpen van de stappen.

## <a name="create"></a>Maken en inrichten van een ExpressRoute-circuit

[Azure Quickstart-sjablonen](https://azure.microsoft.com/resources/templates/) heeft een goede verzameling van Resource Manager-sjabloon. U gebruikt een van de [bestaande sjablonen](https://azure.microsoft.com/resources/templates/101-expressroute-circuit-create/) te maken van een ExpressRoute-circuit.

[!code-json[create-azure-expressroute-circuit](~/quickstart-templates/101-expressroute-circuit-create/azuredeploy.json)]

Meer sjablonen gerelateerde Selecteer [hier](https://azure.microsoft.com/resources/templates/?term=expressroute).

Een ExpressRoute-Circuit maken door het implementeren van een sjabloon:

1. Selecteer **uitproberen** uit het volgende codeblok en volg de instructies om aan te melden bij de Azure Cloud shell.

    ```azurepowershell-interactive
    $circuitName = Read-Host -Prompt "Enter a circuit name"
    $location = Read-Host -Prompt "Enter the location (i.e. centralus)"
    $resourceGroupName = "${circuitName}rg"
    $templateUri = "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-expressroute-circuit-create/azuredeploy.json"

    $serviceProviderName = "Equinix"
    $peeringLocation = "Silicon Valley"
    $bandwidthInMbps = 500
    $sku_tier = "Premium"
    $sku_family = "MeteredData"

    New-AzResourceGroup -Name $resourceGroupName -Location $location
    New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri $templateUri -circuitName $circuitName -serviceProviderName $serviceProviderName -peeringLocation $peeringLocation -bandwidthInMbps $bandwidthInMbps -sku_tier $sku_tier -sku_family $sku_family

    Write-Host "Press [ENTER] to continue ..."
    ```

   * **Laag** bepaalt of een ExpressRoute-standard- of een premium-invoegtoepassing voor ExpressRoute is ingeschakeld. U kunt opgeven **Standard** om op te halen van de standaard-SKU of **Premium** voor de premium-invoegtoepassing.

   * **Peeringlocatie** is de fysieke locatie waar u peering met Microsoft.

     > [!IMPORTANT]
     > De Peeringlocatie geeft aan dat de [fysieke locatie](expressroute-locations.md) waar u peering met Microsoft. Dit is **niet** gekoppeld aan "Locatie" eigenschap, die verwijst naar de geografische locatie waar de Resourceprovider van de Azure-netwerk zich bevindt. Hoewel ze niet zijn gerelateerd, is het raadzaam een Netwerkresourceprovider geografisch dicht bij de locatie-Peering van het circuit kiezen.

    Naam van de resourcegroep is de naam van service bus-naamruimte met **rg** toegevoegd.

2. Selecteer **Kopiëren** om het PowerShell-script te kopiëren.
3. Met de rechtermuisknop op de shell-console en selecteer vervolgens **plakken**.

Het duurt een paar minuten naar een event hub maken.

Azure PowerShell wordt gebruikt voor het implementeren van de sjabloon in deze zelfstudie. Zie voor andere methoden voor het implementeren van sjabloon:

* [Met behulp van de Azure-portal](../azure-resource-manager/resource-group-template-deploy-portal.md).
* [Met behulp van Azure CLI](../azure-resource-manager/resource-group-template-deploy-cli.md).
* [Met behulp van REST-API](../azure-resource-manager/resource-group-template-deploy-rest.md).

## <a name="delete"></a>Opheffen van inrichting en het verwijderen van een ExpressRoute-circuit

U kunt uw ExpressRoute-circuit verwijderen door het selecteren van de **verwijderen** pictogram. Let op de volgende informatie:

* U moet alle virtuele netwerken loskoppelen van het ExpressRoute-circuit. Als deze bewerking is mislukt, controleert u of alle virtuele netwerken zijn gekoppeld aan het circuit.
* Als het ExpressRoute-circuit serviceprovider-Inrichtingsstatus **Provisioning** of **ingerichte** moet u werken met uw serviceprovider inrichting ongedaan maken van het circuit aan hun kant. We blijven reserveer resources en factureren u totdat de service-provider is voltooid ongedaan maken van inrichting van het circuit en ons wordt geïnformeerd.
* Als de service-provider de inrichting van het circuit is beëindigd (de serviceprovider Inrichtingsstatus is ingesteld op **niet ingericht**), kunt u het circuit verwijderen. Hiermee wordt ook de facturering voor het circuit gestopt.

U kunt uw ExpressRoute-circuit verwijderen door het uitvoeren van de volgende PowerShell-opdracht:

```azurepowershell-interactive
$circuitName = Read-Host -Prompt "Enter the same circuit name that you used earlier"
$resourceGroupName = "${circuitName}rg"

Remove-AzExpressRouteCircuit -ResourceGroupName $resourceGroupName -Name $circuitName
```

## <a name="next-steps"></a>Volgende stappen

Nadat u uw circuit hebt gemaakt, kunt u doorgaan met de volgende stappen uitvoeren:

* [Routering voor uw ExpressRoute-circuit maken en wijzigen](expressroute-howto-routing-portal-resource-manager.md)
* [Uw virtueel netwerk koppelen aan uw ExpressRoute-circuit](expressroute-howto-linkvnet-arm.md)
