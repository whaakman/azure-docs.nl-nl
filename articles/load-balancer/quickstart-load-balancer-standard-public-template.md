---
title: 'Quickstart: Een standaard sjabloon voor load balancer-Azure Resource Manager maken'
titlesuffix: Azure Load Balancer
description: In deze Quick start ziet u hoe u een standaard load balancer maakt met behulp van de Azure Resource Manager sjabloon.
services: load-balancer
documentationcenter: na
author: asudbring
manager: twooley
Customer intent: I want to create a Standard load balancer by using an Azure Resource Manager template so that I can load balance internet traffic to VMs.
ms.service: load-balancer
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/19/2019
ms.author: allensu
ms.custom: mvc
ms.openlocfilehash: e1a04cad7fe5c9c95397ffad2faa80c7d657d0f8
ms.sourcegitcommit: 9a699d7408023d3736961745c753ca3cec708f23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/16/2019
ms.locfileid: "68273797"
---
# <a name="quickstart-create-a-standard-load-balancer-to-load-balance-vms-by-using-an-azure-resource-manager-template"></a>Quickstart: Een standaard load balancer maken om taken van Vm's te verdelen met behulp van een Azure Resource Manager sjabloon

Taakverdeling zorgt voor een hogere beschikbaarheid en betere schaalbaarheid door binnenkomende aanvragen te spreiden over meerdere virtuele machines (VM's). In deze Quick start ziet u hoe u een Azure Resource Manager sjabloon kunt implementeren waarmee een standaard-load balancer wordt gemaakt voor de taak verdeling van Vm's.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="create-a-standard-load-balancer"></a>Een standaardversie van een load balancer maken

Standard Load Balancer ondersteunt alleen een standaard openbaar IP-adres. Wanneer u een standaard load balancer maakt, moet u ook een nieuw standaard openbaar IP-adres maken dat is geconfigureerd als de front-end voor de standaard load balancer.

U kunt een groot aantal methoden gebruiken om een standaard load balancer te maken. In deze Quick Start gebruikt u Azure PowerShell voor het implementeren van een [Resource Manager-sjabloon](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-load-balancer-standard-create/azuredeploy.json). Resource Manager-sjablonen zijn JSON-bestanden die de resources definiëren die u voor uw oplossing moet implementeren.

Zie [Azure Resource Manager documentatie](/azure/azure-resource-manager/)voor meer informatie over de concepten van het implementeren en beheren van uw Azure-oplossingen. Zie [Azure Quick](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Network&pageNumber=1&sort=Popular)start-sjablonen voor meer informatie over de sjablonen die zijn gerelateerd aan Azure Load Balancer.

1. Selecteer **deze** in het volgende code blok proberen om Azure Cloud shell te openen en volg de instructies om u aan te melden bij Azure.

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

   Wacht totdat u de prompt van de-console ziet.

1. Selecteer **kopiëren** in het vorige code blok om het Power shell-script te kopiëren.

1. Klik met de rechter muisknop op het deel venster shell console en selecteer vervolgens **Plakken**.

1. Voer de waarden in.

   Met de sjabloon implementatie worden drie beschikbaarheids zones gemaakt. Beschikbaarheids zones worden alleen ondersteund in [bepaalde regio's](../availability-zones/az-overview.md). Gebruik een van de ondersteunde regio's. Als u dat niet zeker weet , voert u centralus in.

   De naam van de resource groep is de naam van het project waaraan **RG** is toegevoegd. U hebt de naam van de resource groep nodig in de volgende sectie.

Het duurt ongeveer 10 minuten om de sjabloon te implementeren.

## <a name="test-the-load-balancer"></a>Load balancer testen

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

1. Selecteer **resource groepen** in het linkerdeel venster.

1. Selecteer de resource groep die u in de vorige sectie hebt gemaakt. De standaard naam van de resource groep is de naam van het project waaraan **RG** is toegevoegd.

1. Selecteer de load balancer. De standaard naam is de project naam met **-lb** toevoegen.

1. Kopieer alleen het IP-adres gedeelte van het open bare IP-adres en plak het in de adres balk van uw browser. In de browser wordt de standaard pagina van de Internet Information Services-webserver (IIS) weer gegeven.

   ![IIS-webserver](./media/tutorial-load-balancer-standard-zonal-portal/load-balancer-test.png)

Als u de load balancer distributie van verkeer over alle drie Vm's wilt zien, kunt u het vernieuwen van uw webbrowser afdwingen van de client computer.

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u deze niet meer nodig hebt, verwijdert u de resource groep, de load balancer en alle gerelateerde resources. Als u dit wilt doen, gaat u naar de Azure Portal, selecteert u de resource groep die de load balancer bevat en selecteert u **resource groep verwijderen**.

## <a name="next-steps"></a>Volgende stappen

In deze Quick Start hebt u een standaard load balancer, gekoppelde virtuele machines, gemaakt, de regel voor de Load Balancer-verkeer geconfigureerd, een status test en vervolgens de load balancer getest.

Ga verder met de zelf studies voor Load Balancer voor meer informatie.

> [!div class="nextstepaction"]
> [Zelfstudies voor Azure Load Balancer](tutorial-load-balancer-standard-public-zone-redundant-portal.md)
 