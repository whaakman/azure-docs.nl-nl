---
title: Maken en aanpassen voor failover en herstel in de Azure Site Recovery-herstelplannen | Microsoft Docs
description: Informatie over het maken en aanpassen in Azure Site Recovery-herstelplannen. In dit artikel wordt beschreven hoe een failover en herstel van virtuele machines en fysieke servers.
services: site-recovery
documentationcenter: ''
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 03/21/2018
ms.author: raynew
ms.openlocfilehash: e02672ea76eada2d660b20f91c4417019d4efc97
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
ms.locfileid: "30189831"
---
# <a name="create-and-customize-recovery-plans"></a>Maken en plannen voor herstel aanpassen

Dit artikel wordt beschreven hoe u maken en aanpassen van een herstelplan in [Azure Site Recovery](site-recovery-overview.md). Voordat u begint, [meer](recovery-plan-overview.md) over plannen voor herstel.

## <a name="create-a-recovery-plan"></a>Een herstelplan maken

1. Selecteer in de Recovery Services-kluis **herstelplannen (Site Recovery)** > **+ herstelplan**.
2. In **herstelplan maken**, Geef een naam voor het plan.
3. Kies een bron en doel op basis van de machines in het plan en selecteer **Resource Manager** voor het implementatiemodel. De bronlocatie moet hebben machines die zijn ingeschakeld voor failover en herstel. 

   **Failover** | **Bron** | **doel** 
   --- | --- | ---
   Azure naar Azure | Azure-regio |Azure-regio
   VMware naar Azure | Configuratieserver | Azure
   Fysieke machines in Azure | Configuratieserver | Azure   
   Hyper-V wordt beheerd door VMM naar Azure  | Weergavenaam van VMM | Azure
   Hyper-V zonder VMM naar Azure | De naam van de Hyper-V-site | Azure
   VMM naar VMM |Beschrijvende naam van VMM | Weergavenaam van VMM 

   > [!NOTE]
   > Een herstelplan kan machines met dezelfde bron en doel bevatten. VMware en Hyper-V-machines die worden beheerd door VMM kan niet in hetzelfde abonnement. Virtuele VMware-machines en fysieke servers kunnen zich in hetzelfde abonnement, waarbij de bron een configuratieserver is.

2. In **Selecteer items virtuele machines**, selecteer de computers (of replicatiegroep) die u wilt toevoegen aan de planning. Klik vervolgens op **OK**.
    - Machines worden standaardgroep (groep 1) toegevoegd in het plan. Na een failover worden alle machines in deze groep op hetzelfde moment start.
    - U kunt alleen machines selecteren zijn in de bron- en doellocaties die u hebt opgegeven. 
1. Klik op **OK** om het plan te maken.

## <a name="add-a-group-to-a-plan"></a>Een groep toevoegen aan een plan

U extra groepen maken en machines toevoegen aan verschillende groepen, zodat u verschillende gedrag op basis van de groep met groepen opgeven kunt. U kunt bijvoorbeeld opgeven wanneer computers in een groep moeten na een failover starten of aangepaste acties per groep opgeven.

1. In **herstelplannen**, met de rechtermuisknop op het plan > **aanpassen**. Na het maken van een plan zich de machines die u hebt toegevoegd aan het weet standaard in groep 1.
2. Klik op **+ groep**. Een nieuwe groep heeft standaard een nummer in de volgorde waarin deze toegevoegd. U kunt maximaal zeven groepen hebben.
3. Selecteer de computer die u wilt verplaatsen naar de nieuwe groep, klikt u op **groep wijzigen**, en selecteer vervolgens de nieuwe groep. U kunt ook met de rechtermuisknop op de naam van de > **beveiligd item**, en machines toevoegen aan de groep. Een groep machine of de replicatie kan alleen in een herstelplan tot één groep behoren.


## <a name="add-a-script-or-manual-action"></a>Een script of handmatige actie toevoegen

U kunt een herstelplan aanpassen door een script of een handmatige actie toe te voegen. Houd rekening met het volgende:

- Als u naar Azure repliceert kunt u Azure automation-runbooks integreren in uw plan voor herstel. [Meer informatie](site-recovery-runbook-automation.md).
- Als u Hyper-V-machines die worden beheerd door System Center VMM repliceert, kunt u een script op de lokale VMM-beheerserver maken en deze opnemen in het herstelplan.
- Wanneer u een script toevoegt, wordt een nieuwe reeks handelingen voor de groep toegevoegd. Bijvoorbeeld, een reeks vooraf stappen voor het groep 1 is gemaakt met de naam *groep 1: vooraf stappen*. Alle vooraf stappen worden vermeld in deze set. U kunt een script toevoegen op de primaire site alleen als u een VMM-server geïmplementeerd.
- Als u een handmatige actie toevoegt wanneer het herstelplan wordt uitgevoerd, stopt hij op het punt waar u de handmatige actie ingevoegd. Een dialoogvenster vraagt u om op te geven dat de handmatige actie is voltooid.
- Volg de instructies in een script maken op de VMM-server, [in dit artikel](hyper-v-vmm-recovery-script.md).
- Scripts kunnen worden toegepast tijdens de failover naar de secundaire site, en wanneer de failback van de secundaire site naar de primaire. Ondersteuning is afhankelijk van uw replicatiescenario:
    
    **Scenario** | **Failover** | **Failback**
    --- | --- | --- 
    Azure naar Azure  | Runbook | Runbook
    VMware naar Azure | Runbook | N.v.t. 
    Hyper-V met VMM naar Azure | Runbook | Script
    Hyper-V-site naar Azure | Runbook | N.v.t.
    VMM naar secundaire VMM | Script | Script

1. Klik op de stap waaraan de actie moet worden toegevoegd en opgeven wanneer de actie moet worden uitgevoerd in het herstelplan: een. Als u wilt dat de actie optreden voordat de machines in de groep worden gestart na een failover, selecteer **vooraf actie toevoegen**.
    b. Als u wilt dat de actie wordt uitgevoerd na de machines in de groep start na een failover, selecteert u **post actie toevoegen**. Om de positie van de actie verplaatsen, selecteert de **omhoog** of **omlaag** knoppen.
2. In **actie invoegen**, selecteer **Script** of **handmatige actie**.
3. Als u een handmatige actie toevoegen wilt, moet u het volgende doen' een. Typ een naam voor de actie en typ in de instructies in te grijpen. De persoon die de failover uitvoert, ziet deze instructies.
    b. Geef op of u wilt toevoegen de handmatige actie voor alle typen failover (Test Failover, geplande failover (indien van toepassing)). Klik vervolgens op **OK**.
4. Als u toevoegen van een script wilt, het volgende doen: een. Als u een script VMM toevoegen wilt, selecteert u **Failover naar VMM script**, en in in **scriptpad** Typ het relatieve pad naar de share. Bijvoorbeeld, als de share bevindt zich op \\ <VMMServerName>\MSSCVMMLibrary\RPScripts, geef het pad: \RPScripts\RPScript.PS1.
    b. Als u een Azure automation-boek uitvoeren toevoegt, geeft u de **Azure Automation-Account** waarin het runbook zich bevindt, en selecteer de relevante **Azure Runbookscript**.
5. Een testfailover uitgevoerd van het herstelplan om ervoor te zorgen dat het script werkt zoals verwacht worden uitgevoerd.

## <a name="watch-a-video"></a>Bekijk een video

Bekijk een video die laat zien hoe u een herstelplan.


> [!VIDEO https://www.youtube.com/embed/1KUVdtvGqw8]

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [failovers met](site-recovery-failover.md).  

    
