---
title: Maak een back-up van WindowsServer of werkstation naar Azure (klassieke model) | Microsoft Docs
description: Back-up van Windows-servers of clients naar een back-upkluis in Azure. Basisprincipes voor het beveiligen van bestanden en mappen die u wilt een back-upkluis met behulp van de Azure Backup agent doorlopen.
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: back-upkluis; back-up van een WindowsServer. back-upvensters;
ms.assetid: 3b543bfd-8978-4f11-816a-0498fe14a8ba
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: markgal;trinadhk;
ms.openlocfilehash: a8daa6a4655b72936b6299c0fa5b80459ffa5da3
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="back-up-a-windows-server-or-workstation-to-azure-using-the-classic-portal"></a>Back-up van een WindowsServer of werkstation naar Azure met de klassieke portal
> [!div class="op_single_selector"]
> * [Klassieke portal](backup-configure-vault-classic.md)
> * [Azure Portal](backup-configure-vault.md)
>
>

In dit artikel bevat informatie over de procedures die u volgen moet om uw omgeving voorbereiden en back-up van een WindowsServer (of werkstation) naar Azure. Het omvat tevens overwegingen voor het implementeren van uw back-upoplossing. Als u geïnteresseerd bent in Azure Backup voor de eerste keer probeert, begeleidt in dit artikel snel u bij het proces.

Azure heeft twee verschillende implementatiemodellen voor het maken en werken met resources: Resource Manager en classic. Dit artikel gaat over het gebruik van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken.

## <a name="before-you-start"></a>Voordat u begint
Als u wilt back-up van een server of client naar Azure, moet u een Azure-account. Als u niet hebt, kunt u een [gratis account](https://azure.microsoft.com/free/) binnen een paar minuten.

## <a name="create-a-backup-vault"></a>Een back-upkluis maken
Als u wilt back-up van bestanden en mappen van een server of client, moet u een back-upkluis maken in de geografische regio waar u de gegevens worden opgeslagen.

> [!IMPORTANT]
> Vanaf maart 2017 is het niet meer mogelijk om de klassieke portal te gebruiken voor het maken van back-upkluizen.
>
> U kunt uw back-upkluizen nu upgraden naar Recovery Services-kluizen. Zie voor meer informatie het artikel [Upgrade a Backup vault to a Recovery Services vault](backup-azure-upgrade-backup-to-recovery-services.md) (Een back-upkluis upgraden naar een Recovery Services-kluis). Microsoft adviseert om uw back-upkluizen te upgraden naar Recovery Services-kluizen.<br/> Vanaf **15 oktober 2017** kunt u PowerShell niet meer gebruiken voor het maken van back-upkluizen. <br/> **Vanaf 1 november 2017**:
>- Alle resterende back-upkluizen worden automatisch omgezet in Recovery Services-kluizen.
>- Het is niet mogelijk om via de klassieke portal toegang te krijgen tot uw back-upgegevens. In plaats daarvan gebruikt u Azure Portal voor toegang tot uw back-upgegevens in Recovery Services-kluizen.
>


## <a name="download-the-vault-credential-file"></a>Download het kluisreferentiebestand
De on-premises machine moet worden geverifieerd met een back-upkluis voordat deze kan back-ups naar Azure. De verificatie wordt bereikt door *vault referenties*. Het kluisreferentiebestand is gedownload via een beveiligd kanaal vanuit de klassieke portal. De persoonlijke sleutel van het certificaat is niet persistent in de portal of de service.

### <a name="to-download-the-vault-credential-file-to-a-local-machine"></a>Het kluisreferentiebestand te downloaden naar een lokale computer
1. Klik in het navigatiedeelvenster links op **Recovery Services**, en selecteer vervolgens de back-upkluis die u hebt gemaakt.

    ![IR voltooid](./media/backup-configure-vault-classic/rs-left-nav.png)
2. Klik op de pagina Quick Start op **kluisreferenties downloaden**.

   De klassieke portal genereert een kluisreferentie met behulp van een combinatie van de kluisnaam van de en de huidige datum. Het kluisreferentiebestand wordt alleen gebruikt tijdens de registratiewerkstroom en verloopt na 48 uur.

   Het kluisreferentiebestand kan worden gedownload vanuit de portal.
3. Klik op **opslaan** het kluisreferentiebestand downloaden naar de map Downloads van het lokale account. U kunt ook selecteren **OpslaanAls** van de **opslaan** menu naar een locatie voor het kluisreferentiebestand opgeven.

   > [!NOTE]
   > Zorg ervoor dat het kluisreferentiebestand is opgeslagen op een locatie die toegankelijk is vanaf uw computer. Als deze is opgeslagen in een blok in het bestandsshare- of -bericht, Controleer of u de machtigingen om deze te openen.
   >
   >

## <a name="download-install-and-register-the-backup-agent"></a>Downloaden, installeren en registreren van de backup-agent
Nadat u de back-upkluis maken en download het kluisreferentiebestand, moet u een agent geïnstalleerd op elk van uw Windows-machines.

### <a name="to-download-install-and-register-the-agent"></a>Als u wilt downloaden, installeren en registreren van de agent
1. Klik op **Recovery Services**, en selecteer vervolgens de back-upkluis die u wilt registreren bij een server.
2. Klik op de pagina snel starten op de agent **Agent voor Windows Server of System Center Data Protection Manager of Windows client**. Klik vervolgens op **Opslaan**.

    ![Opslaan van agent](./media/backup-configure-vault-classic/agent.png)
3. Nadat het bestand MARSagentinstaller.exe is gedownload, klikt u op **uitvoeren** (of dubbelklik op **MARSAgentInstaller.exe** van de opslaglocatie).
4. Kies de installatiemap en de cachemap die vereist zijn voor de agent en klik vervolgens op **volgende**. De locatie van de cache die u opgeeft moet schijfruimte ten minste 5 procent van de back-upgegevens van hebben.
5. U kunt blijven verbinding maken met Internet via de standaard proxy-instellingen.             Als u een proxyserver gebruiken om te verbinden met het Internet, op de pagina proxyconfiguratie selecteert u de **gebruiken van aangepaste proxyinstellingen** selectievakje en voer vervolgens de gegevens van de proxy-server. Als u een geverifieerde proxyserver gebruikt, geef de Gebruikersdetails naam en het wachtwoord en klik vervolgens op **volgende**.
6. Klik op **installeren** om te beginnen met de installatie van de agent. De backup-agent installeert .NET Framework 4.5 en Windows PowerShell (als deze nog niet is geïnstalleerd) om de installatie te voltooien.
7. Nadat de agent is geïnstalleerd, klikt u op **doorgaan met de registratie** om door te gaan met de werkstroom.
8. Blader naar en selecteer het kluisreferentiebestand die u eerder hebt gedownload op de pagina identificatie van de kluis.

    Het kluisreferentiebestand is geldig voor alleen 48 uur nadat deze gedownload vanuit de portal. Als er een fout optreedt op wordt deze pagina (zoals 'kluisreferenties opgegeven bestand is verlopen), meld u aan bij de portal en download het kluisreferentiebestand opnieuw.

    Zorg ervoor dat het kluisreferentiebestand beschikbaar is op een locatie die toegankelijk zijn voor het installatieprogramma. Als u toegang-gerelateerde fouten optreden, wordt het kluisreferentiebestand te kopiëren naar een tijdelijke locatie op dezelfde machine en probeer het opnieuw.

    Als u een kluis referentie fout betreft, zoals 'Ongeldige kluisreferenties die worden opgegeven' tegenkomt, het bestand is beschadigd of heeft niet hebben de meest recente referenties die zijn gekoppeld met de service voor herstel. Probeer het opnieuw nadat het downloaden van een nieuw kluisreferentiebestand via de portal. Deze fout kan ook optreden als een gebruiker de **kluisreferentie downloaden** optie meerdere keren in snel achter elkaar. In dit geval wordt is alleen het laatste kluisreferentiebestand geldig.
9. Op de pagina Versleutelingsinstelling kunt u een wachtwoordzin genereren of voorzien van een wachtwoordzin (minimaal 16 tekens). Moet u de wachtwoordzin op een veilige locatie opslaan.
10. Klik op **Voltooien**. De Wizard Server registreren registreert de server met de back-up.

    > [!WARNING]
    > Als u kwijtraakt of de wachtwoordzin vergeet, niet de back-upgegevens te herstellen door Microsoft helpen. De wachtwoordzin voor versleuteling van jou en Microsoft heeft geen zichtbaarheid van de wachtwoordzin op die u gebruikt. Sla het bestand op een veilige locatie omdat deze tijdens een herstelbewerking wordt vereist.
    >
    >

11. Nadat de versleutelingssleutel is ingesteld, laat de **Start Microsoft Azure Recovery Services Agent** selectievakje geselecteerd en klik vervolgens op **sluiten**.

## <a name="complete-the-initial-backup"></a>De eerste back-up voltooien
Als u voor het eerst een back-up maakt, moet u twee belangrijke taken uitvoeren:

* Maken van back-upschema
* Back-ups van bestanden en mappen voor de eerste keer

Nadat de back-upbeleid is voltooid voor de eerste back-up, maakt het back-uppunten die u gebruiken kunt als u nodig hebt om de gegevens te herstellen. Het back-upbeleid doet dit op basis van de planning die u definieert.

### <a name="to-schedule-the-backup"></a>De back-up plannen
1. Open de Microsoft Azure Backup agent. (Automatisch wordt geopend als u de **Start Microsoft Azure Recovery Services Agent** selectievakje is ingeschakeld wanneer u de Wizard Server registreren gesloten.) U vindt deze door te zoeken naar **Microsoft Azure Backup** op uw machine.

    ![De Azure Backup agent starten](./media/backup-configure-vault-classic/snap-in-search.png)
2. Klik in de Backup-agent op **back-up plannen**.

    ![Een back-up van de Windows Server plannen](./media/backup-configure-vault-classic/schedule-backup-close.png)
3. Op de pagina Aan de slag van de wizard Back-up plannen klikt u op **Volgende**.
4. Op de pagina Items selecteren voor back-up klikt u op **Items toevoegen**.
5. Selecteer de bestanden en mappen waarvan u een back-up wilt maken en klik op **OK**.
6. Klik op **Volgende**.
7. Op de pagina **Back-upschema specificeren** voert u het **back-upschema** in en klikt u op **Volgende**.

    U kunt dagelijkse back-ups (maximaal drie keer per dag en wekelijkse back-ups plannen.

    ![Items voor back-up van Windows Server](./media/backup-configure-vault-classic/specify-backup-schedule-close.png)

   > [!NOTE]
   > Zie het artikel [Uw tape-infrastructuur vervangen met Azure Backup](backup-azure-backup-cloud-as-tape.md) voor meer informatie over hoe u een back-upschema moet invoeren.
   >
   >

8. Op de pagina **Retentiebeleid selecteren** selecteert het de **Retentiebeleid** voor de kopie van de back-up.

    In het retentiebeleid staat hoe lang de back-up zal worden bewaard. In plaats van alleen 'plat beleid' voor alle back-uppunten op te geven kunt u een ander retentiebeleid opgeven op basis van wanneer de back-up plaatsvindt. U kunt het dagelijkse, wekelijkse, maandelijkse en jaarlijkse retentiebeleid aanpassen, zodat het beleid altijd is afgestemd op uw behoeften.
9. Op de pagina Type voor eerste back-up kiezen kiest u het type voor de eerste back-up. Laat de optie **Automatisch via het netwerk** aangevinkt staan en klik vervolgens op **Volgende**.

    U kunt automatisch via het netwerk of offline back-ups maken. In het laatste deel van dit artikel wordt het proces voor automatische back-ups beschreven. Als u liever offline back-ups maakt, lees dan het artikel [Werkstroom voor offline back-up in Azure Backup](backup-azure-backup-import-export.md) voor meer informatie.
10. Lees de informatie op de pagina Bevestiging en klik vervolgens op **Voltooien**.
11. Nadat u de wizard voor het maken van een back-upschema hebt doorlopen, klikt u op **Sluiten**.

### <a name="enable-network-throttling-optional"></a>Netwerkbeperking (optioneel) inschakelen
De backup-agent biedt netwerkbeperking. Gebeurtenisbeperking bepaalt hoe de netwerkbandbreedte tijdens de gegevensoverdracht wordt gebruikt. Dit besturingselement is handig als u back wilt-up van gegevens tijdens werkuren, maar niet wilt dat de back-upproces andere internetverkeer verstoort zijn. Beperking van toepassing op back-up en herstelbewerkingen.

**Netwerkbeperking inschakelen**

1. Klik in de Backup-agent op **eigenschappen wijzigen**.

    ![Eigenschappen wijzigen](./media/backup-configure-vault-classic/change-properties.png)
2. Op de **bandbreedtebeperking** tabblad de **inschakelen voor gebruik van internetbandbreedte voor back-upbewerkingen** selectievakje.

    ![Netwerkbeperking](./media/backup-configure-vault-classic/throttling-dialog.png)
3. Nadat u hebt ingeschakeld beperking, geef de toegestane bandbreedte voor back-up van de gegevensoverdracht tijdens **werkuren** en **niet-werkuren**.

    De waarden van de bandbreedte begint in 512 kilobits per seconde (Kbps) en maximaal 1023 megabytes per seconde (MBps) kunnen gaan. U kunt ook aanwijzen is gestart en voltooien voor **werkuren**, en welke dagen van de week worden beschouwd als werkdagen. Uren buiten aangewezen werk uren worden beschouwd als niet-werk uren.
4. Klik op **OK**.

### <a name="to-back-up-now"></a>Nu back-up maken
1. Klik in de Backup-agent op **Back-Up uit** voltooien van de eerste seeding via het netwerk.

    ![Nu back-up maken van Windows Server](./media/backup-configure-vault-classic/backup-now.png)
2. Controleer op de pagina Bevestiging de instellingen die de wizard Nu back-up maken gebruikt om een back-up te maken van de machine. Klik vervolgens op **Back-up maken**.
3. Klik op **Sluiten** om de wizard te sluiten. Als u dit doet voordat het back-upproces is voltooid, blijft de wizard op de achtergrond aanwezig.

Nadat de eerste back-up is voltooid, wordt de status **Taak voltooid** weergegeven in de back-upconsole.

![IR voltooid](./media/backup-configure-vault-classic/ircomplete.png)

## <a name="next-steps"></a>Volgende stappen
* Aanmelden voor een [gratis Azure-account](https://azure.microsoft.com/free/).

Zie voor meer informatie over back-ups van virtuele machines of andere werkbelastingen:

* [Back-up van IaaS VM 's](backup-azure-vms-prepare.md)
* [Back-up van werkbelastingen naar Azure met Microsoft Azure Backup-Server](backup-azure-microsoft-azure-backup.md)
* [Back-up van werkbelastingen naar Azure met DPM](backup-azure-dpm-introduction.md)
