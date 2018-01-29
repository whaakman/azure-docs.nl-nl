---
title: Maken en aanpassen voor failover en herstel in de Azure Site Recovery-herstelplannen | Microsoft Docs
description: Hierin wordt beschreven hoe u maken en aanpassen van herstelplannen in Azure Site Recovery, failover en virtuele machines en fysieke servers herstellen
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
ms.openlocfilehash: 9839a989246b28c1a194b8d1f0e99c1bd80ac2e5
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2018
---
# <a name="create-recovery-plans"></a>Herstelplannen maken


Dit artikel bevat informatie over het maken en aanpassen in de herstelplannen [Azure Site Recovery](site-recovery-overview.md).

Eventuele opmerkingen of vragen kunt u onder aan dit artikel plaatsen of op het [Azure Recovery Services-forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

 Maak herstelplannen het volgende doen:

* Groepen van de machines die samen een failover en start vervolgens samen definiëren.
* Model afhankelijkheden tussen machines, door ze te groeperen samen in een recovery plan groep. Bijvoorbeeld, failover en online zetten van een bepaalde toepassing is, groeperen u alle van de virtuele machines voor de toepassing naar dezelfde recovery plan groep.
* Een failover uitvoert. U kunt een test, gepland of niet-geplande failover uitvoeren op een herstelplan.

## <a name="why-use-recovery-plans"></a>Waarom herstelplannen gebruiken?

Plannen voor herstel te plannen voor een systematische herstelproces door het maken van kleine onafhankelijke eenheden die u kunt beheren. Deze eenheden vertegenwoordigen meestal een toepassing in uw omgeving. Plan voor herstel kunt u niet alleen voor het definiëren van de volgorde waarin de virtuele machines starten, maar ook kunt u algemene taken automatiseren tijdens het herstel.


**In wezen een manier om te controleren dat u bent voorbereid voor cloudmigratie of herstel na noodgevallen door ervoor te zorgen dat elke toepassing van jouw e-mailadres onderdeel van een herstelplan is en elk van de herstelplannen wordt getest voor herstel naar Microsoft Azure. Met deze voorbereid, kunt u probleemloos migreren of failover uw volledige datacenter naar Microsoft Azure.**
 
De drie belangrijkste waardevoorstellen van een herstelplan zijn als volgt:

### <a name="model-an-application-to-capture-dependencies"></a>Model van een toepassing voor het vastleggen van afhankelijkheden

Een herstelplan is een groep van virtuele machines in het algemeen die bestaat uit een toepassing die samen failover. Met behulp van het herstelplan constructs, kunt u deze groep om vast te leggen van de eigenschappen van uw toepassingsspecifieke verbeteren.
 
Laat het ons nemen in het voorbeeld van een typische toepassing met drie lagen met

* een SQL-back-end
* een middleware
* een web-frontend

Het herstelplan kan worden aangepast om ervoor te zorgen dat de virtuele machines actief in de juiste volgorde na een failover. De SQL-back-end moet eerst komen en de front-end web moet laatst komen de middleware dient afkomstig te zijn van de volgende. Deze volgorde is bepaald dat de toepassing op het moment dat de laatste virtuele machine wordt werkt weergegeven. Wanneer de middleware voordoet, er wordt geprobeerd verbinding maken met de SQL-laag en het herstelplan ervoor heeft gezorgd dat de SQL-laag wordt al uitgevoerd. Front-endservers afkomstig van de laatste ook zorgt ervoor dat eindgebruikers geen verbinding met de URL van de toepassing per ongeluk totdat alle de onderdelen zijn worden uitgevoerd en de toepassing gereed is om aanvragen te accepteren. Deze afhankelijkheden bouwen, kunt u het herstelplan groepen toe te voegen. Selecteert u een virtuele machine en de groep om deze te verplaatsen tussen groepen wijzigen.

![Voorbeeldplan voor herstel](./media/site-recovery-create-recovery-plans/rp.png)

Nadat u de aanpassingen hebt voltooid, kunt u de exacte stappen van de herstelpunten te visualiseren. Dit is de volgorde van stappen uitgevoerd tijdens de failover van een herstelplan:

* Er is eerst een afgesloten-stap waarmee wordt geprobeerd de virtuele machines on-premises uitschakelen (behalve in testfailover waarin de primaire site moet blijven worden uitgevoerd)
* Naast activeert deze failover van de virtuele machines van het herstelplan parallel. De failover-stap bereidt u de virtuele machines schijven van de gerepliceerde gegevens.
* Ten slotte de opstartgroepen worden uitgevoerd in de volgorde eerst starten van de virtuele machines in elke groep - groep 1 vervolgens groep 2 en tot slot groep 3. Als er meerdere virtuele machines in een groep (bijvoorbeeld een frontend taakverdeling web) zijn allemaal parallel opstarten van.

**Sequentiëren over verschillende groepen zorgt ervoor dat de afhankelijkheden tussen de verschillende toepassingslagen worden gehonoreerd en parallelle uitvoering, indien van toepassing de RTO van een toepassing herstellen verbetert.**

   > [!NOTE]
   > Computers die deel van één groep uitmaken wordt failover parallel. Computers die deel van verschillende groepen uitmaken wordt failover in de aangegeven van de groepen. Nadat alle machines van de groep 1 hebben failover en opgestart, gaat de machines van de groep 2 hun failover.

### <a name="automate-most-recovery-tasks-to-reduce-rto"></a>De meeste hersteltaken om te verminderen RTO automatiseren

Herstellen van grote toepassingen kan erg ingewikkeld zijn. Het is ook moeilijk te onthouden de aanpassing van de exacte stappen na de failover of migratie. Soms is is het niet u, maar iemand anders die niet op de hoogte van de beschrijving toepassing die moet de failover activeren. Te veel handmatige stappen in tijden dank is moeilijk te onthouden en foutgevoelig. Een herstelplan biedt u een manier voor het automatiseren van de vereiste acties die u nemen bij elke stap moet met behulp van Microsoft Azure Automation-runbooks. U kunt algemene hersteltaken, zoals de voorbeelden hieronder automatiseren met runbooks. Voor taken die niet automatisch worden uitgevoerd, bieden herstelplannen u ook de mogelijkheid om in te voegen handmatige acties.

* Taken op de virtuele machine van Azure na de failover: dit zijn vereiste meestal zodat u verbinding met de virtuele machine, bijvoorbeeld maken kunt:
    * Maken van een openbaar IP-adres op de virtuele machine na failover
    * Een NSG toewijzen aan de mislukte via NIC van virtuele machine
    * Een load balancer toevoegen aan een beschikbaarheidsset
* Taken in de virtuele machine na de failover – deze de toepassing configureren zodat de netwerkservices nog steeds correct te laten werken in de nieuwe omgeving, bijvoorbeeld:
    * De databaseverbindingsreeks in de virtuele machine wijzigen
    * Web server/configuratieregels wijzigen

**U kunt met een volledige herstelplan waarmee de taken na herstel met automation-runbooks worden geautomatiseerd, failover met één klik bereiken en de RTO optimaliseren.**

### <a name="test-failover-to-be-ready-for-a-disaster"></a>Testfailover gereed is voor een noodgeval

Een herstelplan kan worden gebruikt voor het activeren van zowel een failover of een testfailover. Een testfailover van de toepassing voordat u een failover uitvoert, moet u altijd uitvoeren. Testfailover helpt u om te controleren of de toepassing wordt actief op de herstelsite.  Als u iets hebt gemist, kunt u eenvoudig opschonen activeren en de testfailover opnieuw. Voer de testfailover meerdere keren totdat u weet met zekerheid dat de toepassing goed wordt hersteld.

![Plan voor herstel van test](./media/site-recovery-create-recovery-plans/rptest.png)

**Elke toepassing verschilt, en moet u plannen voor herstel die zijn aangepast voor elk bouwen. Ook in deze wereld dynamische datacenter voortdurend de toepassingen en de bijbehorende afhankelijkheden gewijzigd. Failover van uw toepassingen één keer per kwartaal om te controleren of het herstelplan huidige testen.**

## <a name="how-to-create-a-recovery-plan"></a>Het maken van een herstelplan

1. Klik op **herstelplannen** > **herstelplan maken**.
   Geef een naam voor het herstelplan en een bron en doel. De bronlocatie moet virtuele machines hebt die zijn ingeschakeld voor failover en herstel. Kies een bron en doel op basis van de virtuele machines die u wilt dat deel uitmaken van het herstelplan. 

   |Scenario                   |Bron               |Doel           |
   |---------------------------|---------------------|-----------------|
   |Azure naar Azure             |Azure-regio         |Azure-regio     |
   |VMware naar Azure            |Configuratieserver |Azure            |
   |VMM naar Azure               |Beschrijvende naam van VMM    |Azure            |
   |Hyper-v-Site naar Azure      |De naam van de Hyper-v-site    |Azure            |
   |Fysieke machines in Azure |Configuratieserver |Azure            |
   |VMM naar VMM                 |Beschrijvende naam van VMM    |Beschrijvende naam van VMM|

   > [!NOTE]
   > Een herstelplan kan virtuele machines met dezelfde bron en doel bevatten. Virtuele machines van VMware en VMM kan geen deel uitmaken van hetzelfde herstelplan wordt uitgevoerd. Virtuele VMware-machines en fysieke machines kunnen echter worden toegevoegd aan hetzelfde abonnement als de bron voor beide parameters een configuratieserver is.

2. In **virtuele machines selecteren**, selecteer de virtuele machines (of replicatiegroep) die u wilt toevoegen aan de standaardgroep (groep 1) in het herstelplan. Alleen de virtuele machines die zijn beveiligd op de bron (als dit is geselecteerd in het herstelplan) en zijn beveiligd met de doelserver (zoals geselecteerd in het herstelplan) kunnen worden geselecteerd.

## <a name="how-to-customize-and-extend-recovery-plans"></a>Het aanpassen en uitbreiden herstelplannen

U kunt aanpassen en uitbreiden van plannen voor herstel door te gaan naar de resourceblade van Site Recovery herstel plannen en te klikken op het tabblad aanpassen.

U kunt aanpassen en uitbreiden herstelplannen:

- **Nieuwe groepen toevoegen**: aanvullende herstelpunten planningsgroepen (maximaal zeven) toevoegen aan de standaardgroep en voeg vervolgens meer machines of replicatiegroepen aan deze herstelgroepen-plan. Groepen worden genummerd in de volgorde waarin u ze toevoegt. Een virtuele machine of de replicatiegroep, kan alleen worden opgenomen in één herstelpunt plan groep.
- **Toevoegen van een handmatige actie**— u handmatige acties die worden uitgevoerd vóór of na een groep voor gegevensherstel abonnement kunt toevoegen. Wanneer het herstelplan wordt uitgevoerd, stopt hij op het punt waar u de handmatige actie ingevoegd. Een dialoogvenster vraagt u om op te geven dat de handmatige actie is voltooid.
- **Een script toevoegen**— u kunt scripts toevoegen die worden uitgevoerd vóór of na een groep voor gegevensherstel plannen. Wanneer u een script toevoegt, wordt een nieuwe reeks handelingen voor de groep toegevoegd. Bijvoorbeeld, een reeks vooraf stappen voor het groep 1 wordt gemaakt met de naam: groep 1: vooraf stappen. Alle vooraf stappen worden vermeld in deze set. U kunt alleen een script toevoegen op de primaire site hebt u een VMM-server geïmplementeerd. [Meer informatie](site-recovery-how-to-add-vmmscript.md).
- **Toevoegen van Azure-runbooks**— u kunt herstelplannen met Azure runbooks uitbreiden. Bijvoorbeeld, om taken te automatiseren, of één stap herstel maken. [Meer informatie](site-recovery-runbook-automation.md).


## <a name="how-to-add-a-script-runbook-or-manual-action-to-a-plan"></a>Een script, het runbook of handmatige actie toevoegen aan een plan

U kunt een script of een handmatige actie toevoegen aan de standaardgroep voor herstel plan nadat u hebt toegevoegd virtuele machines of replicatiegroepen en het plan gemaakt.

1. Open het herstelplan.
2. Klik op een item in de **stap** lijst en klik vervolgens op **Script** of **handmatige actie**.
3. Geef op of het script of actie vóór of na het geselecteerde item wilt toevoegen. U kunt de positie van het script omhoog of omlaag verplaatsen met de **omhoog** en **omlaag** knoppen.
4. Als u een VMM-script toevoegen, selecteert u **Failover naar VMM script**. In **scriptpad**, typ het relatieve pad naar de share. In het onderstaande voorbeeld VMM geeft u het pad: **\RPScripts\RPScript.PS1**.
5. Als u een Azure automation-boek uitvoeren toevoegt, geef de Azure Automation-account waarin het runbook zich bevindt en het juiste Azure runbookscript te selecteren.
6. Om te zorgen dat het script werkt zoals verwacht, doet u een failover van het herstelplan.

De script- of runbook-opties zijn alleen beschikbaar in de volgende scenario's bij het uitvoeren van een failover en failback. Een handmatige actie is beschikbaar voor failover en failback.


|Scenario               |Failover |Failback |
|-----------------------|---------|---------|
|Azure naar Azure         |Runbooks |Runbook  |
|VMware naar Azure        |Runbooks |N.v.t.       | 
|VMM naar Azure           |Runbooks |Script   |
|Hyper-v-site naar Azure  |Runbooks |N.v.t.       |
|VMM naar VMM             |Script   |Script   |


## <a name="next-steps"></a>Volgende stappen

[Meer informatie](site-recovery-failover.md) over het uitvoeren van failovers.

Bekijk deze video om te zien van het herstel van plan bent in te grijpen.

> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/One-click-failover-of-a-2-tier-WordPress-application-using-Azure-Site-Recovery/player]
