---
title: Problemen met de systeem status back-up oplossen met Azure Backup
description: Problemen met de systeem status back-up oplossen.
author: srinathvasireddy
manager: sivan
keywords: Hoe kan ik een back-up maken? status van het back-upsysteem
ms.service: backup
ms.topic: conceptual
ms.date: 07/22/2019
ms.author: srinathv
ms.openlocfilehash: 8996270acb1525697f29b4251bf4e11d2db62fdf
ms.sourcegitcommit: c72ddb56b5657b2adeb3c4608c3d4c56e3421f2c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2019
ms.locfileid: "68465363"
---
# <a name="troubleshoot-system-state-backup"></a>Problemen met de systeem status back-up oplossen

In dit artikel worden oplossingen beschreven voor problemen die kunnen optreden bij het gebruik van systeem status back-up.

## <a name="basic-troubleshooting"></a>Eenvoudige probleemoplossing
U wordt aangeraden de onderstaande validatie uit te voeren voordat u begint met het oplossen van problemen met de systeem status back-up:

- [Controleren of de MARS-agent (Microsoft Azure Recovery Services) up-to-date is](https://go.microsoft.com/fwlink/?linkid=229525&clcid=0x409)
- [Zorg ervoor dat er netwerkverbinding is tussen de MARS-agent en Azure](https://aka.ms/AB-A4dp50)
- Controleer of Microsoft Azure Recovery Services wordt uitgevoerd (in Service-console). Indien nodig kunt u de services opnieuw starten en de bewerking nogmaals proberen
- [Zorg ervoor dat er 5-10% ruimte vrij is op de locatie van de scratch-map](https://aka.ms/AB-AA4dwtt)
- [Controleer of er geen ander proces of antivirussoftware conflicten veroorzaakt met Azure Backup](https://aka.ms/AB-AA4dwtk)
- [Geplande back-up mislukt, maar handmatige back-up werkt](https://aka.ms/ScheduledBackupFailManualWorks)
- Zorg ervoor dat uw besturingssysteem helemaal is bijgewerkt
- [Zorg ervoor dat niet-ondersteunde stations en bestanden met niet-ondersteunde kenmerken worden uitgesloten van een back-up](backup-support-matrix-mars-agent.md#supported-drives-or-volumes-for-backup)
- Zorg ervoor dat de **systeemklok** op de beveiligde computer is geconfigureerd voor de juiste tijdzone <br>
- [Zorg ervoor dat op de server ten minste .Net Framework-versie 4.5.2 of hoger is geïnstalleerd](https://www.microsoft.com/download/details.aspx?id=30653)<br>
- Als u probeert om **uw server opnieuw te registreren** bij een kluis, moet u: <br>
  - Ervoor zorgen dat de agent wordt verwijderd op de server en uit de portal <br>
  - Dezelfde wachtwoordzin gebruiken die in eerste instantie is gebruikt voor het registreren van de server <br>
- In het geval van een offline back-up moet Azure PowerShell versie 3.7.0 op zowel de bron-als de Kopieer computer worden geïnstalleerd voordat u een offline back-upbewerking start
- [Overweging wanneer back-upagent wordt uitgevoerd op een virtuele machine van Azure](https://aka.ms/AB-AA4dwtr)

### <a name="limitation"></a>Beperking
- Het herstellen van bestanden naar andere hardware met behulp van systeemstatusherstel wordt niet aanbevolen door Microsoft
- Systeem status back-up ondersteunt momenteel ' on-premises ' Windows-servers. deze functionaliteit is niet beschikbaar voor virtuele Azure-machines.

## <a name="pre-requisite"></a>Vereiste

Voordat u de systeem status back-up met Azure Backup oplost, moet u ervoor zorgen dat u de onderstaande controle vooraf informeert.  

### <a name="verify-windows-server-backup-is-installed"></a>Controleren of de Windows Server Back-up is geïnstalleerd

Zorg ervoor dat Windows Server Back-up is geïnstalleerd en is ingeschakeld op de server. Als u de installatie status wilt controleren, voert u de onderstaande Power shell-opdracht uit:

 ```
 PS C:\> Get-WindowsFeature Windows-Server-Backup
 ```
Als voor de uitvoer de **installatie status** **beschikbaar**wordt weer gegeven, betekent dit dat Windows Server back-upfunctie beschikbaar is voor de installatie, maar niet is geïnstalleerd op de server. Als Windows Server Back-up echter niet is geïnstalleerd, gebruikt u een van de onderstaande methoden om het te installeren.

**Methode 1: Windows Server Back-up installeren met behulp van Power shell**

Als u Windows Server Back-up wilt installeren met behulp van Power shell, voert u de onderstaande opdracht uit:

  ```
  PS C:\> Install-WindowsFeature -Name Windows-Server-Backup
  ```

**Methode 2: Windows Server Back-up installeren met behulp van Serverbeheer**

Als u Windows Server Back-up wilt installeren met behulp van Serverbeheer, voert u de onderstaande stappen uit:

1. Klik in de **Server Manager** op **functies en onderdelen toevoegen**. De **wizard functies en onderdelen toevoegen** wordt weer gegeven.

    ![Dashboard](./media/backup-azure-system-state-troubleshoot/server_management.jpg)

2. Selecteer **installatie type** en klik op **volgende**.

    ![Installatie type](./media/backup-azure-system-state-troubleshoot/install_type.jpg)

3. Selecteer een server uit de Server groep en klik op **volgende**. In de serverrol, behoud de standaard selectie en klik op **volgende**.
4. Selecteer **Windows Server back-up** op het tabblad **functies** en klik op **volgende**.

    ![database](./media/backup-azure-system-state-troubleshoot/features.png)

5. Klik op het tabblad **bevestiging** op **installeren** om het installatie proces te starten.
6. Op het tabblad **resultaten** wordt weer gegeven dat de functie Windows Server back-up is geïnstalleerd op uw Windows-Server.

    ![Resultaat](./media/backup-azure-system-state-troubleshoot/results.jpg)


### <a name="system-volume-information-permission"></a>Machtiging voor systeem volume gegevens

Zorg ervoor dat het lokale systeem volledig beheer heeft over de map **System Volume Information** op het volume waarop Windows is geïnstalleerd. Dit is doorgaans **C:\System Volume gegevens**. Windows Server back-up kan mislukken als de bovenstaande machtigingen niet juist zijn ingesteld

### <a name="dependent-services"></a>Afhankelijke services

Zorg ervoor dat de onderstaande services worden uitgevoerd:

**Service naam** | **Opstart type**
--- | ---
Externe procedure aanroep (RPC) | Automatisch
COM+-gebeurtenis systeem (Event System) | Automatisch
Systeem gebeurtenis meldings service (SENS) | Automatisch
Volume Shadow Copy (VSS) | Handmatig
Micro soft software Shadow Copy Provider (SWPRV) | Handmatig

### <a name="validate-windows-server-backup-status"></a>Windows Server Back-up status valideren

Als u de Windows Server Back-up status wilt valideren, voert u de onderstaande stappen uit:

  * Zorg ervoor dat WSB Power shell wordt uitgevoerd

    -   Voer `Get-WBJob` uit vanuit een Power shell met verhoogde bevoegdheid en zorg ervoor dat de volgende fout niet wordt geretourneerd:

    > [!WARNING]
    > Get-WBJob: De term ' Get-WBJob ' wordt niet herkend als de naam van een cmdlet, functie, script bestand of bruikbaar programma. Controleer of de naam juist is gespeld of Controleer of het pad correct is en probeer het opnieuw.

    -   Als dit mislukt met deze fout, installeert u de Windows Server Back-up-functie op de Server computer zoals vermeld in de vereisten voor stap 1.

  * Zorg ervoor dat WSB-back-up correct werkt door de onderstaande opdracht uit te voeren vanaf een opdracht prompt met verhoogde bevoegdheid:

      `wbadmin start systemstatebackup -backuptarget:X: -quiet`

      > [!NOTE]
      >Vervang X door de stationsletter van het volume waar u de systeem status back-upinstallatie kopie wilt opslaan.

    - Regel matig de status van de taak controleren door `Get-WBJob` de opdracht uit te voeren vanuit een verhoogde Power shell        
    - Nadat de back-uptaak is voltooid, controleert u de uiteindelijke status `Get-WBJob -Previous 1` van de taak door de opdracht uit te voeren

Als de taak mislukt, duidt dit op een WSB-probleem dat ertoe leidt dat de systeem status back-ups van de MARS-agent mislukken.

## <a name="common-errors"></a>Veelvoorkomende fouten

### <a name="vss-writer-timeout-error"></a>Time-outfout voor VSS Writer

| Symptoom | Oorzaak | Oplossing
| -- | -- | --
| -MARS-agent mislukt met fout bericht: De WSB-taak is mislukt met VSS-fouten. Controleer de VSS-gebeurtenis Logboeken om de fout op te lossen<br/><br/> -Het volgende fouten logboek is aanwezig in de logboeken van de VSS-toepassings gebeurtenis: "Een VSS Writer heeft een gebeurtenis met fout 0x800423f2 afgewezen, de time-out van de schrijver is verlopen tussen gebeurtenissen voor blok keren en ontdooien."| VSS Writer kan niet tijdig worden voltooid vanwege een gebrek aan CPU-en geheugen resources op de computer <br/><br/> De VSS Writer wordt al gebruikt door een andere back-upsoftware omdat een moment opname bewerking niet kan worden voltooid voor deze back-up | Wacht tot de CPU/het geheugen is vrijgemaakt op het systeem of de processen die te veel geheugen of CPU verbruiken, hebben en probeer het opnieuw. <br/><br/>  Wacht tot de actieve back-up is voltooid en probeer het later opnieuw wanneer er geen back-ups op de computer worden uitgevoerd.


### <a name="insufficient-disk-space-to-grow-shadow-copies"></a>Onvoldoende schijf ruimte om schaduw kopieën te verg Roten

| Symptoom | Oplossing
| -- | --
| -MARS-agent mislukt met fout bericht: Het maken van de back-up is mislukt omdat het schaduw kopie volume niet kan groeien vanwege onvoldoende schijf ruimte op volumes met systeem bestanden <br/><br/> -Het volgende fout/waarschuwings logboek bevindt zich in de logboeken met systeem gebeurtenissen: Er is onvoldoende schijf ruimte op volume C: voor het verg Roten van de schaduw kopie opslag voor schaduw kopieën van C: vanwege deze fout zijn alle schaduw kopieën van volume C: risico dat ze worden verwijderd. | -Maak ruimte vrij in het gemarkeerde volume in het gebeurtenis logboek, zodat er voldoende ruimte is voor het uitbreiden van schaduw kopieën terwijl de back-up wordt uitgevoerd <br/><br/> -Tijdens het configureren van de schaduw kopie ruimte kunnen we de hoeveelheid ruimte beperken die wordt gebruikt voor de schaduw kopie. Zie dit [artikel](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc788050(v=ws.11)#syntax) voor meer informatie


### <a name="efi-partition-locked"></a>EFI-partitie is vergrendeld

| Symptoom | Oplossing
| -- | --
| MARS-agent mislukt met fout bericht: Het maken van een back-up van de systeem status is mislukt omdat de EFI-systeem partitie is vergrendeld. Dit kan worden veroorzaakt door de toegang tot de systeem partitie door de beveiliging van derden of het maken van een back-up van software. | -Als het probleem wordt veroorzaakt door een beveiligings software van derden, moet u contact opnemen met de leverancier van het anti virus zodat ze MARS-agent kunnen toestaan <br/><br/> -Als er een back-upsoftware van derden wordt uitgevoerd, wacht dan tot deze is voltooid en probeer het vervolgens opnieuw.


## <a name="next-steps"></a>Volgende stappen

- Zie [een back-up maken van de systeem status van Windows Server](backup-azure-system-state.md) voor meer informatie over de systeem status van Windows in de implementatie van Resource Manager.
