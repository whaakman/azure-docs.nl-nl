---
title: Het oplossen van Azure Data Box-schijf ontgrendelen problemen schijf | Microsoft Docs
description: Hierin wordt beschreven hoe u problemen in Azure Data Box Disk oplost.
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: article
ms.date: 06/14/2019
ms.author: alkohli
ms.openlocfilehash: 02cbf64261bbfbf50561e1b7466b46b27b688e0a
ms.sourcegitcommit: 72f1d1210980d2f75e490f879521bc73d76a17e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/14/2019
ms.locfileid: "67148281"
---
# <a name="troubleshoot-disk-unlocking-issues-in-azure-data-box-disk"></a>Problemen met schijf ontgrendelen in Azure Data Box-schijf

In dit artikel is van toepassing op Microsoft Azure Data Box-schijf en een beschrijving van de werkstromen die wordt gebruikt voor het oplossen van problemen bij het gebruik van het hulpprogramma ontgrendelen. 


<!--## Query activity logs

Use the activity logs to find who unlocked and accessed the disks. Your Data Box Disk arrive on your premises in a locked state. You can use the device credentials available in the Azure portal for your order to unlock them.  

To figure out who accessed the **Device credentials** blade, you can query the Activity logs.  Any action that involves accessing **Device details > Credentials** blade is logged into the activity logs as `ListCredentials` action.

![Query Activity logs](media/data-box-logs/query-activity-log-1.png)-->


## <a name="data-box-disk-unlock-tool-errors"></a>Fouten met het ontgrendelingsprogramma voor Data Box Disk


| Fout bericht/gedrag hulpprogramma      | Aanbevelingen                                                                             |
|-------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------|
| Het huidige .NET Framework wordt niet ondersteund. De ondersteunde versies zijn 4.5 of hoger.<br><br>Hulpprogramma wordt beëindigd met een bericht.  | .NET 4.5 is niet geïnstalleerd. Installeer .NET 4.5 of hoger op de hostcomputer waarop het ontgrendelingsprogramma voor Data Box Disk wordt uitgevoerd.                                                                            |
| Kan geen volumes ontgrendelen of verifiëren. Neem contact op met Microsoft Ondersteuning.  <br><br>Vergrendelde stations kunnen niet worden ontgrendeld of geverifieerd door het hulpprogramma. | De vergrendelde stations kunnen niet worden ontgrendeld door het hulpprogramma met de verstrekte wachtwoordsleutel. Neem contact op met Microsoft Ondersteuning voor volgende stappen.                                                |
| De volgende volumes zijn ontgrendeld en geverifieerd. <br>Volume stationsletters: E:<br>Er konden geen volumes worden ontgrendeld met de volgende wachtwoordsleutels: werwerqomnf, qwerwerqwdfda <br><br>Door het hulpprogramma worden sommige stations ontgrendeld en worden de letters van stations waarbij het is mislukt weergegeven.| Gedeeltelijk geslaagd. Sommige van de stations kunnen niet worden ontgrendeld met de verstrekte wachtwoordsleutel. Neem contact op met Microsoft Ondersteuning voor volgende stappen. |
| De vergrendelde volumes zijn niet gevonden. Controleer of de schijf die is ontvangen van Microsoft op de juiste manier is aangesloten en is vergrendeld.          | Er zijn geen vergrendelde stations gevonden door het hulpprogramma. De stations zijn al ontgrendeld of niet gedetecteerd. Zorg ervoor dat de stations zijn verbonden en zijn vergrendeld.                                                           |
| Onherstelbare fout: Ongeldige parameter<br>Parameternaam: invalid_arg<br>GEBRUIK:<br>DataBoxDiskUnlock /PassKeys:<passkey_list_separated_by_semicolon><br><br>Voorbeeld: DataBoxDiskUnlock /PassKeys:passkey1;passkey2;passkey3<br>Voorbeeld: DataBoxDiskUnlock /SystemCheck<br>Voorbeeld: DataBoxDiskUnlock /Help<br><br>/PassKeys:       Deze sleutel ophalen uit Azure DataBox schijforder. De wachtwoordsleutel ontgrendelt uw schijven.<br>/Help:           Deze optie biedt Help-informatie voor cmdlet-syntaxis en voorbeelden.<br>/SystemCheck:    Deze optie wordt gecontroleerd of uw systeem voldoet aan de vereisten voor het uitvoeren van het hulpprogramma.<br><br>Druk op een willekeurige toets om af te sluiten. | Ongeldige parameter ingevoerd. De enige toegestane parameters zijn /SystemCheck en /PassKey/Help.|


## <a name="unlock-issues-for-disks-when-using-a-windows-client"></a>Problemen met de schijven ontgrendelen wanneer u een Windows-client

In deze sectie worden enkele van de meest voorkomende problemen tijdens de implementatie van de Data Box-schijf met het gebruik van een Windows-client voor het kopiëren van gegevens.

### <a name="issue-could-not-unlock-drive-from-bitlocker"></a>Probleem: Station BitLocker kan niet worden ontgrendeld.
 
**Oorzaak** 

U hebt het wachtwoord in het dialoogvenster BitLocker gebruikt en stations dialoogvenster voor het ontgrendelen van de schijf via de BitLocker probeert ontgrendelen. Dit werkt niet.

**Resolutie**

Om toegang te krijgen tot de Data Box-schijven, moet u het hulpprogramma voor het ontgrendelen van gegevens in het schijf gebruikt en geeft u het wachtwoord van de Azure-portal. Voor meer informatie gaat u naar [Zelfstudie: Uitpakken, verbinden en ontgrendelen van Azure Data Box-schijf](data-box-disk-deploy-set-up.md#connect-to-disks-and-get-the-passkey).
 
### <a name="issue-could-not-unlock-or-verify-some-volumes-contact-microsoft-support"></a>Probleem: Kan geen ontgrendelen of enkele volumes te controleren. Neem contact op met Microsoft Ondersteuning.
 
**Oorzaak**

U ziet mogelijk de volgende fout in het foutenlogboek en zijn niet in staat om te ontgrendelen of enkele volumes te verifiëren.

`Exception System.IO.FileNotFoundException: Could not load file or assembly 'Microsoft.Management.Infrastructure, Version=1.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35' or one of its dependencies. The system cannot find the file specified.`
 
Hiermee wordt aangegeven dat u waarschijnlijk de juiste versie van Windows PowerShell op uw Windows-client ontbreken.

**Resolutie**

U kunt installeren [v Windows PowerShell 5.0](https://www.microsoft.com/download/details.aspx?id=54616) en probeer het opnieuw.
 
Als u nog steeds niet lukt de volumes ontgrendelen, de logboeken gekopieerd van de map met het hulpprogramma Data Box Disk ontgrendelen en [Neem contact op met Microsoft Support](data-box-disk-contact-microsoft-support.md).

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [validatie problemen](data-box-disk-troubleshoot.md).
