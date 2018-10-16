---
title: Over Azure Migrate | Microsoft Docs
description: Biedt een overzicht van de service Azure Migrate.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: overview
ms.date: 09/25/2018
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: d2a8885ffb9148d408eff0e8a7d2ef09121e5359
ms.sourcegitcommit: 51a1476c85ca518a6d8b4cc35aed7a76b33e130f
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2018
ms.locfileid: "47162031"
---
# <a name="about-azure-migrate"></a>Over Azure Migrate

De service Azure Migrate beoordeelt on-premises workloads voor migratie naar Azure. De service beoordeelt de geschiktheid voor migratie van on-premises machines en stelt de grootte in op basis van prestaties. Daarnaast geeft de service kostenschattingen voor het uitvoeren van uw on-premises machines in Azure. Als u lift-and-shift-migraties overweegt of zich nog in de beginfase van de evaluatie van de migratie bevindt, is deze service geschikt voor u. Na de evaluatie kunt u services zoals [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview) en [Azure Database Migration Service](https://docs.microsoft.com/azure/dms/dms-overview) gebruiken om de machines te migreren naar Azure.

## <a name="why-use-azure-migrate"></a>Waarom Azure Migrate gebruiken?

Azure Migrate helpt u bij het volgende:

- **Azure-gereedheid beoordelen**: bepalen of uw on-premises machines geschikt zijn om te worden uitgevoerd in Azure.
- **Aanbevelingen voor de grootte geven**: aanbevolen groottes voor virtuele Azure-machines na de migratie, op basis van de prestatiegeschiedenis van de on-premises virtuele machines.
- **Schatting van maandelijkse kosten**: de geschatte kosten voor het uitvoeren van on-premises machines in Azure.  
- **Migreren met hoge betrouwbaarheid**: visualiseren afhankelijkheden van lokale computers maken van groepen computers die u wilt beoordelen en tegelijk migreren.

## <a name="current-limitations"></a>Huidige beperkingen

- Op dit moment kunt u alleen on-premises virtuele VMware-machines (VMware-VM's) beoordelen voor migratie naar virtuele Azure-machines. De VMware-VM's moeten worden beheerd door een vCenter Server (versie 5.5, 6.0 of 6.5).
- Als u Hyper-VM's en fysieke servers wilt beoordelen, gebruikt u de [Azure Site Recovery Deployment Planner](http://aka.ms/asr-dp-hyperv-doc) voor Hyper-V, en onze [hulpprogramma's van partners](https://azure.microsoft.com/migration/partners/) voor fysieke computers.
- U kunt maximaal 1500 VM's detecteren in een enkele detectieronde en maximaal 1500 VM's in een enkel project. Bovendien kunt u maximaal 1500 VM's beoordelen in een enkele beoordeling.
- Als u een grotere omgeving wilt detecteren, kunt u de detectie splitsen en meerdere projecten maken. [Meer informatie](how-to-scale-assessment.md). Azure Migrate ondersteunt maximaal 20 projecten per abonnement.
- Azure Migrate biedt voor migratiebeoordeling alleen ondersteuning voor beheerde schijven.
- U kunt een Azure Migrate-project alleen maken in de geografie van de Verenigde Staten. Dit heeft geen invloed op uw mogelijkheid voor het plannen van de migratie naar een Azure-doellocatie. De geografie van het migratieproject wordt alleen gebruikt om de detecteerde metagegevens uit de on-premises omgeving op te slaan. [Meer informatie](https://docs.microsoft.com/azure/migrate/resources-faq#discovery-and-assessment) over de metagegevens die zijn verzameld door Azure Migrate. De verzamelde metagegevens worden opgeslagen in een van de regio's in de geselecteerde [Azure-geografie](https://azure.microsoft.com/global-infrastructure/geographies/):

**Geografie** | **Regio's**
--- | ---
Verenigde Staten | US - west-centraal, US - oost

## <a name="what-do-i-need-to-pay-for"></a>Waar moet ik voor betalen?

Meer informatie over prijzen voor Azure Migrate vindt u [hier](https://azure.microsoft.com/pricing/details/azure-migrate/).


## <a name="whats-in-an-assessment"></a>Waaruit bestaat een evaluatie?

Instellingen voor evaluatie kunnen worden aangepast op basis van uw behoeften. Evaluatie-eigenschappen worden samengevat in de volgende tabel.

**Eigenschap** | **Details**
--- | ---
**Doellocatie** | De Azure-locatie waarnaar u wilt migreren.<br/><br/>Azure Migrate ondersteunt momenteel 30 regio's. [Regio's controleren](https://azure.microsoft.com/global-infrastructure/services/). De doellocatie is standaard ingesteld op US - west 2.
**Opslagtype** | het type schijf dat u wilt toewijzen in Azure. Dit is van toepassing wanneer het criterium voor het aanpassen van de grootte **zoals on-premises** is. U kunt het doeltype van de schijf opgeven als Premium-beheerde schijven (basisinstelling) of Standard-beheerde schijven. Voor het aanpassen van de grootte op basis van prestaties geldt dat de aanbeveling automatisch wordt gedaan op basis van de prestatiegegevens van de virtuele machines.
**Criterium voor het aanpassen van de grootte** | U kunt de grootte aanpassen op basis van de **prestatiegeschiedenis** van de on-premises VM's of **zoals on-premises** (standaardinstelling). Bij deze optie wordt geen rekening gehouden met de prestatiegeschiedenis.
**Azure-aanbieding** | De [Azure-aanbieding](https://azure.microsoft.com/support/legal/offer-details/) waarop u bent geregistreerd. Azure Migrate maakt dienovereenkomstig een schatting van de kosten.
**Azure Hybrid Benefit** | Geef op of u Software Assurance hebt en in aanmerking komt voor [Azure Hybrid Benefit](https://azure.microsoft.com/pricing/hybrid-use-benefit/) voor de korting op de kosten.
**Gereserveerde instanties** |  Of u [gereserveerde instanties](https://azure.microsoft.com/pricing/reserved-vm-instances/) in Azure hebt. Azure Migrate maakt dienovereenkomstig een schatting van de kosten.
**VM tijd actief** | De tijdsduur waarvoor de virtuele machines in Azure worden uitgevoerd. Schattingen van de kosten worden dienovereenkomstig uitgevoerd.
**Prijscategorie** | U kunt de [prijscategorie (Basic/Standard)](../virtual-machines/windows/sizes-general.md) van de Azure-VM's opgeven. Als u bijvoorbeeld van plan bent om een productieomgeving te migreren, doet u er waarschijnlijk goed aan te kiezen voor de categorie Standard. Hiermee beschikt u over VM's met lage latentie, maar zijn de kosten mogelijk wel hoger. U kunt aan de andere kant in een testomgeving de categorie Basic gebruiken met een hogere latentie en lagere kosten. Standaard wordt de categorie [Standaard](../virtual-machines/windows/sizes-general.md) gebruikt.
**Prestatiegeschiedenis** | Azure Migrate evalueert de prestaties van on-premises machines standaard aan de hand van de prestatiegeschiedenis van de afgelopen dag, met een percentielwaarde van 95%.
**VM-reeks** | De VM-reeks die voor schattingen van grootte wordt gebruikt. Als u bijvoorbeeld een productieomgeving hebt die u niet gaat migreren naar de A-serie van virtuele machines in Azure, kunt u de A-serie uitsluiten van de lijst of reeks. Grootte is alleen gebaseerd op de geselecteerde reeks.   
**Comfortfactor** | Tijdens de evaluatie houdt Azure Migrate rekening met een buffer (comfortfactor). Deze buffer wordt toegepast boven op de gegevens over machinegebruik voor VM's (CPU, geheugen, schijf en netwerk). De comfortfactor houdt rekening met factoren zoals seizoensgebonden gebruik, een korte prestatiegeschiedenis en een mogelijke gebruikstoename in de toekomst.<br/><br/> Een VM met 10 kernen en een gebruik van 20% komt bijvoorbeeld gewoonlijk overeen met een VM met 2 kernen. Met een comfortfactor van 2,0x is het resultaat echter een VM met 4 kernen. De standaardinstelling voor comfort is 1,3x.


## <a name="how-does-azure-migrate-work"></a>Hoe werkt Azure Migrate?

1.  U maakt een Azure Migrate-project.
2.  Azure Migrate gebruikt een on-premises VM, het collectorapparaat genoemd, om informatie over uw on-premises machines te detecteren. U maakt dit apparaat door het installatiebestand in de Open Virtualization Appliance-indeling (.ova) te downloaden en als virtuele machine te importeren in uw on-premises VMware vCenter Server.
3. U maakt verbinding met de virtuele machine van vCenter Server en geeft hiervoor een nieuw wachtwoord op tijdens het verbinden.
4. U kunt de collector uitvoeren op de virtuele machine om detectie op te starten.
5. De collector verzamelt metagegevens van de VM's met behulp van PowerCLI-cmdlets van VMware. Detectie vindt plaats zonder agent en er wordt niets op de VMware-hosts of VM's geïnstalleerd. De verzamelde metagegevens bevatten informatie over de VM's (kernen, geheugen, schijven, schijfgroottes en netwerkadapters). Er worden ook prestatiegegevens van de VM's verzameld, met inbegrip van CPU- en geheugengebruik, schijf-IOPS, schijfdoorvoer (MBps) en netwerkuitvoer (MBps).
5.  De metagegevens worden doorgegeven aan het Azure Migrate-project. U kunt ze bekijken in Azure Portal.
6.  Voor evaluatiedoeleinden deelt u de gedetecteerde virtuele machines in groepen in. U kunt bijvoorbeeld virtuele machines groeperen waarin dezelfde applicatie wordt uitgevoerd. Voor een nauwkeurigere groepering kunt u afhankelijkheidsvisualisatie gebruiken om afhankelijkheden te bekijken van een specifieke computer, of voor alle computers in een groep, en de groep verfijnen.
7.  Nadat een groep is gedefinieerd, kunt u hiervoor een evaluatie maken.
8.  Nadat de evaluatie is voltooid, kunt u deze weergeven in de portal of downloaden in Excel-indeling.

  ![Azure Migrate-architectuur](./media/migration-planner-overview/overview-1.png)

## <a name="what-are-the-port-requirements"></a>Wat zijn de poortvereisten?

De tabel geeft een overzicht van de poorten die nodig zijn voor communicatie met Azure Migrate.

Onderdeel | Communiceert met |  Details
--- | --- |---
Collector  | Azure Migrate-service | De collector maakt verbinding met de service via SSL-poort 443.
Collector | vCenter Server | Standaard maakt de collector verbinding met de VMware vCenter Server op poort 443. Als de server op een andere poort luistert, moet deze worden geconfigureerd als uitgaande poort in de collector-VM.
On-premises VM | Log Analytics-werkruimte | [TCP 443] | [De Microsoft Monitoring Agent (MMA)](../log-analytics/log-analytics-windows-agent.md) gebruikt TCP-poort 443 om verbinding te maken met Log Analytics. U hebt deze poort alleen nodig als u de functie voor visualisatie van afhankelijkheden gebruikt, die de MMA-agent vereist.


## <a name="what-happens-after-assessment"></a>Wat gebeurt er na de evaluatie?

Nadat u de on-premises machines hebt beoordeeld, kunt u een aantal hulpprogramma's gebruiken om de migratie uit te voeren:

- **Azure Site Recovery**: U kunt Azure Site Recovery gebruiken om te migreren naar Azure. Hiervoor moet u [de Azure-onderdelen voorbereiden](../site-recovery/tutorial-prepare-azure.md) die u nodig hebt, met inbegrip van een opslagaccount en een virtueel netwerk. On-premises moet u [uw VMware-omgeving voorbereiden](../site-recovery/vmware-azure-tutorial-prepare-on-premises.md). Als alles voorbereid is, stelt en schakelt u replicatie naar Azure in en migreert u de virtuele machines. [Meer informatie](../site-recovery/vmware-azure-tutorial.md).
- **Azure Database Migration**: Als uw on-premises machines een database zoals SQL Server, MySQL of Oracle uitvoeren, kunt u de [Azure Database Migration Service](../dms/dms-overview.md) gebruiken om ze te migreren naar Azure.


## <a name="next-steps"></a>Volgende stappen

- [Volg de zelfstudie](tutorial-assessment-vmware.md) om een evaluatie uit te voeren voor een on-premises VM met VMware.
- [Bekijk de veelgestelde vragen](resources-faq.md) over Azure Migrate.
