---
title: Evaluatie berekeningen in Azure Migrate | Microsoft Docs
description: Hierin wordt een overzicht gegeven van de evaluatie berekeningen in de Azure Migrate-service.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 07/15/2019
ms.author: raynew
ms.openlocfilehash: 87aa48c992b7887ce86c43cac29910dcc57b3e91
ms.sourcegitcommit: b2db98f55785ff920140f117bfc01f1177c7f7e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/16/2019
ms.locfileid: "68234296"
---
# <a name="assessment-calculations"></a>Beoordelingsberekeningen

[Azure migrate](migrate-services-overview.md) biedt een centrale hub voor het bijhouden van de detectie, beoordeling en migratie van uw on-premises apps en workloads, en persoonlijke/open bare Cloud instanties naar Azure. De hub biedt Azure Migrate-hulpprogram ma's voor evaluatie en migratie, evenals onafhankelijke ISV'S-aanbiedingen (Independent Software Vendor) van derden.

Server evaluatie is een hulp programma in Azure Migrate dat on-premises servers evalueert voor migratie naar Azure. Dit artikel bevat informatie over hoe beoordelingen worden berekend.

## <a name="whats-in-an-assessment"></a>Waaruit bestaat een evaluatie?

**Eigenschap** | **Details**
--- | ---
**Doellocatie** | De Azure-locatie waarnaar u wilt migreren.<br/> Server evaluatie ondersteunt momenteel deze doel regio's: Australië-oost, Australië-zuidoost, Brazilië-zuid, Canada-centraal, Canada-oost, Centraal-India, centraal VS, China-oost, China-noord, Azië-oost, VS-Oost, Oost-VS2, Duitsland-centraal, Duitsland-noordoost, Japan-Oost, Japan-West, Korea-centraal, Korea-zuid, Noord VS-centraal, Europa-noord, Zuid-Centraal VS, Zuidoost-Azië, India-zuid, UK-zuid, UK-west, US Gov-Arizona, US Gov-Texas, US Gov-Virginia, VS-West-Centraal, Europa-west, West-India, westelijke VS en West-VS2.
**Opslagtype** | U kunt deze eigenschap gebruiken om het type schijven op te geven dat u wilt verplaatsen in Azure. <br/><br/> Voor de inrichtings grootte kunt u het type doel opslag opgeven als een Premium-beheerde schijf, Standard-SSD-beheerde schijven of Standard-HDD-beheerde schijven. Voor een schaal op basis van prestaties kunt u het type doel schijf opgeven als automatische, Premium beheerde schijven, Standard-HDD-beheerde schijven of Standard-SSD-beheerde schijven.<br/><br/> Wanneer u het opslag type opgeeft als automatisch, wordt de aanbevolen schijf uitgevoerd op basis van de prestatie gegevens van de schijven (IOPS en door Voer). Als u het opslag type opgeeft als Premium/standaard, wordt een schijf-SKU aanbevolen binnen het geselecteerde opslag type. Als u een VM-SLA met één exemplaar van 99,9% wilt uitvoeren, kunt u het opslag type opgeven als Premium-beheerde schijven. Dit zorgt ervoor dat alle schijven in de evaluatie worden aanbevolen als Premium-beheerde schijven. Houd er rekening mee dat Azure Migrate voor migratiebeoordeling alleen ondersteuning voor beheerde schijven biedt.
**Gereserveerde instanties (RI)** | Met deze eigenschap kunt u opgeven of u [gereserveerde instanties](https://azure.microsoft.com/pricing/reserved-vm-instances/) in azure hebt, kosten ramingen in de beoordeling worden uitgevoerd in RI-kortingen. Gereserveerde instanties worden momenteel alleen ondersteund voor de aanbieding betalen naar gebruik in Azure Migrate.
**Criterium voor het aanpassen van de grootte** | Het criterium dat moet worden gebruikt om virtuele machines met een juiste grootte te gebruiken voor Azure. U kunt de grootte van de virtuele machines aanpassen op *basis van de prestaties* , zonder rekening te *houden*met de prestatie geschiedenis.
**Prestatiegeschiedenis** | De duur die u moet overwegen om de prestatie gegevens van machines te evalueren. Deze eigenschap is alleen van toepassing als het formaat van het criterium *op basis van prestaties*is.
**Percentiel gebruik** | De percentielwaarde van de prestatievoorbeeldset waarmee rekening moet worden gehouden voor een juiste groottebepaling. Deze eigenschap is alleen van toepassing wanneer de grootte van het formaat *op basis van prestaties*is.
**VM-reeks** |     U kunt de VM-reeks opgeven die u wilt overwegen voor de juiste grootte. Als u bijvoorbeeld een productie omgeving hebt die u niet van plan bent om te migreren naar A-serie Vm's in azure, kunt u een-serie uitsluiten van de lijst of reeks en wordt de rechter grootte alleen uitgevoerd in de geselecteerde reeks.
**Comfortfactor** | Bij het evalueren van Azure Migrate-server wordt een buffer (comfort factor) in rekening gehouden tijdens de evaluatie. Deze buffer wordt toegepast boven op de gegevens over machinegebruik voor VM's (CPU, geheugen, schijf en netwerk). De comfortfactor houdt rekening met factoren zoals seizoensgebonden gebruik, een korte prestatiegeschiedenis en een mogelijke gebruikstoename in de toekomst.<br/><br/> Een VM met 10 kernen en een gebruik van 20% komt bijvoorbeeld gewoonlijk overeen met een VM met 2 kernen. Met een comfortfactor van 2,0x is het resultaat echter een VM met 4 kernen.
**Aanbieding** | De [Azure-aanbieding](https://azure.microsoft.com/support/legal/offer-details/) waarop u bent geregistreerd. Azure Migrate maakt dienovereenkomstig een schatting van de kosten.
**Valuta** | Factureringsvaluta.
**Korting (%)** | Een abonnement-specifieke korting die u bovenop de Azure-aanbieding ontvangt.<br/> De standaardinstelling is 0%.
**VM tijd actief** | Als uw Vm's niet 24x7 worden uitgevoerd in azure, kunt u de duur (aantal dagen per maand en het aantal uren per dag) opgeven waarvoor ze worden uitgevoerd en de kosten ramingen dienovereenkomstig worden uitgevoerd.<br/> De standaardwaarde is 31 dagen per maand en 24 uur per dag.
**Azure Hybrid Benefit** | Geef op of u Software Assurance hebt en in aanmerking komt voor [Azure Hybrid Benefit](https://azure.microsoft.com/pricing/hybrid-use-benefit/). Als deze optie is ingesteld op Ja, worden niet-Windows Azure-prijzen voor Windows-VM's gehanteerd. Standaard is Ja.

## <a name="how-are-assessments-calculated"></a>Hoe worden evaluaties berekend?

Een evaluatie van Azure Migrate server beoordeling wordt berekend met behulp van de meta gegevens die zijn verzameld over de on-premises servers. De evaluatie berekening wordt uitgevoerd in drie fasen. voor elke server begint de evaluatie berekening met Azure-geschiktheids analyse, gevolgd door de grootte en ten slotte een maandelijkse schatting van de kosten. Een server wordt alleen verplaatst naar een latere fase als deze het vorige wordt door gegeven. Als een server bijvoorbeeld de Azure-geschiktheids controle uitschakelt, wordt deze gemarkeerd als ongeschikt voor Azure en wordt de grootte en de kosten voor hetzelfde niet in rekening gebracht.

## <a name="azure-suitability-analysis"></a>Azure-geschiktheids analyse

Niet alle machines zijn geschikt om te worden uitgevoerd in Azure. Azure Migrate server-evaluatie evalueert elke on-premises computer voor de migratie naar Azure en categoriseert de geraamde machines in een van de volgende geschiktheids Categorieën:
- **Gereed voor Azure** : de machine kan zonder wijzigingen worden gemigreerd naar Azure. Het wordt in azure opgestart met volledige ondersteuning voor Azure.
- **Voorwaardelijk gereed voor Azure** : de computer kan worden opgestart in azure, maar heeft mogelijk geen volledige ondersteuning voor Azure. Bijvoorbeeld: een machine met een oudere versie van Windows Server-besturings systeem wordt niet ondersteund in Azure. U moet voorzichtig zijn voordat u deze machines naar Azure migreert en de richt lijnen voor herstel in de beoordeling volgen om de gereedheids problemen op te lossen voordat u migreert.
- **Niet gereed voor Azure** : de computer kan niet worden opgestart in Azure. Als een on-premises machine bijvoorbeeld een schijf heeft met een grootte van meer dan 64 TB die eraan zijn gekoppeld, kan deze niet worden gehost op Azure. U moet de richt lijnen voor herstel in de beoordeling volgen om het gereedheids probleem op te lossen voordat u naar Azure migreert. Voor computers die zijn gemarkeerd als niet gereed voor Azure, wordt er geen schatting van de juiste grootte en kosten in rekening gebracht.
- **Gereedheid onbekend** -Azure migrate kan de gereedheid van de machine niet vinden vanwege onvoldoende meta gegevens die zijn verzameld uit de on-premises omgeving.

Azure Migrate server beoordeling controleert de computer eigenschappen en het gast besturingssysteem om de Azure-gereedheid van de on-premises machine te identificeren.

### <a name="machine-properties"></a>Computer eigenschappen

Server beoordeling controleert de volgende eigenschappen van de on-premises virtuele machine om te bepalen of deze kan worden uitgevoerd op Azure.

**Eigenschap** | **Details** | **Status van Azure-gereedheid**
--- | --- | ---
**Opstart type** | Azure ondersteunt Vm's met opstart type als BIOS en niet UEFI. | Voorwaardelijk gereed als het opstart type UEFI is.
**Kernen** | Het aantal kernen op de computers moet gelijk zijn aan of kleiner zijn dan het maximum aantal kern geheugens (128 kernen) dat wordt ondersteund voor een virtuele machine van Azure.<br/><br/> Als er een prestatie geschiedenis beschikbaar is, worden de gebruikte kernen Azure Migrate beschouwd als vergelijking. Als er een comfort factor is opgegeven in de evaluatie-instellingen, wordt het aantal gebruikte kern geheugens vermenigvuldigd met de comfort factor.<br/><br/> Als er geen prestatie geschiedenis is, gebruikt Azure Migrate de toegewezen kernen zonder de comfort factor toe te passen. | Gereed als deze kleiner dan of gelijk aan de limieten zijn.
**Geheugenmetabase** | De grootte van het computer geheugen moet gelijk zijn aan of kleiner zijn dan de maximale hoeveelheid geheugen (3892 GB op&nbsp;Azure M-serie Standard_M128m<sup>2</sup>) die is toegestaan voor een Azure-VM. [Meer informatie](https://docs.microsoft.com/azure/virtual-machines/windows/sizes).<br/><br/> Als er een prestatie geschiedenis beschikbaar is, wordt het gebruikte geheugen in Azure Migrate beschouwd als vergelijking. Als er een comfort factor is opgegeven, wordt het gebruikte geheugen vermenigvuldigd met de comfort factor.<br/><br/> Als er geen geschiedenis is, wordt het toegewezen geheugen gebruikt, zonder dat u de comfort factor hoeft toe te passen.<br/><br/> | Gereed indien binnen de limieten.
**Opslag schijf** | De toegewezen grootte van een schijf moet 32 TB of minder zijn.<br/><br/> Het aantal schijven dat is gekoppeld aan de computer, moet 65 of minder zijn, inclusief de besturingssysteem schijf. | Gereed indien binnen de limieten.
**Netwerken** | Aan een machine moet 32 of minder Nic's zijn gekoppeld. | Gereed indien binnen de limieten.

### <a name="guest-operating-system"></a>Gast besturingssysteem
Naast de VM-eigenschappen controleert Azure Migrate server-evaluatie op het gast besturingssysteem van de computers om te bepalen of het op Azure kan worden uitgevoerd.

> [!NOTE]
> Voor virtuele VMware-machines gebruikt Azure Migrate server assessment het besturings systeem dat is opgegeven voor de virtuele machine in vCenter Server om de gast besturingssysteem analyse uit te voeren. Voor Linux-Vm's die worden uitgevoerd op VMware, wordt momenteel niet de exacte kernel-versie van het gast besturingssysteem geïdentificeerd.

De volgende logica wordt gebruikt door Azure Migrate server-evaluatie om Azure-gereedheid op basis van het besturings systeem te identificeren.

**Besturingssysteem** | **Details** | **Status van Azure-gereedheid**
--- | --- | ---
Windows Server 2016 & alle SPs | Azure biedt volledige ondersteuning. | Gereed voor Azure
Windows Server 2012 R2 & alle SPs | Azure biedt volledige ondersteuning. | Gereed voor Azure
Windows Server 2012 & alle SPs | Azure biedt volledige ondersteuning. | Gereed voor Azure
Windows Server 2008 R2 met alle SPs | Azure biedt volledige ondersteuning.| Gereed voor Azure
Windows Server 2008 (32-bits en 64 bits) | Azure biedt volledige ondersteuning. | Gereed voor Azure
Windows Server 2003, 2003 R2 | Deze besturings systemen hebben hun einde van de ondersteunings datum door lopen en hebben een [aangepaste ondersteunings overeenkomst (CSA)](https://aka.ms/WSosstatement) nodig voor ondersteuning in Azure. | Voor bereid voor Azure kunt u het besturings systeem upgraden voordat u naar Azure migreert.
Windows 2000, 98, 95, NT, 3.1, MS-DOS | Deze besturings systemen hebben hun einde van de ondersteunings datum door gegeven, de computer kan worden opgestart in azure, maar er is geen ondersteuning voor het besturings systeem van Azure. | Voor bereid voor Azure is het raadzaam om het besturings systeem bij te werken voordat u naar Azure migreert.
Windows-client 7, 8 en 10 | Azure biedt alleen ondersteuning voor een [abonnement op Visual Studio.](https://docs.microsoft.com/azure/virtual-machines/windows/client-images) | Voorwaardelijk gereed voor Azure
Windows 10 Pro Desktop | Azure biedt ondersteuning voor [multi tenant-hosting rechten.](https://docs.microsoft.com/azure/virtual-machines/windows/windows-desktop-multitenant-hosting-deployment) | Voorwaardelijk gereed voor Azure
Windows Vista, XP Professional | Deze besturings systemen hebben hun einde van de ondersteunings datum door gegeven, de computer kan worden opgestart in azure, maar er is geen ondersteuning voor het besturings systeem van Azure. | Voor bereid voor Azure is het raadzaam om het besturings systeem bij te werken voordat u naar Azure migreert.
Linux | Azure bevestigt deze [Linux-besturings systemen](../virtual-machines/linux/endorsed-distros.md). Andere Linux-besturings systemen kunnen worden opgestart in azure, maar het wordt aanbevolen het besturings systeem bij te werken naar een officiële versie voordat u naar Azure migreert. | Gereed voor Azure als de versie wordt goedgekeurd.<br/><br/>Voorwaardelijk gereed als de versie niet wordt goedgekeurd.
Andere besturings systemen<br/><br/> Bijvoorbeeld Oracle Solaris, Apple Mac OS etc., FreeBSD, enzovoort. | Deze besturings systemen worden niet door Azure geviseerd. De computer kan worden opgestart in azure, maar er is geen ondersteuning voor het besturings systeem van Azure. | Voor bereid voor Azure is het raadzaam een ondersteund besturings systeem te installeren voordat u naar Azure migreert.  
Besturings systeem opgegeven als een **andere** in vCenter Server | Azure Migrate kan het besturings systeem in dit geval niet identificeren. | Onbekende gereedheid. Zorg ervoor dat het besturings systeem dat wordt uitgevoerd in de virtuele machine wordt ondersteund in Azure.
32-bits besturings systemen | De computer kan worden opgestart in azure, maar Azure biedt mogelijk geen volledige ondersteuning. | Voor bereid voor Azure kunt u overwegen om het besturings systeem van de machine te upgraden van 32-bits besturings systeem naar 64-bits besturings systeem voordat u naar Azure migreert.

## <a name="sizing"></a>Grootte aanpassen

Nadat een computer is gemarkeerd als gereed voor Azure, wordt in de server bepaling de grootte van de Azure-VM en de schijf-SKU voor de on-premises VM aangeduid. De aanbevelingen voor de grootte variëren, afhankelijk van de opgegeven evaluatie-eigenschappen.

- Als de evaluatie gebruikmaakt van *grootte op basis van prestaties*, beschouwt Azure migrate de prestatie geschiedenis van de machine om de VM-grootte en het schijf type in azure te identificeren. Deze methode is vooral nuttig als u de on-premises VM hebt toegewezen, maar het gebruik is beperkt en u de grootte van de virtuele machine in azure op de juiste manier wilt besparen. Deze methode helpt u bij het optimaliseren van de grootten tijdens de migratie.
- Als u de prestatie gegevens voor de VM-grootte niet wilt overwegen en de on-premises machines wilt uitvoeren als-is naar Azure, kunt u het criterium voor de grootte opgeven zoals *on-premises* en de server evaluatie vervolgens het formaat van de virtuele machines op basis van de on-premises configuratie zonder rekening te houden met de gebruiks gegevens. De schijf grootte wordt in dit geval uitgevoerd op basis van het opslag type dat u opgeeft in de eigenschappen van de beoordeling (Standard-HDD schijven, Standard-SSD schijven of Premium-schijven).

### <a name="performance-based-sizing"></a>Grootte op basis van prestaties

Voor een hoge grootte op basis van prestaties begint Azure Migrate server analyse met de schijven die zijn gekoppeld aan de virtuele machine, gevolgd door netwerk adapters, en wordt er vervolgens een Azure VM-SKU toegewezen op basis van de reken vereisten van de on-premises VM. De Azure Migrate apparaat profielen de on-premises omgeving voor het verzamelen van prestatie gegevens voor CPU, geheugen, schijven en netwerk adapters.

**Verzameling van prestatie gegevens**

- Voor virtuele VMware-machines verzamelt het Azure Migrate-apparaat tijdens elke 20 seconden een realtime-steekproef punt voor Hyper-V-Vm's, waarna het realtime-voorbeeld punt wordt verzameld bij elk interval van dertig seconden.
- Het apparaat rolt vervolgens de voor elke tien minuten verzamelde sample punten samen en stuurt de maximum waarde voor de laatste tien minuten naar Azure Migrate server beoordeling.
- Azure Migrate server beoordeling slaat alle tien-minuten voorbeeld punten voor de afgelopen maand op en afhankelijk van de evaluatie-eigenschappen die zijn opgegeven voor de *prestatie geschiedenis* en het *percentiel gebruik*, wordt het juiste gegevens punt geïdentificeerd dat moet worden gebruikt voor de juiste grootte. Als de prestatie geschiedenis bijvoorbeeld is ingesteld op één dag en het percentiel gebruik is een 95e percentiel, worden de voorbeeld punten van 10 minuten voor de laatste dag gebruikt, worden deze in oplopende volg orde gesorteerd en wordt de waarde van het 95e percentiel voor de rechter grootte gekozen.
- De bovenstaande waarde wordt vervolgens vermenigvuldigd met de comfort factor voor het verkrijgen van de effectief prestatie gebruiks gegevens voor elke metriek (CPU-gebruik, geheugen gebruik, schijf-IOPS (lezen en schrijven), schijf doorvoer (lezen en schrijven), netwerk doorvoer (in en uit)) die het apparaat wordt verzameld.
- Zodra de waarde voor effectief gebruik is vastgesteld, worden de berekening, opslag en netwerk grootte als volgt uitgevoerd:

**Opslag grootte**: Azure Migrate probeert elke schijf die aan de computer is gekoppeld, toe te wijzen aan een schijf in Azure.

> [!NOTE]
> Azure Migrate: Server Assessment ondersteunt alleen beheerde schijven voor evaluatie.

  - De IOPS Lees-en schrijf bewerkingen van een schijf worden toegevoegd om het totale aantal IOPS dat vereist is, evenals lees-en schrijf doorvoer waarden toe te voegen om de totale door Voer van elke schijf te verkrijgen
  - Als u opslag type hebt opgegeven als automatisch, op basis van de werkelijke IOPS-en doorvoer waarden, identificeert Azure Migrate server-evaluatie vervolgens of de schijf moet worden toegewezen aan een standaard HDD, standaard SSD of een Premium-schijf in Azure. Als het opslag type is ingesteld op Standard-HDD/SSD/Premium, wordt geprobeerd een schijf-SKU te vinden in het geselecteerde opslag type (Standard-HDD/SSD/Premium-schijven).
  - Als de server bepaling geen schijf kan vinden met de vereiste IOPS & door Voer, wordt de computer gemarkeerd als niet geschikt voor Azure.
  - Als er een set geschikte schijven wordt gevonden, worden deze geselecteerd die ondersteuning bieden voor de locatie die is opgegeven in de evaluatie-instellingen.
  - Als er meerdere in aanmerking komende schijven zijn, wordt er een met de laagste kosten geselecteerd.
  - Als prestatie gegevens voor schijven die niet beschikbaar zijn, de configuratie gegevens van de schijf (schijf grootte) worden gebruikt om een standaard SSD-schijf in azure te vinden.

**Netwerk grootte**: Azure Migrate server beoordeling probeert een Azure VM te vinden die het aantal netwerk adapters kan ondersteunen dat is gekoppeld aan de on-premises machine en de prestaties die zijn vereist voor deze netwerk adapters.
    - Om de effectief netwerk prestaties van de on-premises virtuele machine te verkrijgen, worden de gegevens die per seconde (MBps) van de computer (netwerk uitgaan) door de server bepaling geaggregeerd op alle netwerk adapters en wordt de comfort factor toegepast. Dit nummer wordt gebruikt om een virtuele machine van Azure te zoeken die de vereiste netwerk prestaties kan ondersteunen.
    - Samen met de netwerk prestaties is het ook van toepassing als de virtuele machine van Azure het vereiste aantal netwerk adapters kan ondersteunen.
    - Als er geen netwerk prestatie gegevens beschikbaar zijn, wordt alleen het aantal netwerk adapters in rekening gebracht voor VM-grootte.

**Berekenings grootte**: Nadat de opslag-en netwerk vereisten zijn berekend, neemt Azure Migrate server evaluatie rekening met de vereisten voor de CPU en het geheugen om een geschikte VM-grootte in azure te vinden.
    - Azure Migrate zoekt naar de effectief gebruikte kernen en het geheugen om een geschikte VM-grootte in azure te vinden.
    - Als er geen geschikte grootte wordt gevonden, wordt de computer gemarkeerd als niet geschikt voor Azure.
    - Als er een geschikte grootte wordt gevonden, past Azure Migrate de opslag-en netwerk berekeningen toe. Vervolgens worden de instellingen voor de locatie en de prijs categorie toegepast voor de laatste aanbeveling van de VM-grootte.
    - Als er meer Azure VM-grootten in aanmerking komen, wordt de grootte met de laagste kosten aanbevolen.

### <a name="as-on-premises-sizing"></a>Als een on-premises grootte

Als u gebruikmaakt *van de on-premises grootte*, wordt de prestatie geschiedenis van de vm's en schijven niet door de server bepaling beschouwd en wordt er een VM-SKU in azure toegewezen op basis van de grootte die on-premises is toegekend. Op dezelfde manier wordt het opslag type dat is opgegeven in de eigenschappen van de beoordeling (Standard-HDD/SSD/Premium) op de schijf grootte gecontroleerd en wordt het type van de schijf dienovereenkomstig aanbevolen. Het standaard opslag type is Premium-schijven.

## <a name="confidence-ratings"></a>Betrouwbaarheids classificaties
Elke evaluatie op basis van prestaties in Azure Migrate is gekoppeld aan een betrouwbaarheids classificatie die van één (laagste) tot vijf begin (hoogste) ligt.
- De betrouwbaarheidsclassificatie wordt aan een evaluatie toegewezen op basis van de beschikbaarheid van de gegevenspunten die nodig zijn om de evaluatie te berekenen.
- De betrouwbaarheidsclassificatie van een evaluatie helpt u om de betrouwbaarheid in te schatten van de aanbevelingen voor de grootte die Azure Migrate geeft.
- De betrouwbaarheids classificatie is niet van toepassing op de on-premises evaluaties.
- Azure Migrate server beoordeling vereist voor de grootte van de prestaties:
    - De gebruiks gegevens voor CPU en het geheugen van de virtuele machine.
    - Voor elke schijf die aan de virtuele machine is gekoppeld, zijn de IOPS-gegevens van de schijf en de doorvoergegevens vereist.
    - Op dezelfde manier voor elke netwerk adapter die is gekoppeld aan een virtuele machine, heeft de vertrouwens classificatie de netwerk-I/O nodig om de grootte op basis van prestaties te bepalen.
    - Als een van de bovenstaande gebruiks nummers niet beschikbaar is in vCenter Server, is de aanbeveling voor de grootte mogelijk niet betrouwbaar.

De betrouwbaarheidsclassificatie van de evaluatie wordt toegekend op basis van het percentage beschikbare gegevenspunten, zoals hieronder wordt weergegeven:

   **Beschikbaarheid van gegevenspunten** | **Betrouwbaarheidsclassificatie**
   --- | ---
   0%-20% | 1 ster
   21%-40% | 2 sterren
   41%-60% | 3 sterren
   61%-80% | 4 sterren
   81%-100% | 5 sterren

### <a name="low-confidence-ratings"></a>Classificaties met lage betrouw baarheid

Enkele van de redenen waarom een evaluatie een lage betrouwbaarheids classificatie kan krijgen:

- U hebt uw omgeving niet in het profiel voor de duur waarvoor u de evaluatie maakt. Als u bijvoorbeeld de beoordeling met de prestatie duur hebt ingesteld op één dag, moet u wachten op ten minste een dag nadat u de detectie hebt gestart voor alle gegevens punten die u wilt verzamelen.
- Sommige Vm's zijn afgesloten tijdens de periode waarvoor de evaluatie wordt berekend. Als een virtuele machine al enige tijd is uitgeschakeld, kan Azure Migrate server-evaluatie de prestatie gegevens voor die periode niet verzamelen.
- Sommige Vm's zijn gemaakt tussen de periode waarvoor de evaluatie wordt berekend. Als u bijvoorbeeld een evaluatie maakt voor de prestatie geschiedenis van de laatste maand, maar sommige Vm's slechts een week geleden zijn gemaakt in de omgeving, is de prestatie geschiedenis van de nieuwe Vm's niet voor de hele duur.

> [!NOTE]
> Als de betrouwbaarheids classificatie van een evaluatie onder vijf sterren is, raden we u aan om ten minste een dag te wachten op het apparaat om de omgeving in te stellen en de evaluatie vervolgens opnieuw te berekenen. Als dat niet het geval is, is de grootte van de prestaties mogelijk niet betrouwbaar en raden wij u aan om de evaluatie te wijzigen om te gebruiken als een on-premises grootte.

## <a name="monthly-cost-estimation"></a>Schatting maandelijkse kosten

Nadat de aanbevelingen voor de grootte zijn voltooid, berekent Azure Migrate reken-en opslag kosten voor na de migratie.

- **Reken kosten**: Azure Migrate maakt gebruik van de aanbevolen Azure VM-grootte, de facturerings-API voor het berekenen van de maandelijkse kosten voor de virtuele machine.
    - Met de berekening worden het besturings systeem, Software Assurance, gereserveerde instanties, VM-uptime, locatie en valuta-instellingen in rekening gebracht.
    - Hiermee worden de kosten op alle machines geaggregeerd, om de totale maandelijkse reken kosten te berekenen.
- **Opslag kosten**: De maandelijkse opslag kosten voor een machine worden berekend door het samen voegen van de maandelijkse kosten van alle schijven die aan de machine zijn gekoppeld
    - Azure Migrate server beoordeling berekent de totale maandelijkse opslag kosten door het samen voegen van de opslag kosten van alle machines.
    - Momenteel neemt de berekening geen aanbiedingen op die zijn opgegeven in de beoordelings instellingen in rekening.

De kosten worden weer gegeven in de valuta die is opgegeven in de instellingen voor evaluatie.


## <a name="next-steps"></a>Volgende stappen

Een evaluatie maken voor virtuele [VMware-machines](tutorial-assess-vmware.md) of [virtuele Hyper-V-machines](tutorial-assess-hyper-v.md).
