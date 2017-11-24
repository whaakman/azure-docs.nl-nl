---
title: Problemen met synchroniseren van Azure SQL-gegevens (Preview) | Microsoft Docs
description: Informatie over het oplossen van veelvoorkomende problemen met het synchroniseren van Azure SQL-gegevens (Preview).
services: sql-database
ms.date: 11/13/2017
ms.topic: article
ms.service: sql-database
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 50cabbaa584671e52c1ea7efbd2ad990b8438272
ms.sourcegitcommit: 1d8612a3c08dc633664ed4fb7c65807608a9ee20
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/20/2017
---
# <a name="troubleshoot-issues-with-sql-data-sync-preview"></a>Problemen oplossen met het synchroniseren van de SQL-gegevens (Preview)

In dit artikel wordt beschreven hoe bekende problemen oplossen met Azure SQL-gegevenssynchronisatie (Preview). Als er een oplossing voor een probleem is, het hier opgegeven.

Zie voor een overzicht van het synchroniseren van de SQL-gegevens (Preview) [synchroniseren van gegevens via meerdere cloud en on-premises databases met Azure SQL-gegevenssynchronisatie (Preview)](sql-database-sync-data.md).

## <a name="sync-issues"></a>Problemen met synchronisatie

### <a name="sync-fails-in-the-portal-ui-for-on-premises-databases-that-are-associated-with-the-client-agent"></a>Synchronisatie mislukt in de gebruikersinterface van de portal voor lokale databases die gekoppeld aan de clientagent zijn

#### <a name="description-and-symptoms"></a>Beschrijving en symptomen

Synchronisatie mislukt bij het synchroniseren van de SQL-gegevens (Preview) gebruikersinterface portal voor on-premises-databases die gekoppeld aan de agent zijn. In de lokale computer die de agent wordt uitgevoerd, ziet u System.IO.IOException fouten in het gebeurtenislogboek. De fouten zeggen dat de schijf onvoldoende ruimte is.

#### <a name="resolution"></a>Oplossing

Maak meer schijfruimte vrij op het station waarop de map % TEMP %.

### <a name="my-sync-group-is-stuck-in-the-processing-state"></a>Mijn groep voor synchronisatie is vastgelopen in de verwerkingsstatus

#### <a name="description-and-symptoms"></a>Beschrijving en symptomen

Een groep voor synchronisatie in het synchroniseren van de SQL-gegevens (Preview) is al in de verwerkingsstatus lang duren. Deze reageert niet op de **stoppen** opdracht en de logboeken worden geen nieuwe items weergegeven.

#### <a name="cause"></a>Oorzaak

Een van de volgende voorwaarden kan leiden tot een groep voor synchronisatie wordt vastgelopen in de verwerkingsstatus:

-   **De clientagent is offline**. Zorg ervoor dat de clientagent online is en probeer het opnieuw.

-   **De clientagent is geïnstalleerd of ontbreekt**. Als de clientagent is geïnstalleerd of anderszins ontbreekt:

    1. Verwijder de agent XML-bestand uit de installatiemap synchroniseren van de SQL-gegevens (Preview) als het bestand bestaat.
    2. Installeer de agent op de lokale computer (deze kan zijn hetzelfde of een andere computer). Dien vervolgens de agent-sleutel die wordt gegenereerd in de portal voor de agent die wordt weergegeven als offline.

-   **Het synchroniseren van gegevens van SQL-service wordt gestopt**.

    1. In de **Start** menu, zoekt u **Services**.
    2. Selecteer in de lijst met zoekresultaten **Services**.
    3. Zoek de **synchroniseren van de SQL-gegevens (Preview)** service.
    4. Als de status van de service **gestopt**, met de rechtermuisknop op de servicenaam en selecteer vervolgens **Start**.

#### <a name="resolution"></a>Oplossing

Als de voorgaande informatie niet uw groep voor synchronisatie buiten de verwerkingsstatus verplaatst, kan Microsoft Support opnieuw instellen van de status van de groep voor synchronisatie. Hebben de synchronisatiestatus van de groep opnieuw instelt, in de [-forum Azure SQL Database](https://social.msdn.microsoft.com/Forums/azure/home?forum=ssdsgetstarted), een advertentie maken. In het bericht, zijn uw abonnements-ID en de synchronisatie-ID voor de groep die moet worden ingesteld. Een Microsoft Support engineer reageert op uw advertentie, en kunt u weten wanneer de status is opnieuw ingesteld.

### <a name="i-see-erroneous-data-in-my-tables"></a>Zie ik onjuiste gegevens in Mijn tabellen

#### <a name="description-and-symptoms"></a>Beschrijving en symptomen

Als de tabellen met dezelfde naam, maar die afkomstig zijn van een andere database schema's zijn opgenomen in een synchronisatie, ziet u onjuiste gegevens in de tabellen na de synchronisatie.

#### <a name="cause"></a>Oorzaak

De SQL-gegevenssynchronisatie (Preview) inrichtingsproces maakt gebruik van de dezelfde Traceringstabellen voor tabellen die dezelfde naam hebben, maar die zich in verschillende schema's. Als gevolg hiervan worden wijzigingen van beide tabellen doorgevoerd in de tabel met dezelfde bijhouden. Dit zorgt ervoor dat wijzigingen onjuiste gegevens tijdens de synchronisatie.

#### <a name="resolution"></a>Oplossing

Zorg ervoor dat de namen van tabellen die bij een synchronisatie betrokken zijn verschillend zijn, zelfs als de tabellen tot verschillende schema's in een database behoren.

### <a name="i-see-inconsistent-primary-key-data-after-a-successful-sync"></a>Zie ik inconsistente gegevens voor de primaire sleutel na een synchronisatie

#### <a name="description-and-symptoms"></a>Beschrijving en symptomen

Een synchronisatie wordt gerapporteerd als geslaagd, en het logboek bevat geen mislukte of overgeslagen rijen, maar u merkt dat de primaire-sleutelgegevens niet consistent tussen de databases in de groep voor synchronisatie is.

#### <a name="cause"></a>Oorzaak

Dit is standaard. Wijzigingen in een primaire-sleutelkolom leiden tot inconsistente gegevens in de rijen waarbij de primaire sleutel is gewijzigd.

#### <a name="resolution"></a>Oplossing

Om te voorkomen dat dit probleem, zorg dat er geen gegevens in een primaire sleutelkolom is gewijzigd.

U kunt dit probleem oplossen nadat deze is opgetreden, de rij met inconsistente gegevens uit alle eindpunten in de groep voor synchronisatie te verwijderen. De rij vervolgens opnieuw.

### <a name="i-see-a-significant-degradation-in-performance"></a>Zie ik een significante vermindering in prestaties

#### <a name="description-and-symptoms"></a>Beschrijving en symptomen

Prestaties van uw vermindert aanzienlijk, mogelijk tot het punt waar u zelfs de gebruikersinterface voor het synchroniseren van gegevens niet openen.

#### <a name="cause"></a>Oorzaak

De meest waarschijnlijke oorzaak is een lus synchronisatie. Een synchronisatie-lus treedt op wanneer een groep voor synchroniseren door synchronisatie met een activeert een synchroniseren door synchronisatie-groep B, die vervolgens een a synchroniseren door synchronisatie wordt geactiveerd De werkelijke situatie mogelijk complexere en het mogelijk om het meer dan twee synchronisatiegroepen in de lus. Het probleem is dat er een kringverwijzing activatie is van de synchronisatie die wordt veroorzaakt door synchronisatiegroepen elkaar overlappen.

#### <a name="resolution"></a>Oplossing

De beste oplossing wordt voorkomen. Zorg ervoor dat er geen kringverwijzingen in de synchronisatiegroepen. Een rij die wordt gesynchroniseerd door één groep voor synchronisatie kan niet worden gesynchroniseerd door een andere groep voor synchronisatie.

### <a name="i-see-this-message-cannot-insert-the-value-null-into-the-column-column-column-does-not-allow-nulls-what-does-this-mean-and-how-can-i-fix-it"></a>Dit bericht weergegeven: ' kan de waarde NULL niet invoegen in de kolom \<kolom\>. Kolom mag geen null-waarden." Wat betekent dit en hoe kan ik doen? 
Dit foutbericht geeft aan dat een van de twee volgende problemen is opgetreden:
-  Een tabel hebben niet een primaire sleutel. U kunt dit probleem oplossen door moet u een primaire sleutel toevoegen aan alle tabellen die u wilt synchroniseren.
-  Er is een WHERE-component in de instructie CREATE INDEX. Synchroniseren van gegevens (Preview) kunnen deze voorwaarde niet worden verwerkt. U kunt dit probleem oplossen door de component WHERE verwijderen of wijzigingen handmatig aanbrengen voor alle databases. 
 
### <a name="how-does-data-sync-preview-handle-circular-references-that-is-when-the-same-data-is-synced-in-multiple-sync-groups-and-keeps-changing-as-a-result"></a>Hoe wordt synchroniseren van gegevens (Preview) kringverwijzingen verwerkt? Dat wil zeggen, wanneer dezelfde gegevens in meerdere synchronisatiegroepen is gesynchroniseerd en als gevolg hiervan blijft wijzigen?
Synchroniseren van gegevens (Preview) kunnen kringverwijzingen niet worden verwerkt. Zorg ervoor dat ze kunnen worden vermeden. 

## <a name="client-agent-issues"></a>Problemen met client-agent

### <a name="the-client-agent-install-uninstall-or-repair-fails"></a>De clientagent installeren, verwijderen of herstellen mislukt

#### <a name="cause"></a>Oorzaak

Veel scenario's kunnen deze fout veroorzaken. Bekijk de logboeken om te bepalen van de oorzaak van deze fout.

#### <a name="resolution"></a>Oplossing

Ga voor de oorzaak van de fout genereren en bekijk de logboeken van Windows Installer. U kunt aanmelden bij een opdrachtprompt inschakelen. Bijvoorbeeld, als het gedownloade bestand voor AgentServiceSetup.msi LocalAgentHost.msi, genereren en beoordelen van logboekbestanden met behulp van de volgende opdrachtregels:

-   Voor installaties:`msiexec.exe /i SQLDataSyncAgent-Preview-ENU.msi /l\*v LocalAgentSetup.InstallLog`
-   Wordt verwijderd voor:`msiexec.exe /x SQLDataSyncAgent-se-ENU.msi /l\*v LocalAgentSetup.InstallLog`

U kunt ook logboekregistratie voor alle installaties die worden uitgevoerd door Windows Installer inschakelen. De Microsoft Knowledge Base-artikel [het inschakelen van logboekregistratie voor Windows Installer](https://support.microsoft.com/help/223300/how-to-enable-windows-installer-logging) biedt een oplossing voor één muisklik inschakelen van logboekregistratie voor Windows Installer. Het bevat ook de locatie van de logboeken.

### <a name="my-client-agent-doesnt-work"></a>Mijn clientagent werkt niet

#### <a name="description-and-symptoms"></a>Beschrijving en symptomen

U kunt de volgende berichten te zien krijgen wanneer u probeert de clientagent te gebruiken:

"Synchronisatie is mislukt met uitzondering er een fout is opgetreden tijdens het deserialiseren van de parameter www.microsoft.com/.../05:GetBatchInfoResult. (Zie InnerException voor meer informatie."

"De binnenste Uitzonderingsbericht: Type 'Microsoft.Synchronization.ChangeBatch' is een ongeldig verzameltype aangezien het geen standaardconstructor."

#### <a name="cause"></a>Oorzaak

Dit is een bekend probleem met het synchroniseren van de SQL-gegevens (Preview)-installatie. De meest waarschijnlijke oorzaak van dit bericht is een van de volgende opties:

-   U kunt Windows 8 Developer Preview worden uitgevoerd.
-   Hebt u .NET Framework 4.5 is geïnstalleerd.

#### <a name="resolution"></a>Oplossing

Zorg ervoor dat u de clientagent installeren op een computer waarop Windows 8 Developer Preview wordt niet uitgevoerd en dat .NET Framework 4.5 is niet geïnstalleerd.

### <a name="my-client-agent-doesnt-work-after-i-cancel-the-uninstall"></a>Mijn clientagent werkt niet wanneer ik de verwijdering annuleren

#### <a name="description-and-symptoms"></a>Beschrijving en symptomen

De clientagent werkt niet, zelfs nadat u de verwijdering annuleert.

#### <a name="cause"></a>Oorzaak

Dit gebeurt omdat de clientagent voor het synchroniseren van de SQL-gegevens (Preview) worden niet referenties opgeslagen.

#### <a name="resolution"></a>Oplossing

U kunt deze twee oplossingen proberen:

-   Gebruik services.msc om de referenties opnieuw invoeren voor de clientagent.
-   Verwijderen van deze clientagent en installeer vervolgens een nieuwe. Download en installeer de nieuwste clientagent van [Downloadcentrum](http://go.microsoft.com/fwlink/?linkid=221479).

### <a name="my-database-isnt-listed-in-the-agent-list"></a>Mijn database wordt niet vermeld in de lijst van agent

#### <a name="description-and-symptoms"></a>Beschrijving en symptomen

Wanneer u probeert een bestaande SQL Server-database toevoegen aan een groep voor synchronisatie, wordt de database wordt niet weergegeven in de lijst met agents.

#### <a name="cause"></a>Oorzaak

Dit probleem mogelijk worden veroorzaakt door deze scenario's:

-   De groep van agent en sync-client zich in verschillende datacenters.
-   De clientagent lijst met databases niet actueel.

#### <a name="resolution"></a>Oplossing

De oplossing is afhankelijk van de oorzaak.

- **De groep van agent en sync-client zich in verschillende datacenters**

    De clientagent en de groep voor synchronisatie moeten zich in hetzelfde datacenter. Dit als u wilt instellen, hebt u twee opties:

    -   Maak een nieuwe agent in het datacenter waarin de groep voor synchronisatie bevindt. Registreer de database met die agent.
    -   Verwijder de huidige groep voor synchronisatie. Maak vervolgens opnieuw de groep voor synchronisatie in het datacenter waar de agent zich bevindt.

- **De clientagent lijst met databases niet actueel**

    Stop en start de client agent-service opnieuw op.

    De lokale agent downloadt de lijst met de bijbehorende databases alleen op de eerste verzending van de sleutel van de agent. Deze downloaden niet. de lijst met de bijbehorende databases op latere agent sleutel voorbeelden. Databases die zijn geregistreerd tijdens het verplaatsen van een agent worden niet weergegeven in het oorspronkelijke agentexemplaar.

### <a name="client-agent-doesnt-start-error-1069"></a>Agent-client niet wordt gestart (fout 1069)

#### <a name="description-and-symptoms"></a>Beschrijving en symptomen

U ontdekken dat de agent wordt niet uitgevoerd op een computer die als host fungeert voor SQL Server. Wanneer u probeert de agent handmatig te starten, ziet u een dialoogvenster dat het bericht verschijnt ' fout 1069: de service is niet gestart vanwege een aanmeldingsfout. "

![Gegevens synchroniseren 1069 foutdialoogvenster](media/sql-database-troubleshoot-data-sync/sync-error-1069.png)

#### <a name="cause"></a>Oorzaak

Een mogelijke oorzaak van dit probleem is dat het wachtwoord op de lokale server is gewijzigd sinds u de agent en het wachtwoord voor de agent hebt gemaakt.

#### <a name="resolution"></a>Oplossing

Wachtwoord van de agent een update naar uw huidige serverwachtwoord:

1. Zoek de clientagent voor synchroniseren van de SQL-gegevens (Preview) Preview service.  
    a. Selecteer **Start**.  
    b. Voer in het zoekvak **services.msc**.  
    c. Selecteer in de lijst met zoekresultaten **Services**.  
    d. In de **Services** venster, blader naar de vermelding voor **synchroniseren van de SQL-gegevens (Preview) Agent Preview**.  
2. Met de rechtermuisknop op **synchroniseren van de SQL-gegevens (Preview) Agent Preview**, en selecteer vervolgens **stoppen**.
3. Met de rechtermuisknop op **synchroniseren van de SQL-gegevens (Preview) Agent Preview**, en selecteer vervolgens **eigenschappen**.
4. Op **synchroniseren van de SQL-gegevens (Preview) Preview Agenteigenschappen**, selecteer de **aanmelden** tabblad.
5. In de **wachtwoord** Voer uw wachtwoord.
6. In de **wachtwoord bevestigen** vak, uw wachtwoord opnieuw invoeren.
7. Selecteer **toepassen**, en selecteer vervolgens **OK**.
8. In de **Services** venster met de rechtermuisknop op de **synchroniseren van de SQL-gegevens (Preview) Agent Preview** service en klik vervolgens op **Start**.
9. Sluit de **Services** venster.

### <a name="i-cant-submit-the-agent-key"></a>Ik heb de sleutel van de agent kan niet worden verzonden

#### <a name="description-and-symptoms"></a>Beschrijving en symptomen

Nadat u hebt gemaakt of opnieuw maken van een sleutel voor een agent, die u wilt de sleutel via de toepassing SqlAzureDataSyncAgent verzenden. Verzending van het niet kan worden voltooid.

![Fout bij het dialoogvenster synchronisatie - sleutel van de agent kan niet worden verzonden](media/sql-database-troubleshoot-data-sync/sync-error-cant-submit-agent-key.png)

Voordat u verdergaat, controleert u de volgende voorwaarden:

-   Het synchroniseren van de SQL-gegevens (Preview) Windows-service wordt uitgevoerd.  
-   Het serviceaccount voor het synchroniseren van de SQL-gegevens (Preview) Preview Windows-service heeft toegang tot het netwerk.    
-   De clientagent kunt contact opnemen met de Locator-Service. Controleer of de volgende registersleutel de https://locator.sync.azure.com/LocatorServiceApi.svc waarde is:  
    -   Op een x86 computer:`HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Microsoft\\SQL Azure Data Sync\\LOCATORSVCURI`  
    -   Op een x64 computer:`HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Wow6432Node\\Microsoft\\SQL Azure Data Sync\\LOCATORSVCURI`

#### <a name="cause"></a>Oorzaak

De sleutel van de agent is een unieke identificatie voor elke lokale agent. De sleutel moet voldoen aan twee voorwaarden:

-   De sleutel van de client-agent op de server synchroniseren van de SQL-gegevens (Preview) en de lokale computer moet identiek zijn.
-   De sleutel van de client-agent kan slechts één keer worden gebruikt.

#### <a name="resolution"></a>Oplossing

Als de agent niet werkt, is dit omdat een of beide volgende voorwaarden wordt niet voldaan. Ophalen van de agent opnieuw werken:

1. Een nieuwe sleutel te genereren.
2. De nieuwe sleutel van toepassing op de agent.

De nieuwe sleutel met de agent toepassen:

1. Ga naar de installatiemap van de agent in Windows Verkenner. De standaardinstallatiemap is C:\\Program Files (x86)\\synchroniseren van Microsoft SQL-gegevens.
2. Dubbelklik op de bin-submap.
3. Open de toepassing SqlAzureDataSyncAgent.
4. Selecteer **indienen Agentcode**.
5. Plak de sleutel van het Klembord in de daarvoor bestemde ruimte.
6. Selecteer **OK**.
7. Sluit het programma.

### <a name="the-client-agent-cant-be-deleted-from-the-portal-if-its-associated-on-premises-database-is-unreachable"></a>De clientagent kan niet worden verwijderd uit de portal als de bijbehorende lokale-database niet bereikbaar is

#### <a name="description-and-symptoms"></a>Beschrijving en symptomen

Als een lokaal eindpunt (dat wil zeggen, een database) dat is geregistreerd bij een clientagent synchroniseren van de SQL-gegevens (Preview) wordt niet bereikbaar is, kan de clientagent kan niet worden verwijderd.

#### <a name="cause"></a>Oorzaak

De lokale agent kan niet worden verwijderd omdat de database niet bereikbaar is nog steeds geregistreerd met de agent. Wanneer u probeert te verwijderen van de agent, wordt de verwijdering probeert te bereiken van de database mislukt.

#### <a name="resolution"></a>Oplossing

Gebruik 'geforceerd verwijderen' verwijderen van de database niet bereikbaar.

> [!NOTE]
> Als synchronisatie metagegevenstabellen blijven na een 'geforceerd verwijderen', gebruikt u deprovisioningutil.exe ze opschonen.

### <a name="local-sync-agent-app-cant-connect-to-the-local-sync-service"></a>Lokale Sync-Agent-app kan geen verbinding met de lokale sync-service

#### <a name="resolution"></a>Oplossing

Voer de volgende stappen uit:

1. De app af te sluiten.  
2. Open het deelvenster Component Services.  
    a. Voer in het zoekvak op de taakbalk **services.msc**.  
    b. Dubbelklik in de zoekresultaten op **Services**.  
3. Stop de **synchroniseren van de SQL-gegevens (Preview) Preview** service.
4. Start opnieuw op de **synchroniseren van de SQL-gegevens (Preview) Preview** service.  
5. Open de app opnieuw.

## <a name="setup-and-maintenance-issues"></a>Problemen met Setup en onderhoud

### <a name="i-get-a-disk-out-of-space-message"></a>Er verschijnt een bericht 'geen ruimte meer-schijf'

#### <a name="cause"></a>Oorzaak

Het bericht 'geen ruimte meer-schijf' wordt mogelijk weergegeven als er bestanden overblijven moeten worden verwijderd. Dit wordt mogelijk veroorzaakt door antivirussoftware of bestanden zijn geopend wanneer delete-bewerkingen worden uitgevoerd.

#### <a name="resolution"></a>Oplossing

De synchronisatiebestanden die zich in de map % temp % handmatig verwijderen (`del \*sync\* /s`). Verwijder de submappen in de map % temp %.

> [!IMPORTANT]
> Verwijder geen bestanden terwijl synchronisatie uitgevoerd wordt.

### <a name="i-cant-delete-my-sync-group"></a>Ik kan mijn groep voor synchronisatie niet verwijderen.

#### <a name="description-and-symptoms"></a>Beschrijving en symptomen

Uw poging om te verwijderen van een groep voor synchronisatie is mislukt.

#### <a name="causes"></a>Oorzaken

Een van de volgende scenario's kan leiden tot mislukte pogingen tot het verwijderen van een groep voor synchronisatie:

-   De clientagent is offline.
-   De clientagent is geïnstalleerd of ontbreekt. 
-   Een database is offline. 
-   De groep voor synchronisatie is ingericht of worden gesynchroniseerd. 

#### <a name="resolution"></a>Oplossing

Op te lossen niet verwijderen van een groep voor synchronisatie:

-   Zorg ervoor dat de clientagent online is en probeer het opnieuw.
-   Als de clientagent is geïnstalleerd of anderszins ontbreekt:  
    a. Verwijder de agent XML-bestand uit de installatiemap synchroniseren van de SQL-gegevens (Preview) als het bestand bestaat.  
    b. Installeer de agent op de lokale computer (deze kan zijn hetzelfde of een andere computer). Dien vervolgens de agent-sleutel die wordt gegenereerd in de portal voor de agent die wordt weergegeven als offline.
-   Zorg ervoor dat het synchroniseren van de SQL-gegevens (Preview)-service wordt uitgevoerd:  
    a. In de **Start** menu, zoekt u **Services**.  
    b. Selecteer in de lijst met zoekresultaten **Services**.  
    c. Zoek de **synchroniseren van de SQL-gegevens (Preview) Preview** service.  
    d. Als de status van de service **gestopt**, met de rechtermuisknop op de servicenaam en selecteer vervolgens **Start**.
-   Zorg ervoor dat uw SQL-databases en SQL Server-databases alle online zijn.
-   Wacht totdat het proces voor het inrichten of synchronisatie is voltooid en probeer vervolgens de groep voor synchronisatie te verwijderen.

### <a name="i-cant-unregister-an-on-premises-sql-server-database"></a>Ik kan geen Hef de registratie van een lokale SQL Server-database.

#### <a name="cause"></a>Oorzaak

U wilt waarschijnlijk Hef de registratie van een database die al is verwijderd.

#### <a name="resolution"></a>Oplossing

Selecteer de database om de registratie van een lokale SQL Server-database, en selecteer vervolgens **geforceerd verwijderen**.

Als deze bewerking is mislukt voor de database verwijderen uit de groep voor synchronisatie:

1. Stoppen en opnieuw opstarten van de client-agent host-service:  
    a. Selecteer de **Start** menu.  
    b. Voer in het zoekvak **services.msc**.  
    c. In de **programma's** sectie van de zoekopdracht resulteert deelvenster, dubbelklikt u op **Services**.  
    d. Met de rechtermuisknop op de **synchroniseren van de SQL-gegevens (Preview)** service.  
    e. Als de service wordt uitgevoerd, stoppen.  
    f. Met de rechtermuisknop op de service en selecteer vervolgens **Start**.  
    g. Controleer of de database nog steeds is geregistreerd. Als het is niet langer geregistreerd, bent u klaar. Ga anders verder met de volgende stap.
2. Open de app in client-agent (SqlAzureDataSyncAgent).
3. Selecteer **referenties bewerken**, en voer de referenties voor de database.
4. Doorgaan met verwijderen van de registratie.

### <a name="i-dont-have-sufficient-privileges-to-start-system-services"></a>Ik hebt niet voldoende rechten om systeemservices te starten

#### <a name="cause"></a>Oorzaak

Deze fout treedt op in twee situaties:
-   De gebruikersnaam en/of het wachtwoord zijn onjuist.
-   Het opgegeven gebruikersaccount beschikt niet over voldoende machtigingen om aan te melden als een service.

#### <a name="resolution"></a>Oplossing

Logboek-op-as-a-service-referenties aan de gebruikersaccount verlenen:

1. Ga naar **Start** > **Configuratiescherm** > **Systeembeheer** > **lokaal beveiligingsbeleid**  >  **Lokaal beleid** > **gebruiker Rights Management**.
2. Selecteer **aanmelden als service**.
3. In de **eigenschappen** dialoogvenster Voeg het gebruikersaccount.
4. Selecteer **toepassen**, en selecteer vervolgens **OK**.
5. Sluit alle vensters.

### <a name="a-database-has-an-out-of-date-status"></a>Een database heeft een 'Verouderde' status

#### <a name="cause"></a>Oorzaak

Synchroniseren van de SQL-gegevens (Preview) Hiermee verwijdert u de databases die offline zijn van de service voor een of meer (zoals geteld vanaf het moment dat de database offline ging) 45 dagen zijn. Als een database offline voor een of meer 45 dagen is en vervolgens weer online wordt gezet, wordt de status ervan is **verouderd**.

#### <a name="resolution"></a>Oplossing

U kunt voorkomen dat een **verouderd** status door ervoor te zorgen dat geen van de databases van 45 dagen of meer offline gaan.

Als de status van een database **verouderd**:

1. Verwijderen van de database is een **verouderd** status van de groep voor synchronisatie.
2. Toevoegen van de database weer aan bij de groep voor synchronisatie.

> [!WARNING]
> Alle wijzigingen die in deze database terwijl het offline was gaan verloren.

### <a name="a-sync-group-has-an-out-of-date-status"></a>Een groep voor synchronisatie met heeft een 'Verouderde' status

#### <a name="cause"></a>Oorzaak

Als een of meer wijzigingen niet toepassen voor de hele bewaarperiode van 45 dagen, kunt een groep voor synchronisatie verouderd zijn.

#### <a name="resolution"></a>Oplossing

Om te voorkomen dat een **verouderd** status voor een groep voor synchronisatie, bekijkt u de resultaten van uw synchronisatietaken in de geschiedenisviewer regelmatig. Onderzoeken en oplossen van eventuele wijzigingen die niet voldoen aan te passen.

Als de status van een groep voor synchronisatie **verouderd**, de groep voor synchronisatie verwijderen en vervolgens opnieuw maken.

### <a name="a-sync-group-cant-be-deleted-within-three-minutes-of-uninstalling-or-stopping-the-agent"></a>Een groep voor synchronisatie kan niet binnen drie minuten na het verwijderen of stoppen van de agent worden verwijderd

#### <a name="description-and-symptoms"></a>Beschrijving en symptomen

U kunt een groep voor synchronisatie niet verwijderen binnen drie minuten na het verwijderen of stoppen van de bijbehorende clientagent voor synchroniseren van de SQL-gegevens (Preview).

#### <a name="resolution"></a>Oplossing

1. Verwijderen van een groep voor synchronisatie, terwijl de agents gekoppeld sync online zijn (aanbevolen).
2. Als de agent offline is, maar is geïnstalleerd, brengt u het online op de lokale computer. Wacht totdat de status van de agent moet worden weergegeven als **Online** in de portal voor het synchroniseren van de SQL-gegevens (Preview). Verwijder vervolgens de groep voor synchronisatie.
3. Als de agent offline, is omdat deze is verwijderd:  
    a.  Verwijder de agent XML-bestand uit de installatiemap synchroniseren van de SQL-gegevens (Preview) als het bestand bestaat.  
    b.  Installeer de agent op de lokale computer (deze kan zijn hetzelfde of een andere computer). Dien vervolgens de agent-sleutel die wordt gegenereerd in de portal voor de agent die wordt weergegeven als offline.  
    c. Probeer te verwijderen van de groep voor synchronisatie.

### <a name="what-happens-when-i-restore-a-lost-or-corrupted-database"></a>Wat gebeurt er wanneer ik een verloren of beschadigde database herstellen?

Als u een verloren of beschadigde database vanaf een back-up herstellen, is er mogelijk een nonconvergence van gegevens in de synchronisatiegroepen waartoe de database behoort.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het synchroniseren van de SQL-gegevens (Preview):

-   [Synchronisatie van gegevens over meerdere cloud en on-premises databases met Azure SQL-gegevenssynchronisatie (Preview)](sql-database-sync-data.md)  
-   [Instellen van Azure SQL-gegevenssynchronisatie (Preview)](sql-database-get-started-sql-data-sync.md)  
-   [Aanbevolen procedures voor het synchroniseren van Azure SQL gegevens (Preview)](sql-database-best-practices-data-sync.md)  
-   [Monitor voor Azure SQL synchroniseren van gegevens (Preview) met OMS Log Analytics](sql-database-sync-monitor-oms.md)  
-   Voer de PowerShell-voorbeelden die laten hoe u zien voor het synchroniseren van de SQL-gegevens (Preview) te configureren:  
    -   [PowerShell gebruiken om te synchroniseren tussen meerdere Azure SQL-databases](scripts/sql-database-sync-data-between-sql-databases.md)  
    -   [PowerShell gebruiken om te synchroniseren tussen een Azure SQL Database en een lokale SQL Server-database.](scripts/sql-database-sync-data-between-azure-onprem.md)  
-   [Het synchroniseren van de SQL-gegevens (Preview) REST-API-documentatie downloaden](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_REST_API.pdf?raw=true)

Zie voor meer informatie over SQL-Database:

-   [Overzicht van de SQL-Database](sql-database-technical-overview.md)
-   [Database-levenscyclusbeheer](https://msdn.microsoft.com/library/jj907294.aspx)
