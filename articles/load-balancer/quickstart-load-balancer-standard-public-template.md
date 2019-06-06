---
title: 'Quickstart: Een Standard Load Balancer - Resource Manager-sjabloon maken'
titlesuffix: Azure Load Balancer
description: Deze quickstart laat zien hoe u een Standard Load Balancer maakt met behulp van de Azure Resource Manager-sjabloon.
services: load-balancer
documentationcenter: na
author: KumudD
manager: twooley
Customer intent: I want to create a Standard Load Balancer by using Azure Resource Manager template so that I can load balance internet traffic to VMs.
ms.service: load-balancer
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/30/2019
ms.author: kumud
ms.custom: mvc
ms.openlocfilehash: 0e47560ca43b23f4779da701f3e6f11f53a6b1ce
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2019
ms.locfileid: "66480401"
---
# <a name="quickstart-create-a-standard-load-balancer-to-load-balance-vms-by-using-azure-resource-manager-template"></a>Quickstart: Een Standard Load Balancer om taken te verdelen VM's met behulp van Azure Resource Manager-sjabloon maken

Taakverdeling zorgt voor een hogere beschikbaarheid en betere schaalbaarheid door binnenkomende aanvragen te spreiden over meerdere virtuele machines. U kunt een Azure Resource Manager-sjabloon voor het maken van een load balancer om taken te verdelen over virtuele machines (VM's) kunt implementeren. In deze snelstart wordt getoond hoe u taken van VM's kunt verdelen met een Standard Load Balancer.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="create-a-standard-load-balancer"></a>Een standaardversie van een load balancer maken

In deze sectie maakt u een Standard Load Balancer die helpt bij het laden van verdelen over virtuele machines. Standard Load Balancer biedt alleen ondersteuning voor een standaard, openbaar IP-adres. Als u een Standard Load Balancer maakt, moet u ook een nieuw, standaard, openbaar IP-adres maken dat als de front-end (standaard *LoadBalancerFrontend* genoemd) wordt geconfigureerd voor de Standard Load Balancer. Er zijn veel methoden die kunnen worden gebruikt voor het maken van een standard load balancer. In deze snelstartgids hebt u Azure PowerShell gebruiken om te implementeren een [Resource Manager-sjabloon](https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/101-load-balancer-standard-create/azuredeploy.json). Resource Manager-sjablonen zijn JSON-bestanden die de resources definiëren die u voor uw oplossing moet implementeren. Zie voor meer informatie over de concepten die zijn gekoppeld aan het implementeren en beheren van uw Azure-oplossingen, [Azure Resource Manager-documentatie](/azure/azure-resource-manager/). Als u meer gerelateerde Azure Load Balancer-sjabloon, bezoekt [Azure-Snelstartsjablonen](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Network&pageNumber=1&sort=Popular).

Als u wilt implementeren de sjabloon, selecteert u **uitproberen** Azure Cloud shell openen en plak het volgende PowerShell-script in de shell-venster. Als u de code, met de rechtermuisknop op de shell-venster en selecteer vervolgens **plakken**.

```azurepowershell-interactive
$projectName = Read-Host -Prompt "Enter a project name with 12 or less letters or numbers that is used to generate Azure resource names"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"
$adminUserName = Read-Host -Prompt "Enter the virtual machine administrator account name"
$adminPassword = Read-Host -Prompt "Enter the virtual machine administrator password" -AsSecureString

$resourceGroupName = "${projectName}rg"
$templateUri = "https://raw.githubusercontent.com/Azure/azure-docs-json-samples/master/101-load-balancer-standard-create/azuredeploy.json"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri $templateUri -projectName $projectName -location $location -adminUsername $adminUsername -adminPassword $adminPassword
```

U ziet dat de naam van de resource is de naam van het project met **rg** toegevoegd. U moet de naam van de resourcegroep in de volgende sectie.  Het duurt een paar minuten om de resources te maken.

## <a name="test-the-load-balancer"></a>Test de Load Balancer

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Selecteer **resourcegroepen** in het linkerdeelvenster.
1. Selecteer de resourcegroep die u in de laatste sectie hebt gemaakt.  De standaardnaam van de resource-group is de naam van het project met **rg** toegevoegd.
1. Selecteer de load balancer.  Er is slechts één load balancer. De standaardnaam is de naam van het project met **-lb** toegevoegd.
1. Kopieer de openbare IP-adres (alleen het IP-adres deel) en plak deze in de adresbalk van uw browser. De standaardpagina van IIS-webserver wordt weergegeven in de browser.

   ![IIS-webserver](./media/tutorial-load-balancer-standard-zonal-portal/load-balancer-test.png)

Als u wilt zien hoe de Load Balancer verkeer distribueert naar alle drie virtuele machines, u kunt forceren vernieuwing van uw webbrowser de clientcomputer.

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u niet meer nodig hebt, verwijdert u de resourcegroep, Load Balancer en alle gerelateerde resources. Om dit te doen, selecteert u de resourcegroep met de Load Balancer via Azure portal en selecteer vervolgens **resourcegroep verwijderen**.

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids hebt u een Standard Load Balancer gemaakt, VM's die zijn gekoppeld aan het statustest van de Load Balancer verkeersregel geconfigureerd en vervolgens de Load Balancer getest. Voor meer informatie over Azure Load Balancer gaat u verder met de zelfstudies voor Azure Load Balancer.

> [!div class="nextstepaction"]
> [Zelfstudies voor Azure Load Balancer](tutorial-load-balancer-standard-public-zone-redundant-portal.md)
