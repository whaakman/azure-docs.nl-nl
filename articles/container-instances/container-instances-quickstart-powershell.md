---
title: Quick Start - uw eerste Azure-Containerexemplaren container maken met PowerShell
description: Aan de slag met Azure Containerexemplaren een Windows-container door exemplaar te maken met PowerShell.
services: container-instances
documentationcenter: 
author: mmacy
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/26/2017
ms.author: marsma
ms.custom: mvc
ms.openlocfilehash: fbd1bee04c5180beda23c04607b313eec9edcab4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-your-first-container-in-azure-container-instances"></a>Uw eerste container maken in Azure Container Instances

Azure Containerexemplaren gemakkelijk maken en beheren van Docker-containers in Azure, zonder te hoeven inrichten van virtuele machines of aannemen van een service op een hoger niveau.

In deze snelstartgids maken van een Windows-container in Azure en tijdens het blootgesteld aan internet met een openbaar IP-adres. Deze bewerking wordt uitgevoerd in één opdracht. Binnen enkele ogenblikken ziet u dit in uw browser:

![App die is geïmplementeerd met Azure Container Instances, weergegeven in de browser][qs-powershell-01]

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

Deze snelstartgids vereist Azure PowerShell moduleversie 4.4 of hoger. Voer `Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u PowerShell wilt installeren of upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps).

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Meld u aan bij uw Azure-abonnement met de opdracht `Login-AzureRmAccount` en volg de instructies op het scherm.

```powershell
Login-AzureRmAccount
```

## <a name="create-resource-group"></a>Een resourcegroep maken

Maak een Azure-resourcegroep met [New-AzureRmResourceGroup][New-AzureRmResourceGroup]. Een resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd.

```powershell
New-AzureRmResourceGroup -Name myResourceGroup -Location EastUS
```

## <a name="create-a-container"></a>Een container maken

U kunt een container maken door op te geven van een naam, een Docker-installatiekopie en een Azure-resourcegroep voor de [nieuw AzureRmContainerGroup] [ New-AzureRmContainerGroup] cmdlet. U kunt de container desgewenst weergeven op internet met een openbaar IP-adres. We gebruiken in dit geval een container Windows Nano Server is waarop Internet Information Services (IIS).

```powershell
New-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer -Image microsoft/iis:nanoserver -OsType Windows -IpAddressType Public
```

Binnen een paar seconden als u krijgt een reactie op uw aanvraag. In eerste instantie is de container zich in een **maken** staat, maar moet worden gestart binnen een minuut of twee. U kunt ook het gebruik van de status controleren de [Get-AzureRmContainerGroup] [ Get-AzureRmContainerGroup] cmdlet:

```powershell
Get-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer
```

De Inrichtingsstatus van de container en de IP-adres weergegeven in van de cmdlet uitvoer:

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

Eenmaal in de container **ProvisioningState** wordt verplaatst naar `Succeeded`, u kunt deze bereiken in uw browser met behulp van het IP-adres opgegeven.

![IIS is geïmplementeerd met behulp van Azure Containerexemplaren weergegeven in de browser][qs-powershell-01]

## <a name="delete-the-container"></a>De container verwijderen

Wanneer u klaar bent met de container, kunt u verwijderen met behulp van de [verwijderen AzureRmContainerGroup] [ Remove-AzureRmContainerGroup] cmdlet:

```powershell
Remove-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer
```

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids kunt u een vooraf samengestelde Windows-container in Azure Containerexemplaren gestart. Als u probeert het bouwen van een container en geïmplementeerd op Azure Container-exemplaren die gebruikmaken van het register van de Container Azure doorgaan naar de zelfstudie Containerexemplaren Azure wilt.

> [!div class="nextstepaction"]
> [Zelfstudies voor Azure Container Instances](./container-instances-tutorial-prepare-app.md)

<!-- LINKS -->
[New-AzureRmResourceGroup]: /powershell/module/azurerm.resources/new-azurermresourcegroup
[New-AzureRmContainerGroup]: /powershell/module/azurerm.containerinstance/new-azurermcontainergroup
[Get-AzureRmContainerGroup]: /powershell/module/azurerm.containerinstance/get-azurermcontainergroup
[Remove-AzureRmContainerGroup]: /powershell/module/azurerm.containerinstance/remove-azurermcontainergroup

<!-- IMAGES -->
[qs-powershell-01]: ./media/container-instances-quickstart-powershell/qs-powershell-01.png