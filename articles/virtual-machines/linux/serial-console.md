---
title: De seriële console van virtuele machine van Azure | Microsoft Docs
description: Bidirectionele seriële console voor Azure virtual machines.
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
ms.openlocfilehash: 69f5e29be77f25d649ce357dae6e3905ab2bf6b8
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
ms.locfileid: "31425330"
---
# <a name="virtual-machine-serial-console-preview"></a>Seriële console van de virtuele machine (preview) 


De seriële console van de virtuele machine in Azure biedt toegang tot een op tekst gebaseerde console voor Linux en Windows virtuele machines. Deze seriële verbinding naar de seriële poort COM1 van de virtuele machine is en biedt toegang tot de virtuele machine en niet zijn gerelateerd aan het virtuele machinenetwerk / systeemstatus besturingssysteem. Toegang tot de seriële console voor een virtuele machine worden uitgevoerd alleen via de Azure-portal momenteel en alleen voor gebruikers die VM Inzender hebben of hoger toegang tot de virtuele machine toegestaan. 

> [!Note] 
> Voorbeelden zijn beschikbaar gemaakt voor u op voorwaarde dat u akkoord met de gebruiksvoorwaarden gaat. Zie voor meer informatie [Microsoft Azure aanvullende gebruiksvoorwaarden voor Microsoft Azure-Previews.] (https://azure.microsoft.com/support/legal/preview-supplemental-terms/) Deze service is momenteel **openbare preview** en toegang tot de seriële console voor virtuele machines is beschikbaar voor globale Azure-regio's. Seriële console is op dit moment niet beschikbaar Azure Government, Duitse Azure en Azure voor China cloud.


## <a name="prerequisites"></a>Vereisten 

* Virtuele machine [opstarten diagnostics](boot-diagnostics.md) ingeschakeld 
* Het account met behulp van de seriële console moet hebben [rol van Inzender](../../role-based-access-control/built-in-roles.md) voor de virtuele machine en de [opstarten diagnostics](boot-diagnostics.md) storage-account. 
* Zie voor specifieke instellingen voor Linux distro, [openen van de seriële console voor Linux](#accessing-serial-console-for-linux)


## <a name="open-the-serial-console"></a>Open de seriële console
seriële console voor virtuele machines is alleen toegankelijk via [Azure-portal](https://portal.azure.com). Hieronder volgen de stappen voor toegang tot de seriële console voor virtuele machines via de portal 

  1. De Azure portal openen
  2. Selecteer de virtuele machines in het menu links.
  3. Klik op de virtuele machine in de lijst. De overzichtspagina van de virtuele machine wordt geopend.
  4. Schuif helemaal naar de ondersteuning + sectie Troubleshooting en klik op de optie van de seriële console (Preview). Een nieuw deelvenster met de seriële console openen en de verbinding wordt gestart.

![](../media/virtual-machines-serial-console/virtual-machine-linux-serial-console-connect.gif)


> [!NOTE] 
> Seriële console vereist een lokale gebruiker met een wachtwoord dat is geconfigureerd. Virtuele machines alleen is geconfigureerd met de openbare SSH-sleutel wordt op dit moment geen toegang tot de seriële console. Ga als volgt om een lokale gebruiker met wachtwoord [VM-extensie voor toegang tot](https://docs.microsoft.com/azure/virtual-machines/linux/using-vmaccess-extension) en lokale gebruiker gemaakt met een wachtwoord.

### <a name="disable-feature"></a>Functie uitschakelen
De functionaliteit van de seriële console kan worden gedeactiveerd voor specifieke virtuele machines door die VM boot diagnostics instelling uit te schakelen.

## <a name="serial-console-security"></a>Beveiliging van de seriële console 

### <a name="access-security"></a>-Beveiliging 
Toegang tot de seriële console is beperkt tot gebruikers die hebben [VM inzenders](../../role-based-access-control/built-in-roles.md#virtual-machine-contributor) of boven de toegang tot de virtuele machine. Als uw AAD-tenant is vereist voor multi-factor Authentication wordt toegang tot de seriële console ook de MFA moet omdat de toegang [Azure-portal](https://portal.azure.com).

### <a name="channel-security"></a>Kanaal-beveiliging
Alle gegevens wordt teruggestuurd en weer op de kabel wordt versleuteld.

### <a name="audit-logs"></a>Controlelogboeken
Alle toegang tot de seriële console momenteel is aangemeld de [opstarten diagnostics](https://docs.microsoft.com/azure/virtual-machines/linux/boot-diagnostics) logboeken van de virtuele machine. Toegang tot deze logboeken zijn het eigendom en beheerd door de beheerder van de virtuele machine van Azure.  

>[!CAUTION] 
Terwijl er geen wachtwoorden voor toegang voor de console bent aangemeld als opdrachten uitvoert in de console bevat of uitvoer van wachtwoorden, geheimen, gebruikersnamen of een andere vorm van persoonsgegevens (PII), worden die naar de virtuele machine opstarten diagnostische gegevens geschreven registreert, samen met alle andere zichtbare tekst, als onderdeel van de implementatie van de seriële console scrollback functionaliteit. Deze logboeken vormen een circulaire en alleen personen met leesmachtigingen voor het opslagaccount voor diagnostische gegevens hebben toegang tot deze, maar het is raadzaam na de aanbevolen procedure van het gebruik van de SSH-console voor alles die u mogelijk geheimen en/of PII. 

### <a name="concurrent-usage"></a>Gelijktijdig gebruik
Als een gebruiker is verbonden met de seriële console en een andere gebruiker is om die dezelfde virtuele machine toegang vraagt, de eerste gebruiker wordt verbroken en de tweede gebruiker aangesloten op een manier die lijkt op de eerste gebruiker permanent en het verlaten van de fysieke-console en een nieuwe de gebruiker zich omlaag.

>[!CAUTION] 
Dit betekent dat de gebruiker die wordt verbroken worden niet geregistreerd af! De mogelijkheid om af te dwingen een afmelden bij het verbreken van de verbinding (via SIGHUP of een vergelijkbaar mechanisme) is nog steeds in het overzicht. Voor Windows-er een time-out van automatische ingeschakeld in SAC is moet echter voor Linux kunt u configureren terminal time-outinstelling. Doen dit gewoon toevoegen `export TMOUT=600` in uw .bash_profile of .profile voor de gebruiker u zich aanmelden in de console, om de time-out voor de sessie na 10 minuten.

### <a name="disable-feature"></a>Functie uitschakelen
De functionaliteit van de seriële console kan worden gedeactiveerd voor specifieke virtuele machines door die VM boot diagnostics instelling uit te schakelen.

## <a name="common-scenarios-for-accessing-serial-console"></a>Algemene scenario's voor toegang tot de seriële console 
Scenario          | Acties in de seriële console                |  OS-toepassing 
:------------------|:-----------------------------------------|:------------------
Verbroken FSTAB-bestand | Voer sleutel Corrigeer fstab-bestand met een teksteditor als u wilt doorgaan. Zie [het fstab-problemen oplossen](https://support.microsoft.com/help/3206699/azure-linux-vm-cannot-start-because-of-fstab-errors) | Linux 
Onjuiste firewall-regels | Toegang tot de seriële console en los iptables of Windows firewall-regels | Linux/Windows 
Bestandssysteem beschadigd/selectievakje | Toegang tot de seriële console en het bestandssysteem herstellen | Linux/Windows 
Problemen met SSH/RDP-configuratie | Toegang tot de seriële console en instellingen wijzigen | Linux/Windows 
Systeem voor het vergrendelen van netwerk| De seriële console toegang via de portal voor het beheren van systeem | Linux/Windows 
Interactie met bootloader | Toegang WORMGATEN/BCD via de seriële console | Linux/Windows 

## <a name="accessing-serial-console-for-linux"></a>Toegang tot de seriële console voor Linux
Seriële console goed te laten functioneren als het gastbesturingssysteem worden geconfigureerd om te lezen en schrijven van consoleberichten naar de seriële poort. De meeste [goedgekeurde Azure Linux-distributies](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros) hebben van de seriële console standaard geconfigureerd. Door te klikken in de portal in de sectie van de seriële console biedt toegang tot de console. 

### <a name="access-for-redhat"></a>Toegang voor RedHat 
RedHat installatiekopieën beschikbaar zijn op Azure hebben toegang tot de console standaard ingeschakeld. Modus voor één gebruiker in Red Hat vereist hoofdgebruiker worden ingeschakeld, is standaard uitgeschakeld. Als u een nodig om in te schakelen van modus voor één gebruiker hebt, gebruikt u de volgende instructies:

1. Aanmelden bij het systeem Red Hat via SSH
2. Wachtwoord voor de hoofdgebruiker inschakelen 
 * `passwd root` (een sterke root-wachtwoord instellen)
3. Zorg ervoor dat de basis-gebruiker kan alleen aanmelden via ttyS0
 * `edit /etc/ssh/sshd_config` en zorg ervoor dat PermitRootLog in is ingesteld op no
 * `edit /etc/securetty file` alleen aanmelden via ttyS0 toestaat 

Nu als het systeem wordt opgestart naar de modus voor één gebruiker u kunt zich aanmelden via hoofdwachtwoord.

U kunt ook RHEL 7.4 + of kunt u het inschakelen van 6,9 + modus voor één gebruiker in de GRUB wordt gevraagd, vindt u instructies [hier](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/5/html/installation_guide/s1-rescuemode-booting-single)

### <a name="access-for-ubuntu"></a>Toegang voor Ubuntu 
Ubuntu installatiekopieën beschikbaar zijn op Azure hebben toegang tot de console standaard ingeschakeld. Als het systeem wordt opgestart naar de modus voor één gebruiker kunt u toegang tot zonder aanvullende referenties. 

### <a name="access-for-coreos"></a>Toegang voor virtuele CoreOS
Virtuele CoreOS installatiekopieën beschikbaar zijn op Azure hebben toegang tot de console standaard ingeschakeld. Als de benodigde systeem kan worden opgestart in de modus voor één gebruiker via WORMGATEN parameters te wijzigen en toevoegen `coreos.autologin=ttyS0` zou core gebruiker logboek en beschikbaar zijn in de seriële console inschakelen. 

### <a name="access-for-suse"></a>Toegang voor SUSE
SLES installatiekopieën beschikbaar zijn op Azure hebben toegang tot de console standaard ingeschakeld. Als u oudere versies van SLES op Azure, volgt u de [KB-artikel](https://www.novell.com/support/kb/doc.php?id=3456486) seriële console inschakelen. Nieuwere afbeeldingen van SLES 12 SP3 + kunt ook toegang via de seriële console als het systeem wordt opgestart in de noodherstelmodus bevindt.

### <a name="access-for-centos"></a>Toegang voor CentOS
CentOS installatiekopieën beschikbaar zijn op Azure hebben toegang tot de console standaard ingeschakeld. Volg de instructies die vergelijkbaar is met Red Hat installatiekopieën hierboven voor de modus voor één gebruiker. 

### <a name="access-for-oracle-linux"></a>Toegang voor Oracle Linux
Oracle Linux installatiekopieën beschikbaar zijn op Azure hebben toegang tot de console standaard ingeschakeld. Volg de instructies die vergelijkbaar is met Red Hat installatiekopieën hierboven voor de modus voor één gebruiker.

### <a name="access-for-custom-linux-image"></a>Toegang voor de aangepaste afbeelding van Linux
Schakel toegang tot de console in /etc/inittab voor het uitvoeren van een terminal op ttyS0 zodat de seriële console voor uw aangepaste Linux VM-installatiekopie. Hieronder volgt een voorbeeld is dit in het bestand inittab toevoegen 

`S0:12345:respawn:/sbin/agetty -L 115200 console vt102` 

## <a name="errors"></a>Fouten
De meeste fouten zijn tijdelijke aard en adres verbinding deze opnieuw. Onderstaande tabel bevat een overzicht van de fouten en risicobeperking 

Fout                            |   Oplossing 
:---------------------------------|:--------------------------------------------|
Kan niet worden opgehaald van de opstartinstallatiekopie de diagnostische instellingen voor '<VMNAME>'. Zorg ervoor dat diagnostische gegevens voor opstarten is ingeschakeld voor deze virtuele machine voor het gebruik van de seriële console. | Zorg ervoor dat de virtuele machine heeft [opstarten diagnostics](boot-diagnostics.md) ingeschakeld. 
De virtuele machine is gestopt toewijzing ongedaan is gemaakt. Start de virtuele machine en probeer de seriële console-verbinding. | Virtuele machine moet zijn in een gestarte staat toegang tot de seriële console
U hebt niet de vereiste machtigingen voor deze virtuele machine de seriële console gebruiken. Controleer of er ten minste VM-rol bijdragerrechten.| Toegang tot de seriële console nodig bepaalde machtiging voor toegang tot. Zie [toegangsvereisten](#prerequisites) voor meer informatie
Kan niet bepalen van de resourcegroep voor het opslagaccount voor diagnostische gegevens van opstarten '<STORAGEACCOUNTNAME>'. Controleer of u diagnostische gegevens over opstarten is ingeschakeld voor deze virtuele machine en u toegang hebt tot dit opslagaccount. | Toegang tot de seriële console nodig bepaalde machtiging voor toegang tot. Zie [toegangsvereisten](#prerequisites) voor meer informatie

## <a name="known-issues"></a>Bekende problemen 
We zijn nog steeds in de preview-stadia voor toegang tot de seriële console, we werken via een aantal bekende problemen, onder de lijst is van deze voor mogelijke oplossingen 

Probleem                           |   Oplossing 
:---------------------------------|:--------------------------------------------|
Er is geen optie met de virtuele machine scale set exemplaar seriële console |  Toegang tot de seriële console voor exemplaren van virtuele machine scale set wordt niet ondersteund op het moment van preview.
Roept invoeren nadat de banner verbinding wordt niet weergegeven voor een logboek in prompt | [Roept invoeren, gebeurt er niets](https://github.com/Microsoft/azserialconsole/blob/master/Known_Issues/Hitting_enter_does_nothing.md)


## <a name="frequently-asked-questions"></a>Veelgestelde vragen 
**Q. Hoe kan ik feedback verzenden?**

A. Feedback geven als het probleem door te gaan naar https://aka.ms/serialconsolefeedback. U kunt ook minder (voorkeur) feedback verzenden via azserialhelp@microsoft.com of in de categorie van de virtuele machine van http://feedback.azure.com

**Q.I verschijnt het foutbericht 'de bestaande console heeft conflicterende BS-type 'Windows' met het gevraagde type besturingssysteem van Linux?**

A. Dit is een bekend probleem te verhelpen, opent [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) in bash-modus en probeer het opnieuw.

**Q. Ik kan geen toegang tot de seriële console, waar kan ik een ondersteuningsaanvraag bestand?**

A. Deze preview-functie wordt via Azure Preview-voorwaarden voldaan. Ondersteuning voor deze het beste oplossen via kanalen hierboven vermeld. 

## <a name="next-steps"></a>Volgende stappen
* De seriële console van het is ook beschikbaar voor [Windows](../windows/serial-console.md) virtuele machines
* Meer informatie over [bootdiagnostics](boot-diagnostics.md)