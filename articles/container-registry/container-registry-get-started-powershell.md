---
title: Quick Start - een persoonlijke Docker-register maken in Azure met PowerShell
description: Snel informatie over het maken van een persoonlijke Docker-container register met PowerShell.
services: container-registry
documentationcenter: 
author: neilpeterson
manager: timlt
editor: tysonn
ms.service: container-registry
ms.devlang: na
ms.topic: quicksart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/08/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: d165cc159f7da2c8e7f0be4f0fa7d353997ed5f9
ms.sourcegitcommit: 7136d06474dd20bb8ef6a821c8d7e31edf3a2820
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/05/2017
---
# <a name="create-an-azure-container-registry-using-powershell"></a>Maken van een Azure Container register met behulp van PowerShell

Azure Container Registry is een beheerde service voor Docker-containerregisters die wordt gebruikt voor het opslaan van installatiekopieën van persoonlijke Docker-containers. Deze handleiding gegevens maken van een Azure Container register-exemplaar met behulp van PowerShell.

Deze snelstartgids vereist Azure PowerShell moduleversie 3,6 of hoger. Voer `Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u PowerShell wilt installeren of upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps).

U moet ook Docker lokaal geïnstalleerd hebben. Docker biedt pakketten die eenvoudig Docker op elke configureren op elk [Mac](https://docs.docker.com/docker-for-mac/)-, [Windows](https://docs.docker.com/docker-for-windows/)- of [Linux](https://docs.docker.com/engine/installation/#supported-platforms)-systeem.

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Meld u aan bij uw Azure-abonnement met de opdracht `Login-AzureRmAccount` en volg de instructies op het scherm.

```powershell
Login-AzureRmAccount
```

## <a name="create-resource-group"></a>Een resourcegroep maken

Maak een Azure-resourcegroep met de opdracht [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup). Een resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd.

```powershell
New-AzureRmResourceGroup -Name myResourceGroup -Location EastUS
```

## <a name="create-a-container-registry"></a>Een containerregister maken

Maak een ACR-exemplaar met de [nieuw AzureRMContainerRegistry](/powershell/module/containerregistry/New-AzureRMContainerRegistry) opdracht.

De naam van het register **moeten uniek zijn**. In het volgende voorbeeld *myContainerRegistry007* wordt gebruikt. Hiermee worden bijgewerkt naar een unieke waarde.

```powershell
$registry = New-AzureRMContainerRegistry -ResourceGroupName "myResourceGroup" -Name "myContainerRegistry007" -EnableAdminUser -Sku Basic
```

## <a name="log-in-to-acr"></a>Aanmelden bij ACR

Voordat u installatiekopieën van containers gaat pushen en pullen, moet u zich aanmelden bij het ACR-exemplaar. Gebruik eerst de [Get-AzureRmContainerRegistryCredential](/powershell/module/containerregistry/get-azurermcontainerregistrycredential) opdracht voor het ophalen van de beheerdersreferenties voor de ACR-instantie.

```powershell
$creds = Get-AzureRmContainerRegistryCredential -Registry $registry
```

Gebruik vervolgens de [docker aanmelding](https://docs.docker.com/engine/reference/commandline/login/) opdracht zich aanmelden bij de ACR-exemplaar.

```bash
docker login $registry.LoginServer -u $creds.Username -p $creds.Password
```

De opdracht retourneert een bericht dat de aanmelding is gelukt.

## <a name="push-image-to-acr"></a>Push-installatiekopie naar ACR

Voor het pushen van een afbeelding met een Azure-Container register, moet u eerst een afbeelding hebben. Indien nodig, voer de volgende opdracht voor het ophalen van een installatiekopie van een vooraf gemaakte van Docker-Hub.

```bash
docker pull microsoft/aci-helloworld
```

De afbeelding moet worden gemarkeerd met de servernaam van de ACR-aanmelding. Voer de [Get-AzureRmContainerRegistry](/powershell/module/containerregistry/Get-AzureRmContainerRegistry) opdracht voor het retourneren van de aanmeldingsnaam van de server van het ACR-exemplaar.

```powershell
Get-AzureRmContainerRegistry | Select Loginserver
```

Label van de installatiekopie met de [docker-tag](https://docs.docker.com/engine/reference/commandline/tag/) opdracht. Vervang *acrLoginServer* met de aanmeldingsnaam van de server van uw ACR-exemplaar.

```bash
docker tag microsoft/aci-helloworld <acrLoginServer>/aci-helloworld:v1
```

Gebruik tot slot [docker push](https://docs.docker.com/engine/reference/commandline/push/) voor de push-installatiekopieën van het naar het ACR-exemplaar. Vervang *acrLoginServer* met de aanmeldingsnaam van de server van uw ACR-exemplaar.

```bash
docker push <acrLoginServer>/aci-helloworld:v1
```

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer deze niet langer nodig is, kunt u de [Remove-AzureRmResourceGroup](/powershell/module/azurerm.resources/remove-azurermresourcegroup) opdracht om te verwijderen van de resourcegroep, ACR-exemplaar en alle installatiekopieën van de container.

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids kunt u een Azure Container Registry gemaakt met de Azure CLI. Als u gebruiken van Azure Container register met exemplaren van Azure-Container wilt, blijven de zelfstudie exemplaren van Azure-Container.

> [!div class="nextstepaction"]
> [Zelfstudie voor Azure Containerexemplaren](../container-instances/container-instances-tutorial-prepare-app.md)