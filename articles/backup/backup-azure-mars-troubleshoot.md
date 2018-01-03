---
title: Azure Backup Agent oplossen | Microsoft Docs
description: Installatie en registratie van Azure Backup Agent oplossen
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
ms.openlocfilehash: 52a32d61dd69110ace560fd1e52389140f322678
ms.sourcegitcommit: c87e036fe898318487ea8df31b13b328985ce0e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/19/2017
---
# <a name="troubleshoot-azure-backup-agent-configuration-and-registration-issues"></a>Problemen met configuratie van de Azure Backup Agent en de inschrijving oplossen
## <a name="recommended-steps"></a>Aanbevolen stappen
Raadpleeg de aanbevolen acties in de volgende tabellen fouten die u tijdens de configuratie of de registratie van Azure Backup Agent tegenkomen kunt op te lossen.

## <a name="invalid-vault-credentials-provided-the-file-is-either-corrupted-or-does-not-have-the-latest-credentials-associated-with-recovery-service"></a>Ongeldige kluisreferenties opgegeven. Het bestand is beschadigd of heeft niet hebben de meest recente referenties die zijn gekoppeld aan de herstelservice.
| Foutdetails | Mogelijke oorzaken | Aanbevolen acties |
| ---     | ---     | ---    |
| **Fout** </br> *Ongeldige kluisreferenties opgegeven. Het bestand is beschadigd of heeft niet hebben de meest recente referenties die zijn gekoppeld aan de herstelservice. (ID: 34513)* | <ul><li> De kluisreferenties zijn ongeldig (dat wil zeggen, zijn ze meer dan 48 uur vóór het tijdstip van registratie gedownload).<li>De Azure Backup Agent kan niet worden gedownload van een tijdelijk bestand naar de map Temp van Windows. <li>De kluisreferenties zijn op een netwerklocatie. <li>TLS 1.0 is uitgeschakeld<li> De verbinding wordt geblokkeerd door een geconfigureerde proxyserver. <br> |  <ul><li>De nieuwe kluisreferenties downloaden.<li>Ga naar **Internetopties** > **beveiliging** > **Internet**. Selecteer vervolgens **aangepast niveau**, en schuif totdat u het bestand downloaden sectie ziet. Selecteer vervolgens **inschakelen**.<li>U wellicht ook de site toevoegen aan [vertrouwde websites](https://docs.microsoft.com/azure/backup/backup-try-azure-backup-in-10-mins#network-and-connectivity-requirements).<li>Wijzig de instellingen voor het gebruik van een proxyserver. Geef de proxy-server-gegevens. <li> De datum en tijd overeen met de computer.<li>Ga naar C:/Windows/Temp en controleren of er meer dan 60.000 of 65.000 bestanden met de extensie TMP zijn. Als er, verwijdert u deze bestanden.<li>U kunt dit testen door het SDP-pakket op de server wordt uitgevoerd. Als u een foutmelding weergegeven dat bestandsdownloads zijn niet toegestaan krijgt, is het zeer waarschijnlijk dat er een groot aantal bestanden in de map C:/Windows/Temp zijn.<li>Zorg ervoor dat u .NET framework 4.6.2 geïnstalleerd. <li>Als u TLS 1.0 hebt uitgeschakeld als gevolg van PCI-naleving, raadpleegt u dit [probleemoplossing pagina](https://support.microsoft.com/help/4022913). <li>Als er antivirussoftware geïnstalleerd op de server, moet u de volgende bestanden uitsluiten van de scan AntiVirus: <ul><li>CBengine.exe<li>CSC.exe is gerelateerd aan .NET Framework. Er is een CSC.exe voor elke .NET-versie die op de server geïnstalleerd. CSC.exe-bestanden die zijn verbonden met alle versies van .NET framework op de desbetreffende server uit te sluiten. <li>Scratchruimte map of een cache-locatie. <br>*De standaardlocatie voor de tijdelijke map of het pad van de cache-locatie is C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch*.

## <a name="the-microsoft-azure-recovery-service-agent-was-unable-to-connect-to-microsoft-azure-backup"></a>De Microsoft Azure Recovery Service-Agent is geen verbinding maken met Microsoft Azure Backup

| Foutdetails | Mogelijke oorzaken | Aanbevolen acties |
| ---     | ---     | ---    |
| **Fout** </br><ol><li>*De Microsoft Azure Recovery Service Agent kon geen verbinding maken met Microsoft Azure Backup. (ID: 100050) Controleer de netwerkinstellingen en zorg ervoor dat u verbinding maken met internet*<li>*(407) proxyverificatie is vereist* |Proxy voor het blokkeren van de verbinding. |  <ul><li>Ga naar de **Internetopties** > **beveiliging** > **Internet**. Selecteer vervolgens **aangepast niveau** en schuif totdat u het bestand downloaden sectie ziet. Selecteer **Inschakelen**.<li>U wellicht ook de site toevoegen aan [vertrouwde websites](https://docs.microsoft.com/azure/backup/backup-try-azure-backup-in-10-mins#network-and-connectivity-requirements).<li>Wijzig de instellingen voor het gebruik van een proxyserver. Geef de proxy-server-gegevens. <li>Als er antivirussoftware geïnstalleerd op de server, moet u de volgende bestanden uitsluiten van de scan antivirus. <ul><li>CBEngine.exe (in plaats van dpmra.exe).<li>CSC.exe (in verband met .NET Framework). Er is een CSC.exe voor elke .NET-versie die op de server geïnstalleerd. CSC.exe-bestanden die zijn verbonden met alle versies van .NET framework op de desbetreffende server uit te sluiten. <li>Scratchruimte map of een cache-locatie. <br>*De standaardlocatie voor de tijdelijke map of het pad van de cache-locatie is C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch*.

## <a name="failed-to-set-the-encryption-key-for-secure-backups"></a>Instellen van de versleutelingssleutel voor veilige back-ups is mislukt

| Foutdetails | Mogelijke oorzaken | Aanbevolen acties |
| ---     | ---     | ---    |      
| **Fout** </br>*Instellen van de versleutelingssleutel voor veilige back-ups is mislukt. De huidige bewerking is mislukt vanwege een interne fout 'ongeldig invoer servicefout'. Probeer de bewerking na enige tijd opnieuw. Als het probleem zich blijft voordoen, neem contact op met Microsoft ondersteuning*. |Server is al geregistreerd met een andere kluis.| Hef de registratie van de server uit de kluis en Registreer het opnieuw.

## <a name="the-activation-did-not-complete-successfully-the-current-operation-failed-due-to-an-internal-service-error-0x1fc07"></a>De activering is niet voltooid. De huidige bewerking is mislukt vanwege een interne servicefout [0x1FC07]

| Foutdetails | Mogelijke oorzaken | Aanbevolen acties |
| ---     | ---     | ---    |          
| **Fout** </br><ol><li>*De activering is niet voltooid. De huidige bewerking is mislukt vanwege een interne servicefout [0x1FC07]. Probeer de bewerking na enige tijd opnieuw. Als het probleem zich blijft voordoen, neem contact op met Microsoft ondersteuning* <li>*Fout bij het 34506. De wachtwoordzin voor versleuteling op deze computer is niet correct geconfigureerd*. | <li> De tijdelijke map bevindt zich op een volume dat onvoldoende ruimte heeft. <li> De tijdelijke map is niet goed verplaatst naar een andere locatie. <li> Het bestand OnlineBackup.KEK ontbreekt. | <li>De tijdelijke map of cachelocatie verplaatsen naar een volume met vrije ruimte gelijk is aan 5-10% van de totale grootte van de back-upgegevens. Als u wilt verplaatsen correct de cachelocatie, raadpleegt u de stappen in [vragen over de Azure Backup Agent](https://docs.microsoft.com/azure/backup/backup-azure-file-folder-backup-faq#backup).<li> Zorg ervoor dat het bestand OnlineBackup.KEK aanwezig is. <br>*De standaardlocatie voor de tijdelijke map of het pad van de cache-locatie is C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch*.

## <a name="need-help-contact-support"></a>Hulp nodig? Contact opnemen met ondersteuning
Als u nog hulp nodig hebt, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) ophalen van uw probleem snel worden opgelost.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie opvragen over [reservekopie maken van Windows Server met de Azure Backup Agent](tutorial-backup-windows-server-to-azure.md).
* Als u een back-up moet herstellen, [zet de bestanden dan terug naar een Windows-machine](backup-azure-restore-windows-server.md) aan de hand van dit artikel.
