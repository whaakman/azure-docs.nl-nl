---
title: 'Azure Backup: Bestanden en mappen herstellen vanuit een Azure-VM back-up'
description: Bestanden herstellen vanaf een herstelpunt van de virtuele machine van Azure
services: backup
author: pvrk
manager: shivamg
keywords: herstel op itemniveau; herstel van bestanden uit back-up van virtuele Azure-machine. bestanden herstellen vanaf Azure VM
ms.service: backup
ms.topic: conceptual
ms.date: 3/01/2019
ms.author: pullabhk
ms.openlocfilehash: 6c2ade276e43f5e40d334a62c62a1abaa42b1b78
ms.sourcegitcommit: 3f4ffc7477cff56a078c9640043836768f212a06
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/04/2019
ms.locfileid: "57308938"
---
# <a name="recover-files-from-azure-virtual-machine-backup"></a>Bestanden herstellen vanuit back-up van virtuele Azure-machine

Azure Backup biedt de mogelijkheid om terug te zetten [Azure virtuele machines (VM's) en schijven](./backup-azure-arm-restore-vms.md) vanuit Azure-VM back-ups, ook wel bekend als herstelpunten. In dit artikel wordt uitgelegd hoe u bestanden en mappen herstellen vanuit een Azure-VM back-up. Bestanden en mappen terugzetten is alleen beschikbaar voor Azure-VM's geïmplementeerd met behulp van de Resource Manager-model en beveiligde naar een Recovery services-kluis.

> [!Note]
> Deze functie is beschikbaar voor Azure-VM's geïmplementeerd met behulp van de Resource Manager-model en beveiligde naar een Recovery Services-kluis.
> Bestand herstellen vanuit een versleutelde VM back-up wordt niet ondersteund.
>

## <a name="mount-the-volume-and-copy-files"></a>Het volume en kopieer de bestanden koppelen

Als u bestanden of mappen herstellen vanaf het herstelpunt, gaat u naar de virtuele machine en kies het gewenste herstelpunt.

1. Aanmelden bij de [Azure-portal](http://portal.Azure.com) en klik in het linkerdeelvenster op **virtuele machines**. Selecteer in de lijst met virtuele machines, de virtuele machine om deze virtuele machine-dashboard te openen.

2. Klik in het menu van de virtuele machine op **back-up** om de back-up-dashboard te openen.

    ![Open Recovery Services-kluis back-upitem](./media/backup-azure-restore-files-from-vm/open-vault-for-vm.png)

3. Klik in het menu back-up-dashboard op **bestandsherstel**.

    ![Knop voor het herstel van bestand](./media/backup-azure-restore-files-from-vm/vm-backup-menu-file-recovery-button.png)

    De **bestandsherstel** menu wordt geopend.

    ![Herstel-bestandsmenu](./media/backup-azure-restore-files-from-vm/file-recovery-blade.png)

4. Uit de **herstelpunt selecteren** vervolgkeuzelijst, selecteer het herstelpunt dat u de gewenste bestanden bevat. Het laatste herstelpunt is standaard al geselecteerd.

5. Voor het downloaden van de software die wordt gebruikt om te kopiëren van bestanden vanaf het herstelpunt, klikt u op **uitvoerbaar bestand downloaden** (voor Windows Azure VM) of **downloadscript** (voor virtuele Azure Linux-machine, een python-script is gegenereerd).

    ![Gegenereerd wachtwoord](./media/backup-azure-restore-files-from-vm/download-executable.png)

    Azure wordt het uitvoerbare bestand of script gedownload naar de lokale computer.

    ![bericht voor het uitvoerbare bestand of script downloaden](./media/backup-azure-restore-files-from-vm/run-the-script.png)

    Als u wilt het uitvoerbare bestand of script uitvoeren als beheerder, wordt het aangeraden dat u Sla het bestand op uw computer.

6. Het uitvoerbare bestand of script wachtwoord is beveiligd en een wachtwoord is vereist. In de **bestandsherstel** menu, klikt u op de knop kopiëren voor het laden van het wachtwoord in het geheugen.

    ![Gegenereerd wachtwoord](./media/backup-azure-restore-files-from-vm/generated-pswd.png)

7. Vanaf de downloadlocatie (meestal in de map Downloads), met de rechtermuisknop op het uitvoerbare bestand of script en voer dit met beheerdersreferenties. Wanneer u hierom wordt gevraagd, typt u het wachtwoord of plakt u het wachtwoord van het geheugen en druk op Enter. Zodra de geldig wachtwoord is ingevoerd, is het script maakt verbinding met het herstelpunt.

    ![Herstel-bestandsmenu](./media/backup-azure-restore-files-from-vm/executable-output.png)

    Als u het script op een computer met beperkte toegang uitvoert, controleert u of er toegang tot:

    - download.microsoft.com
    - Recovery Service-URL's (geo-name verwijst naar de regio waar de recovery Services-kluis zich bevindt)
        - <https://pod01-rec2.geo-name.backup.windowsazure.com> (Voor openbare geografische gebieden voor Azure)
        - <https://pod01-rec2.geo-name.backup.windowsazure.cn> (Voor Azure China)
        - <https://pod01-rec2.geo-name.backup.windowsazure.us> (Voor Azure US Government)
        - <https://pod01-rec2.geo-name.backup.windowsazure.de> (Voor Azure Duitsland)
    - uitgaande poort 3260

> [!Note]
>
* De bestandsnaam van het gedownloade script heeft de **geo-name** de URL zijn ingevuld. Voor bijvoorbeeld: Naam van het gedownloade script begint met \'VMname\'\_\'geoname\'_\'GUID\', zoals ContosoVM_wcus_12345678...<br><br>
* De URL 'https://pod01-rec2.wcus.backup.windowsazure.com"


   Het script vereist voor Linux, onderdelen voor 'openen met iscsi-' en 'lshw' verbinding maken met het herstelpunt. Als de componenten nog niet bestaan op de computer waarop het script wordt uitgevoerd, wordt het script vraagt om machtiging om de onderdelen te installeren. Geef toestemming om de benodigde onderdelen te installeren.

   De toegang tot download.microsoft.com is vereist voor het downloaden van de onderdelen die worden gebruikt voor het bouwen van een beveiligd kanaal tussen de computer waarop het script wordt uitgevoerd en de gegevens in het herstelpunt.

   U kunt het script uitvoeren op elke computer waarop het besturingssysteem hetzelfde (of compatibel) als de VM waarvan een back-up is gemaakt. Zie de [compatibel besturingssysteem tabel](backup-azure-restore-files-from-vm.md#system-requirements) voor besturingssystemen die compatibel is. Als de beveiligde virtuele machine van Azure gebruikmaakt van Windows Storage Spaces (voor Windows Azure-VM's) of LVM/RAID-matrices (voor Linux-VM's), kunt u het uitvoerbare bestand of script niet uitvoeren op dezelfde virtuele machine. In plaats daarvan het uitvoerbare bestand of script uitvoeren op elke andere computer met een compatibel besturingssysteem.

### <a name="identifying-volumes"></a>Identificeren van Volumes

#### <a name="for-windows"></a>Voor Windows

Wanneer u het uitvoerbare bestand uitvoert, wordt het besturingssysteem koppelt u de nieuwe volumes en stationsletters worden toegewezen. U kunt Windows Explorer of de Bestandenverkenner gebruiken om te bladeren van deze schijven. De stationsletters toegewezen aan de volumes mag niet dezelfde letters als de oorspronkelijke virtuele machine, maar de naam van het volume is behouden. Bijvoorbeeld, als het volume op de oorspronkelijke virtuele machine is "Data-schijf (E:`\`) ', dat volume kan worden gekoppeld op de lokale computer als ' gegevensschijf ('Willekeurige letter':`\`). Blader door alle volumes die worden vermeld in de uitvoer van het script totdat u uw bestanden en mappen.  

   ![Herstel-bestandsmenu](./media/backup-azure-restore-files-from-vm/volumes-attached.png)

#### <a name="for-linux"></a>Voor Linux

In Linux, worden de volumes van het herstelpunt gekoppeld naar de map waarin het script wordt uitgevoerd. De gekoppelde schijven, volumes en de bijbehorende koppelpunt paden worden dienovereenkomstig weergegeven. Deze paden koppelen zijn zichtbaar voor gebruikers met toegang tot de hoofdmap op. Blader door de volumes die worden vermeld in de uitvoer van het script.

  ![Menu voor herstel van de Linux-bestand](./media/backup-azure-restore-files-from-vm/linux-mount-paths.png)

## <a name="closing-the-connection"></a>De verbinding wordt gesloten

Verwijderen (of ontkoppelen) nadat de bestanden te identificeren en ze kopiëren naar een locatie voor de lokale opslag, op de extra stations. De schijven ontkoppelen op de **bestandsherstel** in Azure portal, klik daarna op **schijven ontkoppelen**.

![Schijven ontkoppelen](./media/backup-azure-restore-files-from-vm/unmount-disks3.png)

Nadat de schijven ontkoppeld zijn, ontvangt u een bericht. Het duurt een paar minuten voor de verbinding met het vernieuwen, zodat u de schijven kunt verwijderen.

In Linux, nadat de verbinding met het herstelpunt is verbroken, verwijderen het besturingssysteem niet de bijbehorende koppelpunt paden automatisch. Het koppelpunt paden bestaan als "zwevende" volumes en ze zichtbaar zijn, maar genereert een fout wanneer u toegang schrijftijd de bestanden. Ze kunnen handmatig worden verwijderd. Het script, wanneer uitvoert, identificeert deze volumes bestaande uit de vorige herstelpunten en ruimt bij toestemming.

## <a name="special-configurations"></a>Speciale configuraties

### <a name="dynamic-disks"></a>Dynamische schijven

Als de beveiligde Azure-VM volumes met één of beide van de volgende kenmerken heeft, kunt u het uitvoerbare bestand script niet uitvoeren op dezelfde virtuele machine.

- Volumes met betrekking meerdere schijven tot (spanned en striped volumes)
- Fouttolerante volumes (volumes met gespiegelde en RAID-5) op de dynamische schijven

In plaats daarvan de uitvoerbare script uitvoeren op een andere computer met een compatibel besturingssysteem.

### <a name="windows-storage-spaces"></a>Windows-opslagruimten

Windows-opslagruimten is een Windows-technologie waarmee u opslag virtualiseren. U kunt met Windows Storage Spaces industriestandaard schijven groeperen in opslaggroepen. Vervolgens gebruikt u de beschikbare ruimte in de opslaggroepen virtuele schijven genaamd opslagruimten maken.

Als de beveiligde Azure-VM gebruikmaakt van Windows Storage Spaces, kunt u het uitvoerbare bestand script niet uitvoeren op dezelfde virtuele machine. In plaats daarvan de uitvoerbare script uitvoeren op elke andere computer met een compatibel besturingssysteem.

### <a name="lvmraid-arrays"></a>LVM/RAID-matrices

In Linux, worden logische volume manager (LVM) en/of software-RAID-matrices gebruikt voor het beheren van logische volumes over meerdere schijven. Als de beveiligde Linux-VM gebruikmaakt van LVM en/of RAID-matrices, kunt u het script niet uitvoeren op dezelfde virtuele machine. In plaats daarvan het script uitvoert op een andere machine met een compatibel besturingssysteem en die ondersteuning biedt voor het bestandssysteem van de beveiligde virtuele machine.

De uitvoer van het volgende script geeft de schijven LVM en/of RAID-matrices en de volumes met het partitietype.

   ![Menu voor Linux LVM-uitvoer](./media/backup-azure-restore-files-from-vm/linux-LVMOutput.png)

Als u wilt deze partities online brengen, voer de opdrachten in de volgende secties.

#### <a name="for-lvm-partitions"></a>Voor LVM-partities

Aan de lijst van de namen van de groep volume onder een fysiek volume.

```bash
#!/bin/bash
$ pvs <volume name as shown above in the script output>
```

Aan de lijst met alle logische volumes, namen en de paden in de volumegroep van een.

```bash
#!/bin/bash
$ lvdisplay <volume-group-name from the pvs command’s results>
```

Koppelen van de logische volumes in het pad van uw keuze.

```bash
#!/bin/bash
$ mount <LV path> </mountpath>
```

#### <a name="for-raid-arrays"></a>Voor RAID-matrices

De volgende opdracht geeft details weer over alle raid-schijven.

```bash
#!/bin/bash
$ mdadm –detail –scan
```

 De relevante RAID-schijf wordt weergegeven als `/dev/mdm/<RAID array name in the protected VM>`

Gebruik de koppelopdracht als de schijf RAID fysieke volumes heeft.

```bash
#!/bin/bash
$ mount [RAID Disk Path] [/mountpath]
```

Als de RAID-schijf een andere LVM erin is geconfigureerd heeft, gebruikt u de voorgaande procedure voor LVM partities maar gebruikt u de naam van het volume in plaats van de naam van de RAID-schijf

## <a name="system-requirements"></a>Systeemvereisten

### <a name="for-windows-os"></a>Voor Windows-besturingssysteem

De volgende tabel ziet u de compatibiliteit tussen besturingssystemen-server en de computer. Tijdens het herstellen van bestanden, kunt u bestanden niet terugzetten naar een vorige of toekomstige besturingssysteemversie. U niet kunt bijvoorbeeld een bestand herstellen vanaf een virtuele machine met Windows Server 2016 naar Windows Server 2012 of een Windows 8-computer. Het dezelfde server-besturingssysteem, of het compatibel clientbesturingssysteem, kunt u bestanden herstellen van een virtuele machine.

|Serverbesturingssysteem | Compatibel besturingssysteem van client  |
| --------------- | ---- |
| Windows Server 2016    | Windows 10 |
| Windows Server 2012 R2 | Windows 8.1 |
| Windows Server 2012    | Windows 8  |
| Windows Server 2008 R2 | Windows 7   |

### <a name="for-linux-os"></a>Voor Linux-besturingssysteem

In Linux, moet het besturingssysteem van de computer die wordt gebruikt om bestanden te herstellen het bestandssysteem van de beveiligde virtuele machine ondersteunen. Bij het selecteren van een computer met het script uit te voeren, zorg ervoor dat de computer heeft een compatibel besturingssysteem, en maakt gebruik van een van de versies die zijn geïdentificeerd in de volgende tabel:

|Linux-besturingssysteem | Versies  |
| --------------- | ---- |
| Ubuntu | 12.04 en hoger |
| CentOS | 6.5 en hoger  |
| RHEL | 6.7 en hoger |
| Debian | 7 en hoger |
| Oracle Linux | 6.4 en hoger |
| SLES | 12 en hoger |
| openSUSE | 42.2 en hoger |

> [!Note]
> We hebben enkele problemen gevonden in het bestand recovery-script is uitgevoerd op virtuele machines met SLES 12 SP4 OS. Onderzoeken met SLES-team.
> Op dit moment, actief wordt de bestandsherstelscript werkt op virtuele machines met SLES 12 SP2 en SP3 OS-versies.
>

Het script Python en bash-onderdelen uit te voeren en veilig verbinding maken met het herstelpunt dat is ook vereist.

|Onderdeel | Versie  |
| --------------- | ---- |
| Bash | 4 en hoger |
| python | 2.6.6 en hoger  |
| TLS | 1.2 moet worden ondersteund  |

## <a name="troubleshooting"></a>Problemen oplossen

Als u problemen hebt bij het herstellen van bestanden van de virtuele machines, controleert u de volgende tabel voor meer informatie.

| Foutbericht / Scenario | Mogelijke oorzaak | Aanbevolen actie |
| ------------------------ | -------------- | ------------------ |
| Exe-uitvoer: *Verbinding maken met de doel-uitzondering* |Script is geen toegang krijgen tot het herstelpunt    | Controleer of de computer voldoet aan de vorige toegangsvereisten. |  
| Exe-uitvoer: *Het doel is al geregistreerd in via een iSCSI-sessie.* | Het script is al uitgevoerd op dezelfde computer en de stations zijn gekoppeld | De volumes van het herstelpunt dat zijn al gekoppeld. Ze kunnen niet worden gekoppeld met de dezelfde stationsletters van de oorspronkelijke virtuele machine. Blader door de beschikbare volumes in de Verkenner voor het bestand |
| Exe-uitvoer: *Met dit script is ongeldig omdat de schijven zijn ontkoppeld via de limiet voor de 12 uur per portal/overschreden. Een nieuw script downloaden via de portal.* |    De schijven zijn ontkoppeld van de portal of de 12 uur overschreden | Deze specifieke exe is nu ongeldig en kan niet worden uitgevoerd. Als u wilt toegang tot de bestanden van die recovery point-in-time, gaat u naar de portal voor een nieuwe exe-bestand|
| Op de machine waarop het exe-bestand wordt uitgevoerd: De nieuwe volumes zijn niet ontkoppeld nadat de dismount-knop wordt geklikt | De iSCSI-initiator op de machine is niet reageert/vernieuwen van de verbinding met het doel en onderhouden van de cache. |  Nadat u hebt geklikt **ontkoppelen**, wacht een paar minuten. Als de nieuwe volumes zijn niet ontkoppeld, blader door alle volumes. Bladeren door alle volumes zorgt ervoor dat de initiator te vernieuwen van de verbinding en het volume met een foutbericht weergegeven dat de schijf is niet beschikbaar is ontkoppeld.|
| Exe-uitvoer: Script met succes is uitgevoerd, maar 'Nieuwe volumes die zijn gekoppeld' wordt niet weergegeven op de uitvoer van het script |    Dit is een tijdelijke fout    | De volumes zou zijn al gekoppeld. Open Verkenner om te bladeren. Als u dezelfde computer gebruikt voor het uitvoeren van scripts telkens, houd rekening met de machine opnieuw opstarten en de lijst moet worden weergegeven in de volgende exe wordt uitgevoerd. |
| Linux-specifieke: Kan niet de gewenste volumes bekijken | Het onderliggende bestandssysteem van de beveiligde virtuele machine mogelijk niet herkend door het besturingssysteem van de machine waarop het script wordt uitgevoerd | Controleer of het herstelpunt dat crash-consistente of bestandsconsistente is. Als het bestand consistent is, het script uitvoert op een andere computer met OS herkend bestandssysteem van de beveiligde virtuele machine |
| Specifieke Windows: Kan niet de gewenste volumes bekijken | De schijven zijn gekoppeld, maar de volumes zijn niet geconfigureerd | Identificeer de extra schijven met betrekking tot het herstelpunt dat van het scherm voor het beheren van schijf. Als een van deze schijven offline status kunt u deze online maken met de rechtermuisknop op de schijf en klikt u op 'Online'|

## <a name="security"></a>Beveiliging

Deze sectie wordt besproken de verschillende beveiligingsmaatregelen getroffen voor de uitvoering van een bestand herstellen vanuit back-ups van virtuele machine van Azure, zodat gebruikers zich bewust zijn van de beveiligingsaspecten van de functie.

### <a name="feature-flow"></a>Functie-stroom

Deze functie is ontwikkeld om toegang tot de VM-gegevens zonder de noodzaak voor de hele VM of virtuele machine herstellen schijven en in de minimale stappen. Toegang tot gegevens van de virtuele machine wordt geleverd door een script (hiermee het herstelvolume wanneer uitvoert, zoals hieronder weergegeven) en kan daarom vormt de hoeksteen van alle implementaties voor beveiliging

  ![Beveiliging functie stroom](./media/backup-azure-restore-files-from-vm/vm-security-feature-flow.png)

### <a name="security-implementations"></a>Beveiliging-implementaties

#### <a name="select-recovery-point-who-can-generate-script"></a>Selecteer een herstelpunt (die kunt-script genereren)

Het script geeft toegang tot gegevens van de virtuele machine, het is belangrijk te reguleren die deze in de eerste plaats kunnen genereren. Een zich aanmeldt bij Azure-portal en moet [RBAC geautoriseerd](backup-rbac-rs-vault.md#mapping-backup-built-in-roles-to-backup-management-actions) kunnen voor het genereren van het script.

Het herstellen van bestanden moet dezelfde mate van autorisatie zoals vereist voor de virtuele machine terugzetten en herstellen van schijven. Met andere woorden, alleen geautoriseerde gebruikers kan de VM-gegevens weergeven kunt het script genereren.

Het gegenereerde script is ondertekend met officiële Microsoft-certificaat voor Azure Backup-service. Alle knoeien met het script dat betekent dat de handtekening verbroken is en wordt elke poging om uit te voeren van het script is gemarkeerd als een potentieel risico door het besturingssysteem.

#### <a name="mount-recovery-volume-who-can-run-script"></a>Koppelpunt herstelvolume (die script kan worden uitgevoerd)

Alleen-beheerder kan het script worden uitgevoerd en moet deze uitvoeren met verhoogde bevoegdheden. Het script wordt alleen wordt uitgevoerd een vooraf gegenereerde reeks stappen en accepteert geen invoer van een externe bron.

Het script wordt uitgevoerd, is een vereist een wachtwoord dat alleen voor de gemachtigde gebruiker op het moment van aanmaak van script in de Azure portal of PowerShell/CLI weergegeven wordt. Dit is om ervoor te zorgen dat de gemachtigde gebruiker die het script downloadt, hebben ook verantwoordelijk voor het script is uitgevoerd.

#### <a name="browse-files-and-folders"></a>Bladeren door bestanden en mappen

Als u bestanden en mappen, het script maakt gebruik van de iSCSI-initiator op de machine en verbinding maken met het herstelpunt dat is geconfigureerd als een iSCSI-doel. Hier een kunt ervan uitgaan dat het scenario's waarbij een probeert te imiteren/vervalsen ofwel/alle onderdelen.

We gebruiken wederzijdse CHAP-verificatiemechanisme zodat elk onderdeel de andere verifieert. Dit betekent dat het is zeer moeilijk voor een valse initiator verbinding maken met het iSCSI-doel en een valse doel zijn verbonden met de machine waarop het script wordt uitgevoerd.

De gegevensstroom tussen de recovery-service en de machine wordt beveiligd door het bouwen van een beveiligde SSL-tunnel via TCP ([TLS 1.2 moet worden ondersteund](#system-requirements) in de machine waarop het script wordt uitgevoerd)

Elk bestand lijst met ACL (Access Control) in de bovenliggende/back-ups van virtuele machine aanwezig blijven ook behouden in het gekoppelde bestandssysteem.

Het script geeft alleen-lezen toegang tot een herstelpunt en alleen 12 uur geldig is. Als de gebruiker wil de toegang eerder verwijderen, meldt u zich in Azure Portal/PowerShell/CLI en voer de **schijven ontkoppelen** voor die bepaalde herstelpunt. Het script wordt direct ongeldig worden gemaakt.
