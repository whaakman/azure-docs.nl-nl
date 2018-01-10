---
title: Azure Site Recovery-implementatieplanner voor Hyper-V naar Azure | Microsoft Docs
description: In dit artikel wordt een scenario beschreven waarin een rapport dat door de Azure Site Recovery-implementatieplanner voor Hyper-V naar Azure is gegenereerd, wordt geanalyseerd.
services: site-recovery
documentationcenter: 
author: nsoneji
manager: garavd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 12/02/2017
ms.author: nisoneji
ms.openlocfilehash: 9340fe48c1da874d6c0cf02c026e5dec6ddabbe7
ms.sourcegitcommit: 357afe80eae48e14dffdd51224c863c898303449
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/15/2017
---
# <a name="analyze-the-azure-site-recovery-deployment-planner-report"></a>Het rapport van de Azure Site Recovery-implementatieplanner analyseren
Het gegenereerde Microsoft Excel-rapport bevat de volgende werkbladen:

## <a name="on-premises-summary"></a>Samenvatting on-premises
Het werkblad Samenvatting on-premises biedt een overzicht van de geprofileerde Hyper-V-omgeving ![Samenvatting on-premises](media/site-recovery-hyper-v-deployment-planner-analyze-report/on-premises-summary-h2a.png)

**Begindatum** en **Einddatum** zijn de begin- en einddatums van de profileringsgegevens die in aanmerking komen voor het genereren van rapporten. De begindatum is standaard de datum waarop de profilering start en de einddatum de datum waarop de profilering stopt. Dit kunnen de waarden voor StartDate en EndDate zijn als het rapport met deze parameters wordt gegenereerd.

**Totale aantal dagen van profilering**: het totale aantal dagen van profilering tussen de begin- en einddatum waarvoor het rapport wordt gegenereerd.

**Aantal compatibele virtuele machine**: het totale aantal compatibele virtuele machines waarvoor de vereiste netwerkbandbreedte, het vereiste aantal opslagaccounts en Microsoft Azure-kerngeheugens wordt berekend.

**Totale aantal schijven voor alle compatibele virtuele machines**: het totale aantal schijven voor alle compatibele virtuele machines.

**Gemiddeld aantal schijven per compatibele virtuele machine**: het gemiddelde aantal schijven dat voor alle compatibele virtuele machines is berekend.

**Gemiddelde schijfgrootte (GB)**: de gemiddelde schijfgrootte die voor alle compatibele virtuele machines is berekend.

**Gewenste RPO (minuten)**: de standaard-RPO-waarde of de waarde die tijdens het genereren van het rapport voor de parameter DesiredRPO wordt doorgegeven om de vereiste bandbreedte in te schatten.

**Gewenste bandbreedte (Mbps)**: de waarde die u tijdens het genereren van het rapport voor de parameter Bandwidth hebt doorgegeven om de haalbare RPO in te schatten.

**Waargenomen typisch gegevensverloop per dag (GB)**: het gemiddelde gegevensverloop dat voor alle profileringsdagen is vastgesteld.

## <a name="recommendations"></a>Aanbevelingen  
Het werkblad met de aanbevelingen voor het Hyper-V naar Azure-rapport heeft de volgende gegevens aan de hand van de geselecteerde gewenste RPO:

![Rapport Aanbevelingen voor Hyper-V naar Azure](media/site-recovery-hyper-v-deployment-planner-analyze-report/Recommendations-h2a.png)

### <a name="profile-data"></a>Profielgegevens
![Profielgegevens](media/site-recovery-hyper-v-deployment-planner-analyze-report/profile-data-h2a.png)

**Periode geprofileerde gegevens**: de periode waarin de profilering is uitgevoerd. Het hulpprogramma neemt standaard alle geprofileerde gegevens in de berekening op. Als u voor het genereren van het rapport de optie StartDate en EndDate hebt gebruikt, wordt het rapport gegenereerd voor die specifieke periode. 

**Aantal geprofileerde Hyper-V-servers**: het aantal Hyper-V-servers waarvoor een VM-rapport wordt gemaakt. Klik op het aantal om de naam van de Hyper-V-servers weer te geven. Hiermee opent u het werkblad On-premises opslagvereiste waar alle servers worden weergegeven samen met hun opslagvereisten.    

**Gewenste RPO**: het gewenste beoogde herstelpunt (RPO) voor uw implementatie. Standaard wordt de vereiste netwerkbandbreedte berekend voor de RPO-waarden van 15, 30 en 60 minuten. Op basis van de selectie worden de betrokken waarden bijgewerkt op het blad. Als u tijdens het genereren van het rapport de parameter DesiredRPOinMin hebt gebruikt, wordt de betreffende waarde weergegeven bij het gewenste RPO-resultaat.

### <a name="profiling-overview"></a>Overzicht van profilering
![Overzicht van profilering](media/site-recovery-hyper-v-deployment-planner-analyze-report/profiling-overview-h2a.png)

**Totale aantal geprofileerde virtuele machines**: het totale aantal virtuele machines waarvan de geprofileerde gegevens beschikbaar zijn. Als het bestand dat is opgegeven bij VMListFile namen van virtuele machines bevat die niet zijn geprofileerd, worden deze virtuele machines niet meegenomen in het rapport en uitgesloten van het totale aantal geprofileerde virtuele machines.

**Compatibele virtuele machines**: het aantal virtuele machines dat op Azure kan worden beveiligd met Azure Site Recovery. Dit is het totale aantal compatibele virtuele machines waarvoor de vereiste netwerkbandbreedte, het aantal opslagaccounts en het aantal Azure-kerngeheugens wordt berekend. De details van elke compatibele virtuele machine zijn beschikbaar in de sectie 'Compatibele VM's'.

**Niet-compatibele virtuele machines**: het aantal geprofileerde virtuele machines dat niet compatibel is met beveiliging met Azure Site Recovery. De redenen voor incompatibiliteit worden vermeld in de sectie 'Niet-compatibele VM's'. Als het bestand dat is opgegeven bij VMListFile namen bevat van virtuele machines die niet zijn geprofileerd, worden die virtuele machines uitgesloten van het aantal niet-compatibele virtuele machines. Deze virtuele machines worden weergegeven als 'Gegevens niet gevonden' aan het einde van de sectie 'Niet-compatibele VM's'.

**Gewenste RPO**: het gewenste beoogde herstelpunt (Recovery Point Objective [RPO]) in minuten. Het rapport wordt gegenereerd voor drie RPO-waarden: 15 (standaard), 30 en 60 minuten. De aanbeveling voor bandbreedte in het rapport wordt gewijzigd op basis van uw selectie in de vervolgkeuzelijst Gewenste RPO in de rechterbovenhoek van het werkblad. Als u het rapport hebt gegenereerd met een aangepaste waarde voor de parameter -DesiredRPO, wordt deze aangepaste waarde als standaardwaarde weergegeven in de vervolgkeuzelijst Gewenste RPO.

### <a name="required-network-bandwidth-mbps"></a>Vereiste netwerkbandbreedte (Mbps)
![Vereiste netwerkbandbreedte](media/site-recovery-hyper-v-deployment-planner-analyze-report/required-network-bandwidth-h2a.png)

**Altijd aan de RPO voldoen:** de aanbevolen bandbreedte in Mbps die moet worden toegewezen om 100 procent van de tijd uw gewenste RPO te halen. Deze bandbreedte moet worden toegewezen voor onveranderlijke replicatie van verschillen bij al uw compatibele virtuele machines om eventuele RPO-schendingen te voorkomen.

**90% van de tijd aan de RPO voldoen**: als u vanwege de kosten voor een breedbandverbinding of om een andere reden niet de benodigde bandbreedte kunt toepassen om altijd aan de RPO te voldoen, kunt u een lager bedrag voor bandbreedte kiezen waarmee u 90% van de tijd aan de RPO kunt voldoen. Voor inzicht in de gevolgen van het instellen van deze lagere bandbreedte bevat het rapport een wat-als-analyse van het aantal en de duur van de RPO-schendingen die u kunt verwachten.

**Behaalde doorvoer:** de doorvoer van de server waar u de GetThroughput-opdracht hebt uitgevoerd naar de Azure-regio waar het opslagaccount zich bevindt. Deze waarde geeft het geschatte niveau aan dat kan worden behaald als u de compatibele virtuele machines beveiligt met Azure Site Recovery, op voorwaarde dat de kenmerken voor opslag en netwerk van uw Hyper-V server hetzelfde blijven als die van de server waarop u het hulpprogramma hebt uitgevoerd.

Voor alle implementaties van Azure Site Recovery wordt het gebruik van [ExpressRoute](https://aka.ms/expressroute) aanbevolen.

### <a name="required-storage-accounts"></a>Vereiste opslagaccounts
De volgende grafiek geeft het totale aantal opslagaccounts aan (Standard Storage en Premium Storage) dat is vereist voor het beveiligen van alle compatibele virtuele machines. Zie de sectie 'VM-opslagplaatsing' voor meer informatie over welk opslagaccount u moet gebruiken voor elke virtuele machine.

![Vereiste opslagaccounts](media/site-recovery-hyper-v-deployment-planner-analyze-report/required-storage-accounts-h2a.png)

### <a name="required-number-of-azure-cores"></a>Vereiste aantal Azure-kerngeheugens
Dit is het totale aantal kerngeheugens dat moet worden ingesteld vóór failover of testfailover van de compatibele virtuele machines. Als er in het abonnement onvoldoende kerngeheugens beschikbaar zijn, kan Azure Site Recovery geen virtuele machines maken op het moment van een failover of testfailover.
![Vereiste aantal Azure-kerngeheugens](media/site-recovery-hyper-v-deployment-planner-analyze-report/required-number-of-azure-cores-h2a.png)


### <a name="additional-on-premises-storage-requirement"></a>Extra on-premises opslagvereiste

De totale vrije opslagruimte op Hyper-V-servers die is vereist om de initiële replicatie en replicatie van verschillen uit te kunnen voeren, zonder dat het repliceren van virtuele machines tot ongewenste downtime voor uw productietoepassingen leidt. In [On-premises opslagvereisten](#on-premises-storage-requirement) vindt u details over elk van de volumevereisten. 

Zie [On-premises storage requirement](#why-do-i-need-free-space-on-the-hyper-v-server-for-the-replication) als u wilt weten waarom vrije ruimte is vereist voor de replicatie.

![on-premises opslagvereiste en kopieerfrequentie](media/site-recovery-hyper-v-deployment-planner-analyze-report/on-premises-storage-and-copy-frequency-h2a.png)

### <a name="maximum-copy-frequency"></a>Maximale kopieerfrequentie
De aanbevolen maximale kopieerfrequentie moet voor de replicatie worden ingesteld om de gewenste RPO te bereiken. Standaard is dit 5 minuten. U kunt een kopieerfrequentie van 30 seconden instellen voor een betere RPO.

### <a name="what-if-analysis"></a>Wat als-analyse
![Wat als-analyse](media/site-recovery-hyper-v-deployment-planner-analyze-report/what-if-analysis-h2a.png) Deze analyse beschrijft hoeveel schendingen zich kunnen voordoen tijdens de profileringsperiode wanneer u een lagere bandbreedte instelt om 90% van de tijd aan de RPO te voldoen. Op elke dag kunnen er een of meer RPO-schendingen optreden. De grafiek toont de piek-RPO van de dag. Op basis van deze analyse kunt u besluiten of het aantal RPO-schendingen voor alle dagen en de hoogste RPO per dag acceptabel zijn in combinatie met de opgegeven lagere bandbreedte. Als u dat vindt, kunt u de lagere bandbreedte toewijzen voor replicatie. Zo niet, dan stelt u de hogere bandbreedte in zoals voorgesteld om altijd aan de RPO te voldoen. 

### <a name="recommendation-for-successful-initial-replication"></a>Aanbeveling voor een geslaagde initiële replicatie
Deze sectie bevat een aanbeveling voor het aantal batches met de virtuele machines die moeten worden beveiligd, evenals de minimale bandbreedte die is vereist voor een geslaagde initiële replicatie (IR). 

![IR-batchverwerking](media/site-recovery-hyper-v-deployment-planner-analyze-report/ir-batching-h2a.png)

De virtuele machine moet worden beveiligd in de opgegeven batchvolgorde. Elke batch heeft een specifieke lijst met virtuele machines. Virtuele machines in batch 1 moeten worden beveiligd vóór die in batch 2. Virtuele machines in batch 2 moeten worden beveiligd vóór die in batch 3 enzovoort. Nadat de initiële replicatie van de virtuele machines in batch 1 is voltooid, kunt u replicatie voor de virtuele machines in batch 2 inschakelen. En nadat de initiële replicatie van de virtuele machines in batch 2 is voltooid, kunt u replicatie voor de virtuele machines in batch 3 inschakelen enzovoort. Als de volgorde van de batch niet wordt gevolgd, is er mogelijk onvoldoende bandbreedte voor de initiële replicatie voor de virtuele machines beschikbaar die later worden beveiligd. Dit heeft tot gevolg dat de initiële replicatie voor virtuele machines mogelijk nooit kan worden voltooid, of dat een klein aantal beveiligde virtuele machines in de hersynchronisatiemodus worden geplaatst. IR batchverwerking voor het geselecteerde RPO-werkblad bevat gedetailleerde informatie over welke virtuele machines moeten worden opgenomen in elke batch.

Dit diagram toont de bandbreedtedistributie voor initiële replicatie en replicatie van verschillen voor alle batches in de opgegeven batchvolgorde. Wanneer u virtuele machines van de eerste batch beveiligt, is volledige bandbreedte beschikbaar voor de initiële replicatie. Zodra de initiële replicatie is uitgevoerd voor de eerste batch, is een deel van de bandbreedte voor de replicatie van verschillen vereist. De resterende bandbreedte is beschikbaar voor de initiële replicatie van virtuele machines in de tweede batch. De balk voor batch 2 toont de bandbreedte die is vereist voor de replicatie van verschillen voor de virtuele machines in batch 1, evenals de bandbreedte die beschikbaar is voor de initiële replicatie voor de virtuele machines in batch 2. Hetzelfde geldt voor de balk voor batch 3 met de bandbreedte die is vereist voor de replicatie van verschillen voor eerdere batches (virtuele machines in batch 1 en 2) en de bandbreedte die beschikbaar is voor de initiële replicatie voor batch 3 enzovoort.  Zodra de initiële replicatie van alle batches is uitgevoerd, toont de laatste balk de bandbreedte die vereist is voor de replicatie van verschillen voor alle beveiligde virtuele machines. 

**Waarom moet ik eerste replicaties via batchverwerking uitvoeren?**
De tijd die een initiële replicatie kost, is gebaseerd op de schijfgrootte van de virtuele machine, de gebruikte schijfruimte en de beschikbare netwerkdoorvoer. De details hiervan zijn te vinden op het werkblad IR-batchverwerking van een geselecteerde RPO.

### <a name="cost-estimation"></a>Kostenraming
De grafiek toont de weergave Samenvatting van de geschatte totale kosten voor noodherstel (DR) voor Azure van de gekozen doelregio en de valuta die u hebt opgegeven voor het genereren van rapporten.
![Samenvatting kostenramingen](media/site-recovery-hyper-v-deployment-planner-analyze-report/cost-estimation-summary-h2a.png) De samenvatting helpt u inzicht te krijgen in de kosten die u betaalt voor opslag, rekensnelheid, het netwerk en de licentie wanneer u alle compatibele virtuele machines voor Azure beveiligt met Azure Site Recovery. De kosten worden berekend op basis van compatibele virtuele machines en niet op basis van alle geprofileerde virtuele machines.  
 
U kunt de kosten maandelijks of jaarlijks weergeven. Meer informatie over [ondersteunde doelregio's](./site-recovery-hyper-v-deployment-planner-cost-estimation.md#supported-target-regions) en [ondersteunde valuta's](./site-recovery-hyper-v-deployment-planner-cost-estimation.md#supported-currencies).

**Kosten per onderdelen** De totale kosten voor Noodherstel zijn onderverdeeld in vier onderdelen: Compute, Storage, netwerk en Azure Site Recovery-licentiekosten. De kosten worden berekend op basis van het verbruik dat zal plaatsvinden tijdens de replicatie, en bij Noodherstel de tijd voor de rekensnelheid, opslag (premium en standard), de ExpressRoute/VPN die is geconfigureerd tussen de lokale site en Azure, en Azure Site Recovery-licentie.

**Kosten per statussen** De kosten voor een totaal noodherstel (DR) zijn is gebaseerd op twee verschillende statussen: Replicatie en Noodherstel. 

**Replicatiekosten**: de kosten die zullen worden gemaakt tijdens de replicatie. Dit dekt de kosten van opslag, netwerk en de Azure Site Recovery-licentie. 

**Kosten voor DR-oefeningen**: de kosten die zullen worden gemaakt tijdens de testfailovers. Azure Site Recovery laat virtuele machines draaien tijdens de testfailover. De details voor DR-kosten zijn de kosten voor de berekenings- en opslagkosten van de actieve virtuele machines. 

**Azure-opslagkosten per maand-jaar** Dit toont de totale opslagkosten die zullen worden gemaakt voor premium en standard-opslag voor replicatie en details voor DR.
U vindt een gedetailleerde kostenanalyse per virtuele machine op het werkblad [Kostenraming](site-recovery-hyper-v-deployment-planner-cost-estimation.md).

### <a name="growth-factor-and-percentile-values-used"></a>Groeifactor en gebruikte percentielwaarden
Deze sectie onder aan het werkblad toont de percentielwaarde die voor alle prestatiemeteritems van de geprofileerde virtuele machines wordt gebruikt (standaard is dit het 95e percentiel), plus de groeifactor in procenten die in alle berekeningen wordt gebruikt (standaard 30%).

![Groeifactor en gebruikte percentielwaarden](media/site-recovery-hyper-v-deployment-planner-run/growth-factor-max-percentile-value.png)

## <a name="recommendations-with-available-bandwidth-as-input"></a>Aanbevelingen met beschikbare bandbreedte als invoer
![Overzicht van profilering met invoer voor de bandbreedte](media/site-recovery-hyper-v-deployment-planner-analyze-report/profiling-overview-bandwidth-input-h2a.png)

Het is mogelijk dat u om wat voor reden dan ook niet meer dan x Mbps bandbreedte kunt instellen voor Azure Site Recovery-replicatie. Met het hulpprogramma kunt u de beschikbare bandbreedte opgeven (dit doet u tijdens het genereren van het rapport met behulp van de parameter -Bandwidth) en zo de haalbare RPO in minuten achterhalen. Aan de hand van deze haalbare RPO-waarde kunt u beslissen of u extra bandbreedte wilt inrichten of bereid bent u te beperken tot een noodhersteloplossing voor deze RPO.

![Haalbare RPO](media/site-recovery-hyper-v-deployment-planner-analyze-report/achivable-rpo-h2a.PNG)

## <a name="vm-storage-placement-recommendation"></a>Aanbeveling voor VM-opslagplaatsing 

![VM-opslagplaatsing](media/site-recovery-hyper-v-deployment-planner-analyze-report/vm-storage-placement-h2a.png)

**Type schijfopslag**: het type opslagaccount (Standard Storage of Premium Storage) dat wordt gebruikt voor het repliceren van alle bijbehorende virtuele machines in de kolom **Te plaatsen VM's**.

**Voorgestelde voorvoegsel**: het voorgestelde voorvoegsel, bestaande uit drie tekens, dat kan worden gebruikt voor de naam van het opslagaccount. U kunt uw eigen voorvoegsel gebruiken, maar de suggestie van het hulpprogramma volgt de [naamgevingsregels voor partities voor opslagaccounts](https://aka.ms/storage-performance-checklist).

**Voorgestelde accountnaam**: de naam van het opslagaccount na toevoeging van het voorgestelde voorvoegsel. Vervang de naam tussen de punthaken (< en >) door uw aangepaste invoer.

**Logboekopslagaccount**: alle replicatielogboeken worden opgeslagen in een opslagaccount van het type Standard. In het geval van virtuele machines die repliceren naar een Premium Storage-account moet u een extra opslagaccount van het type Standard instellen voor het opslaan van logboeken. Eén Standard-account voor het opslaan van logboeken kan worden gebruikt door meerdere Premium-opslagaccounts voor replicatie. Virtuele machines die worden gerepliceerd naar een Standard-opslagaccount gebruiken hetzelfde account voor het opslaan van logboeken.

**Voorgestelde naam logboekaccount**: de naam van het account voor het opslaan van logboeken na toevoeging van het voorgestelde voorvoegsel. Vervang de naam tussen de punthaken (< en >) door uw aangepaste invoer.

**Plaatsing samenvatting**: een samenvatting van de belasting van het totale aantal virtuele machines in het opslagaccount tijdens replicatie en testfailover of failover. Hier ziet u het totale aantal virtuele machines dat is toegewezen aan het opslagaccount, de totale lezen/schrijven-IOPS voor alle virtuele machines die in dit opslagaccount worden geplaatst, totale schrijven-IOPS (replicatie), totale ingestelde grootte voor alle schijven en het totale aantal schijven.

**Te plaatsen virtuele machines**: een lijst van alle virtuele machines die in het opgegeven opslagaccount moeten worden geplaatst voor optimale prestaties en optimaal gebruik.

## <a name="compatible-vms"></a>Compatibele VM's
Het Microsoft Excel-rapport dat is gegenereerd door de Azure Site Recovery-implementatieplanner bevat alle gegevens over de compatibele virtuele machines op het werkblad 'Compatibele VMs'.

![Compatibele VM's](media/site-recovery-hyper-v-deployment-planner-analyze-report/compatible-vms-h2a.png)

**VM-naam**: de naam van de virtuele machine, die wordt gebruikt in het bestand dat is opgegeven voor VMListFile wanneer een rapport wordt gegenereerd. Deze kolom bevat ook de schijven (VHD's) die aan de virtuele machines zijn gekoppeld. De namen zijn onder andere de namen van de Hyper-V-hosts waar de virtuele machines zijn geplaatst toen ze door het hulpprogramma werden gedetecteerd tijdens de profileringsperiode.

**VM-compatibiliteit**: de mogelijke waarden zijn **Ja** en **Ja**\*. **Ja**\* verwijst naar gevallen waarin de virtuele machine in aanmerking komt voor [Azure Premium Storage](https://aka.ms/premium-storage-workload). Hier past het geprofileerde hoge verloop of IOPS bij een grotere Premium-schijfgrootte dan bij de grootte die is toegewezen aan de schijf. Het opslagaccount bepaalt aan welk schijftype voor Premium Storage een schijf wordt toegewezen, op basis van de grootte. 
* <128 GB is een P10.
* 128 GB tot 256 GB is een P15
* 256 GB tot 512 GB is een P20.
* 512 GB tot 1024 GB is een P30.
* 1025 GB tot 2048 GB is een P40.
* 2049 GB tot 4095 GB is een P50.

Als de kenmerken van de workload van een schijf overeenkomen met de categorie P20 of P30, maar de schijf op grond van de grootte ervan aan een lagere categorie schijftype voor Premium Storage wordt gekoppeld, markeert het hulpprogramma die virtuele machine als **Ja**\*. Het hulpprogramma adviseert ook om ofwel de grootte van de bronschijf te wijzigen zodat deze overeenkomt met het schijftype voor Premium Storage of de post-failover van het doelschijftype te wijzigen.

**Opslagtype**: Standard of Premium.

**Voorgestelde voorvoegsel**: het voorvoegsel van drie tekens van het opslagaccount.

**Opslagaccount**: de naam die gebruikmaakt van het voorgestelde voorvoegsel voor het opslagaccount.

**Piek voor R/W IOPS (met groeifactor)**: de piekworkload-IOPS voor lezen/schrijven op de schijf (standaard het 95e percentiel) met inbegrip van de toekomstige groeifactor (standaard 30%). De totale IOPS voor lezen/schrijven van een virtuele machine is niet altijd de som van de IOPS voor lezen/schrijven van de afzonderlijke schijven van de virtuele machine. De piek-IOPS voor lezen/schrijven van de virtuele machine is namelijk de piek van de som van de IOPS voor lezen/schrijven van de afzonderlijke schijven voor elke minuut van de profileringsperiode.

**Piek voor gegevensverloop in Mb/s (met groeifactor)**: het piekverloop op de schijf (standaard het 95e percentiel) met inbegrip van de toekomstige groeifactor (standaard 30%). Houd er rekening mee dat de totale gegevensverloopwaarde van de virtuele machine niet altijd de som is van het gegevensverloop van de afzonderlijke schijven van de virtuele machine. De piekwaarde voor het gegevensverloop van de virtuele machine is namelijk de piek van de som van het gegevensverloop van de individuele schijven voor elke minuut van de profileringsperiode.

**Azure VM-grootte**: de ideale toegewezen grootte voor virtuele machines van Azure Cloud Services voor deze on-premises virtuele machine. De toewijzing is gebaseerd op het geheugen, het aantal schijven/kerngeheugens/NIC’s en de IOPS voor lezen/schrijven van de on-premises virtuele machine. De aanbeveling is altijd de laagste Azure VM-grootte die overeenkomt met alle kenmerken van de on-premises virtuele machine.

**Aantal schijven**: het totale aantal schijven (VHD's) op de virtuele machine.

**Schijfgrootte (GB)**: de totale grootte van alle schijven op de virtuele machine. Het hulpprogramma toont ook de schijfgrootte voor de afzonderlijke schijven in de virtuele machine.

**Kerngeheugens**: het aantal CPU-kerngeheugens van de virtuele machine.

**Geheugen (MB)**: het RAM-geheugen van de virtuele machine.

**NIC's**: het aantal NIC's van de virtuele machine.

**Opstarttype**: het opstarttype van de virtuele machine. Dit kan BIOS of EFI zijn.

## <a name="incompatible-vms"></a>Niet-compatibele VM's
Het Microsoft Excel-rapport dat is gegenereerd door de Azure Site Recovery-implementatieplanner bevat alle gegevens over de niet-compatibele virtuele machines op het werkblad Niet-compatibele VMs.

![Niet-compatibele VM's](media/site-recovery-hyper-v-deployment-planner-analyze-report/incompatible-vms-h2a.png)

**VM-naam**: de naam van de virtuele machine, die wordt gebruikt in het bestand dat is opgegeven voor VMListFile wanneer een rapport wordt gegenereerd. Deze kolom bevat ook de schijven (VHD's) die aan de virtuele machines zijn gekoppeld. De namen zijn onder andere de namen van de Hyper-V-hosts waar de virtuele machines zijn geplaatst toen ze door het hulpprogramma werden gedetecteerd tijdens de profileringsperiode.

**VM-compatibiliteit**: geeft aan waarom de virtuele machine niet compatibel is voor gebruik met Azure Site Recovery. De redenen worden voor elke niet-compatibele schijf van de virtuele machine beschreven. Op basis van gepubliceerde [opslaglimieten](https://aka.ms/azure-storage-scalbility-performance) kan dit een van de volgende redenen zijn:

* De schijf is groter dan 4095 GB. Azure Storage biedt momenteel geen ondersteuning voor gegevensschijven groter dan 4095 GB.

* De besturingssysteemschijf is groter dan 2047 GB voor generatie 1-virtuele machines (BIOS-opstarttype).  Azure Site Recovery biedt geen ondersteuning voor besturingssysteemschijven groter dan 2047 GB voor virtuele machines van generatie 1.

* De besturingssysteemschijf is groter dan 300 GB voor generatie 2-virtuele machines (EFI-opstarttype). Azure Site Recovery biedt geen ondersteuning voor besturingssysteemschijven groter dan 300 GB voor virtuele machines van generatie 2.

* De VM-naam bevat een of meer van de volgende tekens “” [] ` die niet worden ondersteund.  Het hulpprogramma kan de geprofileerde gegevens van virtuele machines die deze tekens in hun namen hebben, niet ophalen. 

* VHD wordt gedeeld door twee of meer virtuele machines. Azure biedt geen ondersteuning voor virtuele machines met een gedeelde VHD.

* VM's met Virtual Fiber Channel worden niet ondersteund. Azure Site Recovery biedt geen ondersteuning voor virtuele machines met Virtual Fiber Channel.

* Hyper-V-cluster bevat geen replicatiebroker. Azure Site Recovery biedt geen ondersteuning voor een virtuele machine in een Hyper-V-cluster als de Hyper-V replicabroker niet voor het cluster is geconfigureerd.

* VM heeft geen hoge beschikbaarheid. Azure Site Recovery biedt geen ondersteuning voor een virtuele machine van een Hyper-V-clusterknooppunt waarvan VHD's op de lokale schijf in plaats van op de clusterschijf worden opgeslagen. 

* Totale grootte van virtuele machine (replicatie + TFO) overschrijdt de ondersteunde limiet voor opslagaccounts (35 TB). Dit probleem treedt meestal op wanneer één schijf in de virtuele machine een prestatiekenmerk heeft dat groter is dan de maximaal ondersteunde limieten voor Standard-opslag van Azure of Azure Site Recovery. De virtuele machine komt dan in aanmerking voor Premium Storage. De maximaal ondersteunde grootte van een Premium Storage-account is echter 35 TB en één beveiligde virtuele machine kan niet worden beveiligd via meerdere opslagaccounts. Houd er ook rekening mee dat als bij het uitvoeren van een testfailover op een beveiligde virtuele machine een niet-beheerde schijf voor testfailover is geconfigureerd, deze wordt uitgevoerd in het opslagaccount waarin ook de replicatie plaatsvindt. In dit geval is dezelfde extra hoeveelheid opslagruimte vereist als die voor replicatie. Hiermee zorgt u ervoor dat de voortgang van een replicatie en een testfailover parallel kunnen plaatsvinden. Wanneer de beheerde schijf is geconfigureerd voor de testfailover, hoeft er geen extra ruimte te worden gereserveerd voor de testfailover-VM.

* De bron-IOPS is groter dan de ondersteunde IOPS-limiet voor opslag van 7500 per schijf.

* De bron-IOPS is groter dan de ondersteunde IOPS-limiet voor opslag van 80.000 per schijf.

* Het gemiddelde gegevensverloop van de bron-VM overschrijdt de ondersteunde Azure Site Recovery-limiet voor gegevensverloop van 10 MB/s voor de gemiddelde I/O-grootte voor de schijf.

* De gemiddelde effectieve schrijf-IOPS van de bron-VM overschrijdt de ondersteunde IOPS-limiet van Azure Site Recovery van 840 voor schijven.

* De berekende opslag voor momentopnamen overschrijdt de ondersteunde opslaglimiet van 10 TB voor momentopnamen.

**Piek voor R/W IOPS (met groeifactor)**: de piekworkload-IOPS op de schijf (standaard het 95e percentiel) met inbegrip van de toekomstige groeifactor (standaard 30%). De totale IOPS voor lezen/schrijven van een virtuele machine is niet altijd de som van de IOPS voor lezen/schrijven van de afzonderlijke schijven van de virtuele machine. De piek-IOPS voor lezen/schrijven van de virtuele machine is namelijk de piek van de som van de IOPS voor lezen/schrijven van de afzonderlijke schijven voor elke minuut van de profileringsperiode.

**Piek voor gegevensverloop in Mb/s (met groeifactor)**: het piekverloop op de schijf (standaard het 95e percentiel) met inbegrip van de toekomstige groeifactor (standaard 30%). Houd er rekening mee dat de totale gegevensverloopwaarde van de virtuele machine niet altijd de som is van het gegevensverloop van de afzonderlijke schijven van de virtuele machine. De piekwaarde voor het gegevensverloop van de virtuele machine is namelijk de piek van de som van het gegevensverloop van de individuele schijven voor elke minuut van de profileringsperiode.

**Aantal schijven**: het totale aantal VHD's van de virtuele machine.

**Schijfgrootte (GB)**: de totale ingestelde grootte van alle schijven van de virtuele machine. Het hulpprogramma toont ook de schijfgrootte voor de afzonderlijke schijven in de virtuele machine.

**Kerngeheugens**: het aantal CPU-kerngeheugens van de virtuele machine.

**Geheugen (MB)**: de hoeveelheid RAM-geheugen van de virtuele machine.

**NIC's**: het aantal NIC's van de virtuele machine.

**Opstarttype**: het opstarttype van de virtuele machine. Dit kan BIOS of EFI zijn.

## <a name="azure-site-recovery-limits"></a>Azure Site Recovery-limieten
De volgende tabel bevat de Azure Site Recovery-limieten. Deze limieten zijn gebaseerd op onze tests, maar dekken niet alle mogelijke toepassings-I/O-combinaties. De werkelijke resultaten kunnen variëren op basis van uw toepassings-I/O-combinatie. Voor optimale resultaten, zelfs na het plannen van de implementatie, is het altijd beter om toepassingen uitgebreid te testen met behulp van een testfailover. Zo krijgt u een nauwkeurig inzicht in de prestaties van de applicatie.
 
**Beoogde replicatieopslag** | **Gemiddelde I/O-grootte van bron-VM** |**Gemiddeld gegevensverloop van bron-VM** | **Totale gegevensverloop van bron-VM per dag**
---|---|---|---
Standard Storage | 8 kB | 2 MB/s per VM | 168 GB per VM
Premium Storage | 8 kB  | 5 MB/s per VM | 421 GB per VM
Premium Storage | 16 kB of hoger| 10 MB/s per VM | 842 GB per VM

Deze limieten zijn gemiddelden uitgaande van een I/O-overlapping van 30%. Azure Site Recovery kan hogere doorvoer verwerken op basis van overlappingsverhouding, grotere schrijfgrootten en daadwerkelijk workload-I/O-gedrag. De bovenstaande waarden zijn gebaseerd op een typische backlog van ongeveer vijf minuten. Dat wil zeggen dat de gegevens na het uploaden binnen vijf minuten worden verwerkt en er een herstelpunt is gemaakt.

## <a name="on-premises-storage-requirement"></a>On-premises opslagvereiste

Het werkblad toont de totale vrije ruimte opslagvereiste voor elk volume van de Hyper-V-servers (waar de VHD's zich bevinden) die nodig is voor een geslaagde initiële replicatie en voor replicatie van verschillen. Voordat u replicatie inschakelt, moet u de vereiste opslagruimte aan de volumes toevoegen om ervoor te zorgen dat de replicatie geen ongewenste downtime van uw productietoepassingen veroorzaakt. 

De Azure Site Recovery-implementatieplanner bepaalt de optimale vereiste opslagruimte op basis van de grootte van de VHD's en de netwerkbandbreedte die voor replicatie wordt gebruikt.

![on-premises opslagvereiste](media/site-recovery-hyper-v-deployment-planner-analyze-report/on-premises-storage-requirement-h2a.png)

### <a name="why-do-i-need-free-space-on-the-hyper-v-server-for-the-replication"></a>Waarom heb ik op de Hyper-V-server vrije ruimte nodig voor de replicatie?
* Wanneer u de replicatie van een virtuele machine inschakelt, maakt Azure Site Recovery voor de initiële replicatie (IR) een momentopname van elke VHD van de virtuele machine. Tijdens de initiële replicatie worden nieuwe wijzigingen door de toepassing maar de schijven geschreven. Azure Site Recovery houdt deze wijzigingen in de logboekbestanden bij en daarvoor is extra opslagruimte nodig.  Totdat de initiële replicatie is voltooid, worden de logboekbestanden lokaal opgeslagen. Als er niet voldoende ruimte beschikbaar is voor de logboekbestanden en de momentopname (AVHDX), gaat de replicatie over in de hersynchronisatiemodus en wordt de replicatie nooit voltooid. In het ergste geval hebt u voor de initiële replicatie 100% van de VHD-grootte aan extra vrije ruimte nodig.
* Nadat de initiële replicatie is voltooid, begint de replicatie van verschillen. Azure Site Recovery houdt de wijzigingen als gevolg van de replicatie van verschillen bij in de logboekbestanden die zijn opgeslagen op het volume waar de VHD's van de virtuele machine zich bevinden. Deze logboekbestanden worden gerepliceerd naar Azure met de frequentie die voor kopiëren is ingesteld. Op basis van de beschikbare netwerkbandbreedte kan het even duren voordat de logboekbestanden naar Azure zijn gerepliceerd. Als er niet voldoende vrije ruimte beschikbaar is voor het opslaan van de logboekbestanden, wordt de replicatie onderbroken en gaat de replicatiestatus van de virtuele machine over in een status waarbij hersynchronisatie is vereist.
* Als er niet voldoende netwerkbandbreedte is om-de logboekbestanden naar Azure te pushen, stapelen de logboekbestanden zich op het volume op. In het ergste geval, waarbij de grootte van logboekbestanden tot 50% van de grootte van de VHD toeneemt, gaat de replicatie van de virtuele machine over in een status waarbij hersynchronisatie is vereist. In het ergste geval hebt u voor de initiële replicatie 50% van de VHD-grootte aan extra vrije ruimte nodig.

**Hyper-V-host**: de lijst met geprofileerde Hyper-V-servers. Als een server deel uitmaakt van een Hyper-V-cluster, worden alle clusterknooppunten gegroepeerd.

**Volume (VHD-pad)**: elk volume van een Hyper-V-host waar VHD's/VHDX-en aanwezig zijn. 

**Vrije beschikbare (GB)**: de hoeveelheid vrije ruimte op het volume.

**Totale opslagruimte vereist op het volume (GB)**: de totale vrije opslagruimte die op het volume is vereist voor een geslaagde initiële replicatie en replicatie van verschillen. 

**Totaal aan extra opslagruimte dat moet worden ingericht op het volume voor een geslaagde replicatie**: dit is de aanbevolen hoeveelheid totale extra ruimte die moet worden ingericht op het volume voor een geslaagde initiële replicatie en replicatie van verschillen.

## <a name="initial-replication-batching"></a>Initiële replicatie via batchverwerking 

### <a name="why-do-i-need-initial-replication-ir-batching"></a>Waarom moet ik een initiële replicatie via batchverwerking uitvoeren?
Als alle virtuele machines tegelijkertijd zouden worden beveiligd, zou de vereiste vrije opslagruimte veel hoger uitvallen, en als er niet voldoende opslagruimte beschikbaar is, gaat de replicatie van de virtuele machines over in de hersynchronisatiemodus. Bovendien zou er veel meer netwerkbandbreedte nodig zijn om een initiële replicatie van alle virtuele machines te kunnen voltooien. 

### <a name="initial-replication-batching-for-a-selected-rpo"></a>Initiële replicatie via batchverwerking voor een geselecteerde RPO
Dit werkblad bevat de detailweergave van elke batch voor een initiële replicatie (IR). Voor elke RPO wordt een afzonderlijk werkblad IR-batchverwerking gemaakt. 

Zodra u de aanbeveling voor lokale vereiste opslag voor elk volume hebt opgevolgd, wordt de belangrijkste informatie die u nodig hebt voor replicatie vermeld in de lijst met virtuele machines die parallel kunnen worden beveiligd. Deze virtuele machines worden samen in een batch gegroepeerd. Er kunnen meerdere batches voorkomen. U moet de virtuele machines in de volgorde van de opgegeven batches beveiligen. Beveilig eerst de virtuele machines in batch 1 en zodra de initiële replicatie is voltooid, beveiligt u de virtuele machines in batch 2 enzovoort. De lijst met batches en bijbehorende virtuele machines kunt u via dit blad verkrijgen. 

![IR-batchverwerking - details1](media/site-recovery-hyper-v-deployment-planner-analyze-report/ir-batching-for-rpo-h2a.png)
![IR-batchverwerking - details2](media/site-recovery-hyper-v-deployment-planner-analyze-report/ir-batching-for-rpo2-h2a.png)

### <a name="each-batch-provides-the-following-information"></a>Elke batch biedt de volgende informatie 
**Hyper-V-host**: de Hyper-V-host van de virtuele machine die moet worden beveiligd.
Virtuele machine: de VM die moet worden beveiligd. 

**Opmerkingen**: als voor een bepaald volume van een virtuele machine een actie is vereist, wordt hier een opmerking weergegeven.  Als er bijvoorbeeld niet voldoende vrije ruimte beschikbaar is op een volume, wordt hier de opmerking ‘Extra opslag toevoegen om deze VM te beveiligen' weergegeven.

**Volume (VHD-pad)**: de naam van het volume waar de VHD's van de virtuele machine zich bevinden. Vrije ruimte beschikbaar op het volume (GB): de vrije schijfruimte die voor de virtuele machine beschikbaar is op het volume. Bij het berekenen van de beschikbare vrije ruimte op de volumes, wordt rekening gehouden met de schijfruimte die wordt gebruikt voor de replicatie van verschillen voor de virtuele machines in de vorige batches waarvan de VHD's zich op hetzelfde volume bevinden.  

Bijvoorbeeld, VM1, VM2 en VM3 bevinden zich op een volume zoals E:\VHDpath. Vóór de replicatie is er 500 GB aan vrije ruimte op het volume. VM1 maakt deel uit van batch 1, VM2 maakt deel uit van batch 2 en VM3 maakt deel uit van batch 3.  Voor VM1 is er 500 G aan vrije ruimte beschikbaar. Voor VM2 is 500 vrije ruimte beschikbaar, de schijfruimte die is vereist voor een replicatie van verschillen voor VM1.  Stel dat er voor VM1 300 GB ruimte nodig is voor een replicatie van verschillen, dan zou de vrije ruimte die beschikbaar is voor VM2 500 GB – 300 GB = 200 GB bedragen.  Voor een replicatie van verschillen van VM2 zou er dus 300 GB nodig zijn. De hoeveelheid vrije ruimte voor VM3 zou 200 GB - 300 GB =-100 GB zijn.

**Opslagruimte vereist op het volume (GB) voor een initiële replicatie**: de vrije opslagruimte die op het volume is vereist om een initiële replicatie voor de virtuele machine te kunnen uitvoeren.

**Opslagruimte vereist op het volume (GB) voor een replicatie van verschillen**: de vrije opslagruimte die op het volume is vereist om een replicatie van verschillen voor de virtuele machine te kunnen uitvoeren.

**Extra opslag vereist op basis van een tekort dat een replicatiefout zou veroorzaken (GB)**: de extra vereiste opslagruimte op het volume voor de virtuele machine.  Dit is de maximale hoeveelheid opslagruimte (beschikbare ruimte op het volume) die is vereist voor een initiële replicatie en een replicatie van verschillen.

**Minimale bandbreedte vereist voor initiële replicatie (Mbps)**: de minimale bandbreedte die is vereist voor de initiële replicatie voor de virtuele machine.

**Minimale bandbreedte vereist voor replicatie van verschillen (Mbps)**: de minimale bandbreedte die is vereist voor de replicatie van verschillen voor de virtuele machine.

### <a name="network-utilization-details-for-each-batch"></a>Gegevens over het netwerkgebruik voor elke batch 
Elke tabel bevat een samenvatting van het netwerkgebruik van de batch.

**Bandbreedte beschikbaar voor batch**: de bandbreedte die beschikbaar is voor de batch, rekening houdend met de bandbreedte voor de replicatie van verschillen van de vorige batch.

**Geschatte bandbreedte beschikbaar voor initiële replicatie van batch**: de bandbreedte die beschikbaar is voor een initiële replicatie van de virtuele machines van de batch. 

**Verbruikte bandbreedte initiële replicatie van verschillen van batch**: de bandbreedte die beschikbaar is voor een replicatie van verschillen van de virtuele machines van de batch. 

**Geschatte tijd van de initiële replicatie voor batch (uu:mm)**: de geschatte tijd voor de initiële replicatie in uren:minuten.

## <a name="cost-estimation"></a>Kostenraming
Meer informatie over [kostenraming](site-recovery-hyper-v-deployment-planner-cost-estimation.md). 

## <a name="next-steps"></a>Volgende stappen
Meer informatie over [kostenraming](site-recovery-hyper-v-deployment-planner-cost-estimation.md).