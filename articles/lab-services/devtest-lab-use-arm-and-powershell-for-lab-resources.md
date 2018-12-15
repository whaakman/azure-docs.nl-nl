---
title: Labs automatisch in PowerShell met Azure Resource Manager-sjablonen maken of wijzigen | Microsoft Docs
description: Meer informatie over het gebruik van Azure Resource Manager-sjablonen met PowerShell voor labs automatisch in een DevTest lab maken of wijzigen
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: dad9944c-0b20-48be-ba80-8f4aa0950903
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: spelluru
ms.openlocfilehash: 2c15cac7897e25018057aa47cc2043050ed0ae5b
ms.sourcegitcommit: b254db346732b64678419db428fd9eb200f3c3c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2018
ms.locfileid: "53413197"
---
# <a name="create-or-modify-labs-automatically-using-azure-resource-manager-templates-and-powershell"></a>Labs automatisch met behulp van Azure Resource Manager-sjablonen en PowerShell maken of wijzigen

DevTest Labs biedt veel Azure Resource Manager-sjablonen en PowerShell-scripts die u kunnen helpen u snel nieuwe labs automatisch maken of wijzigen van bestaande labs en implementeerte deze resources.

In dit artikel begeleidt u bij het proces van het gebruik van deze sjablonen en scripts voor het automatiseren van het maken, wijzigen en implementeren van uw labs. In dit artikel leest u ook waar u meer informatie over het gebruik van PowerShell voor het uitvoeren van enkele veelvoorkomende taken in DevTest Labs kunt vinden.

## <a name="step-1-gather-your-templates-and-scripts"></a>Stap 1: Verzamelen van de sjablonen en -scripts
U kunt vinden en-klare [Azure Resource Manager-sjablonen](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates) en [PowerShell-scripts](https://github.com/Azure/azure-devtestlab/tree/master/Scripts) op onze openbare [GitHub-opslagplaats](https://github.com/Azure/azure-devtestlab). Gebruik deze-is, of ze aanpassen aan uw behoeften en op te slaan in uw eigen [Git-opslagplaats voor persoonlijke](devtest-lab-add-artifact-repo.md).

## <a name="step-2-modify-your-azure-resource-manager-template"></a>Stap 2: Uw Azure Resource Manager-sjabloon aanpassen
U kunt de stappen op [uw eerste Azure Resource Manager-sjabloon maken](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-create-first-template) als u een sjabloon voor nooit hebt gemaakt.

Bovendien [aanbevolen procedures voor het maken van Azure Resource Manager-sjablonen](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-template-best-practices) biedt veel richtlijnen en suggesties voor het maken van Azure Resource Manager-sjablonen die zijn betrouwbare en eenvoudig te gebruiken. Normaal gesproken u gebruikt een variant van een van de wijze van aanpak of voorbeelden en de sjabloon aanpassen aan uw behoeften.

## <a name="step-3-deploy-resources-with-powershell"></a>Stap 3: Resources implementeren met PowerShell
Nadat u de sjablonen en -scripts hebt aangepast, volgt u de benodigde stappen voor [resources implementeren met Resource Manager-sjablonen en Azure PowerShell](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy). Het artikel bevat algemene informatie over het gebruik van Azure PowerShell met Azure Resource Manager-sjablonen voor het implementeren van uw resources naar Azure.


## <a name="common-tasks-you-can-perform-in-devtest-labs-using-powershell"></a>Algemene taken die u kunt uitvoeren in DevTest labs met behulp van PowerShell
Er zijn veel andere algemene taken die u automatiseren kunt met behulp van PowerShell. De volgende secties van de documentatie beschrijven de stappen die nodig zijn om uit te voeren van deze taken.

* [Een aangepaste installatiekopie maken van een VHD-bestand met behulp van PowerShell](devtest-lab-create-custom-image-from-vhd-using-powershell.md)
* [VHD-bestand uploaden naar het lab storage-account met behulp van PowerShell](devtest-lab-upload-vhd-using-powershell.md)
* [Een externe gebruiker toevoegen aan een lab met behulp van PowerShell](devtest-lab-add-devtest-user.md#add-an-external-user-to-a-lab-using-powershell)
* [Maak een aangepaste lab-rol met behulp van PowerShell](devtest-lab-grant-user-permissions-to-specific-lab-policies.md#creating-a-lab-custom-role-using-powershell)

### <a name="next-steps"></a>Volgende stappen
* Meer informatie over het maken van een [privé-Git-opslagplaats](devtest-lab-add-artifact-repo.md) waar u uw aangepaste sjablonen of scripts worden opgeslagen.
* Verken de [Azure Resource Manager-sjablonen van Azure Quickstart-sjablonen](https://github.com/Azure/azure-quickstart-templates).
