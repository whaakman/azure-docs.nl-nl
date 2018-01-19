---
title: 'Snelstartgids: uw eerste Azure Container Instances-container maken met PowerShell'
description: Ga aan de slag met Azure Container Instances door een exemplaar van een Windows-container te maken met PowerShell.
services: container-instances
author: mmacy
manager: timlt
ms.service: container-instances
ms.topic: quickstart
ms.date: 01/02/2018
ms.author: marsma
ms.custom: mvc
ms.openlocfilehash: 18a342fed7e99e82082764d6f5a3429a3ce794b7
ms.sourcegitcommit: 176c575aea7602682afd6214880aad0be6167c52
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2018
---
# <a name="create-your-first-container-in-azure-container-instances"></a>Uw eerste container maken in Azure Container Instances

Met Azure Container Instances kunt u gemakkelijk Docker-containers in Azure maken en beheren, zonder virtuele machines te hoeven inrichten of een service op een hoger niveau te moeten gebruiken.

In deze quick start maakt u een Windows-container in Azure en geeft u deze op internet weer via een openbaar IP-adres. Deze bewerking wordt uitgevoerd in één opdracht. Binnen enkele ogenblikken wordt de toepassing uitgevoerd in de browser:

![App die is geïmplementeerd met Azure Container Instances, weergegeven in de browser][qs-powershell-01]

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Als u PowerShell lokaal wilt installeren en gebruiken, wordt voor deze zelfstudie moduleversie 3.6 of hoger van Azure PowerShell vereist. Voer `Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps). Als u PowerShell lokaal uitvoert, moet u ook `Login-AzureRmAccount` uitvoeren om verbinding te kunnen maken met Azure.

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Maak een Azure-resourcegroep met behulp van de opdracht [New-AzureRmResourceGroup][New-AzureRmResourceGroup]. Een resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd.

 ```azurepowershell-interactive
New-AzureRmResourceGroup -Name myResourceGroup -Location EastUS
```

## <a name="create-a-container"></a>Een container maken

U kunt een container maken door een naam, een Docker-installatiekopie en een Azure-resourcegroep op te geven met de cmdlet [New-AzureRmContainerGroup][New-AzureRmContainerGroup]. U kunt de container desgewenst weergeven op internet met een openbaar IP-adres. In dit geval gebruiken we een Nano Server-container waarin IIS (Internet Information Services) wordt uitgevoerd.

 ```azurepowershell-interactive
New-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer -Image microsoft/iis:nanoserver -OsType Windows -IpAddressType Public
```

Binnen enkele seconden krijgt u een reactie op uw aanvraag. In eerste instantie heeft de container nog de status **Creating**, maar de container moet binnen een of twee minuten starten. U kunt de status controleren met de cmdlet [Get-AzureRmContainerGroup][Get-AzureRmContainerGroup]:

 ```azurepowershell-interactive
Get-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer
```

De inrichtingsstatus van de container en het IP-adres worden weergegeven in de uitvoer van de cmdlet:

```
ResourceGroupName        : myResourceGroup
Id                       : /subscriptions/12345678-1234-1234-1234-12345678abcd/resourceGroups/myResourceGroup/providers/Microsoft.ContainerInstance/containerGroups/mycontainer
Name                     : mycontainer
Type                     : Microsoft.ContainerInstance/containerGroups
Location                 : eastus
Tags                     :
ProvisioningState        : Creating
Containers               : {mycontainer}
ImageRegistryCredentials :
RestartPolicy            :
IpAddress                : 40.71.248.73
Ports                    : {80}
OsType                   : Windows
Volumes                  :
```

Als de container niet meer de status **ProvisioningState** heeft maar `Succeeded`, kunt u de container via uw browser bereiken door het opgegeven IP-adres in te voeren.

![IIS geïmplementeerd via Azure Container Instances, weergegeven in de browser][qs-powershell-01]

## <a name="delete-the-container"></a>De container verwijderen

Wanneer u klaar bent met de container, kunt u deze verwijderen met behulp van de cmdlet [Remove-AzureRmContainerGroup][Remove-AzureRmContainerGroup]:

 ```azurepowershell-interactive
Remove-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer
```

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids hebt u een vooraf samengestelde Windows-container in Azure Container Instances gestart. Als u zelf een container wilt bouwen en deze wilt implementeren in Azure Container Instances met behulp van Azure Container Registry, gaat u verder met de zelfstudie voor Azure Container Instances.

> [!div class="nextstepaction"]
> [Zelfstudie voor Azure Container Instances](./container-instances-tutorial-prepare-app.md)

<!-- IMAGES -->
[qs-powershell-01]: ./media/container-instances-quickstart-powershell/qs-powershell-01.png

<!-- LINKS -->
[New-AzureRmResourceGroup]: /powershell/module/azurerm.resources/new-azurermresourcegroup
[New-AzureRmContainerGroup]: /powershell/module/azurerm.containerinstance/new-azurermcontainergroup
[Get-AzureRmContainerGroup]: /powershell/module/azurerm.containerinstance/get-azurermcontainergroup
[Remove-AzureRmContainerGroup]: /powershell/module/azurerm.containerinstance/remove-azurermcontainergroup
