---
title: Azure-seriële Console voor Linux | Microsoft Docs
description: Bidirectionele seriële Console voor virtuele Azure-Machines en Virtual Machine Scale Sets.
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
ms.date: 5/1/2019
ms.author: harijay
ms.openlocfilehash: 9577a81af3da98c6e8802c586ec468a6e44e46cf
ms.sourcegitcommit: 4891f404c1816ebd247467a12d7789b9a38cee7e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2019
ms.locfileid: "65442046"
---
# <a name="azure-serial-console-for-linux"></a>Azure-seriële Console voor Linux

Seriële Console van het in Azure portal biedt toegang tot een op tekst gebaseerde console voor virtuele Linux-machines (VM's) en virtuele-machineschaalset exemplaren. Deze seriële verbinding maken met de seriële poort COM1 van de virtuele machine of virtuele machine scale set exemplaar, toegang tot deze onafhankelijk van de status van de netwerk- of -besturingssysteem. Seriële console van het alleen kan worden geopend met behulp van Azure portal en is alleen voor gebruikers die een access-rol van inzender zijn toegestaan of hoger als u wilt de schaalset voor virtuele machine of virtuele machine.

Seriële Console werkt op dezelfde manier voor virtuele machines en virtuele-machineschaalset exemplaren. In dit document bevat alle vermeldingen op virtuele machines impliciet schaalsetinstanties virtuele machine, tenzij anders vermeld.

Zie voor de seriële Console-documentatie voor Windows, [seriële Console voor Windows](../windows/serial-console.md).

> [!NOTE]
> De seriële Console is algemeen beschikbaar in de globale Azure-regio's. Het is nog niet beschikbaar in de Azure government en Azure China clouds.


## <a name="prerequisites"></a>Vereisten

- Uw virtuele machine of virtuele machine-exemplaar schaalset moet de resource Manager-implementatiemodel gebruiken. Klassieke implementaties worden niet ondersteund.

- Uw account die gebruikmaakt van de seriële console van de [rol van inzender voor virtuele machines](../../role-based-access-control/built-in-roles.md#virtual-machine-contributor) voor de virtuele machine en de [diagnostische gegevens over opstarten](boot-diagnostics.md) storage-account

- Uw virtuele machine of virtuele machine scale set-exemplaar moet een gebruiker op basis van wachtwoorden hebben. U kunt maken met de [wachtwoord opnieuw instellen](https://docs.microsoft.com/azure/virtual-machines/extensions/vmaccess#reset-password) functie van de VM-extensie voor toegang. Selecteer **wachtwoord opnieuw instellen** uit de **ondersteuning en probleemoplossing** sectie.

- Uw virtuele machine of virtuele machine-exemplaar schaalset moet hebben [diagnostische gegevens over opstarten](boot-diagnostics.md) ingeschakeld.

    ![De instellingen voor diagnostische gegevens over opstarten](./media/virtual-machines-serial-console/virtual-machine-serial-console-diagnostics-settings.png)

- Zie voor specifieke instellingen voor Linux-distributies, [seriële console van beschikbaarheid van de Linux-distributie](#serial-console-linux-distribution-availability).



## <a name="get-started-with-the-serial-console"></a>Aan de slag met de seriële Console
De seriële Console voor virtuele machines en virtuele-machineschaalset is uitsluitend toegankelijk via de Azure-portal:

### <a name="serial-console-for-virtual-machines"></a>Seriële Console voor virtuele Machines
Seriële Console voor virtuele machines is zo eenvoudig als klikken op **seriële console** binnen de **ondersteuning en probleemoplossing** sectie in Azure portal.
  1. Open de [Azure Portal](https://portal.azure.com).

  1. Navigeer naar **alle resources** en selecteert u een virtuele Machine. De overzichtspagina voor de virtuele machine wordt geopend.

  1. Schuif omlaag naar de **ondersteuning en probleemoplossing** sectie en selecteer **seriële console**. Een nieuw deelvenster met de seriële console wordt geopend en wordt de verbinding wordt gestart.

     ![Linux-seriële Console-venster](./media/virtual-machines-serial-console/virtual-machine-linux-serial-console-connect.gif)

### <a name="serial-console-for-virtual-machine-scale-sets"></a>Seriële Console voor Virtual Machine Scale Sets
Seriële Console is beschikbaar op basis van afzonderlijke instanties voor virtuele-machineschaalsets. U hebt om te navigeren naar de afzonderlijke instantie van een virtuele-machineschaalset voordat de **seriële console** knop. Als uw virtuele-machineschaalset geen diagnostische gegevens over opstarten ingeschakeld, controleert u of dat u uw virtuele machine scale set model voor het inschakelen van diagnostische gegevens over opstarten en vervolgens alle exemplaren een upgrade naar het nieuwe model voor toegang tot de seriële console bijwerken.
  1. Open de [Azure Portal](https://portal.azure.com).

  1. Navigeer naar **alle resources** en selecteert u een virtuele-Machineschaalset opgehaald. De overzichtspagina voor de virtuele-machineschaalset instellen wordt geopend.

  1. Navigeer naar **exemplaren**

  1. Selecteer een instantie van virtuele-machineschaalset

  1. Uit de **ondersteuning en probleemoplossing** sectie, selecteer **seriële console**. Een nieuw deelvenster met de seriële console wordt geopend en wordt de verbinding wordt gestart.

     ![Linux machineschaalset virtuele-seriële Console](./media/virtual-machines-serial-console/vmss-start-console.gif)


> [!NOTE]
> De seriële console is een lokale gebruiker met een ingestelde wachtwoord vereist. Virtuele machines of virtuele-machineschaalsets geconfigureerd alleen met een openbare SSH-sleutel zich niet aanmelden bij de seriële console. Voor het maken van een lokale gebruiker met een wachtwoord gebruikt de [VMAccess-extensie](https://docs.microsoft.com/azure/virtual-machines/linux/using-vmaccess-extension), die beschikbaar zijn in de portal is door te selecteren **wachtwoord opnieuw instellen** in de Azure-portal en maak een lokale gebruiker met een wachtwoord.
> U kunt het administrator-wachtwoord ook opnieuw instellen in uw account door [WORMGATEN gebruiken om op te starten in de modus voor één gebruiker](./serial-console-grub-single-user-mode.md).

## <a name="serial-console-linux-distribution-availability"></a>Seriële Console Linux-distributie-beschikbaarheid
Voor de seriële console te laten functioneren, moet het gastbesturingssysteem worden geconfigureerd om te lezen en schrijven van consoleberichten naar de seriële poort. De meeste [goedgekeurd Azure Linux-distributies](https://docs.microsoft.com/azure/virtual-machines/linux/endorsed-distros) hebben van de seriële console standaard geconfigureerd. Selecteren **seriële console** in de **ondersteuning en probleemoplossing** sectie van de Azure-portal biedt toegang tot de seriële console.

Distributie      | Toegang tot seriële console
:-----------|:---------------------
Red Hat Enterprise Linux    | Seriële console-toegang is standaard ingeschakeld.
CentOS      | Seriële console-toegang is standaard ingeschakeld.
Ubuntu      | Seriële console-toegang is standaard ingeschakeld.
CoreOS      | Seriële console-toegang is standaard ingeschakeld.
SUSE        | Nieuwere SLES-installatiekopieën die beschikbaar zijn op Azure hebt seriële console-toegang standaard ingeschakeld. Als u van oudere versies (10 of eerder) van SLES op Azure gebruikmaakt, raadpleegt u de [KB-artikel](https://www.novell.com/support/kb/doc.php?id=3456486) seriële console inschakelen.
Oracle Linux        | Seriële console-toegang is standaard ingeschakeld.
Aangepaste Linux-installatiekopieën     | Als u de seriële console voor uw aangepaste Linux-VM-installatiekopie, wilt inschakelen toegang tot de console in het bestand */etc/inittab* om uit te voeren van een terminal op `ttyS0`. Bijvoorbeeld: `S0:12345:respawn:/sbin/agetty -L 115200 console vt102`. Zie voor meer informatie over het maken van aangepaste installatiekopieën correct [een Linux-VHD in Azure maken en uploaden](https://aka.ms/createuploadvhd). Als u een aangepaste kernel bouwt, kunt u overwegen deze vlaggen kernel inschakelen: `CONFIG_SERIAL_8250=y` en `CONFIG_MAGIC_SYSRQ_SERIAL=y`. Het configuratiebestand bevindt zich doorgaans de */boot/* pad.

> [!NOTE]
> Als u in de seriële console niet ziet, zorg er dan voor dat de diagnostische gegevens over die opstarten is ingeschakeld op de virtuele machine. Bereikt **Enter** wordt vaak problemen opgelost waarbij niets wordt weergegeven in de seriële console.

## <a name="common-scenarios-for-accessing-the-serial-console"></a>Algemene scenario's voor het openen van de seriële Console

Scenario          | Acties in de seriële Console
:------------------|:-----------------------------------------
Verbroken *FSTAB* bestand | Druk op de **Enter** om te gaan en gebruik een teksteditor om op te lossen de *FSTAB* bestand. U moet mogelijk worden in de modus voor één gebruiker om dit te doen. Zie voor meer informatie de sectie van de seriële console van [voor het oplossen van problemen met fstab](https://support.microsoft.com/help/3206699/azure-linux-vm-cannot-start-because-of-fstab-errors) en [seriële console gebruiken voor toegang tot WORMGATEN en de modus voor één gebruiker](serial-console-grub-single-user-mode.md).
Onjuiste firewall-regels |  Als u iptables voor het blokkeren van SSH-verbinding hebt geconfigureerd, kunt u de seriële console gebruiken om te communiceren met uw virtuele machine zonder SSH. Meer informatie kunnen u vinden op de [iptables man pagina](https://linux.die.net/man/8/iptables).<br>Op dezelfde manier als uw firewalld SSH-toegang blokkeert, kunt u toegang tot de virtuele machine via de seriële console en firewalld configureren. Meer informatie vindt u de [firewalld documentatie](https://firewalld.org/documentation/).
Bestandssysteem beschadigd/selectievakje | Zie de sectie van de seriële console van [virtuele Azure Linux-machine kan niet worden gestart vanwege fouten in het bestandssysteem](https://support.microsoft.com/en-us/help/3213321/linux-recovery-cannot-ssh-to-linux-vm-due-to-file-system-errors-fsck) voor meer informatie over het oplossen van problemen met behulp van de seriële console van bestandssystemen beschadigd.
Problemen met SSH-configuratie | Toegang tot de seriële console en de instellingen wijzigen. Seriële console kan worden gebruikt, ongeacht de SSH-configuratie van een virtuele machine, zoals vereist niet de virtuele machine verbinding met het netwerk om te werken. Een gids voor probleemoplossing vindt u op [oplossen SSH-verbindingen met een Azure Linux VM die is mislukt, fouten, of wordt geweigerd](https://docs.microsoft.com/azure/virtual-machines/troubleshooting/troubleshoot-ssh-connection). Meer details zijn beschikbaar op [gedetailleerde SSH probleemoplossing voor problemen die verbinding maken met een Linux-VM in Azure](./detailed-troubleshoot-ssh-connection.md)
Interactie met de bootloader | Start opnieuw op uw virtuele machine uit binnen de seriële console-blade voor toegang tot WORMGATEN op uw Linux-VM. Zie voor meer informatie over en distributie-specifieke informatie [seriële console gebruiken voor toegang tot WORMGATEN en de modus voor één gebruiker](serial-console-grub-single-user-mode.md).

## <a name="disable-the-serial-console"></a>Uitschakelen van de seriële Console
Standaard hebben alle abonnementen seriële console-toegang ingeschakeld. U kunt de seriële console op het abonnementsniveau of op VM/virtuele machine scale set niveau uitschakelen. Houd er rekening mee dat er diagnostische gegevens over opstarten moet zijn ingeschakeld op een virtuele machine in de volgorde voor de seriële console om te werken.

### <a name="vmvirtual-machine-scale-set-level-disable"></a>VM/virtuele machine scale set-niveau uitschakelen
De seriële console kan worden uitgeschakeld voor een specifieke virtuele machine of virtuele machine schaalset door de instelling van de diagnostische gegevens over opstarten uit te schakelen. Diagnostische gegevens over opstarten via de Azure-portal om uit te schakelen van de seriële console voor de virtuele machine of de virtuele-machineschaalset uitschakelen. Als u de seriële console op een virtuele-machineschaalset, controleert u of dat u exemplaren in uw virtuele machineschaalset upgraden naar de nieuwste model.

> [!NOTE]
> Als u wilt in- of uitschakelen van de seriële console voor een abonnement, moet u hebt schrijfmachtigingen voor het abonnement. Deze machtigingen zijn beheerder of eigenaar van rollen. Aangepaste rollen kunnen ook schrijfmachtigingen hebben.

### <a name="subscription-level-disable"></a>Abonnementsniveau uitschakelen
De seriële console kan worden uitgeschakeld voor een volledige-abonnement via de [uitschakelen Console REST API-aanroep](/rest/api/serialconsole/console/disableconsole). U kunt de **uitproberen** functie beschikbaar is op deze pagina van de API-documentatie uitschakelen en inschakelen van de seriële console voor een abonnement. Voer uw abonnements-ID voor **subscriptionId**, voer **standaard** voor **standaard**, en selecteer vervolgens **uitvoeren**. Azure CLI-opdrachten zijn nog niet beschikbaar.

![REST-API uitproberen](./media/virtual-machines-serial-console/virtual-machine-serial-console-rest-api-try-it.png)

U kunt ook de volgende set bash-opdrachten gebruiken in Cloud Shell uitschakelen, inschakelen en de uitgeschakelde status van de seriële console voor een abonnement weergeven:

* De uitgeschakelde status van de seriële console voor een abonnement ophalen:
    ```azurecli-interactive
    $ export ACCESSTOKEN=($(az account get-access-token --output=json | jq .accessToken | tr -d '"'))

    $ export SUBSCRIPTION_ID=$(az account show --output=json | jq .id -r)

    $ curl "https://management.azure.com/subscriptions/$SUBSCRIPTION_ID/providers/Microsoft.SerialConsole/consoleServices/default?api-version=2018-05-01" -H "Authorization: Bearer $ACCESSTOKEN" -H "Content-Type: application/json" -H "Accept: application/json" -s | jq .properties
    ```
* De seriële console voor een abonnement uitschakelen:
    ```azurecli-interactive
    $ export ACCESSTOKEN=($(az account get-access-token --output=json | jq .accessToken | tr -d '"'))

    $ export SUBSCRIPTION_ID=$(az account show --output=json | jq .id -r)

    $ curl -X POST "https://management.azure.com/subscriptions/$SUBSCRIPTION_ID/providers/Microsoft.SerialConsole/consoleServices/default/disableConsole?api-version=2018-05-01" -H "Authorization: Bearer $ACCESSTOKEN" -H "Content-Type: application/json" -H "Accept: application/json" -s -H "Content-Length: 0"
    ```
* Inschakelen van de seriële console voor een abonnement:
    ```azurecli-interactive
    $ export ACCESSTOKEN=($(az account get-access-token --output=json | jq .accessToken | tr -d '"'))

    $ export SUBSCRIPTION_ID=$(az account show --output=json | jq .id -r)

    $ curl -X POST "https://management.azure.com/subscriptions/$SUBSCRIPTION_ID/providers/Microsoft.SerialConsole/consoleServices/default/enableConsole?api-version=2018-05-01" -H "Authorization: Bearer $ACCESSTOKEN" -H "Content-Type: application/json" -H "Accept: application/json" -s -H "Content-Length: 0"
    ```

## <a name="serial-console-security"></a>Seriële console-beveiliging

### <a name="access-security"></a>Beveiliging voor toegang
Toegang tot de seriële console is beperkt tot gebruikers die beschikken over een toegangsrol van [Inzender voor virtuele machines](../../role-based-access-control/built-in-roles.md#virtual-machine-contributor) of hoger op de virtuele machine. Als uw Azure Active Directory-tenant is multi-factor authentication (MFA) vereist, wordt toegang tot de seriële console ook de MFA moet omdat de seriële console-toegang via de [Azure-portal](https://portal.azure.com).

### <a name="channel-security"></a>Beveiliging van het kanaal
Alle gegevens die worden verzonden heen en weer worden versleuteld op de kabel.

### <a name="audit-logs"></a>Auditlogboeken
Alle toegang tot de seriële console momenteel is aangemeld de [diagnostische gegevens over opstarten](https://docs.microsoft.com/azure/virtual-machines/linux/boot-diagnostics) logboeken van de virtuele machine. Toegang tot deze logboeken zijn eigendom van en beheerd door de beheerder van de virtuele machine van Azure.

> [!CAUTION]
> Er zijn geen wachtwoorden voor toegang voor de console worden geregistreerd. Echter, als opdrachten worden uitgevoerd binnen de console bevat of uitvoer van wachtwoorden, geheimen, gebruikersnamen of enige andere vorm van persoonlijk identificeerbare informatie (PII), die wordt geschreven naar de VM boot diagnostics-Logboeken. Ze worden geschreven, samen met andere zichtbare tekst, als onderdeel van de implementatie van de seriële console Schuif terug functie. Deze logboeken zijn circulaire en alleen personen met leesmachtigingen voor het opslagaccount voor diagnostische gegevens over de toegang tot hebben. We raden echter aan na de aanbevolen procedure van het gebruik van de extern bureaublad voor alles wat die hebben mogelijk betrekking op geheimen en/of PII.

### <a name="concurrent-usage"></a>Gelijktijdig gebruik
Als een gebruiker is verbonden met de seriële console en een andere gebruiker is toegang tot deze virtuele machine met dezelfde aanvraagt, wordt de eerste gebruiker verbroken en wordt de tweede gebruiker heeft verbinding gemaakt met dezelfde sessie.

> [!CAUTION]
> Dit betekent dat een gebruiker die niet verbonden wordt niet worden afgemeld. De mogelijkheid om af te dwingen een afmelden bij het verbreken van de verbinding (met behulp van SIGHUP of een vergelijkbaar mechanisme) is nog steeds op de planning. Voor Windows is een automatische time-out ingeschakeld in administratieve Console SAC (Special); u kunt echter de terminal time-outinstelling configureren voor Linux. Om dit te doen, Voeg `export TMOUT=600` in uw *.bash_profile* of *.profile* -bestand voor de gebruiker die u gebruiken om aan te melden bij de console. Deze instelling wordt time-out van de sessie na 10 minuten.

## <a name="accessibility"></a>Toegankelijkheid
Toegankelijkheid is een belangrijke focus voor de seriële Console van Azure. Wat dat betreft, hebben we ervoor gezorgd dat de seriële console volledig toegankelijk is.

### <a name="keyboard-navigation"></a>Toetsenbordnavigatie
Gebruik de **tabblad** sleutel op het toetsenbord om te navigeren in de interface van de seriële console van de Azure-portal. Uw locatie wordt gemarkeerd op het scherm worden weergegeven. Als u wilt de focus van de seriële console-venster laten, drukt u op **Ctrl**+**F6** op het toetsenbord.

### <a name="use-serial-console-with-a-screen-reader"></a>Seriële Console gebruiken met een schermlezer
De seriële console heeft ingebouwde ondersteuning voor schermlezers. Navigeren om met een schermlezer ingeschakeld, kunnen de alt-tekst voor de geselecteerde knop om te worden door de schermlezer voorgelezen.

## <a name="errors"></a>Fouten
Omdat de meeste fouten tijdelijk zijn, opnieuw wordt geprobeerd de verbinding kan vaak worden verholpen. De volgende tabel geeft een lijst met fouten en oplossingen. Deze fouten en oplossingen op voor beide VM's en virtuele-machineschaalset exemplaren.

Fout                            |   Oplossing
:---------------------------------|:--------------------------------------------|
Kan niet worden opgehaald van de instellingen voor diagnostische gegevens over opstarten voor  *&lt;VMNAME&gt;*. Voor het gebruik van de seriële console of dat de diagnostische gegevens over die opstarten is ingeschakeld voor deze virtuele machine. | Zorg ervoor dat de virtuele machine heeft [diagnostische gegevens over opstarten](boot-diagnostics.md) ingeschakeld.
De virtuele machine is gestopt toewijzing ongedaan gemaakt. Start de virtuele machine en probeer de seriële console-verbinding. | De virtuele machine moet zich in een status die toegang tot de seriële console is gestart.
U hebt niet de vereiste machtigingen voor het gebruik van deze virtuele machine met de seriële console. Zorg ervoor dat u ten minste beschikken over machtigingen van de rol Inzender voor virtuele machines.| De toegang tot de seriële console nodig bepaalde machtigingen. Zie voor meer informatie, [vereisten](#prerequisites).
Kan niet bepalen van de resourcegroep voor het opslagaccount voor diagnostische gegevens van opstarten  *&lt;STORAGEACCOUNTNAME&gt;*. Controleer of u diagnostische gegevens over opstarten is ingeschakeld voor deze virtuele machine en u toegang hebt tot dit opslagaccount wordt gebruikt. | De toegang tot de seriële console nodig bepaalde machtigingen. Zie voor meer informatie, [vereisten](#prerequisites).
Web socket is gesloten of kan niet worden geopend. | U moet mogelijk aan lijst met geaccepteerde `*.console.azure.com`. Een meer gedetailleerde maar langer aanpak is het whitelist de [Microsoft Azure Datacenter IP-bereiken](https://www.microsoft.com/download/details.aspx?id=41653), waardoor vrij regelmatig worden gewijzigd.
Een antwoord 'Verboden' is opgetreden bij het openen van deze virtuele machine opstarten diagnostische storage-account. | Zorg ervoor dat diagnostische gegevens over de opstarten beschikt niet over een account-firewall. Een toegankelijke opstarten diagnostische storage-account is nodig voor de seriële console van functie.

## <a name="known-issues"></a>Bekende problemen
We zijn op de hoogte van problemen met de seriële console. Hier volgt een lijst van deze problemen beschreven en stappen voor risicobeperking. Deze problemen en oplossingen op voor beide VM's en virtuele-machineschaalset exemplaren.

Probleem                           |   Oplossing
:---------------------------------|:--------------------------------------------|
Drukken **Enter** nadat de banner van de verbinding niet leidt een aanmeldingsprompt tot moet worden weergegeven. | Zie voor meer informatie, [Hitting invoeren, gebeurt er niets](https://github.com/Microsoft/azserialconsole/blob/master/Known_Issues/Hitting_enter_does_nothing.md). Dit probleem kan optreden als u werkt met een aangepaste VM, beperkte toestel of GRUB-configuratie die ervoor zorgt Linux dat tot geen verbinding maken met de seriële poort.
Tekst van de seriële console duurt slechts een gedeelte van de schermgrootte (vaak na met een teksteditor). | Seriële consoles bieden geen ondersteuning voor het onderhandelen over venstergrootte ([RFC 1073](https://www.ietf.org/rfc/rfc1073.txt)), wat betekent dat er geen signaal SIGWINCH worden verzonden naar het bijwerken van de grootte van het scherm en de virtuele machine heeft geen kennis van de grootte van de terminal. Installeer xterm of een vergelijkbaar hulpprogramma zodat u kunt met de `resize` opdracht in en voer vervolgens `resize`.
Lange tekenreeksen plakken werkt niet. | De seriële console beperkt de lengte van tekenreeksen in de terminal naar 2048 tekens om te voorkomen dat de seriële poort-bandbreedte overbelasten geplakt.
Seriële console werkt niet met een firewall voor storage-account. | Standaard de seriële console werkt niet met een storage-account Firewall ingeschakeld op het opslagaccount van de diagnostische gegevens over opstarten.


## <a name="frequently-asked-questions"></a>Veelgestelde vragen

**Q. Hoe kan ik feedback verzenden?**

A. Feedback geven door het maken van een GitHub-probleem aan https://aka.ms/serialconsolefeedback. U kunt ook (minder bij voorkeur), kunt u feedback via verzenden azserialhelp@microsoft.com of in de categorie van de virtuele machine van https://feedback.azure.com.

**Q. Biedt ondersteuning voor de seriële console kopiëren/plakken?**

A. Ja. Gebruik **Ctrl**+**Shift**+**C** en **Ctrl**+**Shift** + **V** kopiëren en plakken in de terminal.

**Q. Kan ik de seriële console in plaats van een SSH-verbinding gebruiken?**

A. Terwijl dit gebruik technisch mogelijk lijkt, is de seriële console moet worden gebruikt als een hulpprogramma voor probleemoplossing in situaties waar connectiviteit via SSH niet mogelijk is voornamelijk bedoeld. We raden het af met de seriële console als een SSH-vervanging voor de volgende redenen:

- De seriële console geen zoveel bandbreedte als SSH. Omdat het een alleen-verbinding is, wordt meer GUI-zware interacties moeilijk zijn.
- Toegang tot de seriële console is op dit moment mogelijk alleen met behulp van een gebruikersnaam en wachtwoord. Omdat de SSH-sleutels zijn veel veiliger dan combinaties van de gebruikersnaam en wachtwoord, vanuit het beveiligingsoogpunt van aanmelding bij wordt aangeraden SSH via de seriële console.

**Q. Wie kunt inschakelen of uitschakelen van de seriële console voor mijn abonnement?**

A. Als u wilt in- of uitschakelen van de seriële console op het niveau van een brede, door het abonnement, moet u hebt schrijfmachtigingen voor het abonnement. Rollen die gemachtigd schrijven bevatten beheerder of eigenaar rollen. Aangepaste rollen kunnen ook schrijfmachtigingen hebben.

**Q. Wie toegang heeft tot de seriële console voor mijn VM/virtuele-machineschaalset?**

A. U moet de rol Inzender voor virtuele machines hebben of hoger voor een schaalset voor virtuele machine of virtuele machine toegang tot de seriële console.

**Q. Mijn seriële console van alles zijn, niet wordt weergegeven wat moet ik doen?**

A. Uw installatiekopie is waarschijnlijk niet goed is geconfigureerd voor toegang tot de seriële console. Zie voor meer informatie over het configureren van de afbeelding om in te schakelen van de seriële console [seriële console van beschikbaarheid van de Linux-distributie](#serial-console-linux-distribution-availability).

**Q. Is de seriële console beschikbaar voor virtuele-machineschaalsets?**

A. Ja dat is zo! Zie [seriële Console voor Virtual Machine Scale Sets](#serial-console-for-virtual-machine-scale-sets)

**Q. Als ik mijn virtuele machine of virtuele-machineschaalset met behulp van alleen het sleutelverificatie SSH hebt ingesteld, kan ik nog steeds gebruiken de seriële console verbinding maken met mijn VM/virtuele machine scale set exemplaar?**

A. Ja. Omdat de seriële console geen SSH-sleutels vereist, moet u alleen voor het instellen van een combinatie van gebruikersnaam en wachtwoord. U kunt dit doen door het selecteren van **wachtwoord opnieuw instellen** in de Azure portal en met deze referenties aanmelden bij de seriële console.

## <a name="next-steps"></a>Volgende stappen
* Gebruik van de seriële console [toegang tot WORMGATEN en de modus voor één gebruiker](serial-console-grub-single-user-mode.md).
* Gebruik de seriële console voor [NMI en SysRq aanroepen](serial-console-nmi-sysrq.md).
* Informatie over het gebruik van de seriële console [WORMGATEN inschakelen in verschillende distributies](https://blogs.msdn.microsoft.com/linuxonazure/2018/10/23/why-proactively-ensuring-you-have-access-to-grub-and-sysrq-in-your-linux-vm-could-save-you-lots-of-down-time/).
* Seriële console van het is ook beschikbaar voor [Windows VMs](../windows/serial-console.md).
* Meer informatie over [diagnostische gegevens over opstarten](boot-diagnostics.md).

