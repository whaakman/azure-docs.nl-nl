---
title: Over Azure Migrate | Microsoft Docs
description: Biedt een overzicht van de service Azure Migrate.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: overview
ms.date: 02/26/2018
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 45eac1d1ecb173ba0a62ab13f47b7ee6e12f7af3
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/27/2018
---
# <a name="about-azure-migrate"></a>Over Azure Migrate

De service Azure Migrate beoordeelt on-premises workloads voor migratie naar Azure. De service beoordeelt de geschiktheid voor migratie van on-premises machines naar Azure en de grootte op basis van prestaties. Daarnaast geeft de service kostenschattingen voor het uitvoeren van uw on-premises machines in Azure. Als u lift-and-shift-migraties overweegt of zich nog in de beginfase van de evaluatie van de migratie bevindt, is deze service geschikt voor u. Na de evaluatie kunt u services zoals [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview) en [Azure Database Migration Service](https://docs.microsoft.com/azure/dms/dms-overview) gebruiken om de machines te migreren naar Azure.

## <a name="why-use-azure-migrate"></a>Waarom Azure Migrate gebruiken?

Azure Migrate helpt u bij het volgende:

- **Azure-gereedheid beoordelen**: bepalen of uw on-premises machines geschikt zijn om te worden uitgevoerd in Azure. 
- **Aanbevelingen voor de grootte geven**: aanbevolen groottes voor virtuele Azure-machines na de migratie, op basis van de prestatiegeschiedenis van de on-premises virtuele machines. 
- **Schatting van maandelijkse kosten**: de geschatte kosten voor het uitvoeren van on-premises machines in Azure.  
- **Migreren met hoge betrouwbaarheid**: visualiseren afhankelijkheden van lokale computers maken van groepen computers die u wilt beoordelen en tegelijk migreren. 

## <a name="current-limitations"></a>Huidige beperkingen

- Op dit moment kunt u alleen on-premises virtuele VMware-machines (VMware-VM's) beoordelen voor migratie naar virtuele Azure-machines. De VMware-VM's moeten worden beheerd door een vCenter Server (versie 5.5, 6.0 of 6.5)

> [!NOTE]
> Ondersteuning voor Hyper-V staat in de roadmap en komt binnenkort beschikbaar. Ondertussen raden we u aan [Azure Site Recovery Deployment Planner](http://aka.ms/asr-dp-hyperv-doc) te gebruiken om de migratie van Hyper-V-werkbelastingen te plannen. 

- U kunt maximaal 1000 virtuele machines detecteren in een enkele detectieronde en maximaal 1500 virtuele machines in een enkel project. Bovendien kunt u maximaal 400 VM's beoordelen in een enkele beoordeling. Als u meer moet detecteren of beoordelen, kunt u het aantal detecties of beoordelingen verhogen. [Meer informatie](how-to-scale-assessment.md).
- U kunt een Azure Migrate-project alleen maken in de regio West-centraal VS of Oost VS. Dit is echter niet van invloed op de mogelijkheid om uw migratie te plannen voor een andere Azure-doellocatie. De locatie van het migratieproject wordt alleen gebruikt om de gedetecteerde metagegevens uit de on-premises omgeving op te slaan.
- Azure Migrate biedt voor migratiebeoordeling alleen ondersteuning voor beheerde schijven.

## <a name="what-do-i-need-to-pay-for"></a>Waar moet ik voor betalen?

Meer informatie over prijzen voor Azure Migrate vindt u [hier](https://azure.microsoft.com/en-in/pricing/details/azure-migrate/).


## <a name="whats-in-an-assessment"></a>Waaruit bestaat een evaluatie?

Een beoordeling helpt u bij het identificeren van de Azure geschiktheid van de lokale virtuele machines, juiste formaat aanbevelingen krijgen en uw kosten schattingen voor het uitvoeren van de virtuele machines in Azure. Beoordelingen kunnen op basis van uw behoeften worden aangepast door de eigenschappen van de beoordeling te wijzigen. Hieronder vindt u de eigenschappen waarmee rekening wordt gehouden wanneer er een beoordeling wordt gemaakt. 

**Eigenschap** | **Details**
--- | ---
**Doellocatie** | De Azure-locatie waarnaar u wilt migreren.<br/><br/>Azure Migrate ondersteunt momenteel 30 regio's, waaronder Australië - oost, Australië - zuidoost, Brazilië - zuid, Canada - centraal, Canada - oost, Centraal-India, VS - midden, China - oost, China - noord, Oost-Azië, VS - oost, Duitsland - centraal, Duitsland - noordoost, VS - oost 2, Japan - oost, Japan - west, Korea - midden, Korea - zuid, Noord-centraal VS, Noord-Europa, Zuid-centraal VS , Zuidoost-Azië, Zuid-India, UK - zuid, UK - west, West-centraal VS, West-Europa, India - west, VS - west en VS - west 2. De doellocatie is standaard ingesteld op VS - west 2. 
**Opslagredundantie** | Het type [opslagredundantie](https://docs.microsoft.com/azure/storage/common/storage-redundancy) dat na de migratie door de virtuele Azure-machines wordt gebruikt. De standaardinstelling is lokaal redundante opslag (LRS). Houd er rekening mee dat Azure Migrate alleen beoordelingen op basis van beheerde schijven ondersteunt en dat beheerde schijven alleen LRS ondersteunen. Daarom heeft het kenmerk momenteel alleen de LRS-optie. 
**Criterium voor het aanpassen van de grootte** | Het door Azure Migrate te gebruiken criterium om VM's op de juiste grootte te maken voor Azure. U kunt de grootte aanpassen op basis van de *prestatiegeschiedenis* van de on-premises VM's of voor de grootte van de VM's *Zoals on-premises* opgeven in Azure. Bij deze optie wordt geen rekening gehouden met de prestatiegeschiedenis. De standaardwaarde is dat de grootte wordt aangepast op basis van de prestaties.
**Prijsplannen** | Voor de berekening van de kosten wordt er in een beoordeling nagegaan of u Software Assurance hebt en in aanmerking komt voor [Azure Hybrid Benefit](https://azure.microsoft.com/pricing/hybrid-use-benefit/). Er wordt ook rekening gehouden met [Azure-aanbiedingen](https://azure.microsoft.com/support/legal/offer-details/) waar u eventueel gebruik van maakt en u kunt kortingen (%) voor specifieke abonnementen opgeven die worden toegepast boven op de aanbieding. 
**Prijscategorie** | U kunt de [prijscategorie (Basic/Standard)](../virtual-machines/windows/sizes-general.md) van de Azure-VM's opgeven. Als u bijvoorbeeld van plan bent om een productieomgeving te migreren, doet u er waarschijnlijk goed aan te kiezen voor de categorie Standard. Hiermee beschikt u over VM's met lage latentie, maar zijn de kosten mogelijk wel hoger. Hebt u daarentegen een ontwikkel-/testomgeving, dan is de categorie Basic wellicht de juiste keuze. Hierbij krijgt u VM's met een hogere latentie, maar lagere kosten. Standaard wordt de categorie [Standard](../virtual-machines/windows/sizes-general.md) gebruikt.
**Prestatiegeschiedenis** | Alleen van toepassing als voor het bepalen van de grootte wordt uitgegaan van de prestaties. Azure Migrate evalueert de prestaties van on-premises machines standaard aan de hand van de prestatiegeschiedenis van de afgelopen dag, met een percentielwaarde van 95%. U kunt deze waarden wijzigen in de beoordelingseigenschappen. 
**Comfortfactor** | Tijdens de evaluatie houdt Azure Migrate rekening met een buffer (comfortfactor). Deze buffer wordt toegepast boven op de gegevens over machinegebruik voor VM's (CPU, geheugen, schijf en netwerk). De comfortfactor houdt rekening met factoren zoals seizoensgebonden gebruik, een korte prestatiegeschiedenis en een mogelijke gebruikstoename in de toekomst.<br/><br/> Een VM met 10 kernen en een gebruik van 20% komt bijvoorbeeld gewoonlijk overeen met een VM met 2 kernen. Met een comfortfactor van 2,0x is het resultaat echter een VM met 4 kernen. De standaardinstelling voor comfort is 1,3x.


## <a name="how-does-azure-migrate-work"></a>Hoe werkt Azure Migrate?

1.  U maakt een Azure Migrate-project.
2.  Azure Migrate gebruikt een on-premises VM, het collectorapparaat genoemd, om informatie over uw on-premises machines te detecteren. U maakt dit apparaat door het installatiebestand in de Open Virtualization Appliance-indeling (.ova) te downloaden en als virtuele machine te importeren in uw on-premises VMware vCenter Server.
3.  U verbinding maken met de virtuele machine met consoleverbinding in VMware vCenter Server, een nieuw wachtwoord opgeven voor de virtuele machine tijdens het verbinding maken en vervolgens de collector-toepassing in de virtuele machine starten detectie uitgevoerd.
4.  De collector verzamelt metagegevens van de VM's met behulp van PowerCLI-cmdlets van VMware. Detectie vindt plaats zonder agent en er wordt niets op de VMware-hosts of VM's geïnstalleerd. De verzamelde metagegevens bevatten informatie over de VM's (kernen, geheugen, schijven, schijfgroottes en netwerkadapters). Er worden ook prestatiegegevens van de VM's verzameld, met inbegrip van CPU- en geheugengebruik, schijf-IOPS, schijfdoorvoer (MBps) en netwerkuitvoer (MBps).
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
|On-premises VM     | Overzicht van Operations Management Suite (OMS)          |[TCP 443](../log-analytics/log-analytics-windows-agent.md) |De MMA-agent gebruikt TCP 443 om verbinding te maken met Log Analytics. U hebt deze poort alleen nodig als u de functie voor visualisatie van afhankelijkheden gebruikt en de Microsoft Monitoring Agent-agent (MMA) installeert. |


  
## <a name="what-happens-after-assessment"></a>Wat gebeurt er na de evaluatie?

Nadat u de on-premises machines hebt beoordeeld voor migratie met de Azure Migrate-service, kunt u een aantal hulpprogramma's gebruiken om de migratie uit te voeren:

- **Azure Site Recovery**: U kunt Azure Site Recovery als volgt gebruiken om te migreren naar Azure:
  - Bereid Azure-resources voor, waaronder een Azure-abonnement, een virtueel Azure-netwerk en een opslagaccount.
  - Bereid uw on-premises VMware-servers voor op de migratie. U controleert de VMware-ondersteuningsvereisten voor Site Recovery, bereidt VMware-servers voor op detectie en bereidt de installatie van de Site Recovery Mobility-service voor op de VM's die u wilt migreren. 
  - Configureer de migratie. U stelt een Recovery Services-kluis in, configureert migratie-instellingen voor bron en doel, stelt een replicatiebeleid op en schakelt replicatie in. U kunt een herstelanalyse uitvoeren om te controleren of de migratie van een VM naar Azure goed werkt.
  - Voer een failover uit om on-premises machines te migreren naar Azure. 
  - [Meer informatie](../site-recovery/tutorial-migrate-on-premises-to-azure.md) vindt u in de zelfstudie Site Recovery-migratie.

- **Azure Database Migration**: Als uw on-premises machines een database zoals SQL Server, MySQL of Oracle uitvoeren, kunt u de Azure Database Migration Service gebruiken om ze te migreren naar Azure. [Meer informatie](https://azure.microsoft.com/campaigns/database-migration/).



## <a name="next-steps"></a>Volgende stappen 
[Volg een zelfstudie](tutorial-assessment-vmware.md) om een evaluatie uit te voeren voor een on-premises VM met VMware.