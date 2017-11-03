---
title: 'Virtuele Azure-machines repliceren tussen Azure-regio''s voor herstel na noodgevallen moet: Azure naar Azure | Microsoft Docs'
description: Geeft een overzicht van de stappen die u moet virtuele Azure-machines repliceren tussen Azure-regio's (Azure Azure) met de Azure Site Recovery-service voor herstel nodig zijn na noodgevallen.
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmon
editor: 
ms.assetid: dab98aa5-9c41-4475-b7dc-2e07ab1cfd18
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/10/2017
ms.author: raynew
ms.openlocfilehash: 9ca33057f6030fdcc233f6053fdc392d62f8f9f4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="replicate-azure-vms-between-regions-with-azure-site-recovery"></a>Virtuele Azure-machines repliceren tussen regio's met Azure Site Recovery

>[!NOTE]
>
> Azure Site Recovery-replicatie voor virtuele Azure-machines (VM's) is momenteel in preview.

In dit artikel wordt beschreven hoe virtuele Azure-machines repliceren tussen Azure-regio's met behulp van de [siteherstel](site-recovery-overview.md) service in de Azure portal.

Opmerkingen en vragen boeken aan de onderkant van dit artikel of op de [Azure Recovery Services-forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).

## <a name="disaster-recovery-in-azure"></a>Herstel na noodgevallen in Azure

Ingebouwde Azure-infrastructuur functies en mogelijkheden die bijdragen aan een robuuste en robuuste beschikbaarheidsstrategie voor de werkbelastingen die worden uitgevoerd op Azure Virtual machines. Er zijn echter diverse redenen waarom u nodig hebt om te plannen voor herstel na noodgevallen tussen Azure-regio's zelf:

- U moet voldoen aan de richtlijnen van naleving voor specifieke toepassingen en werkbelastingen waarvoor een zakelijke continuïteit en noodherstelplan (BCDR).
- U wilt dat de mogelijkheid om te beveiligen en herstellen van Azure VM's op basis van uw zakelijke beslissingen te nemen en niet alleen op basis van ingebouwde functionaliteit voor Azure.
- U moet testen failovers en herstel in overeenstemming met uw behoeften bedrijven en naleving zonder impact op de productie.
- U moet voor de regio herstel in geval van een noodgeval failover en failback naar de oorspronkelijke bron regio naadloos.

Met Site Recovery voor replicatie van de virtuele machine van Azure naar Azure kunt u deze taken uitvoeren.


## <a name="why-use-site-recovery"></a>Waarom u Site Recovery moet gebruiken      

Site Recovery biedt een eenvoudige manier om virtuele Azure-machines repliceren tussen regio's:

- **Automatische implementatie**. In tegenstelling tot een actief / actief-replicatiemodel hoeft niet voor een dure en complexe infrastructuur in de secundaire regio. Wanneer u replicatie inschakelt, maakt Site Recovery automatisch de vereiste resources in de doelregio, op basis van regio broninstellingen.
- **Regio's beheren**. Met Site Recovery kunt u vanaf een willekeurige regio repliceren naar een willekeurige regio binnen een continent. Vergelijk deze met leestoegang geografisch redundante opslag, dat asynchroon tussen Standaard repliceert [regio's gekoppeld](https://docs.microsoft.com/azure/best-practices-availability-paired-regions) alleen. Geografisch redundante opslag met leestoegang biedt alleen-lezen toegang tot de gegevens in de doelregio.
- **Automatische replicatie**. Site Recovery biedt geautomatiseerde continue replicatie. Failover en failback kunnen worden geactiveerd met een enkele klik.
- **RTO en RPO**. Site Recovery wordt gebruikgemaakt van de Azure-netwerk-infrastructuur die verbinding maakt gebieden om te zorgen dat RTO en RPO zeer lage.
- **Testen van**. U kunt herstel na noodgevallen zoomt uitvoeren met op aanvraag testfailovers, wanneer nodig, zonder de productie-workloads of lopende replicatie te beïnvloeden.
- **De herstelplannen**. U kunt herstelplannen indelen failover en failback van de gehele toepassing uitgevoerd op meerdere virtuele machines. De functie herstel plan heeft uitgebreide klas integratie met Azure automation-runbooks.


## <a name="deployment-summary"></a>Implementatieoverzicht

Hier volgt een samenvatting van wat u moet doen voor het instellen van de replicatie van virtuele machines tussen Azure-regio's:

1. Maak een Recovery Services-kluis. De kluis bevat configuratie-instellingen en stuurt replicatie.
2. Replicatie inschakelen voor de Azure VM's.
3. Een testfailover om ervoor te zorgen dat alles werkt zoals verwacht worden uitgevoerd.

>[!IMPORTANT]
>
> U kunt controleren de [ondersteuningsmatrix voor replicatie van de virtuele machine van Azure](./site-recovery-support-matrix-azure-to-azure.md).

>[!IMPORTANT]
>
> Zie voor meer informatie over het configureren van de vereiste uitgaande netwerkverbinding voor Azure virtuele machines voor replicatie van Site Recovery de [leidraad voor netwerken](./site-recovery-azure-to-azure-networking-guidance.md).

### <a name="before-you-start"></a>Voordat u begint

* Uw Azure gebruikersaccount moet zijn bepaalde [machtigingen](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) replicatie van Azure een virtuele machine in te schakelen.
* Uw Azure-abonnement moet worden ingeschakeld voor het maken van virtuele machines in de doellocatie die u wilt gebruiken als het herstel na noodgevallen regio. Neem contact op met ondersteuning voor het inschakelen van de quota voor vereist.

## <a name="create-a-recovery-services-vault"></a>Een Recovery Services-kluis maken

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]

>[!NOTE]
>
> U wordt aangeraden dat u de Recovery Services-kluis in de locatie waar u uw virtuele machines maken worden gerepliceerd. Bijvoorbeeld, als uw doellocatie is de centraal VS, maken de kluis in **VS-midden**.

## <a name="enable-replication"></a>Replicatie inschakelen

In **Recovery Services-kluizen**, klikt u op de kluisnaam van de. Klik in de kluis op de **+ repliceren** knop.

### <a name="step-1-configure-the-source"></a>Step 1. De configureren
1. In **bron**, selecteer **Azure - PREVIEW**.
2. In **bronlocatie**, selecteer de bron van Azure-regio waarin uw virtuele machines worden uitgevoerd.
3. Selecteer het implementatiemodel van uw virtuele machines: **Resource Manager** of **klassieke**.
4. Selecteer de **resourcegroep** voor Resource Manager virtuele machines of **cloudservice** voor klassieke virtuele machines.

    ![De configureren](./media/site-recovery-azure-to-azure/source.png)

### <a name="step-2-select-virtual-machines"></a>Stap 2. Selecteer de virtuele machines

* Selecteer de virtuele machines die u wilt repliceren, en klik vervolgens op **OK**.

    ![Selecteer de virtuele machines](./media/site-recovery-azure-to-azure/vms.png)

### <a name="step-3-configure-settings"></a>Stap 3. Instellingen configureren

1. Als u wilt overschrijven de standaardinstellingen van het doel en geef de instellingen van uw keuze, klikt u op **aanpassen**. Zie voor meer informatie [doelresources aanpassen](site-recovery-replicate-azure-to-azure.md##customize-target-resources).

    ![Instellingen configureren](./media/site-recovery-azure-to-azure/settings.png)


2. Standaard maakt Site Recovery een replicatiebeleid die ervoor zorgt dat toepassingsconsistente momentopnamen elke 4 uur handhaaft herstelpunten gedurende 24 uur. Als een beleid maken met verschillende instellingen, klikt u op **aanpassen** naast **replicatiebeleid**.

    ![Beleid aanpassen](./media/site-recovery-azure-to-azure/customize-policy.png)

3. Als u wilt inrichten van de doelresources, klikt u op **doelresources maken**. Inrichting duurt een minuut. Sluit de blade niet tijdens het inrichten of u wilt beginnen.

4. Om replicatie van de geselecteerde virtuele machine, klikt u op **replicatie inschakelen**.

5. U kunt de voortgang van volgen de **beveiliging inschakelen** taak **instellingen** > **taken** > **Site Recovery-taken**.

6. In **instellingen** > **gerepliceerde Items**, vindt u de status van virtuele machines en de voortgang van de initiële replicatie. Klik op de virtuele machine Inzoomen op de instellingen ervan.

## <a name="run-a-test-failover"></a>Een testfailover uitvoeren

Nadat u alles instellen, voert u een testfailover om te controleren of dat alles werkt zoals verwacht:

1. Een enkele machine failover **instellingen** > **gerepliceerde Items**, klikt u op de virtuele machine **+ Testfailover** pictogram.

2. Failover van een herstelplan **instellingen** > **herstelplannen**, met de rechtermuisknop op het plan **Testfailover**. Volg [deze instructies](site-recovery-create-recovery-plans.md) om een herstelplan te maken. 

3. In **Testfailover**, selecteer het doelobject voor de virtuele Azure-netwerk voor welke Azure VM's na de failover zijn verbonden.

4. Voor het starten van de failover, klikt u op **OK**. Om de voortgang volgen, klikt u op de virtuele machine om de eigenschappen te openen. U kunt ook op de **Testfailover** taak in de kluisnaam > **instellingen** > **taken** > **Site Recovery-taken**.

5. Nadat de failover is voltooid, de replica-machine van Azure wordt weergegeven in de Azure portal > **virtuele Machines**. Zorg ervoor dat de virtuele machine is de juiste grootte heeft, of deze is verbonden met het juiste netwerk en dat deze wordt uitgevoerd.

6. Als u wilt verwijderen van de virtuele machines die zijn gemaakt tijdens de testfailover, klikt u op **opschonen testfailover** voor het gerepliceerde artikel of het herstelplan. In **notities**, vastleggen en opslaan van eventuele opmerkingen die zijn gekoppeld aan de testfailover. 

[Meer informatie](site-recovery-test-failover-to-azure.md) over testfailovers.


## <a name="next-steps"></a>Volgende stappen

Nadat u de implementatie te testen:

- [Meer informatie](site-recovery-failover.md) over verschillende typen failovers en het uitvoeren hiervan.
- Meer informatie over [met herstelplannen](site-recovery-create-recovery-plans.md) te verminderen RTO.
