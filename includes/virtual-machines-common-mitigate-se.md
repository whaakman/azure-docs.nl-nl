---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 08/14/2018
ms.author: cynthn;kareni
ms.custom: include file
ms.openlocfilehash: 130cc66831b25621cb022eb19005c624fcd71b9e
ms.sourcegitcommit: 7b845d3b9a5a4487d5df89906cc5d5bbdb0507c8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/14/2018
ms.locfileid: "40105503"
---
**Laatste update document**: 14 augustus 2018 10:00 AM PST.

De vermelding van een [nieuwe klasse van CPU-beveiligingsproblemen](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/ADV180002) die bekend staat als speculatieve uitvoering van side-channel aanvallen heeft geresulteerd in vragen van klanten om meer duidelijkheid te zoeken.  

Microsoft heeft oplossingen worden geïmplementeerd in onze cloudservices. De infrastructuur die wordt uitgevoerd van Azure en klantwerkbelastingen van elkaar geïsoleerd is beveiligd. Dit betekent dat een potentiële aanvaller met behulp van dezelfde infrastructuur kan niet aanvallen op uw toepassing met behulp van deze beveiligingslekken.

Het met behulp van Azure [geheugen onderhoud met statusbehoud](https://docs.microsoft.com/azure/virtual-machines/windows/maintenance-and-updates#memory-preserving-maintenance) zoveel mogelijk minimaliseren gevolgen zijn voor klanten en elimineert de noodzaak van opnieuw wordt opgestart. Azure zal blijven deze methoden gebruiken bij het maken van systeembrede updates naar de host en onze klanten te beveiligen.

Meer informatie over hoe de beveiliging is geïntegreerd in elk aspect van Azure is beschikbaar op de [documentatie over Azure-beveiliging](https://docs.microsoft.com/azure/security/) site. 

> [!NOTE] 
> Aangezien dit document werd gepubliceerd, zijn meerdere varianten van deze klasse beveiligingsproblemen gemaakt. Microsoft blijft zwaar worden geïnvesteerd in onze klanten te beschermen en bieden van richtlijnen. Deze pagina wordt bijgewerkt wanneer we doorgaan met de release verder oplossingen. 
> 
> 14 augustus 2018 de bedrijfstak een nieuw kanaal beveiligingslek voor speculatieve uitvoering kant wel vermeld [L1 Terminal fouttolerantie](https://portal.msrc.microsoft.com/en-US/security-guidance/advisory/ADV180018) (L1TF) die is toegewezen meerdere CVEs ([CVE-2018-3615, CVE-2018-3620, en CVE-2018-3646](https://www.intel.com/content/www/us/en/security-center/advisory/intel-sa-00161.html)). Deze kwetsbaarheid is van invloed op de® Core Intel-processors en Intel® Xeon®-processors. Microsoft heeft oplossingen worden geïmplementeerd in onze cloudservices die de isolatie tussen klanten versterken. Lees hieronder voor meer informatie om te beveiligen tegen L1TF en vorige zwakke plekken ([Spectre Variant 2 CVE-2017-5715 en Meltdown Variant 3 CVE-2017-5754](https://support.microsoft.com/help/4072698/windows-server-guidance-to-protect-against-the-speculative-execution)).
>  






## <a name="keeping-your-operating-systems-up-to-date"></a>Het besturingssysteem up-to-date te houden

Tijdens een update van het besturingssysteem niet vereist is voor het isoleren van andere Azure-klanten uw toepassingen op Azure, maar het is altijd een aanbevolen procedure op uw software up-to-date te houden. De meest recente beveiligingsupdate updatepakketten voor Windows bevatten oplossingen voor verschillende speculatieve uitvoering kant kanaal beveiligingslekken. Linux-distributies zijn op deze manier meerdere updates om deze beveiligingslekken verhelpen uitgebracht. Hier volgen de aanbevolen acties om bij te werken van uw besturingssysteem:

| Aanbieding | Aanbevolen actie  |
|----------|---------------------|
| Azure Cloud Services  | Schakel [automatische updates](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-configure-portal) of zorg ervoor dat u het nieuwste Gast-besturingssysteem worden uitgevoerd. |
| Virtuele Linux-machines in Azure | Updates van de provider van uw besturingssysteem geïnstalleerd. Zie voor meer informatie, [Linux](#linux) verderop in dit document. |
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

U kunt aanvullende beveiligingsfuncties die binnen uw virtuele machine of Service in de Cloud inschakelen.

### <a name="windows"></a>Windows 

Het beoogde besturingssysteem moet worden bijgewerkt zodat deze aanvullende beveiligingsfuncties. Hoewel talrijke speculatieve uitvoering kant kanaal oplossingen zijn standaard ingeschakeld, wordt de aanvullende functies die hier worden beschreven handmatig moeten zijn ingeschakeld en kunnen ertoe leiden dat een prestatie-impact. 

**Stap 1**: [contact opnemen met Azure Support](https://aka.ms/MicrocodeEnablementRequest-SupportTechnical) naar expose bijgewerkt firmware (microcode) in uw virtuele Machines. 

**Stap 2**: ondersteuning voor inschakelen Kernel virtuele-adres maken van schaduwkopieën (KVAS) en de vertakking doel injectie (BTI)-besturingssysteem. Volg de instructies in [KB4072698](https://support.microsoft.com/help/4072698/windows-server-guidance-to-protect-against-the-speculative-execution) om in te schakelen die via de `Session Manager` registersleutels. Opnieuw opstarten is vereist. 

**Stap 3**: voor implementaties die gebruikmaken van [geneste virtualisatie](https://docs.microsoft.com/azure/virtual-machines/windows/nested-virtualization) (D3 en E3 alleen): deze instructies zijn van toepassing binnen de virtuele machine die u als een Hyper-V-host gebruikt. 

1. Volg de instructies in [KB4072698](https://support.microsoft.com/help/4072698/windows-server-guidance-to-protect-against-the-speculative-execution) om in te schakelen die via de `MinVmVersionForCpuBasedMitigations` registersleutels.  
 
1. Het type van de scheduler hypervisor instellen op **Core** door de instructies te volgen [hier](https://docs.microsoft.com/windows-server/virtualization/hyper-v/manage/manage-hyper-v-scheduler-types). 

**Stap 4**: Volg de instructies in [KB4072698](https://support.microsoft.com/help/4072698/windows-server-guidance-to-protect-against-the-speculative-execution) om te controleren of beveiliging zijn ingeschakeld met behulp van de [SpeculationControl](https://aka.ms/SpeculationControlPS) PowerShell-module. 

> [!NOTE]
> Als u deze module eerder hebt gedownload, moet u de nieuwste versie te installeren.
>

Alle virtuele machines moeten worden weergegeven:

```
branch target injection mitigation is enabled: True

kernel VA shadow is enabled: True  

L1TFWindowsSupportEnabled: True
```


### <a name="linux"></a>Linux

<a name="linux"></a>Het inschakelen van de reeks aanvullende beveiligingsfuncties in is vereist dat het gebruikte besturingssysteem volledig up-to-date zijn. Sommige oplossingen wordt standaard ingeschakeld. De volgende sectie beschrijft de functies die uitgeschakeld, standaard en/of vertrouwen op hardware-ondersteuning (microcode zijn). Inschakelen van deze functies mogelijk invloed op de prestaties. Verwijzen naar de documentatie van de provider van uw besturingssysteem voor verdere instructies
 
**Stap 1**: [contact opnemen met Azure Support](https://aka.ms/MicrocodeEnablementRequest-SupportTechnical) naar expose bijgewerkt firmware (microcode) in uw virtuele Machines.
 
**Stap 2**: ondersteuning voor inschakelen vertakking doel injectie (BTI) OS CVE-2017-5715 (Spectre Variant 2) beperken door de documentatie van uw besturingssysteem-provider te volgen. 
 
**Stap 3**: inschakelen Kernel pagina tabel isolatie (KPTI) CVE-2017-5754 (Meltdown Variant 3) beperken door de documentatie van uw besturingssysteem-provider te volgen. 
 
Meer informatie vindt u van uw besturingssysteem-provider:  
 
- [RedHat en CentOS](https://access.redhat.com/security/vulnerabilities/speculativeexecution) 
- [SUSE](https://www.suse.com/support/kb/doc/?id=7022512) 
- [Ubuntu](https://wiki.ubuntu.com/SecurityTeam/KnowledgeBase/SpectreAndMeltdown) 


## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie, [Azure-klanten beveiligen tegen CPU-beveiligingsproblemen](https://azure.microsoft.com/blog/securing-azure-customers-from-cpu-vulnerability/).
