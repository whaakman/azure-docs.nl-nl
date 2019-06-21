---
title: Problemen met het kopiëren van Azure Data Box-schijf oplossen van problemen met gegevens | Microsoft Docs
description: Beschrijft hoe u problemen met gezien tijdens het kopiëren van gegevens in Azure Data Box-schijf met behulp van Logboeken.
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: article
ms.date: 06/13/2019
ms.author: alkohli
ms.openlocfilehash: 760f5c6c929aa082993683d7a466a71c6484289a
ms.sourcegitcommit: 72f1d1210980d2f75e490f879521bc73d76a17e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/14/2019
ms.locfileid: "67148346"
---
# <a name="troubleshoot-data-copy-issues-in-azure-data-box-disk"></a>Oplossen van problemen met het kopiëren van gegevens in Azure Data Box-schijf

In dit artikel is van toepassing op Microsoft Azure Data Box-schijf en wordt beschreven hoe u het oplossen van problemen u ziet bij het kopiëren van de gegevens op schijven. Het artikel bevat ook problemen bij het gebruik van het hulpprogramma voor splitsen kopiëren.


## <a name="data-copy-issues-when-using-a-linux-system"></a>Gegevens kopiëren problemen bij het gebruik van een Linux-systeem

In deze sectie worden enkele van de belangrijkste problemen bij het gebruik van een Linux-client om gegevens te kopiëren naar schijven.

### <a name="issue-drive-getting-mounted-as-read-only"></a>Probleem: Schijf ophalen gekoppeld als alleen-lezen
 
**Oorzaak** 

Dit kan zijn vanwege een niet schoon bestandssysteem.

Een station als lezen / schrijven stationseigendom werkt niet met de Data Box-schijven. In dit scenario wordt niet ondersteund met schijven die door dislocker ontsleuteld. U kunt het apparaat met de volgende opdracht met succes hebt gekoppeld:

    `# mount -o remount, rw /mnt/DataBoxDisk/mountVol1`

Hoewel de stationseigendom voltooid is, blijft de gegevens niet actief.

**Resolutie**

Voer de volgende stappen uit op uw Linux-systeem:

1. Installeer de `ntfsprogs` -pakket voor het hulpprogramma ntfsfix.
2. Ontkoppel de koppelpunten van het station wordt geleverd door het hulpprogramma ontgrendelen. Het aantal koppelpunten varieert voor stations.

    ```
    unmount /mnt/DataBoxDisk/mountVol1
    ```

3. Voer `ntfsfix` op het bijbehorende pad. De gemarkeerde waarde moet gelijk zijn aan stap 2.

    ```
    ntfsfix /mnt/DataBoxDisk/bitlockerVol1/dislocker-file
    ```

4. Voer de volgende opdracht om te verwijderen van de metagegevens van de slaapstand en leiden het probleem koppelen tot kan.

    ```
    ntfs-3g -o remove_hiberfile /mnt/DataBoxDisk/bitlockerVol1/dislocker-file /mnt/DataBoxDisk/mountVol1
    ```

5. Voer een schone ontkoppelen.

    ```
    ./DataBoxDiskUnlock_x86_64 /unmount
    ```

6. Voer een schone ontgrendelen en koppelen.
7. Test het koppelpunt van het schrijven van een bestand.
8. Ontkoppel en voor het valideren van de bestandspersistentie koppelen.
9. Ga door met het kopiëren van gegevens.
 
### <a name="issue-error-with-data-not-persisting-after-copy"></a>Probleem: Fout met de gegevens niet permanent worden gemaakt na het kopiëren
 
**Oorzaak** 

Als u ziet dat het station geen gegevens nadat deze is ontkoppeld (Hoewel de gegevens zijn gekopieerd naar het), dan is het mogelijk dat u een station als lezen / schrijven opnieuw gekoppeld nadat het station is gekoppeld als alleen-lezen.

**Resolutie**
 
Als dit het geval is, raadpleegt u de oplossing voor [ophalen gekoppeld als alleen-lezen-stations](#issue-drive-getting-mounted-as-read-only).

Als dat niet het geval is, kopieert u de logboeken uit de map met het hulpprogramma Data Box Disk ontgrendelen en [Neem contact op met Microsoft Support](data-box-disk-contact-microsoft-support.md).


## <a name="data-box-disk-split-copy-tool-errors"></a>Fouten met het splits- en kopieerprogramma voor Data Box Disk

De problemen zien bij het gebruik van een hulpprogramma voor splitsen up splitsen van de gegevens over meerdere schijven worden samengevat in de volgende tabel.

|Foutbericht/waarschuwingen |Aanbevelingen |
|---------|---------|
|[Info] Bij het ophalen van BitLocker-wachtwoord voor volume: min. <br>[Fout] Uitzondering opgetreden tijdens het ophalen van BitLocker-sleutel voor volume m:<br> Reeks bevat geen elementen.|Deze fout wordt gegenereerd als de doel-Data Box Disk offline is. <br> Gebruik hulpprogramma `diskmgmt.msc` voor online schijven.|
|[Fout] Uitzondering geretourneerd: WMI-bewerking is mislukt:<br> Method=UnlockWithNumericalPassword, ReturnValue=2150694965, <br>Win32Message=De indeling van het verstrekte herstelwachtwoord is ongeldig. <br>BitLocker-herstelwachtwoorden bestaan uit 48 cijfers. <br>Controleer of het herstelwachtwoord de juiste indeling heeft en probeer het opnieuw.|Gebruik eerst het ontgrendelingsprogramma van Data Box Disk om de schijven te ontgrendelen en voer dan de opdracht opnieuw uit. Voor meer informatie gaat u naar <li> [Data Box Disk ontgrendelen voor Windows-clients](data-box-disk-deploy-set-up.md#unlock-disks-on-windows-client). </li><li> [Data Box Disk ontgrendelen voor Linux-clients](data-box-disk-deploy-set-up.md#unlock-disks-on-linux-client). </li>|
|[Fout] Uitzondering geretourneerd: Er bestaat een DriveManifest.xml-bestand op de doel-station. <br> Dit betekent dat het doelstation mogelijk is voorbereid met een ander logboekbestand. <br>Als u meer gegevens aan hetzelfde station wilt toevoegen, gebruik dan het vorige logboekbestand. Als u wilt verwijderen van bestaande gegevens en doel-station voor een nieuwe import-taak opnieuw gebruiken, verwijdert u de *DriveManifest.xml* op de schijf. Voer deze opdracht opnieuw uit met een nieuw logboekbestand.| Deze fout wordt gegenereerd wanneer u dezelfde reeks stations probeert te gebruiken voor meerdere importsessies. <br> Gebruik één set schijven maar voor één splits- en kopieersessie.|
|[Fout] Uitzondering geretourneerd: CopySessionId importgegevens-september-test-1 verwijst naar een eerdere sessie. kopiëren en niet opnieuw kan worden gebruikt voor een nieuwe kopieersessie.|Deze fout wordt gerapporteerd wanneer wordt geprobeerd om dezelfde taaknaam voor een nieuwe taak te gebruiken als een eerder met succes voltooide taak.<br> Geef een unieke naam op voor de nieuwe taak.|
|[Info] De naam van het doelbestand of de doelmap overschrijdt de lengtelimiet van NTFS. |Dit bericht wordt gegenereerd als de naam van het doelbestand is gewijzigd vanwege een lang pad.<br> Wijzig de verwijderingsoptie in bestand `config.json` om dit gedrag te regelen.|
|[Fout] Uitzondering geretourneerd: Ongeldige JSON escape-volgorde. |Dit bericht wordt gegenereerd als de config.json een ongeldige opmaak heeft. <br> Valideer uw `config.json` met behulp van [JSONlint](https://jsonlint.com/) voordat u het bestand opslaat.|


## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [validatie hulpprogramma problemen oplossen](data-box-disk-troubleshoot.md).
