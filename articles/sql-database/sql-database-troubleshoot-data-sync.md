---
title: Problemen met Azure SQL Data Sync oplossen | Microsoft Docs
description: Informatie over het oplossen van veelvoorkomende problemen met Azure SQL Data Sync.
services: sql-database
ms.date: 07/16/2018
ms.topic: conceptual
ms.service: sql-database
author: allenwux
ms.author: xiwu
manager: craigg
ms.custom: data-sync
ms.openlocfilehash: 8ba4b32f45dd978439b08650e498c3030c618aab
ms.sourcegitcommit: 35ceadc616f09dd3c88377a7f6f4d068e23cceec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2018
ms.locfileid: "39618706"
---
# <a name="troubleshoot-issues-with-sql-data-sync"></a>Problemen oplossen met SQL Data Sync

In dit artikel wordt beschreven hoe u bekende problemen oplossen met Azure SQL Data Sync. Als er een oplossing voor een probleem is, wordt het hier opgegeven.

Zie [Gegevens synchroniseren tussen meerdere cloud- en on-premises databases met SQL Data Sync](sql-database-sync-data.md) voor een overzicht van SQL Data Sync.

## <a name="sync-issues"></a>Problemen met synchronisatie

- [Synchronisatie mislukt in de gebruikersinterface van de portal voor on-premises databases die gekoppeld aan de clientagent zijn](#sync-fails)

- [Mijn groep voor synchronisatie met is de status van de verwerking vastgelopen](#sync-stuck)

- [Zie ik onjuiste gegevens in tabellen](#sync-baddata)

- [Ik zie inconsistente gegevens voor de primaire sleutel na een geslaagde synchronisatie](#sync-pkdata)

- [Ik zie een aanzienlijke afname van prestaties](#sync-perf)

- [Kan ik dit bericht ziet: "kan niet de waarde NULL niet invoegen in de kolom <column>. Kolom staat niet toe dat null-waarden." Wat betekent dit en hoe kan ik doen?](#sync-nulls)

- [Hoe wordt Data Sync kringverwijzingen verwerkt? Dat wil zeggen, wanneer dezelfde gegevens in meerdere synchronisatiegroepen is gesynchroniseerd en als gevolg hiervan blijft wijzigen?](#sync-circ)

### <a name="sync-fails"></a> Synchronisatie mislukt in de gebruikersinterface van de portal voor on-premises databases die gekoppeld aan de clientagent zijn

Synchronisatie mislukt in de portal voor SQL Data Sync UI voor on-premises databases die gekoppeld aan de clientagent zijn. Op de lokale computer waarop de agent wordt uitgevoerd, ziet u System.IO.IOException fouten in het gebeurtenislogboek. De fouten zeggen dat de schijf niet voldoende ruimte heeft.

- **Oorzaak**. Het station heeft onvoldoende ruimte.

- **Resolutie**. Maak meer ruimte op het station waarop de map % TEMP % zich bevindt.

### <a name="sync-stuck"></a> Mijn groep voor synchronisatie met is de status van de verwerking vastgelopen

Een synchronisatiegroep in SQL Data Sync is in de verwerkingsstatus gedurende een lange periode. Deze reageert niet op de **stoppen** opdracht en de logboeken ziet u geen nieuwe items.

Een van de volgende voorwaarden kan leiden tot een groep voor synchronisatie wordt vastgelopen in de verwerkingsstatus:

- **Oorzaak**. De clientagent is offline

- **Resolutie**. Zorg ervoor dat de clientagent voor online is en probeer het opnieuw.

- **Oorzaak**. De clientagent is geïnstalleerd of ontbreekt.

- **Resolutie**. Als de clientagent is verwijderd of anderszins ontbreekt:

    1. Als het bestand bestaat, moet u de agent XML-bestand verwijderen uit de installatiemap van SQL Data Sync.
    1. Installeer de agent op de lokale computer (het kan zijn hetzelfde of een andere computer). Vervolgens, verzendt de agentsleutel die wordt gegenereerd in de portal voor de agent die wordt weergegeven als offline.

- **Oorzaak**. De SQL Data Sync-service is gestopt.

- **Resolutie**. Start de SQL Data Sync-service.

    1. In de **Start** menu, zoekt u **Services**.
    1. Selecteer in de lijst met zoekresultaten **Services**.
    1. Zoek de **SQL Data Sync** service.
    1. Als de servicestatus van de is **gestopt**, met de rechtermuisknop op de servicenaam en selecteer vervolgens **Start**.

> [!NOTE]
> Als de voorgaande informatie niet van uw groep voor synchronisatie met de verwerkingsstatus verlaten, kan Microsoft Support de status van uw groep voor synchronisatie kunt herstellen. Om de synchronisatiestatus van de groep opnieuw instelt, in de [-forum Azure SQL Database](https://social.msdn.microsoft.com/Forums/azure/home?forum=ssdsgetstarted), een bericht maken. In het bericht, zijn uw abonnements-ID en de synchronisatie-ID voor de groep die moet worden ingesteld. Een Microsoft Support engineer reageren op uw bericht en laat u weten wanneer de status is opnieuw ingesteld.

### <a name="sync-baddata"></a> Zie ik onjuiste gegevens in tabellen

Als tabellen die dezelfde naam hebben, maar die afkomstig zijn van andere database-schema's zijn opgenomen in een synchronisatie, ziet u de onjuiste gegevens in de tabellen na de synchronisatie.

- **Oorzaak**. Het inrichtingsproces van SQL Data Sync maakt gebruik van de dezelfde Traceringstabellen voor tabellen die dezelfde naam hebben, maar die zich in verschillende schema's. Als gevolg hiervan, worden wijzigingen van beide tabellen doorgevoerd in de tabel met dezelfde bijhouden. Dit zorgt ervoor dat onjuiste gegevens worden gewijzigd tijdens de synchronisatie.

- **Resolutie**. Zorg ervoor dat de namen van tabellen die bij een synchronisatie betrokken zijn verschillend zijn, zelfs als de tabellen deel uitmaken van verschillende schema's in een database.

### <a name="sync-pkdata"></a> Ik zie inconsistente gegevens voor de primaire sleutel na een geslaagde synchronisatie

Een synchronisatie wordt gerapporteerd als geslaagd, en het logboek bevat geen rijen mislukte of overgeslagen, maar u ziet dat primaire-sleutelgegevens niet consistent tussen de databases in de groep voor synchronisatie is.

- **Oorzaak**. Dit resultaat is standaard. Wijzigingen in een primaire-sleutelkolom leiden tot inconsistente gegevens in de rijen waarin de primaire sleutel is gewijzigd.

- **Resolutie**. Zorg ervoor dat er geen gegevens in een primaire-sleutelkolom is gewijzigd om te voorkomen dat dit probleem. U kunt dit probleem oplossen nadat deze is opgetreden, verwijdert u de rij met inconsistente gegevens uit alle eindpunten in de groep voor synchronisatie. De rij vervolgens opnieuw.

### <a name="sync-perf"></a> Ik zie een aanzienlijke afname van prestaties

De prestaties van uw vermindert aanzienlijk, mogelijk op het punt waar u de gebruikersinterface voor het synchroniseren van gegevens, zelfs niet openen.

- **Oorzaak**. De meest waarschijnlijke oorzaak is een lus synchroniseren. Een lus synchronisatie treedt op wanneer een groep voor synchroniseren door synchronisatie met een activeert een synchroniseren door synchronisatie groep B, die vervolgens wordt er een groep voor synchroniseren door synchronisatie met A. De werkelijke situatie mogelijk meer complexe, en dit kan betrekking hebben op meer dan twee synchronisatiegroepen op de hoogte. Het probleem is dat er een circulaire activeren is van de synchronisatie die wordt veroorzaakt door synchronisatiegroepen overlappen elkaar.

- **Resolutie**. De beste oplossing is voorkomen. Zorg ervoor dat u geen kringverwijzingen in uw synchronisatiegroepen hebt. Een rij die is gesynchroniseerd met een synchronisatiegroep kan niet worden gesynchroniseerd door een andere synchronisatiegroep.

### <a name="sync-nulls"></a> Kan ik dit bericht ziet: "kan niet de waarde NULL niet invoegen in de kolom <column>. Kolom staat niet toe dat null-waarden." Wat betekent dit en hoe kan ik doen? 
Dit foutbericht geeft aan dat een van de twee volgende problemen is opgetreden:
-  Een tabel hebben niet een primaire sleutel. U kunt dit probleem oplossen, toevoegen een primaire sleutel voor alle tabellen die u synchroniseert.
-  Er is een WHERE-component in de instructie CREATE INDEX is. Data Sync afhandelen niet van dit probleem. Om op te lossen dit probleem, verwijder de component WHERE of wijzigingen handmatig aanbrengen voor alle databases. 
 
### <a name="sync-circ"></a> Hoe wordt Data Sync kringverwijzingen verwerkt? Dat wil zeggen, wanneer dezelfde gegevens in meerdere synchronisatiegroepen is gesynchroniseerd en als gevolg hiervan blijft wijzigen?
Data Sync verwerken niet kringverwijzingen. Moet u deze kunt vermijden. 

## <a name="client-agent-issues"></a>Problemen met client-agent

- [De clientagent installeren, verwijderen of herstellen mislukt](#agent-install)

- [De clientagent werkt niet nadat ik de verwijderen annuleren](#agent-uninstall)

- [Mijn database wordt niet weergegeven in de lijst met agent](#agent-list)

- [Clientagent niet wordt gestart (fout 1069)](#agent-start)

- [Kan ik de agentsleutel kan niet worden verzonden](#agent-key)

- [De clientagent kan niet worden verwijderd uit de portal als de gekoppelde on-premises database niet bereikbaar is](#agent-delete)

- [Lokale Sync-Agent-app kan geen verbinding maken met de lokale synchronisatieservice](#agent-connect)

### <a name="agent-install"></a> De clientagent installeren, verwijderen of herstellen mislukt

- **Oorzaak**. Veel scenario's kan deze fout kunnen veroorzaken. Bekijk de logboeken om te bepalen van de oorzaak van deze fout.

- **Resolutie**. Genereren om de oorzaak van de fout, en bekijk de logboeken van Windows Installer. U kunt aanmelden bij een opdrachtprompt inschakelen. Bijvoorbeeld, als het gedownloade bestand uit AgentServiceSetup.msi LocalAgentHost.msi, genereren en logboekbestanden bekijken met behulp van de volgende opdrachtregels uit:

    -   Voor installaties: `msiexec.exe /i SQLDataSyncAgent-Preview-ENU.msi /l\*v LocalAgentSetup.InstallLog`
    -   Voor verwijdert: `msiexec.exe /x SQLDataSyncAgent-se-ENU.msi /l\*v LocalAgentSetup.InstallLog`

    U kunt ook inschakelen logboekregistratie in voor alle installaties die worden uitgevoerd door de Windows Installer. De Microsoft Knowledge Base-artikel [het inschakelen van logboekregistratie voor Windows Installer](https://support.microsoft.com/help/223300/how-to-enable-windows-installer-logging) biedt een oplossing met één muisklik logboekregistratie voor Windows Installer inschakelen. Het biedt ook de locatie van de logboeken.

### <a name="agent-uninstall"></a> De clientagent werkt niet nadat ik de verwijderen annuleren

De clientagent werkt niet, zelfs na het annuleren van de verwijdering.

- **Oorzaak**. Dit gebeurt omdat de clientagent voor SQL Data Sync referenties niet opslaan.

- **Resolutie**. U kunt deze twee oplossingen proberen:

    -   Gebruik services.msc opnieuw in te voeren van de referenties voor de clientagent.
    -   Verwijder deze clientagent en installeer vervolgens een nieuwe. Download en installeer de nieuwste clientagent vanuit [Downloadcentrum](http://go.microsoft.com/fwlink/?linkid=221479).

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

U ontdekt dat de agent niet wordt uitgevoerd op een computer die als host fungeert voor SQL Server. Wanneer u de agent handmatig te starten probeert, ziet u een dialoogvenster dat het bericht verschijnt ' fout 1069: de service is niet gestart vanwege een fout met aanmelding. "

![Het dialoogvenster fout 1069 synchroniseren](media/sql-database-troubleshoot-data-sync/sync-error-1069.png)

- **Oorzaak**. Een waarschijnlijke oorzaak van deze fout is dat het wachtwoord op de lokale server is gewijzigd sinds u de beveiligingsagent en het wachtwoord voor de agent hebt gemaakt.

- **Resolutie**. Wachtwoord van de agent bijwerken op uw huidige serverwachtwoord:

  1. Ga naar de SQL Data Sync client-agent-service.  
    a. Selecteer **Start**.  
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
  1. Open de app opnieuw.

## <a name="setup-and-maintenance-issues"></a>Problemen met installatie en onderhoud

- [Er verschijnt een bericht 'schijf vol is'](#setup-space)

- [Ik kan mijn synchronisatiegroep niet verwijderen](#setup-delete)

- [Ik kan geen registratie van een on-premises SQL Server-database](#setup-unreg)

- [Ik hoef niet voldoende bevoegdheden voor het starten van systeemservices](#setup-perms)

- [Een database heeft de status van een 'Verouderde'](#setup-date)

- [Een groep voor synchronisatie met heeft een 'Verouderde' status](#setup-date2)

- [Een synchronisatiegroep kan niet binnen drie minuten na het verwijderen of stoppen van de agent worden verwijderd](#setup-delete2)

- [Wat gebeurt er wanneer ik een verloren of beschadigd-database herstellen?](#setup-restore)

### <a name="setup-space"></a> Er verschijnt een bericht 'schijf vol is'

- **Oorzaak**. Het bericht 'schijf vol is' mogelijk weergegeven als er bestanden overblijven moeten worden verwijderd. Dit wordt mogelijk veroorzaakt door antivirussoftware of bestanden zijn geopend wanneer delete-bewerkingen worden uitgevoerd.

- **Resolutie**. De synchronisatiebestanden die zich in de map % temp % handmatig verwijderen (`del \*sync\* /s`). Verwijder vervolgens de submappen in de map % temp %.

> [!IMPORTANT]
> Niet alle bestanden niet verwijderen terwijl synchronisatie uitgevoerd wordt.

### <a name="setup-delete"></a> Ik kan mijn synchronisatiegroep niet verwijderen

Uw poging om een synchronisatiegroep verwijderen is mislukt. Een van de volgende scenario's kan leiden tot een synchronisatiegroep verwijderen is mislukt:

- **Oorzaak**. De clientagent is offline.

- **Resolutie**. Zorg ervoor dat de clientagent voor online is en probeer het opnieuw.

- **Oorzaak**. De clientagent is geïnstalleerd of ontbreekt.

- **Resolutie**. Als de clientagent is verwijderd of anderszins ontbreekt:  
    a. Als het bestand bestaat, moet u de agent XML-bestand verwijderen uit de installatiemap van SQL Data Sync.  
    b. Installeer de agent op de lokale computer (het kan zijn hetzelfde of een andere computer). Vervolgens, verzendt de agentsleutel die wordt gegenereerd in de portal voor de agent die wordt weergegeven als offline.

- **Oorzaak**. Een database is offline.

- **Resolutie**. Zorg ervoor dat uw SQL-databases en SQL Server-databases alle online zijn.

- **Oorzaak**. De synchronisatiegroep is ingericht of worden gesynchroniseerd.

- **Resolutie**. Wacht totdat het inrichten of sync-proces is voltooid en probeer het vervolgens opnieuw de synchronisatiegroep wordt verwijderd.

### <a name="setup-unreg"></a> Ik kan geen registratie van een on-premises SQL Server-database

- **Oorzaak**. U wilt waarschijnlijk registratie ongedaan maken van een database die al is verwijderd.

- **Resolutie**. Als u wilt een on-premises SQL Server-database de registratie ongedaan maken, selecteert u de database en selecteer vervolgens **geforceerd verwijderen**.

  Als deze bewerking is mislukt voor de database verwijderen uit de groep voor synchronisatie:

  1. Stoppen en opnieuw opstarten van de client-agent host-service:  
    a. Selecteer de **Start** menu.  
    b. Voer in het zoekvak **services.msc**.  
    c. In de **programma's** sectie van de zoekopdracht resulteert deelvenster, dubbelklikt u op **Services**.  
    d. Met de rechtermuisknop op de **SQL Data Sync** service.  
    e. Als de service wordt uitgevoerd, stoppen.  
    f. Met de rechtermuisknop op de service en selecteer vervolgens **Start**.  
    g. Controleer of de database nog steeds is geregistreerd. Als deze niet meer is geregistreerd, bent u klaar. Ga anders verder met de volgende stap.
  1. Open de app in client-agent (SqlAzureDataSyncAgent).
  1. Selecteer **referenties bewerken**, en voer vervolgens de referenties voor de database.
  1. Doorgaan met verwijderen van registratie.

### <a name="setup-perms"></a> Ik hoef niet voldoende bevoegdheden voor het starten van systeemservices

- **Oorzaak**. Deze fout treedt op in twee gevallen:
  -   Naam van de gebruiker en/of het wachtwoord zijn onjuist.
  -   Het opgegeven gebruikersaccount beschikt niet over voldoende machtigingen om aan te melden als een service.

- **Resolutie**. Logboek-op-as-a-service-referenties voor het gebruikersaccount waarbij verlenen:

  1. Ga naar **Start** > **Configuratiescherm** > **Systeembeheer** > **lokaal beveiligingsbeleid**  >  **Lokaal beleid** > **gebruiker Rights Management**.
  1. Selecteer **aanmelden als een service**.
  1. In de **eigenschappen** dialoogvenster vak, het gebruikersaccount toevoegen.
  1. Selecteer **Apply** en vervolgens **OK**.
  1. Sluit alle vensters.

### <a name="setup-date"></a> Een database heeft de status van een 'Verouderde'

- **Oorzaak**. SQL Data Sync Hiermee verwijdert u de databases die offline zijn van de service voor 45 dagen of meer (zoals geteld vanaf het moment dat de database offline is gegaan) zijn. Als een database van 45 dagen of meer offline is en vervolgens weer online komt, wordt de status ervan is **verouderde**.

- **Resolutie**. U kunt voorkomen dat een **verouderde** status door ervoor te zorgen dat geen van de databases van 45 dagen of meer offline gaan.

  Als de status van een database **verouderde**:

  1. Verwijderen van de database is een **verouderde** status van de groep voor synchronisatie.
  1. Toevoegen van de database weer aan de groep voor synchronisatie.

  > [!WARNING]
  > U verliest alle wijzigingen die in deze database terwijl deze offline was.

### <a name="setup-date2"></a> Een groep voor synchronisatie met heeft een 'Verouderde' status

- **Oorzaak**. Als een of meer wijzigingen niet toepassen voor de hele bewaarperiode van 45 dagen, kan een synchronisatiegroep verouderd raken.

- **Resolutie**. Om te voorkomen dat een **verouderde** status voor een groep voor synchronisatie, bekijk de resultaten van uw synchronisatietaken in de weergave van geschiedenis van een regelmatig gebruikt. Onderzoeken en oplossen van eventuele wijzigingen die niet voldoen aan te passen.

  Als de status van een synchronisatiegroep **verouderde**, verwijderen van de groep voor synchronisatie en vervolgens opnieuw maken.

### <a name="setup-delete2"></a> Een synchronisatiegroep kan niet binnen drie minuten na het verwijderen of stoppen van de agent worden verwijderd

U kunt een synchronisatiegroep niet verwijderen binnen drie minuten na het verwijderen of stoppen van de bijbehorende SQL Data Sync-clientagent.

- **Resolutie**.

  1. Een synchronisatiegroep verwijderen terwijl de bijbehorende synchronisatieagents online zijn (aanbevolen).
  1. Als de agent offline is, maar is geïnstalleerd, breng online op de lokale computer. Wacht totdat de status van de agent moet worden weergegeven als **Online** in de portal voor SQL Data Sync. Verwijder vervolgens de groep voor synchronisatie.
  1. Als de agent offline is, omdat deze is verwijderd:  
    a.  Als het bestand bestaat, moet u de agent XML-bestand verwijderen uit de installatiemap van SQL Data Sync.  
    b.  Installeer de agent op de lokale computer (het kan zijn hetzelfde of een andere computer). Vervolgens, verzendt de agentsleutel die wordt gegenereerd in de portal voor de agent die wordt weergegeven als offline.  
    c. Probeer te verwijderen van de groep voor synchronisatie.

### <a name="setup-restore"></a> Wat gebeurt er wanneer ik een verloren of beschadigd-database herstellen?

Als u een verloren of beschadigde database vanaf een back-up herstellen, is er mogelijk een non-convergentie van gegevens in de synchronisatiegroepen waartoe de database behoort.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over SQL Data Sync:

-   [Gegevens synchroniseren tussen meerdere cloud- en on-premises databases met SQL Data Sync (Preview)](sql-database-sync-data.md)  
-   [Azure SQL Data Sync instellen](sql-database-get-started-sql-data-sync.md)  
-   [Aanbevolen procedures voor Azure SQL Data Sync](sql-database-best-practices-data-sync.md)  
-   [Azure SQL Data Sync bewaken met Log Analytics](sql-database-sync-monitor-oms.md)  
-   Voer PowerShell-voorbeelden uit die laten zien hoe u SQL Data Sync configureert:  
    -   [PowerShell gebruiken om meerdere Azure SQL-databases te synchroniseren](scripts/sql-database-sync-data-between-sql-databases.md)  
    -   [PowerShell gebruiken om te synchroniseren tussen een Azure SQL-database en een on-premises database](scripts/sql-database-sync-data-between-azure-onprem.md)  

Zie voor meer informatie over SQL Database:

-   [Wat is de Azure SQL Database-service?](sql-database-technical-overview.md)
-   [Database Lifecycle Management (DLM)](https://msdn.microsoft.com/library/jj907294.aspx)
