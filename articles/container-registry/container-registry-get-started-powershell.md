---
title: 'Snelstartgids: een persoonlijk Docker-register in Azure maken met PowerShell'
description: Leer snel hoe u een persoonlijk Docker-containerregister maakt met behulp van PowerShell.
services: container-registry
author: neilpeterson
manager: jeconnoc
ms.service: container-registry
ms.topic: quickstart
ms.date: 03/03/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 3097696d85c738730e725ede84437d0e36ec6bc4
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="quickstart-create-an-azure-container-registry-using-powershell"></a>Snelstart: een Azure Container Registry maken met behulp van PowerShell

Azure Container Registry is een beheerde service voor Docker-containerregisters die wordt gebruikt voor het opslaan van installatiekopieën van persoonlijke Docker-containers. In deze snelstart gaat u een exemplaar van Azure Container Registry maken met behulp van PowerShell, een containerinstallatiekopie naar het register pushen en ten slotte de container vanuit het register in Azure Container Instances (ACI) implementeren.

Voor deze snelstartgids is moduleversie 3.6 of later van Azure PowerShell vereist. Voer `Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u PowerShell wilt installeren of upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps).

Docker moet ook lokaal zijn geïnstalleerd. Docker biedt pakketten die eenvoudig Docker configureren op elk [Mac][docker-mac]-, [Windows][docker-windows]- of [Linux][docker-linux]-systeem.

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Meld u aan bij uw Azure-abonnement met de opdracht `Connect-AzureRmAccount` en volg de instructies op het scherm.

```powershell
Connect-AzureRmAccount
```

## <a name="create-resource-group"></a>Een resourcegroep maken

Maak een Azure-resourcegroep met de opdracht [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Een resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd.

```powershell
New-AzureRmResourceGroup -Name myResourceGroup -Location EastUS
```

## <a name="create-a-container-registry"></a>Een containerregister maken

Maak een ACR-exemplaar met de opdracht [New-AzureRMContainerRegistry](/powershell/module/containerregistry/New-AzureRMContainerRegistry).

De registernaam moet uniek zijn binnen Azure en mag 5 tot 50 alfanumerieke tekens bevatten. In het volgende voorbeeld wordt *myContainerRegistry007* gebruikt. Werk deze waarde bij naar een unieke waarde.

```powershell
$registry = New-AzureRMContainerRegistry -ResourceGroupName "myResourceGroup" -Name "myContainerRegistry007" -EnableAdminUser -Sku Basic
```

## <a name="log-in-to-acr"></a>Aanmelden bij ACR

Voordat u installatiekopieën van containers gaat pushen en pullen, moet u zich aanmelden bij het ACR-exemplaar. Gebruik eerst de opdracht [Get-AzureRmContainerRegistryCredential](/powershell/module/containerregistry/get-azurermcontainerregistrycredential) om de beheerdersreferenties voor het ACR-exemplaar op te halen.

```powershell
$creds = Get-AzureRmContainerRegistryCredential -Registry $registry
```

Gebruik vervolgens de opdracht [docker login][docker-login] om u aan te melden bij het ACR-exemplaar.

```powershell
docker login $registry.LoginServer -u $creds.Username -p $creds.Password
```

De opdracht retourneert `Login Succeeded` nadat deze is voltooid. Mogelijk ziet u een waarschuwing die het gebruik van de parameter `--password-stdin` aanraadt. Hoewel buiten het bestek van dit artikel, wordt u deze best practice aangeraden. Zie de verwijzing voor de opdracht [docker login][docker-login] voor meer informatie.

## <a name="push-image-to-acr"></a>Installatiekopie naar ACR overdragen met een push-bewerking

Als u een installatiekopie naar een Azure Container Registry wilt pushen, moet u eerst over een installatiekopie beschikken. Voer, indien nodig, de volgende opdracht uit om een vooraf gemaakte installatiekopie op te halen uit Docker Hub.

```powershell
docker pull microsoft/aci-helloworld
```

De installatiekopie moet zijn getagd met de naam van de ACR-aanmeldingsserver. Gebruik de opdracht [docker tag][docker-tag] om dit te doen.

```powershell
$image = $registry.LoginServer + "/aci-helloworld:v1"
docker tag microsoft/aci-helloworld $image
```

Gebruik ten slotte [docker push][docker-push] om de installatiekopie naar ACR te pushen.

```powershell
docker push $image
```

## <a name="deploy-image-to-aci"></a>Installatiekopie implementeren naar ACI
Als u de installatiekopie als een containerexemplaar wilt implementeren in Azure Container Instances (ACI), moet u de registerreferenties eerst omzetten in een PSCredential.

```powershell
$secpasswd = ConvertTo-SecureString $creds.Password -AsPlainText -Force
$pscred = New-Object System.Management.Automation.PSCredential($creds.Username, $secpasswd)
```

Voer de volgende opdracht uit om de containerinstallatiekopie vanuit het containerregister te implementeren met 1 processorkern en 1 GB geheugen:

```powershell
New-AzureRmContainerGroup -ResourceGroup myResourceGroup -Name mycontainer -Image $image -Cpu 1 -MemoryInGB 1 -IpAddressType public -Port 80 -RegistryCredential $pscred
```

Als het goed is, krijgt u een eerste reactie van Azure Resource Manager met details van de container. Als u de status van de container wilt volgen om te zien wanneer deze wordt uitgevoerd, herhaalt u de opdracht [Get-AzureRmContainerGroup][Get-AzureRmContainerGroup]. Dit duurt normaal gesproken minder dan een minuut.

```powershell
(Get-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer).ProvisioningState
```

Voorbeelduitvoer: `Succeeded`

## <a name="view-the-application"></a>De toepassing weergeven
Wanneer de implementatie naar ACI is gelukt, haalt u het openbare IP-adres van de container op met de opdracht [Get-AzureRmContainerGroup][Get-AzureRmContainerGroup]:

```powershell
(Get-AzureRmContainerGroup -ResourceGroupName myResourceGroup -Name mycontainer).IpAddress
```

Voorbeelduitvoer: `"13.72.74.222"`

Als u de actieve toepassing wilt bekijken, navigeert u in uw browser naar het openbare IP-adres. Het ziet er ongeveer als volgt uit:

![Hello world-app in browser][qs-portal-15]

## <a name="clean-up-resources"></a>Resources opschonen

Gebruik de opdracht [Remove-AzureRmResourceGroup][Remove-AzureRmResourceGroup] om de resourcegroep, het containerregister van Azure en alle containerexemplaren van Azure te verwijderen wanneer u deze niet meer nodig hebt.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

In deze snelstart hebt u een Azure-containerregister gemaakt met de Azure CLI en een exemplaar van het register gestart in Azure Container Instances. Ga verder met de zelfstudie voor Azure Container Instances om meer te leren over ACI.

> [!div class="nextstepaction"]
> [Zelfstudie voor Azure Container Instances](../container-instances/container-instances-tutorial-prepare-app.md)

<!-- LINKS - external -->
[docker-linux]: https://docs.docker.com/engine/installation/#supported-platforms
[docker-login]: https://docs.docker.com/engine/reference/commandline/login/
[docker-mac]: https://docs.docker.com/docker-for-mac/
[docker-push]: https://docs.docker.com/engine/reference/commandline/push/
[docker-tag]: https://docs.docker.com/engine/reference/commandline/tag/
[docker-windows]: https://docs.docker.com/docker-for-windows/

<!-- Links - internal -->
[Get-AzureRmContainerGroup]: /powershell/module/azurerm.containerinstance/get-azurermcontainergroup
[Remove-AzureRmResourceGroup]: /powershell/module/azurerm.resources/remove-azurermresourcegroup

<!-- IMAGES> -->
[qs-portal-15]: ./media/container-registry-get-started-portal/qs-portal-15.png
