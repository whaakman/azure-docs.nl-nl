---
title: Beheer van lab-beleid in Azure DevTest Labs | Microsoft Docs
description: "Informatie over het lab beleid zoals VM-grootten, maximum aantal virtuele machines per gebruiker en afsluiten automation definiëren."
services: devtest-lab,virtual-machines
documentationcenter: na
author: craigcaseyMSFT
manager: douge
editor: 
ms.assetid: 7756aa64-49ca-45a0-9f90-0fd101c7be85
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/03/2017
ms.author: v-craic
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c2b71fa5ec2935a25b5fb37770dfb5163a286ded
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2018
---
# <a name="manage-all-policies-for-a-lab-in-azure-devtest-labs"></a>Alle beleidsregels voor een testomgeving in Azure DevTest Labs beheren

Azure DevTest Labs kunt u de kosten te beheren en afval in uw labs minimaliseren door het beleid (instellingen) beheren voor elke lab. In dit artikel stapsgewijze wordt gedetailleerd uitgelegd hoe u elk beleid instelt.  

## <a name="set-allowed-virtual-machine-sizes"></a>Set toegestane grootten van virtuele machines
Het beleid voor het instellen van de toegestane VM-grootten helpt te minimaliseren lab verspilling kunt u opgeven welke VM-grootten zijn toegestaan in de testomgeving. Als dit beleid is geactiveerd, kan alleen VM-grootten uit deze lijst kunnen worden gebruikt voor het maken van virtuele machines.

1. In de [Azure-portal](http://go.microsoft.com/fwlink/p/?LinkID=525040), selecteert u een testomgeving en selecteer vervolgens **configuratie en het beleid**.

    ![Toegang tot de configuratie en het beleid in het lab](./media/devtest-lab-set-lab-policy/policies-menu.png)

1. Op de testomgeving **configuratie en het beleid** deelvenster **toegestane grootten voor virtuele machines**.
   
    ![Grootten van de toegestane virtuele machines](./media/devtest-lab-set-lab-policy/allowed-vm-sizes.png)

1. Selecteer **op** waarmee dit beleid en **uit** dat u deze uitschakelt.

1. Als u dit beleid inschakelt, selecteert u een of meer VM-grootten die kunnen worden gemaakt in uw testomgeving.

1. Selecteer **Opslaan**.

## <a name="set-virtual-machines-per-user"></a>Set virtuele machines per gebruiker
Het beleid voor **virtuele machines per gebruiker** kunt u het maximum aantal VM's die kunnen worden gemaakt door een afzonderlijke gebruiker opgeven. Als een gebruiker probeert te maken of een virtuele machine wanneer de gebruikerslimiet is bereikt, wordt een foutbericht geeft aan dat de virtuele machine kan niet gemaakt/geclaimd worden. 

1. Op de testomgeving **configuratie en het beleid** deelvenster **virtuele machines per gebruiker**.
   
    ![Virtuele machines per gebruiker](./media/devtest-lab-set-lab-policy/max-vms-per-user.png)

1. Selecteer **Ja** beperken het aantal virtuele machines per gebruiker. Als u niet beperken het aantal virtuele machines per gebruiker wilt, schakelt u **Nee**. Als u selecteert **Ja**, Geef een numerieke waarde die aangeeft van het maximum aantal VM's die kunnen worden gemaakt of door een gebruiker geclaimd. 

1. Selecteer **Ja** beperken het aantal virtuele machines met SSD (SSD-schijf). Als u niet wilt beperken het aantal virtuele machines die gebruik van SSD **Nee**. Als u selecteert **Ja**, voer een waarde die het maximum aantal VM's die kunnen worden gemaakt met SSD aangeeft. 

1. Selecteer **Opslaan**.

## <a name="set-virtual-machines-per-lab"></a>Set virtuele machines per lab
Het beleid voor **virtuele machines per lab** kunt u het maximum aantal VM's die kunnen worden gemaakt voor het huidige lab opgeven. Als een gebruiker een virtuele machine maken probeert wanneer de limiet van het lab is voldaan, wordt een foutbericht geeft aan dat de virtuele machine kan niet worden gemaakt. 

1. Op de testomgeving **configuratie en het beleid** deelvenster **virtuele machines per lab**.
   
    ![Virtuele machines per lab](./media/devtest-lab-set-lab-policy/max-vms-per-lab.png)

1. Selecteer **Ja** beperken het aantal virtuele machines per lab. Als u niet beperken het aantal virtuele machines per lab wilt, selecteert u **Nee**. Als u selecteert **Ja**, Geef een numerieke waarde die aangeeft van het maximum aantal VM's die kunnen worden gemaakt of door een gebruiker geclaimd. 

1. Selecteer **Ja** beperken het aantal virtuele machines met SSD (SSD-schijf). Als u niet wilt beperken het aantal virtuele machines die gebruik van SSD **Nee**. Als u selecteert **Ja**, voer een waarde die het maximum aantal VM's die kunnen worden gemaakt met SSD aangeeft. 

1. Selecteer **Opslaan**.

## <a name="set-auto-shutdown"></a>Stel automatisch afsluiten
Het beleid voor automatisch afsluiten kunt minimaliseren lab verspilling doordat u de tijd die dit lab virtuele machines afsluiten opgeven.

1. Op de testomgeving **configuratie en het beleid** deelvenster **automatisch afsluiten**.
   
    ![Automatisch afsluiten](./media/devtest-lab-set-lab-policy/auto-shutdown.png)

1. Selecteer **op** waarmee dit beleid en **uit** dat u deze uitschakelt.

1. Als u dit beleid inschakelt, geef de tijd (en tijdzone) de alle VM's in het huidige lab af te sluiten.

1. Geef **Ja** of **Nee** voor de optie een melding te verzenden 15 minuten voordat de tijd opgegeven automatisch afsluiten. Als u ervoor kiest **Ja**, voer de URL-eindpunt van een webhook of een e-mailadres opgeven van waar u de melding moet worden geplaatst of worden verzonden. De gebruiker ontvangt een melding en krijgt de mogelijkheid om uit te stellen voor het afsluiten.

   Zie voor meer informatie over webhooks [maken van een webhook of API-functie voor Azure](../azure-functions/functions-create-a-web-hook-or-api-function.md). 

1. Selecteer **Opslaan**.

Eenmaal is ingeschakeld, geldt dit beleid voor alle VM's in het huidige lab. Als deze instelling uit een specifieke virtuele machine verwijderen, opent u de VM management deelvenster en wijzig de **automatisch afsluiten** instelling.

## <a name="set-auto-start"></a>Set automatisch starten
Het beleid voor automatisch starten, kunt u opgeven wanneer de virtuele machines in het huidige lab moeten worden gestart.  

1. Op de testomgeving **configuratie en het beleid** deelvenster **automatisch starten**.
   
    ![Automatisch starten](./media/devtest-lab-set-lab-policy/auto-start.png)

2. Selecteer **op** waarmee dit beleid en **uit** dat u deze uitschakelt.

3. Als u dit beleid inschakelt, geeft u het geplande begintijdstip, tijdzone en de dagen van de week waarop de tijd van toepassing is. 

4. Selecteer **Opslaan**.

Eenmaal is ingeschakeld, wordt dit beleid niet automatisch toegepast op alle virtuele machines in het huidige lab. Als u wilt deze instelling toepast op een specifieke virtuele machine, van de VM management deelvenster open en wijzig de **automatisch starten** instelling.

## <a name="set-expiration-date"></a>Vervaldatum instellen
U kunt een vervaldatum instellen datum wanneer u [de virtuele machine maken](devtest-lab-add-vm.md). In **geavanceerde instellingen**, kies het pictogram Agenda om op te geven van een datum op waarop de virtuele machine automatisch wordt verwijderd. Standaard is de virtuele machine nooit verlopen.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Volgende stappen
Nadat u hebt gedefinieerd en de verschillende VM-beleidsinstellingen worden toegepast op uw lab, zijn hier proberen het volgende:

* [Gedeelde IP-adressen begrijpen](devtest-lab-shared-ip.md) -wordt uitgelegd hoe gedeelde IP adressen worden gebruikt in DevTest Labs om het aantal openbare IP-adressen die zijn vereist om verbinding met uw lab VM's te minimaliseren.
* [Kostenbeheer van configureren](devtest-lab-configure-cost-management.md) -laat zien hoe u de **maandelijkse geschatte kosten Trend** grafiek  
  Als u wilt weergeven van de huidige maand de geschatte kosten-to-date en de geschatte kosten van de laatste van de maand.
* [Maken van aangepaste installatiekopie](devtest-lab-create-template.md) : wanneer u een virtuele machine, maakt u een basis, kan dit een aangepaste installatiekopie of een Marketplace-installatiekopie opgeven. In dit artikel laat zien hoe een aangepaste installatiekopie maken van een VHD-bestand.
* [Configureren van installatiekopieën van Marketplace](devtest-lab-configure-marketplace-images.md) - Azure DevTest Labs ondersteunt het maken van virtuele machines op basis van Azure Marketplace-installatiekopieën. Dit artikel wordt beschreven hoe u kunt opgeven die, indien aanwezig, Azure Marketplace-installatiekopieën kunnen worden gebruikt bij het maken van virtuele machines in een testomgeving.
* [Een virtuele machine maken in een testomgeving](devtest-lab-add-vm.md) -ziet u hoe u een virtuele machine maken van een basisinstallatiekopie (een aangepaste of Marketplace), en het werken met artefacten in uw virtuele machine.

