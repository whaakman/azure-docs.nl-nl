---
title: Evaluatie van berekeningen in Azure Migrate | Microsoft Docs
description: Biedt een overzicht van evaluatie van berekeningen in de Azure Migrate-service.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 02/19/2019
ms.author: raynew
ms.openlocfilehash: a328549307772cbdf470160cc1ad713fe1ee5e05
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67805989"
---
# <a name="assessment-calculations"></a>Beoordelingsberekeningen

Azure Migrate-Server-evaluatie beoordeelt on-premises werkbelastingen voor migratie naar Azure. In dit artikel bevat informatie over hoe evaluaties worden berekend.


[Azure Migrate](migrate-services-overview.md) biedt een centraal punt voor het volgen van detectie, beoordeling en migratie van uw on-premises toepassingen, workloads en persoonlijke/openbare cloud-instanties naar Azure. De hub biedt hulpprogramma's voor Azure Migrate voor evaluatie en migratie, evenals van derden independent software vendor (ISV)-aanbiedingen.

## <a name="overview"></a>Overzicht

Een Azure Migrate-evaluatie Server-evaluatie bestaat uit drie fasen. Evaluatie begint met een analyse van geschiktheid, gevolgd door de grootte instelt, en ten slotte een maandelijks raming van kosten. Een virtuele machine alleen worden verplaatst langs op een later stadium als de vorige bewerking is geslaagd. Bijvoorbeeld, als een virtuele machine de controle met Azure-geschiktheid mislukt, wordt deze gemarkeerd als niet geschikt voor Azure, en de dimensionering en de kosten wordt niet worden uitgevoerd.


## <a name="whats-in-an-assessment"></a>Waaruit bestaat een evaluatie?

**Eigenschap** | **Details**
--- | ---
**Doellocatie** | De Azure-locatie waarnaar u wilt migreren.<br/><br/> Azure Migrate ondersteunt momenteel deze doelregio's: Australië-Oost, Australië-Zuidoost, Brazilië-Zuid, Canada-centraal, Canada-Oost, centraal-India, VS-midden, China-Oost, China-Noord, Oost-Azië, VS-Oost, VS-Oost 2, Duitsland-centraal, Duitsland-Noordoost, Japan-Oost, Japan-West, Korea centraal, Korea Zuid, Noord VS-midden, Noord-Europa, Zuid-centraal VS, Zuidoost-Azië, Zuid-India, UK-Zuid, UK-West, VS (overheid)-Arizona, VS (overheid)-Texas, VS (overheid) Virginia, West-Centraal VS, West-Europa, West-India, VS-West, en West vs2.<br/> De doellocatie is standaard ingesteld op US - west 2.
**Opslagtype** | Standard HHD schijven/Standard-SSD-schijven/Premium.<br/><br/> Wanneer u het opslagtype als automatisch in een evaluatie opgeeft, wordt de schijf-aanbeveling is gebaseerd op de prestatiegegevens van de schijven (IOPS en doorvoer).<br/><br/> Als u het opslagtype als Premium of standaard opgeeft, wordt de evaluatie van de gebruiker wordt aanbevolen een schijf SKU binnen het opslagtype geselecteerd.<br/><br/> Als u wilt bereiken van één exemplaar VM SLA van 99,9%, kunt u het opslagtype instellen als Premium-beheerde schijven. Vervolgens worden alle schijven in de evaluatie worden aanbevolen als Premium-beheerde schijven. <br/> Azure Migrate biedt voor migratiebeoordeling alleen ondersteuning voor beheerde schijven.<br/> 
**Gereserveerde instanties (RI)** | Deze eigenschap opgeven als u instanties in Azure gereserveerde. In de evaluatie-kosten schattingen worden gereserveerde instanties kortingen rekening gehouden. Gereserveerde instanties zijn momenteel alleen ondersteund voor betalen per gebruik in Azure Migrate biedt.
**Criterium voor het aanpassen van de grootte** | Gebruikt om virtuele machines de juiste grootte. Grootte kan zijn op basis van prestaties of as-on-premises, zonder rekening te houden de prestatiegeschiedenis.
**Prestatiegeschiedenis** | De duur voor het evalueren van prestaties van de virtuele machine. Deze eigenschap is alleen van toepassing wanneer de grootte op basis van prestaties.
**Percentiel gebruik** | De percentielwaarde van het voorbeeld van de prestaties die wordt gebruikt voor het juiste formaat van virtuele machines. Deze eigenschap is alleen van toepassing wanneer de grootte op basis van prestaties.
**VM-reeks** | De VM-reeks die voor schattingen van grootte wordt gebruikt. Als u bijvoorbeeld een productieomgeving hebt die u niet gaat migreren naar de A-serie van virtuele machines in Azure, kunt u de A-serie uitsluiten van de lijst of reeks. Grootte is alleen gebaseerd op de geselecteerde reeks.
**Comfortfactor** | Azure Migrate-Server-evaluatie, rekening gehouden met een buffer (comfortfactor) tijdens de evaluatie. Deze buffer wordt toegepast boven op de gegevens over machinegebruik voor VM's (CPU, geheugen, schijf en netwerk). De comfortfactor houdt rekening met factoren zoals seizoensgebonden gebruik, een korte prestatiegeschiedenis en een mogelijke gebruikstoename in de toekomst.<br/><br/> Een VM met 10 kernen en een gebruik van 20% komt bijvoorbeeld gewoonlijk overeen met een VM met 2 kernen. Met een comfortfactor van 2,0x is het resultaat echter een VM met 4 kernen.
**Aanbieding** | De [Azure-aanbieding](https://azure.microsoft.com/support/legal/offer-details/) waarop u bent geregistreerd. Azure Migrate maakt dienovereenkomstig een schatting van de kosten.
**Valuta** | Factureringsvaluta. 
**Korting (%)** | Een abonnement-specifieke korting die u bovenop de Azure-aanbieding ontvangt.<br/> De standaardinstelling is 0%.
**VM tijd actief** | Als uw virtuele machines niet meer 24 x 7 worden uitgevoerd op Azure, kunt u de duur (aantal dagen per maand) en het aantal uur per dag voor dat deze zou worden uitgevoerd en de kostenramingen dienovereenkomstig wordt uitgevoerd.<br/> De standaardwaarde is 31 dagen per maand en 24 uur per dag.
**Azure Hybrid Benefit** | Geeft aan of u over software Assurance beschikt en komen in aanmerking voor [Azure Hybrid Benefit](https://azure.microsoft.com/pricing/hybrid-use-benefit/). Als deze optie is ingesteld op Ja, worden niet-Windows Azure-prijzen voor Windows-VM's gehanteerd. 



## <a name="azure-suitability-analysis"></a>Azure-geschiktheid analyse

Niet alle machines zijn geschikt voor het uitvoeren in Azure. Azure Migrate-Server-evaluatie beoordeelt elke on-premises computer voor migratie en machines in de volgende geschiktheid categorieën ingedeeld:
- **Gereed voor Azure** -de machine kan worden gemigreerd als-is naar Azure zonder deze te wijzigen. Deze wordt opgestart in Azure met volledige Azure-ondersteuning.
- **Voorwaardelijk gereed voor Azure** -de machine mogelijk op te starten in Azure, maar hebben mogelijk niet de volledige Azure-ondersteuning. Bijvoorbeeld, wordt een machine met een oudere versie van Windows Server-besturingssysteem niet ondersteund in Azure. U moet zorgvuldig worden voordat u deze machines naar Azure migreert en volg de richtlijnen van het herstel voorgesteld in de evaluatie van de van de Gereedheidsproblemen oplossen voordat u migreert.
- **Niet gereed voor Azure** -de machine niet worden opgestart in Azure. Worden bijvoorbeeld, als een on-premises machine een schijf van de grootte meer heeft dan 4 TB is gekoppeld, deze kan niet gehost op Azure. U moet de richtlijnen voor herstel voorgesteld in de evaluatie van gereedheid oplossen voordat u migreert naar Azure te volgen. Schatting van de juiste grootte en kosten geldt niet voor machines die zijn gemarkeerd als niet gereed voor Azure.
- **Gereedheid onbekend** -Azure Migrate, kan de gereedheid van de machine vanwege onvoldoende gegevens beschikbaar in de vCenter-Server niet vinden.



### <a name="machine-properties"></a>Eigenschappen van de machine

Azure Migrate beoordeelt de volgende eigenschappen van de on-premises virtuele machine om te bepalen of het kan worden uitgevoerd in Azure.

**Eigenschap** | **Details** | **Status van de Azure-gereedheid**
--- | --- | ---
**Opstarttype** | Azure biedt ondersteuning voor virtuele machines met opstarttype als BIOS en geen UEFI. | Voorwaardelijk Gereed als het opstarttype UEFI is.
**Kernen** | Het aantal kernen in de virtuele machines moet gelijk zijn aan of kleiner is dan het maximum aantal kernen (128 kernen) ondersteund voor een Azure-VM.<br/><br/> Als de geschiedenis van geheugenprestaties beschikbaar is, Azure Migrate rekening gehouden met de gebruikte kernen voor een vergelijking. Als een comfortfactor is opgegeven in de instellingen voor evaluatie, wordt het aantal gebruikte kernen vermenigvuldigd met de comfortfactor.<br/><br/> Als er geen prestatiegeschiedenis, Azure Migrate de toegewezen kerngeheugens, zonder toe te passen de comfortfactor gebruikt. | Gereed als minder dan of gelijk zijn aan limieten.
**Geheugen** | De grootte van de machine-geheugen moet gelijk zijn aan of kleiner is dan de maximale hoeveelheid geheugen (3892 GB op Azure-M-serie Standard_M128m&nbsp;<sup>2</sup>) toegestaan voor een Azure-VM. [Meer informatie](https://docs.microsoft.com/azure/virtual-machines/windows/sizes).<br/><br/> Als de geschiedenis van geheugenprestaties beschikbaar is, Azure Migrate rekening gehouden met het gebruikte geheugen voor de vergelijking. Als een comfortfactor is opgegeven, wordt het gebruikte geheugen vermenigvuldigd met de comfortfactor.<br/><br/> Als er geen geschiedenis van die de toegewezen geheugen wordt gebruikt, zonder toe te passen de comfortfactor.<br/><br/> | Gereed als binnen de grenzen.
**Schijf voor opslag** | Toegewezen grootte van een schijf moet 4 TB (4096 GB) of minder.<br/><br/> Het aantal schijven die zijn gekoppeld aan de machine moet 65 of minder, met inbegrip van de besturingssysteemschijf. | Gereed als binnen de grenzen.
**Netwerken** | Een virtuele machine moet 32 of minder NIC's die zijn gekoppeld aan deze. | Gereed als binnen de grenzen.

### <a name="guest-operating-system"></a>Gast-besturingssysteem
Azure Migrate-Server-evaluatie behandeld, samen met de VM-eigenschappen, het gastbesturingssysteem van de computers, om te bepalen of het kan worden uitgevoerd in Azure.

> [!NOTE]
> Azure Migrate-Server-evaluatie maakt gebruik van het besturingssysteem die is opgegeven voor de virtuele machine in VMware vCenter Server voor analyse. Azure Migrate-Server-evaluatie is apparaat gebaseerde voor VM-detectie en deze niet kan verifiëren of het besturingssysteem wordt uitgevoerd op de virtuele machine hetzelfde als de versie die is opgegeven in de vCenter-Server.

De volgende logica wordt gebruikt door Azure Migrate-Server-evaluatie, voor het identificeren van de Azure-gereedheid op basis van het besturingssysteem.

**Besturingssysteem** | **Details** | **Status van de Azure-gereedheid**
--- | --- | ---
Windows Server 2016 en alle SP 's | Azure biedt volledige ondersteuning. | Gereed voor Azure
Windows Server 2012 R2 en alle SP 's | Azure biedt volledige ondersteuning. | Gereed voor Azure
Windows Server 2012 & alle SP 's | Azure biedt volledige ondersteuning. | Gereed voor Azure
Windows Server 2008 R2 met alle SP 's | Azure biedt volledige ondersteuning.| Gereed voor Azure
WindowsServer 2008 (32-bits en 64-bits) | Azure biedt volledige ondersteuning. | Gereed voor Azure
Windows Server 2003, 2003 R2 | Deze besturingssystemen zijn geslaagd voor het einde van ondersteuning voor de datum en het moet een [Custom Support Agreement (CSA)](https://aka.ms/WSosstatement) voor ondersteuning in Azure. | Voorwaardelijk gereed voor Azure, kunt u het besturingssysteem te upgraden voordat u migreert naar Azure.
Windows 2000, 98, 95, NT, 3.1, MS-DOS | Deze besturingssystemen zijn geslaagd voor het einde van ondersteuning voor datum, mogelijk op de machine te starten in Azure, maar er is geen ondersteuning voor het besturingssysteem wordt geleverd door Azure. | Voorwaardelijk gereed voor Azure, het is raadzaam het besturingssysteem upgraden voordat u migreert naar Azure.
Client voor Windows 7, 8 en 10 | Azure biedt ondersteuning voor met [Visual Studio-abonnement.](https://docs.microsoft.com/azure/virtual-machines/windows/client-images) | Voorwaardelijk gereed voor Azure
Windows 10 Pro Desktop | Azure biedt ondersteuning voor met [Multitenant Hosting-rechten.](https://docs.microsoft.com/azure/virtual-machines/windows/windows-desktop-multitenant-hosting-deployment) | Voorwaardelijk gereed voor Azure
Windows Vista, XP Professional | Deze besturingssystemen zijn geslaagd voor het einde van ondersteuning voor datum, mogelijk op de machine te starten in Azure, maar er is geen ondersteuning voor het besturingssysteem wordt geleverd door Azure. | Voorwaardelijk gereed voor Azure, het is raadzaam het besturingssysteem upgraden voordat u migreert naar Azure.
Linux | Azure ondersteunt deze [Linux-besturingssystemen](../virtual-machines/linux/endorsed-distros.md). Andere Linux-besturingssystemen mogelijk op te starten in Azure, maar het is raadzaam het besturingssysteem upgraden naar een onderschreven versie voordat u migreert naar Azure. | Gereed voor Azure als de versie is goedgekeurd.<br/><br/>Voorwaardelijk Gereed als de versie niet is goedgekeurd.
Andere besturingssystemen<br/><br/> e.g.,  Oracle Solaris, Apple Mac OS etc., FreeBSD, etc. | Azure biedt geen enkele aanbeveling voor deze besturingssystemen. De computer mogelijk op te starten in Azure, maar er is geen ondersteuning voor het besturingssysteem wordt geleverd door Azure. | Voorwaardelijk gereed voor Azure, het wordt aanbevolen voor het installeren van een ondersteund besturingssysteem voordat u migreert naar Azure.  
Besturingssysteem opgegeven als **andere** in vCenter-Server | Azure Migrate identificeren in dit geval het besturingssysteem niet. | Gereedheid onbekend. Zorg ervoor dat het besturingssysteem die worden uitgevoerd binnen de virtuele machine in Azure wordt ondersteund.
32-bits besturingssystemen | De computer mogelijk op te starten in Azure, maar Azure bieden volledige ondersteuning mogelijk niet. | Voorwaardelijk gereed voor Azure, kunt u een upgrade van het besturingssysteem van de machine van 32-bits besturingssystemen op 64-bits besturingssysteem voordat u migreert naar Azure.

## <a name="sizing"></a>Grootte aanpassen

Nadat een virtuele machine is gemarkeerd als gereed voor Azure, Azure Migrate de grootte van de virtuele machine en de schijven voor Azure.

- Als de evaluatie van de prestatie gebaseerde schaling, Azure Migrate rekening gehouden met de prestatiegeschiedenis van de machine om te identificeren van de VM-grootte en de schijf type in Azure. Deze methode is vooral handig als u de on-premises-VM, te veel hebt toegewezen, maar het gebruik van de laag is en u wilt om op maat de virtuele machine in Azure om kosten te besparen.
- Als u geen gebruik zoals on-premises evaluatie, Azure Migrate-Server-evaluatie wordt het formaat van de virtuele machines op basis van de on-premises-instellingen, zonder rekening te houden gegevens over het gebruik. Schijf grootte, in dit geval is gebaseerd op het opslagtype die u in de evaluatie-eigenschappen (Standard disk of Premium-schijf opgeeft).

### <a name="performance-based-sizing"></a>Prestatie gebaseerde schaling

Azure Migrate begint met de schijven die zijn gekoppeld aan de virtuele machine, gevolgd door de netwerkadapters voor prestatie gebaseerde schaling, en vervolgens maps een Azure-VM op basis van de compute-vereisten van de on-premises VM.

- **Opslag**: Azure Migrate probeert toe te wijzen van elke schijf die is gekoppeld aan de machine naar een schijf in Azure.
    - Als u de effectieve schijf i/o's per seconde (IOPS) en doorvoer (MBps), vermenigvuldigt Azure Migrate de schijf-IOPS en de doorvoer met de comfortfactor. Op basis van de effectieve IOPS en doorvoer waarden, Azure Migrate wordt geïdentificeerd als de schijf moet worden toegewezen aan een standard- of premium-schijf in Azure.
    - Als Azure Migrate, een schijf met de vereiste IOPS en doorvoer vinden kan, wordt de machine als ongeschikt zijn gemarkeerd voor Azure. [Meer informatie](../azure-subscription-service-limits.md#storage-limits) over Azure-limieten per schijf en de VM.
    - Als er een set met geschikte schijven gevonden, Azure Migrate die ondersteuning bieden voor welke opslagmethode voor redundantie en de locatie die is opgegeven in de instellingen voor evaluatie van geselecteerd.
    - Als er meerdere in aanmerking komende schijven, selecteert u de categorie met de laagste kosten.
    - Als u prestatiegegevens voor schijven niet beschikbaar is, worden alle schijven worden toegewezen aan standard-schijven in Azure.

- **Netwerk**: Azure Migrate wordt gezocht naar een Azure-VM die ondersteuning voor het aantal netwerkadapters die zijn gekoppeld aan de on-premises computer en de prestaties die zijn vereist voor deze netwerkadapters bieden.
    - Als u de effectieve netwerkprestaties van de on-premises VM, Azure Migrate combineert de gegevens verzonden per seconde (MBps) buiten de machine (netwerk), op alle netwerkadapters en de comfortfactor is van toepassing. Dit nummer wordt gebruikt om een virtuele machine van Azure die ondersteuning voor de vereiste netwerkprestaties bieden vinden.
    - Samen met de prestaties van het netwerk, er ook rekening gehouden of de virtuele machine in Azure de vereiste ondersteunen het aantal netwerkadapters.
    - Als er geen prestatiegegevens netwerk beschikbaar is, kunnen alleen met het aantal netwerkadapters wordt beschouwd als de grootte van de virtuele machine instelt.

- **COMPUTE**: Nadat de vereisten voor appopslag en netwerken worden berekend, Azure Migrate rekening gehouden met vereisten voor CPU en geheugen aan een geschikte grootte voor de virtuele machine niet vinden in Azure.
    - Azure Migrate gekeken naar de gebruikte kernen en het geheugen en past de comfortfactor om op te halen van de effectieve kernen en geheugen. Op basis van dit getal, probeert het vinden van een geschikte grootte voor de virtuele machine in Azure.
    - Als er geen geschikte grootte wordt gevonden, wordt de machine gemarkeerd als niet geschikt voor Azure.
    - Als een geschikte grootte wordt gevonden, Azure Migrate is van toepassing de berekeningen van opslag en netwerken. Vervolgens wordt toegepast locatie en de instellingen, voor de uiteindelijke aanbeveling voor VM-grootte van de prijscategorie.
    - Als er meer Azure VM-grootten in aanmerking komen, wordt de grootte met de laagste kosten aanbevolen.

### <a name="as-on-premises-sizing"></a>Zoals on-premises formaat wijzigen

Als u als on-premises formaat, wijst de Server-evaluatie een VM-SKU in Azure op basis van de grootte van on-premises. Op dezelfde manier voor grootte van de schijf, het kijkt naar het Storage-type dat is opgegeven in de evaluatie-eigenschappen (Standard/Premium) en raadt het schijftype dienovereenkomstig aan. Het opslagtype is een Premium-schijven.

## <a name="confidence-ratings"></a>Vertrouwen van classificaties
Elke evaluatie op basis van prestaties in Azure Migrate wordt gekoppeld aan een betrouwbaarheidsclassificatie van een (laagste) naar vijf wordt gestart (hoogste).
- De betrouwbaarheidsclassificatie wordt aan een evaluatie toegewezen op basis van de beschikbaarheid van de gegevenspunten die nodig zijn om de evaluatie te berekenen.
- De betrouwbaarheidsclassificatie van een evaluatie helpt u om de betrouwbaarheid in te schatten van de aanbevelingen voor de grootte die Azure Migrate geeft.
- Betrouwbaarheidsclassificatie is niet van toepassing als on-premises evaluaties.
- Op basis van prestaties van de grootte instelt moet de Azure Migrate-Server-evaluatie:
    - De verbruiksgegevens voor CPU en geheugen van de virtuele machine.
    - Voor elke schijf die aan de virtuele machine is gekoppeld, zijn de IOPS-gegevens van de schijf en de doorvoergegevens vereist.
    - Voor elke netwerkadapter die is gekoppeld aan een virtuele machine, moet de betrouwbaarheidsclassificatie op dezelfde manier de netwerk-i / prestaties gebaseerde groottebepaling te doen.
    - Als een van de bovenstaande gebruiksgetallen niet beschikbaar zijn in vCenter Server, aanbeveling voor de grootte mogelijk niet betrouwbaar. 

De betrouwbaarheidsclassificatie van de evaluatie wordt toegekend op basis van het percentage beschikbare gegevenspunten, zoals hieronder wordt weergegeven:

   **Beschikbaarheid van gegevenspunten** | **Betrouwbaarheidsclassificatie**
   --- | ---
   0%-20% | 1 ster
   21%-40% | 2 sterren
   41%-60% | 3 sterren
   61%-80% | 4 sterren
   81%-100% | 5 sterren

### <a name="low-confidence-ratings"></a>Lage vertrouwen beoordelingen

Enkele van de redenen waarom een evaluatie een lage betrouwbaarheidsclassificatie kan ophalen:

- U niet hebt uw omgeving profiel voor de periode waarvoor u de evaluatie wilt maken. Bijvoorbeeld, als u de evaluatie van de duur van de prestaties is ingesteld op 1 dag maakt, moet u na afloop van ten minste een dag na het starten van detectie voor alle gegevenspunten om op te halen die worden verzameld.
- Sommige VM's zijn afgesloten tijdens de periode waarover de evaluatie wordt berekend. Als virtuele machines gedurende een bepaalde tijd zijn uitgeschakeld, kan niet Azure Migrate-Server-evaluatie de prestatiegegevens verzamelen gedurende die periode.
- Enkele VM's zijn gemaakt tijdens de periode waarover de evaluatie wordt berekend. Bijvoorbeeld, als u een evaluatie voor de prestatiegeschiedenis van de laatste maand, maar enkele VM's zijn gemaakt in de omgeving een week geleden, zich de prestatiegeschiedenis van de nieuwe virtuele machines niet er voor de gehele duur.

  > [!NOTE]
  > Als de betrouwbaarheidsclassificatie van een evaluatie lager dan vijf sterren is, raden wij u wacht ten minste een dag voor het apparaat aan het profiel van de omgeving en vervolgens de evaluatie opnieuw te berekenen. Als u dit niet doet, prestatie gebaseerde schaling mogelijk niet betrouwbare, en wij en wordt aanbevolen dat u overschakelt naar de evaluatie te gebruiken als on-premises formaat.
  
## <a name="monthly-cost-estimation"></a>Schatting van de maandelijkse kosten

Nadat het formaat aanbevelingen zijn voltooid, berekent Azure Migrate de berekenings- en opslagkosten voor na de migratie.

- **COMPUTE-kosten**: Met behulp van de aanbevolen grootte van de virtuele machine van Azure, Azure Migrate maakt gebruik van de facturering-API voor het berekenen van de maandelijkse kosten voor de virtuele machine.
    - De berekening wordt het besturingssysteem, software assurance, gereserveerde instanties, VM uptime, locatie en valuta-instellingen rekening.
    - Worden de kosten voor alle machines, voor het berekenen van de totale maandelijkse kosten.
- **Opslagkosten**: De maandelijkse opslagkosten voor voor een virtuele machine wordt berekend door samenvoeging van de maandelijkse kosten van alle schijven die zijn gekoppeld aan de machine
    - Azure Migrate-Server-evaluatie berekent de totale maandelijkse kosten voor opslag door samenvoeging van de kosten voor opslag van alle machines.
    - De berekening nemen niet op dit moment biedt die zijn opgegeven in de instellingen voor evaluatie van in aanmerking.

Kosten worden weergegeven in de valuta die is opgegeven in de instellingen voor evaluatie.


## <a name="next-steps"></a>Volgende stappen

Maak een evaluatie voor [virtuele VMware-machines](tutorial-assess-vmware.md) of [Hyper-V-machines](tutorial-assess-hyper-v.md).
