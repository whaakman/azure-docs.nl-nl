---
title: Maken en aanpassen van plannen voor herstel voor failover en herstel in Azure Site Recovery | Microsoft Docs
description: Informatie over het maken en plannen voor herstel in Azure Site Recovery aanpassen. In dit artikel wordt beschreven hoe u failover en herstel van virtuele machines en fysieke servers.
services: site-recovery
documentationcenter: ''
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 07/06/2018
ms.author: raynew
ms.openlocfilehash: 853b9c2f50967d7a203e5627bdd2fc6f14e0e4f6
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2018
ms.locfileid: "37920998"
---
# <a name="create-and-customize-recovery-plans"></a>Maken en plannen voor herstel aanpassen

In dit artikel wordt beschreven hoe u maken en aanpassen van een plan voor herstel in [Azure Site Recovery](site-recovery-overview.md). Voordat u begint, [meer](recovery-plan-overview.md) over plannen voor herstel.

## <a name="create-a-recovery-plan"></a>Een herstelplan maken

1. Selecteer in de Recovery Services-kluis **herstelplannen (Site Recovery)** > **+ herstelplan**.
2. In **herstelplan maken**, Geef een naam voor het abonnement.
3. Kies een bron en doel op basis van de machines in het abonnement en selecteer **Resource Manager** voor het implementatiemodel. Beschikken over de locatie van de machines die zijn ingeschakeld voor failover en herstel. 

   **Failover** | **Bron** | **Doel** 
   --- | --- | ---
   Azure naar Azure | Azure-regio |Azure-regio
   VMware naar Azure | Configuratieserver | Azure
   Fysieke machines naar Azure | Configuratieserver | Azure   
   Hyper-V wordt beheerd door VMM naar Azure  | VMM-weergavenaam | Azure
   Hyper-V zonder VMM naar Azure | De naam van de Hyper-V-site | Azure
   VMM naar VMM |Beschrijvende naam van VMM | VMM-weergavenaam 

   > [!NOTE]
   > Een herstelplan mogen machines met dezelfde bron en doel. VMware en Hyper-V-machines worden beheerd door VMM mogen niet in hetzelfde abonnement. Virtuele VMware-machines en fysieke servers kunnen zich in hetzelfde abonnement, waarbij de bron een configuratieserver is.

2. In **Select-items voor virtuele machines**, selecteert u de machines (of replicatiegroep) die u wilt toevoegen aan het plan. Klik vervolgens op **OK**.
    - Machines worden standaard-groep (groep 1) in het abonnement toegevoegd. Na een failover, worden alle machines in deze groep op hetzelfde moment start.
    - U kunt alleen machines selecteren zijn in de bron- en doellocaties die u hebt opgegeven. 
1. Klik op **OK** om het plan te maken.

## <a name="add-a-group-to-a-plan"></a>Een groep toevoegen aan een plan

U extra groepen maken en machines toevoegen aan verschillende groepen, zodat u kunt verschillend gedrag op basis van de groep door groe opgeven. U kunt bijvoorbeeld opgeven wanneer computers in een groep moeten na een failover starten of aangepaste acties per groep opgeven.

1. In **herstelplannen**, met de rechtermuisknop op het plan > **aanpassen**. Na het maken van een plan zich alle de machines die u hebt toegevoegd aan het zijn standaard in 1 groep.
2. Klik op **+ groep**. Standaard wordt een nieuwe groep genummerde in de volgorde waarin deze toegevoegd. U kunt maximaal zeven groepen hebben.
3. Selecteer de computer die u wilt verplaatsen naar de nieuwe groep, klikt u op **groep wijzigen**, en selecteer vervolgens de nieuwe groep. U kunt ook met de rechtermuisknop op de naam van de > **beveiligd item**, en computers toevoegen aan de groep. Een machine of replicatie groep kan alleen deel uitmaken van één groep van een herstelplan te gaan.


## <a name="add-a-script-or-manual-action"></a>Een script of handmatige actie toevoegen

U kunt een plan voor herstel aanpassen door een script of handmatige actie toe te voegen. Houd rekening met het volgende:

- Als u naar Azure repliceert kunt u Azure automation-runbooks kunt integreren in uw plan voor herstel. [Meer informatie](site-recovery-runbook-automation.md).
- Als u virtuele Hyper-V-machines worden beheerd door System Center VMM uitvoert, kunt u maken van een script op de on-premises VMM-server en deze opnemen in het herstelplan te gaan.
- Wanneer u een script toevoegt, wordt een nieuwe set acties voor de groep toegevoegd. Bijvoorbeeld, een reeks stappen vóór voor 1 groep is gemaakt met de naam *groep 1: stappen vóór*. Alle vooraf stappen worden vermeld in deze set. U kunt een script toevoegen op de primaire site alleen als u een VMM-server geïmplementeerd.
- Als u een handmatige actie toevoegen wanneer het herstelplan wordt uitgevoerd, wordt gestopt op het punt waarop u de handmatige actie ingevoegd. Een dialoogvenster vraagt u om op te geven dat de handmatige actie is uitgevoerd.
- Volg de instructies in voor het maken van een script op de VMM-server, [in dit artikel](hyper-v-vmm-recovery-script.md).
- Scripts kunnen worden toegepast tijdens de failover naar de secundaire site, en tijdens een failback van de secundaire site naar de primaire. Ondersteuning is afhankelijk van uw replicatiescenario:
    
    **Scenario** | **Failover** | **Failback**
    --- | --- | --- 
    Azure naar Azure  | Runbook | Runbook
    VMware naar Azure | Runbook | N.v.t. 
    Hyper-V met VMM naar Azure | Runbook | Script
    Hyper-V-site naar Azure | Runbook | N.v.t.
    VMM naar secundaire VMM | Script | Script

1. In het herstelplan te gaan, klikt u op de stap waaraan de actie moet worden toegevoegd, en opgeven wanneer de actie moet worden uitgevoerd: een. Als u wilt dat de actie die moet plaatsvinden voordat de machines in de groep worden gestart na een failover, selecteer **actie vooraf toevoegen**.
    b. Als u wilt dat de actie wordt uitgevoerd wanneer de machines in de groep na een failover, selecteert u **actie achteraf toevoegen**. Voor het verplaatsen van de positie van de actie, selecteer de **omhoog** of **omlaag** knoppen.
2. In **actie invoegen**, selecteer **Script** of **handmatige actie**.
3. Als u een handmatige actie toevoegen wilt, moet u het volgende doen"een. Typ een naam voor de actie, en typ in het actie-instructies. De persoon die de failover uitvoert, ziet deze instructies.
    b. Geef op of u wilt toevoegen de handmatige actie voor alle typen failover (Test, Failover, geplande failover (indien van toepassing)). Klik vervolgens op **OK**.
4. Als u toevoegen van een script wilt, doet u het volgende: een. Als u een VMM-script toevoegen wilt, selecteert u **VMM script voor Failover naar**, en in **scriptpad** typt u het relatieve pad naar de share. Bijvoorbeeld, als de share zich bevindt op \\ <VMMServerName>\MSSCVMMLibrary\RPScripts, geeft u het pad: \RPScripts\RPScript.PS1.
    b. Als u een Azure automation-boek uitvoeren toevoegt, geeft u de **Azure Automation-Account** waarin het runbook zich bevindt, en selecteer de juiste **Azure-Runbookscript**.
5. Voert een testfailover uit van het herstelplan om ervoor te zorgen dat het script werkt zoals verwacht.

## <a name="watch-a-video"></a>Een video bekijken

Bekijk een video waarin wordt gedemonstreerd hoe u een plan voor herstel.


> [!VIDEO https://www.youtube.com/embed/1KUVdtvGqw8]

## <a name="next-steps"></a>Volgende stappen

Meer informatie over [uitvoeren van failovers](site-recovery-failover.md).  

    
