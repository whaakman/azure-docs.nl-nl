---
title: Updates voor meerdere virtuele machines in Azure beheren | Microsoft Docs
description: In dit onderwerp wordt beschreven hoe u updates voor virtuele Azure-machines beheert.
services: automation
documentationcenter: 
author: georgewallace
manager: carmonm
editor: 
ms.assetid: 
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/30/2017
ms.author: magoedte;gwallace
ms.openlocfilehash: 1763077aa733fc93dd59147405db9942c6c98960
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2018
---
# <a name="manage-updates-for-multiple-machines"></a>Updates voor meerdere machines beheren

Met Updatebeheer kunt u updates en patches voor uw virtuele machines van Windows en Linux beheren. Vanuit uw [Azure Automation](automation-offering-get-started.md)-account, kunt u het volgende doen:

- Virtuele machines activeren.
- De status van beschikbare updates evalueren.
- De installatie van vereiste updates plannen.
- Implementatieresultaten weergeven om te controleren of updates met succes zijn toegepast op alle virtuele machines waarvoor Updatebeheer is ingeschakeld.

## <a name="prerequisites"></a>Vereisten

Voor het gebruik van Updatebeheer hebt u het volgende nodig:

* Een uitvoeren als-account van Azure Automation. Zie [Aan de slag met Azure Automation](automation-offering-get-started.md) voor instructies voor het maken van een dergelijk account.

* Een virtuele machine of computer met een van de ondersteunde besturingssystemen.

## <a name="supported-operating-systems"></a>Ondersteunde besturingssystemen

Updatebeheer wordt in de volgende besturingssystemen ondersteund.

### <a name="windows"></a>Windows

* Windows Server 2008 en hoger, en de update-implementaties op basis van Windows Server 2008 R2 SP1 en hoger. Nano Server wordt niet ondersteund.

  Ondersteuning voor het implementeren van updates voor Windows Server 2008 R2 SP1 vereist .NET Framework 4.5 en Windows Management Framework 5.0 of hoger.

* Windows-clientbesturingssystemen worden niet ondersteund.

Windows-agents moeten worden geconfigureerd om te communiceren met een WSUS-server (Windows Server Update Services of toegang hebben tot Microsoft Update.

> [!NOTE]
> De Windows-agent kan niet tegelijkertijd door System Center Configuration Manager worden beheerd.
>

### <a name="linux"></a>Linux

* CentOS 6 (x86/x64) en 7 (x64)  
* Red Hat Enterprise 6 (x86/x64) en 7 (x64)  
* SUSE Linux Enterprise Server 11 (x86/x64) en 12 (x64)  
* Ubuntu 12.04 LTS en nieuwer (x86/x64)   

> [!NOTE]  
> Om te voorkomen dat updates buiten een onderhoudsperiode in Ubuntu worden toegepast, moet u het pakket Unattended-Upgrade opnieuw configureren en automatische updates uitschakelen. Zie het onderwerp [Automatic Updates](https://help.ubuntu.com/lts/serverguide/automatic-updates.html) (Automatische updates) in de Ubuntu Server Guide voor meer informatie.

Linux-agents moeten toegang hebben tot een opslagplaats voor updates.

Deze oplossing biedt geen ondersteuning voor een OMS-Agent voor Linux die is geconfigureerd voor rapportage aan meerdere Operations Management Suite-werkruimten.

## <a name="enable-update-management-for-azure-virtual-machines"></a>Updatebeheer voor virtuele Azure-machines inschakelen

1. Open in Azure Portal het Automation-account.
2. Selecteer **Updatebeheer** in het linkerdeelvenster.
3. Selecteer bovenaan het venster de optie **Azure-VM toevoegen**.
   ![Tabblad Azure-VM toevoegen](./media/manage-update-multi/update-onboard-vm.png)
4. Selecteer een virtuele machine om te onboarden. Het dialoogvenster **Updatebeheer inschakelen** wordt weergegeven.
5. Selecteer **Inschakelen**.

   ![Dialoogvenster Updatebeheer inschakelen](./media/manage-update-multi/update-enable.png)

Updatebeheer is ingeschakeld voor uw virtuele machine.

## <a name="enable-update-management-for-non-azure-virtual-machines-and-computers"></a>Updatebeheer voor virtuele machines en computers zonder Azure inschakelen

Zie [Windows-computers verbinden met de Log Analytics-service in Azure](../log-analytics/log-analytics-windows-agent.md) voor instructies over het inschakelen van Updatebeheer voor virtuele Windows-machines en -computers zonder Azure.

Zie [Uw Linux-computers verbinden met Log Analytics](../log-analytics/log-analytics-agent-linux.md) voor instructies over het inschakelen van Updatebeheer voor virtuele Linux-machines en -computers zonder Azure.

## <a name="view-computers-attached-to-your-automation-account"></a>Computers weergeven die zijn gekoppeld aan uw Automation-account
Nadat updatebeheer is ingeschakeld voor uw computers, kunt u de computergegevens bekijken door te klikken op **Computers**. Beschikbare computergegevens zijn: *Naam*, *Naleving*, *Omgeving*, *Type besturingssysteem*, *Essentiële updates en beveiligingsupdates*  en *Andere updates*. 

  ![Tabblad Computers weergeven](./media/manage-update-multi/update-computers-tab.png)

Computers waarvoor updatebeheer recent is ingeschakeld, zijn nog niet beoordeeld. De nalevingsstatus voor deze computers is dan: *Niet beoordeeld*.  Hier volgt een lijst met waarden voor de nalevingsstatus:
* Compatibel: Computers met ontbrekende essentiële updates of beveiligingsupdates.
* Niet-compatibel: Computers met minstens één ontbrekende essentiële update of beveiligingsupdate.
* Niet beoordeeld: De beoordelingsgegevens voor de update zijn niet binnen het verwachte tijdsbestek ontvangen vanaf de computer.  Voor Linux-computers is dat in de afgelopen drie uur en voor Windows-computers in de afgelopen 12 uur.  

## <a name="view-an-update-assessment"></a>Een update-evaluatie bekijken

Nadat **Updatebeheer** is ingeschakeld, wordt het dialoogvenster Updatebeheer weergegeven. U ziet een lijst met ontbrekende updates op het tabblad **Ontbrekende updates**.

## <a name="collect-data"></a>Gegevens verzamelen

Agents die zijn geïnstalleerd op virtuele machines en computers verzamelen gegevens over updates en verzenden deze naar Azure Updatebeheer.

### <a name="supported-agents"></a>Ondersteunde agents

De volgende tabel beschrijft de verbonden bronnen die worden ondersteund door deze oplossing:

| Verbonden bron | Ondersteund | Beschrijving |
| --- | --- | --- |
| Windows-agents |Ja |Updatebeheer verzamelt informatie over systeemupdates van Windows-agents en start de installatie van de vereiste updates. |
| Linux-agents |Ja |Updatebeheer verzamelt informatie over systeemupdates van Linux-agents en start de installatie van de vereiste updates op ondersteunde besturingssysteemversies. |
| Beheergroep Operations Manager |Ja |Updatebeheer verzamelt informatie over systeemupdates van agents in een verbonden beheergroep. |
| Azure Storage-account |Nee |Azure Storage bevat geen informatie over systeemupdates. |

### <a name="collection-frequency"></a>Verzamelingsfrequentie

Voor elke beheerde Windows-computer wordt twee keer per dag een scan uitgevoerd. Om de vijftien minuten wordt de Windows-API aangeroepen voor de laatste updatetijd om te bepalen of de status is gewijzigd. Als dat het geval is, wordt er een nalevingsscan gestart. Voor elke beheerde Linux-computer wordt elke drie uur een scan uitgevoerd.

Het kan dertig minuten tot zes uur duren voordat er in het dashboard bijgewerkte gegevens van beheerde computers worden weergegeven.

## <a name="schedule-an-update-deployment"></a>Een update-implementatie plannen

Als u updates wilt installeren, plant u een implementatie na uw release-planning en servicevenster.
U kunt kiezen welke typen updates moeten worden opgenomen in de implementatie. Zo kunt u belangrijke updates of beveiligingsupdates opnemen en updatepakketten uitsluiten.

Plan een nieuwe update-implementatie voor een of meer virtuele machines door **Update-implementatie plannen** te selecteren bovenaan het dialoogvenster **Updatebeheer**. Geef het volgende op in het deelvenster **Nieuwe update-implementatie**:

* **Naam**: Geef een unieke naam op voor het identificeren van de update-implementatie.
* **Type besturingssysteem**: Selecteer Windows of Linux.
* **Bij te werken computers**: Selecteer de virtuele machines die u wilt bijwerken.

  ![Deelvenster Nieuwe update-implementatie](./media/manage-update-multi/update-select-computers.png)

* **Updateclassificatie**: Selecteer de typen software die u in de update-implementatie wilt opnemen. De classificatietypen zijn:
  * Essentiële updates
  * Beveiligingsupdates
  * Updatepakketten
  * Functiepakketten
  * Servicepacks
  * Definitie-updates
  * Hulpprogramma's
  * Updates
* **Schema-instellingen**: U kunt de standaarddatum en -tijd accepteren (30 minuten na de huidige tijd). U kunt ook een andere tijd opgeven.
   U kunt bovendien opgeven of de implementatie eenmaal moet worden uitgevoerd of volgens een terugkerend schema. Klik op de optie **Terugkerend** onder **Terugkeerpatroon** als u een terugkerend schema wilt instellen.

   ![Dialoogvenster Schema-instellingen](./media/manage-update-multi/update-set-schedule.png)

* **Onderhoudsvenster (minuten)**: Geef op binnen welke periode de update-implementatie moet plaatsvinden. Met deze instelling zorgt u ervoor dat wijzigingen worden uitgevoerd binnen de gedefinieerde onderhoudsvensters.

Als u klaar bent met het configureren van het schema, klikt u op de knop **Maken** en gaat u terug naar het statusdashboard. De zojuist gemaakte implementatieplanning wordt weergegeven in de tabel **Gepland**.

> [!WARNING]
> Voor updates waarvoor opnieuw moet worden opgestart, wordt de virtuele machine automatisch opnieuw opgestart.

## <a name="view-results-of-an-update-deployment"></a>Resultaten van een update-implementatie weergeven

Nadat de geplande implementatie is gestart, ziet u de status van deze implementatie op het tabblad **Update-implementaties** in het dialoogvenster **Updatebeheer**.
Als de implementatie wordt uitgevoerd, is de status ervan **Actief**. Als de implementatie is voltooid, verandert deze in **Geslaagd**.
Als een of meer updates in de implementatie zijn mislukt, verandert de status in **Gedeeltelijk mislukt**.

![Status van update-implementatie](./media/manage-update-multi/update-view-results.png)

Klik op de voltooide implementatie om het dashboard voor de betreffende update-implementatie te bekijken.

In het deelvenster **Updateresultaten** ziet u een overzicht van het totale aantal updates en van de implementatieresultaten op de virtuele machine.
In de tabel aan de rechterkant vindt u gedetailleerde informatie over elke update en het resultaat van de installatie. Een van de volgende installatieresultaatwaarden wordt weergegeven:

* Niet geprobeerd: de update is niet geïnstalleerd omdat er onvoldoende tijd beschikbaar was op basis van het opgegeven onderhoudsvenster.
* Geslaagd: de update is voltooid.
* Mislukt: de update is mislukt.

Selecteer **Alle logboeken** voor een overzicht van alle logboekvermeldingen die tijdens de implementatie zijn gemaakt.

Selecteer de tegel **Uitvoer** om de taakstroom te bekijken van het runbook waarmee de update-implementatie op de virtuele doelmachine wordt beheerd.

Selecteer **Fouten** voor gedetailleerde informatie over fouten die zijn opgetreden tijdens de implementatie.

## <a name="next-steps"></a>Volgende stappen

* Zie [Oplossing Updatebeheer in OMS](../operations-management-suite/oms-solution-update-management.md) voor meer informatie over Updatebeheer, waaronder logboeken, uitvoer en fouten.

