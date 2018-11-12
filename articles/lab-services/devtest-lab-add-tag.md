---
title: Labels toevoegen aan een lab in Azure DevTest Labs | Microsoft Docs
description: Meer informatie over het toevoegen van een label aan een lab in Azure DevTest Labs
services: devtest-lab,virtual-machines
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.assetid: dc5b327a-62e4-41bc-80ef-deb3c23d51b2
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: spelluru
ms.openlocfilehash: 38ec93afd27ac75a97a28fef59bd10fafdf963c9
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51247038"
---
# <a name="add-tags-to-a-lab-in-azure-devtest-labs"></a>Labels toevoegen aan een lab in Azure DevTest Labs

U kunt aangepaste labels maken en deze toepassen op uw DevTest Labs-resources voor het categoriseren van uw resources logisch. Later, u kunt snel en eenvoudig overzicht van alle bronnen in uw abonnement met die tag. Tags zijn handig als u nodig hebt om resources voor facturering of het beheer te organiseren.

Resources die worden ondersteund door de tags behoren

* COMPUTE-VM 's
* NIC’s
* IP-adressen
* Load balancers
* Opslagaccounts
* Managed Disks

U kunt toepassen tags wanneer u [een lab maken](devtest-lab-create-lab.md) en deze later te beheren via de blade van de labels onder instellingen en configuratie.

Elke tag bestaat uit een **naam**/**waarde** paar. U kunt bijvoorbeeld een label maken met de naam van de *costcenter* die een waarde heeft van *34543*. Een label, zoals deze u kan helpen bij later labresources identificeren die zijn factureerbaar aan dit specifieke gebied van uw organisatie. U krijgt de namen en waarden die geschikt zijn voor de manier waarop u wilt organiseren uw abonnement te kiezen.

## <a name="steps-to-manage-tags-in-an-existing-lab"></a>Stappen voor het beheren van de labels in een bestaande lab

1. Meld u aan bij [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Selecteer indien nodig, **alle Services**, en selecteer vervolgens **DevTest Labs** in de lijst. Uw lab mogelijk al worden weergegeven op het Dashboard onder **alle Resources**.
1. Selecteer in de lijst met labs, in het lab waarin u wilt toevoegen of beheren van tags.  
1. Op van het lab **overzicht** gedeelte **configuratie en het beleid**.  

    ![Knop voor configuratie en het beleid](./media/devtest-lab-add-tag/devtestlab-config-and-policies.png)

1. Aan de linkerkant onder **beheren**, selecteer **Tags**.
1. Voor het maken van een nieuwe code voor deze testomgeving, voer een **naam**/**waarde** worden gekoppeld en selecteer **opslaan**. U kunt ook een bestaande tag in de lijst om te bekijken of beheren van de resources die zijn gekoppeld aan dit label selecteren.

    ![Tags beheren](./media/devtest-lab-add-tag/devtestlab-manage-tags.png)

## <a name="understanding-limitations-to-tags"></a>Understanding beperkingen met betrekking tot tags

Voor tags gelden de volgende beperkingen:

* Elke resource of resourcegroep mag maximaal 15 combinaties van tagnaam en -waarde hebben. Deze beperking geldt alleen voor tags die direct worden toegepast op de resourcegroep of resource. Een resourcegroep kan veel bronnen bevatten die elk 15 combinaties van tagnaam en -waarde bevatten. 
* De tagnaam is beperkt tot 512 tekens en de tagwaarde is beperkt tot 256 tekens. Voor opslagaccounts is de tagnaam beperkt tot 128 tekens en de tagwaarde beperkt tot 256 tekens.
* Tags die zijn toegepast op de resourcegroep, worden niet overgenomen door de resources in deze resourcegroep.

[Tags gebruiken om uw Azure-resources te organiseren](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags) vindt u meer informatie over het gebruik van tags in Azure, waaronder over het beheren van labels met PowerShell of Azure CLI.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Volgende stappen
* U kunt beperkingen en conventies met behulp van aangepaste beleidsregels toepassen voor uw abonnement. Een beleid dat u definieert mogelijk dat alle resources een waarde hebben voor een bepaalde tag. Zie voor meer informatie, [beleid en planningen instellen](devtest-lab-set-lab-policy.md).
* Verken de [DevTest Labs Azure Resource Manager QuickStart-sjablonen](https://github.com/Azure/azure-devtestlab/tree/master/Samples).
