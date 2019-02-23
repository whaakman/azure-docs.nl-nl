---
title: Weergeven en gebruiken van een virtuele machine van Azure Resource Manager-sjabloon | Microsoft Docs
description: Informatie over het gebruik van de Azure Resource Manager-sjabloon van een virtuele machine naar een andere virtuele machines maken
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: a759d9ce-655c-4ac6-8834-cb29dd7d30dd
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: spelluru
ms.openlocfilehash: 663864837e2847125d0925b9b82d8962e54290b3
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2019
ms.locfileid: "56726479"
---
# <a name="create-virtual-machines-using-an-azure-resource-manager-template"></a>Virtuele machines met een Azure Resource Manager-sjabloon maken 

Wanneer u een virtuele machine (VM) maakt in DevTest Labs via de [Azure-portal](https://go.microsoft.com/fwlink/p/?LinkID=525040), u kunt de Azure Resource Manager-sjabloon bekijken voordat u de virtuele machine opslaan. De sjabloon kan vervolgens worden gebruikt als basis om te maken van meer lab VM's met dezelfde instellingen.

In dit artikel wordt beschreven Multi-VM versus één VM-resourcemanager-sjablonen en ziet u hoe u weergeven en opslaan van een sjabloon bij het maken van een virtuele machine.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="multi-vm-vs-single-vm-resource-manager-templates"></a>Multi-VM versus één VM-resourcemanager-sjablonen
Er zijn twee manieren om te maken van virtuele machines in DevTest Labs met behulp van Resource Manager-sjabloon: inrichten van de resource Microsoft.DevTestLab/labs/virtualmachines of de resource Microsoft.Commpute/virtualmachines inrichten. Elke wordt gebruikt in verschillende scenario's en andere machtigingen vereist.

- Resource Manager-sjablonen die gebruikmaken van een resourcetype Microsoft.DevTestLab/labs/virtualmachines (zoals opgegeven in de eigenschap 'resource' in de sjabloon) kunnen afzonderlijke lab-virtuele machines inrichten. Elke virtuele machine wordt vervolgens weergegeven als één item in de lijst met DevTest Labs-virtuele machines:

   ![Lijst met virtuele machines als één items in de lijst van de virtuele machines DevTest Labs](./media/devtest-lab-use-arm-template/devtestlab-lab-vm-single-item.png)

   Dit type Resource Manager-sjabloon kan worden ingericht via de Azure PowerShell-opdracht **New-AzResourceGroupDeployment** of via de Azure CLI-opdracht **az group deployment maken**. Dit vereist beheerdersmachtigingen, zodat gebruikers die zijn toegewezen aan een gebruikersrol DevTest Labs kunnen de implementatie niet uitvoeren. 

- Resource Manager-sjablonen die gebruikmaken van een resourcetype Microsoft.Compute/virtualmachines kunnen meerdere virtuele machines inrichten als één omgeving in de lijst met DevTest Labs-virtuele machines:

   ![Lijst met virtuele machines als één items in de lijst van de virtuele machines DevTest Labs](./media/devtest-lab-use-arm-template/devtestlab-lab-vm-single-environment.png)

   Virtuele machines in dezelfde omgeving samen kunnen worden beheerd en de dezelfde levenscyclus delen. Gebruikers die zijn toegewezen aan een gebruikersrol DevTest Labs kunnen maken met behulp van deze sjablonen als de beheerder heeft geconfigureerd in het lab op die manier omgevingen.

De rest van dit artikel wordt beschreven Resource Manager-sjablonen die gebruikmaken van Microsoft.DevTestLab/labs/virtualmachines. Deze worden gebruikt door lab-beheerders om lab VM wordt gemaakt (bijvoorbeeld claimbare virtuele machines) of gouden installatiekopie genereren (bijvoorbeeld installatiekopie factory) te automatiseren.

[Aanbevolen procedures voor het maken van Azure Resource Manager-sjablonen](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-template-best-practices) biedt veel richtlijnen en suggesties voor het maken van Azure Resource Manager-sjablonen die zijn betrouwbare en eenvoudig te gebruiken.

## <a name="view-and-save-a-virtual-machines-resource-manager-template"></a>Weergeven en opslaan van een virtuele machine van Resource Manager-sjabloon
1. Volg de stappen in [uw eerste virtuele machine maken in een testomgeving](tutorial-create-custom-lab.md#add-a-vm-to-the-lab) om te beginnen met het maken van een virtuele machine.
1. Voer de vereiste gegevens voor uw virtuele machine en alle artefacten die u wilt gebruiken voor deze virtuele machine toevoegen.
1. Aan de onderkant van het venster van de instellingen configureren kiezen **weergave ARM-sjabloon**.

   ![Knop voor ARM-sjabloon weergeven](./media/devtest-lab-use-arm-template/devtestlab-lab-view-rm-template.png)
1. Kopieer en bewaar het Resource Manager-sjabloon voor later gebruik te maken van een andere virtuele machine.

   ![Resource Manager-sjabloon op te slaan voor later gebruik](./media/devtest-lab-use-arm-template/devtestlab-lab-copy-rm-template.png)

Nadat u de Resource Manager-sjabloon hebt opgeslagen, moet u de parametersectie van de sjabloon bijwerken voordat u deze kunt gebruiken. U kunt een parameter.json waarmee alleen de parameters, buiten de daadwerkelijke Resource Manager-sjabloon wordt aangepast maken. 

![Parameters met behulp van een JSON-bestand aanpassen](./media/devtest-lab-use-arm-template/devtestlab-lab-custom-params.png)

De Resource Manager-sjabloon is nu gereed voor gebruik bij [maken van een virtuele machine](devtest-lab-create-environment-from-arm.md).

### <a name="next-steps"></a>Volgende stappen
* Meer informatie over het [multi-VM-omgevingen maken met Resource Manager-sjablonen](devtest-lab-create-environment-from-arm.md).
* [Een Resource Manager-sjabloon voor het maken van een virtuele machine implementeren](devtest-lab-create-environment-from-arm.md#deploy-a-resource-manager-template-to-create-a-vm)
* Meer snelstartsjablonen van het type Resource Manager voor het automatiseren van DevTest Labs verkennen de [openbare DevTest Labs GitHub-opslagplaats](https://github.com/Azure/azure-quickstart-templates).
