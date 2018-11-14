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
ms.openlocfilehash: aa0ffbd69e73ddbef72e0eabf79f2736079c3d23
ms.sourcegitcommit: 1f9e1c563245f2a6dcc40ff398d20510dd88fd92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2018
ms.locfileid: "51622020"
---
# <a name="manage-all-policies-for-a-lab-in-azure-devtest-labs"></a>Alle beleidsregels beheren voor een lab in Azure DevTest Labs

Azure DevTest Labs kunt u kosten beheren en minimaliseert verspilt in uw labs door voor elk lab-beleid (instellingen) beheren. In dit artikel wordt uitgelegd in stapsgewijze informatie over het instellen van elk beleid.  

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
Het beleid voor **virtuele machines per gebruiker** kunt u het aantal virtuele machines dat kan worden gemaakt door een afzonderlijke gebruiker opgeven. Als een gebruiker probeert te maken of te claimen van een virtuele machine wanneer de gebruikerslimiet is bereikt, wordt een foutbericht geeft aan dat de virtuele machine kan niet gemaakt/geclaimd worden. 

1. Op van het lab **configuratie en het beleid** venster **virtuele machines per gebruiker**.
   
    ![Virtuele machines per gebruiker](./media/devtest-lab-set-lab-policy/max-vms-per-user.png)

1. Selecteer **Ja** om te beperken van het aantal virtuele machines per gebruiker. Als u niet beperken, het aantal virtuele machines per gebruiker wilt, selecteert u **Nee**. Als u selecteert **Ja**, voer een numerieke waarde die aangeeft van het aantal virtuele machines dat kan worden gemaakt of geclaimd door een gebruiker. 

1. Selecteer **Ja** om te beperken van het aantal virtuele machines met SSD (solid-state schijf). Als u niet wilt beperken, het aantal virtuele machines die gebruik van SSD **Nee**. Als u selecteert **Ja**, voer een waarde die aangeeft van het aantal virtuele machines dat kan worden gemaakt met behulp van SSD. 

1. Selecteer **Opslaan**.

## <a name="set-virtual-machines-per-lab"></a>Virtuele machines per lab
Het beleid voor **virtuele machines per lab** kunt u het aantal virtuele machines dat kan worden gemaakt voor de huidige testomgeving opgeven. Als een gebruiker probeert te maken van een virtuele machine wanneer de lab-limiet is bereikt, wordt een foutbericht geeft aan dat de virtuele machine kan niet worden gemaakt. 

1. Op van het lab **configuratie en het beleid** venster **virtuele machines per lab**.
   
    ![Virtuele machines per lab](./media/devtest-lab-set-lab-policy/max-vms-per-lab.png)

1. Selecteer **Ja** om te beperken van het aantal VM's per lab. Als u niet beperken, het aantal VM's per lab wilt, selecteert u **Nee**. Als u selecteert **Ja**, voer een numerieke waarde die aangeeft van het aantal virtuele machines dat kan worden gemaakt of geclaimd door een gebruiker. 

1. Selecteer **Ja** om te beperken van het aantal virtuele machines met SSD (solid-state schijf). Als u niet wilt beperken, het aantal virtuele machines die gebruik van SSD **Nee**. Als u selecteert **Ja**, voer een waarde die aangeeft van het aantal virtuele machines dat kan worden gemaakt met behulp van SSD. 

1. Selecteer **Opslaan**.

## <a name="set-auto-shutdown"></a>Set automatisch afsluiten
Het beleid voor automatisch afsluiten helpt lab verspilling doordat u de tijd die VM's van deze testomgeving afgesloten opgeven.

1. Op van het lab **configuratie en het beleid** venster **automatisch afsluiten**.
   
    ![Automatisch afsluiten](./media/devtest-lab-set-lab-policy/auto-shutdown.png)

1. Selecteer **op** aan dit beleid inschakelt, en **uit** uitschakelen.

1. Als u dit beleid inschakelt, geeft u de tijd (en tijdzone) om alle virtuele machines in de huidige testomgeving af te sluiten.

1. Geef **Ja** of **geen** voor de optie voor het verzenden van een melding 15 minuten voordat de tijd voor de opgegeven automatisch afsluiten. Als u ervoor kiest **Ja**, voert u een webhook-URL-eindpunt of een e-mailadres op te geven waar u de melding om te worden geplaatst of worden verzonden. De gebruiker melding ontvangt en de optie voor het afsluiten uitstelt is opgegeven.

   Zie voor meer informatie over webhooks [maken van een webhook of API Azure Function](../azure-functions/functions-create-a-web-hook-or-api-function.md). 

1. Selecteer **Opslaan**.

Standaard eenmaal is ingeschakeld, dit beleid is van toepassing op alle virtuele machines in de huidige testomgeving. Als deze instelling uit een specifieke virtuele machine verwijderen, open deelvenster van de VM-management en wijzig de **automatisch afsluiten** instelling.

## <a name="set-auto-shutdown-policy"></a>Beleid voor automatisch afsluiten instellen
Als de eigenaar van een lab, kunt u een planning afsluiten configureren voor alle virtuele machines in uw testomgeving. Op deze manier kunt u kosten kunt besparen van het uitvoeren van virtuele machines die niet worden gebruikt (niet-actief). U kunt afdwingen dat een beleid voor afsluiten op al uw lab-virtuele machines centraal, maar ook opslaan uw labgebruikers de moeite van het instellen van een schema voor de afzonderlijke computers. Deze functie kunt u het beleid op uw lab-schema biedt geen controle op volledig beheer voor uw labgebruikers vanaf instellen. Als de eigenaar van een lab, kunt u dit beleid configureren door de volgende stappen uit:

1. Selecteer op de startpagina van uw lab, **configuratie en het beleid**.
2. Selecteer **automatisch afsluiten beleid** in de **planningen** gedeelte van het menu links.
3. Selecteer een van de opties. De volgende secties geven u meer informatie over deze opties: de set-beleid van toepassing is alleen op nieuwe virtuele machines die zijn gemaakt in de testomgeving en niet op de al bestaande VM's. 

    ![Opties voor automatisch afsluiten](./media/devtest-lab-set-lab-policy/auto-shutdown-policy-options.png)

### <a name="user-sets-a-schedule-and-can-opt-out"></a>Gebruikers-Hiermee stelt u een planning en zich kunt afmelden
Als u uw testomgeving aan dit beleid instelt, wordt het labgebruikers kunnen overschrijven, of afmelden voor het lab-schema. Deze optie geeft labgebruikers volledige controle over de planning voor automatische afsluiting van hun virtuele machines. Labgebruikers zien geen wijziging in hun planningspagina VM automatisch afsluiten.

![Optie voor automatisch afsluiten - 1](./media/devtest-lab-set-lab-policy/auto-shutdown-policy-option-1.png)

### <a name="user-sets-a-schedule-and-cannot-opt-out"></a>Gebruikers-Hiermee stelt u een planning en kan niet opt-out
Als u uw testomgeving aan dit beleid instelt, kunnen labgebruikers het lab-schema negeren. Echter afnemen ze naar beleid voor automatisch afsluiten. Deze optie zorgt ervoor dat elke computer in uw testomgeving onder een planning voor automatische afsluiting is. Labgebruikers kunnen planning voor automatische afsluiting van hun virtuele machines bijwerken en afsluiten meldingen instellen.

![Optie voor automatisch afsluiten - 2](./media/devtest-lab-set-lab-policy/auto-shutdown-policy-option-2.png)

### <a name="user-has-no-control-over-the-schedule-set-by-lab-admin"></a>Gebruiker heeft geen controle over de planning die door de beheerder lab instellen
Als u uw testomgeving aan dit beleid instelt, kunnen de labgebruikers overschrijven of afmelden voor het lab-schema. Deze optie biedt lab-beheerder de volledige controle over de planning voor elke computer in het lab. Labgebruikers kunnen alleen automatisch afsluiten meldingen instellen voor hun VM's.

![Optie voor automatisch afsluiten - 3](./media/devtest-lab-set-lab-policy/auto-shutdown-policy-option-3.png)

## <a name="set-autostart"></a>Stel automatisch starten
Het beleid voor automatisch starten kunt u opgeven wanneer de virtuele machines in de huidige testomgeving moeten worden gestart.  

1. Op van het lab **configuratie en het beleid** venster **Autostart**.
   
    ![Automatisch starten](./media/devtest-lab-set-lab-policy/auto-start.png)

2. Selecteer **op** aan dit beleid inschakelt, en **uit** uitschakelen.

3. Als u dit beleid inschakelt, geeft u de geplande begintijd, tijdzone en de dagen van de week waarop de tijd van toepassing is. 

4. Selecteer **Opslaan**.

Eenmaal is ingeschakeld, wordt dit beleid niet automatisch toegepast op alle virtuele machines in de huidige testomgeving. Als u wilt deze instelling toepast op een specifieke virtuele machine, open deelvenster van de VM-management en wijzig de **Autostart** instelling.

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

