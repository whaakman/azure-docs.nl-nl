---
title: Snelstartgids - Docker-container implementeren in Azure Container Instances - PowerShell
description: In deze snelstartgids gebruikt u Azure PowerShell snel een beperkte web-app die wordt uitgevoerd in een geïsoleerde Azure containerexemplaar implementeren
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: quickstart
ms.date: 03/21/2019
ms.author: danlep
ms.custom: seodec18, mvc
ms.openlocfilehash: 8c50a3069ea8b1303e45c571425a6f4c9b4c0d5b
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/22/2019
ms.locfileid: "58368185"
---
# <a name="quickstart-deploy-a-container-instance-in-azure-using-azure-powershell"></a>Quickstart: Implementeren van een containerinstantie in Azure met behulp van Azure PowerShell

Gebruik Azure Container Instances om uit te voeren zonder server Docker-containers in Azure met de eenvoud en snelheid. Een toepassing implementeren in een container-exemplaar op aanvraag wanneer u niet nodig voor een volledige container-orchestration-platform, zoals Azure Kubernetes Service hebt.

In deze snelstartgids gebruikt u Azure PowerShell een geïsoleerde Windows-container implementeren en de toepassing beschikbaar met een volledig gekwalificeerde domeinnaam (FQDN). Een paar seconden nadat u een met één implementatieopdracht uitvoeren, kunt u bladeren naar de toepassing die wordt uitgevoerd in de container:

![App die is geïmplementeerd in Azure Container Instances, weergegeven in de browser][qs-powershell-01]

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/) aan voordat u begint.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

[!INCLUDE [cloud-shell-powershell.md](../../includes/cloud-shell-powershell.md)]

Als u PowerShell lokaal wilt installeren en gebruiken, is voor deze zelfstudie de Azure PowerShell-module vereist. Voer `Get-Module -ListAvailable Az` uit om de versie te bekijken. Als u PowerShell wilt upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-Az-ps). Als u PowerShell lokaal uitvoert, moet u ook `Connect-AzAccount` uitvoeren om verbinding te kunnen maken met Azure.

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Azure Container Instances moeten, zoals alle Azure-resources, worden geïmplementeerd in een resourcegroep. Met resourcegroepen kunt u gerelateerde Azure-resources organiseren en beheren.

Maak eerst een resourcegroep met de naam *myResourceGroup* op de locatie *eastus* met behulp van de volgende opdracht [New-AzResourceGroup][New-AzResourceGroup]:

 ```azurepowershell-interactive
New-AzResourceGroup -Name myResourceGroup -Location EastUS
```

## <a name="create-a-container"></a>Een container maken

Nu u een resourcegroep hebt, kunt u een container in Azure uitvoeren. Als u een containerinstantie wilt maken met Azure PowerShell, geeft u de naam van een resourcegroep, de naam van een containerinstantie en een Docker-containerinstallatiekopie op voor de cmdlet [New-AzContainerGroup][New-AzContainerGroup]. In deze snelstartgids gebruikt u de openbare `mcr.microsoft.com/windows/servercore/iis:nanoserver` installatiekopie. Deze afbeelding Microsoft Internet Information Services (IIS) om uit te voeren in Nano Server-pakketten.

Als u uw containers beschikbaar wilt maken op internet, moet u een of meer poorten om te openen of een DNS-naamlabel opgeven, of beide. In deze quickstart implementeert u een container met een DNS-naamlabel zodat IIS openbaar bereikbaar is.

Voer een vergelijkbaar met de volgende opdracht om een containerexemplaar te starten. Stel een `-DnsNameLabel` waarde die uniek is binnen de Azure-regio waar u het exemplaar maakt. Als u een foutbericht 'DNS-naamlabel niet beschikbaar' ontvangt, probeert u een ander DNS-naamlabel.

 ```azurepowershell-interactive
New-AzContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer -Image mcr.microsoft.com/windows/servercore/iis:nanoserver -OsType Windows -DnsNameLabel aci-demo-win
```

U ontvangt binnen enkele seconden een reactie van Azure. De `ProvisioningState` van de container is in eerste instantie **Maken**, maar moet binnen twee minuten veranderen in **Geslaagd**. Controleer de implementatiestatus met behulp van de cmdlet [Get-AzContainerGroup][Get-AzContainerGroup]:

 ```azurepowershell-interactive
Get-AzContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer
```

De inrichtingsstatus van de container, FQDN (volledig gekwalificeerde domeinnaam) en het IP-adres worden weergegeven in de uitvoer van de cmdlet:

```console
PS Azure:\> Get-AzContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer


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

Wanneer `ProvisioningState` van de container **Geslaagd** is, gaat u naar de `Fqdn` ervan in de browser. U moet nu een webpagina zien die lijkt op de volgende. U hebt een toepassing geïmplementeerd die wordt uitgevoerd in een Docker-container voor Azure.

![IIS geïmplementeerd via Azure Container Instances, weergegeven in de browser][qs-powershell-01]

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u klaar bent met de container, kunt u deze verwijderen met behulp van de cmdlet [Remove-AzContainerGroup][Remove-AzContainerGroup]:

 ```azurepowershell-interactive
Remove-AzContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer
```

## <a name="next-steps"></a>Volgende stappen

In deze snelstart hebt u een Azure-containerinstantie van een installatiekopie gemaakt in een openbare Docker Hub-opslagplaats. Als u zelf een containerinstallatiekopie wilt bouwen en deze wilt implementeren met behulp van een privé Azure Container-register, gaat u verder met de zelfstudie voor Azure Container Instances.

> [!div class="nextstepaction"]
> [Zelfstudie voor Azure Container Instances](./container-instances-tutorial-prepare-app.md)

<!-- IMAGES -->
[qs-powershell-01]: ./media/container-instances-quickstart-powershell/qs-powershell-01.png

<!-- LINKS -->
[New-AzResourceGroup]: /powershell/module/az.resources/new-Azresourcegroup
[New-AzContainerGroup]: /powershell/module/az.containerinstance/new-Azcontainergroup
[Get-AzContainerGroup]: /powershell/module/az.containerinstance/get-Azcontainergroup
[Remove-AzContainerGroup]: /powershell/module/az.containerinstance/remove-Azcontainergroup
