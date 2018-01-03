---
title: Labels toevoegen aan een lab in Azure DevTest Labs | Microsoft Docs
description: Meer informatie over het toevoegen van een label aan een lab in Azure DevTest Labs
services: devtest-lab,virtual-machines
documentationcenter: na
author: craigcaseyMSFT
manager: douge
editor: 
ms.assetid: dc5b327a-62e4-41bc-80ef-deb3c23d51b2
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/07/2017
ms.author: v-craic
ms.openlocfilehash: 38b6612bc33326b758b357dca09081c02d294e5d
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2018
---
# <a name="add-tags-to-a-lab-in-azure-devtest-labs"></a>Labels toevoegen aan een lab in Azure DevTest Labs

U kunt aangepaste labels maken en deze toepassen op uw resources DevTest Labs logisch categoriseren uw resources. Later, u kunt snel en eenvoudig overzicht van alle bronnen in uw abonnement die dit label hebben. Labels zijn handig als u nodig hebt om resources voor facturerings- of -beheer te organiseren.

Resources die worden ondersteund door tags bevatten

* Virtuele machines te berekenen
* NIC’s
* IP-adressen
* Load balancers
* Opslagaccounts
* Managed Disks

U kunt toepassen tags wanneer u [een lab maken](devtest-lab-create-lab.md) en ze later beheren via de blade labels onder instellingen en configuratie.

Elk label bestaat uit een **naam**/**waarde** paar. U kunt bijvoorbeeld een label maken met de naam *costcenter* die een waarde heeft van *34543*. Een label, zoals dit kan u later helpen identificeren lab-resources die aan dit specifieke gebied van uw organisatie factureerbare. U krijgt namen en waarden die geschikt zijn voor hoe u wilt ordenen van uw abonnement te kiezen.

## <a name="steps-to-manage-tags-in-an-existing-lab"></a>Stappen voor het beheren van de labels in een bestaande lab

1. Meld u aan bij [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Selecteer indien nodig, **meer Services**, en selecteer vervolgens **DevTest Labs** uit de lijst. Uw testomgeving mogelijk al worden weergegeven op het Dashboard onder **alle Resources**.
1. Selecteer in de lijst van labs, de testomgeving waarin u wilt toevoegen of beheren van labels.  
1. Op de testomgeving **overzicht** gebied, selecteer **configuratie en het beleid**.  

    ![Knop voor configuratie en het beleid](./media/devtest-lab-add-tag/devtestlab-config-and-policies.png)

1. Aan de linkerkant onder **beheren**, selecteer **labels**.
1. Voer voor het maken van een nieuwe code voor deze testomgeving een **naam**/**waarde** koppelen en selecteert u **opslaan**. U kunt ook een bestaand label selecteren in de lijst te bekijken of beheren van de resources die zijn gekoppeld met dit label.

    ![Tags beheren](./media/devtest-lab-add-tag/devtestlab-manage-tags.png)

## <a name="understanding-limitations-to-tags"></a>Understanding beperkingen met betrekking tot labels

Voor tags gelden de volgende beperkingen:

* Elke resource of resourcegroep mag maximaal 15 combinaties van tagnaam en -waarde hebben. Deze beperking geldt alleen voor tags die direct worden toegepast op de resourcegroep of resource. Een resourcegroep kan veel bronnen bevatten die elk 15 combinaties van tagnaam en -waarde bevatten. 
* De tagnaam is beperkt tot 512 tekens en de tagwaarde is beperkt tot 256 tekens. Voor opslagaccounts is de tagnaam beperkt tot 128 tekens en de tagwaarde beperkt tot 256 tekens.
* Tags die zijn toegepast op de resourcegroep, worden niet overgenomen door de resources in deze resourcegroep.

[Codes gebruiken om uw Azure-resources te organiseren](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-using-tags) biedt meer details over het gebruik van de labels in Azure, met inbegrip van het beheren van labels met PowerShell of Azure CLI.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Volgende stappen
* U kunt beperkingen en conventies met behulp van aangepaste beleidsregels toepassen voor uw abonnement. Een beleid dat u definieert mogelijk dat alle resources voor een bepaald label een waarde hebben. Zie voor meer informatie [beleidsregels en schema's instellen](devtest-lab-set-lab-policy.md).
* Verken de [DevTest Labs Azure Resource Manager QuickStart sjablonengalerie](https://github.com/Azure/azure-devtestlab/tree/master/Samples).
