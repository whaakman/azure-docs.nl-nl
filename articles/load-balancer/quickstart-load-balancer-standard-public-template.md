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
ms.date: 06/19/2019
ms.author: kumud
ms.custom: mvc
ms.openlocfilehash: a95bedc8b2b395f856512ec49bae630666fe36da
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67272883"
---
# <a name="quickstart-create-a-standard-load-balancer-to-load-balance-vms-by-using-azure-resource-manager-template"></a>Quickstart: Een Standard Load Balancer om taken te verdelen VM's met behulp van Azure Resource Manager-sjabloon maken

Taakverdeling zorgt voor een hogere beschikbaarheid en betere schaalbaarheid door binnenkomende aanvragen te spreiden over meerdere virtuele machines. Deze quickstart laat zien hoe u een Azure Resource Manager-sjabloon voor het maken van een standard load balancer om taken te verdelen over virtuele machines (VM's) implementeert.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="create-a-standard-load-balancer"></a>Een standaardversie van een load balancer maken

Standard Load Balancer biedt alleen ondersteuning voor een standaard, openbaar IP-adres. Wanneer u een Standard Load Balancer maakt, moet u ook een nieuwe standaard, openbaar IP-adres dat is geconfigureerd als de front-end voor de Standard Load Balancer maken. Er zijn veel methoden die kunnen worden gebruikt voor het maken van een standard load balancer. In deze snelstartgids hebt u Azure PowerShell gebruiken om te implementeren een [Resource Manager-sjabloon](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-load-balancer-standard-create/azuredeploy.json). Resource Manager-sjablonen zijn JSON-bestanden die de resources definiëren die u voor uw oplossing moet implementeren. Zie voor meer informatie over de concepten die zijn gekoppeld aan het implementeren en beheren van uw Azure-oplossingen, [Azure Resource Manager-documentatie](/azure/azure-resource-manager/). Als u meer gerelateerde Azure Load Balancer-sjabloon, bezoekt [Azure-Snelstartsjablonen](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Network&pageNumber=1&sort=Popular).

1. Selecteer **uitproberen** uit het volgende codeblok op de Azure Cloud shell openen en volg de instructies om aan te melden bij Azure.

   ```azurepowershell-interactive
   $projectName = Read-Host -Prompt "Enter a project name with 12 or less letters or numbers that is used to generate Azure resource names"
   $location = Read-Host -Prompt "Enter the location (i.e. centralus)"
   $adminUserName = Read-Host -Prompt "Enter the virtual machine administrator account name"
   $adminPassword = Read-Host -Prompt "Enter the virtual machine administrator password" -AsSecureString

   $resourceGroupName = "${projectName}rg"
   $templateUri = "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-load-balancer-standard-create/azuredeploy.json"

   New-AzResourceGroup -Name $resourceGroupName -Location $location
   New-AzResourceGroupDeployment -ResourceGroupName $resourceGroupName -TemplateUri $templateUri -projectName $projectName -location $location -adminUsername $adminUsername -adminPassword $adminPassword

   Write-Host "Press [ENTER] to continue."
   ```

   Wacht tot u de opdrachtprompt vanuit de console.
2. Selecteer **kopie** uit het vorige blok van code te kopiëren van het PowerShell-script.
3. Met de rechtermuisknop op de shell-consolevenster en selecteer vervolgens **plakken**.
4. Geef de waarden op.

   De sjabloonimplementatie maakt drie beschikbaarheidszones.  Beschikbaarheidszones worden alleen ondersteund in [bepaalde regio's](../availability-zones/az-overview.md). Gebruik een van de ondersteunde regio's. Als u niet zeker weet, voert u **centralus**.

   Naam van de resourcegroep met de naam van het project is **rg** toegevoegd. U moet de naam van de resourcegroep in de volgende sectie.

Het duurt ongeveer 10 minuten om de sjabloon te implementeren.

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
