---
title: Weergeven en gebruiken van een virtuele machine van Azure Resource Manager-sjabloon | Microsoft Docs
description: Informatie over het gebruik van de Azure Resource Manager-sjabloon van een virtuele machine naar een andere virtuele machines maken
services: devtest-lab,virtual-machines,visual-studio-online
documentationcenter: na
author: craigcaseyMSFT
manager: douge
editor: 
ms.assetid: a759d9ce-655c-4ac6-8834-cb29dd7d30dd
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: v-craic
ms.openlocfilehash: 97822d5fb11c5c106c67aaaab0b8972e1ec8deee
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2018
---
# <a name="create-virtual-machines-using-an-azure-resource-manager-template"></a>Virtuele machines met een Azure Resource Manager-sjabloon maken 

Wanneer u een virtuele machine (VM) maakt in DevTest Labs via de [Azure-portal](http://go.microsoft.com/fwlink/p/?LinkID=525040), u kunt de Azure Resource Manager-sjabloon bekijken voordat u de virtuele machine opslaan. De sjabloon kan vervolgens worden gebruikt als basis voor het maken van meer lab virtuele machines met dezelfde instellingen.

Dit artikel beschrijft Multi-VM versus één VM-resourcemanager-sjablonen en ziet u hoe weergeven en opslaan van een sjabloon bij het maken van een virtuele machine.

## <a name="multi-vm-vs-single-vm-resource-manager-templates"></a>Multi-VM versus één VM-resourcemanager-sjablonen
Er zijn twee manieren virtuele machines maken in DevTest Labs met een Resource Manager-sjabloon: de bron Microsoft.DevTestLab/labs/virtualmachines inrichten of de resource Microsoft.Commpute/virtualmachines inrichten. Elke wordt gebruikt in verschillende scenario's en andere machtigingen vereist.

- Resource Manager-sjablonen die gebruikmaken van een resourcetype Microsoft.DevTestLab/labs/virtualmachines (zoals opgegeven in de eigenschap 'resource' in de sjabloon) kunnen afzonderlijke lab virtuele machines inrichten. Elke virtuele machine vervolgens wordt weergegeven als één item in de lijst van de virtuele machines DevTest Labs:

   ![Lijst met virtuele machines als één items in de lijst van de virtuele machines DevTest Labs](./media/devtest-lab-use-arm-template/devtestlab-lab-vm-single-item.png)

   Dit type Resource Manager-sjabloon kan worden ingericht via de Azure PowerShell-opdracht **New-AzureRmResourceGroupDeployment** of via de Azure CLI-opdracht **az implementatie maken**. Het vereist administrator-machtigingen zodat gebruikers die zijn toegewezen aan een gebruikersrol DevTest Labs kunnen de implementatie niet uitvoeren. 

- Resource Manager-sjablonen die gebruikmaken van een resourcetype Microsoft.Compute/virtualmachines kunnen meerdere virtuele machines inrichten als een omgeving met één klik in de lijst van de virtuele machines DevTest Labs:

   ![Lijst met virtuele machines als één items in de lijst van de virtuele machines DevTest Labs](./media/devtest-lab-use-arm-template/devtestlab-lab-vm-single-environment.png)

   Virtuele machines in dezelfde omgeving samen kunnen worden beheerd en dezelfde levenscyclus delen. Gebruikers die zijn toegewezen aan een gebruikersrol DevTest Labs kan omgevingen met behulp van deze sjablonen, zolang de beheerder heeft geconfigureerd in het lab op die manier kunnen maken.

De rest van dit artikel wordt beschreven Resource Manager-sjablonen die Mirosoft.DevTestLab/labs/virtualmachines gebruiken. Deze worden gebruikt door beheerders lab te maken van VM-lab (bijvoorbeeld claimable VM's) of gouden installatiekopie generatie (bijvoorbeeld installatiekopie factory) automatiseren.

[Aanbevolen procedures voor het maken van Azure Resource Manager-sjablonen](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-template-best-practices) biedt veel richtlijnen en suggesties voor het maken van Azure Resource Manager-sjablonen die zijn betrouwbaar en eenvoudig te gebruiken.

## <a name="view-and-save-a-virtual-machines-resource-manager-template"></a>Weergeven en opslaan van een virtuele machine Resource Manager-sjabloon
1. Volg de stappen in [maakt van uw eerste virtuele machine in een lab](devtest-lab-create-first-vm.md) om te beginnen met het maken van een virtuele machine.
1. Voer de vereiste informatie voor uw virtuele machine en alle artefacten die u voor deze virtuele machine wilt toevoegen.
1. Aan de onderkant van het venster van de instellingen configureren, kiest u **weergave ARM-sjabloon**.

   ![Knop voor ARM-sjabloon weergeven](./media/devtest-lab-use-arm-template/devtestlab-lab-view-rm-template.png)
1. Kopiëren en opslaan van de Resource Manager-sjabloon voor later gebruik maken van een andere virtuele machine.

   ![Resource Manager-sjabloon op te slaan voor later gebruik](./media/devtest-lab-use-arm-template/devtestlab-lab-copy-rm-template.png)

Nadat u de Resource Manager-sjabloon hebt opgeslagen, moet u het gedeelte parameters van de sjabloon bijwerken voordat u deze kunt gebruiken. U kunt een parameter.json dat alleen de parameters buiten de werkelijke Resource Manager-sjabloon aanpast maken. 

![Parameters in met behulp van een JSON-bestand aanpassen](./media/devtest-lab-use-arm-template/devtestlab-lab-custom-params.png)

De Resource Manager-sjabloon is nu gereed voor gebruik bij [een virtuele machine maken](devtest-lab-create-environment-from-arm.md).

### <a name="next-steps"></a>Volgende stappen
* Meer informatie over hoe [multi-VM-omgevingen maken met Resource Manager-sjablonen](devtest-lab-create-environment-from-arm.md).
* [Een Resource Manager-sjabloon voor het maken van een virtuele machine implementeren](devtest-lab-create-environment-from-arm.md#deploy-a-resource-manager-template-to-create-a-vm)
* Ontdek meer Quick Start-Resource Manager-sjablonen voor het automatiseren van DevTest Labs vanuit de [openbare DevTest Labs GitHub-repo-](https://github.com/Azure/azure-quickstart-templates).
