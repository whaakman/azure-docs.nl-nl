---
title: Een lab maken in Azure DevTest Labs | Microsoft Docs
description: Een lab voor virtuele machines maken in Azure DevTest Labs
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 8b6d3e70-6528-42a4-a2ef-449575d0f928
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/12/2016
ms.author: tarcher
translationtype: Human Translation
ms.sourcegitcommit: 8c4e33a63f39d22c336efd9d77def098bd4fa0df
ms.openlocfilehash: 75188a8f74b08d28b92259348626d5734ed4ce6c
ms.lasthandoff: 04/19/2017


---
# <a name="create-a-lab-in-azure-devtest-labs"></a>Een lab maken in Azure DevTest Labs
## <a name="prerequisites"></a>Vereisten
Als u een lab wilt maken, hebt u het volgende nodig:

* Een Azure-abonnement. Zie [Azure aanschaffen](https://azure.microsoft.com/pricing/purchase-options/) of [Gratis proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/) voor meer informatie over Azure-aankoopopties. U moet de eigenaar van het abonnement zijn om het lab te maken.

## <a name="steps-to-create-a-lab-in-azure-devtest-labs"></a>Stappen voor het maken van een lab in Azure DevTest Labs
In de volgende stappen ziet u hoe u de Azure-portal kunt gebruiken om een lab te maken in Azure DevTest Labs. 

1. Meld u aan bij de [Azure-portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).
2. Selecteer **Meer services** en selecteer in de lijst vervolgens **DevTest Labs**.
3. Selecteer op de blade **DevTest Labs** de optie **Toevoegen**.
   
    ![Een lab toevoegen](./media/devtest-lab-create-lab/add-lab-button.png)
4. Op de blade **Een DevTest Lab maken**:
   
   1. Voer een **labnaam** in voor het nieuwe lab.
   2. Selecteer het **abonnement** dat u wilt koppelen aan het lab.
   3. Selecteer op welke **locatie** u het lab wilt opslaan.
   4. Selecteer **Auto-shutdown** om op te geven of u het automatisch afsluiten van alle virtuele machines van het lab wilt inschakelen en de parameters voor deze machines wilt definiëren. 
   5. Selecteer **Vastmaken aan dashboard** als u wilt dat een snelkoppeling van de testomgeving op het dashboard van de portal wordt weergegeven.
   6. Selecteer **Opties voor Automation** als u Azure Resource Manager-sjablonen wilt ophalen voor automatisering van de configuratie. 
   7. Selecteer **Maken**.
    
    ![Een labblade maken](./media/devtest-lab-create-lab/create-devtestlab-blade.png)

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Volgende stappen
Wanneer u uw lab hebt gemaakt, kunt u onder andere de volgende stappen uitvoeren:

* [Toegang tot een lab beveiligen](devtest-lab-add-devtest-user.md).
* [Labbeleidsregels instellen](devtest-lab-set-lab-policy.md).
* [Een labsjabloon maken](devtest-lab-create-template.md).
* [Aangepaste artefacten maken voor uw virtuele machines](devtest-lab-artifact-author.md).
* [Een VM met artefacten toevoegen aan een lab](devtest-lab-add-vm-with-artifacts.md).


