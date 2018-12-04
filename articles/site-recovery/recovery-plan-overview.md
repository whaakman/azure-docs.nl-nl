---
title: Met behulp van herstelplannen in herstel na noodgeval met Azure Site Recovery | Microsoft Docs
description: Meer informatie over het gebruik van plannen voor herstel voor herstel na noodgeval met de Azure Site Recovery-service.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: article
ms.date: 11/27/2018
ms.author: raynew
ms.openlocfilehash: cb68b71eece998fa72fccc00de45f81e6d2d778c
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52848388"
---
# <a name="about-recovery-plans"></a>Over herstelplannen

Dit artikel wordt beschreven in de herstelplannen [Azure Site Recovery](site-recovery-overview.md).

Een herstelplan verzamelt machines in herstelgroepen. U kunt een abonnement aanpassen door de volgorde, instructies en taken aan toe te voegen. Nadat een abonnement is gedefinieerd, kunt u een failover uitvoert op deze.



## <a name="why-use-a-recovery-plan"></a>Waarom zou ik een herstelplan te gaan gebruiken?

Een herstelplan helpt u bij het definiëren van een systematische herstelproces door het maken van kleine, onafhankelijke eenheden die u voor een failover. Een eenheid vertegenwoordigt doorgaans een app in uw omgeving. Een herstelplan definieert hoe failover van machines en de volgorde waarin ze na een failover start. Plannen voor herstel te gebruiken:

* Het model van een app om de afhankelijkheden ervan.
* Hersteltaken om te beperken RTO automatiseren.
- Controleert u of u bent voorbereid voor migratie of herstel na noodgevallen door ervoor te zorgen dat uw apps deel uit van een plan voor herstel maken.
* Testfailover uitvoert op plannen voor herstel, om ervoor te zorgen voor herstel na noodgevallen of de migratie naar behoren werkt.


## <a name="model-apps"></a>Model-apps

U kunt plannen en maken van een groep voor gegevensherstel om vast te leggen van de app-specifieke eigenschappen. Als een voorbeeld laten we eens een typische toepassing met drie lagen met een SQL server-back-end, middleware en een web-front. Normaal gesproken aanpassen u het herstelplan te gaan zodat de machines in elke laag in de juiste volgorde worden gestart na een failover.

    - De SQL-back-end moet eerst de middleware beginnen volgende en ten slotte de webfrontend.
    - Deze volgorde start zorgt ervoor dat de app werkt op het moment dat de laatste machine wordt gestart.
    - Deze volgorde zorgt ervoor dat wanneer de middleware wordt gestart en probeert verbinding maken met de SQL Server-laag, de SQL Server-laag wordt al uitgevoerd. 
    - Deze volgorde zorgt er ook dat de front-endwebserver laatste is gestart, zodat eindgebruikers geen verbinding maken met de URL van de app voordat alle onderdelen zijn en die wordt uitgevoerd en de app is gereed om aanvragen te accepteren.

Voor het maken van deze volgorde, kunt u groepen toevoegen aan de groep voor gegevensherstel en computers in de groepen toevoegen. 
    - Wanneer de order is opgegeven, wordt sequentiëren gebruikt. Acties parallel worden uitgevoerd indien van toepassing, voor het verbeteren van RTO-toepassing herstellen.
    - Computers in één groep failover parallel.
    - In verschillende groepen failover van machines in de volgorde van groepen, zodat de machines groep 2 hun failover start pas nadat alle machines in de groep 1 hebben een failover en gestart.

    ![Voorbeeld van herstelplan](./media/recovery-plan-overview/rp.png)

Met deze aanpassingen op locatie is dit wat er gebeurt wanneer u een failover op het herstelplan te gaan uitvoert: 

1. Een afsluiten tracht de on-premises machines uitschakelen. De uitzondering hierop is als u een failovertest uitvoert, in welk geval de primaire site blijft om uit te voeren. 
2. De afsluiting activeert een parallelle failover van alle machines in het herstelplan te gaan.
3. De failover wordt voorbereid schijven van virtuele machines met behulp van gerepliceerde gegevens.
4. De opstartgroepen in volgorde worden uitgevoerd en start u de machines in elke groep. Ten eerste groep 1 wordt uitgevoerd, klikt u vervolgens groep 2 en ten slotte, groep 3. Als er meer dan één machine in een groep, klikt u vervolgens starten alle machines parallel.


## <a name="automate-tasks"></a>Taken automatiseren

Herstellen van grote toepassingen, kan een complexe taak zijn. Handmatige stappen maakt u het proces foutgevoelig, en de persoon die uitgevoerd van de failover mogelijk niet op de hoogte van alle complexiteit van de app. U kunt een plan voor herstel gebruiken om op te leggen volgorde, en de acties die nodig zijn bij elke stap automatiseren met behulp van Azure Automation-runbooks voor failover naar Azure of scripts. U kunt pauzes voor handmatige acties voor taken die niet kunnen worden geautomatiseerd, invoegen in plannen voor herstel. Er zijn een aantal typen taken die u kunt configureren:

* **Taken op de virtuele Azure-machine na een failover**: wanneer u bent Failover-overschakeling uitvoeren naar Azure, moet u doorgaans acties uitvoeren, zodat u verbinding met de virtuele machine na een failover maken kunt. Bijvoorbeeld: 
    * Maak een openbaar IP-adres op de virtuele machine van Azure.
    * Een netwerkbeveiligingsgroep toewijzen aan de netwerkadapter van de Azure-VM.
    * Een load balancer toevoegen aan een beschikbaarheidsset.
* **Taken binnen de virtuele machine na een failover**: deze taken doorgaans de app die wordt uitgevoerd op de machine opnieuw configureren zodat deze correct te laten werken in de nieuwe omgeving blijft. Bijvoorbeeld:
    * Wijzig de databaseverbindingsreeks in de machine.
    * De web server-configuratie of regels wijzigen.


## <a name="test-failover"></a>Testfailover

U kunt een plan voor herstel gebruiken voor het activeren van een testfailover. Gebruik de volgende aanbevolen procedures:

- Altijd een testfailover op een app voltooien voordat u een volledige failover uitvoert. Testfailovers helpen u om te controleren of de app weergegeven op de site recovery wordt.
- Als u merkt dat u iets hebt gemist, van een schone activeren en voer de testfailover. 
- Voert een testfailover uit meerdere keren totdat u zeker weet dat de app probleemloos herstelt.
- Omdat elke app uniek is, moet u plannen voor herstel die zijn aangepast voor elke toepassing bouwen en uitvoeren van een testfailover op elk.
- Apps en hun afhankelijkheden regelmatig wordt gewijzigd. Om ervoor te zorgen plannen voor herstel zijn bijgewerkt, voert u een testfailover uit voor elke app elk kwartaal.

    ![Schermafbeelding van een voorbeeld testen plan voor herstel in Site Recovery](./media/recovery-plan-overview/rptest.png)

## <a name="watch-the-video"></a>Video bekijken

Bekijk de video van een kort voorbeeld van een failover op en klik op voor een app met twee lagen WordPress.
    
> [!VIDEO https://channel9.msdn.com/Series/Azure-Site-Recovery/One-click-failover-of-a-2-tier-WordPress-application-using-Azure-Site-Recovery/player]



## <a name="next-steps"></a>Volgende stappen

- [Maak](site-recovery-create-recovery-plans.md) een plan voor herstel.
* Meer informatie over [uitvoeren van failovers](site-recovery-failover.md).  
