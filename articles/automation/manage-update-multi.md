---
title: Updates voor meerdere virtuele machines in Azure beheren
description: In dit artikel wordt beschreven hoe om updates voor Azure virtual machines te beheren.
services: automation
ms.service: automation
ms.component: update-management
author: georgewallace
ms.author: gwallace
ms.date: 04/20/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 66d50c94f2aad15e0d4a1b7500e8a4aeb45b1742
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36214250"
---
# <a name="manage-updates-for-multiple-machines"></a>Updates voor meerdere machines beheren

U kunt de oplossing voor beheer van de Update voor het beheren van updates en patches voor uw virtuele machines van Windows en Linux. Vanuit uw [Azure Automation](automation-offering-get-started.md)-account, kunt u het volgende doen:

- Vrijgeven van virtuele machines
- De status van de beschikbare updates evalueren
- Installatie van vereiste updates plannen
- Bekijk de Implementatieresultaten om te controleren of updates met succes zijn toegepast op alle virtuele machines waarvoor updatebeheer is ingeschakeld

## <a name="prerequisites"></a>Vereisten

Voor het gebruik van beheer van updates, hebt u het volgende nodig:

- Een uitvoeren als-account van Azure Automation. Zie voor meer informatie over het maken van een, [aan de slag met Azure Automation](automation-offering-get-started.md).
- Een virtuele machine of computer met een van de ondersteunde besturingssystemen.

## <a name="supported-operating-systems"></a>Ondersteunde besturingssystemen

Updatebeheer wordt ondersteund door de volgende besturingssystemen:

|Besturingssysteem  |Opmerkingen  |
|---------|---------|
|Windows Server 2008, Windows Server 2008 R2 RTM    | Ondersteunt alleen bijwerken beoordelingen.         |
|Windows Server 2008 R2 SP1 en hoger     |Windows PowerShell 4.0 of hoger is vereist. ([WMF 4.0 downloaden](https://www.microsoft.com/download/details.aspx?id=40855))</br> Windows PowerShell 5.1 wordt aanbevolen voor een hogere mate van betrouwbaarheid. ([Downloaden WMF 5.1](https://www.microsoft.com/download/details.aspx?id=54616))         |
|CentOS 6 (x86/x64) en 7 (x64)      | Linux-agents moeten toegang hebben tot een opslagplaats voor updates.        |
|Red Hat Enterprise 6 (x86/x64) en 7 (x64)     | Linux-agents moeten toegang hebben tot een opslagplaats voor updates.        |
|SUSE Linux Enterprise Server 11 (x86/x64) en 12 (x64)     | Linux-agents moeten toegang hebben tot een opslagplaats voor updates.        |
|Ubuntu 12.04 TNS, 14.04 TNS en 16.04 LTS (x86/x64)      |Linux-agents moeten toegang hebben tot een opslagplaats voor updates.         |

> [!NOTE]
> Om te voorkomen dat updates buiten een onderhoudsperiode in Ubuntu worden toegepast, moet u het pakket Unattended-Upgrade opnieuw configureren en automatische updates uitschakelen. Zie het onderwerp [Automatic Updates](https://help.ubuntu.com/lts/serverguide/automatic-updates.html) (Automatische updates) in de Ubuntu Server Guide voor meer informatie.

Linux-agents moeten toegang hebben tot een opslagplaats voor updates.

Deze oplossing biedt geen ondersteuning voor een Agent Operations Management Suite (OMS) voor Linux dat geconfigureerd voor rapportage aan meerdere Azure Log Analytics-werkruimten.

## <a name="enable-update-management-for-azure-virtual-machines"></a>Updatebeheer inschakelen voor Azure virtual machines

Open uw Automation-account in de Azure portal en selecteer vervolgens **updatebeheer**.

Selecteer **Azure VM toevoegen**.

![Virtuele machine van Azure tabblad toevoegen](./media/manage-update-multi/update-onboard-vm.png)

Selecteer een virtuele machine om te onboarden. 

Onder **updatebeheer inschakelen**, selecteer **inschakelen** voorbereiden de virtuele machine.

![Dialoogvenster Updatebeheer inschakelen](./media/manage-update-multi/update-enable.png)

Wanneer de voorbereiding is voltooid, wordt updatebeheer ingeschakeld voor uw virtuele machine.

## <a name="enable-update-management-for-non-azure-virtual-machines-and-computers"></a>Schakel het beheer van de Update voor niet-Azure virtuele machines en computers

Zie voor meer informatie over hoe u updatebeheer inschakelt voor Azure Windows virtuele machines en computers, [verbinding maken met Windows-computers naar de Log Analytics-service in Azure](../log-analytics/log-analytics-windows-agent.md).

Zie voor meer informatie over hoe u updatebeheer inschakelt voor Azure Linux virtuele machines en computers, [uw Linux-computers verbinden met Log Analytics](../log-analytics/log-analytics-agent-linux.md).

## <a name="view-computers-attached-to-your-automation-account"></a>Computers weergeven die zijn gekoppeld aan uw Automation-account

Nadat u updatebeheer voor uw machines inschakelt, kunt u de machine-informatie weergeven door te selecteren **Computers**. Vindt u informatie over *machinenaam*, *compatibiliteitsstatus*, *omgeving*, *type besturingssysteem*, *kritieke en beveiligingsupdates geïnstalleerd*, *andere updates geïnstalleerd*, en *gereedheid van de agent bijwerken* voor uw computers.

  ![Tabblad Computers weergeven](./media/manage-update-multi/update-computers-tab.png)

Computers die recent zijn ingeschakeld voor het beheer updates zijn mogelijk niet nog beoordeeld. De status van de status van naleving voor deze computers is **niet beoordeeld**. Hier volgt een lijst van mogelijke waarden voor de nalevingsstatus:

- **Compatibele**: Computers die geen ontbrekende essentiële of beveiligingsupdates.

- **Niet-compatibel**: Computers die ten minste één kritieke ontbreekt of de beveiligingsupdate.

- **Niet beoordeeld**: de beoordelingsgegevens voor de update is niet ontvangen van de computer binnen de verwachte periode. Voor Linux-computers is het tijdsbestek expect in de afgelopen drie uur. Voor Windows-computers is de verwachte periode in de afgelopen 12 uur.

Als u wilt weergeven van de status van de agent, selecteer de koppeling in de **UPDATE-AGENT READINESS** kolom. Hiermee opent u deze optie selecteert u de **Hybrid Worker** deelvenster en toont de status van de hybride Worker. De volgende afbeelding toont een voorbeeld van een agent die nog niet is verbonden met updatebeheer gedurende langere tijd:

![Tabblad Computers weergeven](./media/manage-update-multi/update-agent-broken.png)

## <a name="view-an-update-assessment"></a>Een update-evaluatie bekijken

Nadat de Update is ingeschakeld, de **updatebeheer** deelvenster wordt geopend. U ziet een lijst met ontbrekende updates op het tabblad **Ontbrekende updates**.

## <a name="collect-data"></a>Gegevens verzamelen

Agents die zijn geïnstalleerd op virtuele machines en computers worden gegevens verzameld over updates. De agents verzenden de gegevens naar Azure-updatebeheer.

### <a name="supported-agents"></a>Ondersteunde agents

De volgende tabel beschrijft de verbonden bronnen die worden ondersteund door deze oplossing:

| Verbonden bron | Ondersteund | Beschrijving |
| --- | --- | --- |
| Windows-agents |Ja |Updatebeheer verzamelt informatie over systeemupdates van Windows-agents en start vervolgens de installatie van vereiste updates. |
| Linux-agents |Ja |Updatebeheer verzamelt informatie over systeemupdates van Linux-agents en start vervolgens de installatie van vereiste updates op ondersteunde distributies. |
| Beheergroep Operations Manager |Ja |Updatebeheer verzamelt informatie over systeemupdates van agents in een verbonden beheergroep. |
| Azure-opslagaccount |Nee |Azure Storage bevat geen informatie over systeemupdates van het. |

### <a name="collection-frequency"></a>Verzamelingsfrequentie

Een scan wordt uitgevoerd twee keer per dag voor elke beheerde Windows-computer. Elke 15 minuten, de Windows-API wordt aangeroepen voor de query voor het laatst update om te bepalen of de status is gewijzigd. Als de status is gewijzigd, wordt er een scan voor naleving gestart. Een scan wordt uitgevoerd om de 3 uur voor elke beheerde Linux-computer.

Het kost tussen 30 minuten en 6 uur voor het dashboard bijgewerkte gegevens van beheerde computers wilt weergeven.

## <a name="schedule-an-update-deployment"></a>Een update-implementatie plannen

Plannen om updates te installeren, een implementatie die wordt uitgelijnd met de release planning en service-venster. U kunt kiezen welke typen updates moeten worden opgenomen in de implementatie. Zo kunt u belangrijke updates of beveiligingsupdates opnemen en updatepakketten uitsluiten.

Plannen van een nieuwe update-implementatie voor een of meer virtuele machines onder **updatebeheer**, selecteer **schema-update-implementatie**.

In de **New-update-implementatie** deelvenster, geef de volgende informatie:

- **Naam**: Voer een unieke naam voor het identificeren van de update-implementatie.
- **Besturingssysteem**: Selecteer **Windows** of **Linux**.
- **Machines bijwerken**: Selecteer de virtuele machines die u wilt bijwerken. De gereedheid van de machine wordt weergegeven in de **UPDATE-AGENT READINESS** kolom. U ziet de status van de machine voordat u de update-implementatie plannen.

  ![Nieuw deelvenster voor update-implementatie](./media/manage-update-multi/update-select-computers.png)

- **Updateclassificatie**: Selecteer de typen software om op te nemen in de update-implementatie. Zie voor een beschrijving van de typen classificatie [Updateclassificaties](automation-update-management.md#update-classifications). De classificatietypen zijn:
  - Essentiële updates
  - Beveiligingsupdates
  - Updatepakketten
  - Functiepakketten
  - Servicepacks
  - Definitie-updates
  - Hulpprogramma's
  - Updates

- **Updates moeten worden uitgesloten**: Hiermee opent u deze optie selecteert u de **uitsluiten** pagina. Voer het KB-artikelen of pakketnamen moeten worden uitgesloten.

- **Schema-instellingen**: U kunt de standaarddatum en -tijd accepteren (30 minuten na de huidige tijd). U kunt ook een ander tijdstip opgeven.

   U kunt bovendien opgeven of de implementatie eenmaal moet worden uitgevoerd of volgens een terugkerend schema. Voor het instellen van een terugkerend schema onder **terugkeerpatroon**, selecteer **terugkerend**.

   ![Dialoogvenster Schema-instellingen](./media/manage-update-multi/update-set-schedule.png)
- **Onderhoudsvenster (minuten)**: Geef de periode die u wilt dat de update-implementatie om te worden uitgevoerd. Met deze instelling zorgt u ervoor dat wijzigingen worden uitgevoerd binnen de gedefinieerde onderhoudsvensters.

Wanneer u klaar bent met het schema te configureren, selecteert u de **maken** terug te keren naar het dashboard status. De **geplande** tabel ziet u het schema dat u hebt gemaakt.

> [!WARNING]
> Voor updates waarvoor opnieuw opstarten van de virtuele machine wordt automatisch opnieuw opgestart.

## <a name="view-results-of-an-update-deployment"></a>Resultaten van een update-implementatie weergeven

Nadat de geplande implementatie wordt gestart, ziet u de status voor deze implementatie op de **Update-implementaties** tabblad onder **updatebeheer**.

Als de implementatie wordt uitgevoerd, is de status ervan **Actief**. Nadat de implementatie voltooid wordt, wordt de status gewijzigd in **geslaagd**.

Als een of meer updates in de implementatie zijn mislukt, verandert de status in **Gedeeltelijk mislukt**.

![Status van update-implementatie](./media/manage-update-multi/update-view-results.png)

Klik op de voltooide implementatie om het dashboard voor de betreffende update-implementatie te bekijken.

De **Updateresultaten** deelvenster toont het totale aantal updates en de Implementatieresultaten voor de virtuele machine. De tabel aan de rechterkant worden in detail van elke update en de resultaten van de installatie. Een van de volgende installatieresultaatwaarden wordt weergegeven:

- **Niet geprobeerd**: de update is niet geïnstalleerd omdat er onvoldoende tijd beschikbaar is op basis van de gedefinieerde onderhoudsvenster.
- **Geslaagd**: de update is voltooid.
- **Kan geen**: de update is mislukt.

Selecteer **Alle logboeken** voor een overzicht van alle logboekvermeldingen die tijdens de implementatie zijn gemaakt.

Overzicht van de taakstroom van het runbook dat de implementatie van de update op de virtuele doelmachine beheert, selecteer de tegel uitvoer.

Selecteer **Fouten** voor gedetailleerde informatie over fouten die zijn opgetreden tijdens de implementatie.

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over updatebeheer, zoals Logboeken, uitvoer en fouten, [Update beheeroplossing in Azure](../operations-management-suite/oms-solution-update-management.md).
