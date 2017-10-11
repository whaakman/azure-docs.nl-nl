---
title: Een Citrix XenDesktop en XenApp implementatie van meerdere lagen met Azure Site Recovery repliceren | Microsoft Docs
description: In dit artikel wordt beschreven hoe beveiligen en herstellen van Citrix XenDesktop en XenApp implementaties met Azure Site Recovery.
services: site-recovery
documentationcenter: 
author: ponatara
manager: abhemraj
editor: 
ms.assetid: 9126f5e8-e9ed-4c31-b6b4-bf969c12c184
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: ponatara
ms.openlocfilehash: dc064352b1841ff346b705dc63186b12d79350b3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="replicate-a-multi-tier-citrix-xenapp-and-xendesktop-deployment-using-azure-site-recovery"></a>Een Citrix XenApp en XenDesktop implementatie van meerdere lagen met Azure Site Recovery repliceren

## <a name="overview"></a>Overzicht

Citrix XenDesktop is een bureaublad-virtualisatie-oplossing die bureaubladen en toepassingen als een ondemand-service aan een gebruiker een willekeurige plaats biedt. Met de FlexCast levering technologie kunt XenDesktop snel en veilig leveren toepassingen en bureaubladen voor gebruikers.
Vandaag de dag biedt Citrix XenApp geen elke ramp herstelfuncties.

Een goede noodhersteloplossing mag modellering van herstelplannen rond de bovenstaande complexe toepassingsarchitecturen en hebben de mogelijkheid aangepaste stappen voor het afhandelen van de toepassingstoewijzingen tussen verschillende lagen daarom bieden een éénkliks-toevoegen Controleer of de schermopname oplossing er in een noodsituatie leidt tot een lagere RTO.

Dit document bevat een stapsgewijze instructies voor het bouwen van een noodherstel voor uw on-premises Citrix XenApp implementaties op Hyper-V- en VMware vSphere-platforms. Dit document beschrijft ook hoe u een testfailover (drill disaster recovery) en niet-geplande failover naar Azure met herstelplannen, de ondersteunde configuraties en vereisten.


## <a name="prerequisites"></a>Vereisten

Voordat u begint, zorg er dan voor dat u het volgende weten:

1. [Een virtuele machine repliceren naar Azure](site-recovery-vmware-to-azure.md)
1. Hoe [ontwerpen van een netwerk herstel](site-recovery-network-design.md)
1. [Een testfailover uitvoeren naar Azure](site-recovery-test-failover-to-azure.md)
1. [U een failover naar Azure](site-recovery-failover.md)
1. Hoe [repliceren van een domeincontroller](site-recovery-active-directory.md)
1. Hoe [SQL-Server repliceren](site-recovery-sql.md)

## <a name="deployment-patterns"></a>Implementaties

Een farm Citrix XenApp en XenDesktop heeft meestal de volgende implementatie-patroon volgen:

**Patroon van de implementatie**

Citrix XenApp en XenDesktop implementatie met AD DNS-server, SQL databaseserver server, Citrix levering Controller, winkel, XenApp Master (VDA), Citrix XenApp licentieserver

![Implementatie-patroon 1](./media/site-recovery-citrix-xenapp-and-xendesktop/citrix-deployment.png)


## <a name="site-recovery-support"></a>Site Recovery-ondersteuning

In dit artikel wordt Citrix-implementaties op virtuele VMware-machines worden beheerd door vSphere 6.0-System Center VMM 2012 R2 zijn gebruikt voor het instellen van Noodherstel.

### <a name="source-and-target"></a>Bron en doel

**Scenario** | **Een secundaire site** | **Naar Azure**
--- | --- | ---
**Hyper-V** | Niet binnen het bereik | Ja
**VMware** | Niet binnen het bereik | Ja
**Fysieke server** | Niet binnen het bereik | Ja

### <a name="versions"></a>Versies
Klanten kunnen XenApp onderdelen implementeren als virtuele Machines waarop Hyper-V- of VMware of fysieke Servers. Azure Site Recovery kan fysieke en virtuele implementaties naar Azure te beveiligen.
Aangezien XenApp 7,7 of hoger wordt ondersteund in Azure, kunnen alleen implementaties met deze versies failover naar Azure voor herstel na noodgevallen of migratie.

### <a name="things-to-keep-in-mind"></a>Houd er rekening mee houden

1. Beveiliging en herstel van lokale implementaties met Server OS machines leveren XenApp gepubliceerde apps en XenApp gepubliceerd bureaubladen wordt ondersteund.

2. Beveiliging en herstel van on-premises implementaties bureaublad OS-machines met VDI Desktop leveren voor client virtuele bureaubladen, met inbegrip van Windows 10, wordt niet ondersteund. Dit is omdat het herstel van computers met bureaublad OS'es biedt geen ondersteuning voor ASR.  Bovendien sommige client virtueel bureaublad aroma's (bv. Windows 7) zijn nog niet ondersteund voor licentieverlening in Azure. [Meer informatie](https://azure.microsoft.com/pricing/licensing-faq/) over licentieverlening voor clientbureaubladen en serverdesktops in Azure.

3.  Azure Site Recovery kan niet repliceren en de bestaande lokale MCS of PV's klonen beveiligen.
U moet deze met Azure RM inrichten vanuit levering domeincontroller klonen opnieuw.

4. NetScaler kan niet worden beveiligd met Azure Site Recovery zoals NetScaler is gebaseerd op FreeBSD en Azure Site Recovery biedt geen ondersteuning voor beveiliging van het besturingssysteem FreeBSD. U moet implementeren en configureren van een nieuw toestel NetScaler van Azure-marktplaats na een failover naar Azure.


## <a name="replicating-virtual-machines"></a>Repliceren van virtuele machines

De volgende onderdelen van de Citrix XenApp-implementatie moeten worden beveiligd, zodat de replicatie en herstel.

* Beveiliging van AD DNS-server
* Beveiliging van SQL database-server
* Beveiliging van Citrix levering Controller
* Beveiliging van server winkel.
* Beveiliging van XenApp Master (VDA)
* Beveiliging van de licentieserver Citrix XenApp


**AD DNS-server-replicatie**

Raadpleeg [beveiligen van Active Directory en DNS met Azure Site Recovery](site-recovery-active-directory.md) op richtlijnen voor het repliceren van en het configureren van een domeincontroller in Azure.

**SQL Server databasereplicatie**

Raadpleeg [SQL-Server beveiligen met SQL Server-noodherstel en Azure Site Recovery](site-recovery-sql.md) voor gedetailleerde technische richtlijnen voor de aanbevolen opties voor het beveiligen van SQL-servers.

Ga als volgt [in deze richtlijnen](site-recovery-vmware-to-azure.md) naar het andere onderdeel virtuele machines repliceren naar Azure.

![Beveiliging van XenApp onderdelen](./media/site-recovery-citrix-xenapp-and-xendesktop/citrix-enablereplication.png)

**Reken- en netwerkinstellingen**

Nadat de computers zijn beveiligd (status wordt weergegeven als 'Beveiligde' onder gerepliceerde Items), de instellingen van de berekenings- en moeten worden geconfigureerd.
In de berekening en netwerk > Compute-eigenschappen, kunt u de Azure VM-naam en doel-grootte.
De naam om te voldoen aan de Azure-vereisten als u wilt wijzigen. U kunt ook bekijken en informatie over het doelnetwerk, subnet en IP-adres dat wordt toegewezen aan de Azure VM toevoegen.

Houd rekening met het volgende:

* U kunt het doel-IP-adres instellen. Als u geen adres opgeeft, maakt de machine waarvoor een failover is uitgevoerd, gebruik van DHCP. Als u een adres dat is niet beschikbaar bij een failover, werkt de failover niet. Hetzelfde doel-IP-adres kan worden gebruikt voor een testfailover als het adres beschikbaar is in het testfailovernetwerk.

* Voor de AD-en DNS-server kunt het lokale adres behouden u hetzelfde adres opgeven als de DNS-server voor het virtuele Azure-netwerk.

Het aantal netwerkadapters wordt bepaald door de grootte die u voor de virtuele doelmachine opgeeft. Dat werkt als volgt:

*   Als het aantal netwerkadapters op de bronmachine kleiner is dan of gelijk is aan het aantal adapters dat is toegestaan voor de grootte van de doelmachine, heeft het doel hetzelfde aantal adapters als de bron.
*   Als het aantal adapters voor de virtuele bronmachine meer is dan is toegestaan voor de doelgrootte, wordt het maximum voor de doelgrootte gebruikt.
* Als een bronmachine bijvoorbeeld twee netwerkadapters heeft en met de grootte van de doelmachine vier netwerkadapters worden ondersteund, heeft de doelcomputer twee adapters. Als de bronmachine twee adapters heeft, maar met de ondersteunde doelgrootte slechts één wordt ondersteund, heeft de doelcomputer slechts één adapter.
*   Als de virtuele machine meerdere netwerkadapters die worden deze allemaal verbonden met hetzelfde netwerk heeft.
*   Als de virtuele machine meerdere netwerkadapters heeft, wordt het eerste beheerpunt dat wordt weergegeven in de lijst met de standaard-netwerkadapter in de virtuele machine van Azure.


## <a name="creating-a-recovery-plan"></a>Een herstelplan maken

Nadat replicatie is ingeschakeld voor de virtuele machines XenApp-component, wordt de volgende stap is het maken van een herstelplan.
Een herstelplan groepen samen virtuele machines met vergelijkbare vereisten voor failover en herstel.  

**Stappen voor het maken van een herstelplan**

1. De XenApp onderdeel virtuele machines in het herstelplan toevoegen.
2. Klik op de herstelplannen -> + herstelplan. Geef een intuïtieve naam voor het herstelplan.
3. Voor virtuele VMware-machines: bron selecteren als processerver VMware, doel als Microsoft Azure en implementatiemodel als Resource Manager en klik op items selecteren.
4. Voor Hyper-V virtuele machines: bron selecteren als VMM-server, als Microsoft Azure en als Resource Manager-implementatiemodel zijn gericht en klik op items selecteren en selecteer vervolgens de implementatie XenApp virtuele machines.

### <a name="adding-virtual-machines-to-failover-groups"></a>Virtuele machines worden toegevoegd aan failover-groepen

Plannen voor herstel kunnen worden aangepast voor het toevoegen van failover-groepen voor specifieke opstartvolgorde scripts of handmatige acties. De volgende groepen moeten worden toegevoegd aan het herstelplan.

1. Failover Group1: AD DNS
2. Failover-Group2: SQL Server-VM's
2. Failover groep3: VDA Master installatiekopie VM
3. Failover Group4: De levering van Controller en de winkel server VMs


### <a name="adding-scripts-to-the-recovery-plan"></a>Scripts toe te voegen aan het herstelplan

Scripts kunnen worden uitgevoerd vóór of na een bepaalde groep in een plan voor herstel. Handmatige acties kunnen worden ook worden opgenomen en tijdens de failover is uitgevoerd.

Het aangepaste herstelplan lijkt op het onderstaande:

1. Failover Group1: AD DNS
2. Failover-Group2: SQL Server-VM's
3. Failover groep3: VDA Master installatiekopie VM

   >[!NOTE]     
   >Stap 4, 6 en 7 handmatig of script of acties die van toepassing zijn op alleen een lokale XenApp >-omgeving met MCS/PV's catalogussen.

4. De actie handmatig of script groep 3: master VDA VM de Master VDA VM afsluiten wanneer een failover naar Azure worden actief is. Voor het maken van nieuwe MCS catalogussen met behulp van Azure ARM die als host fungeert de master VDA VM is vereist om te worden gestopt (de toegewezen) staat. De virtuele machine van Azure-Portal wordt afgesloten.

5. Failover Group4: De levering van Controller en de winkel server VMs
6. Groep3 handmatig of script actie 1:

    ***Azure RM hostverbinding toevoegen***

    Azure ARM hostverbinding maken in de bezorging Controller machine voor het inrichten van nieuwe MCS catalogussen in Azure. Volg de stappen zoals wordt beschreven in dit [artikel](https://www.citrix.com/blogs/2016/07/21/connecting-to-azure-resource-manager-in-xenapp-xendesktop/).

7. Groep3 handmatig of script actie 2:

    ***MCS-catalogussen in Azure opnieuw maken***

    De bestaande MCS of PV's klonen op de primaire site wordt niet gerepliceerd naar Azure. U moet deze met behulp van de gerepliceerde master VDA en Azure ARM inrichten vanuit levering domeincontroller klonen opnieuw. Volg de stappen zoals wordt beschreven in dit [artikel](https://www.citrix.com/blogs/2016/09/12/using-xenapp-xendesktop-in-azure-resource-manager/) MCS catalogussen maken in Azure.

![Plan voor herstel voor XenApp onderdelen](./media/site-recovery-citrix-xenapp-and-xendesktop/citrix-recoveryplan.png)


   >[!NOTE]
   >U kunt scripts op de [locatie](https://github.com/Azure/azure-quickstart-templates/blob/>master/asr-automation-recovery/scripts) bijwerken van de DNS-server met de nieuwe IP-adressen van de mislukte via > virtuele machines of koppelen van een load balancer op de mislukte via de virtuele machine, indien nodig.


## <a name="doing-a-test-failover"></a>Een testfailover uitvoeren

Ga als volgt [in deze richtlijnen](site-recovery-test-failover-to-azure.md) een testfailover uitvoeren.

![Plan voor herstel](./media/site-recovery-citrix-xenapp-and-xendesktop/citrix-tfo.png)


## <a name="doing-a-failover"></a>U een failover uitvoert

Ga als volgt [in deze richtlijnen](site-recovery-failover.md) bij het uitvoeren van een failover.

## <a name="next-steps"></a>Volgende stappen

U kunt [meer](https://aka.ms/citrix-xenapp-xendesktop-with-asr) over Citrix XenApp en XenDesktop implementaties in deze white paper repliceren. Bekijk de richtlijnen voor [andere toepassingen repliceren](site-recovery-workload.md) met Site Recovery.
