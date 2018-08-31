---
title: Evaluatie van berekeningen in Azure Migrate | Microsoft Docs
description: Biedt een overzicht van evaluatie van berekeningen in de Azure Migrate-service.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 07/25/2018
ms.author: raynew
ms.openlocfilehash: 092f0844854c13898fd7f07ce9b7ddea98ff01ed
ms.sourcegitcommit: f94f84b870035140722e70cab29562e7990d35a3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/30/2018
ms.locfileid: "43286270"
---
# <a name="assessment-calculations"></a>Beoordelingsberekeningen

[Azure Migrate](migrate-overview.md) beoordeelt on-premises werkbelastingen voor migratie naar Azure. In dit artikel bevat informatie over hoe evaluaties worden berekend.


## <a name="overview"></a>Overzicht

Een Azure Migrate-evaluatie bestaat uit drie fasen. Evaluatie begint met een analyse van geschiktheid, gevolgd door de grootte instelt, en ten slotte een maandelijks raming van kosten. Een virtuele machine alleen worden verplaatst langs op een later stadium als de vorige bewerking is geslaagd. Bijvoorbeeld, als een virtuele machine de controle met Azure-geschiktheid mislukt, wordt deze gemarkeerd als niet geschikt voor Azure, en de dimensionering en de kosten wordt niet worden uitgevoerd.


## <a name="azure-suitability-analysis"></a>Azure-geschiktheid analyse

Niet alle machines zijn geschikt voor het uitvoeren op de cloud als cloud een eigen beperkingen en vereisten heeft. Azure Migrate beoordeelt elke on-premises computer voor geschiktheid voor migratie naar Azure en de machines in de volgende categorieën ingedeeld:
- **Gereed voor Azure** -de machine kan worden gemigreerd als-is naar Azure zonder deze te wijzigen. Deze wordt opgestart in Azure met volledige Azure-ondersteuning.
- **Voorwaardelijk gereed voor Azure** -de machine mogelijk op te starten in Azure, maar hebben mogelijk niet de volledige Azure-ondersteuning. Bijvoorbeeld, wordt een machine met een oudere versie van Windows Server-besturingssysteem niet ondersteund in Azure. U moet zorgvuldig worden voordat u deze machines naar Azure migreert en volg de richtlijnen van het herstel voorgesteld in de evaluatie van de van de Gereedheidsproblemen oplossen voordat u migreert.
- **Niet gereed voor Azure** -de machine niet worden opgestart in Azure. Worden bijvoorbeeld, als een on-premises machine een schijf van de grootte meer heeft dan 4 TB is gekoppeld, deze kan niet gehost op Azure. U moet de richtlijnen voor herstel voorgesteld in de evaluatie van gereedheid oplossen voordat u migreert naar Azure te volgen. Schatting van de juiste grootte en kosten geldt niet voor machines die zijn gemarkeerd als niet gereed voor Azure.
- **Gereedheid onbekend** -Azure Migrate, kan de gereedheid van de machine vanwege onvoldoende gegevens beschikbaar in de vCenter-Server niet vinden.

Azure Migrate beoordeelt de eigenschappen van de machine en de Gast-besturingssysteem voor het identificeren van de Azure-gereedheid van de on-premises machine.

### <a name="machine-properties"></a>Eigenschappen van de machine
Azure Migrate beoordeelt de volgende eigenschappen van de on-premises virtuele machine om te bepalen of een virtuele machine op Azure uitvoeren kunt.

**Eigenschap** | **Details** | **Status van de Azure-gereedheid**
--- | --- | ---
**Opstarttype** | Azure biedt ondersteuning voor virtuele machines met opstarttype als BIOS en geen UEFI. | Voorwaardelijk Gereed als het opstarttype UEFI is.
**Kernen** | Het aantal kernen in de virtuele machines moet gelijk zijn aan of kleiner is dan het maximum aantal kernen (32) ondersteund voor een Azure-VM.<br/><br/> Als de geschiedenis van geheugenprestaties beschikbaar is, Azure Migrate rekening gehouden met de gebruikte kernen voor een vergelijking. Als een comfortfactor is opgegeven in de instellingen voor evaluatie, wordt het aantal gebruikte kernen vermenigvuldigd met de comfortfactor.<br/><br/> Als er geen prestatiegeschiedenis, Azure Migrate de toegewezen kerngeheugens, zonder toe te passen de comfortfactor gebruikt. | Gereed als minder dan of gelijk zijn aan limieten.
**Geheugen** | De grootte van de machine-geheugen moet gelijk zijn aan of kleiner is dan de maximale hoeveelheid geheugen (3892 GB op Azure-M-serie Standard_M128m&nbsp;<sup>2</sup>) toegestaan voor een Azure-VM. [Meer informatie](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/sizes).<br/><br/> Als de geschiedenis van geheugenprestaties beschikbaar is, Azure Migrate rekening gehouden met het gebruikte geheugen voor de vergelijking. Als een comfortfactor is opgegeven, wordt het gebruikte geheugen vermenigvuldigd met de comfortfactor.<br/><br/> Als er geen geschiedenis van die de toegewezen geheugen wordt gebruikt, zonder toe te passen de comfortfactor.<br/><br/> | Gereed als binnen de grenzen.
**Schijf voor opslag** | Toegewezen grootte van een schijf moet 4 TB (4096 GB) of minder.<br/><br/> Het aantal schijven die zijn gekoppeld aan de machine moet 65 of minder, met inbegrip van de besturingssysteemschijf. | Gereed als binnen de grenzen.
**Netwerken** | Een virtuele machine moet 32 of minder NIC's die zijn gekoppeld aan deze. | Gereed als binnen de grenzen.

### <a name="guest-operating-system"></a>Gast-besturingssysteem
Samen met VM-eigenschappen, Azure Migrate ook gekeken naar het gastbesturingssysteem van de on-premises virtuele machine om te identificeren als de virtuele machine op Azure uitvoeren kunt.

> [!NOTE]
> Azure Migrate rekening gehouden met het besturingssysteem die is opgegeven in de vCenter-Server de volgende analyse. Omdat de detectie die wordt uitgevoerd door Azure Migrate op basis van een apparaat, heeft het geen een manier om te controleren of het besturingssysteem die worden uitgevoerd binnen de virtuele machine hetzelfde als het een opgegeven in VMware vCenter Server is.

De volgende logica wordt gebruikt door Azure Migrate voor het identificeren van de Azure-gereedheid van de virtuele machine op basis van het besturingssysteem.

**Besturingssysteem** | **Details** | **Status van de Azure-gereedheid**
--- | --- | ---
Windows Server 2016 en alle SP 's | Azure biedt volledige ondersteuning. | Gereed voor Azure
Windows Server 2012 R2 en alle SP 's | Azure biedt volledige ondersteuning. | Gereed voor Azure
Windows Server 2012 & alle SP 's | Azure biedt volledige ondersteuning. | Gereed voor Azure
Windows Server 2008 R2 met alle SP 's | Azure biedt volledige ondersteuning.| Gereed voor Azure
WindowsServer 2008 (32-bits en 64-bits) | Azure biedt volledige ondersteuning. | Gereed voor Azure
WindowsServer 2003, 2003 R2 | Deze besturingssystemen zijn geslaagd voor het einde van ondersteuning voor de datum en het moet een [Custom Support Agreement (CSA)](https://aka.ms/WSosstatement) voor ondersteuning in Azure. | Voorwaardelijk gereed voor Azure, kunt u het besturingssysteem te upgraden voordat u migreert naar Azure.
Windows 2000, 98, 95, NT, 3.1, MS-DOS | Deze besturingssystemen zijn geslaagd voor het einde van ondersteuning voor datum, mogelijk op de machine te starten in Azure, maar er is geen ondersteuning voor het besturingssysteem wordt geleverd door Azure. | Voorwaardelijk gereed voor Azure, het is raadzaam het besturingssysteem upgraden voordat u migreert naar Azure.
Client voor Windows 7, 8 en 10 | Azure biedt ondersteuning voor Visual Studio-abonnement. | Voorwaardelijk gereed voor Azure
Windows Vista, XP Professional | Deze besturingssystemen zijn geslaagd voor het einde van ondersteuning voor datum, mogelijk op de machine te starten in Azure, maar er is geen ondersteuning voor het besturingssysteem wordt geleverd door Azure. | Voorwaardelijk gereed voor Azure, het is raadzaam het besturingssysteem upgraden voordat u migreert naar Azure.
Linux | Azure ondersteunt deze [Linux-besturingssystemen](../virtual-machines/linux/endorsed-distros.md). Andere Linux-besturingssystemen mogelijk op te starten in Azure, maar het is raadzaam het besturingssysteem upgraden naar een onderschreven versie voordat u migreert naar Azure. | Gereed voor Azure als de versie is goedgekeurd.<br/><br/>Voorwaardelijk Gereed als de versie niet is goedgekeurd.
Andere besturingssystemen<br/><br/> bijv, Oracle Solaris, Apple Mac OS enz., toegang tot FreeBSD, enzovoort. | Azure biedt geen enkele aanbeveling voor deze besturingssystemen. De computer mogelijk op te starten in Azure, maar er is geen ondersteuning voor het besturingssysteem wordt geleverd door Azure. | Voorwaardelijk gereed voor Azure, het wordt aanbevolen voor het installeren van een ondersteund besturingssysteem voordat u migreert naar Azure.  
Besturingssysteem opgegeven als **andere** in vCenter-Server | Azure Migrate identificeren in dit geval het besturingssysteem niet. | Gereedheid onbekend. Zorg ervoor dat het besturingssysteem die worden uitgevoerd binnen de virtuele machine in Azure wordt ondersteund.
32-bits besturingssystemen | De computer mogelijk op te starten in Azure, maar Azure bieden volledige ondersteuning mogelijk niet. | Voorwaardelijk gereed voor Azure, kunt u een upgrade van het besturingssysteem van de machine van 32-bits besturingssystemen op 64-bits besturingssysteem voordat u migreert naar Azure.

## <a name="sizing"></a>Grootte aanpassen

Nadat een virtuele machine is gemarkeerd als gereed voor Azure, Azure Migrate de grootte van de virtuele machine en de schijven voor Azure. Als het criterium voor het opgegeven in de evaluatie-eigenschappen instellen op prestaties gebaseerde groottebepaling te doen, Azure Migrate rekening gehouden met de prestatiegeschiedenis van de machine om te identificeren van de VM-grootte en de schijf type in Azure. Deze methode is handig in scenario's waarbij u de on-premises VM te veel hebt toegewezen, maar het gebruik van de laag is en u wilt de juiste grootte de virtuele machines in Azure om kosten te besparen.

> [!NOTE]
> Azure Migrate verzamelt de prestatiegeschiedenis van on-premises virtuele machines van vCenter-Server. Om ervoor te zorgen nauwkeurige juiste groottebepaling, zorg ervoor dat de instelling voor statistieken in vCenter-Server is ingesteld op niveau 3 en wacht ten minste een dag voor de detectie van de on-premises machines begon. Als de instelling voor statistieken in vCenter Server lager dan niveau 3 is, wordt de prestatiegegevens voor schijven en het netwerk niet verzameld.

Als u niet wilt Houd rekening met de geschiedenis van geheugenprestaties voor VM-grootte en de virtuele machine als wilt-is naar Azure, kunt u het criterium voor het instellen als *zoals on-premises* en Azure Migrate wordt vervolgens het formaat van de virtuele machines op basis van de on-premises configuratie zonder rekening te houden de gegevens over het gebruik. Schijf grootte, in dit geval wordt uitgevoerd op basis van het type opslag dat u in de evaluatie-eigenschappen (Standard disk of Premium-schijf opgeeft)

### <a name="performance-based-sizing"></a>Prestatie gebaseerde schaling

Azure Migrate begint met de schijven die zijn gekoppeld aan de virtuele machine, gevolgd door de netwerkadapters voor prestatie gebaseerde schaling, en vervolgens maps een Azure-VM op basis van de compute-vereisten van de on-premises VM.

- **Opslag**: Azure Migrate probeert toe te wijzen van elke schijf die is gekoppeld aan de machine naar een schijf in Azure.

    > [!NOTE]
    > Azure Migrate ondersteunt alleen beheerde schijven voor de beoordeling.

    - Als u de effectieve schijf i/o's per seconde (IOPS) en doorvoer (MBps), vermenigvuldigt Azure Migrate de schijf-IOPS en de doorvoer met de comfortfactor. Op basis van de effectieve IOPS en doorvoer waarden, Azure Migrate wordt geïdentificeerd als de schijf moet worden toegewezen aan een standard- of premium-schijf in Azure.
    - Als Azure Migrate, een schijf met de vereiste IOPS en doorvoer vinden kan, wordt de machine als ongeschikt zijn gemarkeerd voor Azure. [Meer informatie](../azure-subscription-service-limits.md#storage-limits) over Azure-limieten per schijf en de VM.
    - Als er een set met geschikte schijven gevonden, Azure Migrate die ondersteuning bieden voor welke opslagmethode voor redundantie en de locatie die is opgegeven in de instellingen voor evaluatie van geselecteerd.
    - Als er meerdere in aanmerking komende schijven, selecteert u de categorie met de laagste kosten.
    - Als u prestatiegegevens voor schijven niet beschikbaar is, alle schijven zijn toegewezen aan standard-schijven in Azure.

- **Netwerk**: Azure Migrate gezocht naar een Azure-VM die ondersteuning voor het aantal netwerkadapters die zijn gekoppeld aan de on-premises computer en de prestaties die zijn vereist voor deze netwerkadapters bieden.
    - Als u de effectieve netwerkprestaties van de on-premises VM, Azure Migrate combineert de gegevens verzonden per seconde (MBps) buiten de machine (netwerk), op alle netwerkadapters en de comfortfactor is van toepassing. Dit nummer wordt gebruikt om een virtuele machine van Azure die ondersteuning voor de vereiste netwerkprestaties bieden vinden.
    - Samen met de prestaties van het netwerk, er ook rekening gehouden of de virtuele machine in Azure de vereiste ondersteunen het aantal netwerkadapters.
    - Als er geen prestatiegegevens netwerk beschikbaar is, kunnen alleen met het aantal netwerkadapters wordt beschouwd als de grootte van de virtuele machine instelt.

- **COMPUTE**: nadat de vereisten voor appopslag en netwerken worden berekend, Azure Migrate acht CPU en geheugen vereisten voor het vinden van een geschikte grootte voor de virtuele machine in Azure.
    - Azure Migrate gekeken naar de gebruikte kernen en het geheugen en past de comfortfactor om op te halen van de effectieve kernen en geheugen. Op basis van dit getal, probeert het vinden van een geschikte grootte voor de virtuele machine in Azure.
    - Als er geen geschikte grootte wordt gevonden, wordt de machine gemarkeerd als niet geschikt voor Azure.
    - Als een geschikte grootte wordt gevonden, Azure Migrate is van toepassing de berekeningen van opslag en netwerken. Vervolgens wordt toegepast locatie en de instellingen, voor de uiteindelijke aanbeveling voor VM-grootte van de prijscategorie.
    - Als er meer Azure VM-grootten in aanmerking komen, wordt de grootte met de laagste kosten aanbevolen.

### <a name="as-on-premises-sizing"></a>Zoals on-premises formaat wijzigen
Als het criterium voor groottebepaling *zoals on-premises '*, Azure Migrate houdt geen rekening met de prestatiegeschiedenis van de virtuele machines en schijven en een VM-SKU in Azure op basis van de on-premises toegewezen grootte kan worden toegewezen. Op dezelfde manier voor grootte van de schijf, het kijkt naar het Storage-type dat is opgegeven in de evaluatie-eigenschappen (Standard/Premium) en raadt het schijftype dienovereenkomstig aan. Opslagtype standaard is de Premium-schijven.

### <a name="confidence-rating"></a>Betrouwbaarheidsclassificatie
Elke prestatie-evaluatie in Azure Migrate wordt gekoppeld aan een betrouwbaarheidsclassificatie van 1 ster tot 5 sterren (1 ster is de laagste score en 5 sterren de hoogste). De betrouwbaarheidsclassificatie wordt aan een evaluatie toegewezen op basis van de beschikbaarheid van de gegevenspunten die nodig zijn om de evaluatie te berekenen. De betrouwbaarheidsclassificatie van een evaluatie helpt u om de betrouwbaarheid in te schatten van de aanbevelingen voor de grootte die Azure Migrate geeft. Betrouwbaarheidsclassificatie is niet van toepassing op als on-premises evaluaties.

Als u de grootte instelt op basis van de prestaties, heeft Azure Migrate de gebruiksgegevens nodig van de CPU en het geheugen van de virtuele machine. Voor elke schijf die aan de virtuele machine is gekoppeld, zijn de IOPS-gegevens van de schijf en de doorvoergegevens vereist. Ook heeft Azure Migrate voor iedere netwerkadapter die aan een VM is gekoppeld, gegevens over het inkomende/uitgaande netwerkverkeer nodig om de grootte in te kunnen stellen op basis van de prestaties. Als een of meer van de bovenstaande gebruiksgetallen niet beschikbaar zijn in vCenter Server, is de aanbeveling voor de grootte die Azure Migrate doet, mogelijk niet betrouwbaar. De betrouwbaarheidsclassificatie van de evaluatie wordt toegekend op basis van het percentage beschikbare gegevenspunten, zoals hieronder wordt weergegeven:

   **Beschikbaarheid van gegevenspunten** | **Betrouwbaarheidsclassificatie**
   --- | ---
   0%-20% | 1 ster
   21%-40% | 2 sterren
   41%-60% | 3 sterren
   61%-80% | 4 sterren
   81%-100% | 5 sterren

Het kan voorkomen dat niet alle gegevenspunten beschikbaar zijn voor een evaluatie. Dit kan de volgende oorzaken hebben:
- De instelling voor statistieken in vCenter Server is niet ingesteld op niveau 3. Als de instelling voor statistieken in vCenter Server op een lager niveau dan niveau 3 is ingesteld, worden de prestatiegegevens voor de schijf en het netwerk niet verzameld vanuit vCenter Server. In dat geval wordt de aanbeveling van Azure Migrate voor de schijf en het netwerk niet gebaseerd op gebruik. Als er geen rekening wordt gehouden met de IOPS/doorvoer van de schijf, kan in Azure Migrate niet worden bepaald of voor de schijf een premium-schijf in Azure nodig is. In dat geval adviseert Azure Migrate Standard-schijven voor alle schijven.
- De instelling voor statistieken in vCenter Server is korte tijd op niveau 3 ingesteld geweest voordat de detectie begon. Laten we het scenario als voorbeeld nemen waarin u het niveau van de instelling voor statistieken vandaag bijvoorbeeld op 3 zet en morgen (na 24 uur) met de detectie begint met behulp van het collector-apparaat. Als u een evaluatie voor één dag maakt, hebt u alle gegevenspunten en is de betrouwbaarheidsclassificatie van de evaluatie 5 sterren. Maar als u de duur van de prestaties in de evaluatie-eigenschappen wijzigt in één maand, gaat de betrouwbaarheidsclassificatie omlaag, omdat de prestatiegegevens voor schijven en netwerken voor de laatste maand niet beschikbaar zijn. Als u rekening wilt houden met de prestatiegegevens voor de laatste maand, kunt u het beste de instelling voor statistieken in vCenter Server op niveau 3 laten staan gedurende één maand voordat u met de detectie begint.
- Er zijn enkele VM's uitgeschakeld geweest in de periode waarover de evaluatie wordt berekend. Als er VM's zijn die gedurende een bepaalde tijd uitgeschakeld zijn geweest, zal vCenter Server voor die periode geen prestatiegegevens hebben.
- Er zijn enkele VM's gemaakt tijdens de periode waarover de evaluatie wordt berekend. Als u bijvoorbeeld een evaluatie maakt voor de prestatiegeschiedenis van de laatste maand, maar er een week geleden enkele VM's in de omgeving zijn gemaakt. In dergelijke gevallen is de prestatiegeschiedenis van de nieuwe virtuele machines niet voor de hele periode beschikbaar.

> [!NOTE]
> Als de betrouwbaarheidsclassificatie van een evaluatie lager dan 4 sterren is, raden we u aan om de instelling voor statistieken in vCenter Server op 3 te zetten, te wachten gedurende de periode waarover u de evaluatie wilt uitvoeren (1 dag/1 week/1 maand) en vervolgens een detectie en evaluatie uit te voeren. Als de voorgaande stappen niet kunnen worden uitgevoerd, wordt de grootte mogelijk niet op betrouwbare wijze ingesteld. In dat geval doet u er verstandig aan over te schakelen naar *instelling van de grootte op basis van 'zoals on-premises'* door de evaluatie-eigenschappen te wijzigen.

## <a name="monthly-cost-estimation"></a>Schatting van de maandelijkse kosten

Nadat het formaat aanbevelingen zijn voltooid, berekent Azure Migrate na de migratie berekenings- en opslagkosten.

- **COMPUTE-kosten**: de API van de facturering voor het berekenen van de maandelijkse kosten voor de virtuele machine met behulp van de aanbevolen grootte van de virtuele machine van Azure, Azure Migrate gebruikt. De berekening wordt het besturingssysteem, software assurance, gereserveerde instanties, VM uptime, locatie en valuta-instellingen rekening. Worden de kosten voor alle machines, voor het berekenen van de totale maandelijkse kosten.
- **Opslagkosten**: de maandelijkse opslagkosten voor een virtuele machine wordt berekend door samenvoeging van de maandelijkse kosten van alle schijven die zijn gekoppeld aan de machine. Azure Migrate berekent de totale maandelijkse kosten voor opslag door samenvoeging van de kosten voor opslag van alle machines. De berekening nemen niet op dit moment biedt die zijn opgegeven in de instellingen voor evaluatie van in aanmerking.

Kosten worden weergegeven in de valuta die is opgegeven in de instellingen voor evaluatie.


## <a name="next-steps"></a>Volgende stappen

[Een evaluatie maken voor on-premises VMware-machines](tutorial-assessment-vmware.md)
