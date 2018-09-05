---
title: Problemen met Azure Backup Agent oplossen
description: Installatie en registratie van Azure Backup Agent oplossen
services: backup
author: saurabhsensharma
manager: shreeshd
ms.service: backup
ms.topic: conceptual
ms.date: 7/25/2018
ms.author: saurse
ms.openlocfilehash: 2c8978cfba8fc56d4dbc565cb3a91c75d9d54679
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/05/2018
ms.locfileid: "43700192"
---
# <a name="troubleshoot-microsoft-azure-recovery-services-mars-agent-issues"></a>Microsoft Azure Recovery Services (MARS) met agents oplossen
## <a name="recommended-steps"></a>Aanbevolen stappen
Raadpleeg dit artikel om op te lossen fouten tijdens de configuratie, registratie, back-up en herstellen met behulp van de MARS-Agent.

## <a name="invalid-vault-credentials-provided-the-file-is-either-corrupted-or-does-not-have-the-latest-credentials-associated-with-recovery-service"></a>Ongeldige kluisreferenties opgegeven. Het bestand is beschadigd of heeft niet zijn de meest recente referenties die zijn gekoppeld aan recovery-service.
| Foutdetails | Mogelijke oorzaken | Aanbevolen acties |
| ---     | ---     | ---    |
| **Fout** </br> *Er zijn ongeldige kluisreferenties opgegeven. Het bestand is beschadigd of heeft niet zijn de meest recente referenties die zijn gekoppeld aan recovery-service. (ID: 34513)* | <ul><li> De kluisreferenties zijn ongeldig (dat wil zeggen, zijn ze meer dan 48 uur vóór het tijdstip van inschrijving gedownload).<li>MARS-Agent, kan geen bestanden downloaden naar de map Temp van Windows. <li>De kluisreferenties bevinden zich op een netwerklocatie bevindt. <li>TLS 1.0 is uitgeschakeld<li> De verbinding wordt geblokkeerd door een geconfigureerde proxyserver. <br> |  <ul><li>Download nieuwe kluisreferenties.<li>Ga naar **Internetopties** > **Security** > **Internet**. Selecteer vervolgens **aangepast niveau**, en schuif totdat u het bestand downloaden sectie ziet. Selecteer vervolgens **inschakelen**.<li>Kunt u wellicht ook om toe te voegen van de site [vertrouwde sites](https://docs.microsoft.com/azure/backup/backup-try-azure-backup-in-10-mins#network-and-connectivity-requirements).<li>Wijzig de instellingen voor het gebruik van een proxyserver. Geeft u de proxy-server-gegevens. <li> De datum en tijd overeen met de computer.<li>Ga naar C:/Windows/Temp en controleren of er meer dan 60.000 of 65.000 bestanden met de extensie .tmp. Als er, moet u deze bestanden verwijdert.<li>U kunt controleren of deze de SDP-pakket op de server wordt uitgevoerd. Als u een foutmelding waarin staat dat downloaden van bestanden niet zijn toegestaan, is het waarschijnlijk dat er een groot aantal bestanden in de C:/Windows/Temp-map zijn.<li>Zorg dat u .NET framework 4.6.2 is geïnstalleerd. <li>Als u TLS 1.0 hebt uitgeschakeld vanwege PCI-naleving, verwijzen naar dit [pagina voor probleemoplossing](https://support.microsoft.com/help/4022913). <li>Als u antivirussoftware op de server geïnstalleerd hebt, kunt u de volgende bestanden uitsluiten van de scan antivirusprogramma's: <ul><li>CBengine.exe<li>CSC.exe is gerelateerd aan .NET Framework. Er is een CSC.exe voor elk .NET-versie die geïnstalleerd op de server. CSC.exe-bestanden die zijn gekoppeld aan alle versies van .NET framework op de betrokken server uitsluiten. <li>Map of cache scratchlocatie. <br>*De standaardlocatie voor de tijdelijke map of het pad naar de locatie van de cache is C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch*.

## <a name="the-microsoft-azure-recovery-service-agent-was-unable-to-connect-to-microsoft-azure-backup"></a>De Microsoft Azure Recovery Services-Agent is kan geen verbinding maken met Microsoft Azure Backup

| Foutdetails | Mogelijke oorzaken | Aanbevolen acties |
| ---     | ---     | ---    |
| **Fout** </br><ol><li>*De Microsoft Azure Recovery Services-Agent is kan geen verbinding maken met Microsoft Azure Backup. (ID: 100050) Controleer uw netwerkinstellingen en zorg ervoor dat u geen verbinding maken met internet*<li>*(407) proxyverificatie is vereist* |Proxy voor het blokkeren van de verbinding. |  <ul><li>Ga naar de **Internetopties** > **Security** > **Internet**. Selecteer vervolgens **aangepast niveau** en schuif totdat u het bestand downloaden sectie ziet. Selecteer **Inschakelen**.<li>Kunt u wellicht ook om toe te voegen van de site [vertrouwde sites](https://docs.microsoft.com/azure/backup/backup-try-azure-backup-in-10-mins#network-and-connectivity-requirements).<li>Wijzig de instellingen voor het gebruik van een proxyserver. Geeft u de proxy-server-gegevens. <li>Als u antivirussoftware op de server geïnstalleerd hebt, kunt u de volgende bestanden uitsluiten van de scan antivirusprogramma's. <ul><li>CBEngine.exe (in plaats van dpmra.exe).<li>CSC.exe (met betrekking tot .NET Framework). Er is een CSC.exe voor elk .NET-versie die geïnstalleerd op de server. CSC.exe-bestanden die zijn gekoppeld aan alle versies van .NET framework op de betrokken server uitsluiten. <li>Map of cache scratchlocatie. <br>*De standaardlocatie voor de tijdelijke map of het pad naar de locatie van de cache is C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch*.

## <a name="failed-to-set-the-encryption-key-for-secure-backups"></a>Instellen van de versleutelingssleutel voor veilige back-ups is mislukt

| Foutdetails | Mogelijke oorzaken | Aanbevolen acties |
| ---     | ---     | ---    |      
| **Fout** </br>*Instellen van de versleutelingssleutel voor veilige back-ups activering niet volledig is gelukt, maar de wachtwoordzin voor versleuteling is opgeslagen in het volgende bestand is mislukt*. |<li>Server is al geregistreerd bij een andere kluis.<li>Tijdens het configureren van is de wachtwoordzin beschadigd| Registratie van de server uit de kluis en Registreer opnieuw met een nieuwe wachtwoordzin.

## <a name="the-activation-did-not-complete-successfully-the-current-operation-failed-due-to-an-internal-service-error-0x1fc07"></a>De activering is niet voltooid. De huidige bewerking is mislukt vanwege een interne servicefout [0x1FC07]

| Foutdetails | Mogelijke oorzaken | Aanbevolen acties |
|---------|---------|---------|
|**Fout** </br><ol>*De activering is niet voltooid. De huidige bewerking is mislukt vanwege een interne servicefout [0x1FC07]. Probeer de bewerking na enige tijd opnieuw. Als het probleem zich blijft voordoen, neem contact op met Microsoft ondersteuning*     | <li> De tijdelijke map bevindt zich op een volume dat onvoldoende ruimte heeft. <li> De tijdelijke map is niet juist verplaatst naar een andere locatie. <li> Het bestand OnlineBackup.KEK ontbreekt.         | <li>Een upgrade uitvoeren naar de [meest recente versie](http://aka.ms/azurebackup_agent) van de MARS-Agent.<li>De locatie van de nieuwe map of cache verplaatsen naar een volume met vrije ruimte gelijk is aan 5-10% van de totale grootte van de back-upgegevens. Als u wilt correct Verplaats de cachelocatie, raadpleegt u de stappen in [vragen over de Azure Backup Agent](https://docs.microsoft.com/azure/backup/backup-azure-file-folder-backup-faq#backup).<li> Zorg ervoor dat het bestand OnlineBackup.KEK aanwezig is. <br>*De standaardlocatie voor de tijdelijke map of het pad naar de locatie van de cache is C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch*.        |

## <a name="error-34506-the-encryption-passphrase-stored-on-this-computer-is-not-correctly-configured"></a>Fout bij het 34506. De wachtwoordzin voor versleuteling die zijn opgeslagen op deze computer is niet correct geconfigureerd

| Foutdetails | Mogelijke oorzaken | Aanbevolen acties |
|---------|---------|---------|
|**Fout** </br><ol>*Fout bij het 34506. De wachtwoordzin voor versleuteling die zijn opgeslagen op deze computer is niet correct geconfigureerd*.    | <li> De tijdelijke map bevindt zich op een volume dat onvoldoende ruimte heeft. <li> De tijdelijke map is niet juist verplaatst naar een andere locatie. <li> Het bestand OnlineBackup.KEK ontbreekt.        | <li>Een upgrade uitvoeren naar de [meest recente versie](http://aka.ms/azurebackup_agent) van de MARS-Agent.<li>Verplaats de scratchmap of de cachelocatie naar een volume met vrije ruimte gelijk is aan 5-10% van de totale grootte van de back-upgegevens. Als u wilt correct Verplaats de cachelocatie, raadpleegt u de stappen in [vragen over de Azure Backup Agent](https://docs.microsoft.com/azure/backup/backup-azure-file-folder-backup-faq#backup).<li> Zorg ervoor dat het bestand OnlineBackup.KEK aanwezig is. <br>*De standaardlocatie voor de tijdelijke map of het pad naar de locatie van de cache is C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch*.         |

## <a name="backups-do-not-run-as-per-schedule"></a>Back-ups niet worden uitgevoerd volgens schema
De volgende stappen uitvoeren wanneer geplande back-ups niet automatisch, doet u geactiveerd terwijl er een handmatige back-ups werken zonder problemen. 
 
<li>Zorg ervoor dat PowerShell 3.0 of hoger is geïnstalleerd op de server. Voer de volgende opdracht uit om te controleren of de PowerShell-versie, en controleer de *belangrijke* versienummer is gelijk aan of groter is dan 3.

`$PSVersionTable.PSVersion`
<li>Zorg ervoor dat het volgende pad deel van uitmaakt de *PSMODULEPATH* omgevingsvariabele.

`<MARS agent installation path>\Microsoft Azure Recovery Services Agent\bin\Modules\MSOnlineBackup`
<li>Als de powershell-uitvoeringsbeleid voor de *LocalMachine /* is ingesteld op beperkte, de powershell-cmdlet die de back-uptaak activeert kan mislukken. De volgende opdrachten uitvoeren met verhoogde bevoegdheden om te controleren en het uitvoeringsbeleid instellen op *onbeperkt* of *RemoteSigned*

`PS C:\WINDOWS\system32> Get-ExecutionPolicy -List`

`PS C:\WINDOWS\system32> Set-ExecutionPolicy Unrestricted`
<li>Ga naar Configuratiescherm -> beheerprogramma's Task Scheduler -> -> Microsoft Vouw Selecteer Online back-up ->
<li>Dubbelklik op de taak 'Microsoft-OnlineBackup' en Ga naar het tabblad 'Triggers'.
<li>Zorg ervoor dat de Status van de taak is ingesteld op 'Enabled'. Als dit niet het geval is, klikt u op 'Bewerken' en schakel het selectievakje 'Enabled'
<li>Navigeer naar de *beveiligingsopties* sectie van de *algemene* tabblad
<li>Zorg ervoor dat het gebruikersaccount dat is geselecteerd voor het uitvoeren van de taak een is *SYSTEM* of de groep lokale beheerders op de server

> [!TIP]
> Het verdient aanbeveling op de server opnieuw opstarten na het uitvoeren van de stappen hierboven om ervoor te zorgen dat wijzigingen consequent worden toegepast


## <a name="troubleshooting-restore-issues"></a>Problemen met herstellen oplossen

### <a name="failure-to-mount-the-recovery-volume-during-recovery-of-files-and-folders"></a>Fout bij het Herstelvolume koppelen bij het herstellen van bestanden en mappen
Als u Azure Backup het herstelvolume niet is gekoppeld, zelfs na enkele minuten van het klikken op **koppelen** of koppelen van het volume herstellen met een of meer fouten, wordt niet de volgende stappen om te beginnen met normaal gesproken worden hersteld.

1.  Het koppelpunt lopende annuleren als deze actief is geweest gedurende enkele minuten.

2.  Zorg ervoor dat u zich op de nieuwste versie van de Azure backup-agent. Als u wilt weten de versie-informatie van de Azure backup-agent, klikt u op **over Microsoft Azure Recovery Services Agent** op de **acties** deelvenster van Microsoft Azure Backup-console en zorg ervoor dat de **Versie** getal gelijk is aan of hoger is dan de versie vermeld in [in dit artikel](https://go.microsoft.com/fwlink/?linkid=229525). U kunt de meest recente versie van downloaden [hier](https://go.microsoft.com/fwLink/?LinkID=288905)

3.  Ga naar **Apparaatbeheer** -> **opslagcontrollers** en zorg ervoor dat u kunt vinden **Microsoft iSCSI-Initiator**. Als u deze vinden kunt, Ga rechtstreeks naar stap 7 hieronder. 

4.  Als u kunt Microsoft iSCSI Initiator-service niet kan vinden, zoals vermeld in stap 3, controleert u of u een item onder kunt vinden **Apparaatbeheer** -> **opslagcontrollers** met de naam  **Onbekend apparaat** met Hardware-ID **ROOT\ISCSIPRT**.

5.  Klik met de rechtermuisknop op **onbekend apparaat** en selecteer **Update-stuurprogramma**.

6.  Werk bij het stuurprogramma de optie voor het **zoeken voor bijgewerkte stuurprogramma's automatisch**. Voltooiing van de update moet wijzigen **onbekend apparaat** naar **Microsoft iSCSI-Initiator** zoals hieronder wordt weergegeven. 

    ![Versleuteling](./media/backup-azure-restore-windows-server/UnknowniSCSIDevice.png)

7.  Ga naar **Taakbeheer** -> **Services (lokaal)** -> **Microsoft iSCSI Initiator-Service**. 

    ![Versleuteling](./media/backup-azure-restore-windows-server/MicrosoftInitiatorServiceRunning.png)
    
8.  Start de Microsoft iSCSI Initiator-service opnieuw op met de rechtermuisknop op de service, te klikken op stoppen, en verder met de rechtermuisknop op het opnieuw en te klikken op **Start**.

9.  Probeer opnieuw te herstellen met behulp van directe herstellen. 

Als het herstel is nog steeds mislukt, opnieuw opstarten van uw server/client. Als een opnieuw opstarten niet wenselijk is of het herstel wordt nog steeds mislukt, zelfs na het opnieuw opstarten van de server, probeer herstellen vanaf een alternatieve Machine door de stappen te volgen die worden vermeld in [in dit artikel](backup-azure-restore-windows-server.md#use-instant-restore-to-restore-data-to-an-alternate-machine)

## <a name="need-help-contact-support"></a>Hulp nodig? Contact opnemen met ondersteuning
Als u nog steeds hulp nodig hebt, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om uw probleem snel worden opgelost.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie opvragen over [hoe u back-up van Windows Server met de Azure Backup Agent](tutorial-backup-windows-server-to-azure.md).
* Als u een back-up moet herstellen, [zet de bestanden dan terug naar een Windows-machine](backup-azure-restore-windows-server.md) aan de hand van dit artikel.
