---
title: Meer informatie over een systeem opnieuw is opgestart voor een Azure-VM | Microsoft Docs
description: Geeft een lijst van de gebeurtenissen die kunnen leiden tot een virtuele machine opnieuw op te starten
services: virtual-machines
documentationcenter: ''
author: genlin
manager: willchen
editor: ''
tags: ''
ms.service: virtual-machines
ms.devlang: na
ms.topic: troubleshooting
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/31/2018
ms.author: genli
ms.openlocfilehash: f50be01d067032b1143a80e56ccb5d49d899d4d6
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2018
ms.locfileid: "50415964"
---
# <a name="understand-a-system-reboot-for-azure-vm"></a>Meer informatie over een systeem opnieuw is opgestart voor de virtuele machine van Azure

Virtuele Azure-machines (VM's) kunnen soms opnieuw opstarten geen zichtbare reden, zonder bewijs van uw dat de opstartbewerking heeft gestart opnieuw. In dit artikel bevat de acties en gebeurtenissen die kunnen leiden tot virtuele machines opnieuw op te starten en biedt inzicht in hoe u onverwacht opnieuw opstarten problemen voorkomen en vermindert de impact van dergelijke problemen.

## <a name="configure-the-vms-for-high-availability"></a>De virtuele machines voor hoge beschikbaarheid configureren
De beste manier om het beveiligen van een toepassing die wordt uitgevoerd in Azure op basis van de virtuele machine opnieuw wordt opgestart en uitvaltijd is het configureren van de VM's voor hoge beschikbaarheid.

Voor dit niveau van redundantie voor uw toepassingen, wordt u aangeraden twee of meer virtuele machines in een beschikbaarheidsset te groeperen. Deze configuratie zorgt ervoor dat ten minste één virtuele machine tijdens een gepland of ongepland onderhoud, beschikbaar is en voldoet aan de 99,95 procent [Azure SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_5/).

Zie voor meer informatie over beschikbaarheidssets voor de volgende artikelen:

- [De beschikbaarheid van virtuele machines beheren](../windows/manage-availability.md)
- [Beschikbaarheid van virtuele machines configureren](../windows/classic/configure-availability.md)

## <a name="resource-health-information"></a>Resource Health information 
Azure Resource Health is een service waarmee wordt aangegeven dat de status van afzonderlijke Azure-resources en praktische richtlijnen voor het oplossen van problemen. In een cloudomgeving waarbij het niet mogelijk voor rechtstreekse toegang tot servers of Infrastructuurelementen, is het doel van Resource Health de tijd die u kwijt zijn aan probleemoplossing te verkorten. In het bijzonder is het doel om de tijd die u besteedt aan het waarmee wordt bepaald of de hoofdmap van het probleem in de toepassing of een gebeurtenis in het Azure-platform ligt te beperken. Zie voor meer informatie, [Understand and use Resource Health](../../resource-health/resource-health-overview.md).

## <a name="actions-and-events-that-can-cause-the-vm-to-reboot"></a>Acties en gebeurtenissen die kunnen leiden tot de virtuele machine opnieuw op te starten

### <a name="planned-maintenance"></a>Gepland onderhoud
Microsoft Azure voert regelmatig updates over de hele wereld te verbeteren van de betrouwbaarheid, prestaties en beveiliging van de host-infrastructuur waarop virtuele machines. Veel van deze updates, met inbegrip van updates met geheugenbehoud worden uitgevoerd zonder dat dit van invloed is op uw virtuele machines of cloudservices.

Sommige updates vereisen echter opnieuw worden opgestart. In dergelijke gevallen kan de VM's zijn afgesloten terwijl wij patching van de infrastructuur en vervolgens de virtuele machines opnieuw worden gestart.

Als u wilt weten welke Azure gepland onderhoud is en hoe dit invloed hebben op de beschikbaarheid van uw virtuele Linux-machines, Zie de artikelen die hier worden vermeld. De artikelen bieden achtergrondinformatie over de Azure gepland onderhoudsproces en over het plannen van gepland onderhoud om verder te beperken de gevolgen.

- [Gepland onderhoud voor virtuele machines in Azure](../windows/planned-maintenance.md)
- [Gepland onderhoud op virtuele Azure-machines plannen](../windows/classic/planned-maintenance-schedule.md)

### <a name="memory-preserving-updates"></a>Updates met geheugenbehoud   
Gebruikers krijgen voor deze klasse van updates in Microsoft Azure, zonder gevolgen voor de actieve virtuele machines. Dit zijn veelal updates van onderdelen of services die kunnen worden bijgewerkt zonder de actieve sessie te verstoren. Sommige zijn updates van de platform-infrastructuur op het hostbesturingssysteem die kunnen worden toegepast zonder opnieuw opstarten van de virtuele machines.

Deze updates met geheugenbehoud worden uitgevoerd met technologie die in-place livemigratie mogelijk maakt. Wanneer deze wordt bijgewerkt, de virtuele machine wordt geplaatst in een *onderbroken* staat. In deze status blijft de geheugeninhoud in het RAM behouden. Ondertussen ontvangt het besturingssysteem van de onderliggende host de noodzakelijke updates en patches. De VM wordt 30 seconden na het onderbreken weer hervat. Op dat moment wordt de klok van de VM automatisch gesynchroniseerd.

Vanwege de periode korte onderbreking minder implementeren van updates via dit mechanisme aanzienlijk invloed op de virtuele machines. Niet alle updates kunnen echter op deze manier worden geïmplementeerd. 

Updates voor meerdere exemplaren (voor VM's in een beschikbaarheidsset) worden telkens voor één updatedomein tegelijk toegepast.

> [!NOTE]
> Linux-machines waarvoor de oude kernelversies worden beïnvloed door een kernelprobleem tijdens deze updatemethode. Om dit te voorkomen, bijwerken naar de kernel versie 3.10.0-327.10.1 of hoger. Zie voor meer informatie, [een Azure Linux-VM op een kernel 3.10 gebaseerde panics na de upgrade van een host-knooppunt](https://support.microsoft.com/help/3212236).     
    
### <a name="user-initiated-reboot-or-shutdown-actions"></a>De gebruiker geïnitieerde opnieuw opstarten of afsluiten acties
 
Als u een opnieuw opstarten van de Azure portal, Azure PowerShell, opdrachtregel-interface of Reset API uitvoert, vindt u de gebeurtenis in de [Azure Activity Log](../../monitoring-and-diagnostics/monitoring-overview-activity-logs.md).

Als u de actie vanuit het besturingssysteem van de VM uitvoert, vindt u de gebeurtenis in het systeemlogboek in Logboeken.

Andere scenario's die doorgaans ertoe leiden dat de virtuele machine opnieuw op te starten zijn meerdere configuratiewijziging acties. Normaal gesproken ziet u een waarschuwing die aangeeft dat het uitvoeren van een bepaalde actie zal leiden tot een opnieuw opstarten van de virtuele machine. Voorbeelden zijn VM resize bewerkingen, wijzigen van het wachtwoord van de Administrator-account en het instellen van een statisch IP-adres.

### <a name="azure-security-center-and-windows-update"></a>Azure Security Center en Windows Update
Azure Security Center bewaakt dagelijkse Windows en Linux-VM's voor het besturingssysteem updates ontbreken. Security Center wordt een lijst met beschikbare beveiligingsupdates en essentiële updates opgehaald van Windows Update of Windows Server Update Services (WSUS), afhankelijk van welke service is geconfigureerd op een Windows-VM. Security Center controleert ook of de meest recente updates voor Linux-systemen. Als uw virtuele machine het bijwerken van een systeem ontbreekt, wordt door Security Center adviseert systeemupdates toe te passen. De toepassing van deze systeemupdates wordt geregeld via de Security Center in Azure portal. Nadat u voor sommige updates toepast, is het mogelijk dat virtuele machine opnieuw opstarten vereist. Zie voor meer informatie, [systeemupdates in Azure Security Center toepassen](../../security-center/security-center-apply-system-updates.md).

Zoals on-premises servers, biedt Azure niet updates pushen vanaf Windows Update voor Windows-VM's, omdat deze machines zijn bedoeld om te worden beheerd door de gebruikers. U bent echter aangemoedigd om de instelling voor automatisch Windows Update ingeschakeld laten. Automatische installatie van updates via Windows Update kan ook leiden tot opnieuw wordt opgestart plaatsvindt nadat de updates worden toegepast. Zie voor meer informatie, [Veelgestelde vragen over de Windows Update](https://support.microsoft.com/help/12373/windows-update-faq).

### <a name="other-situations-affecting-the-availability-of-your-vm"></a>Andere situaties die betrekking hebben op de beschikbaarheid van uw virtuele machine
Er zijn andere gevallen waarin Azure actief wordt het gebruik van een virtuele machine mogelijk onderbreken. U ontvangt e-mailmeldingen voordat deze actie wordt ondernomen, dus een kans hebt u om de onderliggende problemen oplossen. Voorbeelden van problemen die invloed hebben op de VM-beschikbaarheid zijn schendingen van de beveiliging en de vervaldatum van de betalingswijzen.

### <a name="host-server-faults"></a>Host-server-fouten 
De virtuele machine wordt gehost op een fysieke server die wordt uitgevoerd binnen een Azure-datacenter. De fysieke server wordt uitgevoerd een agent de Hostagent naast een paar andere Azure-onderdelen genoemd. Wanneer deze Azure-software-onderdelen op de fysieke server reageert, wordt een opnieuw opstarten van de host-server om herstel door het systeem van toezicht geactiveerd. De virtuele machine is gewoonlijk beschikbaar binnen vijf minuten opnieuw en blijft live op dezelfde host als voorheen.

Server-fouten worden meestal veroorzaakt door hardware-uitval, zoals de uitval van een harde schijf of een SSD-schijf. Azure continu bewaakt deze instanties, identificeert de onderliggende bugs en de updates worden nadat de oplossing is geïmplementeerd en getest.

Omdat sommige host-server-fouten specifiek voor die server zijn kunnen, kan een situatie voor opnieuw opstarten van herhaalde virtuele machine worden verbeterd door handmatig opnieuw distribueren van de virtuele machine naar een andere hostserver. Met deze bewerking kan worden geactiveerd met behulp van de **opnieuw implementeren** optie op de pagina met details van de virtuele machine, of door te stoppen en opnieuw starten van de virtuele machine in Azure portal.

### <a name="auto-recovery"></a>Automatisch herstel
Als de host-server kan niet opnieuw voor een bepaalde reden opstarten, initieert de Azure-platform een automatische herstelactie om uit te voeren van de defecte host-server uitgeschakeld voor verder onderzoek. 

Alle virtuele machines op die host worden automatisch verplaatst naar een andere, gezonde host-server. Dit proces is meestal voltooid binnen 15 minuten. Zie voor meer informatie over het proces voor automatisch herstel, [automatisch herstel van virtuele machines](https://azure.microsoft.com/blog/service-healing-auto-recovery-of-virtual-machines).

### <a name="unplanned-maintenance"></a>Niet-gepland onderhoud
In uitzonderlijk gevallen moet het team van Azure-bewerkingen mogelijk onderhoudsactiviteiten om te controleren of de algemene status van het Azure-platform uit te voeren. Dit probleem mogelijk invloed op de VM-beschikbaarheid en het meestal resulteert in dezelfde automatisch herstel actie, zoals eerder beschreven.  

Niet-geplande maintenances omvatten het volgende:

- Dringende knooppunt defragmentatie
- Updates van de switch urgente netwerken

### <a name="vm-crashes"></a>VM-crashes
Virtuele machines kunnen opnieuw worden gestart vanwege problemen binnen de virtuele machine zelf. De workload of de rol die wordt uitgevoerd op de virtuele machine mogelijk een bug uit binnen het gastbesturingssysteem activeren. Voor meer informatie over het bepalen van de reden voor het vastlopen, het systeem en toepassingslogboeken voor Windows-VM's en de seriële logboeken voor Linux-VM's te bekijken.

### <a name="storage-related-forced-shutdowns"></a>Geforceerde afsluiten met betrekking tot opslag
Virtuele machines in Azure, is afhankelijk van virtuele schijven voor het besturingssysteem en gegevensopslag die wordt gehost op de infrastructuur van Azure Storage. Wanneer de beschikbaarheid of de verbinding tussen de virtuele machine en de bijbehorende virtuele schijven van meer dan 120 seconden wordt beïnvloed, voert de Azure-platform een geforceerde afsluiting van de virtuele machines om te voorkomen dat gegevens beschadigd raken. De virtuele machines worden automatisch weer ingeschakeld nadat de opslagverbinding is hersteld. 

De duur van het afsluiten kan is zo kort vijf minuten maar aanzienlijk langer. Hier volgt een van de specifieke aanvragen die is gekoppeld aan de geforceerde afsluiten met betrekking tot opslag: 

**I/o van meer dan beperkt**

Virtuele machines mogelijk tijdelijk uitgeschakeld wanneer de i/o-aanvragen zijn consistent beperkt omdat het volume van de i/o-bewerkingen per seconde (IOPS) groter is dan de i/o-limieten voor de schijf. (Standard disk-opslag is beperkt tot 500 IOPS.) Gebruik schijfsegmentering of configureren van de opslagruimte in de Gast-VM, afhankelijk van de werkbelasting voor het oplossen van dit probleem. Zie voor meer informatie, [Azure VM's configureren voor optimale opslagprestaties](http://blogs.msdn.com/b/mast/archive/2014/10/14/configuring-azure-virtual-machines-for-optimal-storage-performance.aspx).

Hogere IOPS-limieten zijn beschikbaar via Azure Premium Storage met maximaal 80.000 IOPS. Zie voor meer informatie, [High-Performance Premium Storage](../windows/premium-storage.md).

### <a name="other-incidents"></a>Andere incidenten
In zeldzame gevallen kan een probleem met de optie in regio's kan invloed hebben op meerdere servers in een Azure-datacenter. Als dit probleem optreedt, wordt in het Azure-team e-mailmeldingen verzendt naar de betrokken abonnementen. U kunt controleren de [Azure Service Health dashboard](https://azure.microsoft.com/status/) en de Azure-portal voor de status van lopende storingen en eerdere incidenten.
