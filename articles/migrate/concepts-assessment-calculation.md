---
title: Assessment berekeningen in Azure migreren | Microsoft Docs
description: Biedt een overzicht van assessment berekeningen in de service Azure migreren.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 05/15/2018
ms.author: raynew
ms.openlocfilehash: be4fb15d96f5598d4b1ddbbaa4befe7f6530152c
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
---
# <a name="assessment-calculations"></a>Beoordelingsberekeningen

[Azure migreren](migrate-overview.md) voor lokale werkbelastingen voor migratie naar Azure evalueert. Dit artikel bevat informatie over hoe beoordelingen zijn berekend.


## <a name="overview"></a>Overzicht

Een beoordeling voor het migreren van Azure bestaat uit drie fasen. Beoordeling begint met een geschiktheid-analyse, gevolgd door formaat wijzigen, en ten slotte een maandelijkse kosten schatting. Een machine alleen wordt verplaatst langs naar een later stadium als de vorige bewerking is geslaagd. Bijvoorbeeld, als een machine de controle van de geschiktheid van Azure mislukt, deze gemarkeerd als geschikt voor Azure, en sizing en goedkoper won't worden uitgevoerd.


## <a name="azure-suitability-analysis"></a>Azure geschiktheid analyse

Niet alle machines zijn geschikt voor uitgevoerd in de cloud, zoals cloud een eigen beperkingen en vereisten heeft. Azure migreren beoordeelt elke lokale machine voor de geschiktheid van de migratie naar Azure en de machines ingedeeld in een van de volgende categorieën:
- **Gereed voor Azure** -de machine kan worden gemigreerd als-is naar Azure zonder deze te wijzigen. Deze wordt opgestart in Azure met volledige Azure-ondersteuning.
- **Voorwaardelijk klaar te maken voor Azure** -de computer kan worden opgestart in Azure, maar mogelijk geen volledige Azure-ondersteuning. Bijvoorbeeld, wordt een machine met een oudere versie van Windows Server-besturingssysteem niet ondersteund in Azure. U moet zorgvuldig worden voordat deze machines migreren naar Azure en volg de richtlijnen van het herstel voorgesteld in de beoordeling van de van de Gereedheidsproblemen oplossen voordat u migreert.
- **Niet gereed voor Azure** -de machine niet worden opgestart in Azure. Worden bijvoorbeeld, als een lokale virtuele machine een schijf met een grootte van meer heeft dan 4 TB is gekoppeld, deze kan niet gehost op Azure. U moet de herstel-richtlijnen voorgesteld in de beoordeling van gereedheid oplossen voordat u migreert naar Azure te volgen. Juiste formaat en de kosten schatting geldt niet voor de machines die zijn gemarkeerd als niet gereed voor Azure.
- **Gereedheid van de onbekende** -Azure migreren, kan de gereedheid van de machine vanwege onvoldoende gegevens beschikbaar zijn in de vCenter-Server niet vinden.

Azure migreren controleert de eigenschappen van de machine en het gastbesturingssysteem te identificeren van de gereedheid van de on-premises machine van Azure.

### <a name="machine-properties"></a>Eigenschappen van de machine
De volgende eigenschappen van de lokale virtuele machine om te bepalen of een virtuele machine in Azure uitvoeren kunt een overzicht van Azure migreren.

**Eigenschap** | **Details** | **Gereedheidsstatus van Azure**
--- | --- | ---
**Type opstarten** | Azure biedt ondersteuning voor virtuele machines met opstarten type als de BIOS- en niet UEFI. | Voorwaardelijk klaar voor Azure als opstarten type UEFI.
**Kernen** | Het aantal kernen op de machines moet gelijk zijn aan of kleiner is dan het maximum aantal kernen (32) ondersteund voor een virtuele machine in Azure.<br/><br/> Als prestatiegeschiedenis beschikbaar is, Azure migreren rekening gehouden met de gebruikte kernen voor vergelijking. Als een factor comfort is opgegeven in de instellingen voor de beoordeling, wordt het aantal gebruikte kernen vermenigvuldigd met de factor comfort.<br/><br/> Als er geen prestatiegeschiedenis van, de toegewezen kernen Azure migreren gebruikt zonder het toepassen van de factor comfort. | Niet-gereed als het aantal kernen groter dan 32 is.
**Geheugen** | De grootte van de machine-geheugen moet gelijk zijn aan of kleiner is dan de maximale hoeveelheid geheugen (448 GB) toegestaan voor een virtuele machine in Azure. <br/><br/> Als prestatiegeschiedenis beschikbaar is, beschouwt Azure migreren het gebruikte geheugen voor vergelijking. Als een factor comfort is opgegeven, wordt het gebruikte geheugen vermenigvuldigd met de factor comfort.<br/><br/> Als er geen geschiedenis van die het toegewezen geheugen wordt gebruikt, zonder de factor comfort toepassen.<br/><br/> | Niet-gereed als geheugen groter dan 448 GB is.
**Schijf voor opslag** | Toegewezen grootte van een schijf moet 4 TB (4096 GB) of minder.<br/><br/> Het aantal schijven die zijn gekoppeld aan de machine moet 65 of minder, inclusief de besturingssysteemschijf. | Niet gereed eventuele schijf is groter dan 4 TB of als er meer dan 65 schijven die zijn gekoppeld aan de machine.
**Netwerken** | Een machine moet 32 of minder NIC's gekoppeld. | Niet-gereed als de computer meer dan 32 NIC's heeft

### <a name="guest-operating-system"></a>Gastbesturingssysteem
Samen met VM-eigenschappen Azure migreren ook gekeken naar het gastbesturingssysteem van de lokale virtuele machine om te bepalen of de virtuele machine kan worden uitgevoerd op Azure.

> [!NOTE]
> Azure migreren houdt rekening met het besturingssysteem die is opgegeven in vCenter Server kan de volgende analyse uitvoeren. Omdat de detectie uitgevoerd door Azure migreren op basis van het toestel is, heeft een manier om te controleren of het besturingssysteem in de virtuele machine hetzelfde als de één opgegeven in de vCenter-Server is niet meer.

De volgende logica wordt gebruikt door Azure migreren om te identificeren van de Azure gereedheid van de virtuele machine op basis van het besturingssysteem.

**Besturingssysteem** | **Details** | **Gereedheidsstatus van Azure**
--- | --- | ---
Windows Server 2016 & alle SP 's | Azure biedt volledige ondersteuning. | Gereed voor Azure
Windows Server 2012 R2 & alle SP 's | Azure biedt volledige ondersteuning. | Gereed voor Azure
Windows Server 2012 & alle SP 's | Azure biedt volledige ondersteuning. | Gereed voor Azure
Windows Server 2008 R2 met alle SP 's | Azure biedt volledige ondersteuning.| Gereed voor Azure
Windows Server 2003-2008 R2 | Deze besturingssystemen zijn geslaagd voor hun einde van ondersteuning voor de datum en het moet een [aangepaste ondersteunen overeenkomst (CSA)](https://aka.ms/WSosstatement) voor ondersteuning in Azure. | Voorwaardelijk klaar te maken voor Azure, moet u een upgrade van het besturingssysteem vóór de migratie naar Azure.
Windows 2000, 98, 95 NT 3.1, MS-DOS | Het einde van ondersteuning datum zijn geslaagd voor deze besturingssystemen, de machine kan worden opgestart in Azure, maar er is geen ondersteuning van het besturingssysteem wordt verstrekt door Azure. | Voorwaardelijk klaar voor Azure, verdient het besturingssysteem bijwerken voordat u migreert naar Azure.
Client voor Windows 7, 8 en 10 | Azure biedt ondersteuning voor Visual Studio-abonnement. | Voorwaardelijk gereed voor Azure
Windows Vista, XP Professional | Het einde van ondersteuning datum zijn geslaagd voor deze besturingssystemen, de machine kan worden opgestart in Azure, maar er is geen ondersteuning van het besturingssysteem wordt verstrekt door Azure. | Voorwaardelijk klaar voor Azure, verdient het besturingssysteem bijwerken voordat u migreert naar Azure.
Linux | Azure onderschrijft deze [Linux-besturingssystemen](../virtual-machines/linux/endorsed-distros.md). Andere Linux-besturingssystemen kunnen worden opgestart in Azure, maar het is raadzaam het besturingssysteem bijwerkt naar een aangebracht versie voordat u migreert naar Azure. | Gereed voor Azure als de versie is goedgekeurd.<br/><br/>Voorwaardelijk klaar te maken als de versie niet is goedgekeurd.
Andere besturingssystemen<br/><br/> bijvoorbeeld, Oracle Solaris, Apple Mac OS enz., FreeBSD, enzovoort. | Azure biedt deze besturingssystemen niet goedkeuren. De machine kan worden opgestart in Azure, maar er is geen ondersteuning van het besturingssysteem wordt verstrekt door Azure. | Voorwaardelijk klaar voor Azure, het is raadzaam een ondersteund besturingssysteem installeren voordat u migreert naar Azure.  
OS opgegeven als *andere* in vCenter-Server | Het besturingssysteem identificeren in dit geval niet in Azure migreren. | Onbekende gereedheid. Zorg ervoor dat het besturingssysteem in de virtuele machine in Azure wordt ondersteund.
32-bits besturingssystemen. | De machine kan worden opgestart in Azure, maar Azure kan geen volledige ondersteuning bieden. | Voorwaardelijk klaar te maken voor Azure, overweeg dan het besturingssysteem van de machine met 32-bits besturingssysteem upgraden naar de 64-bits besturingssysteem voordat u migreert naar Azure.

## <a name="sizing"></a>Grootte aanpassen

Nadat een machine is gemarkeerd als gereed voor Azure, Azure migreren de grootte van de virtuele machine en de schijven voor Azure. Als het criterium sizing is opgegeven in de eigenschappen van de assessment doen formaat op basis van prestaties, beschouwt Azure migreren de prestatiegeschiedenis van de machine naar een VM-grootte in Azure te identificeren. Deze methode is nuttig in scenario's waarin u de lokale virtuele machine te veel hebt toegewezen, maar het gebruik van de laag is en u wilt het juiste formaat de virtuele machines in Azure om op te slaan kosten.

> [!NOTE]
> Azure migreren verzamelt prestatiegeschiedenis van de lokale virtuele machines van de vCenter-Server. Zorg ervoor dat de instelling van de statistieken in vCenter-Server is ingesteld op niveau 3 zodat het juiste formaat nauwkeurige en wachten op ten minste een dag voor bij de detectie van de lokale virtuele machines start. Als de instelling van de statistieken in vCenter Server lager niveau 3 is, worden prestatiegegevens voor de schijf en netwerk is niet verzameld.

Als u niet wilt rekening met de prestatiegeschiedenis van de voor VM-grootte en wilt worden van de virtuele machine als-is naar Azure, kunt u het criterium sizing als *als lokale* en Azure migreren wordt vervolgens het formaat van de virtuele machines op basis van de on-premises configuratie zonder rekening te houden de gebruiksgegevens. Formaat van schijf, in dit geval wordt nog steeds gebaseerd op prestatiegegevens.

### <a name="performance-based-sizing"></a>Formaat op basis van prestaties

Migreren van Azure begint met de schijven die zijn gekoppeld aan de virtuele machine, gevolgd door netwerkadapters voor schaling op basis van prestaties en vervolgens maps een Azure VM op basis van de compute-vereisten van de lokale virtuele machine.

- **Opslag**: Azure migreren probeert te koppelen van elke schijf die is gekoppeld aan de machine naar een schijf in Azure.

    > [!NOTE]
    > Azure migreren ondersteunt alleen beheerde schijven voor evaluatie.

    - Azure migreren vermenigvuldigen om de effectieve schijf-i/o per seconde (IOPS) en doorvoer (MBps), de schijf-IOPS en de doorvoer met de factor comfort. Op basis van de effectieve IOP's en -waarden van doorvoer, identificeert Azure migreren als de schijf moet worden toegewezen aan een standard- of premium-schijf in Azure.
    - Als Azure migreren, een schijf met de vereiste IOPS & doorvoer vinden kan, wordt de computer als ongeschikt zijn gemarkeerd voor Azure. [Meer informatie](../azure-subscription-service-limits.md#storage-limits) over Azure beperkt per schijf en de VM.
    - Als het een set geschikte schijven gevonden, selecteert Azure migreren degene die ondersteuning bieden voor de opslag redundantie methode en de locatie die is opgegeven in de instellingen van de assessment.
    - Als er meerdere schijven voor in aanmerking komt, wordt deze met de laagste kosten geselecteerd.
    - Als u prestatiegegevens voor de schijven in niet beschikbaar is, alle schijven zijn toegewezen aan standaardschijven in Azure.

- **Netwerk**: Azure migreren gezocht naar een virtuele machine van Azure die ondersteuning voor het aantal netwerkadapters die zijn gekoppeld aan de lokale machine en de prestaties vereist zijn voor deze netwerkadapters biedt.
    - Als u de effectieve netwerkervaring voor prestaties van de lokale virtuele machine, migreren van Azure maakt een aggregatie van de gegevens verzonden per seconde (MBps) buiten de machine (netwerk), op alle netwerkadapters en de factor comfort past. Dit nummer wordt gebruikt voor het vinden van een virtuele machine van Azure die ondersteuning voor de vereiste netwerkprestaties biedt.
    - Samen met de netwerkprestaties, het ook acht als de virtuele machine van Azure ondersteuning voor de vereiste bieden kunnen het aantal netwerkadapters.
    - Als geen prestatiegegevens netwerk beschikbaar is, kan alleen met het aantal netwerkadapters wordt beschouwd als voor VM-grootte.

- **COMPUTE**: nadat de vereisten voor appopslag en netwerk worden berekend, Azure migreren beschouwt CPU en geheugen vereisten voor het zoeken van een geschikte VM-grootte in Azure.
    - Azure migreren wordt gekeken naar de gebruikte kernen en het geheugen en de factor comfort om de effectieve kernen en het geheugen past. Op basis van dit nummer, wordt geprobeerd een geschikte VM-grootte niet vinden in Azure.
    - Als er geen geschikte grootte wordt gevonden, wordt de machine gemarkeerd als niet geschikt voor Azure.
    - Als een geschikte grootte wordt gevonden, Azure migreren is van toepassing de berekeningen voor opslag en netwerken. Deze locatie en prijzen van laag-instellingen voor de laatste aanbeveling voor VM-grootte is van toepassing.
    - Als er meer Azure VM-grootten in aanmerking komen, wordt de grootte met de laagste kosten aanbevolen.

### <a name="as-on-premises-sizing"></a>Als de lokale formaat wijzigen
Als het criterium sizing *als lokale sizing*, Azure migreren houdt geen rekening met de prestatiegeschiedenis van de virtuele machines en virtuele machines op basis van de grootte van de lokale toegewezen toewijst. Echter voor schaling van de schijf beschouwt het prestatiegeschiedenis van de schijven aan te bevelen Standard of Premium-schijven.  
- **Opslag**: Azure migreren elke schijf die is gekoppeld aan de machine naar een schijf in Azure wordt toegewezen.

    > [!NOTE]
    > Azure migreren ondersteunt alleen beheerde schijven voor evaluatie.

    - Azure migreren vermenigvuldigen om de effectieve schijf-i/o per seconde (IOPS) en doorvoer (MBps), de schijf-IOPS en de doorvoer met de factor comfort. Op basis van de effectieve IOP's en -waarden van doorvoer, identificeert Azure migreren als de schijf moet worden toegewezen aan een standard- of premium-schijf in Azure.
    - Als Azure migreren, een schijf met de vereiste IOPS & doorvoer vinden kan, wordt de computer als ongeschikt zijn gemarkeerd voor Azure. [Meer informatie](../azure-subscription-service-limits.md#storage-limits) over Azure beperkt per schijf en de VM.
    - Als het een set geschikte schijven gevonden, selecteert Azure migreren degene die ondersteuning bieden voor de opslag redundantie methode en de locatie die is opgegeven in de instellingen van de assessment.
    - Als er meerdere schijven voor in aanmerking komt, wordt deze met de laagste kosten geselecteerd.
    - Als u prestatiegegevens voor de schijven in niet beschikbaar is, alle schijven zijn toegewezen aan standaardschijven in Azure.
- **Netwerk**: voor elke netwerkadapter een netwerkadapter in Azure wordt aanbevolen.
- **COMPUTE**: Azure migreren wordt gekeken naar het aantal kernen en de grootte van het geheugen van de lokale virtuele machine en raadt aan om een Azure-VM met dezelfde configuratie. Als er meer Azure VM-grootten in aanmerking komen, wordt de grootte met de laagste kosten aanbevolen. Gebruiksgegevens voor de CPU en geheugen is niet in aanmerking voor als sizing lokale.

### <a name="confidence-rating"></a>Betrouwbaarheidsclassificatie

Elke evaluatie in Azure Migrate wordt gekoppeld aan een betrouwbaarheidsclassificatie van 1 ster tot 5 sterren (1 ster is de laagste score en 5 sterren de hoogste). De betrouwbaarheidsclassificatie wordt aan een evaluatie toegewezen op basis van de beschikbaarheid van de gegevenspunten die nodig zijn om de evaluatie te berekenen. De betrouwbaarheidsclassificatie van een evaluatie helpt u om de betrouwbaarheid in te schatten van de aanbevelingen voor de grootte die Azure Migrate geeft.

Als u de grootte van de VM instelt op basis van de prestaties, heeft Azure Migrate de gebruiksgegevens nodig voor de CPU en het geheugen. Ook voor de grootte van elke schijf die is gekoppeld aan de virtuele machine, moet de lezen/schrijven IOPS en doorvoer. Ook heeft Azure Migrate voor iedere netwerkadapter die aan de VM is gekoppeld, gegevens over het inkomende/uitgaande netwerkverkeer nodig om de grootte in te kunnen stellen op basis van de prestaties. Als een of meer van de bovenstaande gebruiksgetallen niet beschikbaar zijn in vCenter Server, is de aanbeveling voor de grootte die Azure Migrate doet, mogelijk niet betrouwbaar. De classificatie vertrouwen voor de beoordeling is afhankelijk van het percentage van de gegevens die beschikbaar zijn, zoals hieronder opgegeven:

   **Beschikbaarheid van gegevenspunten** | **Betrouwbaarheidsclassificatie**
   --- | ---
   0%-20% | 1 ster
   21%-40% | 2 sterren
   41%-60% | 3 sterren
   61%-80% | 4 sterren
   81%-100% | 5 sterren

Het kan voorkomen dat niet alle gegevenspunten beschikbaar zijn voor een evaluatie. Dit kan de volgende oorzaken hebben:
- De instelling van de statistieken in vCenter Server is niet ingesteld op niveau 3. Als de instelling voor statistieken in vCenter Server op een lager niveau dan niveau 3 is ingesteld, worden de prestatiegegevens voor de schijf en het netwerk niet verzameld vanuit vCenter Server. In dat geval wordt de aanbeveling van Azure Migrate voor de schijf en het netwerk niet gebaseerd op gebruik. Zonder rekening te houden de IOP's / doorvoer van de schijf, kan niet worden geïdentificeerd Azure migreren als de schijf moet een premium-schijf in Azure, daarom in dit geval Azure migreren raadt standaardschijven voor alle schijven.
- De instelling voor statistieken in vCenter Server is korte tijd op niveau 3 ingesteld geweest voordat de detectie begon. Laten we het scenario als voorbeeld nemen waarin u het niveau van de instelling voor statistieken vandaag bijvoorbeeld op 3 zet en morgen (na 24 uur) met de detectie begint met behulp van het collector-apparaat. Als u een evaluatie voor één dag maakt, hebt u alle gegevenspunten en is de betrouwbaarheidsclassificatie van de evaluatie 5 sterren. Maar als u de duur van de prestaties in de evaluatie-eigenschappen wijzigt in één maand, gaat de betrouwbaarheidsclassificatie omlaag, omdat de prestatiegegevens voor schijven en netwerken voor de laatste maand niet beschikbaar zijn. Als u rekening wilt houden met de prestatiegegevens voor de laatste maand, kunt u het beste de instelling voor statistieken in vCenter Server op niveau 3 laten staan gedurende één maand voordat u met de detectie begint.
- Er zijn enkele VM's uitgeschakeld geweest in de periode waarover de evaluatie wordt berekend. Als er VM's zijn die gedurende een bepaalde tijd uitgeschakeld zijn geweest, zal vCenter Server voor die periode geen prestatiegegevens hebben.
- Er zijn enkele VM's gemaakt tijdens de periode waarover de evaluatie wordt berekend. Als u bijvoorbeeld een evaluatie maakt voor de prestatiegeschiedenis van de laatste maand, maar er een week geleden enkele VM's in de omgeving zijn gemaakt. In dergelijke gevallen is de prestatiegeschiedenis van de nieuwe virtuele machines niet voor de hele periode beschikbaar.

> [!NOTE]
> Als de betrouwbaarheidsclassificatie van een evaluatie lager dan 4 sterren is, raden we u aan om de instelling voor statistieken in vCenter Server op 3 te zetten, te wachten gedurende de periode waarover u de evaluatie wilt uitvoeren (1 dag/1 week/1 maand) en vervolgens een detectie en evaluatie uit te voeren. Als de voorgaande stappen niet kunnen worden uitgevoerd, wordt de grootte mogelijk niet op betrouwbare wijze ingesteld. In dat geval doet u er verstandig aan over te schakelen naar *instelling van de grootte op basis van 'zoals on-premises'* door de evaluatie-eigenschappen te wijzigen.

## <a name="monthly-cost-estimation"></a>Maandelijkse kosten schatting

Nadat sizing aanbevelingen voltooid zijn, berekend Azure migreren na de migratie berekenings- en kosten.

- **COMPUTE-kosten**: de API van de facturering voor het berekenen van de maandelijkse kosten voor de virtuele machine met behulp van de aanbevolen grootte van de virtuele machine in Azure, Azure migreren gebruikt. De berekening wordt het besturingssysteem, software assurance, locatie en valuta-instellingen rekening. Deze cumuleert de kosten van alle machines, voor het berekenen van de totale maandelijkse compute-kosten.
- **Opslagkosten**: de maandelijkse opslag kosten voor een machine wordt berekend door het samenvoegen van de maandelijkse kosten van alle schijven die zijn gekoppeld aan de machine. De totale maandelijkse opslagkosten berekend Azure migreren door de kosten voor opslag van alle machines samen te voegen. De berekening niet op dit moment aanbiedingen die zijn opgegeven in de instellingen voor evaluatie in aanmerking genomen.

Kosten worden weergegeven in de opgegeven in de instellingen voor beoordeling valuta.


## <a name="next-steps"></a>Volgende stappen

[Maken van een beoordeling voor lokale virtuele VMware-machines](tutorial-assessment-vmware.md)
