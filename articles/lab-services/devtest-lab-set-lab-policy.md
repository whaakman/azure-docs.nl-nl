---
title: Beheren van beleid voor lab maken in Azure DevTest Labs | Microsoft Docs
description: Leer hoe u beleid voor lab maken, zoals VM-grootten, maximum aantal virtuele machines per gebruiker en afsluiten van automation.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.assetid: 7756aa64-49ca-45a0-9f90-0fd101c7be85
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/17/2018
ms.author: spelluru
ms.openlocfilehash: 378eb8c1f2070e8f4b28c221369938e2ff04e2f3
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51255168"
---
# <a name="manage-all-policies-for-a-lab-in-azure-devtest-labs"></a>Alle beleidsregels beheren voor een lab in Azure DevTest Labs

Azure DevTest Labs kunt u kosten beheren en minimale verspilling in uw labs door het beheer van beleid (instellingen) voor elk lab. In dit artikel wordt uitgelegd in stapsgewijze informatie over het instellen van elk beleid.  

## <a name="set-allowed-virtual-machine-sizes"></a>Set toegestane machinegrootten van virtuele
Het beleid voor het instellen van de toegestane VM-grootten helpt bij het lab verspilling minimaliseren door te geven welke VM-grootten zijn toegestaan in het lab. Als dit beleid wordt geactiveerd, kunnen alleen VM-grootten uit deze lijst worden gebruikt om virtuele machines maken.

1. In de [Azure-portal](https://go.microsoft.com/fwlink/p/?LinkID=525040), selecteert u een lab en selecteer vervolgens **configuratie en het beleid**.

    ![Toegang tot de configuratie en het beleid van het lab](./media/devtest-lab-set-lab-policy/policies-menu.png)

1. Op van het lab **configuratie en het beleid** venster **toegestane grootte van virtuele machines**.
   
    ![Grootte van toegestane virtuele machines](./media/devtest-lab-set-lab-policy/allowed-vm-sizes.png)

1. Selecteer **op** aan dit beleid inschakelt, en **uit** uitschakelen.

1. Als u dit beleid inschakelt, selecteert u een of meer VM-grootten die kunnen worden gemaakt in uw testomgeving.

1. Selecteer **Opslaan**.

## <a name="set-virtual-machines-per-user"></a>Virtuele machines per gebruiker
Het beleid voor **virtuele machines per gebruiker** kunt u het maximum aantal virtuele machines dat kan worden gemaakt door een afzonderlijke gebruiker opgeven. Als een gebruiker probeert te maken of te claimen van een virtuele machine wanneer de gebruikerslimiet is bereikt, wordt een foutbericht geeft aan dat de virtuele machine kan niet gemaakt/geclaimd worden. 

1. Op van het lab **configuratie en het beleid** venster **virtuele machines per gebruiker**.
   
    ![Virtuele machines per gebruiker](./media/devtest-lab-set-lab-policy/max-vms-per-user.png)

1. Selecteer **Ja** om te beperken van het aantal virtuele machines per gebruiker. Als u niet beperken, het aantal virtuele machines per gebruiker wilt, selecteert u **Nee**. Als u selecteert **Ja**, voer een numerieke waarde die aangeeft van het maximum aantal VM's die kunnen worden gemaakt of geclaimd door een gebruiker. 

1. Selecteer **Ja** om te beperken van het aantal virtuele machines met SSD (solid-state schijf). Als u niet wilt beperken, het aantal virtuele machines die gebruik van SSD **Nee**. Als u selecteert **Ja**, voer een waarde die aangeeft van het maximum aantal virtuele machines dat kan worden gemaakt met behulp van SSD. 

1. Selecteer **Opslaan**.

## <a name="set-virtual-machines-per-lab"></a>Virtuele machines per lab
Het beleid voor **virtuele machines per lab** kunt u het maximum aantal virtuele machines dat kan worden gemaakt voor de huidige testomgeving opgeven. Als een gebruiker probeert te maken van een virtuele machine wanneer de lab-limiet is bereikt, wordt een foutbericht geeft aan dat de virtuele machine kan niet worden gemaakt. 

1. Op van het lab **configuratie en het beleid** venster **virtuele machines per lab**.
   
    ![Virtuele machines per lab](./media/devtest-lab-set-lab-policy/max-vms-per-lab.png)

1. Selecteer **Ja** om te beperken van het aantal VM's per lab. Als u niet beperken, het aantal VM's per lab wilt, selecteert u **Nee**. Als u selecteert **Ja**, voer een numerieke waarde die aangeeft van het maximum aantal VM's die kunnen worden gemaakt of geclaimd door een gebruiker. 

1. Selecteer **Ja** om te beperken van het aantal virtuele machines met SSD (solid-state schijf). Als u niet wilt beperken, het aantal virtuele machines die gebruik van SSD **Nee**. Als u selecteert **Ja**, voer een waarde die aangeeft van het maximum aantal virtuele machines dat kan worden gemaakt met behulp van SSD. 

1. Selecteer **Opslaan**.

## <a name="set-auto-shutdown"></a>Set automatisch afsluiten
Het beleid voor automatisch afsluiten helpt lab verspilling doordat u de tijd die VM's van deze testomgeving afgesloten opgeven.

1. Op van het lab **configuratie en het beleid** venster **Auto-shutdown**.
   
    ![Automatisch afsluiten](./media/devtest-lab-set-lab-policy/auto-shutdown.png)

1. Selecteer **op** aan dit beleid inschakelt, en **uit** uitschakelen.

1. Als u dit beleid inschakelt, geeft u de tijd (en tijdzone) om alle virtuele machines in de huidige testomgeving af te sluiten.

1. Geef **Ja** of **geen** voor de optie voor het verzenden van een melding 15 minuten voordat de tijd opgegeven automatisch afsluiten. Als u ervoor kiest **Ja**, voert u een webhook-URL-eindpunt of een e-mailadres op te geven waar u de melding om te worden geplaatst of worden verzonden. De gebruiker melding ontvangt en de optie voor het uitstellen van het afsluiten is opgegeven.

   Zie voor meer informatie over webhooks [maken van een webhook of API Azure Function](../azure-functions/functions-create-a-web-hook-or-api-function.md). 

1. Selecteer **Opslaan**.

Standaard eenmaal is ingeschakeld, dit beleid is van toepassing op alle virtuele machines in de huidige testomgeving. Als deze instelling uit een specifieke virtuele machine verwijderen, open deelvenster van de VM-management en wijzig de **Auto-shutdown** instelling.

## <a name="set-auto-start"></a>Set automatisch wordt gestart
Het beleid voor automatisch starten kunt u opgeven wanneer de virtuele machines in de huidige testomgeving moeten worden gestart.  

1. Op van het lab **configuratie en het beleid** venster **automatisch starten**.
   
    ![Automatisch starten](./media/devtest-lab-set-lab-policy/auto-start.png)

2. Selecteer **op** aan dit beleid inschakelt, en **uit** uitschakelen.

3. Als u dit beleid inschakelt, geeft u de geplande begintijd, tijdzone en de dagen van de week waarop de tijd van toepassing is. 

4. Selecteer **Opslaan**.

Eenmaal is ingeschakeld, wordt dit beleid niet automatisch toegepast op alle virtuele machines in de huidige testomgeving. Als u wilt deze instelling toepast op een specifieke virtuele machine, open deelvenster van de VM-management en wijzig de **automatisch starten** instelling.

## <a name="set-expiration-date"></a>Vervaldatum instellen
U kunt instellen dat een verlopen datum wanneer u [maken van de virtuele machine](devtest-lab-add-vm.md). In **geavanceerde instellingen**, kies het pictogram Agenda om op te geven van een datum op waarop de virtuele machine automatisch wordt verwijderd. Standaard is de virtuele machine nooit verlopen.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Volgende stappen
Nadat u hebt gedefinieerd en de verschillende VM-beleidsinstellingen worden toegepast op uw lab, vindt hier u wat u moet het volgende proberen:

* [Gedeelde IP-adressen begrijpen](devtest-lab-shared-ip.md) -wordt uitgelegd hoe gedeelde IP-adressen in DevTest Labs worden gebruikt voor het aantal openbare IP-adressen die zijn vereist om verbinding met uw lab VM's te minimaliseren.
* [Kostenbeheer configureren](devtest-lab-configure-cost-management.md) -laat zien hoe u de **maandelijkse Trend voor de geschatte kosten** grafiek  
  Als u wilt weergeven van de huidige maand de geschatte kosten tot heden en de geschatte kosten voor de laatste van de maand.
* [Aangepaste installatiekopie maken](devtest-lab-create-template.md) : wanneer u een virtuele machine, maakt u een base, dit kan een aangepaste installatiekopie of een Marketplace-installatiekopie opgeven. In dit artikel ziet u hoe u een aangepaste installatiekopie maken van een VHD-bestand.
* [Marketplace-installatiekopieën configureren](devtest-lab-configure-marketplace-images.md) - Azure DevTest Labs ondersteunt het maken van virtuele machines gebaseerd op Azure Marketplace-installatiekopieën. In dit artikel laat zien hoe u om op te geven die, indien van toepassing, de Azure Marketplace-installatiekopieën kunnen worden gebruikt bij het maken van virtuele machines in een testomgeving.
* [Een virtuele machine maken in een testomgeving](devtest-lab-add-vm.md) -ziet u hoe u een virtuele machine maken vanaf een basisinstallatiekopie (een aangepaste of Marketplace), en hoe u werkt met artefacten in uw virtuele machine.

