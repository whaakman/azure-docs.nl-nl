---
title: Een meerlagige Citrix XenDesktop en XenApp-implementatie met behulp van Azure Site Recovery repliceert | Microsoft Docs
description: In dit artikel wordt beschreven hoe u beveiligen en herstellen van XenApp- en Citrix XenDesktop-implementaties die met Azure Site Recovery.
services: site-recovery
documentationcenter: ''
author: ponatara
manager: abhemraj
editor: ''
ms.assetid: 9126f5e8-e9ed-4c31-b6b4-bf969c12c184
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/06/2018
ms.author: ponatara
ms.openlocfilehash: 45d366842416ddfa7b0153a1d075ee6de58e45a1
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/23/2018
ms.locfileid: "39213630"
---
# <a name="replicate-a-multi-tier-citrix-xenapp-and-xendesktop-deployment-using-azure-site-recovery"></a>Een meerlagige Citrix XenApp en XenDesktop-implementatie met behulp van Azure Site Recovery worden gerepliceerd

## <a name="overview"></a>Overzicht

Citrix XenDesktop is een bureaublad-virtualisatie-oplossing die voorziet in bureaubladen en toepassingen als een ondemand-service op een willekeurige gebruiker, waar dan ook. Met FlexCast delivery-technologie, kan XenDesktop snel en veilig leveren en bureaubladen voor gebruikers.
Vandaag de dag Citrix XenApp zorgt niet voor noodgevallen mogelijkheden voor herstel.

Een goede noodhersteloplossing mag modellering van herstelplannen rond de bovenstaande architecturen voor complexe toepassingen en ook de mogelijkheid het toevoegen van aangepaste stappen voor het afhandelen van toepassingstoewijzingen tussen verschillende lagen daarom bieden een één-op Zorg ervoor dat de schermopname oplossing in het geval van een noodgeval leidt tot een lagere RTO.

Dit document bevat een stapsgewijze instructies voor het bouwen van een oplossing voor noodherstel voor uw on-premises Citrix XenApp-implementaties op Hyper-V en VMware vSphere-platforms. Dit document beschrijft ook hoe u een testfailover (Dr-herstelanalyse) en niet-geplande failover naar Azure met behulp van herstelplannen, de ondersteunde configuraties en vereisten uitvoert.


## <a name="prerequisites"></a>Vereisten

Voordat u begint, zorg er dan voor dat u het volgende weten:

1. [Een virtuele machine repliceren naar Azure](site-recovery-vmware-to-azure.md)
1. Hoe u [een herstelnetwerk ontwerpen](site-recovery-network-design.md)
1. [Een testfailover uitvoeren naar Azure](site-recovery-test-failover-to-azure.md)
1. [Een failover uitvoert naar Azure](site-recovery-failover.md)
1. Hoe u [repliceren van een domeincontroller](site-recovery-active-directory.md)
1. Hoe u [SQL Server repliceren](site-recovery-sql.md)

## <a name="deployment-patterns"></a>Implementaties

Een Citrix XenApp en XenDesktop-farm is doorgaans de volgende implementatie-patroon:

**Implementatie-patroon**

Citrix XenApp en XenDesktop-implementaties met AD DNS-server, SQL database-server, Citrix Delivery Controller, StoreFront-server, XenApp Master (VDA), Citrix XenApp-licentieserver

![Implementatie-patroon 1](./media/site-recovery-citrix-xenapp-and-xendesktop/citrix-deployment.png)


## <a name="site-recovery-support"></a>Ondersteuning voor site Recovery

In dit artikel, Citrix-implementaties op virtuele VMware-machines worden beheerd door vSphere 6.0 / System Center VMM 2012 R2 werden gebruikt voor het instellen van herstel na Noodgevallen.

### <a name="source-and-target"></a>Bron en doel

**Scenario** | **Naar een secundaire site** | **Naar Azure**
--- | --- | ---
**Hyper-V** | Niet in bereik | Ja
**VMware** | Niet in bereik | Ja
**Fysieke server** | Niet in bereik | Ja

### <a name="versions"></a>Versies
Klanten kunnen XenApp-onderdelen implementeren als virtuele Machines op Hyper-V of VMware of fysieke Servers. Azure Site Recovery kan zowel fysieke als virtuele implementaties naar Azure te beveiligen.
Aangezien XenApp 7,7 of hoger wordt ondersteund in Azure, alleen implementaties met deze versies kunnen een failover naar Azure voor herstel na noodgevallen of de migratie.

### <a name="things-to-keep-in-mind"></a>Houd er rekening mee

1. Beveiliging en herstel van on-premises implementaties van besturingssysteem van de Server met machines om te leveren XenApp gepubliceerde apps en XenApp bureaubladen gepubliceerd wordt ondersteund.

2. Beveiliging en herstel van on-premises implementaties desktopcomputers OS gebruiken om te leveren van VDI Desktop Client virtuele bureaubladen, met inbegrip van Windows 10, wordt niet ondersteund. Dit is omdat het herstel van machines met desktop OS'es biedt geen ondersteuning voor ASR.  Bovendien sommige virtuele bureaublad clientbesturingssystemen (bv. Windows 7) zijn nog niet ondersteund voor licentieverlening in Azure. [Meer informatie](https://azure.microsoft.com/pricing/licensing-faq/) over licentieverlening voor clientbureaubladen en serverdesktops in Azure.

3.  Azure Site Recovery kan niet repliceren en beveiligen van bestaande on-premises MCS of PV's klonen.
U moet deze met Azure RM inrichting van Delivery controller klonen opnieuw maken.

4. NetScaler kan niet worden beveiligd met Azure Site Recovery als NetScaler is gebaseerd op toegang tot FreeBSD en Azure Site Recovery biedt geen ondersteuning voor beveiliging van FreeBSD-besturingssysteem. U moet implementeren en configureren van een nieuw toestel NetScaler van Azure-marktplaats na een failover naar Azure.


## <a name="replicating-virtual-machines"></a>Replicatie van virtuele machines

De volgende onderdelen van de Citrix XenApp-implementatie moeten worden beveiligd als u wilt inschakelen replicatie en het herstel.

* Beveiliging van AD DNS-server
* Beveiliging van SQL database-server
* Beveiliging van Citrix Delivery Controller
* Beveiliging van StoreFront-server.
* Beveiliging van XenApp Master (VDA)
* Beveiliging van Citrix XenApp-licentieserver


**AD DNS-server-replicatie**

Raadpleeg [beveiligen van Active Directory en DNS met Azure Site Recovery](site-recovery-active-directory.md) over de richtlijnen voor het repliceren en een domeincontroller configureren in Azure.

**SQL database Server-replicatie**

Raadpleeg [SQL-Server beveiligen met SQL Server-noodherstel en Azure Site Recovery](site-recovery-sql.md) voor gedetailleerde technische richtlijnen voor de aanbevolen opties voor het beveiligen van SQL-servers.

Ga als volgt [deze richtlijnen](site-recovery-vmware-to-azure.md) om te beginnen met het andere onderdeel virtuele machines repliceren naar Azure.

![Beveiliging van XenApp-onderdelen](./media/site-recovery-citrix-xenapp-and-xendesktop/citrix-enablereplication.png)

**Reken- en netwerkinstellingen**

Nadat de computers zijn beveiligd (status wordt weergegeven als 'Beveiligd' onder gerepliceerde Items), de instellingen berekening en netwerk moeten worden geconfigureerd.
In berekening en netwerk > Compute-eigenschappen, kunt u de Azure VM-naam en doel-grootte.
De naam om te voldoen aan de vereisten voor Azure als u wilt wijzigen. U kunt ook bekijken en toevoegen van informatie over het doelnetwerk, subnet en IP-adres dat wordt toegewezen aan de virtuele machine van Azure.

Houd rekening met het volgende:

* U kunt het doel-IP-adres instellen. Als u geen adres opgeeft, maakt de machine waarvoor een failover is uitgevoerd, gebruik van DHCP. Als u een adres dat is niet beschikbaar na een failover hebt ingesteld, werkt de failover niet. Hetzelfde doel-IP-adres kan worden gebruikt voor een testfailover als het adres beschikbaar is in het testfailovernetwerk.

* Voor de AD-en DNS-server kunt behoud van de on-premises-adres u hetzelfde adres opgeven als de DNS-server voor het Azure-netwerk.

Het aantal netwerkadapters wordt bepaald door de grootte die u voor de virtuele doelmachine opgeeft. Dat werkt als volgt:

*   Als het aantal netwerkadapters op de bronmachine kleiner is dan of gelijk is aan het aantal adapters dat is toegestaan voor de grootte van de doelmachine, heeft het doel hetzelfde aantal adapters als de bron.
*   Als het aantal adapters voor de virtuele bronmachine meer is dan is toegestaan voor de doelgrootte, wordt het maximum voor de doelgrootte gebruikt.
* Als een bronmachine bijvoorbeeld twee netwerkadapters heeft en met de grootte van de doelmachine vier netwerkadapters worden ondersteund, heeft de doelcomputer twee adapters. Als de bronmachine twee adapters heeft, maar met de ondersteunde doelgrootte slechts één wordt ondersteund, heeft de doelcomputer slechts één adapter.
*   Als de virtuele machine meerdere netwerkadapters die worden deze allemaal met hetzelfde netwerk verbonden heeft.
*   Als de virtuele machine meerdere netwerkadapters heeft, wordt de eerste architectuur die wordt weergegeven in de lijst met de standaard-netwerkadapter in de virtuele machine van Azure.


## <a name="creating-a-recovery-plan"></a>Het maken van een herstelplan

Nadat de replicatie is ingeschakeld voor de virtuele machines XenApp-onderdeel, wordt de volgende stap is het maken van een plan voor herstel.
Een herstelplan groepen samen virtuele machines met dezelfde vereisten voor failover en herstel.  

**Stappen voor het maken van een herstelplan**

1. De XenApp onderdeel virtuele machines in het herstelplan toevoegen.
2. Klik op de plannen voor herstel -> + het herstelplan. Geef een intuïtieve naam voor het herstelplan te gaan.
3. Voor virtuele VMware-machines: bron selecteren als processerver VMware, doel als Microsoft Azure, en het implementatiemodel Resource Manager en klik op items selecteren.
4. Voor Hyper-V virtuele machines: bron selecteren als VMM-server, gericht op Microsoft Azure, en als Resource Manager-implementatiemodel en klik op items selecteren en selecteer vervolgens de implementatie XenApp VM's.

### <a name="adding-virtual-machines-to-failover-groups"></a>VM's toevoegt aan de failover-groepen

Herstelplannen kunnen worden aangepast om toe te voegen, failover-groepen voor specifieke opstartvolgorde te wijzigen, scripts of handmatige acties. De volgende groepen moeten worden toegevoegd aan het herstelplan.

1. Failover Group1: AD DNS
2. Failover-Group2: SQL Server-VM 's
2. Failover groep3: VDA Master-installatiekopie VM
3. Failover Group4: Delivery Controller en StoreFront-server-VM 's


### <a name="adding-scripts-to-the-recovery-plan"></a>Scripts toe te voegen aan het herstelplan te gaan

Scripts kunnen worden uitgevoerd vóór of na een specifieke groep van een herstelplan te gaan. Handmatige acties kunnen worden ook worden opgenomen en tijdens de failover is uitgevoerd.

Het aangepaste herstelplan ziet eruit als het hieronder:

1. Failover Group1: AD DNS
2. Failover-Group2: SQL Server-VM 's
3. Failover groep3: VDA Master-installatiekopie VM

   >[!NOTE]     
   >Stap 4, 6 en 7 met handmatige of script acties zijn van toepassing op slechts een on-premises XenApp > omgeving met MCS/PV's catalogussen.

4. Actie voor het handmatig of script van groep 3: afsluiten master VDA VM de Master VDA VM na failover naar Azure is in een status running doorbrengt. Voor het maken van nieuwe MCS-catalogi met behulp van Azure ARM die als host fungeert, de hoofd VDA-VM is vereist om te worden gestopt (de toegewezen) staat. Afsluiten van de virtuele machine van Azure-Portal.

5. Failover Group4: Delivery Controller en StoreFront-server-VM 's
6. Groep3 handmatig of script actie 1:

    ***Azure RM-hostverbinding toevoegen***

    Azure ARM-hostverbinding maken in machine Delivery Controller voor het inrichten van nieuwe MCS-catalogi in Azure. Volg de stappen, zoals wordt beschreven in deze [artikel](https://www.citrix.com/blogs/2016/07/21/connecting-to-azure-resource-manager-in-xenapp-xendesktop/).

7. Groep3 handmatig of script actie 2:

    ***MCS-catalogi in Azure opnieuw maken***

    De bestaande MCS of PV's klonen op de primaire site worden, niet gerepliceerd naar Azure. U moet deze met behulp van de gerepliceerde master VDA en Azure ARM inrichten Delivery controller klonen opnieuw maken. Volg de stappen, zoals wordt beschreven in deze [artikel](https://www.citrix.com/blogs/2016/09/12/using-xenapp-xendesktop-in-azure-resource-manager/) MCS-catalogi maken in Azure.

![Plan voor herstel voor XenApp-onderdelen](./media/site-recovery-citrix-xenapp-and-xendesktop/citrix-recoveryplan.png)


   >[!NOTE]
   >U kunt scripts op de [locatie](https://github.com/Azure/azure-quickstart-templates/tree/master/asr-automation-recovery/scripts) om bij te werken van de DNS-server met het nieuwe IP-adressen van de mislukte via > virtuele machines of koppelen van een load balancer op de via virtuele machine, indien nodig.


## <a name="doing-a-test-failover"></a>Een testfailover uitvoeren

Ga als volgt [deze richtlijnen](site-recovery-test-failover-to-azure.md) doet een test-failover.

![Plan voor herstel](./media/site-recovery-citrix-xenapp-and-xendesktop/citrix-tfo.png)


## <a name="doing-a-failover"></a>Een failover uitvoert

Ga als volgt [deze richtlijnen](site-recovery-failover.md) wanneer u een failover uitvoert.

## <a name="next-steps"></a>Volgende stappen

U kunt [meer](https://aka.ms/citrix-xenapp-xendesktop-with-asr) over het repliceren van Citrix XenApp en XenDesktop-implementaties in dit technische document. De richtlijnen voor het kijken [andere toepassingen repliceren](site-recovery-workload.md) met Site Recovery.
