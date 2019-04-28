---
title: HANA failover-procedure voor het noodherstelsite voor SAP HANA op Azure (grote instanties) | Microsoft Docs
description: Het uitvoeren van failover naar de site voor noodherstel voor SAP HANA op Azure (grote instanties)
services: virtual-machines-linux
documentationcenter: ''
author: saghorpa
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 04/22/2019
ms.author: saghorpa
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 76d8bb816bdf229d13a49fa61337899a8bf29ecd
ms.sourcegitcommit: 61c8de2e95011c094af18fdf679d5efe5069197b
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "62098283"
---
# <a name="disaster-recovery-failover-procedure"></a>Procedure bij failover van het noodherstel


>[!IMPORTANT]
>Deze documentatie is geen vervanging van de documentatie bij het beheer van SAP HANA of SAP-opmerkingen. Er wordt verwacht dat de lezer een goed begrip van en deskundigheid in SAP HANA-beheer en bewerkingen, vooral bij de onderwerpen van back-up, herstel, hoge beschikbaarheid en herstel na noodgevallen is. Schermafbeeldingen van SAP HANA Studio worden in deze documentatie wordt weergegeven. Inhoud, de structuur en de aard van de schermen van SAP-beheerprogramma's en de hulpprogramma's zelf kunnen worden gewijzigd van SAP HANA, release-versie.

Er zijn twee mogelijke situaties moet overwegen bij het uitvoeren van een failover naar de DR-site:

- U moet de SAP HANA-database naar de meest recente status van gegevens. In dit geval is er een selfservice-script waarmee u de failover zonder de noodzaak contactgegevens van Microsoft kunt uitvoeren. Voor de failback moet u echter werken met Microsoft.
- U wilt herstellen naar een momentopname van een opslag die niet de meest recente gerepliceerde momentopname. In dit geval moet u werken met Microsoft. 

>[!NOTE]
>De volgende stappen moeten worden uitgevoerd op de eenheid HANA grote instantie, die staat voor de DR-eenheid. 
 
Voer de stappen uit als u wilt herstellen naar de meest recente gerepliceerde opslagmomentopnamen, zoals vermeld in de sectie **uitvoeren van volledige DR-failover - azure_hana_dr_failover** van het document [Microsoft snapshot-hulpprogramma's voor SAP HANA op Azure ](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf). 

Als u wilt dat meerdere SAP HANA-instanties failover is uitgevoerd, moet u de opdracht azure_hana_dr_failover meerdere keren uitgevoerd. Wanneer aangevraagd, typt u in de SAP HANA-SID die u wilt een failover en herstel. 


U kunt ook de DR-failover testen zonder enige impact op de werkelijke replicatierelatie. Als u een test-failover, volg de stappen in **uitvoeren van een test DR-failover - azure_hana_test_dr_failover** van het document [Microsoft snapshot-hulpprogramma's voor SAP HANA op Azure](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf). 

>[!IMPORTANT]
>Voer *niet* productietransacties worden uitgevoerd op het exemplaar dat u hebt gemaakt in de DR-site door het proces van **testen van een failover**. De azure_hana_test_dr_failover van deze opdracht wordt een set van volumes waarvoor geen relatie met de primaire site. Als gevolg hiervan synchronisatie naar de primaire site is *niet* mogelijk. 

Als u wilt dat meerdere SAP HANA-instanties om te testen, moet u het script meerdere keren uitgevoerd. Wanneer aangevraagd, typt u in de SAP HANA-SID van het exemplaar dat u wilt testen voor failover. 

>[!NOTE]
>Als u een failover wilt uitvoeren naar de DR-site te redden ook gegevens die uur geleden is verwijderd en moet de DR-volumes worden ingesteld op een eerdere momentopname, wordt deze procedure geldt. 

1. Sluit de niet-productie-exemplaar van HANA op de eenheid van het herstel na noodgevallen van HANA grote instanties die u uitvoert. Dit is omdat een niet-actieve productie-exemplaar voor HANA is vooraf geïnstalleerd is.
1. Zorg ervoor dat er geen SAP HANA-processen worden uitgevoerd. Gebruik de volgende opdracht voor deze controle: `/usr/sap/hostctrl/exe/sapcontrol –nr <HANA instance number> - function GetProcessList`. U moet worden weergegeven door de uitvoer de **hdbdaemon** verwerken in een status ' gestopt ' en er zijn geen andere HANA-processen in een status die wordt uitgevoerd of gestart.
1. Bepaal waarvan de naam van de momentopname of SAP HANA-back-ID die u wilt dat de site voor noodherstel hersteld. Deze momentopname is in de echte disaster recovery gevallen, meestal de meest recente momentopname. Als u herstellen, verlies van gegevens wilt, kiest u een eerdere momentopname.
1. Contact opnemen met ondersteuning voor Azure via een ondersteuningsaanvraag met hoge prioriteit. Vraag voor het herstellen van die momentopname (met de naam en datum van de momentopname) of de back-HANA-ID op de DR-site. De standaardwaarde is dat de operations-zijde/hana/gegevensvolume alleen herstelt. Als u de hana-/ logbackups-volumes wilt, moet u die specifiek status. *Moet het volume /hana/shared niet herstellen.* In plaats daarvan moet u specifieke bestanden, zoals global.ini van de **.snapshot** map en submappen nadat u / hana koppelen/gedeeld clustervolume voor PRD. 

   Aan de bewerkingen voor de volgende stappen plaats:

   a. De replicatie van de volumes van de herstel na noodgevallen van momentopnamen van de productievolume is gestopt. Deze onderbreking mogelijk al is dit als een storing op de productiesite is de reden moet u de noodherstelprocedure uitvoeren.
   
   b. De opslag een momentopname van de naam of momentopname met de back-ID die u hebt gekozen op de disaster recovery volumes is hersteld.
   
   c. Na het herstel zijn de disaster recovery-volumes beschikbaar moet worden gekoppeld aan de eenheden HANA grote instantie in de Dr-regio.
      
1. Koppel de disaster recovery-volumes in de eenheid HANA grote instantie in de site voor noodherstel. 
1. Start de niet-actieve productie-instantie van SAP HANA.
1. Als u wilt het logboek back-up-transactielogboeken kopiëren om de RPO-tijd te beperken, moet u deze transactielogboekback-ups in de directory van de zojuist gekoppelde DR/hana/logbackups samenvoegen. Bestaande back-ups niet worden overschreven. Kopieer de nieuwere back-ups die niet zijn gerepliceerd met de meest recente replicatie van een momentopname van de opslag.
1. U kunt ook enkele bestanden uit de momentopnamen die zijn gerepliceerd naar het volume /hana/shared/PRD in de DR-Azure-regio herstellen.

De volgende reeks stappen omvat het herstellen van de productie-instantie van SAP HANA op basis van de momentopname van het herstelde opslag en de transactielogboekback-ups die beschikbaar zijn:

1. Wijzigen van de back-uplocatie naar **/hana/logbackups** met behulp van SAP HANA Studio.
   ![Wijzigen van de back-uplocatie voor DR-herstel](./media/hana-overview-high-availability-disaster-recovery/change_backup_location_dr1.png)

1. SAP HANA via de back-upbestand locaties wordt gescand en stelt de meest recente transactielogboek om naar te herstellen. De scan kan duurt enkele minuten tot een scherm zoals hieronder wordt weergegeven: ![Lijst met transactielogboekback-ups voor DR-herstel](./media/hana-overview-high-availability-disaster-recovery/backup_list_dr2.PNG)

1. Enkele van de standaardinstellingen aan te passen:

      - Schakel **gebruik back-ups verschillen**.
      - Selecteer **initialiseren Log gebied**.

   ![Het logboek initialiseren gebied instellen](./media/hana-overview-high-availability-disaster-recovery/initialize_log_dr3.PNG)

1. Selecteer **Voltooien**.

   ![Het terugzetten van de DR voltooien](./media/hana-overview-high-availability-disaster-recovery/finish_dr4.PNG)

Een venster uitgevoerd, zoals hieronder, moet worden weergegeven. Houd er rekening mee dat het voorbeeld van een herstel na noodgevallen herstel van een configuratie met drie knooppunten scale-out SAP HANA.

![Voortgang van terugzetten](./media/hana-overview-high-availability-disaster-recovery/restore_progress_dr5.PNG)

Als het herstel lijkt te reageren op de **voltooien** scherm en niet weergeven van het scherm wordt uitgevoerd, bevestigen dat de SAP HANA-exemplaren op de worker-knooppunten worden uitgevoerd. Indien nodig, start u de SAP HANA-instanties handmatig in.


## <a name="failback-from-a-dr-to-a-production-site"></a>Failback van een herstel na Noodgevallen naar een productiesite
U kunt een failback van een herstel na Noodgevallen met een productiesite. We bekijken een scenario waarin de failover naar de site voor noodherstel is veroorzaakt door problemen in de productie van Azure-regio, en niet door de noodzaak om verloren gegevens te herstellen. U uitvoert uw SAP-productie-werkbelasting een tijdje in de site voor noodherstel. Als de problemen in de productiesite zijn opgelost, die u wilt een failback uitvoeren naar de productiesite. Omdat u kunt geen gegevens verloren gaan, wordt de stap terug naar de productiesite omvat meerdere stappen en nauwe samenwerking met de SAP HANA op Azure-bewerkingen team. Het is aan u voor het activeren van het operationele team om te beginnen met het synchroniseren terug naar de productiesite nadat de problemen zijn opgelost.

Dit is de volgorde van stappen moeten uitvoeren:

1. De SAP HANA op Azure-bewerkingen team Hiermee haalt u de trigger om te synchroniseren van de productie-opslagvolumes uit de opslagvolumes disaster recovery, die nu de productiestatus van de vertegenwoordigen. De eenheid HANA grote instantie in de productiesite wordt in deze status wordt afgesloten.
1. De SAP HANA op Azure-bewerkingen team controleert de replicatie en zorgt ervoor dat deze wordt bijgewerkt voordat aan u gemeld.
1. U afsluiten de toepassingen die gebruikmaken van de productie HANA-exemplaar in de site voor noodherstel. Vervolgens voert u een transactielogboek van HANA. Vervolgens maakt u de HANA-instantie die wordt uitgevoerd op de eenheden HANA grote instantie in de site voor noodherstel te stoppen.
1. Nadat de HANA-instantie die wordt uitgevoerd in de eenheid HANA grote instantie in de site voor noodherstel wordt afgesloten, synchroniseert het operationele team handmatig de schijfvolumes opnieuw.
1. De SAP HANA op Azure operationele team de eenheid HANA grote instantie in de productiesite opnieuw gestart en geeft die boven aan u. U ervoor zorgen dat de SAP HANA-instantie in een afsluitingsstaat bij het opstarten van de eenheid HANA grote instantie.
1. U kunt stappen voor het terugzetten van dezelfde database uitvoeren zoals u deed toen eerder Failover-overschakeling uitvoeren naar de site voor noodherstel.

## <a name="monitor-disaster-recovery-replication"></a>Disaster recovery-replicatie controleren

U kunt de status van de voortgang van uw storage-replicatie controleren door het uitvoeren van script `azure_hana_replication_status`. Met deze opdracht moet worden uitgevoerd vanaf een eenheid die wordt uitgevoerd in de locatie voor noodherstel werken zoals verwacht. De opdracht werkt, ongeacht of de replicatie actief is. De opdracht kan worden uitgevoerd voor elke eenheid HANA grote instantie van uw tenant in de locatie voor noodherstel. Het kan niet worden gebruikt voor het verkrijgen van informatie over het opstartvolume. Lees voor meer informatie van de opdracht en de uitvoer **'Get replicatiestatus van de DR - azure_hana_replication_status'** van het document [Microsoft snapshot-hulpprogramma's voor SAP HANA op Azure](https://github.com/Azure/hana-large-instances-self-service-scripts/blob/master/snapshot_tools_v4.0/Microsoft%20Snapshot%20Tools%20for%20SAP%20HANA%20on%20Azure%20v4.0.pdf).


## <a name="next-steps"></a>Volgende stappen
- Raadpleeg [bewaking en probleemoplossing van HANA kant](hana-monitor-troubleshoot.md).
