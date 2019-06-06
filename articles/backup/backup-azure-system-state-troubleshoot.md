---
title: Oplossen van de systeemstatusback-up met Azure Backup
description: Oplossen van problemen in de systeemstatusback-up.
services: backup
author: srinathvasireddy
manager: sivan
keywords: back-ups maken; back-up van systeemstatus
ms.service: backup
ms.topic: conceptual
ms.date: 05/09/2019
ms.author: srinathvasireddy
ms.openlocfilehash: 53b9f8fb58a6e70a4bd2cd02adb9ce824466d7de
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2019
ms.locfileid: "66481592"
---
# <a name="troubleshoot-system-state-backup"></a>Back-up van systeemstatus oplossen

Dit artikel wordt beschreven oplossingen voor problemen die u tegenkomen kunt tijdens het gebruik van systeemstatusback-up.

## <a name="pre-requisite"></a>Vereiste

Voordat we systeemstatusback-up met Azure Backup oplossen, controleert u of u voeren de volgende vereisten controleren.  

### <a name="verify-windows-server-backup-is-installed"></a>Controleer of dat Windows Server back-up is geïnstalleerd

Zorg ervoor dat Windows Server back-up is geïnstalleerd en ingeschakeld in de server. Om te controleren of de status van de installatie, voer de onderstaande PowerShell-opdracht:

 ```
 PS C:\> Get-WindowsFeature Windows-Server-Backup
 ```
Als de uitvoer geeft de **installatiestatus** als **beschikbaar**, betekent dat dat Windows Server back-functie is beschikbaar voor de installatie, maar niet geïnstalleerd op de server. Maar als Windows Server back-up is niet geïnstalleerd, voert u een van de onderstaande methoden om deze te installeren.

**Methode 1: Windows Server back-up installeren met behulp van PowerShell**

Voer voor het installeren van Windows Server back-up met behulp van PowerShell de onderstaande opdracht:

  ```
  PS C:\> Install-WindowsFeature -Name Windows-Server-Backup
  ```

**Methode 2: Windows Server back-up met behulp van Serverbeheer installeren**

Installeer Windows Server back-up met behulp van Serverbeheer, het volgende:

1. In de **Server Manager** en klikt u op **functies en onderdelen toevoegen**. De **de wizard functies en onderdelen toevoegen** wordt weergegeven.

    ![Dashboard](./media/backup-azure-system-state-troubleshoot/server_management.jpg)

2. Selecteer **installatietype** en klikt u op **volgende**.

    ![Installatietype](./media/backup-azure-system-state-troubleshoot/install_type.jpg)

3. Selecteer een server uit de servergroep en klik op **volgende**. In de functie, laat de standaardselectie en klik op **volgende**.
4. Selecteer **Windows Server back-up** in **functies** tabblad en klik op **volgende**.

    ![database](./media/backup-azure-system-state-troubleshoot/features.png)

5. In de **bevestiging** tabblad **installeren** om het installatieproces te starten.
6. In de **resultaten** tabblad worden weergegeven de Windows Server Backup-functie is op uw Windows-Server is geïnstalleerd.

    ![Resultaat](./media/backup-azure-system-state-troubleshoot/results.jpg)


### <a name="system-volume-information-permission"></a>System Volume information machtiging

Zorg ervoor dat het lokale systeem volledig beheer heeft op **System Volume Information** map zich in het volume waarop windows is geïnstalleerd. Dit is doorgaans **C:\System Volume Information**. Windows Server back-up kan mislukken als de bovenstaande machtigingen niet correct zijn ingesteld

### <a name="dependent-services"></a>Afhankelijke services

Zorg ervoor dat de hieronder services worden uitgevoerd:

**Servicenaam** | **Opstarttype**
--- | ---
Remote Procedure Call (RPC) | Automatisch
COM +-gebeurtenis System(EventSystem) | Automatisch
Systeem Event Notification Service(SENS) | Automatisch
Volume Shadow Copy(VSS) | Handmatig
Microsoft-Software Shadow Copy Provider(SWPRV) | Handmatig

### <a name="validate-windows-server-backup-status"></a>Status van Windows Server back-up valideren

Voor het valideren van de status van Windows Server Backup uitvoeren van de hieronder:

  * Controleer of dat WSB PowerShell wordt uitgevoerd

    -   Voer `Get-WBJob` vanaf een verhoogde PowerShell en zorg ervoor dat deze niet de volgende fout is geretourneerd:

    > [!WARNING]
    > Get-WBJob: De term 'Get-WBJob' wordt niet herkend als de naam van een cmdlet, functie, scriptbestand of programma. Controleer de spelling van de naam, of als een pad opgenomen is, dat het pad juist is en probeer het opnieuw.

    -   Als dit vanwege de volgende fout mislukt Installeer vervolgens opnieuw de functie Windows Server back-up op de server zoals vermeld in de stap 1-vereisten.

  * Zorg ervoor dat WSB back-up goed werkt door het uitvoeren van de onderstaande opdracht vanaf een opdrachtprompt met verhoogde bevoegdheid:

      ` wbadmin start systemstatebackup -backuptarget:X: -quiet `

      > [!NOTE]
      >Vervang X met de stationsletter van het volume waar u voor het opslaan van de systeemstatus back-up van de installatiekopie.

    - Controleer regelmatig de status van de taak door het uitvoeren `Get-WBJob` opdracht met verhoogde bevoegdheden PowerShell        
    - Nadat de back-uptaak is voltooid de definitieve status van de taak controleren door te voeren `Get-WBJob -Previous 1` opdracht

Als de taak is mislukt, betekent dit een probleem met de WSB dat tot mislukte systeemstatusback-ups van de MARS-agent leiden zou.

## <a name="common-errors"></a>Veelvoorkomende fouten

### <a name="vss-writer-timeout-error"></a>VSS-schrijver time-outfout

| Symptoom | Oorzaak | Oplossing
| -- | -- | --
| -MARS agent is mislukt met het volgende foutbericht: "WSB-taak is mislukt met de VSS-fouten. Controleer de systeemgebeurtenislogboeken VSS om op te lossen van de fout"<br/><br/> -Logboek is aanwezig in de gebeurtenislogboeken van de VSS-toepassing de volgende fout: "Een VSS-schrijver heeft een gebeurtenis met de volgende fout 0x800423f2 geweigerd, time-out opgetreden van de schrijver tussen de gebeurtenissen stilzetten en ontdooien."| VSS-writer is kan niet worden voltooid in de tijd vanwege gebrek aan resources CPU en geheugen op de machine <br/><br/> De VSS-schrijver wordt al gebruikt door een andere back-upsoftware, als gevolg hiervan de momentopnamebewerking kan niet worden voltooid voor deze back-up | Wachten op CPU/geheugen worden vrijgemaakt systeem of afbreken processen, waarbij te veel geheugen/CPU en probeer het opnieuw <br/><br/>  Wacht totdat de lopende back-up voltooid en probeer de bewerking op een later tijdstip wanneer er geen back-ups worden uitgevoerd op de machine


### <a name="insufficient-disk-space-to-grow-shadow-copies"></a>Onvoldoende schijfruimte om te groeien schaduwkopieën

| Symptoom | Oplossing
| -- | --
| -MARS agent is mislukt met het volgende foutbericht: Back-up is mislukt omdat de volume shadow copy kan niet groter wegens onvoldoende schijfruimte op volumes met de systeembestanden <br/><br/> -Fout/waarschuwingslogboek volgen is aanwezig in de logboeken voor systeemgebeurtenissen volsnap: "Er is onvoldoende schijfruimte op volume C: te laten groeien van de opslagruimte op schaduwkopie voor schaduwkopieën van C: vanwege deze fout alle schaduwkopieën van volume C: lopen risico wordt verwijderd" | -Ruimte vrij in het geselecteerde volume in het gebeurtenislogboek zodat er voldoende ruimte voor schaduwkopieën te laten groeien wanneer back-up uitgevoerd wordt <br/><br/> -Tijdens het configureren van schaduwkopie heeft we de hoeveelheid ruimte die wordt gebruikt voor de schaduwkopie kan beperken, Zie voor meer informatie dit [artikel](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc788050(v=ws.11)#syntax)


### <a name="efi-partition-locked"></a>EFI-systeempartitie is vergrendeld

| Symptoom | Oplossing
| -- | --
| MARS-agent is mislukt met het volgende foutbericht: "System state back-up is mislukt omdat de EFI-systeempartitie is vergrendeld. Dit kan worden veroorzaakt door partitie toegang tot het systeem door de beveiliging van een derde partij of back-up software" | -Als het probleem veroorzaakt door van een derde partij beveiligingssoftware wordt, moet u contact op met de leverancier van de Anti-Virus zodat ze MARS-agent <br/><br/> -Als een back-upsoftware van derden wordt uitgevoerd, wacht totdat deze zijn voltooid en probeer vervolgens back up


## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over de status van de Windows system Resource Manager-implementatie, [maakt u een Back-up van Windows Server-systeemstatus](backup-azure-system-state.md)
