---
title: Azure-seriële Console voor het WORMGATEN en de modus voor één gebruiker | Microsoft Docs
description: Met behulp van de seriële Console van wormgaten in virtuele machines van Azure.
services: virtual-machines-linux
documentationcenter: ''
author: alsin
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 08/14/2018
ms.author: alsin
ms.openlocfilehash: 54b5fb4ace64cc304a8eb8ce6d762d33ae18619a
ms.sourcegitcommit: d2f2356d8fe7845860b6cf6b6545f2a5036a3dd6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/16/2018
ms.locfileid: "40177593"
---
# <a name="use-serial-console-for-sysrq-and-nmi-calls"></a>Seriële Console gebruiken voor SysRq en NMI aanroepen

## <a name="system-request-sysrq"></a>Systeem-aanvraag (SysRq)
Een SysRq is een opeenvolging van sleutels die worden geïnterpreteerd door de bewerking system kernel Linux, die een set vooraf gedefinieerde acties kan activeren. Deze opdrachten worden vaak gebruikt bij het oplossen van de virtuele machine of herstel kan niet worden uitgevoerd via een traditioneel beheer (bijvoorbeeld, als de virtuele machine is vastgelopen). Met de functie SysRq van de seriële Console van Azure nabootsen drukken op de SysRq sleutel en de tekens die zijn ingevoerd op een fysieke toetsenbord.

Wanneer de reeks SysRq wordt geleverd, wordt de configuratie van de kernel bepalen hoe het systeem reageert. Zie voor meer informatie over het inschakelen en uitschakelen van SysRq de *beheerdershandleiding voor de SysRq* [tekst](https://aka.ms/kernelorgsysreqdoc) | [markdown](https://aka.ms/linuxsysrq).  

De seriële Console van Azure kan worden gebruikt voor het verzenden van een SysRq met een Azure-machine via het toetsenbordpictogram in de opdrachtbalk die hieronder wordt weergegeven.

![](../media/virtual-machines-serial-console/virtual-machine-serial-console-command-menu.jpg)

'Verzenden SysRq opdracht' kiest, wordt een dialoogvenster dat wordt bieden algemene SysRq opties of accepteren van een reeks SysRq opdrachten ingevoerd in het dialoogvenster geopend.  Hiermee wordt voor de reeks SysRq's een bewerking op hoog niveau zoals het gebruik van een veilige opnieuw opstarten uit te voeren: `REISUB`.

![](../media/virtual-machines-serial-console/virtual-machine-serial-console-sysreq_UI.png)

De opdracht SysRq kan niet worden gebruikt op virtuele machines die zijn gestopt of waarvan kernel bevindt zich in een niet-reagerend status. (bijvoorbeeld een kernelprobleem).

### <a name="enable-sysrq"></a>SysRq inschakelen 
Zoals beschreven in de *beheerdershandleiding voor de SysRq* hierboven SysRq kan worden geconfigureerd dat alle, geen of alleen bepaalde opdrachten zijn beschikbaar. Kunt u alle SysRq opdrachten met behulp van de onderstaande stappen, maar deze niet blijven van kracht na opnieuw opstarten:
```
echo "1" >/proc/sys/kernel/sysrq
```
Als u de configuratie van de SysReq permanente, u kunt als volgt als u wilt alle SysRq opdrachten inschakelen
1. Deze regel aan toe te voegen */etc/sysctl.conf* <br>
    `kernel.sysrq = 1`
1. Opnieuw opstarten of sysctl bijwerken door te voeren <br>
    `sysctl -p`

### <a name="command-keys"></a>Opdracht sleutels 
In de beheerdershandleiding SysRq hierboven is:

|Opdracht| Functie
| ------| ----------- |
|``b``  |   Wordt onmiddellijk het systeem opnieuw opstarten zonder synchroniseren of de schijven ontkoppelen.
|``c``  |   Een systeem vastlopen wordt uitgevoerd door een NULL-aanwijzer voor referentie ongedaan maken. Een crashdump worden uitgevoerd als geconfigureerd.
|``d``  |   Geeft alle vergrendelingen die zijn ingesteld.
|``e``  |   Een SIGTERM verzenden naar alle processen, met uitzondering van init.
|``f``  |   De killer oom als u wilt een geheugen hog proces beëindigen wordt gebeld, maar kan geen paniek als er niets kan worden beëindigd.
|``g``  |   Gebruikt door kgdb (kernelfoutopsporingsprogramma)
|``h``  |   Help-informatie wordt weergegeven (een andere sleutel dan de hier vermelde gegevens worden ook weergegeven voor Help-informatie, maar ``h`` gemakkelijk te onthouden :-)
|``i``  |    Een SIGKILL verzenden naar alle processen, met uitzondering van init.
|``j``  |    Geforceerd 'Net ontdooien deze' - bestandssystemen geblokkeerd met de ioctl FIFREEZE.
|``k``  |    Veilige toegang sleutel (SAK) beëindigt alle programma's op de huidige virtuele-console. Opmerking: Belangrijke opmerkingen hieronder in de sectie SAK zien.
|``l``  |    Toont een stack-backtrace voor alle actieve CPU's.
|``m``  |    Huidige geheugen informatie die u kunt de console zal worden dump.
|``n``  |    Gebruikt voor het RT taken goed te kunnen maken
|``o``  |    Uit het systeem wordt afgesloten (indien geconfigureerd en wordt ondersteund).
|``p``  |    De huidige wordt geregistreerd en de vlaggen voor de console zal worden dump.
|``q``  |    Dump wordt per CPU-lijsten met alle gewapend hrtimers (maar niet regelmatig timer_list timers) en gedetailleerde informatie weer over alle clockevent-apparaten.
|``r``  |    Schakelt toetsenbord onbewerkte modus en wordt dit XLATE ingesteld.
|``s``  |    Probeert te synchroniseren van alle gekoppelde bestandssystemen.
|``t``  |    Wordt een lijst met huidige taken en hun gegevens aan uw console dump.
|``u``  |    Probeert te koppelen van alle gekoppelde bestandssystemen alleen-lezen.
|``v``  |    Geforceerd worden hersteld framebuffer-console
|``v``  |    Zorgt ervoor dat ETM buffer dump [ARM-specifieke]
|``w``  |    Dumpbestanden voor foutopsporing taken die ononderbroken (geblokkeerd) status hebben.
|``x``  |    Gebruikt door de interface xmon op ppc/powerpc platforms. Globale PMU registreert op sparc64 weergeven. Alle TLB vermeldingen op MIPS dump.
|``y``  |    Algemene CPU-Registers [SPARC-64-specifieke] weergeven
|``z``  |    De buffer ftrace dump
|``0``-``9`` | Hiermee stelt u de console logboek-niveau, bepalen welke kernel-berichten worden afgedrukt aan de console. (``0``voor voorbeeld zou ervoor zorgen dat alleen noodgevallen berichten, zoals PANICs of OOPSes deze aan uw console zouden.)

### <a name="distribution-specific-documentation"></a>Distributie-specifieke documentatie ###
Voor distributie-specifieke documentatie over SysRq en stappen voor het configureren van Linux te maken van een crashdump wanneer deze een opdracht SysRq "Vastloopt" ontvangt, Zie de onderstaande koppelingen:
#### <a name="ubuntu"></a>Ubuntu ####
 - [Kernel-crashdump](https://help.ubuntu.com/lts/serverguide/kernel-crash-dump.html)
#### <a name="red-hat"></a>Red Hat ####
- [Wat is de faciliteit SysRq en hoe gebruik ik dit?](https://access.redhat.com/articles/231663)
- [Het gebruik van de faciliteit SysRq voor het verzamelen van gegevens van een RHEL-server](https://access.redhat.com/solutions/2023)
#### <a name="suse"></a>SUSE ####
- [Configureren van de kernel dump van core vastleggen](https://www.suse.com/support/kb/doc/?id=3374462)
#### <a name="coreos"></a>CoreOS ####
- [Verzamelen crash-Logboeken](https://coreos.com/os/docs/latest/collecting-crash-logs.html)

## <a name="non-maskable-interrupt-nmi"></a>Niet te maskeren Interrupt (NMI) 
Een niet-maskeren interrupt (NMI) is ontworpen voor het maken van een signaal dat software op een virtuele machine worden niet genegeerd. In het verleden zijn NMIs gebruikt om te controleren op hardwareproblemen op systemen die specifieke reactietijden vereist.  NMI vandaag, programmeurs en systeembeheerders vaak gebruikt als een mechanisme voor foutopsporing of problemen met systemen die zijn vastgelopen.

De seriële Console kan worden gebruikt voor het verzenden van een NMI met een Azure-machine via het toetsenbordpictogram in de opdrachtbalk die hieronder wordt weergegeven. Zodra de NMI wordt geleverd, worden de virtuele-machineconfiguratie bepalen hoe het systeem reageert.  Linux-besturingssystemen kunnen worden geconfigureerd voor crashes en het besturingssysteem van een geheugendump maken ontvangt een NMI.

![](../media/virtual-machines-serial-console/virtual-machine-serial-console-command-menu.jpg) <br>

Voor Linux-systemen die ondersteuning bieden voor sysctl voor het configureren van de kernel-parameters, kunt u een paniek inschakelen bij de ontvangst van deze NMI met behulp van de volgende:
1. Deze regel aan toe te voegen */etc/sysctl.conf* <br>
    `kernel.panic_on_unrecovered_nmi=1`
1. Opnieuw opstarten of sysctl bijwerken door te voeren <br>
    `sysctl -p`

Voor meer informatie over Linux-kernel-configuraties, met inbegrip van `unknown_nmi_panic`, `panic_on_io_nmi`, en `panic_on_unrecovered_nmi`, Zie: [documentatie voor/proc/sys/kernel / *](https://www.kernel.org/doc/Documentation/sysctl/kernel.txt). Voor distributie-specifieke documentatie over NMI en stappen voor het configureren van Linux te maken van een crashdump wanneer deze een NMI ontvangt, Zie de onderstaande koppelingen:
 
 ### <a name="ubuntu"></a>Ubuntu 
 - [Kernel-crashdump](https://help.ubuntu.com/lts/serverguide/kernel-crash-dump.html)

 ### <a name="red-hat"></a>Red Hat 
 - [Wat is er een NMI en wat kan ik deze voor gebruiken?](https://access.redhat.com/solutions/4127)
 - [Hoe kan ik mijn systeem vastlopen wanneer NMI switch wordt gepusht configureren?](https://access.redhat.com/solutions/125103)
 - [Beheerdershandleiding voor de crash Dump](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/pdf/kernel_crash_dump_guide/kernel-crash-dump-guide.pdf)

### <a name="suse"></a>SUSE 
- [Configureren van de kernel dump van core vastleggen](https://www.suse.com/support/kb/doc/?id=3374462)
### <a name="coreos"></a>CoreOS 
- [Verzamelen crash-Logboeken](https://coreos.com/os/docs/latest/collecting-crash-logs.html)

## <a name="next-steps"></a>Volgende stappen
* De hoofdpagina van de seriële Console-Linux-documentatie bevindt zich [hier](serial-console.md).
* Seriële Console gebruiken om op te starten in [van WORMGATEN en voer de modus voor één gebruiker](serial-console-grub-single-user-mode.md)
* Seriële Console van het is ook beschikbaar voor [Windows](../windows/serial-console.md) VM's
* Meer informatie over [diagnostische gegevens over opstarten](boot-diagnostics.md)