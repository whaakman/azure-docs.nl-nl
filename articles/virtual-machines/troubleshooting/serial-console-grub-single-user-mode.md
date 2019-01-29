---
title: Azure-seriële Console voor het WORMGATEN en de modus voor één gebruiker | Microsoft Docs
description: Met behulp van de seriële Console van wormgaten in virtuele machines van Azure.
services: virtual-machines-linux
documentationcenter: ''
author: asinn826
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
ms.openlocfilehash: 5029365e665ce3ee9ba65886a3d6d5bbced0ed9a
ms.sourcegitcommit: eecd816953c55df1671ffcf716cf975ba1b12e6b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/28/2019
ms.locfileid: "55103306"
---
# <a name="use-serial-console-to-access-grub-and-single-user-mode"></a>Seriële Console gebruiken voor toegang tot WORMGATEN en de modus voor één gebruiker
WORMGATEN is het eindtotaal Unified Bootloader, dit waarschijnlijk het eerste wat dat u ziet is wanneer u een virtuele machine wordt opgestart. Omdat deze wordt weergegeven voordat het besturingssysteem is gestart, is het niet toegankelijk via SSH. Vanaf WORMGATEN, u kunt wijzigen van de opstartconfiguratie om op te starten in de modus voor één gebruiker, onder andere.

Modus voor één gebruiker is een minimale omgeving met een minimale functionaliteit. Kan het nuttig zijn voor het onderzoeken van opstartproblemen bij, bestandssysteem problemen of netwerkproblemen. Minder services kunnen worden uitgevoerd op de achtergrond en, afhankelijk van de (uitvoeringsniveau), een bestandssysteem kan niet ook worden automatisch gekoppeld.

Modus voor één gebruiker is ook handig in situaties waar uw virtuele machine kan alleen worden geconfigureerd om te accepteren van SSH-sleutels om aan te melden. In dit geval wordt u mogelijk modus voor één gebruiker gebruiken om te maken van een account met wachtwoordverificatie. Houd er rekening mee dat de seriële console-service alleen gebruikers met toegang op Inzender of hoger kunnen voor toegang tot de seriële console van een virtuele machine.

Om in te voeren modus voor één gebruiker, moet u WORMGATEN invoeren wanneer uw virtuele machine wordt opgestart, en de configuratie van de opstartinstallatiekopie in WORMGATEN wijzigen. Gedetailleerde instructies voor het invoeren van WORMGATEN staan hieronder. In het algemeen kunt u de knop opnieuw opstarten in de seriële console van de virtuele machine opnieuw opstarten van uw virtuele machine en WORMGATEN weergeven als uw virtuele machine is geconfigureerd om weer te geven van WORMGATEN.

![Knop Linux seriële Console starten](./media/virtual-machines-serial-console/virtual-machine-serial-console-restart-button-bar.png)

## <a name="general-grub-access"></a>Algemene GRUB-toegang
Voor toegang tot WORMGATEN, moet u opnieuw opstarten van uw virtuele machine terwijl de seriële console-blade geopend. Bepaalde distributies moeten toetsenbordinvoer om weer te geven van WORMGATEN, terwijl anderen automatisch worden WORMGATEN voor een paar seconden weergeven en toetsenbord gebruikersinvoer om te annuleren de time-out toestaan.

U wordt om ervoor te zorgen dat WORMGATEN is ingeschakeld op de virtuele machine om te kunnen toegangsmodus voor één gebruiker. Afhankelijk van uw distributie worden sommige work instellen om ervoor te zorgen dat de GRUB is ingeschakeld. Distributie-specifieke informatie vindt u hieronder en op [deze koppeling](https://blogs.msdn.microsoft.com/linuxonazure/2018/10/23/why-proactively-ensuring-you-have-access-to-grub-and-sysrq-in-your-linux-vm-could-save-you-lots-of-down-time/).

### <a name="restart-your-vm-to-access-grub-in-serial-console"></a>Opnieuw opstarten van uw virtuele machine voor toegang tot WORMGATEN in seriële Console
U kunt uw virtuele machine binnen de seriële console opnieuw door te navigeren naar de / uit-knop en te klikken op 'Virtuele machine opnieuw starten'. Er wordt nu een virtuele machine opnieuw opstarten en u ziet een melding in Azure portal met betrekking tot het opnieuw opstarten.
Opnieuw opstarten van uw virtuele machine kan ook worden uitgevoerd met een SysRq `'b'` als de opdracht [SysRq](./serial-console-nmi-sysrq.md) is ingeschakeld. Volg de instructies distributie-specifieke hieronder voor meer informatie over wat u kunt verwachten van WORMGATEN wanneer u de computer opnieuw opstart.

![Linux-seriële Console opnieuw opstarten](./media/virtual-machines-serial-console/virtual-machine-serial-console-restart-button-ubuntu.gif)

## <a name="general-single-user-mode-access"></a>Algemene modus voor één gebruiker toegang
Handmatige toegang tot de modus voor één gebruiker is mogelijk in situaties waar u niet een account hebt geconfigureerd met wachtwoordverificatie nodig. U moet de WORMGATEN configuratie handmatig invoeren modus voor één gebruiker. Nadat u dit hebt gedaan, gaat u naar [gebruikersmodus voor één gebruiker opnieuw instellen of het toevoegen van een wachtwoord](#-Use-Single-User-Mode-to-reset-or-add-a-password) voor verdere instructies.

In gevallen waarin de virtuele machine kan niet op te starten, worden distributies vaak automatisch verwijderen u in de modus voor één gebruiker of noodherstelmodus bevindt. Andere, nodig aanvullende instellingen echter voordat ze u in de modus voor één gebruiker of noodgevallen automatisch (zoals het instellen van een hoofdwachtwoord) kunnen verwijderen.

### <a name="use-single-user-mode-to-reset-or-add-a-password"></a>Modus voor één gebruiker opnieuw instellen of het toevoegen van een wachtwoord gebruiken
Wanneer u in de modus voor één gebruiker bent, kunt u het volgende als u wilt toevoegen van een nieuwe gebruiker met sudo-machtigingen doen:
1. Voer `useradd <username>` een gebruiker toevoegen
1. Voer `sudo usermod -a -G sudo <username>` aan de nieuwe gebruiker bevoegdheden op hoofdniveau
1. Gebruik `passwd <username>` om in te stellen van het wachtwoord voor de nieuwe gebruiker. Kunt u zich vervolgens aanmelden als de nieuwe gebruiker


## <a name="access-for-red-hat-enterprise-linux-rhel"></a>Toegang voor Red Hat Enterprise Linux (RHEL)
RHEL doorlaat u in de modus voor één gebruiker automatisch als het niet normaal mag opstarten. Als u geen toegang tot de hoofdmap voor de modus voor één gebruiker hebt ingesteld, u heeft geen een hoofdwachtwoord en geen om aan te melden. Er is een oplossing (Zie 'Handmatig invoeren van modus voor één gebruiker' hieronder), maar het voorstel is het instellen van toegang tot de hoofdmap in eerste instantie.

### <a name="grub-access-in-rhel"></a>Toegang tot WORMGATEN in RHEL
RHEL wordt geleverd met WORMGATEN standaard ingeschakeld. Om in te voeren WORMGATEN, start opnieuw op uw virtuele machine met `sudo reboot` en druk op een willekeurige toets. Hier ziet u de GRUB-scherm weergegeven.

> Opmerking: Red Hat biedt ook de documentatie voor opstart in de modus redden, EMS-modus, foutopsporingsmodus en het root-wachtwoord opnieuw instellen. [Klik hier voor toegang tot deze](https://aka.ms/rhel7grubterminal).

### <a name="set-up-root-access-for-single-user-mode-in-rhel"></a>Toegang tot de hoofdmap voor de modus voor één gebruiker in RHEL instellen
Modus voor één gebruiker in RHEL is vereist voor de hoofdgebruiker zijn ingeschakeld, die standaard is uitgeschakeld. Als u een noodzaak voor het inschakelen van de modus voor één gebruiker hebt, gebruikt u de volgende instructies:

1. Meld u aan bij het Red Hat-systeem via SSH
1. Overschakelen naar de hoofdmap
1. Wachtwoord voor de hoofdgebruiker inschakelen
    * `passwd root` (een sterke root-wachtwoord instellen)
1. Controleer of hoofdgebruiker kunt alleen aanmelden via ttyS0
    * `edit /etc/ssh/sshd_config` en zorg ervoor dat PermitRootLogIn is ingesteld op no
    * `edit /etc/securetty file` om toe te staan alleen aanmelden via ttyS0

Nu als het systeem wordt opgestart naar de modus voor één gebruiker kunt u aanmelden via hoofdwachtwoord.

U kunt ook voor RHEL 7.4 + of u kunt inschakelen voor 6,9 + modus voor één gebruiker in de GRUB wordt gevraagd, Zie instructies [hier](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/5/html/installation_guide/s1-rescuemode-booting-single)

### <a name="manually-enter-single-user-mode-in-rhel"></a>Modus voor één gebruiker handmatig invoeren in RHEL
Als u hebt ingesteld WORMGATEN en root openen met de bovenstaande instructies, dan kunt u de modus voor één gebruiker met de volgende instructies:

1. Druk op 'Esc' tijdens het opnieuw opstarten van de virtuele machine om in te voeren van WORMGATEN
1. In de GRUB, drukt u op 'e' om te bewerken van het geselecteerde besturingssysteem moet worden opgestart in (doorgaans de eerste regel)
1. De regel kernel - vinden in Azure, deze begint met `linux16`
1. Druk op Ctrl + E te gaan naar het einde van de regel
1. Voeg het volgende toe aan het einde van de regel: `systemd.unit=rescue.target`
    * Dit zal u start in de modus voor één gebruiker. Als u gebruiken noodherstelmodus bevindt wilt, voegt u toe `systemd.unit=emergency.target` aan het einde van de regel in plaats van `systemd.unit=rescue.target`
1. Druk op Ctrl + X af te sluiten en opnieuw opstarten met de toegepaste instellingen
1. U wordt gevraagd het beheerderswachtwoord modus voor één gebruiker invoeren voordat u: dit is de hetzelfde wachtwoord dat u hebt gemaakt in de bovenstaande instructies

    ![](../media/virtual-machines-serial-console/virtual-machine-linux-serial-console-rhel-enter-emergency-shell.gif)

### <a name="enter-single-user-mode-without-root-account-enabled-in-rhel"></a>Modus voor één gebruiker invoeren zonder root-account is ingeschakeld in RHEL
Als u niet de stappen hierboven om in te schakelen van de hoofdgebruiker hebt doorlopen, kunt u nog steeds het root-wachtwoord opnieuw instellen. Gebruik de volgende instructies:

> Opmerking: Als u SELinux gebruikt, zorg ervoor dat u de extra stappen die worden beschreven in de documentatie van Red Hat hebt ondernomen [hier](https://aka.ms/rhel7grubterminal) wanneer het root-wachtwoord opnieuw instellen.

1. Druk op 'Esc' tijdens het opnieuw opstarten van de virtuele machine om in te voeren van WORMGATEN
1. In de GRUB, drukt u op 'e' om te bewerken van het geselecteerde besturingssysteem moet worden opgestart in (doorgaans de eerste regel)
1. De regel kernel - vinden in Azure, deze begint met `linux16`
1. Voeg `rd.break` aan het einde van de regel, ervoor te zorgen dat er is een spatie vóór `rd.break` (Zie het onderstaande voorbeeld)
    - Hiermee wordt het opstartproces onderbroken voordat besturingselement wordt doorgegeven vanuit `initramfs` naar `systemd`, zoals beschreven in de documentatie van Red Hat [hier](https://aka.ms/rhel7rootpassword).
1. Druk op Ctrl + X af te sluiten en opnieuw opstarten met de toegepaste instellingen
1. Zodra u opstart, wordt u in de noodherstelmodus bevinden met een alleen-lezen bestandssysteem worden verwijderd. Voer `mount -o remount,rw /sysroot` in de shell te koppelen van het root-bestandssysteem met machtigingen voor lezen/schrijven
1. Zodra u in de modus voor één gebruiker opstart, typt u in `chroot /sysroot` overschakelen naar de `sysroot` jailbroken
1. U bent nu hoofdmap. U kunt opnieuw instellen van het root-wachtwoord met `passwd` en gebruik vervolgens de bovenstaande instructies om in te voeren van de modus voor één gebruiker. Type `reboot -f` opnieuw op te starten wanneer u klaar bent.

![](../media/virtual-machines-serial-console/virtual-machine-linux-serial-console-rhel-emergency-mount-no-root.gif)

> Opmerking: De bovenstaande instructies wilt doorlopen doorlaat u in noodgevallen shell, zodat u kunt ook taken zoals het bewerken van `fstab`. Het algemeen aanvaarde voorstel is echter het root-wachtwoord opnieuw instellen en gebruiken die voor het invoeren van de modus voor één gebruiker.


## <a name="access-for-centos"></a>Toegang voor CentOS
Veel, zoals Red Hat Enterprise Linux vereist modus voor één gebruiker in CentOS WORMGATEN en de hoofdgebruiker zijn ingeschakeld.

### <a name="grub-access-in-centos"></a>Toegang tot WORMGATEN in CentOS
CentOS wordt geleverd met WORMGATEN standaard ingeschakeld. Om in te voeren WORMGATEN, start opnieuw op uw virtuele machine met `sudo reboot` en druk op een willekeurige toets. Hier ziet u de GRUB-scherm weergegeven.

### <a name="single-user-mode-in-centos"></a>Modus voor één gebruiker in CentOS
Volg de instructies voor RHEL hierboven om in te schakelen van modus voor één gebruiker in CentOS.

## <a name="access-for-ubuntu"></a>Toegang voor Ubuntu
Ubuntu-installatiekopieën hebben een hoofdwachtwoord niet nodig. Als het systeem wordt opgestart in de modus voor één gebruiker, kunt u deze kunt gebruiken zonder aanvullende referenties.

### <a name="grub-access-in-ubuntu"></a>In Ubuntu WORMGATEN toegang
Voor toegang tot WORMGATEN, houdt u 'Esc' ingedrukt terwijl de virtuele machine wordt opgestart.

Standaard Ubuntu-installatiekopieën kunnen niet automatisch wordt weergegeven de GRUB-scherm. Dit kan worden gewijzigd met de volgende instructies:
1. Open `/etc/default/grub.d/50-cloudimg-settings.cfg` in een teksteditor van uw keuze
1. Wijzig de `GRUB_TIMEOUT` waarde naar een andere waarde dan nul
1. Open `/etc/default/grub` in een teksteditor van uw keuze
1. Opmerkingen bij de `GRUB_HIDDEN_TIMEOUT=1` regel
1. Voer `sudo update-grub` uit.

### <a name="single-user-mode-in-ubuntu"></a>Modus voor één gebruiker in Ubuntu
Ubuntu doorlaat u in de modus voor één gebruiker automatisch als het niet normaal mag opstarten. Als u wilt handmatig invoeren modus voor één gebruiker, gebruik de volgende instructies:

1. Van WORMGATEN, drukt u op 'e' uw opstartvermelding (de Ubuntu-vermelding) bewerken
1. Zoek de regel die met begint `linux`, zoek naar `ro`
1. Voeg `single` nadat `ro`, ervoor zorgen dat er is een ruimte voor en na `single`
1. Druk op Ctrl + X op te starten met de volgende instellingen en geef de modus voor één gebruiker

### <a name="using-grub-to-invoke-bash-in-ubuntu"></a>Met behulp van WORMGATEN om aan te roepen bash in Ubuntu
Mogelijk zijn er situaties (zoals een wachtwoord vergeten hoofdmap) waar u kunt nog steeds mogelijk geen toegang krijgen tot de modus voor één gebruiker in uw Ubuntu-VM nadat u hebt geprobeerd de bovenstaande instructies. U kunt ook de kernel uit te voeren /bin/bash als init, in plaats van het systeem init, wat wordt krijgt u een bash-shell en toestaan voor systeemonderhoud vertellen. Gebruik de volgende instructies:

1. Van WORMGATEN, drukt u op 'e' uw opstartvermelding (de Ubuntu-vermelding) bewerken
1. Zoek de regel die met begint `linux`, zoek naar `ro`
1. Vervang `ro` met `rw init=/bin/bash`
    - Hiermee koppelt uw bestandssysteem als lezen / schrijven en /bin/bash gebruiken als de init-proces
1. Druk op Ctrl + X opnieuw op te starten met deze instellingen

## <a name="access-for-coreos"></a>Toegang voor CoreOS
Modus voor één gebruiker in CoreOS vereist WORMGATEN moet worden ingeschakeld.

### <a name="grub-access-in-coreos"></a>Toegang tot WORMGATEN in CoreOS
Voor toegang tot WORMGATEN, drukt u op een willekeurige toets wanneer uw virtuele machine wordt opgestart.

### <a name="single-user-mode-in-coreos"></a>Modus voor één gebruiker in CoreOS
CoreOS doorlaat u in de modus voor één gebruiker automatisch als het niet normaal mag opstarten. Als u wilt handmatig invoeren modus voor één gebruiker, gebruik de volgende instructies:
1. Van WORMGATEN, drukt u op 'e' uw opstartvermelding bewerken
1. Zoek de regel die met begint `linux$`. Er moet 2, ingekapseld in verschillende als/anders van de EU
1. Toevoeg- `coreos.autologin=ttyS0` aan het einde van beide `linux$` regels
1. Druk op Ctrl + X op te starten met de volgende instellingen en geef de modus voor één gebruiker

## <a name="access-for-suse-sles"></a>Toegang voor SUSE SLES
Nieuwe afbeeldingen van SLES 12 SP3 + toestaan toegang via de seriële console als het systeem wordt opgestart in de noodherstelmodus bevindt.

### <a name="grub-access-in-suse-sles"></a>Toegang tot WORMGATEN in SUSE SLES
Toegang tot WORMGATEN in SLES vereist bootloader configuratie via YaST. U doet dit door deze instructies te volgen:

1. SSH in uw SLES VM en voer `sudo yast bootloader`. Gebruik de `tab` sleutel `enter` sleutel en de pijltoetsen om te navigeren via het menu.
1. Navigeer naar `Kernel Parameters`, en Controleer `Use serial console`.
1. Voeg `serial --unit=0 --speed=9600 --parity=no` naar de Console-argumenten

1. Druk op F10 om uw instellingen opslaan en sluiten
1. Om in te voeren WORMGATEN, opnieuw opstarten van uw virtuele machine en drukt u op een willekeurige toets tijdens opstartprocedure te blijven op het scherm WORMGATEN
    - De standaardtime-out voor WORMGATEN is 1s. U kunt dit wijzigen door het wijzigen van de `GRUB_TIMEOUT` variabele in `/etc/default/grub`

![](../media/virtual-machines-serial-console/virtual-machine-linux-serial-console-sles-yast-grub-config.gif)

### <a name="single-user-mode-in-suse-sles"></a>Modus voor één gebruiker in SUSE SLES
U wordt automatisch verwijderd in noodgevallen shell als SLES niet normaal mag opstarten. Als u wilt de noodgevallen shell handmatig invoeren, gebruikt u de volgende instructies:

1. Van WORMGATEN, drukt u op 'e' uw opstartvermelding (de SLES-vermelding) bewerken
1. Zoek de regel van de kernel die wordt gestart met `linux`
1. Toevoeg- `systemd.unit=emergency.target` aan het einde van de regel
1. Druk op Ctrl + X op te starten met de volgende instellingen en noodgevallen shell invoeren
> Houd er rekening mee dat u wordt verwijderd in noodgevallen shell met een _alleen-lezen_ bestandssysteem. Als u maken van alle wijzigingen aan alle bestanden wilt, moet u te koppelen van het bestandssysteem met machtigingen voor lezen / schrijven. U doet dit door Voer `mount -o remount,rw /` in shell

## <a name="access-for-oracle-linux"></a>Toegang voor Oracle Linux
Veel, zoals Red Hat Enterprise Linux vereist modus voor één gebruiker in Oracle Linux WORMGATEN en de hoofdgebruiker zijn ingeschakeld.

### <a name="grub-access-in-oracle-linux"></a>Toegang tot WORMGATEN in Oracle Linux
Oracle Linux wordt geleverd met WORMGATEN standaard ingeschakeld. Om in te voeren WORMGATEN, start opnieuw op uw virtuele machine met `sudo reboot` en druk op 'Esc'. Hier ziet u de GRUB-scherm weergegeven.

### <a name="single-user-mode-in-oracle-linux"></a>Modus voor één gebruiker in Oracle Linux
Volg de instructies voor RHEL hierboven om in te schakelen van modus voor één gebruiker in Oracle Linux.

## <a name="next-steps"></a>Volgende stappen
* De belangrijkste seriële console van het Linux-documentatiepagina bevindt [hier](serial-console-linux.md).
* Informatie over het gebruik van de seriële Console [WORMGATEN in verschillende distributies inschakelen](https://blogs.msdn.microsoft.com/linuxonazure/2018/10/23/why-proactively-ensuring-you-have-access-to-grub-and-sysrq-in-your-linux-vm-could-save-you-lots-of-down-time/)
* Gebruik voor de seriële Console [NMI en SysRq aanroepen](serial-console-nmi-sysrq.md)
* Seriële Console van het is ook beschikbaar voor [Windows](serial-console-windows.md) VM's
* Meer informatie over [diagnostische gegevens over opstarten](boot-diagnostics.md)