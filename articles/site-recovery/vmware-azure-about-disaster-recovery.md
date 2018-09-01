---
title: Over herstel na noodgevallen van virtuele VMware-machines naar Azure met behulp van Azure Site Recovery | Microsoft Docs
description: Dit artikel bevat een overzicht van herstel na noodgevallen van virtuele VMware-machines naar Azure met behulp van de Azure Site Recovery-service.
author: rayne-wiselman
ms.service: site-recovery
ms.topic: conceptual
services: site-recovery
ms.date: 08/29/2018
ms.author: raynew
ms.openlocfilehash: 105ad94b442790c82af0f6b9533f195d93c5cd1d
ms.sourcegitcommit: 0c64460a345c89a6b579b1d7e273435a5ab4157a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/31/2018
ms.locfileid: "43339146"
---
# <a name="about-disaster-recovery-of-vmware-vms-to-azure"></a>Over herstel na noodgevallen van virtuele VMware-machines naar Azure

In dit artikel biedt een overzicht van herstel na noodgevallen voor on-premises VMware-machines naar Azure met de [Azure Site Recovery](site-recovery-overview.md) service.

## <a name="what-is-bcdr"></a>Wat is BCDR?

Een strategie voor zakelijke continuïteit en noodherstel (BCDR) recovery helpt uw bedrijf bij te houden. BCDR gegevens veilig en beschikbaar houdt en zorgt ervoor dat apps actief blijven tijdens geplande uitvaltijd en onverwachte storingen. Naast platform BCDR-functies, zoals regioparen en opslag met hoge beschikbaarheid biedt Azure Recovery Services als een integraal onderdeel van uw BCDR-oplossing. Recovery services omvatten: 

- [Azure Backup](https://docs.microsoft.com/azure/backup/backup-introduction-to-azure-backup) back-ups van uw on-premises en Azure VM-gegevens. U kunt back-up van een bestand en mappen, specifieke werkbelastingen of een hele virtuele machine. 
- [Azure Site Recovery](site-recovery-overview.md) biedt tolerantie en herstel na noodgevallen voor apps en workloads die worden uitgevoerd op on-premises virtuele machines of Azure IaaS VM's. Site Recovery deelt replicatie en failover naar Azure worden verwerkt als er uitval optreedt. Ook verwerkt deze herstel van Azure naar uw primaire site. 

## <a name="how-does-site-recovery-do-disaster-recovery"></a>Hoe doet Site Recovery herstel na noodgevallen?

1. Na het voorbereiden van Azure en uw on-premises site, die u kunt instellen en replicatie inschakelen voor uw on-premises machines.
2. Site Recovery coördineert de initiële replicatie van de virtuele machine, in overeenstemming met de beleidsinstellingen van uw.
3. Site Recovery worden na de initiële replicatie deltawijzigingen gerepliceerd naar Azure. 
4. Als alles wordt replicatie uitgevoerd zoals verwacht, kunt u een Dr-herstelanalyse uitvoeren.
    - De analyse zorgt ervoor dat de failover werkt zoals verwacht wanneer een echte noodzaak daartoe zich voordoet.
    - De analyse voert een testfailover zonder enige impact op uw productieomgeving.
5. Als er een storing optreedt, moet u een volledige failover naar Azure uitvoert. Kunt u een failover uitgevoerd voor een enkele machine, of u een herstelplan waarin meerdere virtuele machines op hetzelfde moment een failover kunt maken.
6. Bij failover worden virtuele Azure-machines gemaakt vanuit de VM-gegevens in Azure Storage. Gebruikers kunnen doorgaan met het openen van toepassingen en workloads van de Azure VM
7. Als uw on-premises site weer beschikbaar is, schakelt u van Azure.
8. Nadat u een failback uitvoeren en vanaf uw primaire locatie zodra er meer werken, start u voor het repliceren van on-premises machines naar Azure is het opnieuw.


## <a name="how-do-i-know-if-my-environment-is-suitable-for-disaster-recovery-to-azure"></a>Hoe weet ik of Mijn omgeving geschikt voor herstel na noodgevallen naar Azure is?

Site Recovery kan elke workload die worden uitgevoerd op een ondersteunde VMware-VM of een fysieke server gerepliceerd. Hier volgen de dingen die u in uw omgeving nodig hebt om te controleren:

- Als u virtuele VMware-machines repliceert, voert u de juiste versies van VMware-servers voor virtualisatie? [Kom hier](vmware-physical-azure-support-matrix.md#on-premises-virtualization-servers).
- Zijn de machines die u wilt repliceren met een ondersteund besturingssysteem? [Kom hier](vmware-physical-azure-support-matrix.md#replicated-machines).
- Voor herstel na noodgevallen Linux, worden computers een ondersteunde bestandsopslag met system/Gast uitgevoerd? [Kom hier](vmware-physical-azure-support-matrix.md#linux-file-systemsguest-storage)
- De machines die u wilt repliceren voldoen aan de vereisten voor Azure? [Kom hier](vmware-physical-azure-support-matrix.md#azure-vm-requirements).
- Wordt uw netwerkconfiguratie ondersteund? [Kom hier](vmware-physical-azure-support-matrix.md#network).
- Wordt de opslagconfiguratie ondersteund? [Kom hier](vmware-physical-azure-support-matrix.md#storage).


## <a name="what-do-i-need-to-set-up-in-azure-before-i-start"></a>Wat heb ik nodig om in te stellen in Azure voordat ik beginnen?

U moet de volgende voorbereidingen in Azure:

1. Controleer of uw Azure-account heeft machtigingen voor het maken van virtuele machines in Azure.
2. Maak een opslagaccount voor het opslaan van installatiekopieën van gerepliceerde machines.
3. Maak een Azure-netwerk dat Azure-VM's deel uitmaken gaan wanneer ze worden gemaakt uit de opslag na een failover.
4. Instellen van een Azure Recovery Services-kluis voor Site Recovery. De kluis zich bevindt in de Azure-portal en wordt gebruikt om te implementeren, configureren, beheren, controleren en oplossen van uw implementatie van Site Recovery.

*Meer hulp nodig?*

Meer informatie over het instellen van Azure door [verificatie van uw account](tutorial-prepare-azure.md#verify-account-permissions), maken een [opslagaccount](tutorial-prepare-azure.md#create-a-storage-account) en [netwerk](tutorial-prepare-azure.md#set-up-an-azure-network), en [instellen van een kluis](tutorial-prepare-azure.md#create-a-recovery-services-vault).



## <a name="what-do-i-need-to-set-up-on-premises-before-i-start"></a>Wat heb ik nodig voor het instellen van on-premises voordat ik beginnen?

On-premises dit is wat u nodig hebt om te doen:

1. U moet een aantal accounts instellen:

    - Als u virtuele VMware-machines repliceert, is een account voor Site Recovery voor toegang tot vCenter-SErver of vSphere ESXi-hosts nodig voor het automatisch detecteren van virtuele machines.
    - Er is een account nodig voor het installeren van de Site Recovery Mobility service-agent op elke fysieke computer of de virtuele machine die u wilt repliceren.

2. U moet de compatibiliteit van uw VMware-infrastructuur controleren als u die eerder niet hebt gedaan.
3. Zorg ervoor dat u verbinding met virtuele Azure-machines na een failover maken kunt. Instellen van RDP voor on-premises Windows-computers of SSH op Linux-machines.

*Meer hulp nodig?*
- Accounts voor te bereiden [automatische detectie](vmware-azure-tutorial-prepare-on-premises.md#prepare-an-account-for-automatic-discovery) en voor [installatie van de Mobility-service](vmware-azure-tutorial-prepare-on-premises.md#prepare-an-account-for-mobility-service-installation).
- [Controleer of](vmware-azure-tutorial-prepare-on-premises.md#check-vmware-requirements) uw VMware-instellingen die compatibel zijn.
- [Voorbereiden](vmware-azure-tutorial-prepare-on-premises.md#prepare-to-connect-to-azure-vms-after-failover) zodat u verbinding in Azure na een failover maken.
- Als u meer gedetailleerde informatie over het instellen van het IP-adressen voor virtuele Azure-machines na een failover wilt [Lees dit artikel](concepts-on-premises-to-azure-networking.md).

## <a name="how-do-i-set-up-disaster-recovery"></a>Hoe stel ik u herstel na noodgevallen?

Nadat u uw infrastructuur van Azure en on-premises geïmplementeerd hebt, kunt u herstel na noodgevallen instellen.

1. Voor informatie over de onderdelen die u moet implementeren, raadpleegt de [VMware naar Azure-architectuur](vmware-azure-architecture.md), en de [fysiek naar Azure-architectuur](physical-azure-architecture.md). Er zijn een aantal onderdelen, dus is het belangrijk om te begrijpen hoe ze allemaal bij elkaar passen.
2. **Bronomgeving**: als een eerste stap in de implementatie, instellen van uw replicatiebronomgeving. U opgeven wat u wilt repliceren en waar u naar wilt repliceren.
3. **Configuratieserver**: U moet een configuratieserver in uw on-premises bron-omgeving instellen:
    - De configuratieserver is een één on-premises machine. Voor herstel na noodgevallen VMware, wordt u aangeraden als een VMware-VM die kunnen worden geïmplementeerd vanaf een downloadbare OVF-sjabloon te implementeren.
    - De configuratieserver coördineert de communicatie tussen on-premises en Azure
    - Een aantal andere onderdelen die worden uitgevoerd op de servercomputer van de configuratie.
        - De processerver ontvangt, optimaliseert en replicatiegegevens verzendt naar Azure storage. Ook verwerkt deze automatische installatie van de Mobility-service op computers die u repliceren wilt, en voert automatische detectie van virtuele machines op VMware-servers.
        - Op de hoofddoelserver worden de replicatiegegevens tijdens de failback vanuit Azure afgehandeld.
    - Instellen bevat de configuratieserver registreren in de kluis, MySQL-Server en VMware PowerCLI, downloaden en op te geven de accounts die zijn gemaakt voor automatische detectie en installatie van de Mobility-service.
4. **Doelomgeving**: instellen van het doel-Azure-omgeving door op te geven uw Azure-abonnement, opslag- en netwerkinstellingen.
5. **Beleid voor wachtwoordreplicatie**: U opgeven hoe replicatie moet worden uitgevoerd. Instellingen omvatten onder meer hoe vaak herstelpunten worden gemaakt en opgeslagen, en of u toepassingsconsistente momentopnamen moeten worden gemaakt.
6. **Replicatie inschakelen**. U schakelt replicatie voor on-premises computers. Als u een account voor het installeren van de Mobility-service hebt gemaakt, wordt wordt deze geïnstalleerd wanneer u replicatie voor een machine inschakelt. 

*Meer hulp nodig?*

- Voor een snel overzicht van deze stappen, u kunt uitproberen onze [VMware zelfstudie](vmware-azure-tutorial.md), en [fysieke server scenario](physical-azure-disaster-recovery.md).
- [Meer informatie](vmware-azure-set-up-source.md) over het instellen van uw bronomgeving.
- [Meer informatie over](vmware-azure-deploy-configuration-server.md) configuratievereisten voor de server, en het instellen van de configuratieserver met behulp van een OVF-sjabloon voor VMware-replicatie. Als voor een of andere reden niet kan u een sjabloon, of u fysieke servers repliceert, [Volg deze instructies](physical-azure-set-up-source.md#set-up-the-source-environment).
- [Meer informatie](vmware-azure-set-up-target.md) over doelinstellingen.
- [Meer informatie](vmware-azure-set-up-replication.md) over het instellen van beleid voor replicatie.
- [Informatie over](vmware-azure-enable-replication.md) inschakelen replicatie en [uitsluiten](vmware-azure-exclude-disk.md) schijven van replicatie.


## <a name="something-went-wrong-how-do-i-troubleshoot"></a>Er is iets misgegaan hoe los ik?

- Als een eerste stap probeer [controleren van uw implementatie](site-recovery-monitor-and-troubleshoot.md) om te controleren of de status van gerepliceerde items, taken en problemen met infrastructuur en eventuele fouten te identificeren.
- Als u om de initiële replicatie te voltooien, of continue replicatie werkt niet zoals verwacht, [raadpleegt u dit artikel](vmware-azure-troubleshoot-replication.md) voor veelvoorkomende fouten en tips voor probleemoplossing.
- Als er problemen zijn met de automatische installatie van de Mobility-service op computers die u wilt repliceren, raadpleegt u veelvoorkomende fouten in [in dit artikel](vmware-azure-troubleshoot-push-install.md).
- Als failover werkt niet zoals verwacht, controleert u veelvoorkomende fouten [in dit artikel](site-recovery-failover-to-azure-troubleshoot.md).
- Als u failback niet werkt, controleert u of uw probleem wordt weergegeven [in dit artikel](vmware-azure-troubleshoot-failback-reprotect.md).



## <a name="next-steps"></a>Volgende stappen

Bij replicatie nu op locatie, moet u [een Dr-herstelanalyse uitvoeren](tutorial-dr-drill-azure.md) om ervoor te zorgen dat de failover werkt zoals verwacht. 
