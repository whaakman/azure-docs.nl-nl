---
title: Maken of wijzigen van automatisch in PowerShell met Azure Resource Manager-sjablonen labs | Microsoft Docs
description: Informatie over het gebruik van Azure Resource Manager-sjablonen met PowerShell maken of wijzigen van labs automatisch in een DevTest lab
services: devtest-lab,virtual-machines,visual-studio-online
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: dad9944c-0b20-48be-ba80-8f4aa0950903
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/21/2017
ms.author: tarcher
ms.openlocfilehash: cea4531175df2cc39790497dc049d27e23ffa0c6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="create-or-modify-labs-automatically-using-azure-resource-manager-templates-and-powershell"></a>Maken of wijzigen van labs automatisch met Azure Resource Manager-sjablonen en PowerShell

DevTest Labs biedt tal van sjablonen voor Azure Resource Manager en PowerShell-scripts die kunnen helpen u snel en automatisch maken van nieuwe labs of wijzig bestaande labs en vervolgens implementeert deze resources.

In dit artikel begeleidt u bij het proces van het gebruik van deze sjablonen en scripts voor het maken, wijzigen en implementeren van uw labs automatiseren. In dit artikel ziet u ook waar u meer informatie over het gebruik van PowerShell enkele veelvoorkomende taken uitvoeren in DevTest Labs kan vinden.

## <a name="step-1-gather-your-templates-and-scripts"></a>Stap 1: Uw sjablonen en scripts verzamelen
U kunt vinden en-klare [Azure Resource Manager-sjablonen](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates) en [PowerShell-scripts](https://github.com/Azure/azure-devtestlab/tree/master/Scripts) op onze openbare [Github-opslagplaats](https://github.com/Azure/azure-devtestlab). Gebruik deze als-is, of pas ze aan uw behoeften en op te slaan in uw eigen [persoonlijke Git-opslagplaats](devtest-lab-add-artifact-repo.md). 

## <a name="step-2-modify-your-azure-resource-manager-template"></a>Stap 2: Uw Azure Resource Manager-sjabloon wijzigen
U kunt de stappen op [maken van uw eerste Azure Resource Manager-sjabloon](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-manager-create-first-template) als u een sjabloon voordat nooit hebt gemaakt.

Bovendien [aanbevolen procedures voor het maken van Azure Resource Manager-sjablonen](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-template-best-practices) biedt veel richtlijnen en suggesties voor het maken van Azure Resource Manager-sjablonen die zijn betrouwbaar en eenvoudig te gebruiken. U wordt normaal gesproken gebruik een variant van een van de wijze van aanpak of voorbeelden en wijzigen van de sjabloon voor uw behoeften.

## <a name="step-3-deploy-resources-with-powershell"></a>Stap 3: Implementatie van resources met PowerShell
Nadat u uw sjablonen en scripts hebt aangepast, volg de stappen die nodig zijn voor [implementeren van resources met Resource Manager-sjablonen en Azure PowerShell](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy). Het artikel bevat algemene informatie over het implementeren van uw resources in Azure met Azure PowerShell met Azure Resource Manager-sjablonen.


## <a name="common-tasks-you-can-perform-in-devtest-labs-using-powershell"></a>Algemene taken die u kunt uitvoeren in DevTest labs met behulp van PowerShell
Er zijn veel andere veelvoorkomende taken die u automatiseren kunt met behulp van PowerShell. De volgende secties van de documentatie worden de stappen die nodig zijn om uit te voeren deze taken.

* [Een aangepaste installatiekopie maken van een VHD-bestand met behulp van PowerShell](devtest-lab-create-custom-image-from-vhd-using-powershell.md)
* [VHD-bestand uploaden naar het lab-opslagaccount met behulp van PowerShell](devtest-lab-upload-vhd-using-powershell.md)
* [Een externe gebruiker toevoegen aan een testomgeving met behulp van PowerShell](devtest-lab-add-devtest-user.md#add-an-external-user-to-a-lab-using-powershell)
* [Maakt een lab aangepaste rol met behulp van PowerShell](devtest-lab-grant-user-permissions-to-specific-lab-policies.md#creating-a-lab-custom-role-using-powershell)

### <a name="next-steps"></a>Volgende stappen
* Meer informatie over het maken van een [persoonlijke Git-opslagplaats](devtest-lab-add-artifact-repo.md) waar u uw aangepaste sjablonen of scripts wilt opslaan.
* Verken de [Azure Resource Manager-sjablonen uit Azure sjabloon snelstartgalerie](https://github.com/Azure/azure-quickstart-templates).
