---
title: Updates voor meerdere virtuele machines in Azure beheren
description: In dit artikel wordt beschreven hoe u updates beheren voor virtuele machines van Azure.
services: automation
ms.service: automation
ms.component: update-management
author: georgewallace
ms.author: gwallace
ms.date: 01/10/2019
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 3897225ef6ed7fcc0db75e82058e5b5b273ccbd4
ms.sourcegitcommit: e7312c5653693041f3cbfda5d784f034a7a1a8f1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/11/2019
ms.locfileid: "54214025"
---
# <a name="manage-updates-for-multiple-machines"></a>Updates voor meerdere machines beheren

U kunt de oplossing Update Management gebruiken voor het beheren van updates en patches voor uw virtuele machines van Windows en Linux. Vanuit uw [Azure Automation](automation-offering-get-started.md)-account, kunt u het volgende doen:

- Virtuele machines activeren
- De status van de beschikbare updates beoordelen
- Installatie van vereiste updates plannen
- Implementatieresultaten weergeven om te controleren of updates met succes zijn toegepast op alle virtuele machines waarvoor updatebeheer is ingeschakeld

## <a name="prerequisites"></a>Vereisten

Voor het gebruik van updatebeheer hebt u het volgende nodig:

- Een uitvoeren als-account van Azure Automation. Zie voor meer informatie over het maken van een, [aan de slag met Azure Automation](automation-offering-get-started.md).
- Een virtuele machine of computer met een van de ondersteunde besturingssystemen.

## <a name="supported-operating-systems"></a>Ondersteunde besturingssystemen

Updatebeheer wordt ondersteund op de volgende besturingssystemen:

|Besturingssysteem  |Opmerkingen  |
|---------|---------|
|Windows Server 2008, Windows Server 2008 R2 RTM    | Ondersteunt alleen bijwerken evaluaties.         |
|Windows Server 2008 R2 SP1 en hoger     |Windows PowerShell 4.0 of hoger is vereist. ([WMF 4.0 downloaden](https://www.microsoft.com/download/details.aspx?id=40855))</br> Windows PowerShell 5.1 wordt aanbevolen voor hogere mate van betrouwbaarheid. ([Downloaden van WMF 5.1](https://www.microsoft.com/download/details.aspx?id=54616))         |
|CentOS 6 (x86/x64) en 7 (x64)      | Linux-agents moeten toegang hebben tot een opslagplaats voor updates.        |
|Red Hat Enterprise 6 (x86/x64) en 7 (x64)     | Linux-agents moeten toegang hebben tot een opslagplaats voor updates.        |
|SUSE Linux Enterprise Server 11 (x86/x64) en 12 (x64)     | Linux-agents moeten toegang hebben tot een opslagplaats voor updates.        |
|Ubuntu 14.04 LTS, 16.04 LTS en 18.04 LTS (x86/x64)      |Linux-agents moeten toegang hebben tot een opslagplaats voor updates.         |

> [!NOTE]
> Om te voorkomen dat updates buiten een onderhoudsperiode in Ubuntu worden toegepast, moet u het pakket Unattended-Upgrade opnieuw configureren en automatische updates uitschakelen. Zie het onderwerp [Automatic Updates](https://help.ubuntu.com/lts/serverguide/automatic-updates.html) (Automatische updates) in de Ubuntu Server Guide voor meer informatie.

Linux-agents moeten toegang hebben tot een opslagplaats voor updates.

Deze oplossing biedt geen ondersteuning voor een Log Analytics-Agent voor Linux die geconfigureerd voor rapportage aan meerdere Azure Log Analytics-werkruimten.

## <a name="enable-update-management-for-azure-virtual-machines"></a>Inschakelen van updatebeheer voor virtuele machines van Azure

Open uw Automation-account in de Azure-portal en selecteer vervolgens **updatebeheer**.

Selecteer **Azure VM's toevoegen**.

![Tabblad Azure-VM toevoegen](./media/manage-update-multi/update-onboard-vm.png)

Selecteer een virtuele machine om te onboarden. 

Onder **updatebeheer inschakelen**, selecteer **inschakelen** moet worden vrijgegeven de virtuele machine.

![Dialoogvenster Updatebeheer inschakelen](./media/manage-update-multi/update-enable.png)

Zodra de voorbereiding is voltooid, wordt updatebeheer is ingeschakeld voor uw virtuele machine.

## <a name="enable-update-management-for-non-azure-virtual-machines-and-computers"></a>Updatebeheer inschakelen voor niet-Azure virtuele machines en computers

Zie voor informatie over het inschakelen van updatebeheer voor virtuele machines van Azure Windows- en -computers, [verbinding maken met Windows-computers naar de Log Analytics-service in Azure](../log-analytics/log-analytics-windows-agent.md).

Zie voor informatie over het inschakelen van updatebeheer voor virtuele Azure Linux-machines en computers, [uw Linux-computers verbinden met Log Analytics](../log-analytics/log-analytics-agent-linux.md).

## <a name="view-computers-attached-to-your-automation-account"></a>Computers weergeven die zijn gekoppeld aan uw Automation-account

Nadat u updatebeheer voor uw virtuele machines inschakelt, kunt u de informatie van machine weergeven door te selecteren **Computers**. Ziet u informatie over *computernaam*, *nalevingsstatus*, *omgeving*, *besturingssysteemtype*, *kritieke en beveiligingsupdates geïnstalleerd*, *andere updates geïnstalleerd*, en *gereedheid van de agent bijwerken* voor uw computers.

  ![Tabblad Computers weergeven](./media/manage-update-multi/update-computers-tab.png)

Computers die onlangs zijn ingeschakeld voor het beheer updates mogelijk zijn nog niet geëvalueerd. De status van de status van naleving voor deze computers is **niet beoordeeld**. Hier volgt een lijst van mogelijke waarden voor de nalevingsstatus:

- **Compatibele**: Computers die geen ontbrekende essentiële of beveiligingsupdates.

- **Niet-compatibele**: Computers die niet over ten minste één essentiële beschikken of beveiligingsupdate.

- **Niet beoordeeld**: De gegevens van de evaluatie van de update nog niet is ontvangen van de computer binnen het verwachte tijdsbestek. Voor Linux-computers is het verwachte tijdsbestek in de afgelopen 3 uur. Voor Windows-computers is het verwachte tijdsbestek in de afgelopen 12 uur.

Als u wilt de status van agent weergeven, selecteert u de koppeling in de **gereedheid voor UPDATE-AGENT** kolom. Hiermee opent u deze optie selecteert, de **Hybrid Worker** in het deelvenster en toont de status van de Hybrid Worker. De volgende afbeelding toont een voorbeeld van een agent die nog niet is verbonden met updatebeheer voor een lange periode:

![Tabblad Computers weergeven](./media/manage-update-multi/update-agent-broken.png)

## <a name="view-an-update-assessment"></a>Een update-evaluatie bekijken

Nadat Updatebeheer is ingeschakeld, wordt het deelvenster **Updatebeheer** geopend. U ziet een lijst met ontbrekende updates op het tabblad **Ontbrekende updates**.

## <a name="collect-data"></a>Gegevens verzamelen

Agents die zijn geïnstalleerd op virtuele machines en computers verzamelen gegevens over updates. De agents worden de gegevens verzenden naar Azure updatebeheer.

### <a name="supported-agents"></a>Ondersteunde agents

De volgende tabel beschrijft de verbonden bronnen die worden ondersteund door deze oplossing:

| Verbonden bron | Ondersteund | Description |
| --- | --- | --- |
| Windows-agents |Ja |Updatebeheer verzamelt informatie over systeemupdates van Windows-agents en start vervolgens de installatie van vereiste updates. |
| Linux-agents |Ja |Updatebeheer verzamelt informatie over systeemupdates van Linux-agents en start vervolgens de installatie van vereiste updates op ondersteunde distributies. |
| Beheergroep Operations Manager |Ja |Updatebeheer verzamelt informatie over systeemupdates van agents in een verbonden beheergroep. |
| Azure Storage-account |Nee |Azure Storage bevat geen informatie over systeemupdates. |

### <a name="collection-frequency"></a>Verzamelingsfrequentie

Nadat een computer is voltooid scannen voor Updatevereisten, stuurt de agent de informatie in bulk door naar Azure Log Analytics. Op een Windows-computer, de nalevingsscan standaard elke 12 uur uitgevoerd.

Naast het schema voor scannen, wordt de scan voor naleving van updates binnen 15 minuten van de MMA opnieuw wordt gestart, voordat de installatie van de update en na installatie van update gestart.

Voor een Linux-computer wordt de nalevingsscan standaard elke drie uur uitgevoerd. Als de MMA-agent opnieuw is opgestart, wordt een nalevingsscan gestart binnen 15 minuten.

Duurt tussen 30 minuten en 6 uur voor het dashboard bijgewerkte gegevens van beheerde computers worden weergegeven.

## <a name="schedule-an-update-deployment"></a>Een update-implementatie plannen

Om updates te installeren, plant u een implementatie die met uw release-planning en servicevenster overeenstemt. U kunt kiezen welke typen updates moeten worden opgenomen in de implementatie. Zo kunt u belangrijke updates of beveiligingsupdates opnemen en updatepakketten uitsluiten.

Voor het plannen van een nieuwe update-implementatie voor een of meer virtuele machines onder **updatebeheer**, selecteer **update-implementatie plannen**.

In de **nieuwe update-implementatie** in het deelvenster de volgende informatie opgeven:

- **Naam**: Voer een unieke naam voor het identificeren van de update-implementatie.
- **Besturingssysteem**: Selecteer **Windows** of **Linux**.
- **Groepen om bij te werken (preview)**: definieer een query op basis van een combinatie van abonnement, resourcegroepen, locaties en tags om een dynamische groep virtuele Azure-machines te bouwen voor opname in uw implementatie. Zie [Dynamische groepen](automation-update-management.md#using-dynamic-groups) voor meer informatie
- **Bij te werken computers**: Selecteer een opgeslagen zoekopdracht, geïmporteerd groep, of selecteer Machines om te kiezen op de computers die u wilt bijwerken. Als u **Computers** selecteert, wordt de gereedheid van de computer weergegeven in de kolom **GEREEDHEID VOOR UPDATE-AGENT**. U ziet de status van de machine voordat u de update-implementatie plannen. Zie [Computergroepen in Log Analytics](../azure-monitor/platform/computer-groups.md) voor meer informatie over de verschillende manieren waarop u computergroepen kunt maken in Log Analytics

  ![Deelvenster met nieuwe update-implementatie](./media/manage-update-multi/update-select-computers.png)

- **Updateclassificatie**: Selecteer de typen software om op te nemen in de update-implementatie. Zie voor een beschrijving van de classificatietypen [Updateclassificaties](automation-update-management.md#update-classifications). De classificatietypen zijn:
  - Essentiële updates
  - Beveiligingsupdates
  - Updatepakketten
  - Functiepakketten
  - Servicepacks
  - Definitie-updates
  - Hulpprogramma's
  - Updates

- **Updates om op te nemen/uit te sluiten**: hiermee opent u de pagina **Opnemen/uitsluiten**. Updates die moeten worden opgenomen of uitgesloten, worden op afzonderlijke tabbladen weergegeven. Zie [Werking van opname](automation-update-management.md#inclusion-behavior) voor meer informatie over hoe de opname wordt verwerkt

- **Planningsinstellingen**: U kunt de standaarddatum en -tijd 30 minuten na de huidige tijd is accepteren. U kunt ook een andere tijd opgeven.

   U kunt bovendien opgeven of de implementatie eenmaal moet worden uitgevoerd of volgens een terugkerend schema. Voor het instellen van een terugkerend schema uit onder **terugkeerpatroon**, selecteer **periodiek**.

   ![Dialoogvenster Schema-instellingen](./media/manage-update-multi/update-set-schedule.png)

- **Voorafgaande scripts en navolgende scripts**: selecteer de scripts die moeten worden uitgevoerd vóór en na de implementatie. Zie[Manage Pre and Post scripts](pre-post-scripts.md) (Voorafgaande en navolgende scripts beheren) voor meer informatie.
- **Onderhoudsvenster (minuten)**: Geef de periode die u wilt dat de update-implementatie moet plaatsvinden. Met deze instelling zorgt u ervoor dat wijzigingen worden uitgevoerd binnen de gedefinieerde onderhoudsvensters.

- **Opnieuw opstarten besturingselement** -deze instelling bepaalt hoe opnieuw wordt opgestart voor de update-implementatie worden verwerkt.

   |Optie|Description|
   |---|---|
   |Opnieuw opstarten indien nodig| **(Standaard)**  Indien nodig, opnieuw opstarten wordt gestart als het onderhoudsvenster toestaat.|
   |Altijd opnieuw opstarten|Een systeem opnieuw wordt opgestart, ongeacht of deze vereist is. |
   |Nooit opnieuw opstarten|Als een herstart vereist is, zijn ongeacht opnieuw opstarten onderdrukt.|
   |Alleen opnieuw opstarten - updates worden niet geïnstalleerd|Deze optie wordt genegeerd installatie van updates en initieert alleen opnieuw worden opgestart.|

Wanneer u klaar bent met het plannen, selecteert u de **maken** terug te keren naar het statusdashboard. De **geplande** tabel ziet u de implementatieplanning die u hebt gemaakt.

> [!NOTE]
> Updatebeheer biedt ondersteuning voor het implementeren van eerste partij-updates en het vooraf downloaden van patches. Dit vereist wijzigingen in de systemen die wordt hersteld, Zie [eerste partij en vooraf downloaden ondersteuning](automation-update-management.md#firstparty-predownload) voor meer informatie over deze instellingen configureren op uw systemen.

## <a name="view-results-of-an-update-deployment"></a>Resultaten van een update-implementatie weergeven

Nadat de geplande implementatie is gestart, ziet u de status van deze implementatie op het tabblad **Update-implementaties** onder **Updatebeheer**.

Als de implementatie wordt uitgevoerd, is de status ervan **Actief**. Nadat de implementatie voltooid is, verandert de status in **geslaagd**.

Als een of meer updates in de implementatie zijn mislukt, verandert de status in **Gedeeltelijk mislukt**.

![Status van update-implementatie](./media/manage-update-multi/update-view-results.png)

Klik op de voltooide implementatie om het dashboard voor de betreffende update-implementatie te bekijken.

De **Updateresultaten** deelvenster toont het totale aantal updates en de resultaten van de implementatie voor de virtuele machine. De tabel aan de rechterkant vindt u gedetailleerde informatie van elke update en de resultaten van de installatie. Een van de volgende installatieresultaatwaarden wordt weergegeven:

- **Niet geprobeerd**: De update is niet geïnstalleerd omdat er onvoldoende tijd beschikbaar is op basis van het opgegeven onderhoudsvenster.
- **Geslaagd**: De update is bijgewerkt.
- **Mislukt**: De update is mislukt.

Selecteer **Alle logboeken** voor een overzicht van alle logboekvermeldingen die tijdens de implementatie zijn gemaakt.

Selecteer de tegel uitvoer de taakstroom van het runbook waarmee de update-implementatie op de virtuele doelmachine wordt beheerd.

Selecteer **Fouten** voor gedetailleerde informatie over fouten die zijn opgetreden tijdens de implementatie.

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over updatebeheer, waaronder Logboeken, uitvoer en fouten, [oplossing voor updatebeheer in Azure](../operations-management-suite/oms-solution-update-management.md).
