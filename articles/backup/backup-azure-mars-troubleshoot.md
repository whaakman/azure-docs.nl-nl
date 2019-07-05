---
title: Problemen met de Azure Backup-agent oplossen
description: Installatie en registratie van de Azure backup-agent oplossen
services: backup
author: saurabhsensharma
manager: shivamg
ms.service: backup
ms.topic: conceptual
ms.date: 05/21/2019
ms.author: saurse
ms.openlocfilehash: 1c4c2ed6265bdb3c29986fb0b90c3d85d32aadca
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67434009"
---
# <a name="troubleshoot-the-microsoft-azure-recovery-services-mars-agent"></a>Oplossen van de Microsoft Azure Recovery Services agent (MARS)

In dit artikel wordt beschreven hoe u fouten u tijdens de configuratie, registratie, back-up ziet mogelijk en herstellen.

## <a name="basic-troubleshooting"></a>Eenvoudige probleemoplossing

U wordt aangeraden dat u het volgende controleren voordat u begint met het oplossen van Microsoft de Azure Recovery Services agent (MARS):

- [Zorg ervoor dat de MARS-agent is bijgewerkt](https://go.microsoft.com/fwlink/?linkid=229525&clcid=0x409).
- [Zorg ervoor dat u hebt de netwerkverbinding tussen de MARS-agent en de Azure](https://aka.ms/AB-A4dp50).
- Zorg ervoor dat MARS (in de console-Service) wordt uitgevoerd. Als u wilt, start opnieuw op en probeer het opnieuw.
- [Zorg ervoor dat 5 tot 10% vrije ruimte beschikbaar is op de locatie van de scratchmap](https://aka.ms/AB-AA4dwtt).
- [Controleer of een ander proces of antivirusprogramma verstoort Azure Backup](https://aka.ms/AB-AA4dwtk).
- Als de geplande back-up mislukt, maar handmatige back-up werkt, raadpleegt u [back-ups worden niet uitgevoerd volgens schema](https://aka.ms/ScheduledBackupFailManualWorks).
- Zorg ervoor dat uw besturingssysteem heeft de meest recente updates.
- [Zorg ervoor dat niet-ondersteunde schijven en bestanden met niet-ondersteunde kenmerken zijn uitgesloten van back-up](backup-support-matrix-mars-agent.md#supported-drives-or-volumes-for-backup).
- Zorg ervoor dat de klok op de beveiligde computer is geconfigureerd op de juiste tijdzone.
- [Zorg ervoor dat .NET Framework 4.5.2 of hoger is geïnstalleerd op de server](https://www.microsoft.com/download/details.aspx?id=30653).
- Als u de server om een kluis te registreren probeert:
  - Zorg ervoor dat de agent op de server is verwijderd en wordt deze verwijderd uit de portal.
  - Gebruik dezelfde wachtwoordzin op die in eerste instantie is gebruikt om de server te registreren.
- Voor offline back-ups, zorg ervoor dat Azure PowerShell 3.7.0 is geïnstalleerd op de bronserver en op de computer kopiëren voordat u begint met de back-up.
- Als de backup-agent wordt uitgevoerd op een virtuele machine van Azure, Zie [in dit artikel](https://aka.ms/AB-AA4dwtr).

## <a name="invalid-vault-credentials-provided"></a>Ongeldige kluisreferenties ingevoerd

**Foutbericht**: Er zijn ongeldige kluisreferenties opgegeven. Het bestand is beschadigd of heeft niet zijn de meest recente referenties die zijn gekoppeld aan recovery-service. (ID: 34513)

| Oorzaak | Aanbevolen acties |
| ---     | ---    |
| **Kluisreferenties niet geldig** <br/> <br/> Kluisreferenties bestanden is mogelijk beschadigd of is vervallen. (Bijvoorbeeld: ze zijn mogelijk gedownload meer dan 48 uur vóór het tijdstip van inschrijving.)| Download nieuwe referenties uit de Recovery Services-kluis in Azure portal. (Zie stap 6 van de [de MARS-agent downloaden](https://docs.microsoft.com/azure/backup/backup-configure-vault#download-the-mars-agent) sectie.) Vervolgens worden deze stappen: <ul><li> Als u hebt al geïnstalleerd en geregistreerd MARS, open de Microsoft Azure Backup Agent MMC-console en selecteer vervolgens **Server registreren** in de **acties** deelvenster aan de registratie bij de nieuwe voltooid de referenties. <br/> <li> Als de nieuwe installatie is mislukt, probeer het opnieuw met de nieuwe referenties te installeren.</ul> **Opmerking**: Als meerdere kluis referentie bestanden zijn gedownload, alleen het meest recente bestand is geldig voor de volgende 48 uur. Het is raadzaam dat u een nieuw kluisreferentiebestand downloaden.
| **Proxy-server/firewall blokkeert registratie** <br/>of <br/>**Er is geen verbinding met internet** <br/><br/> Als uw computer of proxy server heeft beperkte verbinding met internet en u geen zorgen toegang voor de vereiste URL's dat, mislukt de registratie.| De volgende stappen uitvoeren:<br/> <ul><li> Werken met uw IT-team om te controleren of dat het systeem verbinding heeft met internet.<li> Als u geen een proxyserver, controleert u of dat de proxyoptie niet is geselecteerd tijdens de registratie van de agent. [Controleer de proxyinstellingen](#verifying-proxy-settings-for-windows).<li> Als u een firewall/proxy-server hebt, contact op met uw netwerk team om te controleren of deze URL's en IP-adressen hebben toegang tot:<br/> <br> **URL 's**<br> www.msftncsi.com <br> .Microsoft.com <br> .WindowsAzure.com <br> .microsoftonline.com <br> .windows.net <br>**IP-adressen**<br>  20.190.128.0/18 <br>  40.126.0.0/18 <br/></ul></ul>Probeer opnieuw te registreren nadat u de voorgaande stappen hebt voltooid.
| **Registratie wordt geblokkeerd door antivirussoftware** | Als u antivirussoftware is geïnstalleerd op de server hebt, kunt u de benodigde uitsluitingsregels toevoegen aan het antivirusprogramma voor deze bestanden en mappen: <br/><ui> <li> CBengine.exe <li> CSC.exe<li> De tijdelijke map. De standaardlocatie is C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch. <li> De bin-map op C:\Program Files\Microsoft Azure Recovery Services Agent\Bin.

### <a name="additional-recommendations"></a>Extra aanbevelingen
- Ga naar C:/Windows/Temp en controleren of er meer dan 60.000 of 65.000 bestanden met de extensie .tmp. Als er, moet u deze bestanden verwijdert.
- Controleer of u van de machine datum en tijd overeen met de lokale tijdzone.
- Zorg ervoor dat [deze sites](backup-configure-vault.md#verify-internet-access) worden toegevoegd aan uw vertrouwde sites in Internet Explorer.

### <a name="verifying-proxy-settings-for-windows"></a>Controleren van proxy-instellingen voor Windows

1. Downloaden van PsExec uit de [Sysinternals](https://docs.microsoft.com/sysinternals/downloads/psexec) pagina.
1. Voer `psexec -i -s "c:\Program Files\Internet Explorer\iexplore.exe"` vanaf een opdrachtprompt met verhoogde bevoegdheid.

   Met deze opdracht wordt Internet Explorer geopend.
1. Ga naar **extra** > **Internetopties** > **verbindingen** > **LAN-instellingen**.
1. Controleer de proxy-instellingen voor het systeem-account.
1. Als er geen proxy is geconfigureerd en proxy-gegevens zijn opgegeven, verwijdert u de details.
1. Als een proxy is geconfigureerd en de proxygegevens onjuist zijn, controleert u of de **Proxy IP** en **poort** gegevens juist zijn.
1. Close Internet Explorer.

## <a name="unable-to-download-vault-credential-file"></a>Kan geen kluisreferentiebestand downloaden

| Fout   | Aanbevolen acties |
| ---     | ---    |
|Downloaden van het kluisreferentiebestand is mislukt. (ID: 403) | <ul><li> Probeer de kluisreferenties downloaden met behulp van een andere browser of de volgende stappen uitvoeren: <ul><li> Start Internet Explorer. Selecteer F12. </li><li> Ga naar de **netwerk** tabblad en de cache leegmaken en cookies. </li> <li> Vernieuw de pagina.<br></li></ul> <li> Controleer of het abonnement uitgeschakeld of verlopen is.<br></li> <li> Controleer als de download wordt geblokkeerd door een firewallregel. <br></li> <li> Zorg ervoor dat u de limiet voor de kluis (50 machines per kluis) nog niet hebt verbruikt.<br></li>  <li> Zorg ervoor dat de gebruiker heeft de Azure Backup-machtigingen die nodig zijn voor de kluisreferenties downloaden en een server registreren bij de kluis. Zie [Use Role-Based toegangsbeheer voor het beheren van Azure Backup-herstelpunten](backup-rbac-rs-vault.md).</li></ul> |

## <a name="the-microsoft-azure-recovery-service-agent-was-unable-to-connect-to-microsoft-azure-backup"></a>De Microsoft Azure Recovery Services-agent kan geen verbinding maken met Microsoft Azure Backup

| Fout  | Mogelijke oorzaak | Aanbevolen acties |
| ---     | ---     | ---    |
| <br /><ul><li>De Microsoft Azure Recovery Services-Agent is kan geen verbinding maken met Microsoft Azure Backup. (ID: 100050) Controleer uw netwerkinstellingen en zorg ervoor dat u geen verbinding maken met internet.<li>(407) proxyverificatie is vereist. |De verbinding wordt geblokkeerd door een proxy. |  <ul><li>Ga in Internet Explorer naar **extra** > **Internetopties** > **Security** > **Internet**. Selecteer **aangepast niveau** en schuif omlaag naar de **downloaden van bestand** sectie. Selecteer **inschakelen**.<p>Kunt u wellicht ook om toe te voegen [URL's en IP-adressen](backup-configure-vault.md#verify-internet-access) aan uw vertrouwde sites in Internet Explorer.<li>Wijzig de instellingen voor het gebruik van een proxyserver. Geeft u de proxy-server-gegevens.<li> Als uw computer toegang tot internet beperkt heeft, zorgt u ervoor dat de firewall-instellingen op de machine of de proxy dit toestaan [URL's en IP-adressen](backup-configure-vault.md#verify-internet-access). <li>Als er antivirussoftware is geïnstalleerd op de server, moet u deze bestanden uitsluiten van het antivirusprogramma: <ul><li>CBEngine.exe (in plaats van dpmra.exe).<li>CSC.exe (met betrekking tot .NET Framework). Er is een CSC.exe voor elke versie van .NET Framework geïnstalleerd op de server. Sluit bestanden uit CSC.exe voor alle versies van .NET Framework op de desbetreffende server. <li>De nieuwe map of een cache-locatie. <br>De standaardlocatie voor de tijdelijke map of het cachepad is C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch.<li>De bin-map op C:\Program Files\Microsoft Azure Recovery Services Agent\Bin.


## <a name="failed-to-set-the-encryption-key-for-secure-backups"></a>Instellen van de versleutelingssleutel voor veilige back-ups is mislukt

| Fout | Mogelijke oorzaken | Aanbevolen acties |
| ---     | ---     | ---    |
| <br />Instellen van de versleutelingssleutel voor veilige back-ups is mislukt. Activering niet volledig is gelukt, maar de wachtwoordzin voor versleuteling is opgeslagen in het volgende bestand. |<li>De server is al geregistreerd bij een andere kluis.<li>Tijdens het configureren van is de wachtwoordzin beschadigd.| Registratie van de server uit de kluis en registreer deze opnieuw met een nieuwe wachtwoordzin.

## <a name="the-activation-did-not-complete-successfully"></a>De activering is niet voltooid

| Fout  | Mogelijke oorzaken | Aanbevolen acties |
|---------|---------|---------|
|<br />De activering is niet voltooid. De huidige bewerking is mislukt vanwege een interne servicefout [0x1FC07]. Wacht even en probeer het opnieuw. Neem contact op met Microsoft Ondersteuning als het probleem zich blijft voordoen.     | <li> De tijdelijke map bevindt zich op een volume dat u beschikt niet over voldoende ruimte. <li> De scratchmap is onjuist verplaatst. <li> Het bestand OnlineBackup.KEK ontbreekt.         | <li>Een upgrade uitvoeren naar de [meest recente versie](https://aka.ms/azurebackup_agent) van de MARS-agent.<li>De locatie van de nieuwe map of cache verplaatsen naar een volume met vrije ruimte die is tussen 5 en 10% van de totale grootte van de back-upgegevens. Als u wilt correct Verplaats de cachelocatie, raadpleegt u de stappen in [Veelgestelde vragen over back-ups van bestanden en mappen](https://docs.microsoft.com/azure/backup/backup-azure-file-folder-backup-faq#backup).<li> Zorg ervoor dat het bestand OnlineBackup.KEK aanwezig is. <br>*De standaardlocatie voor de tijdelijke map of het cachepad is C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch*.        |

## <a name="encryption-passphrase-not-correctly-configured"></a>Wachtwoordzin voor versleuteling niet correct geconfigureerd

| Fout  | Mogelijke oorzaken | Aanbevolen acties |
|---------|---------|---------|
| <br />Fout bij het 34506. De wachtwoordzin voor versleuteling die zijn opgeslagen op deze computer is niet juist geconfigureerd.    | <li> De tijdelijke map bevindt zich op een volume dat u beschikt niet over voldoende ruimte. <li> De scratchmap is onjuist verplaatst. <li> Het bestand OnlineBackup.KEK ontbreekt.        | <li>Een upgrade uitvoeren naar de [meest recente versie](https://aka.ms/azurebackup_agent) van de MARS-Agent.<li>De locatie van de nieuwe map of cache verplaatsen naar een volume met vrije ruimte die is tussen 5 en 10% van de totale grootte van de back-upgegevens. Als u wilt correct Verplaats de cachelocatie, raadpleegt u de stappen in [Veelgestelde vragen over back-ups van bestanden en mappen](https://docs.microsoft.com/azure/backup/backup-azure-file-folder-backup-faq#backup).<li> Zorg ervoor dat het bestand OnlineBackup.KEK aanwezig is. <br>*De standaardlocatie voor de tijdelijke map of het cachepad is C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch*.         |


## <a name="backups-dont-run-according-to-schedule"></a>Back-ups worden niet uitgevoerd volgens schema
Als de geplande back-ups automatisch geactiveerd niet ophalen, maar handmatige back-ups correct werkt, probeert u de volgende acties:

- Zorg ervoor dat de Windows Server back-upschema niet conflicteert met de Azure-bestanden en mappen back-upschema.

- Zorg ervoor dat de status van de online back-up is ingesteld op **inschakelen**. Als u wilt controleren of de status, de volgende stappen uitvoeren:

  1. Vouw in de Task Scheduler, **Microsoft** en selecteer **Online back-up**.
  1. Dubbelklik op **Microsoft OnlineBackup** en Ga naar de **Triggers** tabblad.
  1. Of als de status is ingesteld op **ingeschakeld**. Als dit niet, selecteer **bewerken**, selecteer **ingeschakeld**, en selecteer vervolgens **OK**.

- Zorg ervoor dat het gebruikersaccount dat is geselecteerd voor het uitvoeren van de taak is **SYSTEM** of **lokale beheerdersgroep** op de server. Als u wilt controleren of het gebruikersaccount, gaat u naar de **algemene** tabblad en controleer de **Security** opties.

- Zorg ervoor dat PowerShell 3.0 of hoger is geïnstalleerd op de server. Voer deze opdracht uit om te controleren of de PowerShell-versie, en controleer de `Major` versienummer is 3 of hoger:

  `$PSVersionTable.PSVersion`

- Controleer of dit pad deel uitmaakt van de `PSMODULEPATH` omgevingsvariabele:

  `<MARS agent installation path>\Microsoft Azure Recovery Services Agent\bin\Modules\MSOnlineBackup`

- Als de PowerShell-uitvoeringsbeleid voor `LocalMachine` is ingesteld op beperkte, de PowerShell-cmdlet die de back-uptaak activeert kan mislukken. Deze opdrachten uitvoeren met verhoogde bevoegdheden om te controleren en het uitvoeringsbeleid instellen op `Unrestricted` of `RemoteSigned`:

  `PS C:\WINDOWS\system32> Get-ExecutionPolicy -List`

  `PS C:\WINDOWS\system32> Set-ExecutionPolicy Unrestricted`

- Controleer of dat er zijn geen ontbreekt of is beschadigd PowerShell-module MSOnlineBackup bestanden. Als er een ontbrekende of beschadigde bestanden, voert u deze stappen uit:

  1. Vanaf elke computer die heeft een MARS-agent die goed werkt, de map MSOnlineBackup van C:\Program Files\Microsoft Azure Recovery Services Agent\bin\Modules te kopiëren.
  1. Op de problematische virtuele machine, plak de gekopieerde bestanden op dezelfde maplocatie (C:\Program Files\Microsoft Azure Recovery Services Agent\bin\Modules).

     Als er al een map MSOnlineBackup op de computer, de bestanden plakken of vervangen van alle bestaande bestanden.


> [!TIP]
> Om ervoor te zorgen wijzigingen consequent worden toegepast, de server opnieuw opstarten na het uitvoeren van de voorgaande stappen.


## <a name="troubleshoot-restore-problems"></a>Problemen met herstellen oplossen

Azure Backup kan is niet het herstelvolume, zelfs na enkele minuten koppelen. En u ontvangt mogelijk foutberichten tijdens het proces. Om te beginnen met het herstellen van normaal gesproken de volgende stappen uitvoeren:

1.  Het koppelpunt annuleren als deze actief is geweest gedurende enkele minuten.

2.  Controleer of u de nieuwste versie van de backup-agent hebt. Om te controleren de versie op de **acties** deelvenster van de MARS-console, selecteer **over Microsoft Azure Recovery Services Agent**. Bevestig dat de **versie** getal gelijk is aan of hoger is dan de versie vermeld in [in dit artikel](https://go.microsoft.com/fwlink/?linkid=229525). Selecteer deze koppeling naar [download de nieuwste versie](https://go.microsoft.com/fwLink/?LinkID=288905).

3.  Ga naar **Apparaatbeheer** > **opslagcontrollers** en zoek **Microsoft iSCSI-Initiator**. Als u deze hebt gevonden, gaat u verder met stap 7.

4.  Als u de Microsoft iSCSI Initiator-service niet kan vinden, proberen te vinden van een item onder **Apparaatbeheer** > **opslagcontrollers** met de naam **onbekend apparaat** met de Hardware-ID **ROOT\ISCSIPRT**.

5.  Met de rechtermuisknop op **onbekend apparaat** en selecteer **Update-stuurprogramma**.

6.  Werk bij het stuurprogramma de optie voor het **zoeken voor bijgewerkte stuurprogramma's automatisch**. Deze update moet wijzigen **onbekend apparaat** naar **Microsoft iSCSI-Initiator**:

    ![Schermopname van Azure back-up van Apparaatbeheer met opslagcontrollers gemarkeerd](./media/backup-azure-restore-windows-server/UnknowniSCSIDevice.png)

7.  Ga naar **Taakbeheer** > **Services (lokaal)**  > **Microsoft iSCSI Initiator-Service**:

    ![Schermopname van Azure back-up Taakbeheer met Services (lokaal) gemarkeerd](./media/backup-azure-restore-windows-server/MicrosoftInitiatorServiceRunning.png)

8.  Start de Microsoft iSCSI Initiator-service opnieuw. U doet dit door met de rechtermuisknop op de service en selecteer **stoppen**. Klik met de rechtermuisknop op het opnieuw en selecteer **Start**.

9.  Probeer opnieuw te herstellen met behulp van [direct herstellen](backup-instant-restore-capability.md).

Als het herstel is nog steeds mislukt, opnieuw opstarten van de server of -client. Als u niet wilt op te starten, of als het herstel wordt nog steeds mislukt, zelfs nadat u de server opnieuw opstarten probeer [herstellen vanaf een andere computer](backup-azure-restore-windows-server.md#use-instant-restore-to-restore-data-to-an-alternate-machine).

## <a name="need-help-contact-support"></a>Hulp nodig? Neem contact op met ondersteuning
Als u nog steeds hulp nodig hebt, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om op te halen van uw probleem op te lossen snel.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie opvragen over [hoe u back-up van Windows Server met de Azure Backup agent](tutorial-backup-windows-server-to-azure.md).
* Als u herstellen van een back-up wilt, Zie [bestanden herstellen naar een Windows-machine](backup-azure-restore-windows-server.md).
