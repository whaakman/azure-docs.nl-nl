---
title: Updates voor meerdere virtuele machines in Azure beheren | Microsoft Docs
description: In dit onderwerp wordt beschreven hoe u updates voor virtuele Azure-machines beheert.
services: automation
documentationcenter: 
author: eslesar
manager: carmonm
editor: 
ms.assetid: 
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/31/2017
ms.author: magoedte;eslesar
ms.openlocfilehash: 80a6caff51631637825d560d270198be0336e806
ms.sourcegitcommit: 43c3d0d61c008195a0177ec56bf0795dc103b8fa
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/01/2017
---
# <a name="manage-updates-for-multiple-azure-virtual-machines"></a>Updates voor meerdere virtuele machines in Azure beheren

Met Updatebeheer kunt u updates en patches voor uw virtuele Azure-machines beheren.
Vanuit uw [Azure Automation](automation-offering-get-started.md)-account kunt u virtuele machines snel vrijgeven, de status van de beschikbare updates beoordelen, de installatie van vereiste updates plannen en de implementatieresultaten bekijken om te controleren of de updates zijn toegepast op alle virtuele machines waarvoor Updatebeheer is ingeschakeld.

## <a name="prerequisites"></a>Vereisten

Voor het gebruik van Updatebeheer hebt u het volgende nodig:

* Een Azure Automation-account. Zie [Aan de slag met Azure Automation](automation-offering-get-started.md) voor instructies over het maken van een Azure Automation Uitvoeren-als-account.

* Een virtuele machine of computer met een van de ondersteunde besturingssystemen.

## <a name="supported-operating-systems"></a>Ondersteunde besturingssystemen

Updatebeheer wordt in de volgende besturingssystemen ondersteund.

### <a name="windows"></a>Windows

* Windows Server 2008 en hoger, en de update-implementaties op basis van Windows Server 2008 R2 SP1 en hoger.  Server Core- en nanoserverinstallatieopties worden niet ondersteund.

    > [!NOTE]
    > Ondersteuning voor het implementeren van updates voor Windows Server 2008 R2 SP1 vereist .NET Framework 4.5 en WMF 5.0 of hoger.
    > 
* Windows-clientbesturingssystemen worden niet ondersteund.

Windows-agents moeten worden geconfigureerd om te communiceren met een WSUS-server (Windows Server Update Services of toegang hebben tot Microsoft Update.

> [!NOTE]
> De Windows-agent kan niet tegelijkertijd door System Center Configuration Manager worden beheerd.
>

### <a name="linux"></a>Linux

* CentOS 6 (x86/x64) en 7 (x64)  
* Red Hat Enterprise 6 (x86/x64) en 7 (x64)  
* SUSE Linux Enterprise Server 11 (x86/x64) en 12 (x64)  
* Ubuntu 12.04 LTS en nieuwer x86/x64   

> [!NOTE]  
> Om te voorkomen dat updates buiten een onderhoudsperiode in Ubuntu worden toegepast, moet u het pakket Unattended-Upgrade opnieuw configureren en automatische updates uitschakelen. Zie [het onderwerp Automatic Updates in de Engelstalige Ubuntu Server Guide](https://help.ubuntu.com/lts/serverguide/automatic-updates.html) voor meer informatie.

Linux-agents moeten toegang hebben tot een opslagplaats voor updates.

> [!NOTE]
> OMS-agents voor Linux die zijn geconfigureerd om te rapporteren aan meerdere OMS-werkruimten, worden niet ondersteund in deze oplossing.  
>

## <a name="enable-update-management-for-azure-virtual-machines"></a>Updatebeheer voor virtuele Azure-machines inschakelen

1. Open in Azure Portal het Automation-account.
2. Selecteer aan de linkerkant van het scherm **Updatebeheer**.
3. Klik boven aan het scherm op **Azure-VM toevoegen**.
    ![VM's vrijgeven](./media/manage-update-multi/update-onboard-vm.png)
4. Selecteer een virtuele machine om te onboarden. Het scherm **Updatebeheer inschakelen** wordt weergegeven.
5. Klik op **Inschakelen**.

   ![Updatebeheer inschakelen](./media/manage-update-multi/update-enable.png)

Updatebeheer is ingeschakeld voor uw virtuele machine.

## <a name="enable-update-management-for-non-azure-virtual-machines-and-computers"></a>Updatebeheer voor virtuele machines en computers zonder Azure inschakelen

Zie [Windows-computers verbinden met de Log Analytics-service in Azure](../log-analytics/log-analytics-windows-agents.md) voor instructies over het inschakelen van Updatebeheer voor virtuele Windows-machines en -computers zonder Azure.

Zie [Uw Linux-computers verbinden met Operations Management Suite (OMS)](../log-analytics/log-analytics-agent-linux.md) voor instructies over het inschakelen van Updatebeheer voor virtuele Linux-machines en -computers zonder Azure.

## <a name="view-update-assessment"></a>Update-evaluatie bekijken

Na **Updatebeheer** is ingeschakeld, wordt het scherm **Updatebeheer** weergegeven. U ziet een lijst met ontbrekende updates op het tabblad **Ontbrekende updates**.

## <a name="data-collection"></a>Gegevensverzameling

Agents die zijn geïnstalleerd op virtuele machines en computers verzamelen gegevens over updates en verzenden deze naar Azure Updatebeheer.

### <a name="supported-agents"></a>Ondersteunde agents

De volgende tabel beschrijft de verbonden bronnen die worden ondersteund door deze oplossing.

| Verbonden bron | Ondersteund | Beschrijving |
| --- | --- | --- |
| Windows-agents |Ja |Updatebeheer verzamelt informatie over systeemupdates van Windows-agents en start de installatie van de vereiste updates. |
| Linux-agents |Ja |Updatebeheer verzamelt informatie over systeemupdates van Linux-agents en start de installatie van de vereiste updates op ondersteunde besturingssysteemversies. |
| Beheergroep Operations Manager |Ja |Updatebeheer verzamelt informatie over systeemupdates van agents in een verbonden beheergroep. |
| Azure Storage-account |Nee |Azure Storage bevat geen informatie over systeemupdates. |

### <a name="collection-frequency"></a>Verzamelingsfrequentie

Voor elke beheerde Windows-computer wordt twee keer per dag een scan uitgevoerd. Om de vijftien minuten wordt de Windows-API aangeroepen voor de laatste updatetijd om op die manier te bepalen of de status is gewijzigd. Als dat het geval is, wordt er een nalevingsscan gestart.  Voor elke beheerde Linux-computer wordt elke drie uur een scan uitgevoerd.

Het kan dertig minuten tot zes uur duren voordat er in het dashboard bijgewerkte gegevens worden weergegeven van de beheerder computers.

## <a name="schedule-an-update-deployment"></a>Een update-implementatie plannen

Als u updates wilt installeren, plant u een implementatie na uw release-planning en servicevenster.
U kunt kiezen welke typen updates moeten worden opgenomen in de implementatie. Zo kunt u belangrijke updates of beveiligingsupdates opnemen en updatepakketten uitsluiten.

Plan een nieuwe update-implementatie voor één of meer virtuele machines door te klikken op **Update-implementatie plannen** boven aan het scherm **Updatebeheer**. Geef het volgende op in het scherm **Nieuwe update-implementatie**:

* **Naam**: geef een unieke naam op voor het identificeren van de update-implementatie.
* **Type besturingssysteem**: selecteer Windows of Linux.
* **Bij te werken computers**: selecteer de virtuele machines die u wilt bijwerken.

  ![De virtuele machines die u wilt bijwerken selecteren](./media/manage-update-multi/update-select-computers.png)

* **Updateclassificatie**: selecteer de typen software die de update-implementatie moet opnemen in de implementatie. De classificatietypen zijn:
  * Essentiële updates
  * Beveiligingsupdates
  * Updatepakketten
  * Functiepakketten
  * Servicepacks
  * Definitie-updates
  * Hulpprogramma's
  * Updates
* **Schema-instellingen**: u kunt de standaarddatum en -tijd accepteren (30 minuten na de huidige tijd) of een andere tijd opgeven.
   U kunt ook opgeven of de implementatie eenmaal moet worden uitgevoerd of een planning met meerdere implementaties instellen. Klik op de optie Terugkerend onder Terugkeerpatroon om een terugkerende planning in te stellen.

   ![Scherm met instellingen voor de updateplanning](./media/manage-update-multi/update-set-schedule.png)

* **Onderhoudsvenster (minuten)**: geef op binnen welke periode de update-implementatie moet plaatsvinden.  Dit zorgt ervoor dat wijzigingen worden uitgevoerd binnen de gedefinieerde servicevensters.

Nadat u klaar bent met het configureren van de planning, klikt u op de knop **Maken** en gaat u terug naar het statusdashboard.
U ziet dat het tabblad **Gepland** de implementatieplanning die u zojuist hebt gemaakt weergeeft.

> [!WARNING]
> Voor updates waarvoor opnieuw moet worden opgestart, wordt de virtuele machine automatisch opnieuw opgestart.

## <a name="view-results-of-an-update-deployment"></a>Resultaten van een update-implementatie weergeven

Nadat de geplande implementatie is gestart, ziet u de status van deze implementatie op het tabblad **Update-implementaties** in het scherm **Updatebeheer**.
Als de implementatie momenteel wordt uitgevoerd, wordt de status weergegeven als **Wordt uitgevoerd**. Nadat deze is voltooid, verandert de status in **Geslaagd**.
Als er een fout is opgetreden met één of meer updates in de implementatie, wordt de status **Gedeeltelijk mislukt**.

![Status van update-implementatie ](./media/manage-update-multi/update-view-results.png)

Klik op de voltooide update-implementatie om het dashboard voor de betreffende update-implementatie te bekijken.

In de tegel **Updateresultaten** ziet u een overzicht van het totale aantal updates en van de implementatieresultaten op de virtuele machine.
In de tabel aan de rechterkant vindt u gedetailleerde informatie over elke update en het resultaat van de installatie. Een van de volgende waarden wordt hier weergegeven:

* Niet geprobeerd: de update is niet geïnstalleerd omdat er onvoldoende tijd beschikbaar was op basis van het opgegeven onderhoudsvenster.
* Geslaagd: de update is voltooid
* Mislukt: de update is mislukt

Klik op **Alle logboeken** voor een overzicht van alle logboekvermeldingen die tijdens de implementatie zijn gemaakt.

Klik op de tegel **Uitvoer** om de taakstroom te bekijken van het runbook dat verantwoordelijk is voor het beheren van de implementatie van de update op de virtuele doelmachine.

Klik op **Fouten** voor gedetailleerde informatie over fouten die zijn opgetreden tijdens de implementatie.

Zie [Updatebeheer](../operations-management-suite/oms-solution-update-management.md) voor gedetailleerde informatie over de logboeken, uitvoer en foutinformatie.

## <a name="next-steps"></a>Volgende stappen

* Zie [Updatebeheer](../operations-management-suite/oms-solution-update-management.md) voor meer informatie over Updatebeheer.