---
title: Maken en aanpassen van een herstelplan voor failover en herstel in de Azure Site Recovery | Microsoft Docs
description: Informatie over het maken en aanpassen in Azure Site Recovery-herstelplannen. In dit artikel wordt beschreven hoe een failover en herstel van virtuele machines en fysieke servers.
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 72408c62-fcb6-4ee2-8ff5-cab1218773f2
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 01/26/2018
ms.author: raynew
ms.openlocfilehash: 6ad82a8a2f8e16ab794ba90c109904bd45cb6b89
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2018
---
# <a name="create-a-recovery-plan-by-using-site-recovery"></a>Een herstelplan maken met behulp van Site Recovery

Dit artikel wordt beschreven hoe u maken en aanpassen van een herstelplan in [Azure Site Recovery](site-recovery-overview.md).

Maak een herstelplan het volgende doen:

* Groepen machines die samen een failover en start vervolgens samen definiëren.
* Model afhankelijkheden tussen machines door ze te groeperen samen in een groep voor gegevensherstel plannen. Voor failover en online zetten van een specifieke toepassing, bijvoorbeeld alle virtuele machines voor die toepassing in dezelfde recovery plan groep.
* Een failover uitvoert. U kunt een test, gepland of niet-geplande failover uitvoeren op een herstelplan.

## <a name="why-use-a-recovery-plan"></a>Waarom een herstelplan gebruiken

Een herstelplan kan helpen bij het voorbereiden van een systematische herstelproces door het maken van kleine onafhankelijke eenheden die u kunt beheren. De eenheden vertegenwoordigen meestal een toepassing in uw omgeving. Een herstelplan kunt u de volgorde waarin de virtuele machines starten definiëren. U kunt ook een herstelplan gebruiken voor het automatiseren van algemene taken tijdens het herstel.

> [!TIP]
> Een manier om te controleren of u bent voorbereid voor cloud migratie of herstel na noodgevallen is om ervoor te zorgen dat elke toepassing deel van een herstelplan uitmaakt. Controleer ook of elke herstelplan wordt getest voor herstel naar Azure. Met deze voorbereid, kunt u probleemloos migreren of failover van uw volledige datacenter naar Azure.
 
Een herstelplan heeft drie belangrijkste waardevoorstellen:

* Het model van een toepassing voor het vastleggen van afhankelijkheden.
* De meeste hersteltaken om te verminderen RTO automatiseren.
* Testfailover gereed is voor een noodgeval.

### <a name="model-an-application-to-capture-dependencies"></a>Model van een toepassing voor het vastleggen van afhankelijkheden

Een herstelplan is een groep van virtuele machines die in het algemeen bestaan uit een toepassing en die samen een failover. Met behulp van herstelplan constructs, kunt u een groep voor gegevensherstel plan voor het vastleggen van de eigenschappen van uw toepassingsspecifieke verbeteren. 

In dit artikel gebruiken we een typische toepassing in drie lagen die mogelijk een back-end, middleware en een webfront-end SQL. U kunt het herstelplan om ervoor te zorgen dat de virtuele machines in de juiste volgorde na een failover gestart. De SQL-back-end moet eerst worden gestart, de middleware volgende moet beginnen en de webfront-end laatste moet beginnen. Deze volgorde zorgt ervoor dat de toepassing werkt op het moment dat de laatste virtuele machine wordt gestart. 

Bijvoorbeeld, wanneer de middleware wordt gestart, wordt geprobeerd verbinding maken met de SQL-laag. Het herstelplan zorgt ervoor dat de SQL-laag wordt al uitgevoerd. Met een front-endserver start laatste er ook zorgt dat eindgebruikers geen verbinding met de URL van de toepassing maken voordat alle onderdelen actief zijn en die wordt uitgevoerd en de toepassing is gereed om aanvragen te accepteren. Als u wilt deze afhankelijkheden maken, aanpassen van het herstelplan groepen toevoegen en selecteer vervolgens een virtuele machine. Een virtuele machine te verplaatsen tussen groepen, moet u de virtuele machine groep wijzigen.

![Schermafbeelding van een herstelplan voorbeeld in Site Recovery](./media/site-recovery-create-recovery-plans/rp.png)

Nadat u de aanpassingen hebt voltooid, kunt u de exacte stappen van de herstelpunten te visualiseren. Dit is de volgorde van stappen die worden uitgevoerd tijdens de failover van een herstelplan:

1. Een stap afsluiten probeert de virtuele machines on-premises uitschakelen. (Behalve in een testfailover gedurende welke de primaire site moet worden uitgevoerd).
2. De poging afsluiten activeert failover van de virtuele machines in het herstelplan parallel. De failover-stap bereidt de schijven voor virtuele machine met behulp van gerepliceerde gegevens.
3. Het starten van de groepen worden uitgevoerd in de volgorde en start de virtuele machines in elke groep. Ten eerste groep 1 wordt uitgevoerd, en vervolgens groep 2 wordt uitgevoerd en ten slotte, groep 3 wordt uitgevoerd. Als er meer dan één virtuele machine in een groep (bijvoorbeeld een taakverdeling webfront-end), worden alle virtuele machines starten parallel.

> [!TIP]
> Sequentiëren over verschillende groepen zorgt ervoor dat de afhankelijkheden tussen de verschillende toepassingslagen worden gehonoreerd. Parallelle uitvoering, verbetert indien van toepassing moet worden gebruikt, de RTO herstel van de toepassing.

   > [!NOTE]
   > Machines die deel van één groep uitmaken failover parallel. Machines die deel van verschillende groepen uitmaken failover in de volgorde van de groepen. De machines van de groep 2 begint hun failover pas nadat alle machines van de groep 1 failover en gestart.

### <a name="automate-most-recovery-tasks-to-reduce-rto"></a>De meeste hersteltaken om te verminderen RTO automatiseren

Herstellen van grote toepassingen kan erg ingewikkeld zijn. Met veel handmatige stappen om te onthouden in tijden dank is moeilijk en foutgevoelige klus maakt van het proces. Bovendien kan dit worden iemand die niet op de hoogte van de toepassing gecompliceerde aspecten die daadwerkelijk activeert de failover. 

U kunt een plan voor herstel gebruiken voor het automatiseren van de vereiste acties die u moet nemen bij elke stap. U kunt de vereiste acties instellen met behulp van Azure Automation-runbooks. U kunt algemene taken voor herstel, zoals de taken in de volgende voorbeelden kunt automatiseren met runbooks. Voor taken die niet automatisch worden uitgevoerd, kunt u handmatige acties invoegen in uw plannen voor herstel.

* **Taken op de virtuele machine van Azure na een failover**: deze taken zijn doorgaans kunnen verbinding maken met de virtuele machine vereist. Voorbeelden:
    * Maak een openbaar IP-adres op de virtuele machine na een failover.
    * Een netwerkbeveiligingsgroep toewijzen aan de Netwerkadapter op de virtuele machine van failover.
    * Een load balancer toevoegen aan een beschikbaarheidsset.
* **Taken in de virtuele machine na een failover**: de toepassing van deze taken opnieuw configureren zodat deze correct te laten werken in de nieuwe omgeving blijft. Voorbeelden:
    * Wijzig de databaseverbindingsreeks in de virtuele machine.
    * De webserverconfiguratie of regels wijzigen.

> [!TIP]
> Failover met één klik bereiken en RTO optimaliseren door het maken van een volledige herstelplan waarmee taken na herstel met behulp van Automation-runbooks worden geautomatiseerd.

### <a name="test-failover-to-be-ready-for-a-disaster"></a>Testfailover gereed is voor een noodgeval

U kunt een plan voor herstel gebruiken voor het activeren van een failover of een testfailover. Voltooi altijd een testfailover van de toepassing voordat u een failover uitvoert. Test de failovers help die u controleren of de toepassing weergegeven op de herstelsite wordt. Als u iets in uw installatie gemist hebt, kunt u eenvoudig activeren opruimen en vervolgens opnieuw de testfailover. Voer de testfailover meerdere keren totdat u er zeker van te zijn dat de toepassing zonder problemen herstelt.

![Schermafbeelding van een voorbeeld herstelplan testen in Site Recovery](./media/site-recovery-create-recovery-plans/rptest.png)

> [!TIP]
> Omdat elke toepassing uniek is, moet u plannen voor herstel die zijn aangepast voor elke toepassing bouwen. 
>
> Ook in de huidige dynamische, datacenter gerichte omgeving wijzigen toepassingen en afhankelijkheden zijn vaak. Om ervoor te zorgen dat het herstelplan actueel is, failover testen voor uw toepassingen elk kwartaal.

## <a name="create-a-recovery-plan"></a>Een herstelplan maken

1. Selecteer **herstelplannen** > **herstelplan maken**.
   Geef een naam voor het herstelplan en een bron en doel. De bronlocatie moet virtuele machines hebt die zijn ingeschakeld voor failover en herstel. Kies een bron en doel op basis van de virtuele machines die u wilt dat deel uitmaken van het herstelplan. 

   |Scenario                   |Bron               |Doel           |
   |---------------------------|---------------------|-----------------|
   |Azure naar Azure             |Azure-regio         |Azure-regio     |
   |VMware naar Azure            |Configuratieserver |Azure            |
   |Virtual Machine Manager (VMM) naar Azure               |Weergavenaam van VMM    |Azure            |
   |Hyper-V-site naar Azure      |De naam van de Hyper-V-site    |Azure            |
   |Fysieke machines in Azure |Configuratieserver |Azure            |
   |VMM naar VMM                 |Beschrijvende naam van VMM    |Weergavenaam van VMM|

   > [!NOTE]
   > Een herstelplan kan virtuele machines met dezelfde bron en doel bevatten. Virtuele machines van VMware en System Center Virtual Machine Manager (VMM) kan niet in hetzelfde herstelplan wordt uitgevoerd. U kunt echter virtuele VMware-machines en fysieke machines toevoegen aan hetzelfde herstelplan wordt uitgevoerd. In dit geval wordt is de bron voor beide computers een configuratieserver.

2. Onder **virtuele machines selecteren**, selecteer de virtuele machines (of replicatiegroep) die u wilt toevoegen aan de standaardgroep (groep 1) in het herstelplan. U kunt alleen virtuele machines die zijn beveiligd op de bron (zoals geselecteerd in het herstelplan) en die zijn beveiligd op de doelserver (zoals geselecteerd in het herstelplan) selecteren.

## <a name="customize-and-extend-recovery-plans"></a>Aanpassen en uitbreiden herstelplannen

Als u wilt aanpassen en uitbreiden van de herstelplannen, gaat u naar het deelvenster met Site Recovery herstel plan-resource. Selecteer de **aanpassen** tabblad. U kunt aanpassen en uitbreiden van herstelplannen met behulp van de volgende opties:

- **Nieuwe groepen toevoegen**: U kunt maximaal zeven aanvullende herstelpunten planningsgroepen toevoegen aan de standaardgroep. Vervolgens kunt u meer machines of replicatiegroepen toevoegen aan deze herstelgroepen-plan. Groepen worden genummerd in de volgorde waarin u ze toevoegt. U kunt een virtuele machine of de replicatiegroep, in slechts één recovery plan groep opnemen.
- **Toevoegen van een handmatige actie**: U handmatige acties die worden uitgevoerd vóór of na een groep voor gegevensherstel abonnement kunt toevoegen. Wanneer het herstelplan wordt uitgevoerd, stopt hij op het punt waar u de handmatige actie ingevoegd. Een dialoogvenster vraagt u om op te geven dat de handmatige actie is voltooid.
- **Een script toevoegen**: U kunt scripts toevoegen die worden uitgevoerd vóór of na een groep voor gegevensherstel plannen. Wanneer u een script toevoegt, wordt een nieuwe reeks handelingen voor de groep toegevoegd. Bijvoorbeeld, een reeks vooraf stappen voor het groep 1 is gemaakt met de naam *groep 1: vooraf stappen*. Alle vooraf stappen worden vermeld in deze set. U kunt een script toevoegen op de primaire site alleen als u een VMM-server geïmplementeerd. Zie voor meer informatie [een VMM-script toevoegen aan een herstelplan](site-recovery-how-to-add-vmmscript.md).
- **Toevoegen van Azure-runbooks**: U kunt herstelplannen uitbreiden met behulp van Azure-runbooks. U kunt bijvoorbeeld een runbook gebruiken om taken te automatiseren, of één stap herstel maken. Zie voor meer informatie [toevoegen Azure Automation-runbooks aan herstelplannen](site-recovery-runbook-automation.md).


## <a name="add-a-script-runbook-or-manual-action-to-a-plan"></a>Een script, het runbook of de handmatige actie toevoegen aan een plan

Nadat u virtuele machines of replicatiegroepen aan een standaardgroep recovery-abonnement hebt toegevoegd, kunt u een script of een handmatige actie toevoegen aan de groep voor gegevensherstel plannen.

1. Open het herstelplan.
2. In de **stap** , selecteert u een item. Selecteer **Script** of **handmatige actie**.
3. Geef op of u wilt toevoegen voor het script of actie vóór of na het geselecteerde item. Als u wilt de positie van het script omhoog of omlaag verplaatsen, selecteert u de **omhoog** of **omlaag** knoppen.
4. Als u een VMM-script toevoegen, selecteert u **Failover naar VMM script**. In **scriptpad**, voert u het relatieve pad naar de share. Bijvoorbeeld: **\RPScripts\RPScript.PS1**.
5. Als u een Automation-runbook toevoegt, geeft u het Automation-account waarin het runbook zich bevindt. Selecteer vervolgens het runbookscript Azure.
6. Voer een failover van herstelplan om ervoor te zorgen dat het script werkt zoals verwacht.

Script of runbook-opties zijn alleen beschikbaar in de volgende scenario's, en tijdens een failover en failback. Een handmatige actie is beschikbaar voor failover en failback.


|Scenario               |Failover |Failback |
|-----------------------|---------|---------|
|Azure naar Azure         |Runbook |Runbook  |
|VMware naar Azure        |Runbook |N.v.t.       | 
|VMM naar Azure           |Runbook |Script   |
|Hyper-V-site naar Azure  |Runbook |N.v.t.       |
|VMM naar VMM             |Script   |Script   |


## <a name="next-steps"></a>Volgende stappen

* Meer informatie over [failovers met](site-recovery-failover.md).  
* Zie deze video van het herstel van plan bent in actie:
    
    > [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/One-click-failover-of-a-2-tier-WordPress-application-using-Azure-Site-Recovery/player]
