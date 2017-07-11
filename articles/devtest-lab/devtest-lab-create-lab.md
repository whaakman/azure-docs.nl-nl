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
ms.date: 05/30/2017
ms.author: tarcher
ms.translationtype: Human Translation
ms.sourcegitcommit: a643f139be40b9b11f865d528622bafbe7dec939
ms.openlocfilehash: 015782373e59d1aaf10a7b089c84c982031b36b2
ms.contentlocale: nl-nl
ms.lasthandoff: 05/31/2017


---
<a id="create-a-lab-in-azure-devtest-labs" class="xliff"></a>

# Een lab maken in Azure DevTest Labs
Een lab in Azure DevTest Labs is de infrastructuur die een groep resources omvat, zoals VM's (virtuele machines), waarmee u deze resources beter kunt beheren door limieten en quota op te geven. In dit artikel wordt uitgelegd hoe u een lab maakt met behulp van Azure Portal.

<a id="prerequisites" class="xliff"></a>

## Vereisten
Als u een lab wilt maken, hebt u het volgende nodig:

* Een Azure-abonnement. Zie [Azure aanschaffen](https://azure.microsoft.com/pricing/purchase-options/) of [Gratis proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/) voor meer informatie over Azure-aankoopopties. U moet de eigenaar van het abonnement zijn om het lab te maken.

<a id="steps-to-create-a-lab-in-azure-devtest-labs" class="xliff"></a>

## Stappen voor het maken van een lab in Azure DevTest Labs
In de volgende stappen ziet u hoe u Azure Portal kunt gebruiken om een lab te maken in Azure DevTest Labs. 

1. Meld u aan bij [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Selecteer in het hoofdmenu aan de linkerkant **Meer services** (onder aan de lijst).

    ![Menuoptie Meer services](./media/devtest-lab-create-lab/more-services-menu-option.png)

1. Selecteer **DevTest Labs** in de lijst met beschikbare services.
1. Selecteer op de blade **DevTest Labs** de optie **Toevoegen**.
   
    ![Een lab toevoegen](./media/devtest-lab-create-lab/add-lab-button.png)

1. Op de blade **Een DevTest Lab maken**:
   
    1. Voer een **labnaam** in voor het nieuwe lab.
    2. Selecteer het **abonnement** dat u wilt koppelen aan het lab.
    3. Selecteer op welke **locatie** u het lab wilt opslaan.
    4. Selecteer **Auto-shutdown** om op te geven of u het automatisch afsluiten van alle virtuele machines van het lab wilt inschakelen en de parameters voor deze machines wilt definiëren. De functie Auto-shutdown is voornamelijk een kostenbesparende functie waar u kunt opgeven wanneer de VM automatisch moet worden afgesloten. Nadat u een lab hebt gemaakt, kunt u de Auto-shutdown-instellingen wijzigen door de stappen te volgen in het artikel [Alle beleidsregels beheren voor een lab in Azure DevTest Labs](./devtest-lab-set-lab-policy.md#set-auto-shutdown).
    5. Selecteer **Vastmaken aan dashboard** als u wilt dat een snelkoppeling van de testomgeving op het dashboard van de portal wordt weergegeven.
    6. Selecteer **Opties voor Automation** als u Azure Resource Manager-sjablonen wilt ophalen voor automatisering van de configuratie. 
    7. Selecteer **Maken**. Nadat u **Maken** hebt geselecteerd, wordt de blade **DevTest Labs** weergegeven. U kunt de vorderingen bij het maken van het lab bijhouden in de sectie **Meldingen**. Zodra het maken is voltooid, vernieuwt u de pagina en ziet u het zojuist gemaakte lab in de lijst met labs.  
    
    ![Een labblade maken](./media/devtest-lab-create-lab/create-devtestlab-blade.png)

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

<a id="next-steps" class="xliff"></a>

## Volgende stappen
Wanneer u uw lab hebt gemaakt, kunt u onder andere de volgende stappen uitvoeren:

* [Toegang tot een lab beveiligen](devtest-lab-add-devtest-user.md).
* [Labbeleidsregels instellen](devtest-lab-set-lab-policy.md).
* [Een labsjabloon maken](devtest-lab-create-template.md).
* [Aangepaste artefacten maken voor uw virtuele machines](devtest-lab-artifact-author.md).
* [Een VM met artefacten toevoegen aan een lab](devtest-lab-add-vm-with-artifacts.md).


