---
title: 'Azure Backup: Bestanden en mappen herstellen van een virtuele machine van Azure back-up | Microsoft Docs'
description: Bestanden herstellen vanaf een herstelpunt voor virtuele machine van Azure
services: backup
documentationcenter: dev-center-name
author: pvrk
manager: shivamg
keywords: herstel op itemniveau; Bestandsherstel van back-up van virtuele machine van Azure. bestanden herstellen van de virtuele machine in Azure
ms.assetid: f1c067a2-4826-4da4-b97a-c5fd6c189a77
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 12/20/2017
ms.author: pullabhk;markgal
ms.openlocfilehash: d1ebda145b7e355bd9763025dece742d2a23239b
ms.sourcegitcommit: 384d2ec82214e8af0fc4891f9f840fb7cf89ef59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/16/2018
---
# <a name="recover-files-from-azure-virtual-machine-backup"></a>Bestanden herstellen vanuit back-up van virtuele machine van Azure

Azure Backup biedt de mogelijkheid om te herstellen [Azure virtual machines (VM's) en schijven](./backup-azure-arm-restore-vms.md) van back-ups van virtuele machine in Azure, ook wel bekend als herstelpunten. In dit artikel wordt uitgelegd hoe bestanden en mappen herstellen van een virtuele machine van Azure back-up. Bestanden en mappen terugzetten is alleen beschikbaar voor virtuele Azure-machines geïmplementeerd met behulp van de Resource Manager-model en beveiligd door een Recovery services-kluis.

> [!Note]
> Herstel van bestanden vanuit een versleutelde VM back-up wordt niet ondersteund.
>

## <a name="mount-the-volume-and-copy-files"></a>Koppelen van het volume en bestanden wilt kopiëren

Als u bestanden of mappen herstellen van het herstelpunt, gaat u naar de virtuele machine en kiest u het herstelpunt. 

1. Meld u aan bij de [Azure-portal](http://portal.Azure.com) en klik in het menu links op **virtuele machines**. Selecteer in de lijst van virtuele machines, de virtuele machine om die virtuele machine dashboard te openen. 

2. Klik in het menu van de virtuele machine op **back-up** om de back-up-dashboard te openen.

    ![Recovery Services-kluis back-item openen](./media/backup-azure-restore-files-from-vm/open-vault-from-vm.png)

3. Klik in het menu van het dashboard back-up op **bestandsherstel** om het menu te openen.

    ![Bestandsmenu-herstel](./media/backup-azure-restore-files-from-vm/file-recovery-blade.png)

4. Van de **herstelpunt selecteren** vervolgkeuzelijst, selecteer het herstelpunt dat u de gewenste bestanden bevat. Het laatste herstelpunt is standaard al geselecteerd.

5. Klik voor het downloaden van de software die wordt gebruikt om bestanden te kopiëren van het herstelpunt dat **uitvoerbaar bestand downloaden** (voor Windows Azure VM) of **downloadscript** (voor Linux Azure VM). 

    ![Gegenereerd wachtwoord](./media/backup-azure-restore-files-from-vm/download-executable.png)

    Azure downloadt het uitvoerbare bestand of script naar de lokale computer.

    ![bericht voor het uitvoerbare bestand of script downloaden](./media/backup-azure-restore-files-from-vm/run-the-script.png)

    Als u wilt het uitvoerbare bestand of script uitvoeren als beheerder, het beste opslaan van de download op uw computer.

6. Het uitvoerbare bestand of script is beveiligd met een wachtwoord en een wachtwoord is vereist. In de **bestandsherstel** menu, klikt u op de knop kopiëren om het wachtwoord in het geheugen laden.

    ![Gegenereerd wachtwoord](./media/backup-azure-restore-files-from-vm/generated-pswd.png)

7. Vanaf de downloadlocatie (meestal de map Downloads) met de rechtermuisknop op het uitvoerbare bestand of script en voer dit met beheerdersreferenties. Wanneer u wordt gevraagd, typt u het wachtwoord of plak het wachtwoord van het geheugen en druk op Enter. Zodra het geldige wachtwoord wordt ingevoerd, is het script maakt verbinding met het herstelpunt.

    ![Bestandsmenu-herstel](./media/backup-azure-restore-files-from-vm/executable-output.png)

    Als u het script op een computer met beperkte toegang uitvoert, zorg dat er toegang tot:

    - download.microsoft.com
    - [Azure-eindpunten die wordt gebruikt voor back-ups van virtuele machine in Azure](backup-azure-arm-vms-prepare.md#establish-network-connectivity)
    - uitgaande poort 3260

    Het script vereist voor Linux 'open-iscsi' en 'lshw' onderdelen verbinding maken met het herstelpunt. Als de componenten niet bestaan op de computer waarop het script wordt uitgevoerd, wordt het script vraagt om de machtiging om de onderdelen te installeren. Geef toestemming voor het installeren van de vereiste onderdelen.
    
    De toegang tot download.microsoft.com is vereist voor het downloaden van de onderdelen die worden gebruikt voor het bouwen van een beveiligd kanaal tussen de computer waarop het script wordt uitgevoerd en de gegevens in het herstelpunt.         

    U kunt het script uitvoeren op elke computer waarop het besturingssysteem hetzelfde (of compatibel) als de back-up-VM. Zie de [compatibel besturingssysteem tabel](backup-azure-restore-files-from-vm.md#system-requirements) voor compatibele besturingssystemen. Als de beveiligde virtuele machine van Azure Windows Storage Spaces (voor Windows Azure VM's) of LVM/RAID-Arrays (voor Linux VM's) gebruikt, kunt u het uitvoerbare bestand of script niet uitvoeren op dezelfde virtuele machine. In plaats daarvan het uitvoerbare bestand of script uitvoeren op een andere machine met een compatibel besturingssysteem.
 

### <a name="identifying-volumes"></a>Identificeren van Volumes

#### <a name="for-windows"></a>Voor Windows

Wanneer u het uitvoerbare bestand uitvoert, wordt het besturingssysteem koppelt de nieuwe volumes en stationsletters worden toegewezen. U kunt Windows Verkenner of File Explorer gebruiken om te bladeren die stations. De stationsletters toegewezen aan de volumes mogelijk niet dezelfde letters als de oorspronkelijke virtuele machine, maar de naam van het volume wordt behouden. Als het volume op de oorspronkelijke virtuele machine is bijvoorbeeld "gegevensschijf (E:`\`) ', dat het volume kan worden gekoppeld op de lokale computer als ' gegevensschijf ('Willekeurige letter':`\`). Door alle volumes die worden vermeld in de scriptuitvoer totdat u de bestanden/map bladeren.  
       
   ![Bestandsmenu-herstel](./media/backup-azure-restore-files-from-vm/volumes-attached.png)
           
#### <a name="for-linux"></a>Voor Linux

In Linux, worden de volumes van het herstelpunt dat is gekoppeld naar de map waarin het script wordt uitgevoerd. De gekoppelde schijven, volumes en de bijbehorende mount-paden worden dienovereenkomstig weergegeven. Deze paden koppelen zijn zichtbaar voor gebruikers met toegang tot de hoofdmap niveau. Blader door de volumes die zijn vermeld in de uitvoer van het script.

  ![Herstel van Linux bestandsmenu](./media/backup-azure-restore-files-from-vm/linux-mount-paths.png)
  

## <a name="closing-the-connection"></a>De verbinding wordt gesloten

Na identificatie van de bestanden en ze te kopiëren naar een locatie van lokale opslag, verwijderen of ontkoppel de extra stations. Ontkoppelen van de stations op de **bestandsherstel** menu in de Azure-portal klikt u op **schijven ontkoppelen**.

![Ontkoppel de schijven](./media/backup-azure-restore-files-from-vm/unmount-disks3.png)

Zodra de schijven ontkoppeld zijn, krijgt u een bericht laten weten dat goed is verlopen. Duurt enkele minuten duren voordat de verbinding met het vernieuwen, zodat u de schijven kunt verwijderen.

In Linux, nadat de verbinding met het herstelpunt dat is verbroken, verwijderen het besturingssysteem niet de bijbehorende mount-paden automatisch. De paden koppelpunt bestaan als 'zwevende' volumes en ze worden weergegeven maar genereert een fout wanneer u toegang schrijftijd de bestanden. Ze kunnen handmatig worden verwijderd. Het script, wanneer uitgevoerd, worden dergelijke volumes bestaande uit de vorige herstelpunten identificeert en ruimt bij toestemming.

## <a name="special-configurations"></a>Speciale configuraties

### <a name="dynamic-disks"></a>Dynamische schijven

Als de beveiligde virtuele machine van Azure volumes met één of beide van de volgende kenmerken heeft, kunt u het uitvoerbare script niet uitvoeren op dezelfde virtuele machine. 

  - Volumes waarop span meerdere schijven (spanned en striped volumes)
  - Fouttolerante volumes (volumes mirrored en RAID-5) op dynamische schijven 

In plaats daarvan het uitvoerbare script uitvoeren op een andere computer met een compatibel besturingssysteem.

### <a name="windows-storage-spaces"></a>Windows-opslagruimten

Windows Storage Spaces is een Windows-technologie waarmee u opslag virtualiseren. U kunt met Windows Storage Spaces industriestandaard schijven groeperen in opslaggroepen. Vervolgens kunt u de beschikbare ruimte in de opslaggroepen virtuele schijven, opslagruimten genaamd, maken.

Als de beveiligde virtuele machine in Azure maakt gebruik van Windows Storage Spaces, kunt u het uitvoerbare script niet uitvoeren op dezelfde virtuele machine. In plaats daarvan het uitvoerbare script uitvoeren op een andere machine met een compatibel besturingssysteem.

### <a name="lvmraid-arrays"></a>LVM/RAID-Arrays

Logische volumebeheer (LVM) en/of software-RAID-matrices in Linux, worden gebruikt voor het logische volumes beheren via meerdere schijven. Als de beveiligde Linux VM LVM en/of RAID-Arrays gebruikt, kunt u het script niet uitvoeren op dezelfde virtuele machine. In plaats daarvan het script uitvoert op een andere machine met een compatibel besturingssysteem en die ondersteuning biedt voor het bestandssysteem van de beveiligde virtuele machine.

De uitvoer van het volgende script geeft de LVM en/of RAID-Arrays schijven en de volumes met het partitietype.

   ![Linux LVM uitvoer menu](./media/backup-azure-restore-files-from-vm/linux-LVMOutput.png)
   
Voer de opdrachten in de volgende secties voor het maken van deze partities online. 

**Voor LVM partities**

Voor een lijst met namen voor de volume onder een fysiek volume.
```
$ pvs <volume name as shown above in the script output> 
```
Voor een lijst met alle logische volumes, namen en de paden in de groep van een volume.

```
$ lvdisplay <volume-group-name from the pvs command’s results> 
```

Koppelen van de logische volumes aan het pad van uw keuze.

```
$ mount <LV path> </mountpath>
```



**Voor RAID-Arrays**

De volgende opdracht geeft details weer over alle raid-schijven.

```
$ mdadm –detail –scan
```
 De relevante RAID-schijf wordt weergegeven als`/dev/mdm/<RAID array name in the protected VM>`

Gebruik de koppelopdracht als de schijf RAID fysieke volumes heeft.
```
$ mount [RAID Disk Path] [/mountpath]
```

Als de schijf RAID een andere LVM geconfigureerd in het heeft, gebruikt u de voorgaande procedure voor LVM partities maar gebruikt u de naam van het volume in plaats van de naam van de RAID-schijf

## <a name="system-requirements"></a>Systeemvereisten

### <a name="for-windows"></a>Voor Windows

De volgende tabel ziet u de compatibiliteit tussen server en de computer-besturingssystemen. Tijdens het herstellen van bestanden, kunt u bestanden niet terugzetten naar een vorige of toekomstige besturingssysteemversie. U terugzetten niet bijvoorbeeld een bestand vanaf een Windows Server 2016 VM naar Windows Server 2012 of een computer met Windows 8. U kunt bestanden terugzetten van een virtuele machine op hetzelfde server-besturingssysteem, of op het besturingssysteem compatibel client.   

|Server OS | Compatibel clientbesturingssysteem  |
| --------------- | ---- |
| Windows Server 2016    | Windows 10 |
| Windows Server 2012 R2 | Windows 8.1 |
| Windows Server 2012    | Windows 8  |
| Windows Server 2008 R2 | Windows 7   |

### <a name="for-linux"></a>Voor Linux

In Linux, moet het besturingssysteem van de computer die wordt gebruikt om bestanden te herstellen het bestandssysteem van de beveiligde virtuele machine ondersteunen. Als u een computer met het script wilt uitvoeren, moet u de computer heeft een compatibel besturingssysteem en maakt gebruik van een van de versies die zijn geïdentificeerd in de volgende tabel:

|Linux-besturingssysteem | Versies  |
| --------------- | ---- |
| Ubuntu | 12.04 en hoger |
| CentOS | 6.5 en hoger  |
| RHEL | 6.7 en hoger |
| Debian | 7 en hoger |
| Oracle Linux | 6.4 en hoger |
| SLES | 12 en hoger |
| openSUSE | 42,2 en hoger |

Het script is ook vereist voor Python en bash-onderdelen uit te voeren en veilige verbinding met het herstelpunt.

|Onderdeel | Versie  |
| --------------- | ---- |
| Bash | 4 en hoger |
| python | 2.6.6 en hoger  |
| TLS | 1.2 moeten worden ondersteund.  |

## <a name="troubleshooting"></a>Problemen oplossen

Als u problemen tijdens het herstellen van bestanden vanuit de virtuele machines hebt, controleert u de volgende tabel voor meer informatie.

| Foutbericht / Scenario | Mogelijke oorzaak | Aanbevolen actie |
| ------------------------ | -------------- | ------------------ |
| Exe-uitvoer: *uitzondering verbinding te maken met het doel* |Script kan geen toegang tot het herstelpunt | Controleer of de computer voldoet aan de vorige toegangsvereisten. |  
|   Exe-uitvoer: *het doel is al aangemeld via een iSCSI-sessie.* | Het script al is uitgevoerd op dezelfde computer en de stations zijn gekoppeld | De volumes van het herstelpunt dat zijn al gekoppeld. Ze kunnen niet worden gekoppeld met de dezelfde stationsletters van de oorspronkelijke virtuele machine. Blader door de beschikbare volumes in de Verkenner voor het bestand |
| Exe-uitvoer: *dit script is ongeldig omdat de schijven via het 12 uur per portal/overschreden limiet hebt ontkoppeld. Download een nieuw script via de portal.* | De schijven hebt ontkoppeld van de portal of de 12 uur is overschreden |    Deze bepaalde exe is nu ongeldig en kan niet worden uitgevoerd. Als u toegang wilt tot de bestanden van die recovery point-in-time, gaat u naar de portal voor een nieuwe exe-bestand|
| Op de computer waar het exe-bestand wordt uitgevoerd: de nieuwe volumes zijn niet ontkoppeld nadat de ontkoppeling uit-knop wordt geklikt |    De iSCSI-initiator op de machine niet reageert/vernieuwen van de verbinding met het doel en onderhouden van de cache |    Wacht enkele minuten nadat de ontkoppeling uit-knop wordt ingedrukt. Als de nieuwe volumes worden nog steeds niet ontkoppeld, moet u bladeren door middel van alle volumes. Dit zorgt ervoor dat de initiator vernieuwen van de verbinding en het volume met een foutbericht weergegeven dat de schijf is niet beschikbaar is ontkoppeld|
| Exe-uitvoer: Script met succes is uitgevoerd, maar 'Nieuwe volumes gekoppeld' wordt niet weergegeven in de uitvoer van het script | Dit is een tijdelijke fout   | De volumes zou zijn al gekoppeld. Open Verkenner om te bladeren. Als u dezelfde machine voor het uitvoeren van scripts elke keer gebruikt, kunt u de machine en de lijst opnieuw te starten moet worden weergegeven in de volgende exe wordt uitgevoerd. |
| Linux-specifieke: kan niet de gewenste volumes bekijken | Het onderliggende bestandssysteem van de beveiligde virtuele machine mogelijk niet herkend door het besturingssysteem van de computer waarop het script wordt uitgevoerd | Controleer of het herstelpunt dat crashes consistent of bestandsconsistente. Als het bestand consistent is, voer het script op een andere computer waarvan OS herkent van de beveiligde VM bestandssysteem |
| Windows-specifieke: kan niet de gewenste volumes bekijken | De schijven zijn gekoppeld, maar de volumes zijn niet geconfigureerd | Identificeer de extra schijven die zijn gerelateerd aan het herstelpunt van het scherm voor het beheer van schijf. Als een van deze schijven offline status kunt u ze online maken met de rechtermuisknop op de schijf op en klik op 'Online'|
