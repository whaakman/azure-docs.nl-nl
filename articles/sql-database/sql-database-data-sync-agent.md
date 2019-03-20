---
title: Gegevens synchroniseren-Agent voor Azure SQL Data Sync | Microsoft Docs
description: Meer informatie over het installeren en uitvoeren van de Data Sync-Agent voor Azure SQL Data Sync om gegevens te synchroniseren met on-premises SQL Server-databases
services: sql-database
ms.service: sql-database
ms.subservice: data-movement
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: allenwux
ms.author: xiwu
ms.reviewer: douglasl
manager: craigg
ms.date: 12/20/2018
ms.openlocfilehash: 5d8b3b91771fda3a714112979b8bf9df39c4a9b3
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58088686"
---
# <a name="data-sync-agent-for-azure-sql-data-sync"></a>Data Sync-Agent voor Azure SQL Data Sync

Gegevens synchroniseren met on-premises SQL Server-databases te installeren en configureren van de Data Sync-Agent voor Azure SQL Data Sync. Zie voor meer informatie over SQL Data Sync [gegevens synchroniseren tussen meerdere cloud en on-premises databases met SQL Data Sync](sql-database-sync-data.md).

> [!IMPORTANT]
> Azure SQL Data Sync biedt op dit moment **geen** ondersteuning voor beheerde exemplaren voor Azure SQL Database.

## <a name="download-and-install"></a>Downloaden en installeren

Als u wilt de gegevens Sync-Agent downloaden, gaat u naar [SQL Azure Data Sync-Agent](https://www.microsoft.com/download/details.aspx?id=27693).

### <a name="install-silently"></a>Op de achtergrond installeren

Als u wilt de gegevens Sync-Agent op de achtergrond installeren vanaf de opdrachtprompt, voert u een opdracht die vergelijkbaar is met het volgende voorbeeld. Controleer of de bestandsnaam van het gedownloade MSI-bestand, en geef uw eigen waarden voor de **TARGETDIR** en **SERVICEACCOUNT** argumenten.

- Als u geen waarde opgeeft voor **TARGETDIR**, de standaardwaarde is `C:\Program Files (x86)\Microsoft SQL Data Sync 2.0`.

- Als u opgeeft `LocalSystem` als de waarde van **SERVICEACCOUNT**, SQL Server-verificatie gebruikt bij het configureren van de agent verbinding maken met de on-premises SQL-Server.

- Als u een domeingebruikersaccount of een lokaal gebruikersaccount als de waarde van opgeven **SERVICEACCOUNT**, hoeft u ook te geven van het wachtwoord met de **SERVICEPASSWORD** argument. Bijvoorbeeld `SERVICEACCOUNT="<domain>\<user>"  SERVICEPASSWORD="<password>"`.

```cmd
msiexec /i "SQLDataSyncAgent-2.0-x86-ENU.msi" TARGETDIR="C:\Program Files (x86)\Microsoft SQL Data Sync 2.0" SERVICEACCOUNT="LocalSystem" /qn
```

## <a name="sync-data-with-sql-server-on-premises"></a>Gegevens synchroniseren met on-premises SQL Server

Zie configureren van de Data Sync-Agent, zodat u kunt gegevens met een of meer on-premises SQL Server-databases synchroniseren [toevoegen van een on-premises SQL Server-database](sql-database-get-started-sql-data-sync.md#add-on-prem).

## <a name="agent-faq"></a> Veelgestelde vragen over Data Sync-Agent

### <a name="why-do-i-need-a-client-agent"></a>Waarom moet ik een clientagent

De SQL Data Sync-service communiceert met SQL Server-databases via de clientagent. Deze beveiligingsfunctie voorkomt dat rechtstreekse communicatie met databases achter een firewall. Wanneer de SQL Data Sync-service communiceert met de agent, dit gebeurt met behulp van versleutelde verbindingen en een unieke token of *agentsleutel*. De agent met behulp van de connection string en agent sleutel worden geverifieerd door de SQL Server-databases. Dit ontwerp biedt een hoge mate van beveiliging voor uw gegevens.

### <a name="how-many-instances-of-the-local-agent-ui-can-be-run"></a>Hoeveel exemplaren van de lokale agent kan gebruikersinterface worden uitgevoerd

Er kan slechts één exemplaar van de gebruikersinterface worden uitgevoerd.

### <a name="how-can-i-change-my-service-account"></a>Hoe kan ik mijn serviceaccount wijzigen

Nadat u een clientagent hebt geïnstalleerd, is de enige manier om de serviceaccount te wijzigen voor de installatie ongedaan en een nieuwe clientagent installeren met het nieuwe serviceaccount.

### <a name="how-do-i-change-my-agent-key"></a>Hoe kan ik mijn agentsleutel wijzigen

Een agentsleutel kan slechts één keer worden gebruikt door een agent. Het kan niet opnieuw worden gebruikt wanneer u verwijdert vervolgens een nieuwe agent opnieuw installeren en deze worden gebruikt door meerdere agents. Als u maken van een nieuwe sleutel voor een bestaande agent wilt, moet u ervoor dat dezelfde sleutel wordt vastgelegd met de clientagent en de SQL Data Sync-service zijn.

### <a name="how-do-i-retire-a-client-agent"></a>Hoe ik een clientagent buiten gebruik stellen

Als u onmiddellijk ongeldig te maken of buiten gebruik stellen van een agent, de sleutel in de portal opnieuw genereren, maar niet indienen in de gebruikersinterface van de Agent. Een sleutel opnieuw genereren, wordt de vorige sleutel ongeacht als de bijbehorende agent online of offline is ongeldig.

### <a name="how-do-i-move-a-client-agent-to-another-computer"></a>Hoe verplaats ik een clientagent naar een andere computer

Als u de lokale agent uitvoeren vanaf een andere computer wilt dan de momenteel op, kunt u het volgende doen:

1. Installeer de agent op de gewenste computer.
2. Meld u aan bij de portal voor SQL Data Sync en een agentsleutel genereren voor de nieuwe agent.
3. De gebruikersinterface van de nieuwe agent gebruiken voor het indienen van de nieuwe agentsleutel.
4. Wacht terwijl de clientagent downloadt de lijst met on-premises databases die eerder zijn geregistreerd.
5. Databasereferenties opgeven voor alle databases die worden weergegeven als niet bereikbaar. Deze databases moet bereikbaar is vanaf de nieuwe computer waarop de agent is geïnstalleerd.

## <a name="agent-tshoot"></a> Problemen met Data Sync-Agent oplossen

- [De clientagent installeren, verwijderen of herstellen mislukt](#agent-install)

- [De clientagent werkt niet nadat ik de verwijderen annuleren](#agent-uninstall)

- [Mijn database wordt niet weergegeven in de lijst met agent](#agent-list)

- [Clientagent niet wordt gestart (fout 1069)](#agent-start)

- [Kan ik de agentsleutel kan niet worden verzonden](#agent-key)

- [De clientagent kan niet worden verwijderd uit de portal als de gekoppelde on-premises database niet bereikbaar is](#agent-delete)

- [Lokale Sync-Agent-app kan geen verbinding maken met de lokale synchronisatieservice](#agent-connect)

### <a name="agent-install"></a> De clientagent installeren, verwijderen of herstellen mislukt

- **Oorzaak**. Veel scenario's kan deze fout kunnen veroorzaken. Bekijk de logboeken om te bepalen van de oorzaak van deze fout.

- **Resolutie**. Genereren om de oorzaak van de fout, en bekijk de logboeken van Windows Installer. U kunt aanmelden bij een opdrachtprompt inschakelen. Bijvoorbeeld, als het gedownloade installatiebestand `SQLDataSyncAgent-2.0-x86-ENU.msi`, genereren en logboekbestanden bekijken met behulp van de volgende opdrachtregels uit:

  - Voor installaties: `msiexec.exe /i SQLDataSyncAgent-2.0-x86-ENU.msi /l*v LocalAgentSetup.Log`
  - Voor verwijdert: `msiexec.exe /x SQLDataSyncAgent-2.0-x86-ENU.msi /l*v LocalAgentSetup.Log`

    U kunt ook inschakelen logboekregistratie in voor alle installaties die worden uitgevoerd door de Windows Installer. De Microsoft Knowledge Base-artikel [het inschakelen van logboekregistratie voor Windows Installer](https://support.microsoft.com/help/223300/how-to-enable-windows-installer-logging) biedt een oplossing met één muisklik logboekregistratie voor Windows Installer inschakelen. Het biedt ook de locatie van de logboeken.

### <a name="agent-uninstall"></a> De clientagent werkt niet nadat ik de verwijderen annuleren

De clientagent werkt niet, zelfs na het annuleren van de verwijdering.

- **Oorzaak**. Dit gebeurt omdat de clientagent voor SQL Data Sync referenties niet opslaan.

- **Resolutie**. U kunt deze twee oplossingen proberen:

    -   Gebruik services.msc opnieuw in te voeren van de referenties voor de clientagent.
    -   Verwijder deze clientagent en installeer vervolgens een nieuwe. Download en installeer de nieuwste clientagent vanuit [Downloadcentrum](https://go.microsoft.com/fwlink/?linkid=221479).

### <a name="agent-list"></a> Mijn database wordt niet weergegeven in de lijst met agent

Wanneer u probeert een bestaande SQL Server-database toevoegen aan een groep voor synchronisatie, kan de database niet wordt weergegeven in de lijst van agents.

Dit probleem kunnen worden veroorzaakt door deze scenario's:

- **Oorzaak**. De groep van agent en sync-client zich in verschillende datacenters.

- **Resolutie**. De clientagent en de groep voor synchronisatie moeten zich in hetzelfde datacenter. Dit als u wilt instellen, hebt u twee opties:

    -   Maak een nieuwe agent in het datacenter waar de synchronisatiegroep zich bevindt. Registreer vervolgens de database met die agent.
    -   Verwijder de huidige groep voor synchronisatie. Maak vervolgens opnieuw de groep voor synchronisatie in het datacenter waar de agent zich bevindt.

- **Oorzaak**. Lijst met databases van de clientagent niet actueel.

- **Resolutie**. Stoppen en opnieuw opstarten van de client-agent-service.

    De lokale agent downloadt de lijst met gekoppelde databases op de eerste verzending van de agentsleutel. Deze downloaden de lijst met gekoppelde databases op de volgende agent sleutel inzendingen niet. Databases die zijn geregistreerd tijdens het verplaatsen van een agent niet worden weergegeven in het oorspronkelijke exemplaar van de agent.

### <a name="agent-start"></a> Clientagent niet wordt gestart (fout 1069)

U ontdekt dat de agent niet wordt uitgevoerd op een computer die als host fungeert voor SQL Server. Wanneer u de agent handmatig te starten probeert, ziet u een dialoogvenster dat het bericht verschijnt ' fout 1069: De service is niet gestart vanwege een fout met aanmelding."

![Het dialoogvenster fout 1069 synchroniseren](media/sql-database-troubleshoot-data-sync/sync-error-1069.png)

- **Oorzaak**. Een waarschijnlijke oorzaak van deze fout is dat het wachtwoord op de lokale server is gewijzigd sinds u de beveiligingsagent en het wachtwoord voor de agent hebt gemaakt.

- **Resolutie**. Wachtwoord van de agent bijwerken op uw huidige serverwachtwoord:

  1. Ga naar de SQL Data Sync client-agent-service.  
    a. Selecteer **Starten**.  
    b. Voer in het zoekvak **services.msc**.  
    c. Selecteer in de lijst met zoekresultaten **Services**.  
    d. In de **Services** venster, Ga naar de vermelding voor **SQL Data Sync-Agent**.  
  1. Met de rechtermuisknop op **SQL Data Sync-Agent**, en selecteer vervolgens **stoppen**.
  1. Met de rechtermuisknop op **SQL Data Sync-Agent**, en selecteer vervolgens **eigenschappen**.
  1. Op **eigenschappen van SQL Data Sync-Agent**, selecteer de **aanmelden** tabblad.
  1. In de **wachtwoord** vak, Voer uw wachtwoord.
  1. In de **wachtwoord bevestigen** vak, uw wachtwoord opnieuw invoeren.
  1. Selecteer **Apply** en vervolgens **OK**.
  1. In de **Services** venster met de rechtermuisknop op de **SQL Data Sync-Agent** service, en klik vervolgens op **Start**.
  1. Sluit de **Services** venster.

### <a name="agent-key"></a> Kan ik de agentsleutel kan niet worden verzonden

Nadat u maakt of opnieuw maken van een sleutel voor een agent, die u wilt de sleutel via de toepassing SqlAzureDataSyncAgent indienen. Het verzenden is mislukt om te voltooien.

![Fout bij het dialoogvenster synchronisatie - agentsleutel kan niet worden verzonden](media/sql-database-troubleshoot-data-sync/sync-error-cant-submit-agent-key.png)

- **Vereisten**. Voordat u doorgaat, controleert u de volgende vereisten:

  - De SQL Data Sync Windows-service wordt uitgevoerd.

  - Het serviceaccount voor SQL Data Sync Windows-service heeft toegang tot het netwerk.

  - De uitgaande 1433-poort is geopend in de lokale firewall-regel.

  - Het lokale IP-adres is toegevoegd aan de server of database-firewall-regel voor de metagegevensdatabase voor synchronisatie.

- **Oorzaak**. De agentsleutel identificatie unieke van elke lokale agent. De sleutel moet voldoen aan twee voorwaarden:

  -   De sleutel van de client-agent op de SQL Data Sync-server en de lokale computer moet identiek zijn.
  -   De sleutel van de client-agent kan slechts één keer worden gebruikt.

- **Resolutie**. Als uw agent niet werkt, is omdat één of beide van deze voorwaarden niet wordt voldaan. Ophalen van de agent opnieuw werken:

  1. Genereer een nieuwe sleutel.
  1. De nieuwe sleutel van toepassing op de agent.

  De nieuwe sleutel toepassen op de agent:

  1. In Windows Verkenner, Ga naar de map voor agentinstallatie. De standaardinstallatiemap is C:\\Program Files (x86)\\Microsoft SQL Data Sync.
  1. Dubbelklik op de bin-submap.
  1. Open de toepassing SqlAzureDataSyncAgent.
  1. Selecteer **indienen Agentsleutel**.
  1. Plak de sleutel van het Klembord in de daarvoor bestemde ruimte.
  1. Selecteer **OK**.
  1. Sluit het programma.

### <a name="agent-delete"></a> De clientagent kan niet worden verwijderd uit de portal als de gekoppelde on-premises database niet bereikbaar is

Als een lokaal eindpunt (dat wil zeggen, een database) die is geregistreerd bij een SQL Data Sync-client-agent niet meer bereikbaar is, kan de clientagent kan niet worden verwijderd.

- **Oorzaak**. De lokale agent kan niet worden verwijderd omdat de database niet bereikbaar is nog steeds geregistreerd bij de agent. Wanneer u probeert te verwijderen van de agent, wordt de verwijdering probeert te bereiken van de database, die is mislukt.

- **Resolutie**. Gebruik 'geforceerd verwijderen' om de onbereikbaar database te verwijderen.

> [!NOTE]
> Als u synchronisatie metagegevenstabellen, blijven na een "geforceerd verwijderen', gebruik `deprovisioningutil.exe` om op te schonen ze.

### <a name="agent-connect"></a> Lokale Sync-Agent-app kan geen verbinding maken met de lokale synchronisatieservice

- **Resolutie**. Voer de volgende stappen uit:

  1. Sluit de app.  
  1. Open het deelvenster met Component Services.  
    a. Voer in het zoekvak op de taakbalk **services.msc**.  
    b. Dubbelklik in de lijst met zoekresultaten op **Services**.  
  1. Stop de **SQL Data Sync** service.
  1. Start opnieuw op de **SQL Data Sync** service.  
  1. de app opnieuw openen.

## <a name="run-the-data-sync-agent-from-the-command-prompt"></a>De Data Sync-Agent uitvoeren vanaf de opdrachtprompt

U kunt de volgende Data Sync-Agent-opdrachten uitvoeren vanaf de opdrachtprompt:

### <a name="ping-the-service"></a>Ping de service

#### <a name="usage"></a>Gebruik

```cmd
SqlDataSyncAgentCommand.exe -action pingsyncservice
```

#### <a name="example"></a>Voorbeeld

```cmd
SqlDataSyncAgentCommand.exe -action "pingsyncservice"
```

### <a name="display-registered-databases"></a>Geregistreerde databases weergeven

#### <a name="usage"></a>Gebruik

```cmd
SqlDataSyncAgentCommand.exe -action displayregistereddatabases
```

#### <a name="example"></a>Voorbeeld

```cmd
SqlDataSyncAgentCommand.exe -action "displayregistereddatabases"
```

### <a name="submit-the-agent-key"></a>Verzenden van de agentsleutel

#### <a name="usage"></a>Gebruik

```cmd
Usage: SqlDataSyncAgentCommand.exe -action submitagentkey -agentkey [agent key]  -username [user name] -password [password]
```

#### <a name="example"></a>Voorbeeld

```cmd
SqlDataSyncAgentCommand.exe -action submitagentkey -agentkey [agent key generated from portal, PowerShell, or API] -username [user name to sync metadata database] -password [user name to sync metadata database]
```

### <a name="register-a-database"></a>Registreren van een database

#### <a name="usage"></a>Gebruik

```cmd
SqlDataSyncAgentCommand.exe -action registerdatabase -servername [on-premisesdatabase server name] -databasename [on-premisesdatabase name]  -username [domain\\username] -password [password] -authentication [sql or windows] -encryption [true or false]
```

#### <a name="examples"></a>Voorbeelden

```cmd
SqlDataSyncAgentCommand.exe -action "registerdatabase" -serverName localhost -databaseName testdb -authentication sql -username <user name> -password <password> -encryption true

SqlDataSyncAgentCommand.exe -action "registerdatabase" -serverName localhost -databaseName testdb -authentication windows -encryption true

```

### <a name="unregister-a-database"></a>Registratie van een database

Wanneer u deze opdracht om de registratie van een database te gebruiken, deprovisions deze de database volledig. Als de database maakt deel uit van andere synchronisatiegroepen, breekt deze bewerking de synchronisatiegroepen.

#### <a name="usage"></a>Gebruik

```cmd
SqlDataSyncAgentCommand.exe -action unregisterdatabase -servername [on-premisesdatabase server name] -databasename [on-premisesdatabase name]
```

#### <a name="example"></a>Voorbeeld

```cmd
SqlDataSyncAgentCommand.exe -action "unregisterdatabase" -serverName localhost -databaseName testdb
```

### <a name="update-credentials"></a>Referenties bijwerken

#### <a name="usage"></a>Gebruik

```cmd
SqlDataSyncAgentCommand.exe -action updatecredential -servername [on-premisesdatabase server name] -databasename [on-premisesdatabase name]  -username [domain\\username] -password [password] -authentication [sql or windows] -encryption [true or false]
```

#### <a name="examples"></a>Voorbeelden

```cmd
SqlDataSyncAgentCommand.exe -action "updatecredential" -serverName localhost -databaseName testdb -authentication sql -username <user name> -password <password> -encryption true

SqlDataSyncAgentCommand.exe -action "updatecredential" -serverName localhost -databaseName testdb -authentication windows -encryption true
```

## <a name="next-steps"></a>Volgende stappen

Zie de volgende artikelen voor meer informatie over SQL Data Sync:

-   Overzicht: [Gegevens synchroniseren tussen meerdere cloud- en on-premises databases met SQL Data Sync](sql-database-sync-data.md)
-   Data Sync instellen
    - In de portal - [Zelfstudie: SQL Data Sync instellen om gegevens te synchroniseren tussen Azure SQL Database en SQL Server on-premises](sql-database-get-started-sql-data-sync.md)
    - Met PowerShell
        -  [PowerShell gebruiken om meerdere Azure SQL-databases te synchroniseren](scripts/sql-database-sync-data-between-sql-databases.md)
        -  [PowerShell gebruiken om te synchroniseren tussen een Azure SQL-database en een on-premises database](scripts/sql-database-sync-data-between-azure-onprem.md)
-   Best practices: [Best practices voor Azure SQL Data Sync](sql-database-best-practices-data-sync.md)
-   Monitor - [SQL Data Sync bewaken met Azure Monitor-Logboeken](sql-database-sync-monitor-oms.md)
-   Problemen oplossen: [Problemen met Azure SQL Data Sync oplossen](sql-database-troubleshoot-data-sync.md)
-   Het synchronisatieschema bijwerken
    -   Met Transact-SQL: [De replicatie van schemawijzigingen in Azure SQL Data Sync automatiseren](sql-database-update-sync-schema.md)
    -   Met PowerShell: [PowerShell gebruiken voor het bijwerken van het synchronisatieschema in een bestaande synchronisatiegroep](scripts/sql-database-sync-update-schema.md)
