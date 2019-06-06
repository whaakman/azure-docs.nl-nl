---
title: Problemen met Azure Backup Agent oplossen
description: Installatie en registratie van Azure Backup Agent oplossen
services: backup
author: saurabhsensharma
manager: shivamg
ms.service: backup
ms.topic: conceptual
ms.date: 05/21/2019
ms.author: saurse
ms.openlocfilehash: d8a1d261808eb8f97d1e0dab78b767b37ae6802f
ms.sourcegitcommit: 7042ec27b18f69db9331b3bf3b9296a9cd0c0402
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/06/2019
ms.locfileid: "66743144"
---
# <a name="troubleshoot-microsoft-azure-recovery-services-mars-agent"></a>Microsoft Azure Recovery Services (MARS)-Agent oplossen

Hier wordt beschreven hoe u fouten u tijdens de configuratie, registratie, back-up ziet mogelijk en herstellen.

## <a name="basic-troubleshooting"></a>Eenvoudige probleemoplossing

Is het beter om u de onderstaande validatie, voordat u begint het oplossen van Microsoft Azure Recovery Services agent (MARS):

- [Zorg ervoor dat Microsoft Azure Recovery Services Agent (MARS) is bijgewerkt](https://go.microsoft.com/fwlink/?linkid=229525&clcid=0x409)
- [Zorg ervoor dat er netwerkverbinding is tussen de MARS-agent en Azure](https://aka.ms/AB-A4dp50)
- Controleer of Microsoft Azure Recovery Services wordt uitgevoerd (in Service-console). Indien nodig kunt u de services opnieuw starten en de bewerking nogmaals proberen
- [Zorg ervoor dat er 5-10% ruimte vrij is op de locatie van de scratch-map](https://aka.ms/AB-AA4dwtt)
- [Controleer of er geen ander proces of antivirussoftware conflicten veroorzaakt met Azure Backup](https://aka.ms/AB-AA4dwtk)
- [Geplande back-up mislukt, maar handmatige back-up werkt](https://aka.ms/ScheduledBackupFailManualWorks)
- Zorg ervoor dat uw besturingssysteem helemaal is bijgewerkt
- [Zorg ervoor dat niet-ondersteunde schijven en bestanden met niet-ondersteunde kenmerken zijn uitgesloten van back-up](backup-support-matrix-mars-agent.md#supported-drives-or-volumes-for-backup)
- Zorg ervoor dat de **systeemklok** op de beveiligde computer is geconfigureerd voor de juiste tijdzone <br>
- [Zorg ervoor dat op de server ten minste .Net Framework-versie 4.5.2 of hoger is geïnstalleerd](https://www.microsoft.com/download/details.aspx?id=30653)<br>
- Als u probeert om **uw server opnieuw te registreren** bij een kluis, moet u: <br>
  - Ervoor zorgen dat de agent wordt verwijderd op de server en uit de portal <br>
  - Dezelfde wachtwoordzin gebruiken die in eerste instantie is gebruikt voor het registreren van de server <br>
- Zorg ervoor dat Azure PowerShell versie 3.7.0 op zowel de bron-en de kopie is geïnstalleerd voordat u offline back-upbewerking een offline back-up
- [Overweging bij het backup-agent wordt uitgevoerd op een virtuele machine van Azure](https://aka.ms/AB-AA4dwtr)

## <a name="invalid-vault-credentials-provided"></a>Ongeldige kluisreferenties ingevoerd

| Foutdetails | Mogelijke oorzaken | Aanbevolen acties |
| ---     | ---     | ---    |
| **Fout** </br> *Er zijn ongeldige kluisreferenties opgegeven. Het bestand is beschadigd of heeft niet zijn de meest recente referenties die zijn gekoppeld aan recovery-service. (ID: 34513)* | <ul><li> De kluisreferenties zijn ongeldig (dat wil zeggen, zijn ze meer dan 48 uur vóór het tijdstip van inschrijving gedownload).<li>MARS-Agent, kan geen bestanden downloaden naar de map Temp van Windows. <li>De kluisreferenties bevinden zich op een netwerklocatie bevindt. <li>TLS 1.0 is uitgeschakeld<li> De verbinding wordt geblokkeerd door een geconfigureerde proxyserver. <br> |  <ul><li>Download nieuwe kluisreferenties. (**Opmerking**: Als meerdere bestanden van de kluis referentie eerder zijn gedownload, alleen het meest recente gedownloade bestand is geldig binnen 48 uur.) <li>Start **IE** > **instelling** > **Internetopties** > **Security**  >  **Internet**. Selecteer vervolgens **aangepast niveau**, en schuif totdat u het bestand downloaden sectie ziet. Selecteer vervolgens **inschakelen**.<li>Kunt u wellicht ook om toe te voegen deze sites in Internet Explorer [vertrouwde sites](https://docs.microsoft.com/azure/backup/backup-configure-vault#verify-internet-access).<li>Wijzig de instellingen voor het gebruik van een proxyserver. Geeft u de proxy-server-gegevens. <li> De datum en tijd overeen met de computer.<li>Als u een foutmelding waarin staat dat downloaden van bestanden niet zijn toegestaan, is het waarschijnlijk dat er een groot aantal bestanden in de C:/Windows/Temp-map zijn.<li>Ga naar C:/Windows/Temp en controleren of er meer dan 60.000 of 65.000 bestanden met de extensie .tmp. Als er, moet u deze bestanden verwijdert.<li>Zorg dat u .NET framework 4.6.2 is geïnstalleerd. <li>Als u TLS 1.0 hebt uitgeschakeld vanwege PCI-naleving, verwijzen naar dit [pagina voor probleemoplossing](https://support.microsoft.com/help/4022913). <li>Als u antivirussoftware op de server geïnstalleerd hebt, kunt u de volgende bestanden uitsluiten van de scan antivirusprogramma's: <ul><li>CBengine.exe<li>CSC.exe is gerelateerd aan .NET Framework. Er is een CSC.exe voor elk .NET-versie die geïnstalleerd op de server. CSC.exe-bestanden die zijn gekoppeld aan alle versies van .NET Framework op de betrokken server uitsluiten. <li>Map of cache scratchlocatie. <br>*De standaardlocatie voor de tijdelijke map of het pad naar de locatie van de cache is C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch*.<br><li>De bin-map C:\Program Files\Microsoft Azure Recovery Services Agent\Bin

## <a name="unable-to-download-vault-credential-file"></a>Kan geen kluisreferentiebestand downloaden

| Foutdetails | Aanbevolen acties |
| ---     | ---    |
|Downloaden van het kluisreferentiebestand is mislukt. (ID: 403) | <ul><li> Probeer andere browser met kluisreferenties downloaden of uitvoeren van de onderstaande stappen te volgen: <ul><li> Start Internet Explorer, op F12 te drukken. </li><li> Ga naar **netwerk** tabblad IE cache leegmaken en cookies </li> <li> Vernieuw de pagina<br>(OR)</li></ul> <li> Controleer of het abonnement uitgeschakeld of verlopen is<br>(OR)</li> <li> Controleer als een firewallregel wordt geblokkeerd door de kluis referentie-bestand downloaden <br>(OR)</li> <li> Zorg ervoor dat u hebt niet volledig gebruikt de limiet voor de kluis (50 machines per kluis)<br>(OR)</li>  <li> Zorg ervoor dat de gebruiker heeft toestemming van de Azure Backup kluisreferentie downloaden en server registreren bij de kluis, Zie [artikel](backup-rbac-rs-vault.md)</li></ul> |

## <a name="the-microsoft-azure-recovery-service-agent-was-unable-to-connect-to-microsoft-azure-backup"></a>De Microsoft Azure Recovery Services-agent kan geen verbinding maken met Microsoft Azure Backup

| Foutdetails | Mogelijke oorzaken | Aanbevolen acties |
| ---     | ---     | ---    |
| **Fout** <br /><ol><li>*De Microsoft Azure Recovery Services-Agent is kan geen verbinding maken met Microsoft Azure Backup. (ID: 100050) Controleer uw netwerkinstellingen en zorg ervoor dat u geen verbinding maken met internet*<li>*(407) Verificatie op de proxy is vereist* |Proxy voor het blokkeren van de verbinding. |  <ul><li>Start **IE** > **instelling** > **Internetopties** > **Security**  >  **Internet**. Selecteer vervolgens **aangepast niveau** en schuif totdat u het bestand downloaden sectie ziet. Selecteer **inschakelen**.<li>Kunt u wellicht ook om toe te voegen deze sites in Internet Explorer [vertrouwde sites](https://docs.microsoft.com/azure/backup/backup-try-azure-backup-in-10-mins).<li>Wijzig de instellingen voor het gebruik van een proxyserver. Geeft u de proxy-server-gegevens.<li> Als uw computer toegang tot internet beperkt heeft, zorgt u ervoor dat de firewall-instellingen op de machine of de proxy dit toestaan [URL's](backup-configure-vault.md#verify-internet-access) en [IP-adres](backup-configure-vault.md#verify-internet-access). <li>Als u antivirussoftware op de server geïnstalleerd hebt, kunt u de volgende bestanden uitsluiten van de scan antivirusprogramma's. <ul><li>CBEngine.exe (in plaats van dpmra.exe).<li>CSC.exe (met betrekking tot .NET Framework). Er is een CSC.exe voor elk .NET-versie die geïnstalleerd op de server. CSC.exe-bestanden die zijn gekoppeld aan alle versies van .NET framework op de betrokken server uitsluiten. <li>Map of cache scratchlocatie. <br>*De standaardlocatie voor de tijdelijke map of het pad naar de locatie van de cache is C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch*.<li>De bin-map C:\Program Files\Microsoft Azure Recovery Services Agent\Bin



## <a name="failed-to-set-the-encryption-key-for-secure-backups"></a>Instellen van de versleutelingssleutel voor veilige back-ups is mislukt

| Foutdetails | Mogelijke oorzaken | Aanbevolen acties |
| ---     | ---     | ---    |
| **Fout** <br />*Instellen van de versleutelingssleutel voor veilige back-ups is mislukt. Activering niet volledig is gelukt, maar de wachtwoordzin voor versleuteling is opgeslagen in het volgende bestand*. |<li>De server is al geregistreerd bij een andere kluis.<li>Tijdens het configureren van is de wachtwoordzin beschadigd.| Registratie van de server uit de kluis en Registreer opnieuw met een nieuwe wachtwoordzin.

## <a name="the-activation-did-not-complete-successfully"></a>De activering is niet voltooid

| Foutdetails | Mogelijke oorzaken | Aanbevolen acties |
|---------|---------|---------|
|**Fout** <br />*De activering is niet voltooid. De huidige bewerking is mislukt vanwege een interne servicefout [0x1FC07]. Wacht even en probeer het opnieuw. Als het probleem zich blijft voordoen, neem contact op met Microsoft ondersteuning*     | <li> De tijdelijke map bevindt zich op een volume dat onvoldoende ruimte heeft. <li> De tijdelijke map is niet juist verplaatst naar een andere locatie. <li> Het bestand OnlineBackup.KEK ontbreekt.         | <li>Een upgrade uitvoeren naar de [meest recente versie](https://aka.ms/azurebackup_agent) van de MARS-Agent.<li>De locatie van de nieuwe map of cache verplaatsen naar een volume met vrije ruimte gelijk is aan 5-10% van de totale grootte van de back-upgegevens. Als u wilt correct Verplaats de cachelocatie, raadpleegt u de stappen in [vragen over de Azure Backup Agent](https://docs.microsoft.com/azure/backup/backup-azure-file-folder-backup-faq#backup).<li> Zorg ervoor dat het bestand OnlineBackup.KEK aanwezig is. <br>*De standaardlocatie voor de tijdelijke map of het pad naar de locatie van de cache is C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch*.        |

## <a name="encryption-passphrase-not-correctly-configured"></a>Wachtwoordzin voor versleuteling niet correct geconfigureerd

| Foutdetails | Mogelijke oorzaken | Aanbevolen acties |
|---------|---------|---------|
|**Fout** <br />*Fout bij het 34506. De wachtwoordzin voor versleuteling die zijn opgeslagen op deze computer is niet correct geconfigureerd*.    | <li> De tijdelijke map bevindt zich op een volume dat onvoldoende ruimte heeft. <li> De tijdelijke map is niet juist verplaatst naar een andere locatie. <li> Het bestand OnlineBackup.KEK ontbreekt.        | <li>Een upgrade uitvoeren naar de [meest recente versie](https://aka.ms/azurebackup_agent) van de MARS-Agent.<li>Verplaats de scratchmap of de cachelocatie naar een volume met vrije ruimte gelijk is aan 5-10% van de totale grootte van de back-upgegevens. Als u wilt correct Verplaats de cachelocatie, raadpleegt u de stappen in [vragen over de Azure Backup Agent](https://docs.microsoft.com/azure/backup/backup-azure-file-folder-backup-faq#backup).<li> Zorg ervoor dat het bestand OnlineBackup.KEK aanwezig is. <br>*De standaardlocatie voor de tijdelijke map of het pad naar de locatie van de cache is C:\Program Files\Microsoft Azure Recovery Services Agent\Scratch*.         |


## <a name="backups-dont-run-according-to-the-schedule"></a>Back-ups uitvoeren volgens het schema niet
Als de geplande back-ups niet automatisch ophalen geactiveerd terwijl er een handmatige back-ups zonder problemen werkt, probeert u de volgende acties:

- Zorg ervoor dat Windows Server back-upschema niet in strijd is met Azure-bestanden en mappen back-upschema.

- Zorg ervoor dat de status van de Online back-up is ingesteld op **inschakelen**. Om te controleren of het uitvoeren van de status van de hieronder:

  - Ga naar **Configuratiescherm** > **Systeembeheer** > **Task Scheduler**.
    - Vouw **Microsoft**, en selecteer **Online back-up**.
  - Dubbelklik op **Microsoft OnlineBackup**, en Ga naar de **Triggers** tabblad.
  - Controleer of als de status is ingesteld op **ingeschakeld**. Als dit niet, selecteer **bewerken**, en selecteer de **ingeschakeld** selectievakje in en klikt u op **OK**.

- Zorg ervoor dat het gebruikersaccount dat is geselecteerd voor het uitvoeren van de taak is **SYSTEM** of **lokale beheerdersgroep** op de server. Als u wilt controleren of het gebruikersaccount, gaat u naar de **algemene** tabblad en controleer de **beveiligingsopties**.

- Zie als PowerShell 3.0 of hoger is geïnstalleerd op de server. Voer de volgende opdracht uit om te controleren of de PowerShell-versie, en controleer de *belangrijke* versienummer is gelijk aan of groter is dan 3.

  `$PSVersionTable.PSVersion`

- Zie als het volgende pad deel uit van maakt de *PSMODULEPATH* omgevingsvariabele.

  `<MARS agent installation path>\Microsoft Azure Recovery Services Agent\bin\Modules\MSOnlineBackup`

- Als de PowerShell-uitvoeringsbeleid voor *LocalMachine /* is ingesteld op beperkte, de PowerShell-cmdlet die de back-uptaak activeert kan mislukken. Voer de volgende opdrachten met verhoogde bevoegdheden, om te controleren en het uitvoeringsbeleid instellen op *onbeperkt* of *RemoteSigned*.

  `PS C:\WINDOWS\system32> Get-ExecutionPolicy -List`

  `PS C:\WINDOWS\system32> Set-ExecutionPolicy Unrestricted`

- Zorg ervoor dat de server opnieuw is opgestart na de installatie van de backup-agent

- Zorg ervoor dat er geen ontbreekt of beschadigd **PowerShell** module **MSonlineBackup**. Wanneer er een bestand ontbreekt of is beschadigd, om op te lossen het probleem uitvoeren van de hieronder:

  - Vanaf een andere computer (Windows 2008 R2), met de MARS-agent werkt goed, Kopieer de map MSOnlineBackup van *(C:\Program Files\Microsoft Azure Recovery Services Agent\bin\Modules)* pad.
  - Plak deze in de problematische machine in hetzelfde pad *(C:\Program Files\Microsoft Azure Recovery Services Agent\bin\Modules)* .
  - Als **MSOnlineBackup** map is al bestaat in de machine plakken/vervangen de inhoudsbestanden in het.


> [!TIP]
> Om ervoor te zorgen dat wijzigingen consequent worden toegepast, start u de server opnieuw na het uitvoeren van de bovenstaande stappen.


## <a name="troubleshoot-restore-issues"></a>Problemen met herstellen oplossen

Azure Backup kan is niet het herstelvolume, zelfs na enkele minuten koppelen. Mogelijk krijgt u ook foutberichten tijdens het proces. Om te beginnen met het herstellen van normaal gesproken de volgende stappen uit:

1.  Het annuleren lopende koppelen, als deze actief is geweest gedurende enkele minuten.

2.  Zien als u op de nieuwste versie van de Backup-Agent. Om de versie van het, de **acties** deelvenster van de MARS-console, selecteer **over Microsoft Azure Recovery Services Agent**. Bevestig dat de **versie** getal gelijk is aan of hoger is dan de versie vermeld in [in dit artikel](https://go.microsoft.com/fwlink/?linkid=229525). U kunt de nieuwste versie [hier](https://go.microsoft.com/fwLink/?LinkID=288905) downloaden.

3.  Ga naar **Apparaatbeheer** > **opslagcontrollers**, en Ga naar **Microsoft iSCSI-Initiator**. Als u deze vinden kunt, gaat u verder met stap 7.

4.  Als u Microsoft iSCSI Initiator-service niet kan vinden, proberen te vinden van een item onder **Apparaatbeheer** > **opslagcontrollers** met de naam **onbekend apparaat**, met de Hardware-ID **ROOT\ISCSIPRT**.

5.  Met de rechtermuisknop op **onbekend apparaat**, en selecteer **Update-stuurprogramma**.

6.  Werk bij het stuurprogramma de optie voor het **zoeken voor bijgewerkte stuurprogramma's automatisch**. Voltooiing van de update moet wijzigen **onbekend apparaat** naar **Microsoft iSCSI-Initiator**, zoals hieronder weergegeven.

    ![Schermopname van Azure back-up van Apparaatbeheer met opslagcontrollers gemarkeerd](./media/backup-azure-restore-windows-server/UnknowniSCSIDevice.png)

7.  Ga naar **Taakbeheer** > **Services (lokaal)**  > **Microsoft iSCSI Initiator-Service**.

    ![Schermopname van Azure back-up Taakbeheer met Services (lokaal) gemarkeerd](./media/backup-azure-restore-windows-server/MicrosoftInitiatorServiceRunning.png)

8.  Start de Microsoft iSCSI Initiator-service opnieuw. U doet dit door met de rechtermuisknop op de service en selecteer **stoppen**, met de rechtermuisknop opnieuw op en selecteer **Start**.

9.  Probeer opnieuw te herstellen met behulp van [ **direct herstellen**](backup-instant-restore-capability.md).

Als het herstel is nog steeds mislukt, opnieuw opstarten van de server of -client. Als u niet opnieuw wilt opstarten of het herstel wordt nog steeds mislukt, zelfs na het opnieuw opstarten van de server, kunt u herstellen vanaf een alternatieve machine. Volg de stappen in [in dit artikel](backup-azure-restore-windows-server.md#use-instant-restore-to-restore-data-to-an-alternate-machine).

## <a name="need-help-contact-support"></a>Hulp nodig? Neem contact op met ondersteuning
Als u nog steeds hulp nodig hebt, [contact op met ondersteuning](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) om uw probleem snel worden opgelost.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie opvragen over [hoe u back-up van Windows Server met de Azure Backup Agent](tutorial-backup-windows-server-to-azure.md).
* Als u een back-up moet herstellen, [zet de bestanden dan terug naar een Windows-machine](backup-azure-restore-windows-server.md) aan de hand van dit artikel.
