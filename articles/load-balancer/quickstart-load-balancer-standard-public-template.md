---
title: 'Quickstart: Een Standard load balancer - Resource Manager-sjabloon maken'
titlesuffix: Azure Load Balancer
description: Deze quickstart laat zien hoe u een Standard load balancer maken met behulp van de Azure Resource Manager-sjabloon.
services: load-balancer
documentationcenter: na
author: KumudD
manager: twooley
Customer intent: I want to create a Standard load balancer by using an Azure Resource Manager template so that I can load balance internet traffic to VMs.
ms.service: load-balancer
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/19/2019
ms.author: kumud
ms.custom: mvc
ms.openlocfilehash: 00dbb29cac30f2a3bbecf71727c79617e24af3e0
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67441300"
---
# <a name="quickstart-create-a-standard-load-balancer-to-load-balance-vms-by-using-an-azure-resource-manager-template"></a>Quickstart: Een Standard load balancer om taken te verdelen VM's met behulp van een Azure Resource Manager-sjabloon maken

Taakverdeling zorgt voor een hogere beschikbaarheid en betere schaalbaarheid door binnenkomende aanvragen te spreiden over meerdere virtuele machines (VM's). Deze quickstart laat zien hoe u een Azure Resource Manager-sjabloon waarmee een standaardversie van load balancer om taken te verdelen VM's implementeren.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="create-a-standard-load-balancer"></a>Een standaardversie van een load balancer maken

Standard Load Balancer ondersteunt alleen een Standard openbaar IP-adres. Wanneer u een Standard load balancer maakt, moet u ook een nieuwe standaard, openbaar IP-adres dat is geconfigureerd als de front-end voor de standaardversie van load balancer maken.

U kunt veel methoden gebruiken om te maken van een Standard load balancer. In deze snelstartgids hebt u Azure PowerShell gebruiken om te implementeren een [Resource Manager-sjabloon](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-load-balancer-standard-create/azuredeploy.json). Resource Manager-sjablonen zijn JSON-bestanden die de resources definiëren die u voor uw oplossing moet implementeren.

Zie voor meer informatie over de concepten die zijn gekoppeld aan het implementeren en beheren van uw Azure-oplossingen, [Azure Resource Manager-documentatie](/azure/azure-resource-manager/). Meer sjablonen die zijn gerelateerd aan Azure Load Balancer, Zie [Azure-Snelstartsjablonen](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Network&pageNumber=1&sort=Popular).

1. Selecteer **uitproberen** uit het volgende codeblok Azure Cloud Shell openen en volg de instructies om aan te melden bij Azure.

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

1. Selecteer **kopie** uit het vorige blok van code te kopiëren van het PowerShell-script.

1. Met de rechtermuisknop op de shell-consolevenster en selecteer vervolgens **plakken**.

1. Geef de waarden op.

   De sjabloonimplementatie maakt drie beschikbaarheidszones. Beschikbaarheidszones worden alleen ondersteund in [bepaalde regio's](../availability-zones/az-overview.md). Gebruik een van de ondersteunde regio's. Als u niet zeker weet, voert u **centralus**.

   Naam van de resourcegroep met de naam van het project is **rg** toegevoegd. U moet de naam van de resourcegroep in de volgende sectie.

Het duurt ongeveer 10 minuten om de sjabloon te implementeren.

## <a name="test-the-load-balancer"></a>Load balancer testen

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

1. Selecteer **resourcegroepen** in het linkerdeelvenster.

1. Selecteer de resourcegroep die u in de vorige sectie hebt gemaakt. De standaardnaam van de resource-group is de naam van het project met **rg** toegevoegd.

1. Selecteer de load balancer. De standaardnaam is de naam van het project met **-lb** toegevoegd.

1. Kopieer alleen het IP-adres-gedeelte van het openbare IP-adres en plak deze in de adresbalk van uw browser. De browser weergegeven de standaardpagina van de webserver van Internet Information Services (IIS).

   ![IIS-webserver](./media/tutorial-load-balancer-standard-zonal-portal/load-balancer-test.png)

Als u wilt zien hoe de load balancer verkeer distribueert naar alle drie virtuele machines, kunt u een vernieuwing van uw webbrowser dwingen de clientcomputer.

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u deze niet meer nodig, verwijdert u de resourcegroep, de load balancer en alle gerelateerde resources. Om dit te doen, gaat u naar de Azure-portal, selecteert u de resourcegroep met de load balancer en selecteer vervolgens **resourcegroep verwijderen**.

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids hebt u een Standard load balancer gemaakt, virtuele machines die zijn gekoppeld aan deze regel voor het load balancer-verkeer geconfigureerd, heeft een statustest en vervolgens de load balancer getest.

Voor meer informatie gaat u verder met de zelfstudies voor Load Balancer.

> [!div class="nextstepaction"]
> [Zelfstudies voor Azure Load Balancer](tutorial-load-balancer-standard-public-zone-redundant-portal.md)
 