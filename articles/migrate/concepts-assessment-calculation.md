---
title: Assessment berekeningen in Azure migreren | Microsoft Docs
description: Biedt een overzicht van assessment berekeningen in de service Azure migreren.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 06/02/2017
ms.author: raynew
ms.openlocfilehash: db09ff30ff9f3852e84162b8400572e76515230f
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2018
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
**Cores** | Het aantal kernen op de machines moet gelijk zijn aan of kleiner is dan het maximum aantal kernen (32) ondersteund voor een virtuele machine in Azure.<br/><br/> Als prestatiegeschiedenis beschikbaar is, Azure migreren rekening gehouden met de gebruikte kernen voor vergelijking. Als een factor comfort is opgegeven in de instellingen voor de beoordeling, wordt het aantal gebruikte kernen vermenigvuldigd met de factor comfort.<br/><br/> Als er geen prestatiegeschiedenis van, de toegewezen kernen Azure migreren gebruikt zonder het toepassen van de factor comfort. | Niet-gereed als het aantal kernen groter dan 32 is. 
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
Windows 2000, 98, 95, NT, 3.1, MS-DOS | Het einde van ondersteuning datum zijn geslaagd voor deze besturingssystemen, de machine kan worden opgestart in Azure, maar er is geen ondersteuning van het besturingssysteem wordt verstrekt door Azure. | Voorwaardelijk klaar voor Azure, verdient het besturingssysteem bijwerken voordat u migreert naar Azure.
Client voor Windows 7, 8 en 10 | Azure biedt ondersteuning voor Visual Studio-abonnement. | Voorwaardelijk klaar te maken voor Azure
Windows Vista, XP Professional | Het einde van ondersteuning datum zijn geslaagd voor deze besturingssystemen, de machine kan worden opgestart in Azure, maar er is geen ondersteuning van het besturingssysteem wordt verstrekt door Azure. | Voorwaardelijk klaar voor Azure, verdient het besturingssysteem bijwerken voordat u migreert naar Azure.
Linux | Azure onderschrijft deze [Linux-besturingssystemen](../virtual-machines/linux/endorsed-distros.md). Andere Linux-besturingssystemen kunnen worden opgestart in Azure, maar het is raadzaam het besturingssysteem bijwerkt naar een aangebracht versie voordat u migreert naar Azure. | Gereed voor Azure als de versie is goedgekeurd.<br/><br/>Voorwaardelijk klaar te maken als de versie niet is goedgekeurd.
Andere besturingssystemen<br/><br/> bijvoorbeeld, Oracle Solaris, Apple Mac OS enz., FreeBSD, enzovoort. | Azure biedt deze besturingssystemen niet goedkeuren. De machine kan worden opgestart in Azure, maar er is geen ondersteuning van het besturingssysteem wordt verstrekt door Azure. | Voorwaardelijk klaar voor Azure, het is raadzaam een ondersteund besturingssysteem installeren voordat u migreert naar Azure.  
OS opgegeven als *andere* in vCenter-Server | Het besturingssysteem identificeren in dit geval niet in Azure migreren. | Onbekende gereedheid. Zorg ervoor dat het besturingssysteem in de virtuele machine in Azure wordt ondersteund. 
32-bits besturingssystemen. | De machine kan worden opgestart in Azure, maar Azure kan geen volledige ondersteuning bieden. | Voorwaardelijk klaar te maken voor Azure, overweeg dan het besturingssysteem van de machine met 32-bits besturingssysteem upgraden naar de 64-bits besturingssysteem voordat u migreert naar Azure.

## <a name="sizing"></a>Schaling

Nadat een machine is gemarkeerd als gereed voor Azure, Azure migreren de grootte van de virtuele machine en de schijven voor Azure. Als het criterium sizing is opgegeven in de eigenschappen van de assessment doen formaat op basis van prestaties, beschouwt Azure migreren de prestatiegeschiedenis van de machine naar een VM-grootte in Azure te identificeren. Deze methode is nuttig in scenario's waarin u de lokale virtuele machine te veel hebt toegewezen, maar het gebruik van de laag is en u wilt het juiste formaat de virtuele machines in Azure om op te slaan kosten.

> [!NOTE]
> Azure migreren verzamelt prestatiegeschiedenis van de lokale virtuele machines van de vCenter-Server. Zorg ervoor dat de instelling van de statistieken in vCenter-Server is ingesteld op niveau 3 zodat het juiste formaat nauwkeurige en wachten op ten minste een dag voor bij de detectie van de lokale virtuele machines start. Als de instelling van de statistieken in vCenter Server lager niveau 3 is, worden prestatiegegevens voor de schijf en netwerk is niet verzameld. 

Als u niet wilt rekening met de prestatiegeschiedenis van de en wilt worden van de virtuele machine als-is naar Azure, kunt u het criterium sizing als *als lokale* en Azure migreren wordt vervolgens het formaat van de virtuele machines op basis van de on-premises-configuratie zonder kijken we naar de gebruiksgegevens.

### <a name="performance-based-sizing"></a>Formaat op basis van prestaties

Migreren van Azure begint met de schijven die zijn gekoppeld aan de virtuele machine, gevolgd door netwerkadapters voor schaling op basis van prestaties en vervolgens maps een Azure VM op basis van de compute-vereisten van de lokale virtuele machine. 
 
- **Schijven**: Azure migreren probeert te koppelen van elke schijf die is gekoppeld aan de machine naar een schijf in Azure. 
    
    > [!NOTE]
    > Azure migreren ondersteunt alleen beheerde schijven voor evaluatie.
    
    - Azure migreren vermenigvuldigen om de effectieve schijf-i/o per seconde (IOPS) en doorvoer (MBps), de schijf-IOPS en de doorvoer met de factor comfort. Op basis van de effectieve IOP's en -waarden van doorvoer, identificeert Azure migreren als de schijf moet worden toegewezen aan een standard- of premium-schijf in Azure.
    - Als Azure migreren, een schijf met de vereiste IOPS & doorvoer vinden kan, wordt de computer als ongeschikt zijn gemarkeerd voor Azure. [Meer informatie](../azure-subscription-service-limits.md#storage-limits) over Azure beperkt per schijf en de VM.
    - Als het een set geschikte schijven gevonden, selecteert Azure migreren degene die ondersteuning bieden voor de opslag redundantie methode en de locatie die is opgegeven in de instellingen van de assessment.
    - Als er meerdere schijven voor in aanmerking komt, wordt deze met de laagste kosten geselecteerd.
    - Als u prestatiegegevens voor de schijven in niet beschikbaar is, alle schijven zijn toegewezen aan standaardschijven in Azure.

- **Netwerkadapters**: Azure migreren gezocht naar een virtuele machine van Azure die ondersteuning voor het aantal netwerkadapters die zijn gekoppeld aan de lokale machine en de prestaties vereist zijn voor deze netwerkadapters biedt.
    - Als u de effectieve netwerkervaring voor prestaties van de lokale virtuele machine, migreren van Azure maakt een aggregatie van de gegevens verzonden per seconde (MBps) buiten de machine (netwerk), op alle netwerkadapters en de factor comfort past. Dit nummer wordt gebruikt voor het vinden van een virtuele machine van Azure die ondersteuning voor de vereiste netwerkprestaties biedt.
    - Samen met de netwerkprestaties, het ook acht als de virtuele machine van Azure ondersteuning voor de vereiste bieden kunnen het aantal netwerkadapters.
    - Als geen prestatiegegevens netwerk beschikbaar is, kan alleen met het aantal netwerkadapters wordt beschouwd als voor VM-grootte.

- **VM-grootte**: nadat de vereisten voor appopslag en netwerk worden berekend, Azure migreren beschouwt CPU en geheugen vereisten voor het zoeken van een geschikte VM-grootte in Azure.
    - Azure migreren wordt gekeken naar de gebruikte kernen en het geheugen en de factor comfort om de effectieve kernen en het geheugen past. Op basis van dit nummer, wordt geprobeerd een geschikte VM-grootte niet vinden in Azure.
    - Als er geen geschikte grootte wordt gevonden, wordt de machine gemarkeerd als niet geschikt voor Azure.
    - Als een geschikte grootte wordt gevonden, Azure migreren is van toepassing de berekeningen voor opslag en netwerken. Deze locatie en prijzen van laag-instellingen voor de laatste aanbeveling voor VM-grootte is van toepassing.
    - Als er meerdere in aanmerking komende Azure VM-grootten, wordt de categorie met de laagste kosten aanbevolen.

### <a name="as-on-premises-sizing"></a>Als de lokale formaat wijzigen
Als het criterium sizing *als lokale sizing*, Azure migreren houdt geen rekening met de prestatiegeschiedenis van de virtuele machines en virtuele machines toewijst en schijven op basis van de grootte van de lokale toegewezen.
- **Opslag**: voor elke schijf, een standaard schijf in Azure met dezelfde grootte hebben als de lokale schijf wordt aanbevolen.
- **Netwerk**: voor elke netwerkadapter een netwerkadapter in Azure wordt aanbevolen.
- **COMPUTE**: Azure migreren wordt gekeken naar het aantal kernen en de grootte van het geheugen van de lokale virtuele machine en raadt aan om een Azure-VM met dezelfde configuratie. Als er meerdere in aanmerking komende Azure VM-grootten, wordt de categorie met de laagste kosten aanbevolen.
 
### <a name="confidence-rating"></a>Betrouwbaarheid

Elke beoordeling in Azure migreren is gekoppeld aan een classificatie van vertrouwen met een bereik van 1 ster tot 5 sterren (1 ster wordt laagste en 5 sterren wordt hoogste). De betrouwbaarheid wordt toegewezen aan een beoordeling op basis van de beschikbaarheid van de gegevenspunten die nodig zijn voor de beoordeling te berekenen. Kunt u de schatting van de betrouwbaarheid van de grootte van aanbevelingen die door Azure migreren. 

Betrouwbaarheid is handig als u de *formaat op basis van prestaties* als niet alle gegevenspunten zijn mogelijk beschikbaar. Voor *als lokale sizing*, de betrouwbaarheid is altijd 5 sterren, als de benodigde grootte van de virtuele machine gegevens migreren van Azure heeft. 

Voor op basis van prestaties sizing moet Azure migreren de gebruiksgegevens voor de CPU en geheugen. Voor elke schijf die is gekoppeld aan de virtuele machine, moet de lezen/schrijven IOPS en doorvoer te doen formaat op basis van prestaties. Op dezelfde manier voor elke netwerkadapter die is gekoppeld aan de virtuele machine, moet Azure migreren het netwerk in/out doen formaat op basis van prestaties. Als een van de bovenstaande gebruik nummers niet beschikbaar in de vCenter-Server zijn, de grootte aanbeveling uitgevoerd door Azure migreren mogelijk niet betrouwbaar. Afhankelijk van het percentage van de gegevens die beschikbaar zijn, wordt de betrouwbaarheid voor de beoordeling beschikbaar:

   **Beschikbaarheid van gegevenspunten** | Betrouwbaarheid
   --- | ---
   0%-20% | 1 ster
   21%-40% | 2 sterren
   41%-60% | 3 sterren
   61%-80% | 4 sterren
   81%-100% | 5 sterren

Een evaluatie kan niet alle gegevenspunten beschikbaar vanwege een van de volgende redenen hebben:
- De instelling van de statistieken in vCenter die server niet is ingesteld op niveau 3 en de evaluatie heeft sizing als criterium formaat op basis van prestaties. Als de instelling van de statistieken in vCenter Server lager dan niveau 3 is, is geen prestatiegegevens voor de schijf en netwerk verzameld van vCenter-Server. In dit geval dat de aanbeveling is opgegeven door de Azure migreren voor de schijf en netwerk alleen is gebaseerd op wat lokale is toegewezen. Voor opslag raadt Azure migreren standaardschijven als er geen manier om te bepalen is of de schijf hoog IOP's / doorvoer is en moet premium-schijven.
- De instelling van de statistieken in vCenter Server is ingesteld op niveau 3 gedurende een korte periode voordat de detectie bij de start. Als u het niveau van de instelling statistieken in 3 vandaag en kick uitschakelen van de detectie met behulp van het toestel collector morgen (na 24 uur), wijzigt als u een beoordeling voor één dag maakt, hebt u bijvoorbeeld alle gegevenspunten. Maar als u de duur van de prestaties in de eigenschappen van de assessment in een maand verandert, de betrouwbaarheid wordt uitgeschakeld als de schijf en netwerk prestatiegegevens voor de laatste één maand is niet beschikbaar. Als u wilt rekening houden met de prestatiegegevens van de laatste één maand, verdient het aanbeveling vCenter Server statistieken instelling op niveau 3 gedurende één maand voordat u ere van de detectie te houden. 
- Enkele virtuele machines zijn afgesloten tijdens de periode waarvoor de beoordeling wordt berekend. Als er geen VM's zijn uitgeschakeld voor bepaalde duur, heeft vCenter-Server geen prestatiegegevens van de voor deze periode. 
- Enkele virtuele machines zijn gemaakt tussen de periode waarvoor de beoordeling wordt berekend. Bijvoorbeeld, als u maakt een beoordeling van de prestatiegeschiedenis van afgelopen één maand, maar enkele virtuele machines zijn gemaakt in de omgeving alleen een week geleden. In dergelijke gevallen de prestatiegeschiedenis van de nieuwe virtuele machines niet meer worden er voor de gehele duur.

> [!NOTE]
> Als de betrouwbaarheid van de beoordeling lager dan 3 sterren is, wij raden u aan te wijzigen van de vCenter-Server statistieken instellingen niveau tot 3, wacht u totdat de duur die u wilt overwegen voor beoordeling (1 dag/1 week/1 maand) en voert u detectie en beoordeling. Als de voorgaande kan niet worden uitgevoerd, op basis van prestaties sizing mogelijk niet betrouwbaar en het wordt aanbevolen overschakelen naar *als lokale sizing* door de eigenschappen van de evaluatie te wijzigen.

## <a name="monthly-cost-estimation"></a>Maandelijkse kosten schatting

Nadat sizing aanbevelingen voltooid zijn, berekend Azure migreren na de migratie berekenings- en kosten.

- **COMPUTE-kosten**: de API van de facturering voor het berekenen van de maandelijkse kosten voor de virtuele machine met behulp van de aanbevolen grootte van de virtuele machine in Azure, Azure migreren gebruikt. De berekening wordt het besturingssysteem, software assurance, locatie en valuta-instellingen rekening. Deze cumuleert de kosten van alle machines, voor het berekenen van de totale maandelijkse compute-kosten.
- **Opslagkosten**: de maandelijkse opslag kosten voor een machine wordt berekend door het samenvoegen van de maandelijkse kosten van alle schijven die zijn gekoppeld aan de machine. De totale maandelijkse opslagkosten berekend Azure migreren door de kosten voor opslag van alle machines samen te voegen. De berekening niet op dit moment aanbiedingen die zijn opgegeven in de instellingen voor evaluatie in aanmerking genomen.

Kosten worden weergegeven in de opgegeven in de instellingen voor beoordeling valuta. 


## <a name="next-steps"></a>Volgende stappen

[Maken van een beoordeling voor lokale virtuele VMware-machines](tutorial-assessment-vmware.md)
