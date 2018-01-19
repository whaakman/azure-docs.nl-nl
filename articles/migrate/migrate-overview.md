---
title: Over Azure Migrate | Microsoft Docs
description: Biedt een overzicht van de service Azure Migrate.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: overview
ms.date: 01/08/2018
ms.author: raynew
ms.openlocfilehash: 0bd3d7a9961e7a095684262ae1031f5a3ac0c3fb
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2018
---
# <a name="about-azure-migrate"></a>Over Azure Migrate

De service Azure Migrate beoordeelt on-premises werkbelastingen voor migratie naar Azure. De service beoordeelt de geschiktheid voor migratie en de grootte op basis van prestaties, en biedt kostenschattingen voor het uitvoeren van uw on-premises machines in Azure. Als u lift-and-shift-migraties overweegt of zich nog in de beginfase van de evaluatie van de migratie bevindt, is deze service voor u geschikt. Na de evaluatie kunt u services zoals Azure Site Recovery en Azure Database Migration gebruiken om de machines te migreren naar Azure.

> [!NOTE]
> Azure Migrate is momenteel een preview-versie en biedt ondersteuning voor productiewerkbelastingen.

## <a name="why-use-azure-migrate"></a>Waarom Azure Migrate gebruiken?

Azure Migrate helpt u bij het volgende:

- **Azure-gereedheid beoordelen**: bepalen of uw on-premises machines geschikt zijn om te worden uitgevoerd in Azure. 
- **Aanbevelingen voor de grootte geven**: aanbevolen groottes voor virtuele Azure-machines na de migratie, op basis van de prestatiegeschiedenis van de on-premises virtuele machines. 
- **Schatting van maandelijkse kosten**: de geschatte kosten voor het uitvoeren van on-premises machines in Azure.  
- **Migreren met hoge betrouwbaarheid**: visualiseren afhankelijkheden van lokale computers maken van groepen computers die u wilt beoordelen en tegelijk migreren. U kunt nauwkeurig de afhankelijkheden weergeven voor een specifieke machine of voor alle machines in een groep.

## <a name="current-limitations"></a>Huidige beperkingen

- Op dit moment kunt u on-premises virtuele machines (VM's) met VMware beoordelen voor migratie naar virtuele Azure-machines.

> [!NOTE]
> Ondersteuning voor Hyper-V staat in de roadmap en komt binnenkort beschikbaar. Ondertussen raden we u aan [Azure Site Recovery Deployment Planner](http://aka.ms/asr-dp-hyperv-doc) te gebruiken om de migratie van Hyper-V-werkbelastingen te plannen. 

- U kunt maximaal 1000 virtuele machines detecteren in een enkele detectieronde en maximaal 1500 virtuele machines in een enkel project. Bovendien kunt u maximaal 400 VM's beoordelen in een enkele beoordeling. Als u meer moet detecteren of beoordelen, kunt u het aantal detecties of beoordelingen verhogen. [Meer informatie](how-to-scale-assessment.md).
- Virtuele machines die u wilt beoordelen, moeten worden beheerd door een vCenter Server, versie 5.5, 6.0 of 6.5.
- U kunt een Azure Migrate-project alleen maken in de regio West-centraal VS. Dit is echter niet van invloed op de mogelijkheid om uw migratie te plannen voor een andere Azure-doellocatie. De locatie van het migratieproject wordt alleen gebruikt om de gedetecteerde metagegevens uit de on-premises omgeving op te slaan.
- Azure Migrate biedt voor migratiebeoordeling alleen ondersteuning voor beheerde schijven.

## <a name="what-do-i-need-to-pay-for"></a>Waar moet ik voor betalen?

Azure Migrate is zonder extra kosten beschikbaar. Tijdens de openbare preview gelden echter extra kosten voor het gebruik van functies voor visualisatie van afhankelijkheden. Om [visualisatie van afhankelijkheden](concepts-dependency-visualization.md) te ondersteunen, creëert Azure Migrate standaard een Log Analytics-werkruimte. Als u visualisatie van afhankelijkheden gebruikt of de werkruimte buiten Azure Migrate gebruikt, moet u betalen voor het gebruik van de werkruimte. [Meer informatie](https://azure.microsoft.com/en-us/pricing/details/insight-analytics/) over de kosten. Zodra de service algemeen beschikbaar is, vervallen de kosten voor het gebruik van functies voor visualisatie van afhankelijkheden.


## <a name="whats-in-an-assessment"></a>Wat maakt er deel uit van een evaluatie?

Een beoordeling helpt u bij het identificeren van de Azure geschiktheid van de lokale virtuele machines, juiste formaat aanbevelingen krijgen en uw kosten schattingen voor het uitvoeren van de virtuele machines in Azure. Azure Migrate-evaluaties zijn gebaseerd op de instellingen in de volgende tabel. U kunt deze eigenschappen aanpassen in het portaal van Azure Migrate. 

**Eigenschap** | **Details**
--- | ---
**Doellocatie** | De Azure-locatie waarnaar u wilt migreren. De doellocatie is standaard ingesteld op VS - west 2. 
**Opslagredundantie** | Het type opslag dat na de migratie door de virtuele Azure-machines wordt gebruikt. LRS is de standaardinstelling.
**Prijsplannen** | Bij de beoordeling wordt er rekening mee gehouden of u bent geregistreerd voor Software Assurance en de [Azure Hybrid User Benefit](https://azure.microsoft.com/pricing/hybrid-use-benefit/) kunt gebruiken. Er wordt ook rekening gehouden met Azure-aanbiedingen die moeten worden toegepast en u kunt kortingen (%) voor specifieke abonnementen opgeven die worden toegepast boven op de aanbieding. 
**Prijscategorie** | U kunt de [prijscategorie (basis/standaard)](../virtual-machines/windows/sizes-general.md) van virtuele Azure-machines opgeven. Dit helpt u te migreren naar een geschikte Azure-VM-serie op basis van het feit of u zich in een productieomgeving bevindt. Standaard wordt de categorie [Standaard](../virtual-machines/windows/sizes-general.md) gebruikt.
**Prestatiegeschiedenis** | Azure Migrate evalueert de prestaties van on-premises machines standaard aan de hand van de geschiedenis van een maand, met een percentielwaarde van 95%. U kunt deze instelling wijzigen.
**Comfortfactor** | Tijdens de evaluatie houdt Azure Migrate rekening met een buffer (comfortfactor). Deze buffer wordt toegepast boven op de gegevens over machinegebruik voor virtuele machines (CPU, geheugen, schijf en netwerk). De comfortfactor houdt rekening met factoren zoals seizoensgebonden gebruik, een korte prestatiegeschiedenis en een mogelijke toename in gebruik in de toekomst.<br/><br/> Een virtuele machine met 10 kernen en een gebruik van 20% komt bijvoorbeeld gewoonlijk overeen met een virtuele machine met 2 kernen. Met een comfortfactor van 2,0x is het resultaat echter een virtuele machine met 4 kernen. De standaardinstelling voor comfort is 1,3x.


## <a name="how-does-azure-migrate-work"></a>Hoe werkt Azure Migrate?

1.  U maakt een Azure Migrate-project.
2.  Azure Migrate gebruikt een on-premises virtuele machine, het collectorapparaat genoemd, om informatie over uw on-premises machines te detecteren. U maakt dit apparaat door het installatiebestand in de Open Virtualization Appliance-indeling (.ova) te downloaden en als virtuele machine te importeren in uw on-premises VMware vCenter Server.
3.  U verbinding maken met de virtuele machine met consoleverbinding in VMware vCenter Server, een nieuw wachtwoord opgeven voor de virtuele machine tijdens het verbinding maken en vervolgens de collector-toepassing in de virtuele machine starten detectie uitgevoerd.
4.  De collector verzamelt metagegevens van de virtuele machines met behulp van PowerCLI-cmdlets van VMware. Detectie vindt plaats zonder agent en er wordt niets op de VMware-hosts of virtuele machines geïnstalleerd. De verzamelde metagegevens bevatten informatie over de virtuele machines (kernen, geheugen, schijven, schijfgroottes en netwerkadapters). Er worden ook prestatiegegevens van de virtuele machines verzameld, met inbegrip van CPU- en geheugengebruik, schijf-IOPS, schijfdoorvoer (MBps) en netwerkuitvoer (MBps).
5.  De metagegevens worden doorgegeven aan het Azure Migrate-project. U kunt ze bekijken in Azure Portal.
6.  Voor evaluatiedoeleinden deelt u de gedetecteerde virtuele machines in groepen in. U kunt bijvoorbeeld virtuele machines groeperen waarin dezelfde applicatie wordt uitgevoerd. Voor een nauwkeurigere groepering kunt u afhankelijkheidsvisualisatie gebruiken om afhankelijkheden te bekijken van een specifieke computer, of voor alle computers in een groep, en de groep verfijnen.
7.  Als uw groep is gevormd maakt u een beoordeling van de groep. 
8.  Nadat de evaluatie is voltooid, kunt u deze weergeven in de portal of downloaden in Excel-indeling.



  ![Azure Planner-architectuur](./media/migration-planner-overview/overview-1.png)

## <a name="what-are-the-port-requirements"></a>Wat zijn de poortvereisten?

De tabel geeft een overzicht van de poorten die nodig zijn voor communicatie met Azure Migrate.

|Onderdeel          |Voor communicatie met     |Vereiste poort  |Reden   |
|-------------------|------------------------|---------------|---------|
|Collector          |Azure Migrate-service   |TCP 443        |De collector maakt verbinding met de service via SSL-poort 443|
|Collector          |vCenter Server          |Standaard 9443   | Standaard maakt de collector verbinding met de vCenter-server op poort 9443. Als de server op een andere poort luistert, moet deze worden geconfigureerd als uitgaande poort in de collector-VM. |
|On-premises virtuele machine     | Overzicht van Operations Management Suite (OMS)          |[TCP 443](../log-analytics/log-analytics-windows-agent.md) |De MMA-agent gebruikt TCP 443 om verbinding te maken met Log Analytics. U hebt deze poort alleen nodig als u de functie voor visualisatie van afhankelijkheden gebruikt en de Microsoft Monitoring Agent-agent (MMA) installeert. |


  
## <a name="what-happens-after-assessment"></a>Wat gebeurt er na de evaluatie?

Nadat u de on-premises machines hebt beoordeeld voor migratie met de Azure Migrate-service, kunt u een aantal hulpprogramma's gebruiken om de migratie uit te voeren:

- **Azure Site Recovery**: U kunt Azure Site Recovery als volgt gebruiken om te migreren naar Azure:
  - Bereid Azure-resources voor, waaronder een Azure-abonnement, een virtueel Azure-netwerk en een opslagaccount.
  - Bereid uw on-premises VMware-servers voor op de migratie. U controleert de vereisten voor ondersteuning van VMware voor Site Recovery, bereidt VMware-servers voor op detectie en bereidt de installatie van de Site Recovery Mobility-service voor op de virtuele machines die u wilt migreren. 
  - Configureer de migratie. U stelt een Recovery Services-kluis in, configureert migratie-instellingen voor bron en doel, stelt een replicatiebeleid op en schakelt replicatie in. U kunt een herstelanalyse uitvoeren om te controleren of de migratie van een virtuele machine naar Azure goed werkt.
  - Voer een failover uit om on-premises machines te migreren naar Azure. 
  - [Meer informatie](../site-recovery/tutorial-migrate-on-premises-to-azure.md) vindt u in de zelfstudie Site Recovery-migratie.

- **Azure Database Migration**: Als uw on-premises machines een database zoals SQL Server, MySQL of Oracle uitvoeren, kunt u de Azure Database Migration Service gebruiken om ze te migreren naar Azure. [Meer informatie](https://azure.microsoft.com/campaigns/database-migration/).



## <a name="next-steps"></a>Volgende stappen 
[Volg een zelfstudie](tutorial-assessment-vmware.md) om een evaluatie uit te voeren voor een on-premises virtuele machine met VMware.