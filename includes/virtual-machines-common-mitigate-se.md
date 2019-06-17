---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 06/04/2019
ms.author: cynthn;kareni
ms.custom: include file
ms.openlocfilehash: 46ade0ecb0e2e081585803a0b1bc7eab989e21e6
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66735225"
---
**Laatste update document**: 4 juni 2019 3:00 uur PST.

De vermelding van een [nieuwe klasse van CPU-beveiligingsproblemen](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/ADV180002) die bekend staat als speculatieve uitvoering van side-channel aanvallen heeft geresulteerd in vragen van klanten om meer duidelijkheid te zoeken.  

Microsoft heeft oplossingen worden geïmplementeerd in onze cloudservices. De infrastructuur die wordt uitgevoerd van Azure en klantwerkbelastingen van elkaar geïsoleerd is beveiligd. Dit betekent dat een potentiële aanvaller met behulp van dezelfde infrastructuur kan niet aanvallen op uw toepassing met behulp van deze beveiligingslekken.

Het met behulp van Azure [geheugen onderhoud met statusbehoud](https://docs.microsoft.com/azure/virtual-machines/windows/maintenance-and-updates#maintenance-not-requiring-a-reboot) zoveel mogelijk minimaliseren gevolgen zijn voor klanten en elimineert de noodzaak van opnieuw wordt opgestart. Azure zal blijven deze methoden gebruiken bij het maken van systeembrede updates naar de host en onze klanten te beveiligen.

Meer informatie over hoe de beveiliging is geïntegreerd in elk aspect van Azure is beschikbaar op de [documentatie over Azure-beveiliging](https://docs.microsoft.com/azure/security/) site. 

> [!NOTE] 
> Aangezien dit document werd gepubliceerd, zijn meerdere varianten van deze klasse beveiligingsproblemen gemaakt. Microsoft blijft zwaar worden geïnvesteerd in onze klanten te beschermen en bieden van richtlijnen. Deze pagina wordt bijgewerkt wanneer we doorgaan met de release verder oplossingen. 
> 
> Op 14 mei 2019 [Intel vermeld](https://www.intel.com/content/www/us/en/security-center/advisory/intel-sa-00233.html) een nieuwe set speculatieve uitvoering kant kanaal beveiligingslek bekend als micro-architecturale steekproeven te nemen (MDS Zie het Microsoft-beveiligingsrichtlijnen [ADV190013](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/ADV190013)), die is toegewezen meerdere CVEs: 
> - CVE-2019-11091 - micro-architecturale gegevens steekproeven Uncacheable geheugen (MDSUM)
> - CVE-2018-12126 - micro-architecturale Store Buffer gegevens steekproeven (MSBDS) 
> - CVE-2018-12127 - micro-architecturale Load poort gegevens steekproeven (MLPDS)
> - CVE-2018-12130 - micro-architecturale opvulling Buffer gegevens steekproeven (MFBDS)
>
> Deze kwetsbaarheid is van invloed op de® Core Intel-processors en Intel® Xeon®-processors.  Microsoft Azure is besturingssysteemupdates en is de implementatie van nieuwe microcode als deze beschikbaar wordt gesteld door Intel, in onze vloot om onze klanten op basis van deze nieuwe beveiligingsproblemen te beschermen.   Azure werkt nauw samen met de Intel om te testen en valideren van de nieuwe microcode vóór de officiële release van het platform. 
>
> **Klanten met niet-vertrouwde code binnen hun VM** moet actie ondernemen om u te beschermen tegen deze beveiligingslekken door te lezen hieronder voor meer informatie over alle speculatieve uitvoering van side-channel beveiligingsproblemen (Microsoft aanbevelingen ADV [180002](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/ADV180002), [180018](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/adv180018), en [190013](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/ADV190013)).
>
> Andere klanten moeten deze beveiligingslekken uit een ingrijpende in het perspectief van de diepte evalueren en houd rekening met de gevolgen voor beveiliging en prestaties van de gekozen configuratie.



## <a name="keeping-your-operating-systems-up-to-date"></a>Het besturingssysteem up-to-date te houden

Tijdens een update van het besturingssysteem niet vereist is voor het isoleren van andere Azure-klanten uw toepassingen op Azure, maar het is altijd een aanbevolen procedure op uw software up-to-date te houden. De meest recente beveiligingsupdate updatepakketten voor Windows bevatten oplossingen voor verschillende speculatieve uitvoering kant kanaal beveiligingslekken. Linux-distributies zijn op deze manier meerdere updates om deze beveiligingslekken verhelpen uitgebracht. Hier volgen de aanbevolen acties om bij te werken van uw besturingssysteem:

| Aanbieding | Aanbevolen actie  |
|----------|---------------------|
| Azure Cloud Services  | Schakel [automatische updates](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-configure-portal) of zorg ervoor dat u het nieuwste Gast-besturingssysteem worden uitgevoerd. |
| Virtuele Azure Linux-Machines | Updates van de provider van uw besturingssysteem geïnstalleerd. Zie voor meer informatie, [Linux](#linux) verderop in dit document. |
| Windows Azure virtuele Machines  | Installeer de meest recente updatepakket van beveiliging.
| Andere Azure PaaS-Services | Er is geen actie nodig is voor klanten die gebruikmaken van deze services. Azure blijft automatisch uw versies van het besturingssysteem up-to-date. |

## <a name="additional-guidance-if-you-are-running-untrusted-code"></a>Aanvullende richtlijnen als u niet-vertrouwde code worden uitgevoerd 

Klanten die niet-vertrouwde gebruikers uit te voeren willekeurige code wilt implementeren enkele aanvullende beveiligingsfuncties in hun Azure Virtual Machines of Cloud Services. Deze functies beschermen tegen de openbaarmaking van intra verwerken vectoren die beveiligingsproblemen speculatieve uitvoering worden beschreven.

Voorbeeld van de scenario's waarin aanvullende beveiligingsfuncties die worden aanbevolen:

- U kunt code die u niet vertrouwt om te worden uitgevoerd binnen uw virtuele machine.  
    - *U kunt bijvoorbeeld een van uw klanten om te uploaden van een binair bestand of script dat u vervolgens uit te voeren in uw toepassing*. 
- U kunt gebruikers die u niet vertrouwt om u te melden bij uw VM lage bevoegde accounts gebruiken.   
    - *U kunt bijvoorbeeld een gebruiker met beperkte machtigingen aan te melden bij een van uw virtuele machines met behulp van extern bureaublad of SSH*.  
- U kunt niet-vertrouwde gebruikers toegang tot virtuele machines die zijn geïmplementeerd via geneste virtualisatie.  
    - *Bijvoorbeeld, u de Hyper-V-host te beheren, maar de virtuele machines toewijzen aan niet-vertrouwde gebruikers*. 

Klanten die niet een scenario met betrekking tot niet-vertrouwde code geïmplementeerd hoeft niet te deze aanvullende beveiligingsfuncties inschakelen. 

## <a name="enabling-additional-security"></a>Extra beveiliging inschakelen 

Als u niet-vertrouwde code uitvoert, kunt u aanvullende beveiligingsfuncties die binnen uw virtuele machine of Service in de Cloud inschakelen. Parallel, zorg ervoor dat het besturingssysteem is bijgewerkt om in te schakelen van beveiligingsfuncties die binnen uw virtuele machine of Cloudservice

### <a name="windows"></a>Windows 

Het beoogde besturingssysteem moet worden bijgewerkt zodat deze aanvullende beveiligingsfuncties. Hoewel talrijke speculatieve uitvoering kant kanaal oplossingen zijn standaard ingeschakeld, wordt de aanvullende functies die hier worden beschreven handmatig moeten zijn ingeschakeld en kunnen ertoe leiden dat een prestatie-impact. 


**Stap 1: Hyper-threading op de virtuele machine uitschakelen** -klanten die niet-vertrouwde code uitvoeren op een hyper-threaded VM moet uitschakelen hyper-threading of verplaatsen naar een niet-hyper-threaded VM-grootte. Naslaginformatie over [dit document](https://docs.microsoft.com/azure/virtual-machines/windows/acu) voor een overzicht van hyper-threaded VM-grootten (verhouding van vCPU Core is 2:1). Als u wilt controleren of uw virtuele machine hyper-threading ingeschakeld heeft, raadpleegt u het onderstaande script met de Windows-opdrachtregel uit vanuit de virtuele machine.

Type `wmic` in te voeren van de interactieve interface. Typ vervolgens de onderstaande om weer te geven van de hoeveelheid fysieke en logische processors op de virtuele machine.

```console
CPU Get NumberOfCores,NumberOfLogicalProcessors /Format:List
```

Als het aantal logische processors groter is dan de fysieke processors (kernen), is klikt u vervolgens hyperthreading ingeschakeld.  Als u een virtuele hyper-threaded machine uitvoert, moet u [Neem contact op met ondersteuning voor Azure](https://aka.ms/MicrocodeEnablementRequest-SupportTechnical) ophalen-hyperthreading uitgeschakeld.  Als hyperthreading is uitgeschakeld, **ondersteuning moet een volledige VM opnieuw wordt opgestart**. Raadpleeg [aantal Core](#core-count) om te begrijpen waarom het aantal VM-kernen worden verlaagd.


**Stap 2**: Volg de instructies in parallel aan stap 1, [KB4072698](https://support.microsoft.com/help/4072698/windows-server-guidance-to-protect-against-the-speculative-execution) om te controleren of beveiliging zijn ingeschakeld met behulp van de [SpeculationControl](https://aka.ms/SpeculationControlPS) PowerShell-module.

> [!NOTE]
> Als u deze module eerder hebt gedownload, moet u de nieuwste versie te installeren.
>


De uitvoer van het PowerShell-script moet de onderstaande waarden om te valideren ingeschakeld beveiliging biedt tegen deze beveiligingslekken:

```
Windows OS support for branch target injection mitigation is enabled: True
Windows OS support for kernel VA shadow is enabled: True
Windows OS support for speculative store bypass disable is enabled system-wide: False
Windows OS support for L1 terminal fault mitigation is enabled: True
Windows OS support for MDS mitigation is enabled: True
```

Als de uitvoer ziet u `MDS mitigation is enabled: False`, meldt [Neem contact op met ondersteuning voor Azure](https://aka.ms/MicrocodeEnablementRequest-SupportTechnical) voor risicobeperking beschikbare opties.



**Stap 3**: Volg de instructies in om in te schakelen Kernel virtuele-adres maken van schaduwkopieën (KVAS) en de vertakking doel injectie (BTI) OS-ondersteuning, [KB4072698](https://support.microsoft.com/help/4072698/windows-server-guidance-to-protect-against-the-speculative-execution) om in te schakelen van beveiliging met behulp van de `Session Manager` registersleutels. Opnieuw opstarten is vereist.


**Stap 4**: Voor implementaties die gebruikmaken van [geneste virtualisatie](https://docs.microsoft.com/azure/virtual-machines/windows/nested-virtualization) (D3 en E3 alleen): Deze instructies zijn van toepassing binnen de virtuele machine die u als een Hyper-V-host gebruikt.

1.  Volg de instructies in [KB4072698](https://support.microsoft.com/help/4072698/windows-server-guidance-to-protect-against-the-speculative-execution) om in te schakelen van beveiliging met behulp van de `MinVmVersionForCpuBasedMitigations` registersleutels.
2.  Het type van de scheduler hypervisor instellen op `Core` door de instructies te volgen [hier](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-scheduler-types).


### <a name="linux"></a>Linux

<a name="linux"></a>Het inschakelen van de reeks aanvullende beveiligingsfuncties in is vereist dat het gebruikte besturingssysteem volledig up-to-date zijn. Sommige oplossingen wordt standaard ingeschakeld. De volgende sectie beschrijft de functies die uitgeschakeld, standaard en/of vertrouwen op hardware-ondersteuning (microcode zijn). Inschakelen van deze functies mogelijk invloed op de prestaties. Verwijzen naar de documentatie van de provider van uw besturingssysteem voor verdere instructies


**Stap 1: Hyper-threading op de virtuele machine uitschakelen** -klanten die niet-vertrouwde code uitvoeren op een hyper-threaded VM moet uitschakelen hyper-threading of verplaatsen naar een niet-hyper-threaded-virtuele machine.  Naslaginformatie over [dit document](https://docs.microsoft.com/azure/virtual-machines/linux/acu) voor een overzicht van hyper-threaded VM-grootten (verhouding van vCPU Core is 2:1). Als u wilt controleren of u een virtuele hyper-threaded machine wordt uitgevoerd, voer de `lscpu` opdracht in de Linux-VM. 

Als `Thread(s) per core = 2`, klikt u vervolgens hyperthreading is ingeschakeld. 

Als `Thread(s) per core = 1`, klikt u vervolgens hyperthreading is uitgeschakeld. 

 
Voorbeeld van uitvoer voor een virtuele machine met hyper-threading ingeschakeld: 

```console
CPU Architecture:      x86_64
CPU op-mode(s):        32-bit, 64-bit
Byte Order:            Little Endian
CPU(s):                8
On-line CPU(s) list:   0-7
Thread(s) per core:    2
Core(s) per socket:    4
Socket(s):             1
NUMA node(s):          1

```

Als u een virtuele hyper-threaded machine uitvoert, moet u [Neem contact op met ondersteuning voor Azure](https://aka.ms/MicrocodeEnablementRequest-SupportTechnical) ophalen-hyperthreading uitgeschakeld.  Als hyperthreading is uitgeschakeld, **ondersteuning moet een volledige VM opnieuw wordt opgestart**. Raadpleeg [aantal Core](#core-count) om te begrijpen waarom het aantal VM-kernen worden verlaagd.



**Stap 2**: Bescherming tegen een van de onderstaande speculatieve uitvoering van side-channel beveiligingsproblemen, raadpleegt u de documentatie van de provider van uw besturingssysteem:   
 
- [RedHat en CentOS](https://access.redhat.com/security/vulnerabilities) 
- [SUSE](https://www.suse.com/support/kb/?doctype%5B%5D=DT_SUSESDB_PSDB_1_1&startIndex=1&maxIndex=0) 
- [Ubuntu](https://wiki.ubuntu.com/SecurityTeam/KnowledgeBase/) 


### <a name="core-count"></a>Aantal kernen

Wanneer een virtuele hyper-threaded machine wordt gemaakt, Azure toewijst 2 threads per kern - deze vcpu's worden genoemd. Wanneer hyper-threading is uitgeschakeld, wordt in Azure een thread en oppervlakken van één threaded kernen (fysieke kernen) verwijderd. De verhouding van vCPU aan CPU 2:1 is, dus eenmaal hyperthreading is uitgeschakeld, de CPU tellen in de virtuele machine wordt weergegeven met de helft is verminderd. Een VM D8_v3 is bijvoorbeeld een hyper-threaded VM wordt uitgevoerd op 8 vcpu's (2 threads per core x 4 cores).  Als hyperthreading is uitgeschakeld, doorlaat CPU's op 4 fysieke kernen met 1 thread per kern. 

## <a name="next-steps"></a>Volgende stappen

Dit artikel bevat richtlijnen aan de onderstaande speculatieve uitvoering van side-channel aanvallen die invloed hebben op veel moderne processors:

[Spectre Meltdown](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/ADV180002):
- CVE-2017-5715 - vertakking doel injectie (BTI)  
- CVE-2017-5754 - Kernel pagina tabel isolatie (KPTI)
- CVE-2018-3639 – speculatieve Store overslaan (KPTI) 
 
[L1 Terminal fouttolerantie (L1TF)](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/ADV180018):
- CVE-2018-3615 - Intel Software Guard-extensies (Intel SGX)
- CVE-2018-3620 - besturingssystemen (OS) en System Management-modus (SMM)
- CVE-2018-3646 – heeft gevolgen voor Virtual Machine Manager (VMM)

[Steekproef nemen voor gegevens micro-architecturale](https://portal.msrc.microsoft.com/en-us/security-guidance/advisory/ADV190013): 
- CVE-2019-11091 - micro-architecturale gegevens steekproeven Uncacheable geheugen (MDSUM)
- CVE-2018-12126 - micro-architecturale Store Buffer gegevens steekproeven (MSBDS)
- CVE-2018-12127 - micro-architecturale Load poort gegevens steekproeven (MLPDS)
- CVE-2018-12130 - micro-architecturale opvulling Buffer gegevens steekproeven (MFBDS)








