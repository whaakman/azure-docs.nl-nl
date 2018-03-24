---
title: Met herstelplannen in Azure Site Recovery | Microsoft Docs
description: Meer informatie over in Azure Site Recovery-herstelplannen.
services: site-recovery
documentationcenter: ''
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.date: 03/21/2018
ms.author: raynew
ms.openlocfilehash: 871c9e8404438f966cab2fc5ab782e254295569e
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
# <a name="about-recovery-plans"></a>Over plannen voor herstel

Dit artikel wordt beschreven in de herstelplannen [Azure Site Recovery](site-recovery-overview.md).

Een herstelplan verzamelt machines in recovery groepen. U kunt een plan op te tellen volgorde, instructies en taken. Nadat u een plan is gedefinieerd, kunt u een failover uitvoeren op deze.





## <a name="why-use-a-recovery-plan"></a>Waarom een herstelplan gebruiken?

Een herstelplan helpt u bij het definiëren van een systematische herstelproces door het maken van kleine onafhankelijke eenheden die u kunt een failover uitvoert. Een eenheid vertegenwoordigt doorgaans een app in uw omgeving. Een herstelplan definieert hoe machines failover en de volgorde waarin ze starten na een failover. Plannen voor herstel te gebruiken:

* Het model van een app rond de bijbehorende afhankelijkheden.
* Hersteltaken om te verminderen RTO automatiseren.
- Controleer of dat u bent voorbereid voor migratie of herstel na noodgevallen door ervoor te zorgen dat uw apps deel van een herstelplan uitmaken.
* Testfailover uitvoeren op de herstelplannen, zodat herstel na noodgevallen of migratie werkt zoals verwacht.


## <a name="model-apps"></a>Model-apps

U kunt plannen en maken van een groep voor gegevensherstel voor het vastleggen van app-specifieke eigenschappen. Als u bijvoorbeeld laten we eens een typische toepassing in drie lagen met een SQL server-back-end, middleware en een web-front. Normaal gesproken aanpassen u het herstelplan zodat machines in elke laag in de juiste volgorde worden gestart na een failover.

    - De SQL-back-end moet eerst de middleware beginnen volgende en ten slotte de front-end web.
    - Start de volgorde zorgt ervoor dat de app werkt op het moment dat de laatste machine wordt gestart.
    - Deze volgorde zorgt ervoor dat wanneer de middleware wordt gestart en probeert verbinding maken met de SQL Server-laag, de SQL Server-laag wordt al uitgevoerd. 
    - Deze volgorde zorgt er ook dat de front-end-server laatste begint, zodat eindgebruikers geen verbinding maken met de app-URL voordat alle onderdelen actief zijn en die wordt uitgevoerd en de app is gereed voor het accepteren van aanvragen.

Voor het maken van deze volgorde op groepen toevoegen aan de groep voor gegevensherstel en machines in de groepen toevoegen. 
    - Indien de volgorde is opgegeven, wordt sequentiëren gebruikt. Acties parallel worden uitgevoerd indien van toepassing voor het verbeteren van toepassingsherstel RTO.
    - Computers in één groep een failover parallel.
    - Machines in verschillende groepen failover in Groepsvolgorde, zodat de groep 2 machines hun failover pas beginnen nadat alle machines in groep 1 failover en gestart.

    ![Het herstelplan voorbeeld](./media/recovery-plan-overview/rp.png)

Met deze aanpassing aanwezig is dit wat er gebeurt wanneer u een failover op het herstelplan uitvoeren: 

1. Een tracht afsluiten op de lokale machines uitschakelen. De uitzondering hierop is als u een failovertest uitvoert, in welk geval de primaire site blijft uitvoeren. 
2. Het afsluiten activeert een parallelle failover van alle machines in het herstelplan.
3. De failover wordt voorbereid schijven van de virtuele machine met behulp van gerepliceerde gegevens.
4. Het starten van de groepen in volgorde uitgevoerd en start u de machines in elke groep. Ten eerste groep 1 wordt uitgevoerd, klikt u vervolgens de groep 2 en tot slot, groep 3. Als er meer dan één machine in een groep en alle machines parallel start.


## <a name="automate-tasks"></a>Taken automatiseren

Herstellen van grote toepassingen kan erg ingewikkeld zijn. Handmatige stappen maakt u het proces foutgevoelige klus en de persoon die de failover uitvoert mogelijk niet op de hoogte van alle app-beschrijving. U kunt een herstelplan gebruiken om op te leggen volgorde, en de acties die nodig zijn bij elke stap automatiseren met behulp van Azure Automation-runbooks voor failover naar Azure of scripts. Voor taken die niet automatisch worden uitgevoerd, kunt u gepauzeerd voor handmatige acties invoegen herstelplannen. Er zijn een aantal typen taken die u kunt configureren:

* **Taken op de Azure VM na een failover**: wanneer u bent failover wordt uitgevoerd naar Azure, moet u doorgaans acties uitvoeren, zodat u verbinding met de virtuele machine na een failover maken kunt. Bijvoorbeeld: 
    * Maak een openbaar IP-adres op de Azure VM.
    * Een netwerkbeveiligingsgroep toewijzen aan de netwerkadapter van de Azure VM.
    * Een load balancer toevoegen aan een beschikbaarheidsset.
* **Taken in virtuele machine na een failover**: de app actief op de machine deze taken doorgaans configureren zodat deze correct te laten werken in de nieuwe omgeving blijft. Bijvoorbeeld:
    * Wijzig de databaseverbindingsreeks in de machine.
    * De webserverconfiguratie of regels wijzigen.


## <a name="test-failover"></a>Testfailover

U kunt een plan voor herstel gebruiken voor het activeren van een testfailover. Gebruik de volgende aanbevolen procedures:

- Altijd een testfailover voor een app voltooien voordat u een volledige failover uitvoert. Testfailovers helpen u om te controleren of de app weergegeven op de herstelsite wordt.
- Als u dat u iets hebt gemist vindt, activeert u een schone en voer de testfailover. 
- Een testfailover meerdere keren uitvoeren, totdat u zeker weet dat de app probleemloos herstelt.
- Omdat elke app uniek is, moet u plannen voor herstel die zijn aangepast voor elke toepassing bouwen en een testfailover uitvoeren op elk.
- Apps en de bijbehorende afhankelijkheden regelmatig wordt gewijzigd. Plannen voor herstel zijn bijgewerkt, zodat een testfailover voor elke app uitvoeren elk kwartaal.

    ![Schermafbeelding van een voorbeeld herstelplan testen in Site Recovery](./media/recovery-plan-overview/rptest.png)

## <a name="watch-the-video"></a>Video bekijken

Bekijk een video snel voorbeeld van een failover op en klik op voor een tweelagige WordPress-app wordt weergegeven.
    
> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/One-click-failover-of-a-2-tier-WordPress-application-using-Azure-Site-Recovery/player]



## <a name="next-steps"></a>Volgende stappen

- [Maak](site-recovery-create-recovery-plans.md) een herstelplan.
* Meer informatie over [failovers met](site-recovery-failover.md).  
