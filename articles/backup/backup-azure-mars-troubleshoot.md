---
title: Problemen met Azure Backup-Agent (MARS-Agent) | Microsoft Docs
description: Problemen met de installatie en registratie van Azure backup-agent
services: backup
documentationcenter: 
author: saurabhsensharma
manager: shreeshd
editor: 
ms.assetid: 778c6ccf-3e57-4103-a022-367cc60c411a
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/4/2017
ms.author: saurse;markgal;
ms.openlocfilehash: d05b951277515f3100aefcfb06a17b661267cb37
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="troubleshooting-azure-backup-agent-configurationregistration-issues"></a>Het oplossen van problemen met configuratie/registreren voor Azure Backup-Agent
## <a name="recommended-steps"></a>Aanbevolen stappen
Raadpleeg de aanbevolen acties hieronder aangegeven overeenkomende fouten weergegeven tijdens de configuratie of de registratie van de Azure Backup agent op te lossen.

## <a name="error-invalid-vault-credentials-provided-the-file-is-either-corrupted-or-does-not-have-the-latest-credentials-associated-with-recovery-service"></a>Fout: Ongeldige kluisreferenties opgegeven. Het bestand is beschadigd of heeft niet hebben de meest recente referenties die zijn gekoppeld aan de herstelservice.
| Foutdetails | Mogelijke oorzaken | Aanbevolen actie (s) |
| ---     | ---     | ---    |
| **Fout** </br> *Ongeldige kluisreferenties opgegeven. Het bestand is beschadigd of heeft niet hebben de meest recente referenties die zijn gekoppeld aan de herstelservice. (ID: 34513)* | <ol><li> De kluisreferenties zijn niet geldig (dat wil. ze zijn meer dan 48 uur vóór registratie worden gedownload).<li>   De Azure Backup agent kan niet worden gedownload van een tijdelijk bestand in de map Temp van Windows. <li>De Kluisreferenties zijn op een netwerklocatie. <li>TLS 1.0 is uitgeschakeld<li> De verbinding wordt geblokkeerd door een geconfigureerde proxyserver <br> |  <ol><li>De nieuwe Kluisreferenties downloaden<li>Ga naar Internetopties > Beveiliging > Internet > Klik op Aangepast niveau > blader totdat u de sectie downloaden bestand zien en selecteer inschakelen.<li>Misschien hebt u ook de site toevoegen aan [vertrouwde websites](https://docs.microsoft.com/azure/backup/backup-try-azure-backup-in-10-mins#network-and-connectivity-requirements).<li>Wijzig de instellingen voor het gebruik van proxy en geef de details van de proxy <li> De datum en tijd (UTC) overeenkomt met de computer<li>Ga naar C:/Windows/Temp en controleren of er meer dan 60.000 of 65.000-bestanden (met de extensie TMP zijn) en deze bestanden te verwijderen.<li>U kunt dit testen door het SDP-pakket op de server wordt uitgevoerd. Als u krijgt de fout downloads vermelden bestand zijn niet toegestaan, kunt u weet over een groot aantal bestanden in de map C:/Windows/Temp zijn.<li>Zorg ervoor dat u .NET framework 4.6.2 geïnstalleerd <li>Als u TLS 1.0 hebt uitgeschakeld als gevolg van PCI-naleving, raadpleegt u dit [oplossen koppeling](https://support.microsoft.com/help/4022913). <li>Als er een antivirusprogramma wordt geïnstalleerd op de server, moet u de volgende bestanden uitsluiten van virusscan uitvoert. <ol><li>CBengine.exe<li>CSC.exe (in verband met .NET Framework. Er is een CSC.exe per .NET versie is geïnstalleerd op de server. Alle CSC.exe bestanden die zijn gekoppeld aan alle versies van .NET framework op de desbetreffende server sluiten). <li>Scratchruimte map of een cache-locatie. <br>*De standaardlocatie voor de tijdelijke map of het pad van de cache-locatie is C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch*

## <a name="error-the-microsoft-azure-recovery-service-agent-was-unable-to-connect-to-microsoft-azure-backup"></a>Fout: De Microsoft Azure Recovery Service-Agent is geen verbinding maken met Microsoft Azure Backup

| Foutdetails | Mogelijke oorzaken | Aanbevolen actie (s) |
| ---     | ---     | ---    |
| **Fout** </br><ol><li>*De Microsoft Azure Recovery Service Agent kon geen verbinding maken met Microsoft Azure Backup. (ID: 100050) Controleer de netwerkinstellingen en zorg ervoor dat u verbinding maken met internet*<li>*(407) proxyverificatie is vereist* |Proxy blokkeren verbinding * |  <ol><li>Ga naar Internetopties > Beveiliging > Internet > Klik op Aangepast niveau > blader totdat u de sectie downloaden bestand zien en selecteer inschakelen.<li>Misschien hebt u ook de site toevoegen aan [vertrouwde websites](https://docs.microsoft.com/azure/backup/backup-try-azure-backup-in-10-mins#network-and-connectivity-requirements).<li>Wijzig de instellingen voor het gebruik van proxy en geef de details van de proxy <li>Als er een antivirusprogramma wordt geïnstalleerd op de server, moet u de volgende bestanden uitsluiten van virusscan uitvoert. <ol><li>CBengine.exe<li>(in plaats van dpmra.exe)<li>CSC.exe (in verband met .NET Framework. Er is een CSC.exe per .NET versie is geïnstalleerd op de server. Alle CSC.exe bestanden die zijn gekoppeld aan alle versies van .NET framework op de desbetreffende server sluiten). <li>Locatie van map of cache scratchruimte <br>*De standaardlocatie voor de tijdelijke map of het pad van de cache-locatie is C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch*

## <a name="error-failed-to-set-the-encryption-key-for-secure-backups"></a>Fout: Kan niet worden ingesteld van de versleutelingssleutel voor beveiligde back-ups

| Foutdetails | Mogelijke oorzaken | Aanbevolen actie (s) |
| ---     | ---     | ---    |      
| **Fout** </br>*Instellen van de versleutelingssleutel voor veilige back-ups is mislukt. De huidige bewerking is mislukt vanwege een interne fout 'ongeldig invoer servicefout'. Probeer de bewerking na enige tijd opnieuw. Als het probleem zich blijft voordoen, neem contact op met Microsoft ondersteuning* |Server is al geregistreerd met een andere kluis| Opheffen van de registratie de server uit de kluis en opnieuw registreren.

## <a name="error-the-activation-did-not-complete-successfully-the-current-operation-failed-due-to-an-internal-service-error-0x1fc07"></a>Fout: De activering is niet voltooid. De huidige bewerking is mislukt vanwege een interne servicefout [0x1FC07]

| Foutdetails | Mogelijke oorzaken | Aanbevolen actie (s) |
| ---     | ---     | ---    |          
| **Fout** </br><ol><li>*De activering is niet voltooid. De huidige bewerking is mislukt vanwege een interne servicefout [0x1FC07]. Probeer de bewerking na enige tijd opnieuw. Als het probleem zich blijft voordoen, neem contact op met Microsoft ondersteuning* <li>*Fout bij het 34506. De wachtwoordzin voor versleuteling op deze computer is niet correct geconfigureerd* | <li> Tijdelijke map bevindt zich op een volume dat onvoldoende ruimte heeft <li> Tijdelijke map wordt onjuist verplaatst naar een andere locatie <li> OnlineBackup.KEK-bestand ontbreekt | <li>De tijdelijke map of cachelocatie verplaatsen naar een volume met vrije ruimte gelijk is aan 5-10% van de totale grootte van de back-upgegevens. Raadpleeg de stappen die worden vermeld [in dit artikel](https://docs.microsoft.com/azure/backup/backup-azure-file-folder-backup-faq#backup) juist verplaatsen de cachelocatie.<li> Zorg ervoor dat OnlineBackup.KEK-bestand aanwezig is <br>*De standaardlocatie voor de tijdelijke map of het pad van de cache-locatie is C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch*

## <a name="need-help-contact-support"></a>Hulp nodig? Contact opnemen met ondersteuning
Als u nog hulp nodig hebt, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) ophalen van uw probleem snel worden opgelost.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie opvragen over [back-ups van uw Windows-Server met de Azure Backup agent](tutorial-backup-windows-server-to-azure.md).
* Als u een back-up moet herstellen, [zet de bestanden dan terug naar een Windows-machine](backup-azure-restore-windows-server.md) aan de hand van dit artikel.
