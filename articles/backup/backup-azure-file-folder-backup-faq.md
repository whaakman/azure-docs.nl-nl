---
title: Veelgestelde vragen over de Azure Backup-agent | Microsoft Docs
description: Antwoorden op veelgestelde vragen over hoe de Azure Backup-agent werkt, en over limieten voor back-up en retentie.
services: backup
documentationcenter: 
author: trinadhk
manager: shreeshd
editor: 
keywords: back-up en herstel na noodgeval; Backup-service
ms.assetid: 778c6ccf-3e57-4103-a022-367cc60c411a
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/18/2017
ms.author: trinadhk;pullabhk;
ms.openlocfilehash: 227cdc87f3e2c8ed393145f4bbde7f74606bdf3b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="questions-about-the-azure-backup-agent"></a>Vragen over de Azure Backup-agent
In dit artikel vindt u antwoorden op veelgestelde vragen om u snel een beeld te geven van de verschillende onderdelen van de Azure Backup-agent. Sommige antwoorden bevatten koppelingen naar artikelen met uitgebreide informatie over het onderwerp. U kunt ook in het [discussieforum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazureonlinebackup) vragen over de Azure Backup-service plaatsen.

## <a name="configure-backup"></a>Back-up configureren
### <a name="where-can-i-download-the-latest-azure-backup-agent-br"></a>Waar kan ik de meest recente Azure Backup-agent downloaden? <br/>
U kunt [hier](http://aka.ms/azurebackup_agent) de meest recente agent downloaden voor back-ups van Windows Server, System Center DPM of Windows-client. Als u een back-up van een virtuele machine wilt maken, gebruikt u de VM-agent (die automatisch de juiste extensie installeert). Virtuele machines die zijn gemaakt via de Azure-galerie, beschikken al over de VM-agent.

### <a name="when-configuring-the-azure-backup-agent-i-am-prompted-to-enter-the-vault-credentials-do-vault-credentials-expire"></a>Wanneer ik de Azure Backup-agent configureer, moet ik de kluisreferenties opgeven. Verlopen kluisreferenties?
Ja, de kluisreferenties verlopen na 48 uur. Als het bestand is verlopen, meldt u zich aan bij de Azure Portal en downloadt u de kluisreferentiebestanden uit uw kluis.

### <a name="what-types-of-drives-can-i-back-up-files-and-folders-from-br"></a>Van welke typen stations kan ik back-ups van bestanden en mappen maken? <br/>
U kunt geen back-up maken van de volgende stations/volumes:

* Verwisselbare media: de bron van alle back-upitems moet worden gerapporteerd als vast.
* Alleen-lezenvolumes: de Volume Shadow Copy Service (VSS) werkt alleen als het volume schrijfbaar is.
* Offlinevolumes: VSS werkt alleen als het volume online is.
* Netwerkshare: er kan alleen een online back-up worden gemaakt wanneer het volume zich lokaal op de server bevindt.
* Met BitLocker beveiligde volumes: het volume moet worden ontgrendeld voordat er een back-up kan worden gemaakt.
* Bestandssysteemidentificatie: NTFS is het enige bestandssysteem dat wordt ondersteund.

### <a name="what-file-and-folder-types-can-i-back-up-from-my-serverbr"></a>Van welke typen bestanden en mappen op mijn server kan ik een back-up maken?<br/>
De volgende typen worden ondersteund:

* Versleuteld
* Gecomprimeerd
* Sparse
* Gecomprimeerd + Sparse
* Vaste koppelingen: niet ondersteund, worden overgeslagen
* Reparsepunt: niet ondersteund, wordt overgeslagen
* Versleuteld + sparse: niet ondersteund, wordt overgeslagen
* Gecomprimeerde stream: niet ondersteund, wordt overgeslagen
* Sparse stream: niet ondersteund, wordt overgeslagen

### <a name="can-i-install-the-azure-backup-agent-on-an-azure-vm-already-backed-by-the-azure-backup-service-using-the-vm-extension-br"></a>Kan ik de Azure Backup-agent installeren op een virtuele machine van Azure waarvan de Azure Backup-service al een back-up heeft gemaakt met de VM-extensie? <br/>
Absoluut. Azure Backup biedt back-ups op VM-niveau voor de virtuele machines van Azure door gebruik te maken van de VM-extensie. Installeer de Azure Backup-agent op het Windows-gastbesturingssysteem om bestanden en mappen op dat gastbesturingssysteem te beveiligen.

### <a name="can-i-install-the-azure-backup-agent-on-an-azure-vm-to-back-up-files-and-folders-present-on-temporary-storage-provided-by-the-azure-vm-br"></a>Kan ik de Azure Backup-agent op een virtuele machine van Azure installeren om back-ups van bestanden en mappen te maken die zich in de tijdelijke opslag van de virtuele machine van Azure bevinden? <br/>
Ja. Installeer de Azure Backup-agent op het Windows-gastbesturingssysteem en maak back-ups van bestanden en mappen naar de tijdelijke opslag. Houd er rekening mee dat back-ups mislukken als de tijdelijke opslaggegevens worden gewist. Bovendien kunt u alleen herstelbewerkingen naar een niet-vluchtige opslag uitvoeren als de tijdelijke opslaggegevens zijn verwijderd.

### <a name="whats-the-minimum-size-requirement-for-the-cache-folder-br"></a>Wat is de vereiste minimumgrootte voor de cachemap? <br/>
De grootte van de cachemap bepaalt de hoeveelheid gegevens waarvan u een back-up maakt. Uw cachemap moet 5% van de benodigde ruimte voor de opslag van gegevens zijn.

### <a name="how-do-i-register-my-server-to-another-datacenterbr"></a>Hoe kan ik mijn server bij een ander datacenter registreren?<br/>
De back-upgegevens worden verzonden naar het datacenter van de kluis waarbij de server is geregistreerd. De eenvoudigste manier om het datacenter te wijzigen, is de agent te verwijderen en opnieuw te installeren en u vervolgens te registreren bij een nieuwe kluis die behoort bij het gewenste datacenter.

### <a name="does-the-azure-backup-agent-work-on-a-server-that-uses-windows-server-2012-deduplication-br"></a>Werkt de Azure Backup-agent op een server die gegevensontdubbeling van Windows Server 2012 gebruikt? <br/>
Ja. De agentservice converteert de ontdubbelde gegevens naar normale gegevens wanneer de back-upbewerking wordt voorbereid. Vervolgens worden de gegevens geoptimaliseerd voor het maken van een back-up en versleuteld en worden de versleutelde gegevens verzonden naar de online back-upservice.

## <a name="backup"></a>Back-up maken
### <a name="how-do-i-change-the-cache-location-specified-for-the-azure-backup-agentbr"></a>Hoe kan ik de cachelocatie wijzigen die is opgegeven voor de Azure Backup-agent?<br/>
Gebruik de volgende lijst om de cachelocatie te wijzigen.

1. Stop de Backup-engine door de volgende opdracht uit te voeren via een opdrachtprompt met verhoogde bevoegdheid:

    ```PS C:\> Net stop obengine``` 
  
2. Verplaats de bestanden niet. Kopieer in plaats daarvan de map met de cacheruimte naar een ander station met voldoende ruimte. De oorspronkelijke cacheruimte kan worden verwijderd nadat is bevestigd dat de back-ups werken met de nieuwe cacheruimte.
3. Werk de volgende registervermeldingen bij met het pad naar de nieuwe map van de cacheruimte.<br/>

    | Registerpad | Registersleutel | Waarde |
    | --- | --- | --- |
    | `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config` |ScratchLocation |*Nieuwe locatie van de cachemap* |
    | `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider` |ScratchLocation |*Nieuwe locatie van de cachemap* |

4. Start de Backup-engine opnieuw door de volgende opdracht uit te voeren via een opdrachtprompt met verhoogde bevoegdheid:

    ```PS C:\> Net start obengine```

Zodra de back-up op de nieuwe cachelocatie is gemaakt, kunt u de oorspronkelijke cachemap verwijderen.


### <a name="where-can-i-put-the-cache-folder-for-the-azure-backup-agent-to-work-as-expectedbr"></a>Waar moet ik de cachemap voor de Azure Backup-agent plaatsen om ervoor te zorgen dat deze naar verwachting werkt?<br/>
De volgende locaties worden niet aanbevolen voor de cachemap:

* Netwerkshare of verwisselbare media: de cachemap moet zich lokaal op de server bevinden waarvan back-ups moeten worden gemaakt via onlineback-up. Netwerklocaties of verwisselbare media zoals USB-stations worden niet ondersteund.
* Offlinevolumes: de cachemap moet online zijn voor de verwachte back-up met de Azure Backup-agent.

### <a name="are-there-any-attributes-of-the-cache-folder-that-are-not-supportedbr"></a>Zijn er kenmerken van de cachemap die niet worden ondersteund?<br/>
De volgende kenmerken of combinaties van deze kenmerken worden niet ondersteund voor de cachemap:

* Versleuteld
* Ontdubbeld
* Gecomprimeerd
* Sparse
* Reparsepunt

De cachemap en de metagegevens-VHD hebben beide niet de benodigde kenmerken voor de Azure Backup-agent.

### <a name="is-there-a-way-to-adjust-the-amount-of-bandwidth-used-by-the-backup-servicebr"></a>Is er een manier om de hoeveelheid bandbreedte aan te passen die door de Backup-service wordt gebruikt?<br/>
  Ja, u kunt in de Backup-agent de optie **Eigenschappen wijzigen** gebruiken om de bandbreedte aan te passen. U kunt de hoeveelheid bandbreedte aanpassen, evenals de tijden waarop deze bandbreedte wordt gebruikt. Zie **[Enable network throttling](backup-configure-vault.md#enable-network-throttling)** (Netwerkbeperking inschakelen) voor stapsgewijze instructies.

## <a name="manage-backups"></a>Back-ups beheren
### <a name="what-happens-if-i-rename-a-windows-server-that-is-backing-up-data-to-azurebr"></a>Wat gebeurt er als ik de naam van een Windows-server wijzig waarmee back-ups van gegevens naar Azure worden opgeslagen?<br/>
Wanneer u de naam van een server wijzigt, worden alle back-ups die momenteel zijn geconfigureerd, gestopt.
Registreer de nieuwe naam van de server bij de back-upkluis. Als u de nieuwe naam bij de kluis registreert, is de eerste back-upbewerking een *volledige* back-up. Als u gegevens moet herstellen waarvan een back-up naar de kluis is gemaakt met de oude servernaam, kunt u deze gegevens herstellen met de optie [**Andere server**](backup-azure-restore-windows-server.md#use-instant-restore-to-restore-data-to-an-alternate-machine) in de wizard **Gegevens herstellen**.

### <a name="what-is-the-maximum-file-path-length-that-can-be-specified-in-backup-policy-using-azure-backup-agent-br"></a>Wat is de maximale lengte voor een bestandspad dat met de Azure Backup-agent kan worden opgegeven als onderdeel van het Backup-beleid? <br/>
De Azure Backup-agent is afhankelijk van NTFS. De [lengtespecificatie van het bestandspad wordt beperkt door de Windows-API](https://msdn.microsoft.com/library/aa365247.aspx#fully_qualified_vs._relative_paths). Als u bestanden wilt beveiligen met een bestandspad dat langer is dan toegestaan door de Windows-API, kunt u een back-up van de bovenliggende map of het schijfstation maken.  

### <a name="what-characters-are-allowed-in-file-path-of-azure-backup-policy-using-azure-backup-agent-br"></a>Welke tekens zijn toegestaan in het bestandspad van het Azure Backup-beleid met Azure Backup-agent? <br>
 De Azure Backup-agent is afhankelijk van NTFS. Er worden [NTFS-ondersteunde tekens](https://msdn.microsoft.com/library/aa365247.aspx#naming_conventions) ingeschakeld als onderdeel van de bestandsspecificatie. 
 
### <a name="i-receive-the-warning-azure-backups-have-not-been-configured-for-this-server-even-though-i-configured-a-backup-policy-br"></a>Ik krijg een waarschuwing dat er geen Azure-back-ups zijn geconfigureerd voor deze server, terwijl ik wel een back-upbeleid heb geconfigureerd. <br/>
Deze waarschuwing wordt weergegeven wanneer de instellingen voor het back-upschema die zijn opgeslagen op de lokale server niet overeenkomen met de instellingen die zijn opgeslagen in de back-upkluis. Wanneer de server of de instellingen zijn hersteld naar een bekende goede status, worden de back-upschema's mogelijk niet meer gesynchroniseerd. Als u deze waarschuwing ontvangt, [configureert u het back-upbeleid opnieuw](backup-azure-manage-windows-server.md) en **voert u een back-up uit** om de lokale server te synchroniseren met Azure.
