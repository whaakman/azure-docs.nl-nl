---
title: Seriële console van virtuele Azure-machine | Microsoft Docs
description: Bidirectionele seriële console voor virtuele machines van Azure.
services: virtual-machines-linux
documentationcenter: ''
author: harijay
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 03/05/2018
ms.author: harijay
ms.openlocfilehash: 00a776131321500386b507f45ea84817b08147f7
ms.sourcegitcommit: ab3b2482704758ed13cccafcf24345e833ceaff3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/06/2018
ms.locfileid: "37867858"
---
# <a name="virtual-machine-serial-console-preview"></a>Seriële console van virtuele machine (preview) 


De seriële console van de virtuele machine op Azure biedt toegang tot een op tekst gebaseerde console voor Linux en Windows virtuele machines. Deze seriële verbinding is op de seriële poort COM1 van de virtuele machine en biedt toegang tot de virtuele machine en niet zijn gerelateerd aan virtuele machinenetwerk / operationele status van het systeem. Toegang tot de seriële console voor een virtuele machine kan worden uitgevoerd op dit moment alleen via Azure portal en toegestaan alleen voor gebruikers die VM-Inzender hebben of hoger toegang tot de virtuele machine. 

> [!Note] 
> Previews worden voor u beschikbaar gesteld op voorwaarde dat u akkoord met de gebruiksvoorwaarden van gaat is. Zie voor meer informatie [Microsoft Azure aanvullende gebruiksvoorwaarden voor Microsoft Azure-Previews.] (https://azure.microsoft.com/support/legal/preview-supplemental-terms/) Deze service is momenteel **preview-versie** en toegang tot de seriële console voor virtuele machines is beschikbaar voor globale Azure-regio's. Seriële console is op dit moment niet beschikbaar Azure Government, Azure Duitsland en Azure China-cloud.


## <a name="prerequisites"></a>Vereisten 

* U moet de resource Manager-implementatiemodel gebruiken. Klassieke implementaties worden niet ondersteund. 
* Virtuele machine moet zijn [diagnostische gegevens over opstarten](boot-diagnostics.md) ingeschakeld 
* Het account met behulp van de seriële console moet hebben [rol van Inzender](../../role-based-access-control/built-in-roles.md) voor virtuele machine en de [diagnostische gegevens over opstarten](boot-diagnostics.md) storage-account. 
* Zie voor specifieke instellingen voor Linux-distributie, [toegang tot de seriële console voor Linux](#accessing-serial-console-for-linux)


## <a name="open-the-serial-console"></a>Open de seriële console
seriële console voor virtuele machines is alleen toegankelijk via [Azure-portal](https://portal.azure.com). Hieronder volgen de stappen voor toegang tot de seriële console voor virtuele machines via de portal 

  1. De Azure-portal openen
  2. Selecteer de virtuele machines in het menu links.
  3. Klik op de virtuele machine in de lijst. De overzichtspagina voor de virtuele machine wordt geopend.
  4. Schuif omlaag naar de ondersteuning en probleemoplossing sectie en klik op de optie seriële console (Preview). Een nieuw deelvenster met de seriële console opent en start de verbinding.

![](../media/virtual-machines-serial-console/virtual-machine-linux-serial-console-connect.gif)


> [!NOTE] 
> Seriële console vereist een lokale gebruiker met een wachtwoord dat is geconfigureerd. VM's alleen worden geconfigureerd met openbare SSH-sleutel wordt op dit moment geen toegang tot de seriële console. Voor het maken van een lokale gebruiker met wachtwoord, gaat u als volgt [VM-extensie voor toegang](https://docs.microsoft.com/azure/virtual-machines/linux/using-vmaccess-extension) en lokale gebruiker met wachtwoord maken.

### <a name="disable-feature"></a>Functie uitschakelen
De seriële console-functionaliteit, kan door het uitschakelen van deze virtuele machine opstarten diagnostische instelling voor specifieke virtuele machines worden gedeactiveerd.

## <a name="serial-console-security"></a>Seriële console-beveiliging 

### <a name="access-security"></a>Beveiliging voor toegang 
Toegang tot de seriële console is beperkt tot gebruikers die hebben [VM inzenders](../../role-based-access-control/built-in-roles.md#virtual-machine-contributor) of hoger dan toegang tot de virtuele machine. Als uw AAD-tenant meervoudige verificatie vereist en toegang tot de seriële console ook de MFA moet omdat de toegang [Azure-portal](https://portal.azure.com).

### <a name="channel-security"></a>Beveiliging van het kanaal
Alle gegevens weer wordt verzonden en weer worden versleuteld op de kabel.

### <a name="audit-logs"></a>Controlelogboeken
Alle toegang tot de seriële console momenteel is aangemeld de [diagnostische gegevens over opstarten](https://docs.microsoft.com/azure/virtual-machines/linux/boot-diagnostics) logboeken van de virtuele machine. Toegang tot deze logboeken zijn eigendom van en beheerd door de beheerder van de virtuele machine van Azure.  

>[!CAUTION] 
Terwijl er geen wachtwoorden voor toegang voor de console bent aangemeld als opdrachten worden uitgevoerd binnen de console bevat of uitvoer van wachtwoorden, geheimen, gebruikersnamen of enige andere vorm van persoonlijk identificeerbare informatie (PII), worden die geschreven naar de diagnostische gegevens over opstarten van virtuele machine registreert, samen met alle andere zichtbare tekst, als onderdeel van de implementatie van de seriële console scrollback functionaliteit. Deze logboeken zijn circulaire en alleen personen met leesmachtigingen voor het opslagaccount voor diagnostische gegevens hebben de toegang tot, maar we raden aan na de aanbevolen procedure van het gebruik van de SSH-console voor alles wat die hebben mogelijk betrekking op geheimen en/of PII. 

### <a name="concurrent-usage"></a>Gelijktijdig gebruik
Als een gebruiker is verbonden met de seriële console en een andere gebruiker is toegang tot deze virtuele machine met dezelfde aanvraagt, wordt de eerste gebruiker verbroken en wordt de tweede gebruiker op een manier die lijkt op de eerste gebruiker permanent van en het verlaten van de fysieke-console en een nieuwe verbinding de gebruiker zich omlaag.

>[!CAUTION] 
Dit betekent dat de gebruiker die wordt verbroken wordt niet afgemeld. De mogelijkheid om af te dwingen een afmelden bij het verbreken van de verbinding (via SIGHUP of een vergelijkbaar mechanisme) is nog steeds in de roadmap. Voor Windows er een automatische time-out in SAC ingeschakeld is, maar voor Linux kunt u configureren terminal time-outinstelling. Doet dit gewoon toevoegen `export TMOUT=600` in uw .bash_profile of .profile voor de gebruiker die u in de console, time-out voor de sessie 10 minuten na de aanmelding.

### <a name="disable-feature"></a>Functie uitschakelen
De seriële console-functionaliteit, kan door het uitschakelen van deze virtuele machine opstarten diagnostische instelling voor specifieke virtuele machines worden gedeactiveerd.

## <a name="common-scenarios-for-accessing-serial-console"></a>Algemene scenario's voor toegang tot de seriële console 
Scenario          | Acties in de seriële console                |  OS-toepassing 
:------------------|:-----------------------------------------|:------------------
Verbroken FSTAB-bestand | Voer-toets om door te gaan en op te lossen van fstab-bestand met een teksteditor. Zie [voor het oplossen van problemen met fstab](https://support.microsoft.com/help/3206699/azure-linux-vm-cannot-start-because-of-fstab-errors) | Linux 
Onjuiste firewall-regels | Toegang tot de seriële console en op te lossen van iptables of Windows firewall-regels | Linux/Windows 
Bestandssysteem beschadigd/selectievakje | Toegang tot de seriële console en het bestandssysteem herstellen | Linux/Windows 
Problemen met SSH/RDP-configuratie | Toegang tot de seriële console en -instellingen wijzigen | Linux/Windows 
Systeem voor het vergrendelen van netwerk| Seriële console van de toegang via de portal voor het beheren van systeem | Linux/Windows 
Interactie met de bootloader | Toegang tot WORMGATEN/BCD via de seriële console | Linux/Windows 

## <a name="accessing-serial-console-for-linux"></a>Toegang tot de seriële console voor Linux
In de volgorde voor de seriële console te laten functioneren, moet het gastbesturingssysteem worden geconfigureerd om te lezen en schrijven van consoleberichten naar de seriële poort. De meeste [Azure Linux-distributies die zijn goedgekeurd](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros) hebben van de seriële console standaard geconfigureerd. Eenvoudig te klikken in de portal op de seriële console-sectie biedt toegang tot de console. 

### <a name="access-for-red-hat"></a>Toegang voor Red Hat 
Red Hat-afbeeldingen die beschikbaar zijn op Azure hebt toegang tot de console standaard ingeschakeld. Modus voor één gebruiker in Red Hat vereist hoofdgebruiker zijn ingeschakeld, die standaard is uitgeschakeld. Als u een noodzaak voor het inschakelen van de modus voor één gebruiker hebt, gebruikt u de volgende instructies:

1. Meld u aan bij het Red Hat-systeem via SSH
2. Wachtwoord voor de hoofdgebruiker inschakelen 
 * `passwd root` (een sterke root-wachtwoord instellen)
3. Controleer of hoofdgebruiker kunt alleen aanmelden via ttyS0
 * `edit /etc/ssh/sshd_config` en zorg ervoor dat PermitRootLog in is ingesteld op no
 * `edit /etc/securetty file` om toe te staan alleen aanmelden via ttyS0 

Nu als het systeem wordt opgestart naar de modus voor één gebruiker kunt u aanmelden via hoofdwachtwoord.

U kunt ook voor RHEL 7.4 + of u kunt inschakelen voor 6,9 + modus voor één gebruiker in de GRUB wordt gevraagd, Zie instructies [hier](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/5/html/installation_guide/s1-rescuemode-booting-single)

### <a name="access-for-ubuntu"></a>Toegang voor Ubuntu 
Ubuntu-installatiekopieën die beschikbaar zijn op Azure hebt toegang tot de console standaard ingeschakeld. Als het systeem wordt opgestart naar de modus voor één gebruiker kunt u openen zonder aanvullende referenties. 

### <a name="access-for-coreos"></a>Toegang voor CoreOS
CoreOS-installatiekopieën die beschikbaar zijn op Azure hebt toegang tot de console standaard ingeschakeld. Als de benodigde systeem kan worden opgestart in de modus voor één gebruiker via het wijzigen van WORMGATEN parameters en toe te voegen `coreos.autologin=ttyS0` core gebruiker voor aanmelding en beschikbaar in de seriële console wilt inschakelen. 

### <a name="access-for-suse"></a>Toegang voor SUSE
SLES-installatiekopieën die beschikbaar zijn op Azure hebt toegang tot de console standaard ingeschakeld. Als u oudere versies van SLES op Azure gebruikt, volgt u de [KB-artikel](https://www.novell.com/support/kb/doc.php?id=3456486) seriële console inschakelen. Nieuwere afbeeldingen van SLES 12 SP3 + kunnen ook toegang via de seriële console als het systeem wordt opgestart in de noodherstelmodus bevindt.

### <a name="access-for-centos"></a>Toegang voor CentOS
CentOS-installatiekopieën die beschikbaar zijn op Azure hebt toegang tot de console standaard ingeschakeld. Volg de instructies die vergelijkbaar met de bovenstaande Red Hat-afbeeldingen voor de modus voor één gebruiker. 

### <a name="access-for-oracle-linux"></a>Toegang voor Oracle Linux
Oracle Linux-installatiekopieën die beschikbaar zijn op Azure hebt toegang tot de console standaard ingeschakeld. Volg de instructies die vergelijkbaar met de bovenstaande Red Hat-afbeeldingen voor de modus voor één gebruiker.

### <a name="access-for-custom-linux-image"></a>Toegang voor de aangepaste installatiekopie van Linux
Om in te schakelen voor uw aangepaste Linux-VM-installatiekopie de seriële console, schakel toegang tot de console in /etc/inittab om uit te voeren van een terminal op ttyS0. Hieronder volgt een voorbeeld om toe te voegen deze in het bestand inittab 

`S0:12345:respawn:/sbin/agetty -L 115200 console vt102` 

## <a name="errors"></a>Fouten
De meeste fouten zijn tijdelijke aard en het adres van de verbinding opnieuw wordt geprobeerd deze. Onderstaande tabel geeft een lijst van fouten en risicobeperking 

Fout                            |   Oplossing 
:---------------------------------|:--------------------------------------------|
Kan niet worden opgehaald van de instellingen voor diagnostische gegevens over opstarten voor '<VMNAME>'. Voor het gebruik van de seriële console of dat de diagnostische gegevens over die opstarten is ingeschakeld voor deze virtuele machine. | Zorg ervoor dat de virtuele machine heeft [diagnostische gegevens over opstarten](boot-diagnostics.md) ingeschakeld. 
De virtuele machine is gestopt toewijzing ongedaan gemaakt. Start de virtuele machine en probeer de seriële console-verbinding. | Virtuele machine moet zich in een status gestart voor toegang tot de seriële console
U hebt niet de vereiste machtigingen voor deze virtuele machine de seriële console gebruiken. Zorg ervoor dat u ten minste beschikken over machtigingen van de rol Inzender voor virtuele machine.| Toegang tot de seriële console moet bepaalde machtiging voor toegang tot. Zie [vereisten](#prerequisites) voor meer informatie
Kan niet bepalen van de resourcegroep voor het opslagaccount van de diagnostische gegevens over opstarten '<STORAGEACCOUNTNAME>'. Controleer of u diagnostische gegevens over opstarten is ingeschakeld voor deze virtuele machine en u toegang hebt tot dit opslagaccount wordt gebruikt. | Toegang tot de seriële console moet bepaalde machtiging voor toegang tot. Zie [vereisten](#prerequisites) voor meer informatie

## <a name="known-issues"></a>Bekende problemen 
Aangezien we nog steeds in de previewfase voor toegang tot de seriële console, we werken via een aantal bekende problemen, hieronder ziet u de lijst met mogelijke oplossingen 

Probleem                           |   Oplossing 
:---------------------------------|:--------------------------------------------|
Er bestaat geen optie met virtual machine scale set exemplaar seriële console |  Toegang tot de seriële console voor schaalsetinstanties virtuele machine wordt niet ondersteund op het moment van de Preview-versie.
Nadat de banner van de verbinding wordt niet weergegeven voor een logboek in de prompt te maken met invoeren | [Doet niets te maken met invoeren](https://github.com/Microsoft/azserialconsole/blob/master/Known_Issues/Hitting_enter_does_nothing.md)


## <a name="frequently-asked-questions"></a>Veelgestelde vragen 
**Q. Hoe kan ik feedback verzenden?**

A. Feedback geven als het probleem door te gaan naar https://aka.ms/serialconsolefeedback. U kunt ook minder (aanbevolen) feedback verzenden via azserialhelp@microsoft.com of in de categorie van de virtuele machine van de http://feedback.azure.com

**Q.I krijg de foutmelding 'bestaande console heeft conflicterende OS type 'Windows' met het aangevraagde besturingssysteemtype van Linux?**

A. Dit is een bekend probleem op te lossen, opent [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) in bash-modus en probeer het opnieuw.

**Q. Ik ben geen toegang krijgen tot de seriële console, waar kan ik een ondersteuningsaanvraag bestand?**

A. Deze preview-functie wordt behandeld via Azure Preview-voorwaarden. Ondersteuning voor dit is het beste worden opgelost via de kanalen die hierboven worden vermeld. 

## <a name="next-steps"></a>Volgende stappen
* Seriële console van het is ook beschikbaar voor [Windows](../windows/serial-console.md) VM's
* Meer informatie over [bootdiagnostics](boot-diagnostics.md)