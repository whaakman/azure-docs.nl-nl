---
title: Beheren van basisbeleid voor in Azure DevTest Labs | Microsoft Docs
description: Meer informatie over het instellen van enkele van de basic-beleid (instellingen) voor een lab in DevTest Labs
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: spelluru
ms.openlocfilehash: 8cc529fbf9b24335be1bec07f81c732ced7a2b72
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51227005"
---
# <a name="manage-basic-policies-for-a-lab-in-azure-devtest-labs"></a>Basic-beleid voor een lab in Azure DevTest Labs beheren

Azure DevTest Labs kunt u kosten beheren en minimale verspilling in uw labs door het beheer van beleid (instellingen) voor elk lab. In dit artikel hebt u aan de slag met beleid door te leren hoe u kunt twee van de meest kritieke beleidsregels instellen - het aantal virtuele machines (VM) die kunnen worden gemaakt of geclaimd door één gebruiker te beperken en het configureren van automatisch afsluiten. Als u wilt bekijken over het instellen van elk lab-beleid, Zie [definiëren van beleid voor lab maken in Azure DevTest Labs](devtest-lab-set-lab-policy.md).  

## <a name="accessing-a-labs-policies-in-azure-devtest-labs"></a>Openen van een lab-beleid in Azure DevTest Labs
De volgende stappen begeleiden u bij het instellen van beleidsregels voor een lab in Azure DevTest Labs:

Als u wilt weergeven (en wijzigen) op het beleid voor een testomgeving, de volgende stappen uit:

1. Meld u aan bij [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040).

1. Selecteer **alle services**, en selecteer vervolgens **DevTest Labs** in de lijst.

1. Selecteer de gewenste lab in de lijst met labs.   

1. Selecteer **configuratie en het beleid**.

    ![Deelvenster instellingen met beleid](./media/devtest-lab-set-lab-policy/policies-menu.png)

1. De **configuratie en het beleid** deelvenster bevat een menu met instellingen die u kunt opgeven. Dit artikel behandelt alleen de instellingen voor **virtuele machines per gebruiker**, **Auto-shutdown**, en **automatisch starten**. Zie voor meer informatie over de resterende instellingen, [alle beleidsregels beheren voor een lab in Azure DevTest Labs](./devtest-lab-set-lab-policy.md). 
   
## <a name="set-virtual-machines-per-user"></a>Virtuele machines per gebruiker
Het beleid voor **virtuele machines per gebruiker** kunt u het maximum aantal virtuele machines dat kan worden gemaakt door een afzonderlijke gebruiker opgeven. Als een gebruiker probeert te maken of te claimen van een virtuele machine wanneer de gebruikerslimiet is bereikt, wordt een foutbericht geeft aan dat de virtuele machine kan niet gemaakt/geclaimd worden. 

1. Op van het lab **configuratie en het beleid** in het menu **virtuele machines per gebruiker**.
   
    ![Virtuele machines per gebruiker](./media/devtest-lab-set-lab-policy/max-vms-per-user.png)

1. Selecteer **Ja** om te beperken van het aantal virtuele machines per gebruiker. Als u niet beperken, het aantal virtuele machines per gebruiker wilt, selecteert u **Nee**. Als u selecteert **Ja**, voer een numerieke waarde die aangeeft van het maximum aantal VM's die kunnen worden gemaakt of geclaimd door een gebruiker. 

1. Selecteer **Ja** om te beperken van het aantal virtuele machines met SSD (solid-state schijf). Als u niet wilt beperken, het aantal virtuele machines die gebruik van SSD **Nee**. Als u selecteert **Ja**, voer een waarde die aangeeft van het maximum aantal virtuele machines dat kan worden gemaakt met behulp van SSD. 

1. Selecteer **Opslaan**.

## <a name="set-auto-shutdown"></a>Set automatisch afsluiten
Het beleid voor automatisch afsluiten helpt bij het lab verspilling minimaliseren met de mogelijkheid om op te geven van de tijd die VM's van deze testomgeving afgesloten.

1. Op van het lab **configuratie en het beleid** venster **Auto-shutdown**.
   
    ![Automatisch afsluiten](./media/devtest-lab-set-lab-policy/auto-shutdown.png)

1. Selecteer **op** aan dit beleid inschakelt, en **uit** uitschakelen.

1. Als u dit beleid inschakelt, geeft u de tijd (en tijdzone) om alle virtuele machines in de huidige testomgeving af te sluiten.

1. Geef **Ja** of **geen** voor de optie voor het verzenden van een melding 15 minuten voordat de tijd opgegeven automatisch afsluiten. Als u ervoor kiest **Ja**voert u een webhook-URL-eindpunt of een e-mailadres op te geven waar u de melding om te worden geplaatst of worden verzonden. De gebruiker melding ontvangt en de optie voor het uitstellen van het afsluiten is opgegeven.

   Zie voor meer informatie over webhooks [maken van een webhook of API Azure Function](../azure-functions/functions-create-a-web-hook-or-api-function.md). 

1. Selecteer **Opslaan**.

Standaard eenmaal is ingeschakeld, dit beleid is van toepassing op alle virtuele machines in de huidige testomgeving. Als deze instelling uit een specifieke virtuele machine verwijderen, open deelvenster van de VM-management en wijzig de **Auto-shutdown** instelling.

## <a name="set-auto-start"></a>Set automatisch wordt gestart
Het beleid voor automatisch starten kunt u om op te geven wanneer de virtuele machines in de huidige testomgeving moeten worden gestart.  

1. Op van het lab **configuratie en het beleid** venster **automatisch starten**.
   
    ![Automatisch starten](./media/devtest-lab-set-lab-policy/auto-start.png)

2. Selecteer **op** aan dit beleid inschakelt, en **uit** uitschakelen.

3. Als u dit beleid inschakelt, geeft u de geplande begintijd, tijdzone en de dagen van de week waarop de tijd van toepassing is. 

4. Selecteer **Opslaan**.

Eenmaal is ingeschakeld, wordt dit beleid niet automatisch toegepast op alle virtuele machines in de huidige testomgeving. Als u wilt deze instelling toepast op een bestaande virtuele machine, open deelvenster van de VM-management en wijzig de **automatisch starten** instelling.

## <a name="next-steps"></a>Volgende stappen

- [Het definiëren van beleid voor lab maken in Azure DevTest Labs](devtest-lab-set-lab-policy.md) -informatie over het wijzigen van andere lab-beleid.
