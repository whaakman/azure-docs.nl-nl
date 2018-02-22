---
title: Azure Site Recovery implementatie Planner voor Hyper-V-die naar Azure | Microsoft Docs
description: In dit artikel beschrijft analyse van een gegenereerde rapport door Azure Site Recovery-implementatie plannen voor een Hyper-V op Azure scenario.
services: site-recovery
author: nsoneji
manager: garavd
ms.service: site-recovery
ms.topic: article
ms.date: 02/14/2018
ms.author: nisoneji
ms.openlocfilehash: 060d51406f67ad8a55cdf61506cd66f5390ebe4c
ms.sourcegitcommit: d1f35f71e6b1cbeee79b06bfc3a7d0914ac57275
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/22/2018
---
# <a name="analyze-the-azure-site-recovery-deployment-planner-report"></a>Het Azure Site Recovery implementatie Planner rapport analyseren
Dit artikel worden de werkbladen in het Excel-rapport gegenereerd door Azure Site Recovery-implementatie plannen voor een Hyper-V op Azure scenario.

## <a name="on-premises-summary"></a>Samenvatting on-premises
Het werkblad lokale samenvatting biedt een overzicht van de profiled Hyper-V-omgeving.

![Samenvatting on-premises](media/hyper-v-deployment-planner-analyze-report/on-premises-summary-h2a.png)

**Begindatum** en **einddatum**: de begin- en einddatums van de profileringsgegevens in aanmerking voor het genereren van rapporten. De begindatum is standaard de datum waarop de profilering start en de einddatum de datum waarop de profilering stopt. Deze informatie kan de waarden 'StartDate' en 'EndDate' zijn als het rapport is gegenereerd met de volgende parameters.

**Totale aantal dagen van profilering**: het totale aantal dagen van profilering tussen de begin- en einddatum waarvoor het rapport wordt gegenereerd.

**Aantal compatibele virtuele machines**: het totale aantal compatibele virtuele machines waarvoor de vereiste netwerkbandbreedte, het vereiste aantal van de storage-accounts en Azure kernen worden berekend.

**Totale aantal schijven voor alle compatibele virtuele machines**: het totale aantal schijven voor alle compatibele virtuele machines.

**Gemiddeld aantal schijven per compatibele virtuele machine**: het gemiddelde aantal schijven dat voor alle compatibele virtuele machines is berekend.

**Gemiddelde schijfgrootte (GB)**: de gemiddelde schijfgrootte die voor alle compatibele virtuele machines is berekend.

**Gewenste RPO (minuten)**: ofwel de standaardwaarde van het herstelpunt doel of de waarde die is doorgegeven voor de parameter 'DesiredRPO' op het moment van het genereren van rapporten om in te schatten van de vereiste bandbreedte.

**Gewenste bandbreedte (Mbps)**: de waarde die u op het moment van het genereren van rapporten om in te schatten haalbare beoogd herstelpunt (RPO) doorgegeven voor de parameter "Bandbreedte".

**Waargenomen typisch gegevensverloop per dag (GB)**: het gemiddelde gegevensverloop dat voor alle profileringsdagen is vastgesteld.

## <a name="recommendations"></a>Aanbevelingen 
Het werkblad met de aanbevelingen voor het Hyper-V naar Azure-rapport heeft de volgende gegevens aan de hand van de geselecteerde gewenste RPO:

![Rapport Aanbevelingen voor Hyper-V naar Azure](media/hyper-v-deployment-planner-analyze-report/Recommendations-h2a.png)

### <a name="profile-data"></a>Profielgegevens
![Profielgegevens](media/hyper-v-deployment-planner-analyze-report/profile-data-h2a.png)

**Periode geprofileerde gegevens**: de periode waarin de profilering is uitgevoerd. Het hulpprogramma neemt standaard alle geprofileerde gegevens in de berekening op. Als u de optie StartDate en EndDate bevat in rapporten te genereren gebruikt, wordt het rapport gegenereerd voor de specifieke periode. 

**Aantal Hyper-V-servers profiel**: het aantal Hyper-V-servers waarvan VM's rapport is gegenereerd. Selecteer het getal aan dat de naam van de Hyper-V-servers weergeven. De On-premises eisen aan opslag blad wordt geopend met alle servers samen met hun opslagvereisten weergeven. 

**Gewenste RPO**: het gewenste beoogde herstelpunt (RPO) voor uw implementatie. Standaard wordt de vereiste netwerkbandbreedte berekend voor de RPO-waarden van 15, 30 en 60 minuten. Op basis van de selectie worden de betrokken waarden bijgewerkt op het blad. Als u de parameter DesiredRPOinMin gebruikt tijdens het genereren van het rapport, wordt die waarde weergegeven in het resultaat van de gewenste RPO.

### <a name="profiling-overview"></a>Overzicht van profilering
![Overzicht van profilering](media/hyper-v-deployment-planner-analyze-report/profiling-overview-h2a.png)

**Totale aantal geprofileerde virtuele machines**: het totale aantal virtuele machines waarvan de geprofileerde gegevens beschikbaar zijn. Als de VMListFile namen van alle virtuele machines die niet zijn profiel heeft, wordt deze VMs worden niet meegenomen in het rapport genereren en zijn uitgesloten van het totale aantal voor profiled virtuele machines.

**Compatibele virtuele machines**: het aantal virtuele machines dat op Azure kan worden beveiligd met Azure Site Recovery. Het is het totale aantal compatibele virtuele machines waarvoor de vereiste netwerkbandbreedte, het aantal van opslagaccounts en het aantal kernen dat Azure worden berekend. De details van elke compatibele virtuele machine zijn beschikbaar in de sectie 'Compatibele VM's'.

**Niet-compatibele virtuele machines**: het aantal geprofileerde virtuele machines dat niet compatibel is met beveiliging met Site Recovery. De redenen voor incompatibiliteit worden vermeld in de sectie 'Niet-compatibele VM's'. Als de VMListFile namen van alle virtuele machines die niet waren profiling wordt gestart heeft, worden deze VMs uitgesloten van de niet-compatibele telling van de virtuele machines. Deze virtuele machines worden weergegeven als 'Gegevens niet gevonden' aan het einde van de sectie 'Niet-compatibele VM's'.

**Gewenste RPO**: het gewenste beoogde herstelpunt (Recovery Point Objective [RPO]) in minuten. Het rapport wordt gegenereerd voor drie RPO-waarden: 15 (standaard), 30 en 60 minuten. De aanbeveling bandbreedte in het rapport is gewijzigd op basis van uw selectie in de **RPO gewenst** vervolgkeuzelijst in de rechterbovenhoek van het blad. Als u het rapport gegenereerd met behulp van de parameter - DesiredRPO met een aangepaste waarde, ziet u deze aangepaste waarde als de standaard in de **RPO gewenst** vervolgkeuzelijst.

### <a name="required-network-bandwidth-mbps"></a>Vereiste netwerkbandbreedte (Mbps)
![Vereiste netwerkbandbreedte](media/hyper-v-deployment-planner-analyze-report/required-network-bandwidth-h2a.png)

**Om te voldoen aan de RPO 100% van de tijd**: de aanbevolen bandbreedte in Mbps moet worden toegewezen om te voldoen aan uw gewenste RPO 100 procent van de tijd. Deze bandbreedte moet worden toegewezen voor onveranderlijke replicatie van verschillen bij al uw compatibele virtuele machines om eventuele RPO-schendingen te voorkomen.

**Om te voldoen aan de RPO 90% van de tijd**: vanwege breedband prijzen of een andere reden niet kan u mogelijk de bandbreedte die nodig zijn om te voldoen aan uw gewenste RPO 100 procent van de tijd instellen. Als dit het geval is, kunt u een lagere bandbreedte-instelling die in de gewenste RPO 90 procent van de tijd voorzien kan. Voor inzicht in de gevolgen van het instellen van deze lagere bandbreedte bevat het rapport een wat-als-analyse van het aantal en de duur van de RPO-schendingen die u kunt verwachten.

**Doorvoer bereikt**: de doorvoer van de server waarop u de opdracht GetThroughput uitvoert naar de Azure-regio waar het opslagaccount zich bevindt. Dit nummer doorvoer geeft het geschatte die u bereiken kunt wanneer u de compatibel VM's beveiligen met behulp van Site Recovery. De Hyper-V-server opslag en netwerk-kenmerken moeten blijven hetzelfde als die van de server van waaruit u het hulpprogramma uitvoert.

Voor alle Enterprise-implementaties van Site Recovery wordt het gebruik van [ExpressRoute](https://aka.ms/expressroute) aanbevolen.

### <a name="required-storage-accounts"></a>Vereiste opslagaccounts
De volgende grafiek geeft het totale aantal opslagaccounts aan (Standard Storage en Premium Storage) dat is vereist voor het beveiligen van alle compatibele virtuele machines. Zie de sectie 'VM-opslagplaatsing' voor meer informatie over welk opslagaccount u moet gebruiken voor elke virtuele machine.

![Azure storage-accounts vereist](media/hyper-v-deployment-planner-analyze-report/required-storage-accounts-h2a.png)

### <a name="required-number-of-azure-cores"></a>Vereiste aantal Azure-kerngeheugens
Dit is het totale aantal kerngeheugens dat moet worden ingesteld vóór failover of testfailover van de compatibele virtuele machines. Als er in het abonnement onvoldoende kerngeheugens beschikbaar zijn, kan Site Recovery geen virtuele machines maken op het moment van een failover of testfailover.

![Vereiste aantal Azure-kerngeheugens](media/hyper-v-deployment-planner-analyze-report/required-number-of-azure-cores-h2a.png)


### <a name="additional-on-premises-storage-requirement"></a>Extra on-premises opslagvereiste

De totale vrije opslagruimte op Hyper-V-servers voor geslaagde initiële replicatie en replicatie van verschillen is vereist om ervoor te zorgen dat de VM-replicatie biedt geen ongewenste uitvaltijd voor uw productietoepassingen. Meer informatie over elke vereiste volume is beschikbaar in [eisen aan opslag lokale](#on-premises-storage-requirement). 

Om te begrijpen waarom vrije ruimte is vereist voor de replicatie, Zie de [eisen aan opslag On-premises](#why-do-i-need-free-space-on-the-hyper-v-server-for-the-replication) sectie.

![Lokale opslag vereist en kopieer frequentie](media/hyper-v-deployment-planner-analyze-report/on-premises-storage-and-copy-frequency-h2a.png)

### <a name="maximum-copy-frequency"></a>Maximale kopieerfrequentie
De aanbevolen maximale kopieerfrequentie moet voor de replicatie worden ingesteld om de gewenste RPO te bereiken. Standaard is 5 minuten. U kunt de kopieerfrequentie instellen op 30 seconden voor een betere RPO.

### <a name="what-if-analysis"></a>Wat als-analyse
![Wat-als-analyse](media/hyper-v-deployment-planner-analyze-report/what-if-analysis-h2a.png) deze analyse geeft een overzicht van hoeveel schendingen zich mogelijk voordoen tijdens het samenstellen van profiel periode bij het instellen van een lagere bandbreedte voor de gewenste RPO worden voldaan slechts 90 procent van de tijd. Op elke dag kunnen er een of meer RPO-schendingen optreden. De grafiek toont de piek-RPO van de dag. Op basis van deze analyse kunt u besluiten of het aantal RPO-schendingen voor alle dagen en de hoogste RPO per dag acceptabel zijn in combinatie met de opgegeven lagere bandbreedte. Als het is acceptabel, kunt u de lagere bandbreedte toewijzen voor replicatie. Als dit onacceptabel is, kunt u hogere bandbreedte zoals voorgesteld om te voldoen aan de gewenste RPO 100 procent van de tijd toewijzen. 

### <a name="recommendation-for-successful-initial-replication"></a>Aanbeveling voor een geslaagde initiële replicatie
Deze sectie wordt beschreven voor het aantal batches waarvoor de virtuele machines moeten worden beveiligd zijn en de minimale bandbreedte die is vereist voor de initiële replicatie (IR) met succes voltooid. 

![IR-batchverwerking](media/hyper-v-deployment-planner-analyze-report/ir-batching-h2a.png)

Virtuele machines moeten worden beveiligd in de volgorde van de opgegeven batch. Elke batch heeft een specifieke lijst van virtuele machines. Batch 1 virtuele machines moeten worden beveiligd voordat VMs van Batch 2. Batch 2 virtuele machines moeten worden beveiligd voordat VMs van Batch 3, enzovoort. Nadat de initiële replicatie van de Batch 1 virtuele machines is voltooid, kunt u replicatie voor Batch 2 virtuele machines kunt inschakelen. Op deze manier nadat de initiële replicatie van Batch 2 virtuele machines is voltooid, kunt u replicatie inschakelen voor Batch 3 VM's, enzovoort. 

Als de volgorde van de batch wordt niet gevolgd, voldoende bandbreedte voor de initiële replicatie mogelijk niet beschikbaar voor de virtuele machines die later worden beveiligd. Het resultaat is dat beide virtuele machines nooit voltooien initiële replicatie of enkele beveiligde virtuele machines mogelijk gaan in de resync-modus. IR batchverwerking voor het geselecteerde RPO-werkblad bevat gedetailleerde informatie over welke virtuele machines moeten worden opgenomen in elke batch.

Dit diagram toont de bandbreedtedistributie voor initiële replicatie en replicatie van verschillen voor alle batches in de opgegeven batchvolgorde. Wanneer u de eerste batch van virtuele machines beveiligt, is volledig bandbreedte beschikbaar is voor de initiële replicatie. Nadat de initiële replicatie is voltooid voor de eerste batch, is onderdeel van de bandbreedte vereist voor replicatie van verschillen. De resterende bandbreedte is beschikbaar voor de initiële replicatie van de tweede batch van virtuele machines. 

De balk voor batch 2 toont de bandbreedte die is vereist voor de replicatie van verschillen voor de virtuele machines in batch 1, evenals de bandbreedte die beschikbaar is voor de initiële replicatie voor de virtuele machines in batch 2. Op dezelfde manier ziet de balk Batch 3 u de bandbreedte die vereist zijn voor replicatie van verschillen voor vorige batches (Batch 1 en 2 virtuele machines van Batch) en de bandbreedte die beschikbaar zijn voor de initiële replicatie voor Batch 3, enzovoort. Nadat de initiële replicatie van alle batches is voltooid, geeft de laatste balk de bandbreedte die vereist zijn voor replicatie van verschillen voor de beveiligde virtuele machines.

**Waarom moet ik eerste replicaties via batchverwerking uitvoeren?**
De tijd die een initiële replicatie kost, is gebaseerd op de schijfgrootte van de virtuele machine, de gebruikte schijfruimte en de beschikbare netwerkdoorvoer. De details hiervan zijn te vinden op het werkblad IR-batchverwerking van een geselecteerde RPO.

### <a name="cost-estimation"></a>Kostenraming
De grafiek toont de weergave Samenvatting van de kosten van geschatte totale disaster recovery (DR) naar Azure van uw doelregio gekozen en de valuta die u hebt opgegeven voor het genereren van rapporten.

![Samenvatting kostenramingen](media/hyper-v-deployment-planner-analyze-report/cost-estimation-summary-h2a.png)

De samenvatting helpt u bij het begrijpen van de kosten die u nodig hebt om te betalen voor opslag, compute, netwerk en wanneer u alle compatibele virtuele machines naar Azure beveiligen met behulp van Site Recovery-licentieverlening. De kosten worden berekend voor compatibele virtuele machines en niet op de profiled virtuele machines. 
 
U kunt de kosten maandelijks of jaarlijks weergeven. Meer informatie over [ondersteunde doelregio's](./hyper-v-deployment-planner-cost-estimation.md#supported-target-regions) en [ondersteunde valuta's](./hyper-v-deployment-planner-cost-estimation.md#supported-currencies).

**Kosten per onderdelen**: de totale kosten voor Noodherstel is onderverdeeld in vier onderdelen: berekenings-, opslag-, netwerk- en Site Recovery-licentie kosten. De kosten wordt berekend op basis van het verbruik die is gemaakt tijdens de replicatie en Noodherstel inzoomen tegelijk. COMPUTE, storage (premium en standard), de ExpressRoute/VPN-verbinding die is geconfigureerd tussen de lokale site en Azure en de Site Recovery-licentie worden gebruikt voor de berekeningen.

**Kosten per statussen**: de kosten van de totale disaster recovery is ingedeeld op basis van twee verschillende statussen: replicatie en details voor Dr. 

**Replicatie kosten**: de kosten die tijdens de replicatie is ontstaan. Deze heeft de kosten voor opslag, netwerk en de Site Recovery-licentie. 

**Details voor Dr-kosten**: de kosten die tijdens de testfailovers is ontstaan. Site Recovery draait virtuele machines tijdens de testfailover. De kosten DR-herstelanalyse bevat informatie over de actieve VM's berekenings- en kosten. 

**Azure Storage-kosten per maand-jaar**: het staafdiagram ziet u de totale opslagkosten die voor premium en standard-opslag voor replicatie en details voor Dr is ontstaan. U vindt een gedetailleerde kostenanalyse per virtuele machine op het werkblad [Kostenraming](hyper-v-deployment-planner-cost-estimation.md).

### <a name="growth-factor-and-percentile-values-used"></a>Groeifactor en gebruikte percentielwaarden
Deze sectie aan de onderkant van het blad wordt de percentielwaarde gebruikt voor de prestatiemeteritems van de profiled virtuele machines (de standaardwaarde is 95e percentiel). U ziet ook de groeifactor (de standaardwaarde is 30 procent) die wordt gebruikt in alle berekeningen.

![Groeifactor en gebruikte percentielwaarden](media/hyper-v-deployment-planner-run/growth-factor-max-percentile-value.png)

## <a name="recommendations-with-available-bandwidth-as-input"></a>Aanbevelingen met beschikbare bandbreedte als invoer
![Overzicht van profilering met invoer voor de bandbreedte](media/hyper-v-deployment-planner-analyze-report/profiling-overview-bandwidth-input-h2a.png)

U hebt mogelijk een situatie waarin u weet dat u een bandbreedte van meer dan x Mbps voor replicatie van Site Recovery kan niet worden ingesteld. U kunt het hulpprogramma voor het invoeren van de beschikbare bandbreedte (met behulp van de - parameter bandbreedte tijdens het genereren van rapporten) en krijgt u de haalbare RPO in minuten. Met deze haalbare RPO-waarde, kunt u beslissen of u wilt inrichten van extra bandbreedte of u tevreden met een oplossing voor het herstel na noodgevallen met deze RPO bent.

![Haalbare RPO](media/hyper-v-deployment-planner-analyze-report/achivable-rpo-h2a.PNG)

## <a name="vm-storage-placement-recommendation"></a>VM-opslag plaatsing aanbeveling 
![VM-opslagplaatsing](media/hyper-v-deployment-planner-analyze-report/vm-storage-placement-h2a.png)

**Type schijfopslag**: het type opslagaccount (Standard Storage of Premium Storage) dat wordt gebruikt voor het repliceren van alle bijbehorende virtuele machines in de kolom **Te plaatsen VM's**.

**Voorgestelde voorvoegsel**: het voorgestelde voorvoegsel, bestaande uit drie tekens, dat kan worden gebruikt voor de naam van het opslagaccount. U kunt uw eigen voorvoegsel gebruiken, maar de suggestie van het hulpprogramma volgt de [naamgevingsregels voor partities voor opslagaccounts](https://aka.ms/storage-performance-checklist).

**Voorgestelde accountnaam**: de naam van het opslagaccount na toevoeging van het voorgestelde voorvoegsel. Vervang de naam tussen de punthaken (< en >) door uw aangepaste invoer.

**Logboekopslagaccount**: alle replicatielogboeken worden opgeslagen in een opslagaccount van het type Standard. In het geval van virtuele machines die repliceren naar een Premium Storage-account moet u een extra opslagaccount van het type Standard instellen voor het opslaan van logboeken. Eén Standard-account voor het opslaan van logboeken kan worden gebruikt door meerdere Premium-opslagaccounts voor replicatie. Virtuele machines die worden gerepliceerd naar een Standard-opslagaccount gebruiken hetzelfde account voor het opslaan van logboeken.

**Voorgestelde naam logboekaccount**: de naam van het account voor het opslaan van logboeken na toevoeging van het voorgestelde voorvoegsel. Vervang de naam tussen de punthaken (< en >) door uw aangepaste invoer.

**Plaatsing samenvatting**: een samenvatting van de belasting van het totale aantal virtuele machines in het opslagaccount tijdens replicatie en testfailover of failover. Deze samenvatting bevat de:

* Totaal aantal virtuele machines die zijn toegewezen aan de storage-account. 
* Totaal aantal lezen/schrijven IOP's over alle VM's in dit opslagaccount wordt geplaatst.
* Totaal aantal schrijven (replicatie) IOPS.
* De grootte van de totale setup over alle schijven.
* Totaal aantal schijven.

**Virtuele machines place**: een lijst met alle virtuele machines die moeten worden geplaatst op het opgegeven opslagaccount voor optimale prestaties en het gebruik.

## <a name="compatible-vms"></a>Compatibele VM's
De Excel-rapport gegenereerd door de Planner van Site Recovery-implementatie biedt alle compatibel VM details in het werkblad 'Compatibel VMs'.

![Compatibele VM's](media/hyper-v-deployment-planner-analyze-report/compatible-vms-h2a.png)

**VM-naam**: de naam van de virtuele machine, die wordt gebruikt in het bestand dat is opgegeven voor VMListFile wanneer een rapport wordt gegenereerd. Deze kolom bevat ook de schijven (VHD's) die aan de virtuele machines zijn gekoppeld. De namen zijn onder andere de namen van de Hyper-V-hosts waar de virtuele machines zijn geplaatst toen ze door het hulpprogramma werden gedetecteerd tijdens de profileringsperiode.

**VM-compatibiliteit**: de mogelijke waarden zijn **Ja** en **Ja**\*. **Ja** \* is voor gevallen waarin de virtuele machine is een geschikt is voor [Azure premium-opslag](https://aka.ms/premium-storage-workload). Hier past het geprofileerde hoge verloop of IOPS bij een grotere Premium-schijfgrootte dan bij de grootte die is toegewezen aan de schijf. Het opslagaccount besluit welk type opslagschijf premium toewijzen van een schijf, op basis van de grootte: 
* <128 GB is een P10.
* 128 GB tot 256 GB is een P15.
* 256 GB tot 512 GB is een P20.
* Er is een P30 voor 512 GB 1.024 GB.
* 1,025 GB tot 2048 GB is een p 40.
* Er is een P50 voor 2,049 GB 4095 GB.

Als de kenmerken van de workload van een schijf overeenkomen met de categorie P20 of P30, maar de schijf op grond van de grootte ervan aan een lagere categorie schijftype voor Premium Storage wordt gekoppeld, markeert het hulpprogramma die virtuele machine als **Ja**\*. Het hulpprogramma adviseert ook om ofwel de grootte van de bronschijf te wijzigen zodat deze overeenkomt met het schijftype voor Premium Storage of de post-failover van het doelschijftype te wijzigen.

**Opslagtype**: Standard of Premium.

**Voorgestelde voorvoegsel**: het voorvoegsel van drie tekens van het opslagaccount.

**Opslagaccount**: de naam die gebruikmaakt van het voorgestelde voorvoegsel voor het opslagaccount.

**Piek R/W IOP's (met groeifactor)**: de piek werkbelasting lezen/schrijven IOPS op de schijf (standaard is 95e percentiel) samen met de factor met toekomstige groei (de standaardwaarde is 30 procent). De totale lezen/schrijven IOPS van een virtuele machine is niet altijd de som van de VM afzonderlijke schijven lezen/schrijven IOPS. De piek lezen/schrijven IOP's van de virtuele machine zijn de piek van de som van de afzonderlijke schijven lezen/schrijven IOPS tijdens elke minuut van de periode voor profielservices gegeven.

**Piek gegevens verloop in MB/s (met groeifactor)**: de piek verloop tarief op de schijf (standaard is 95e percentiel) samen met de factor met toekomstige groei (de standaardwaarde is 30 procent). Het totale verloop van de virtuele machine is niet altijd de som van de VM afzonderlijke schijven gegevensverloop. Het verloop piek gegevens van de virtuele machine is de piek van de som van de afzonderlijke schijven verloop tijdens elke minuut van de periode voor profielservices gegeven.

**Azure VM-grootte**: de ideale toegewezen Azure Cloud Services-VM-grootte voor dit on-premises VM. De toewijzing is gebaseerd op de lokale van de virtuele machine geheugen, aantal kernen-schijven-netwerkinterfacekaarten en IOPS lezen/schrijven. De aanbeveling is altijd de laagste Azure VM-grootte die overeenkomt met alle kenmerken van de on-premises virtuele machine.

**Aantal schijven**: het totale aantal schijven (VHD's) op de virtuele machine.

**Grootte (in GB)**: de totale grootte van alle schijven van de virtuele machine. Het hulpprogramma toont ook de schijfgrootte voor de afzonderlijke schijven in de virtuele machine.

**Kerngeheugens**: het aantal CPU-kerngeheugens van de virtuele machine.

**Geheugen (MB)**: het RAM-geheugen van de virtuele machine.

**NIC's**: het aantal NIC's van de virtuele machine.

**Type opstarten**: het type opstarten van de virtuele machine. Dit kan BIOS of EFI zijn.

## <a name="incompatible-vms"></a>Niet-compatibele VM's
De Excel-rapport gegenereerd door de Planner implementatie van Site Recovery biedt alle niet-compatibele VM details in het werkblad 'Incompatibel VMs'.

![Niet-compatibele VM's](media/hyper-v-deployment-planner-analyze-report/incompatible-vms-h2a.png)

**VM-naam**: de naam van de virtuele machine, die wordt gebruikt in het bestand dat is opgegeven voor VMListFile wanneer een rapport wordt gegenereerd. Deze kolom bevat ook de schijven (VHD's) die aan de virtuele machines zijn gekoppeld. De namen zijn onder andere de namen van de Hyper-V-hosts waar de virtuele machines zijn geplaatst toen ze door het hulpprogramma werden gedetecteerd tijdens de profileringsperiode.

**VM-compatibiliteit**: geeft aan waarom de virtuele machine niet compatibel is voor gebruik met Site Recovery. De redenen worden voor elke niet-compatibele schijf van de virtuele machine beschreven. Op basis van gepubliceerde [opslaglimieten](https://aka.ms/azure-storage-scalbility-performance) kan dit een van de volgende redenen zijn:

* Grootte van de schijf is groter dan 4095 GB. Azure Storage momenteel biedt geen ondersteuning voor schijfgrootten gegevens groter is dan 4095 GB.

* OS-schijf is groter dan 2.047 GB voor het genereren van 1 (BIOS opstarten type) VM. Site Recovery biedt geen ondersteuning voor OS de grootte van de schijf is groter dan 2.047 GB voor virtuele machines van generatie 1.

* OS-schijf is groter dan 300 GB voor de generatie 2 (EFI boot-type) VM. Site Recovery biedt geen ondersteuning voor OS schijfgrootte groter is dan 300 GB voor virtuele machines van generatie 2.

* De naam van een VM wordt niet ondersteund met een van de volgende tekens bevatten: ' "[] '. Het hulpprogramma kan profiel gegevens niet ophalen voor virtuele machines die deze tekens in hun naam hebben. 

* Een VHD wordt gedeeld door twee of meer virtuele machines. Azure biedt geen ondersteuning voor virtuele machines met een gedeelde VHD.

* Met virtuele Fibre Channel een virtuele machine wordt niet ondersteund. Site Recovery biedt geen ondersteuning voor virtuele machines met virtuele Fibre Channel.

* Een Hyper-V-cluster bevat geen broker replicatie. Site Recovery ondersteunt een virtuele machine in Hyper-V-cluster als de Hyper-V Replica Broker is niet geconfigureerd voor het cluster.

* Er is een virtuele machine niet maximaal beschikbaar. Site Recovery biedt geen ondersteuning voor een virtuele machine van een Hyper-V-clusterknooppunt waarvan VHD's op de lokale schijf in plaats van op de clusterschijf worden opgeslagen. 

* Totaal aantal VM-grootte (replicatie + testfailover) overschrijdt de limiet van de ondersteunde premium storage-account (35 TB). Dit probleem treedt meestal op wanneer één schijf in de virtuele machine een prestatiekenmerk heeft dat groter is dan de maximaal ondersteunde limieten voor Standard-opslag van Azure of Site Recovery. De virtuele machine komt dan in aanmerking voor Premium Storage. De maximale ondersteunde grootte van een premium storage-account is echter 35 TB. Een enkele, beveiligde virtuele machine kan niet worden beveiligd via meerdere opslagaccounts. 

    Wanneer een testfailover wordt uitgevoerd op een beveiligde virtuele machine en een niet-beheerde schijf is geconfigureerd voor de testfailover, deze wordt uitgevoerd in hetzelfde opslagaccount waarin de replicatie wordt uitgevoerd. In dit exemplaar is aanvullende dezelfde hoeveelheid opslagruimte vereist als die van replicatie. Hiermee zorgt u ervoor dat de voortgang van een replicatie en een testfailover parallel kunnen plaatsvinden. Wanneer een beheerde schijf is geconfigureerd voor de testfailover, wordt er niet meer ruimte moet worden gehouden met de testfailover VM.

* Bron IOPS overschrijdt de limiet van de IOPS ondersteunde opslag van 7.500 per schijf.

* Bron IOPS overschrijdt de limiet van de IOPS ondersteunde opslag van 80.000 per VM.

* Bron VM gemiddelde gegevensverloop overschrijdt de ondersteunde limiet gegevensverloop Site Recovery van 10 MB/s voor de gemiddelde i/o-grootte.

* Bron-VM gemiddelde effectief schrijven IOPS overschrijdt de ondersteunde limiet van de Site Recovery IOPS van 840.

* De berekende opslag voor momentopnamen overschrijdt de ondersteunde opslaglimiet van 10 TB voor momentopnamen.

**Piek R/W IOP's (met groeifactor)**: de piek-werkbelasting IOPS op de schijf (standaard is 95e percentiel) samen met de factor met toekomstige groei (de standaardwaarde is 30 procent). De totale lezen/schrijven IOP's van de virtuele machine is niet altijd de som van de VM afzonderlijke schijven lezen/schrijven IOPS. De piek lezen/schrijven IOP's van de virtuele machine is de piek van de som van de afzonderlijke schijven lezen/schrijven IOPS tijdens elke minuut van de profileringsoptimalisaties periode.

**Piek gegevens verloop (MB/s) (met groeifactor)**: de piek verloop tarief op de schijf (standaard is 95e percentiel) samen met de factor met toekomstige groei (de standaardwaarde is 30 procent). Houd er rekening mee dat het verloop totale gegevens van de virtuele machine niet altijd de som van de VM afzonderlijke schijven gegevensverloop. Het verloop piek gegevens van de virtuele machine is de piek van de som van de afzonderlijke schijven verloop tijdens elke minuut van de periode voor profielservices gegeven.

**Aantal schijven**: het totale aantal VHD's van de virtuele machine.

**Grootte (in GB)**: de setup totale grootte van alle schijven van de virtuele machine. Het hulpprogramma toont ook de schijfgrootte voor de afzonderlijke schijven in de virtuele machine.

**Kerngeheugens**: het aantal CPU-kerngeheugens van de virtuele machine.

**Geheugen (MB)**: de hoeveelheid RAM-geheugen van de virtuele machine.

**NIC's**: het aantal NIC's van de virtuele machine.

**Type opstarten**: het type opstarten van de virtuele machine. Dit kan BIOS of EFI zijn.

## <a name="azure-site-recovery-limits"></a>Azure Site Recovery-limieten
De volgende tabel bevat de limieten van de Site Recovery. Deze ondersteuningslimieten zijn gebaseerd op tests, maar ze niet alle mogelijke toepassing i/o-combinaties dekken. De werkelijke resultaten kunnen variëren op basis van uw toepassings-I/O-combinatie. Uitvoeren voor de beste resultaten, zelfs na de implementatie plannen, uitgebreide toepassing testen door uitgifte van een testfailover ophalen van de afbeelding true prestaties van de toepassing.
 
**Beoogde replicatieopslag** | **VM gemiddelde i/o-Brongrootte** |**Bron VM gemiddelde gegevensverloop** | **Totale gegevensverloop van bron-VM per dag**
---|---|---|---
Standard Storage | 8 kB | 2 MB/s per VM | 168 GB per VM
Premium Storage | 8 kB  | 5 MB/s per VM | 421 GB per VM
Premium Storage | 16 kB of hoger| 10 MB/s per VM | 842 GB per VM

Deze limieten zijn gemiddelden uitgaande van een I/O-overlapping van 30%. Site Recovery kan een hogere doorvoer verwerken op basis van overlappingsverhouding, grotere schrijfgrootten en daadwerkelijk workload-I/O-gedrag. De bovenstaande waarden zijn gebaseerd op een typische backlog van ongeveer vijf minuten. Nadat de gegevens worden geüpload, dat wil zeggen, deze wordt verwerkt en een herstelpunt wordt gemaakt binnen vijf minuten.

## <a name="on-premises-storage-requirement"></a>On-premises opslagvereiste

Het werkblad toont de totale vrije ruimte opslagvereiste voor elk volume van de Hyper-V-servers (waar de VHD's zich bevinden) die nodig is voor een geslaagde initiële replicatie en voor replicatie van verschillen. Voordat u replicatie inschakelt, kunt u de vereiste opslagruimte toevoegen op de volumes om ervoor te zorgen dat de replicatie niet ongewenste uitvaltijd van uw productietoepassingen. 

  Site Recovery implementatie Planner identificeert de optimale opslag schijfruimte vrij te maken op basis van de VHD-grootte en de netwerkbandbreedte die wordt gebruikt voor replicatie.

![On-premises opslagvereiste](media/hyper-v-deployment-planner-analyze-report/on-premises-storage-requirement-h2a.png)

### <a name="why-do-i-need-free-space-on-the-hyper-v-server-for-the-replication"></a>Waarom heb ik op de Hyper-V-server vrije ruimte nodig voor de replicatie?
* Wanneer u de replicatie van een virtuele machine inschakelt, maakt Site Recovery een momentopname van elke VHD van de virtuele machine voor de initiële replicatie. Tijdens de initiële replicatie worden nieuwe wijzigingen door de toepassing maar de schijven geschreven. Site Recovery houdt deze wijzigingen in de logboekbestanden, waarvoor extra opslagruimte vereist. Totdat de initiële replicatie is voltooid, wordt de logboekbestanden worden lokaal opgeslagen. 

    Als er niet voldoende ruimte beschikbaar is voor de logboekbestanden en de momentopname (AVHDX), replicatie probeert het opnieuw synchroniseren modus en replicatie nooit is voltooid. In het ergste geval moet u extra vrije ruimte van 100 procent van de VHD-grootte voor de initiële replicatie.
* Nadat de initiële replicatie is voltooid, begint de replicatie van verschillen. Site Recovery houdt deze wijzigingen in de logboekbestanden die zijn opgeslagen op het volume waar de VHD's van de virtuele machine zich bevinden. Deze logboekbestanden worden gerepliceerd naar Azure met een frequentie geconfigureerde kopiëren. Op basis van de beschikbare netwerkbandbreedte kan het even duren voordat de logboekbestanden naar Azure zijn gerepliceerd. 

    Als er niet voldoende vrije ruimte beschikbaar is voor het opslaan van de logboekbestanden, is replicatie onderbroken. De replicatiestatus van de virtuele machine wordt vervolgens in "hersynchronisatie is vereist."
* Als netwerkbandbreedte niet voldoende is voor het pushen van de logboekbestanden in Azure, ophalen van de logboekbestanden piled op het volume. In een slechtste scenario wanneer u de logboekbestanden wordt vergroot tot 50 procent van de VHD-grootte, probeert de replicatie van de virtuele machine het "hersynchronisatie is vereist." In het ergste geval moet u extra vrije ruimte van 50 procent van de VHD-grootte voor replicatie van verschillen.

**Hyper-V-host**: de lijst met geprofileerde Hyper-V-servers. Als een server deel uitmaakt van een Hyper-V-cluster, worden alle clusterknooppunten gegroepeerd.

**Volume (VHD-pad)**: elk volume van een Hyper-V-host waar VHD's/VHDX-en aanwezig zijn. 

**Vrije beschikbare (GB)**: de hoeveelheid vrije ruimte op het volume.

**Totale opslagruimte is om op het volume (GB)**: de totale vrije opslagruimte op het volume voor geslaagde initiële replicatie en replicatie van verschillen is vereist. 

**Extra opslagruimte worden ingericht op het volume voor geslaagde replicatie (GB) totaal**: het raadt aan om de totale extra ruimte die moet worden ingericht op het volume voor de geslaagde initiële replicatie en replicatie van verschillen.

## <a name="initial-replication-batching"></a>Initiële replicatie via batchverwerking 

### <a name="why-do-i-need-initial-replication-batching"></a>Waarom ik moet eerste replicatie batchverwerking?
Als de virtuele machines op hetzelfde moment worden beschermd, wordt de vereiste vrije opslagruimte veel hoger is. Als er niet voldoende opslagruimte beschikbaar is, wordt de replicatie van de virtuele machines verplaatst naar de hersynchronisatie-modus. De vereiste netwerkbandbreedte is ook een veel hoger initiële replicatie van alle VM's samen met succes te voltooien. 

### <a name="initial-replication-batching-for-a-selected-rpo"></a>Initiële replicatie via batchverwerking voor een geselecteerde RPO
Dit werkblad biedt de detailweergave van elke batch voor IR Voor elke RPO wordt een afzonderlijk werkblad IR-batchverwerking gemaakt. 

Nadat u de aanbeveling lokale opslag vereist voor elk volume hebt gevolgd, is de belangrijkste informatie die u nodig hebt voor replicatie van de lijst met virtuele machines die parallel kunnen worden beveiligd. Deze VM's zijn gegroepeerd in een batch en er kunnen meerdere batches. Beveiligen van de virtuele machines in de volgorde van de opgegeven batch. Eerst Batch 1 VM's beveiligen. Nadat de initiële replicatie is voltooid, Batch 2 VM's beveiligen, enzovoort. De lijst met batches en bijbehorende virtuele machines kunt u via dit blad verkrijgen. 

![IR batchverwerking details](media/hyper-v-deployment-planner-analyze-report/ir-batching-for-rpo-h2a.png)

![Aanvullende IR batchverwerking details](media/hyper-v-deployment-planner-analyze-report/ir-batching-for-rpo2-h2a.png)

### <a name="each-batch-provides-the-following-information"></a>Elke batch biedt de volgende informatie 
**Hyper-V-host**: de Hyper-V-host van de virtuele machine die moet worden beveiligd.

**Virtuele Machine**: de VM moet worden beveiligd. 

**Opmerkingen**: als voor een bepaald volume van een virtuele machine een actie is vereist, wordt hier een opmerking weergegeven. Bijvoorbeeld, als er voldoende vrije ruimte is niet beschikbaar op een volume wordt vermeld de opmerking, "toevoegen extra opslagruimte ter bescherming van deze virtuele machine."

**Volume (VHD-pad)**: de naam van het volume waar de VM VHD's zich bevinden. 

**Vrije ruimte beschikbaar op het volume (GB)**: de vrije schijfruimte beschikbaar op het volume voor de virtuele machine. Bij het berekenen van de beschikbare vrije ruimte op de volumes, wordt rekening gehouden met de schijfruimte die wordt gebruikt voor de replicatie van verschillen voor de virtuele machines in de vorige batches waarvan de VHD's zich op hetzelfde volume bevinden. 

Bijvoorbeeld: VM1 VM2 en VM3 bevinden zich op een volume, zeggen E:\VHDpath. Vóór de replicatie is er 500 GB aan vrije ruimte op het volume. VM1 maakt deel uit van de Batch-1 en VM3 maakt deel uit van Batch3 VM2 maakt deel uit van de Batch-2. Voor VM1 is er 500 G aan vrije ruimte beschikbaar. Voor VM2 is de hoeveelheid vrije ruimte 500 – schijfruimte vereist voor replicatie van verschillen voor VM1. Als VM1 300 GB ruimte nodig hebben voor replicatie van verschillen, is de hoeveelheid vrije ruimte voor VM2 van 500 GB – 300 GB = 200 GB. Voor een replicatie van verschillen van VM2 zou er dus 300 GB nodig zijn. De vrije ruimte beschikbaar voor VM3 is 200 GB-300 GB =-100 GB.

**Opslagruimte vereist op het volume (GB) voor een initiële replicatie**: de vrije opslagruimte die op het volume is vereist om een initiële replicatie voor de virtuele machine te kunnen uitvoeren.

**Opslag die is vereist op het volume voor de deltareplicatie (GB)**: de vrije opslagruimte op het volume is vereist voor de virtuele machine voor replicatie van verschillen.

**Extra opslag vereist op basis van een tekort dat een replicatiefout zou veroorzaken (GB)**: de extra vereiste opslagruimte op het volume voor de virtuele machine. De maximale waarde van de initiële replicatie en delta-replicatie opslagvereiste schijfruimte minus de hoeveelheid vrije ruimte op het volume is.

**Minimale bandbreedte vereist voor initiële replicatie (Mbps)**: de minimale bandbreedte die is vereist voor de initiële replicatie voor de virtuele machine.

**Minimale bandbreedte die vereist zijn voor replicatie van verschillen (Mbps)**: de minimale bandbreedte die vereist zijn voor replicatie van verschillen voor de virtuele machine.

### <a name="network-utilization-details-for-each-batch"></a>Gegevens over het netwerkgebruik voor elke batch 
Elke tabel bevat een samenvatting van het netwerkgebruik van de batch.

**Bandbreedte die beschikbaar zijn voor batch**: de bandbreedte die beschikbaar zijn voor de batch met inbegrip van de vorige batch delta replicatie bandbreedte.

**Geschatte bandbreedte beschikbaar voor initiële replicatie van batch**: de bandbreedte die beschikbaar is voor een initiële replicatie van de virtuele machines van de batch. 

**Verbruikte bandbreedte initiële replicatie van verschillen van batch**: de bandbreedte die beschikbaar is voor een replicatie van verschillen van de virtuele machines van de batch. 

**Geschatte tijd van de initiële replicatie voor batch (uu: mm)**: de initiële replicatie geschatte tijd in uren: minuten.



## <a name="next-steps"></a>Volgende stappen
Meer informatie over [kostenraming](hyper-v-deployment-planner-cost-estimation.md).
