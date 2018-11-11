---
title: Seriële console van de virtuele machine van Azure voor Windows | Microsoft Docs
description: Bidirectionele seriële console voor Windows Azure virtuele machines.
services: virtual-machines-windows
documentationcenter: ''
author: harijay
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 10/31/2018
ms.author: harijay
ms.openlocfilehash: 0a4a31a4905de38de444604c8ffdf4d4a2e632d4
ms.sourcegitcommit: 5a1d601f01444be7d9f405df18c57be0316a1c79
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/10/2018
ms.locfileid: "51515859"
---
# <a name="virtual-machine-serial-console-for-windows"></a>Seriële console van de virtuele machine voor Windows

De seriële console van de virtuele machine (VM) in Azure portal biedt toegang tot een op tekst gebaseerde console voor Windows-machines. Deze seriële verbinding maken met de seriële poort COM1 van de virtuele machine, toegang tot deze onafhankelijk van het netwerk of besturingssysteem status van de virtuele machine. Toegang tot de seriële console voor een virtuele machine kan alleen met behulp van de Azure-portal worden gedaan. Dit alleen toegestaan voor gebruikers die een access-rol van inzender voor virtuele machines of hoger op de virtuele machine hebben. 

Zie voor de seriële console van documentatie voor virtuele Linux-machines, [seriële console van de virtuele machine voor Linux](serial-console-linux.md).

> [!NOTE] 
> De seriële console voor virtuele machines is algemeen beschikbaar in de globale Azure-regio's. Het is nog niet beschikbaar in de Azure government en Azure China clouds.


## <a name="prerequisites"></a>Vereisten 

* De virtuele machine waar u toegang wilt een seriële console tot krijgen moet de resource Manager-implementatiemodel gebruiken. Klassieke implementaties worden niet ondersteund. 

* De virtuele machine waar u toegang wilt een seriële console tot krijgen moet hebben [diagnostische gegevens over opstarten](boot-diagnostics.md) ingeschakeld. 

    ![De instellingen voor diagnostische gegevens over opstarten](../media/virtual-machines-serial-console/virtual-machine-serial-console-diagnostics-settings.png)

* Een account met behulp van een seriële console moet beschikken over de [rol van inzender voor virtuele machines](../../role-based-access-control/built-in-roles.md#virtual-machine-contributor) voor de virtuele machine en de [diagnostische gegevens over opstarten](boot-diagnostics.md) storage-account. 

* De virtuele machine waar u toegang wilt een seriële console tot krijgen moet een account op basis van wachtwoorden hebben. U kunt maken met de [wachtwoord opnieuw instellen](https://docs.microsoft.com/azure/virtual-machines/extensions/vmaccess#reset-password) functie van de VM-extensie voor toegang. Selecteer **wachtwoord opnieuw instellen** uit de **ondersteuning en probleemoplossing** sectie. 


## <a name="get-started-with-the-serial-console"></a>Aan de slag met de seriële console
De seriële console voor virtuele machines is uitsluitend toegankelijk via de Azure-portal:

  1. Open de [Azure Portal](https://portal.azure.com).
  1. Selecteer in het menu links **virtuele machines**.
  1. Selecteer een virtuele machine in de lijst. De overzichtspagina voor de virtuele machine wordt geopend.
  1. Schuif omlaag naar de **ondersteuning en probleemoplossing** sectie en selecteer **seriële console**. Een nieuw deelvenster met de seriële console wordt geopend en wordt de verbinding wordt gestart.


## <a name="enable-the-serial-console-in-custom-or-older-images"></a>Inschakelen van de seriële console in aangepaste of oudere installatiekopieën
Nieuwere Windows Server-installatiekopieën op Azure hebt [speciale beheerconsole](https://technet.microsoft.com/library/cc787940(v=ws.10).aspx) (SAC) standaard ingeschakeld. SAC wordt ondersteund in serverversies van Windows, maar is niet beschikbaar in clientversies (bijvoorbeeld Windows 10, Windows 8 of Windows 7). 

Voor oudere Windows Server-installatiekopieën (die zijn gemaakt vóór februari 2018), kunt u automatisch de seriële console via de Azure-portal RunCommand-functie inschakelen. Selecteer in de Azure portal, **RunCommand-**, selecteer vervolgens de opdracht met de naam **EnableEM** in de lijst.

![Lijst met opdrachten uitvoeren](./media/virtual-machines-serial-console/virtual-machine-windows-serial-console-runcommand.png)

U kunt ook om in te schakelen handmatig de seriële console voor Windows-machines die zijn gemaakt vóór februari 2018, als volgt te werk: 

1. Verbinding maken met uw Windows-machine via Extern bureaublad
1. Voer de volgende opdrachten vanaf een opdrachtprompt met beheerdersrechten: 
    - `bcdedit /ems {current} on`
    - `bcdedit /emssettings EMSPORT:1 EMSBAUDRATE:115200`
1. Start opnieuw op het systeem voor de console SAC zijn ingeschakeld.

    ![SAC-console](./media/virtual-machines-serial-console/virtual-machine-windows-serial-console-connect.gif)

Indien nodig, kan de SAC ook worden ingeschakeld offline:

1. De windows-schijf waarvan u SAC geconfigureerd als een gegevensschijf aan de bestaande virtuele machine wilt koppelen. 

1. Voer de volgende opdrachten vanaf een opdrachtprompt met beheerdersrechten: 
   - `bcdedit /store <mountedvolume>\boot\bcd /ems {default} on`
   - `bcdedit /store <mountedvolume>\boot\bcd /emssettings EMSPORT:1 EMSBAUDRATE:115200`

### <a name="how-do-i-know-if-sac-is-enabled"></a>Hoe weet ik of SAC is ingeschakeld?

Als [SAC](https://technet.microsoft.com/library/cc787940(v=ws.10).aspx) niet is ingeschakeld, wordt niet de seriële console weergegeven de SAC-prompt. In sommige gevallen, gegevens over de servicestatus van de virtuele machine wordt weergegeven en in andere gevallen zijn de velden leeg. Als u een installatiekopie van Windows Server die zijn gemaakt vóór februari 2018, SAC waarschijnlijk niet ingeschakeld.

## <a name="enable-the-windows-boot-menu-in-the-serial-console"></a>Het opstartmenu Windows in de seriële console inschakelen 

Als u nodig hebt om in te schakelen Windows boot loader aanwijzingen om weer te geven in de seriële console, kunt u de volgende aanvullende opties toevoegen aan uw opstartconfiguratiegegevens. Zie voor meer informatie, [bcdedit](https://docs.microsoft.com/windows-hardware/drivers/devtest/bcdedit--set).

1. Verbinding maken met uw Windows-machine via Extern bureaublad.

1. Voer de volgende opdrachten vanaf een opdrachtprompt met beheerdersrechten: 
   - `bcdedit /set {bootmgr} displaybootmenu yes`
   - `bcdedit /set {bootmgr} timeout 10`
   - `bcdedit /set {bootmgr} bootems yes`

1. Opnieuw opstarten van het systeem voor het opstartmenu worden ingeschakeld

> [!NOTE] 
> De time-out die u hebt ingesteld voor het opstartmenu manager om weer te geven is van invloed op de opstarttijd van uw besturingssysteem. Als u denkt dat de 10 seconden time-outwaarde is te lang of te kort dat, doet u het op een andere waarde.

## <a name="use-the-serial-console-for-nmi-calls-in-windows-vms"></a>De seriële console voor NMI aanroepen in de Windows-VM's gebruiken
Een niet-maskeren interrupt (NMI) is ontworpen voor het maken van een signaal dat software op een virtuele machine wordt niet negeren. In het verleden zijn NMIs gebruikt om te controleren op hardwareproblemen op systemen die specifieke reactietijden vereist. NMI vandaag, programmeurs en systeembeheerders vaak gebruikt als een mechanisme voor foutopsporing of problemen met systemen vastgelopen.

De seriële console kan worden gebruikt voor het verzenden van een NMI met een Azure-machine met behulp van het toetsenbordpictogram in de opdrachtbalk. Nadat de NMI wordt geleverd, wordt de virtuele-machineconfiguratie bepalen hoe het systeem reageert. Windows kan worden geconfigureerd voor crashes en een geheugendumpbestand maken wanneer er een NMI worden ontvangen.

![NMI verzenden](../media/virtual-machines-serial-console/virtual-machine-windows-serial-console-nmi.png) <br>

Zie voor meer informatie over het configureren van Windows voor het maken van een crashdumpbestand wanneer deze een NMI ontvangt [het genereren van een crashdump-bestand met behulp van een NMI](https://support.microsoft.com/help/927069/how-to-generate-a-complete-crash-dump-file-or-a-kernel-crash-dump-file).

## <a name="open-cmd-or-powershell-in-serial-console"></a>Open CMD of Powershell in de seriële console

1. Verbinding maken met de seriële console. Als u bent verbonden, is het de prompt **SAC >**:

    ![Verbinding maken met SAC](./media/virtual-machines-serial-console/virtual-machine-windows-serial-console-connect-sac.png)

1.  Voer `cmd` om een kanaal waaraan een CMD-exemplaar te maken. 

1.  Voer `ch -si 1` om over te schakelen naar het kanaal dat de CMD-exemplaar wordt uitgevoerd. 

1.  Druk op **Enter**, en geef vervolgens referenties aanmelden met beheerdersmachtigingen.

1.  Nadat u geldige referenties hebt ingevoerd, wordt de CMD-instantie wordt geopend.

1.  Voer een PowerShell-sessie starten, `PowerShell` in de CMD-exemplaar, en druk vervolgens op **Enter**. 

    ![Open PowerShell-exemplaar](./media/virtual-machines-serial-console/virtual-machine-windows-serial-console-powershell.png)


## <a name="disable-the-serial-console"></a>Uitschakelen van de seriële console
Standaard hebben alle abonnementen seriële console-toegang ingeschakeld voor alle virtuele machines. U kunt de seriële console op het abonnementsniveau of op het niveau van de virtuele machine uitschakelen.

> [!NOTE]       
> Als u wilt in- of uitschakelen van de seriële console voor een abonnement, moet u hebt schrijfmachtigingen voor het abonnement. Deze machtigingen zijn, maar niet beperkt zijn tot de beheerder of eigenaar van rollen. Aangepaste rollen kunnen ook schrijfmachtigingen hebben.

### <a name="subscription-level-disable"></a>Abonnementsniveau uitschakelen
De seriële console kan worden uitgeschakeld voor een volledige-abonnement via de [uitschakelen Console REST API-aanroep](https://docs.microsoft.com/rest/api/serialconsole/console/console_disableconsole). U kunt de **uitproberen** functie beschikbaar is op deze pagina van de API-documentatie uitschakelen en inschakelen van de seriële console voor een abonnement. Voer uw abonnements-ID voor **subscriptionId**, "default" invoeren voor **standaard**, en selecteer vervolgens **uitvoeren**. Azure CLI-opdrachten zijn nog niet beschikbaar.

![REST-API uitproberen](../media/virtual-machines-serial-console/virtual-machine-serial-console-rest-api-try-it.png)

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

### <a name="vm-level-disable"></a>VM-niveau uitschakelen
De seriële console kan worden uitgeschakeld voor een specifieke virtuele machine door het uitschakelen van deze virtuele machine opstarten diagnostische instelling. Diagnostische gegevens over opstarten via de Azure-portal om uit te schakelen van de seriële console voor de virtuele machine uitschakelen.

## <a name="serial-console-security"></a>Seriële console-beveiliging 

### <a name="access-security"></a>Beveiliging voor toegang 
Toegang tot de seriële console is beperkt tot gebruikers die beschikken over een toegangsrol van [Inzender voor virtuele machines](../../role-based-access-control/built-in-roles.md#virtual-machine-contributor) of hoger op de virtuele machine. Als uw Azure Active Directory-tenant is multi-factor authentication (MFA) vereist, wordt toegang tot de seriële console ook de MFA moet omdat de seriële console-toegang via de [Azure-portal](https://portal.azure.com).

### <a name="channel-security"></a>Beveiliging van het kanaal
Alle gegevens die worden verzonden heen en weer worden versleuteld op de kabel.

### <a name="audit-logs"></a>Controlelogboeken
Alle toegang tot de seriële console momenteel is aangemeld de [diagnostische gegevens over opstarten](https://docs.microsoft.com/azure/virtual-machines/linux/boot-diagnostics) logboeken van de virtuele machine. Toegang tot deze logboeken zijn eigendom van en beheerd door de beheerder van de virtuele machine van Azure.  

>[!CAUTION] 
Er zijn geen wachtwoorden voor toegang voor de console worden geregistreerd. Echter, als opdrachten worden uitgevoerd binnen de console bevat of uitvoer van wachtwoorden, geheimen, gebruikersnamen of enige andere vorm van persoonlijk identificeerbare informatie (PII), die wordt geschreven naar de VM boot diagnostics-Logboeken. Ze worden geschreven, samen met andere zichtbare tekst, als onderdeel van de implementatie van de seriële console Schuif terug functie. Deze logboeken zijn circulaire en alleen personen met leesmachtigingen voor het opslagaccount voor diagnostische gegevens over de toegang tot hebben. We raden echter aan na de aanbevolen procedure van het gebruik van de extern bureaublad voor alles wat die hebben mogelijk betrekking op geheimen en/of PII. 

### <a name="concurrent-usage"></a>Gelijktijdig gebruik
Als een gebruiker is verbonden met de seriële console en een andere gebruiker is toegang tot deze virtuele machine met dezelfde aanvraagt, wordt de eerste gebruiker verbroken en wordt de tweede gebruiker heeft verbinding gemaakt met dezelfde sessie.

>[!CAUTION] 
Dit betekent dat een gebruiker die niet verbonden wordt niet worden afgemeld. De mogelijkheid om af te dwingen een afmelden bij het verbreken van de verbinding (met behulp van SIGHUP of een vergelijkbaar mechanisme) is nog steeds in de roadmap. Voor Windows, er is een automatische time-out ingeschakeld in SAC; u kunt de terminal time-outinstelling configureren voor Linux. 

## <a name="common-scenarios-for-accessing-the-serial-console"></a>Algemene scenario's voor het openen van de seriële console 
Scenario          | Acties in de seriële console                
:------------------|:-----------------------------------------
Onjuiste firewall-regels | Toegang tot de seriële console en de oplossing Windows firewall-regels. 
Bestandssysteem beschadigd/selectievakje | Toegang tot de seriële console en het bestandssysteem herstellen. 
Problemen met RDP-configuratie | Toegang tot de seriële console en de instellingen wijzigen. Zie voor meer informatie de [RDP documentatie](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/clients/remote-desktop-allow-access).
Systeem voor het vergrendelen van netwerk | Toegang tot de seriële console van de Azure-portal voor het beheren van het systeem. Sommige netwerkopdrachten worden vermeld in [Windows-opdrachten: CMD en PowerShell](serial-console-cmd-ps-commands.md). 
Interactie met de bootloader | Toegang tot het BCD via de seriële console. Zie voor meer informatie, [inschakelen het opstartmenu Windows in de seriële console](#enable-the-windows-boot-menu-in-the-serial-console). 

## <a name="accessibility"></a>Toegankelijkheid
Toegankelijkheid is een belangrijke focus voor de seriële console van Azure. Wat dat betreft, hebben we ervoor gezorgd dat de seriële console is toegankelijk voor het visuele element en verminderde horen, evenals mensen die mogelijk niet langer een muis te gebruiken.

### <a name="keyboard-navigation"></a>Toetsenbordnavigatie
Gebruik de **tabblad** sleutel op het toetsenbord om te navigeren in de interface van de seriële console van de Azure-portal. Uw locatie wordt gemarkeerd op het scherm worden weergegeven. Als u wilt de focus van de seriële console-venster laten, drukt u op **Ctrl**+**F6** op het toetsenbord.

### <a name="use-the-serial-console-with-a-screen-reader"></a>Gebruik de seriële console met een schermlezer
De seriële console heeft ingebouwde ondersteuning voor schermlezers. Navigeren om met een schermlezer ingeschakeld, kunnen de alt-tekst voor de geselecteerde knop om te worden door de schermlezer voorgelezen.

## <a name="errors"></a>Fouten
Omdat de meeste fouten tijdelijk zijn, opnieuw wordt geprobeerd de verbinding kan vaak worden verholpen. De volgende tabel geeft een lijst met fouten en oplossingen.

Fout                            |   Oplossing 
:---------------------------------|:--------------------------------------------|
Kan niet worden opgehaald van de instellingen voor diagnostische gegevens over opstarten voor  *&lt;VMNAME&gt;*. Voor het gebruik van de seriële console of dat de diagnostische gegevens over die opstarten is ingeschakeld voor deze virtuele machine. | Zorg ervoor dat de virtuele machine heeft [diagnostische gegevens over opstarten](boot-diagnostics.md) ingeschakeld. 
De virtuele machine is gestopt toewijzing ongedaan gemaakt. Start de virtuele machine en probeer de seriële console-verbinding. | Virtuele machine moet zich in een status gestart voor toegang tot de seriële console
U hebt niet de vereiste machtigingen voor het gebruik van de seriële console van deze virtuele machine. Zorg ervoor dat u ten minste beschikken over machtigingen van de rol Inzender voor virtuele machines.| Toegang tot de seriële console nodig bepaalde machtigingen. Zie voor meer informatie, [vereisten](#prerequisites).
Kan niet bepalen van de resourcegroep voor het opslagaccount voor diagnostische gegevens van opstarten  *&lt;STORAGEACCOUNTNAME&gt;*. Controleer of u diagnostische gegevens over opstarten is ingeschakeld voor deze virtuele machine en u toegang hebt tot dit opslagaccount wordt gebruikt. | Toegang tot de seriële console nodig bepaalde machtigingen. Zie voor meer informatie, [vereisten](#prerequisites).
Een antwoord 'Verboden' is opgetreden bij het openen van deze virtuele machine opstarten diagnostische storage-account. | Zorg ervoor dat diagnostische gegevens over de opstarten beschikt niet over een account-firewall. Een toegankelijke opstarten diagnostische storage-account is nodig voor de seriële console van functie.
Web socket is gesloten of kan niet worden geopend. | U moet mogelijk aan lijst met geaccepteerde `*.console.azure.com`. Een meer gedetailleerde maar langer aanpak is het whitelist de [Microsoft Azure Datacenter IP-bereiken](https://www.microsoft.com/download/details.aspx?id=41653), waardoor vrij regelmatig worden gewijzigd.
Alleen gegevens over de servicestatus wordt weergegeven bij het verbinden met een Windows-VM| Deze fout treedt op als de beheerconsole van speciale is niet ingeschakeld voor uw Windows-installatiekopie. Zie [inschakelen van de seriële console in aangepaste of oudere installatiekopieën](#enable-the-serial-console-in-custom-or-older-images) voor instructies over het handmatig inschakelen SAC op uw Windows-VM. Zie voor meer informatie, [Windows health signalen](https://github.com/Microsoft/azserialconsole/blob/master/Known_Issues/Windows_Health_Info.md).

## <a name="known-issues"></a>Bekende problemen 
We zijn op de hoogte van problemen met de seriële console. Hier volgt een lijst van deze problemen beschreven en stappen voor risicobeperking.

Probleem                             |   Oplossing 
:---------------------------------|:--------------------------------------------|
Drukken **Enter** nadat de banner van de verbinding niet leidt een aanmeldingsprompt tot moet worden weergegeven. | Zie voor meer informatie, [Hitting invoeren, gebeurt er niets](https://github.com/Microsoft/azserialconsole/blob/master/Known_Issues/Hitting_enter_does_nothing.md). Deze fout kan optreden als u werkt met een aangepaste VM, beperkte toestel of boot-configuratie die ervoor zorgt Windows dat niet correct verbinding maken met de seriële poort. Deze fout wordt ook optreden als u werkt met een Windows 10 client VM, omdat alleen Windows Server VM's worden geconfigureerd met EMS is ingeschakeld.
Kan niet naar het type op SAC vragen als kernelfoutopsporing is ingeschakeld. | RDP-verbinding VM en voer `bcdedit /debug {current} off` vanaf een opdrachtprompt met verhoogde bevoegdheid. Als u niet de RDP-verbinding, kunt u in plaats daarvan de besturingssysteemschijf koppelen aan een andere Azure-virtuele machine en wijzigen terwijl als een gegevensschijf gekoppeld door uit te voeren `bcdedit /store <drive letter of data disk>:\boot\bcd /debug <identifier> off`, klikt u vervolgens wisselen van de schijf weer.
Plakken in PowerShell in SAC resulteert in een derde teken als de oorspronkelijke inhoud beschikt over een herhalende teken. | Voor een tijdelijke oplossing Voer `Remove-Module PSReadLine` te verwijderen van de module PSReadLine uit de huidige sessie. Deze actie wordt niet verwijderen of de module verwijderen.
Sommige invoer toetsenbord vreemd SAC uitvoer produceren (bijvoorbeeld **[A**, **[3 ~**). | [VT100](https://aka.ms/vtsequences) escapereeksen worden niet ondersteund door de SAC-prompt.
Lange tekenreeksen plakken werkt niet. | De seriële console beperkt de lengte van tekenreeksen in de terminal naar 2048 tekens om te voorkomen dat de seriële poort-bandbreedte overbelasten geplakt.


## <a name="frequently-asked-questions"></a>Veelgestelde vragen 

**Q. Hoe kan ik feedback verzenden?**

A. Feedback geven door het maken van een GitHub-probleem aan https://aka.ms/serialconsolefeedback. U kunt ook (minder bij voorkeur), kunt u feedback via verzenden azserialhelp@microsoft.com of in de categorie van de virtuele machine van http://feedback.azure.com.

**Q. Biedt ondersteuning voor de seriële console kopiëren/plakken?**

A. Ja. Gebruik **Ctrl**+**Shift**+**C** en **Ctrl**+**Shift** + **V** kopiëren en plakken in de terminal.

**Q. Wie kunt inschakelen of uitschakelen van de seriële console voor mijn abonnement?**

A. Als u wilt in- of uitschakelen van de seriële console op het niveau van een brede, door het abonnement, moet u hebt schrijfmachtigingen voor het abonnement. Rollen die gemachtigd schrijven bevatten beheerder of eigenaar rollen. Aangepaste rollen kunnen ook schrijfmachtigingen hebben.

**Q. Wie toegang heeft tot de seriële console voor mijn VM?**

A. U moet de rol Inzender voor virtuele machines hebben of hoger voor een virtuele machine voor toegang tot de seriële console van de virtuele machine. 

**Q. Mijn seriële console van alles zijn, niet wordt weergegeven wat moet ik doen?**

A. Uw installatiekopie is waarschijnlijk niet goed is geconfigureerd voor toegang tot de seriële console. Zie voor meer informatie over het configureren van de afbeelding om in te schakelen van de seriële console [inschakelen van de seriële console in aangepaste of oudere installatiekopieën](#enable-the-serial-console-in-custom-or-older-images).

**Q. Is de seriële console beschikbaar voor virtuele-machineschaalsets?**

A. Op dit moment wordt niet toegang tot de seriële console voor schaalsetinstanties virtuele machine ondersteund.

## <a name="next-steps"></a>Volgende stappen
* Zie voor een uitgebreide handleiding voor het CMD- en PowerShell-opdrachten kunt u in de Windows-SAC, [Windows-opdrachten: CMD en PowerShell](serial-console-cmd-ps-commands.md).
* Seriële console van het is ook beschikbaar voor [Linux](serial-console-linux.md) VM's.
* Meer informatie over [diagnostische gegevens over opstarten](boot-diagnostics.md).