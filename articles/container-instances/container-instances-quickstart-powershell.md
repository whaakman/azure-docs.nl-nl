---
title: 'Quickstart: uw eerste Azure Container Instances-container maken met PowerShell'
description: In deze snelstart gebruikt u PowerShell om een Windows-container in Azure Container Instances te implementeren
services: container-instances
author: mmacy
manager: jeconnoc
ms.service: container-instances
ms.topic: quickstart
ms.date: 05/11/2018
ms.author: marsma
ms.custom: mvc
ms.openlocfilehash: 4a1d338304dbd5e2845768b7bf0273eed23af0ec
ms.sourcegitcommit: fc64acba9d9b9784e3662327414e5fe7bd3e972e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/12/2018
---
# <a name="quickstart-create-your-first-container-in-azure-container-instances"></a>Snelstart: Uw eerste container maken in Azure Container Instances

Met Azure Container Instances kunt u gemakkelijk Docker-containers in Azure maken en beheren, zonder virtuele machines te hoeven inrichten of een service op een hoger niveau te moeten gebruiken. In deze snelstart maakt u een Windows-container in Azure en publiceert u deze op internet met een FQDN (Fully Qualified Domain Name). Deze bewerking wordt uitgevoerd in één opdracht. Binnen enkele ogenblikken wordt de toepassing uitgevoerd in de browser:

![App die is geïmplementeerd met Azure Container Instances, weergegeven in de browser][qs-powershell-01]

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/) aan voordat u begint.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Als u PowerShell lokaal wilt installeren en gebruiken, is voor deze zelfstudie versie 5.5 of later van de Azure PowerShell-module vereist. Voer `Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps). Als u PowerShell lokaal uitvoert, moet u ook `Connect-AzureRmAccount` uitvoeren om verbinding te kunnen maken met Azure.

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Maak een Azure-resourcegroep met behulp van de opdracht [New-AzureRmResourceGroup][New-AzureRmResourceGroup]. Een resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd.

 ```azurepowershell-interactive
New-AzureRmResourceGroup -Name myResourceGroup -Location EastUS
```

## <a name="create-a-container"></a>Een container maken

U kunt een container maken door een naam, een Docker-installatiekopie en een Azure-resourcegroep op te geven met de cmdlet [New-AzureRmContainerGroup][New-AzureRmContainerGroup]. U kunt de container desgewenst beschikbaar maken op internet met een DNS-naamlabel.

Voer de volgende opdracht uit om een Nano Server-container te starten waarin IIS (Internet Information Services) wordt uitgevoerd. De waarde `-DnsNameLabel` moet uniek zijn binnen de Azure-regio waar u het exemplaar maakt, dus u zult deze waarde mogelijk moeten wijzigen om ervoor te zorgen dat deze uniek is.

 ```azurepowershell-interactive
New-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer -Image microsoft/iis:nanoserver -OsType Windows -DnsNameLabel aci-demo-win
```

U ontvangt binnen een paar seconden een reactie op uw aanvraag. De container heeft in eerste instantie de status **Creating**, maar zou binnen een of twee minuten moeten worden gestart. U kunt de implementatiestatus controleren met behulp van de cmdlet [Get-AzureRmContainerGroup][Get-AzureRmContainerGroup]:

 ```azurepowershell-interactive
Get-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer
```

De inrichtingsstatus van de container, FQDN (volledig gekwalificeerde domeinnaam) en het IP-adres worden weergegeven in de uitvoer van de cmdlet:

```console
PS Azure:\> Get-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer


ResourceGroupName        : myResourceGroup
Id                       : /subscriptions/<Subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.ContainerInstance/containerGroups/mycontainer
Name                     : mycontainer
Type                     : Microsoft.ContainerInstance/containerGroups
Location                 : eastus
Tags                     :
ProvisioningState        : Creating
Containers               : {mycontainer}
ImageRegistryCredentials :
RestartPolicy            : Always
IpAddress                : 52.226.19.87
DnsNameLabel             : aci-demo-win
Fqdn                     : aci-demo-win.eastus.azurecontainer.io
Ports                    : {80}
OsType                   : Windows
Volumes                  :
State                    : Pending
Events                   : {}
```

Nadat de container **ProvisioningState** naar `Succeeded` is verplaatst, navigeert u in uw browser naar de `Fqdn` ervan:

![IIS geïmplementeerd via Azure Container Instances, weergegeven in de browser][qs-powershell-01]

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u klaar bent met de container, kunt u deze verwijderen met behulp van de cmdlet [Remove-AzureRmContainerGroup][Remove-AzureRmContainerGroup]:

 ```azurepowershell-interactive
Remove-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer
```

## <a name="next-steps"></a>Volgende stappen

In deze snelstart hebt u een Azure-containerinstantie van een installatiekopie gemaakt in een openbare Docker Hub-opslagplaats. Als u zelf een container wilt bouwen en deze wilt implementeren in Azure Container Instances met behulp van Azure Container Registry, gaat u verder met de zelfstudie voor Azure Container Instances.

> [!div class="nextstepaction"]
> [Zelfstudie voor Azure Container Instances](./container-instances-tutorial-prepare-app.md)

<!-- IMAGES -->
[qs-powershell-01]: ./media/container-instances-quickstart-powershell/qs-powershell-01.png

<!-- LINKS -->
[New-AzureRmResourceGroup]: /powershell/module/azurerm.resources/new-azurermresourcegroup
[New-AzureRmContainerGroup]: /powershell/module/azurerm.containerinstance/new-azurermcontainergroup
[Get-AzureRmContainerGroup]: /powershell/module/azurerm.containerinstance/get-azurermcontainergroup
[Remove-AzureRmContainerGroup]: /powershell/module/azurerm.containerinstance/remove-azurermcontainergroup
