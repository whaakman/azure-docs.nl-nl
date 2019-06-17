---
title: Veelgestelde vragen over back-ups van bestanden en mappen met Azure Backup
description: Veelgestelde vragen over back-ups van bestanden en mappen met Azure Backup-adressen.
author: dcurwin
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 05/28/2019
ms.author: dacurwin
ms.openlocfilehash: 56dc87b1cdf36d761c46133004a05f8fa225a091
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66808297"
---
# <a name="common-questions-about-backing-up-files-and-folders"></a>Veelgestelde vragen over back-ups van bestanden en mappen 

In dit artikel vindt u antwoorden op veelgestelde vragen over alle back-ups van bestanden en mappen met de Microsoft Azure Recovery Services Agent (MARS) in de [Azure Backup](backup-overview.md) service.

## <a name="general"></a>Algemeen

### <a name="why-does-the-mars-agent-need-net-framework-452-or-higher"></a>Waarom de MARS-agent moet .NET framework 4.5.2 of hoger?

Nieuwe functionaliteit die beschikbaar is in [instant restore](backup-azure-restore-windows-server.md#use-instant-restore-to-recover-data-to-the-same-machine) moet .NET Framework 4.5.2 of hoger.

## <a name="configure-backups"></a>Back-ups configureren

### <a name="where-can-i-download-the-latest-version-of-the-mars-agent"></a>Waar kan ik de meest recente versie van de MARS-agent downloaden? 
De meest recente MARS-agent gebruikt bij het back-ups van Windows Server-machines, System Center DPM en Microsoft Azure Backup server is beschikbaar voor [downloaden](https://aka.ms/azurebackup_agent). 

### <a name="how-long-are-vault-credentials-valid"></a>Hoe lang zijn ongeldige kluisreferenties?
De kluisreferenties verlopen na 48 uur. Als het referentiebestand is verlopen, download u het bestand opnieuw vanuit de Azure-portal.

### <a name="from-what-drives-can-i-back-up-files-and-folders"></a>Van welke stations kan ik back-up van bestanden en mappen? 

U kunt geen back-up van de volgende typen schijven en volumes:

* Verwisselbare media: Alle back-upitem bronnen moeten worden gerapporteerd als vast.
* Alleen-lezen volumes: Het volume moet beschrijfbaar zijn voor de volume shadow copy service (VSS) om te werken.
* Offlinevolumes: Het volume moet online VSS functie zijn.
* Netwerkshares: Het volume moet lokaal op de server naar een back-up met behulp van online back-up.
* Met BitLocker beveiligde volumes: Het volume moet worden ontgrendeld voordat de back-up kan plaatsvinden.
* Bestandssysteemidentificatie: NTFS is het enige bestandssysteem dat wordt ondersteund.

### <a name="what-file-and-folder-types-are-supported"></a>Welke typen bestanden en mappen worden ondersteund?

De volgende typen worden ondersteund:

* Versleuteld
* Gecomprimeerd
* Sparse
* Gecomprimeerd + Sparse
* Vaste koppelingen: Niet ondersteund, wordt overgeslagen
* Reparsepunt: Niet ondersteund, wordt overgeslagen
* Versleuteld + Sparse: Niet ondersteund, wordt overgeslagen
* Gecomprimeerde Stream: Niet ondersteund, wordt overgeslagen
* Reparsepunten, inclusief DFS-koppelingen en koppelingspunten


### <a name="can-i-use-the-mars-agent-to-back-up-files-and-folders-on-an-azure-vm"></a>Kan ik de MARS-agent gebruiken voor back-up van bestanden en mappen op een Azure VM?  
Ja. Azure Backup biedt back-up van VM-niveau voor Azure VM's met behulp van de VM-extensie voor de Azure VM-agent. Als u back-up van bestanden en mappen op het gastbesturingssysteem van Windows op de virtuele machine wilt, kunt u de MARS-agent om dit te doen installeren. 

### <a name="can-i-use-the-mars-agent-to-back-up-files-and-folders-on-temporary-storage-for-the-azure-vm"></a>Kan ik de MARS-agent gebruiken voor back-up van bestanden en mappen in de tijdelijke opslag voor de virtuele machine van Azure? 
Ja. De MARS-agent installeren en back-up van bestanden en mappen op het gastbesturingssysteem van Windows naar de tijdelijke opslag. -Back-uptaken mislukken over tijdelijke opslag gegevens zijn gewist.
- Als de tijdelijke opslaggegevens wordt verwijderd, kunt u alleen herstellen naar een niet-vluchtige opslag.

### <a name="how-do-i-register-a-server-to-another-region"></a>Hoe kan ik een server naar een andere regio registreren?

Back-upgegevens worden verzonden naar het datacenter van de kluis waarin de server is geregistreerd. De eenvoudigste manier om het datacenter te wijzigen is verwijderen en opnieuw installeren van de agent en registreert de machine naar een nieuwe kluis in de regio die u nodig hebt

### <a name="does-the-mars-agent-support-windows-server-2012-deduplication"></a>De MARS-agent ondersteuning voor Windows Server 2012 Ontdubbeling?
Ja. De MARS-agent converteert de ontdubbelde gegevens naar normale gegevens wanneer deze de back-upbewerking wordt voorbereid. Deze en optimaliseert de gegevens voor back-up, versleutelt de gegevens en verzendt vervolgens de versleutelde gegevens naar de kluis.

## <a name="manage-backups"></a>Back-ups beheren

### <a name="what-happens-if-i-rename-a-windows-machine-configured-for-backup"></a>Wat gebeurt er als ik een Windows-machine die is geconfigureerd voor back-up wijzigen?

Wanneer u de naam van een Windows-machine, worden alle back-ups op dat moment geconfigureerde gestopt.

- U moet de naam van de nieuwe computer registreren bij de Backup-kluis.
- Als u de nieuwe naam bij de kluis registreert, wordt de eerste bewerking wordt een *volledige* back-up.
- Als u nodig hebt om een back-up naar de kluis met de naam van de oude gegevens te herstellen, moet u de optie gebruiken om te herstellen naar een andere locatie in de Wizard gegevens herstellen. [Meer informatie](backup-azure-restore-windows-server.md#use-instant-restore-to-restore-data-to-an-alternate-machine). 

### <a name="what-is-the-maximum-file-path-length-for-backup"></a>Wat is de maximale padlengte voor back-up?
De MARS-agent is afhankelijk van NTFS en maakt gebruik van de lengtespecificatie beperkt door de [Windows API](/windows/desktop/FileIO/naming-a-file#fully-qualified-vs-relative-paths). Als de bestanden die u wilt beveiligen zich langer zijn dan de toegestane waarde, maak een back-up van de bovenliggende map of het schijfstation.  

### <a name="what-characters-are-allowed-in-file-paths"></a>Welke tekens zijn toegestaan in bestandspaden?

De MARS-agent is afhankelijk van NTFS en kunt [-ondersteunde tekens](/windows/desktop/FileIO/naming-a-file#naming-conventions) in bestandspaden namen.

### <a name="the-warning-azure-backups-have-not-been-configured-for-this-server-appears"></a>De waarschuwing 'Azure back-ups zijn niet geconfigureerd voor deze server' wordt weergegeven.
Deze waarschuwing kan verschijnen, ondanks dat u hebt een back-upbeleid geconfigureerd wanneer de instellingen van de back-upschema die zijn opgeslagen op de lokale server niet overeenkomen met de instellingen die zijn opgeslagen in de back-upkluis.
- Wanneer de server of de instellingen zijn hersteld naar een bekende goede staat, is back-upschema's kunnen worden niet gesynchroniseerd.
- Als u deze waarschuwing ontvangt [configureren](backup-azure-manage-windows-server.md) de back-upbeleid opnieuw en voer een on-demand back-up opnieuw synchroniseren van de lokale server met Azure.


## <a name="manage-the-backup-cache-folder"></a>De cachemap van de back-ups beheren

### <a name="whats-the-minimum-size-requirement-for-the-cache-folder"></a>Wat is de vereiste minimumgrootte voor de cachemap?
De grootte van de cachemap bepaalt de hoeveelheid gegevens waarvan u een back-up maakt.
- De cache map volumes hebt vrije ruimte die gelijk is aan ten minste 5-10% van de totale grootte van back-upgegevens.
- Als het volume minder dan 5% vrije ruimte heeft, grootte van het volume vergroten of de cachemap verplaatsen naar een volume met voldoende ruimte.
- Als u back-up van systeemstatus van Windows, moet u een extra 30 en 35 GB aan vrije ruimte op het volume met de cachemap
### <a name="how-do-i-change-the-cache-location-for-the-mars-agent"></a>Hoe kan ik de cachelocatie voor de MARS-agent wijzigen?


1. Voer deze opdracht uit in een opdrachtprompt met verhoogde bevoegdheid om te stoppen van de Backup-engine:

    ```PS C:\> Net stop obengine```

2. Verplaats de bestanden niet. In plaats daarvan de cachemap van de ruimte kopiëren naar een ander station met voldoende ruimte.
3. Werk de volgende registervermeldingen bij met het pad van de cachemap van de nieuwe.<br/>

    | Registerpad | Registersleutel | Value |
    | --- | --- | --- |
    | `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config` |ScratchLocation |*Nieuwe locatie van de cachemap* |
    | `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider` |ScratchLocation |*Nieuwe locatie van de cachemap* |

4. Start opnieuw op de Backup-engine op een opdrachtprompt met verhoogde bevoegdheid:

    ```PS C:\> Net start obengine```

5. Nadat de back-up is voltooid met behulp van de nieuwe locatie, kunt u de oorspronkelijke cachemap verwijderen.


### <a name="where-should-the-cache-folder-be-located"></a>Waar de cachemap moet zich?

De volgende locaties worden niet aanbevolen voor de cachemap:

* Netwerk share/verwisselbare media: De cachemap moet lokaal op de server die een back-up met behulp van online back-up. Netwerklocaties of verwisselbare media zoals USB-stations worden niet ondersteund
* Offlinevolumes: De cachemap moet online zijn voor de verwachte back-up met behulp van Azure Backup Agent

### <a name="are-there-any-attributes-of-the-cache-folder-that-arent-supported"></a>Zijn er kenmerken van de cachemap die niet worden ondersteund?
De volgende kenmerken of combinaties van deze kenmerken worden niet ondersteund voor de cachemap:

* Versleuteld
* Ontdubbeld
* Gecomprimeerd
* Sparse
* Reparsepunt

De cachemap en de metagegevens-VHD hebben beide niet de benodigde kenmerken voor de Azure Backup-agent.

### <a name="is-there-a-way-to-adjust-the-amount-of-bandwidth-used-for-backup"></a>Is er een manier om aan te passen van de hoeveelheid bandbreedte die wordt gebruikt voor back-up?
 
Ja, u kunt de **eigenschappen wijzigen** optie in de MARS-agent voor het aanpassen van de bandbreedte en de timing. [Meer informatie](backup-configure-vault.md#enable-network-throttling)**.

## <a name="restore"></a>Herstellen

### <a name="what-happens-if-i-cancel-an-ongoing-restore-job"></a>Wat gebeurt er als ik een continue hersteltaak annuleren?

Als een lopende hersteltaak is geannuleerd, stopt het herstelproces. Alle bestanden hersteld voordat de annulering blijven in de geconfigureerde bestemming (oorspronkelijke of een alternatieve locatie), zonder eventuele terugdraaiacties.


## <a name="next-steps"></a>Volgende stappen

[Informatie over](tutorial-backup-windows-server-to-azure.md) hoe u back-up van een Windows-machine.
