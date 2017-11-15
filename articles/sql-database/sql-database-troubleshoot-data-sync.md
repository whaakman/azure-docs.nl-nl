---
title: Synchroniseren van Azure SQL-gegevens oplossen | Microsoft Docs
description: Informatie over het oplossen van veelvoorkomende problemen met het synchroniseren van Azure SQL-gegevens
services: sql-database
ms.date: 11/13/2017
ms.topic: article
ms.service: sql-database
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 8f1d2bf95c0e34f97e7075c062ea887f33f9785c
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2017
---
# <a name="troubleshoot-issues-with-sql-data-sync-preview"></a>Problemen oplossen met het synchroniseren van de SQL-gegevens (Preview)

Dit artikel wordt beschreven hoe u problemen met huidige die bekend zijn bij het synchroniseren van de SQL-gegevens (Preview)-team. Als er een oplossing voor een probleem is, is het hier opgegeven.

Zie voor een overzicht van de SQL-gegevenssynchronisatie [synchroniseren van gegevens via meerdere cloud en on-premises databases met Azure SQL-gegevenssynchronisatie (Preview)](sql-database-sync-data.md).

## <a name="troubleshooting-sync"></a>Het oplossen van synchronisatie

### <a name="sync-fails-in-the-portal-ui-for-on-premises-databases-associated-with-the-client-agent"></a>Synchronisatie mislukt in de gebruikersinterface van de portal voor on-premises-databases die zijn gekoppeld aan de clientagent

#### <a name="description-and-symptoms"></a>Beschrijving en symptomen

Synchronisatie mislukt bij het synchroniseren van de SQL-gegevens (Preview) gebruikersinterface portal voor on-premises-databases die zijn gekoppeld aan de agent. In de lokale computer waarop de agent wordt uitgevoerd, ziet u System.IO.IOException fouten in het gebeurtenislogboek, waarin staat dat de schijf onvoldoende ruimte is.

#### <a name="solution-or-workaround"></a>Oplossing of oplossing

Maak meer ruimte vrij op het station waarop de map % TEMP % zich bevindt.

### <a name="my-sync-group-is-stuck-in-the-processing-state"></a>Mijn groep voor synchronisatie is vastgelopen in de verwerkingsstatus

#### <a name="description-and-symptoms"></a>Beschrijving en symptomen

Een groep voor synchronisatie in het synchroniseren van de SQL-gegevens (Preview) is in de verwerkingsstatus gedurende lange tijd, niet gereageerd op de opdracht stoppen en de logboeken tonen geen nieuwe items.

#### <a name="causes"></a>Oorzaken

Een van de volgende voorwaarden kan resulteren in een groep voor synchronisatie wordt vastgelopen in de verwerkingsstatus.

-   **De clientagent is offline.** Zorg ervoor dat de clientagent online is en probeer het opnieuw.

-   **De clientagent is geïnstalleerd of ontbreekt.** Als de clientagent is geïnstalleerd of anderszins ontbreekt:

    1. Verwijder de agent XML-bestand uit de installatiemap voor het synchroniseren van de SQL-gegevens (Preview) als het bestand bestaat.

    2. Installeer de agent op dezelfde/een andere on-premises computer. Vervolgens dient u de sleutel van de agent uit de portal gegenereerd voor de agent die wordt weergegeven als offline.

-   **Het synchroniseren van de SQL-gegevens (Preview)-service is gestopt.**

    1. In de **Start** menu, zoekt u Services.

    2. Klik op Services in de zoekresultaten.

    3. Zoek de **synchroniseren van de SQL-gegevens (Preview)** service.

    4. Als de status van de service **gestopt**, met de rechtermuisknop op de servicenaam en selecteert u **Start** in de vervolgkeuzelijst.

#### <a name="solution-or-workaround"></a>Oplossing of oplossing

Als u niet het probleem wilt verhelpen, de status van de groep voor synchronisatie met opnieuw instellen met Microsoft ondersteuning. Om de status opnieuw instellen, maakt u een bericht op de [-forum Azure SQL Database](https://social.msdn.microsoft.com/Forums/azure/home?forum=ssdsgetstarted), maar ook uw abonnements-ID en de synchronisatie-ID voor de groep die moet worden ingesteld. Een support engineer van Microsoft wordt gereageerd op uw bericht en kunt u weten wanneer de status is opnieuw ingesteld.

### <a name="i-see-erroneous-data-in-my-tables"></a>Zie ik onjuiste gegevens in Mijn tabellen

#### <a name="description-and-symptoms"></a>Beschrijving en symptomen

Als de tabellen met dezelfde naam maar vanuit verschillende schema's in een database betrokken zijn gesynchroniseerd, ziet u onjuiste gegevens in deze tabellen na synchronisatie.

#### <a name="cause-and-fix"></a>Oorzaak en oplossing

De SQL-gegevenssynchronisatie (Preview) inrichtingsproces maakt gebruik van de dezelfde Traceringstabellen voor tabellen met dezelfde naam, maar in verschillende schema's. Als gevolg hiervan van beide tabellen wijzigingen in de tabel met dezelfde bijhouden en dit probleem veroorzaakt door onjuiste gegevens tijdens de synchronisatie wordt gewijzigd.

#### <a name="resolution-or-workaround"></a>Oplossing of oplossing

Zorg ervoor dat de namen van tabellen die zijn betrokken bij synchronisatie verschillend zijn, zelfs als ze tot verschillende schema's behoren.

### <a name="i-see-inconsistent-primary-key-data-after-a-successful-synchronization"></a>Zie ik inconsistente gegevens voor de primaire sleutel na een geslaagde synchronisatie

#### <a name="description-and-symptoms"></a>Beschrijving en symptomen

Na een synchronisatie die wordt gerapporteerd als geslaagd en het logboek bevat geen rijen mislukte of overgeslagen dat u merkt dat de primaire-sleutelgegevens niet consistent tussen de databases in de groep voor synchronisatie is.

#### <a name="cause"></a>Oorzaak

Dit gedrag is standaard. Wijzigingen in een primaire-sleutelkolom leiden tot inconsistente gegevens in de rijen waarbij de primaire sleutel is gewijzigd.

#### <a name="resolution-or-workaround"></a>Oplossing of oplossing

Om te voorkomen dat dit probleem, zorg dat er geen gegevens in een primaire sleutelkolom is gewijzigd.

U kunt dit probleem oplossen nadat deze is opgetreden, moet u de betreffende rij niet verwijderen uit alle eindpunten in de groep voor synchronisatie en de rij vervolgens opnieuw.

### <a name="i-see-a-significant-degradation-in-performance"></a>Zie ik een significante vermindering in prestaties

#### <a name="description-and-symptoms"></a>Beschrijving en symptomen

Prestaties van uw vermindert aanzienlijk, mogelijk tot het punt waar de gebruikersinterface voor het synchroniseren van gegevens ook kunnen niet worden gestart.

#### <a name="cause"></a>Oorzaak

De meest waarschijnlijke oorzaak is een lus synchronisatie. Een synchronisatie-lus treedt op wanneer een synchronisatie door synchronisatie groep A triggers een synchroniseren door synchronisatie groep B, die op zijn beurt een synchronisatie met synchronisatie groep a activeert De werkelijke situatie is mogelijk meer complexe, met betrekking tot meer dan twee synchronisatiegroepen in de lus, maar de belangrijke factor is dat er een circulaire activatie van synchronisaties die worden veroorzaakt door synchronisatiegroepen elkaar overlappen.

#### <a name="resolution-or-workaround"></a>Oplossing of oplossing

De beste oplossing wordt voorkomen. Zorg ervoor dat er geen kringverwijzingen in de synchronisatiegroepen. Een rij die is gesynchroniseed met één synchronisatie-groep kan niet worden gesynchroniseerd door een andere groep voor synchronisatie.

### <a name="i-got-an-error-message-that-said-cannot-insert-the-value-null-into-the-column-column-column-does-not-allow-nulls-what-does-this-mean-and-how-can-i-fix-the-error"></a>Ik heb een foutbericht weergegeven dat gezegd ' kan de waarde NULL niet invoegen in de kolom *kolom*. Kolom mag geen null-waarden." Wat betekent dit en hoe kan ik de fout oplossen? 
Dit foutbericht geeft aan dat een van de twee volgende problemen:
1.  Mogelijk zijn er geen tabel zonder een primaire sleutel. U kunt dit probleem oplossen door moet u een primaire sleutel toevoegen aan alle tabellen die u wilt synchroniseren.
2.  Mogelijk zijn er een WHERE-component in de instructie CREATE INDEX. Deze voorwaarde verwerkt synchronisatie niet. U kunt dit probleem oplossen door de component WHERE verwijderen of wijzigingen handmatig aanbrengen voor alle databases. 
 
### <a name="how-does-data-sync-handle-circular-references-that-is-when-the-same-data-is-synced-in-multiple-sync-groups-and-keeps-changing-as-a-result"></a>Hoe wordt gegevenssynchronisatie kringverwijzingen verwerkt? Dat wil zeggen, wanneer dezelfde gegevens in meerdere synchronisatiegroepen is gesynchroniseerd en als gevolg hiervan blijft wijzigen?
Synchroniseren van gegevens kunnen kringverwijzingen niet worden verwerkt. Zorg ervoor dat ze kunnen worden vermeden. 

## <a name="troubleshooting-the-client-agent"></a>De clientagent voor het oplossen van problemen

### <a name="install-uninstall-or-repair-fails"></a>Installeren, verwijderen of herstellen mislukt

#### <a name="cause"></a>Oorzaak

Er zijn veel mogelijke oorzaken voor de fout. Om te bepalen van de oorzaak van deze fout, moet u kijken naar de logboeken.

#### <a name="solution-or-workaround"></a>Oplossing of oplossing

Ga voor de oorzaak van het probleem optrad, moet u genereren en bekijk de logboeken van Windows Installer. U kunt logboekregistratie vanaf de opdrachtregel. Bijvoorbeeld, wordt ervan uitgegaan dat het gedownloade bestand voor AgentServiceSetup.msi LocalAgentHost.msi. Genereren en bekijk de logboekbestanden met de volgende opdrachtregels:

-   Voor installaties:`msiexec.exe /i SQLDataSyncAgent-Preview-ENU.msi /l\*v LocalAgentSetup.InstallLog`

-   Wordt verwijderd voor:`msiexec.exe /x SQLDataSyncAgent-se-ENU.msi /l\*v LocalAgentSetup.InstallLog`

U kunt ook inschakelen van logboekregistratie voor alle installaties uitgevoerd door Windows Installer. De Microsoft Knowledge Base-artikel [het inschakelen van logboekregistratie voor Windows Installer](https://support.microsoft.com/help/223300/how-to-enable-windows-installer-logging) biedt een oplossing voor één muisklik inschakelen van logboekregistratie voor Windows Installer. Het bevat ook de locatie van deze logboeken.

### <a name="my-client-agent-doesnt-work"></a>Mijn clientagent werkt niet

#### <a name="description-and-symptoms"></a>Beschrijving en symptomen

U krijgt de volgende foutberichten weergegeven wanneer u probeert de clientagent te gebruiken.

"Synchronisatie is mislukt met uitzondering er een fout is opgetreden tijdens het deserialiseren van de parameter www.microsoft.com/.../05:GetBatchInfoResult. Zie InnerException voor meer informatie.

"De binnenste Uitzonderingsbericht: Type 'Microsoft.Synchronization.ChangeBatch' is een ongeldig verzameltype aangezien het geen standaardconstructor."

#### <a name="cause"></a>Oorzaak

Deze fout is een probleem met het synchroniseren van de SQL-gegevens (Preview).

De meest waarschijnlijke oorzaak van dit probleem is:

-   U kunt Windows 8 Developer Preview worden uitgevoerd of

-   U hebt .NET 4.5 geïnstalleerd.

#### <a name="solution-or-workaround"></a>Oplossing of oplossing

Zorg ervoor dat u de clientagent installeren op een computer waarop Windows 8 Developer Preview niet wordt uitgevoerd en .NET Framework 4.5 is niet geïnstalleerd.

### <a name="my-client-agent-doesnt-work-after-i-cancel-the-uninstall"></a>Mijn clientagent werkt niet wanneer ik de verwijdering annuleren

#### <a name="description-and-symptoms"></a>Beschrijving en symptomen

De clientagent werkt niet, zelfs als u de installatie geannuleerd.

#### <a name="cause"></a>Oorzaak

Dit probleem treedt op omdat de clientagent synchroniseren van de SQL-gegevens (Preview) niet referenties opgeslagen worden.

#### <a name="solution-or-workaround"></a>Oplossing of oplossing

Er zijn twee oplossingen proberen:

-   Gebruik services.msc eerst uw referenties opnieuw invoeren voor de clientagent.

-   Ten tweede installatie van deze clientagent en installeer een nieuwe. Download en installeer de nieuwste clientagent van [Downloadcentrum](http://go.microsoft.com/fwlink/?linkid=221479).

### <a name="my-database-isnt-listed-in-the-agent-dropdown"></a>Mijn database wordt niet vermeld in de vervolgkeuzelijst voor agent

#### <a name="description-and-symptoms"></a>Beschrijving en symptomen

Wanneer u probeert een bestaande SQL Server-database toevoegen aan een groep voor synchronisatie, wordt de database wordt niet vermeld in de vervolgkeuzelijst.

#### <a name="cause"></a>Oorzaak

Er zijn verschillende mogelijke oorzaken voor dit probleem:

-   De agent en sync clientgroep zijn in verschillende data centers.

-   Lijst met databases van de clientagent is niet actueel.

#### <a name="solution"></a>Oplossing

De oplossing is afhankelijk van de oorzaak.

**De agent en sync clientgroep zich in verschillende data centers**

U moet de clientagent voor zowel de groep voor synchronisatie hebben in hetzelfde Datacenter. U kunt deze configuratie instellen op een van de volgende dingen:

-   Maak een nieuwe agent in hetzelfde Datacenter als de groep voor synchronisatie. Registreer de database met die agent.

-   Verwijder de huidige groep voor synchronisatie. Maak deze opnieuw in hetzelfde Datacenter als de agent.

**Lijst met databases van de clientagent is niet actueel**

Stop en start de client agent-service opnieuw op.
De lokale agent downloadt de lijst met de bijbehorende databases alleen op de eerste verzending van de sleutel van de agent en niet op latere agent sleutel inzendingen. Dus worden databases die zijn geregistreerd tijdens het verplaatsen van een agent niet weergegeven op het oorspronkelijke agentexemplaar.

### <a name="client-agent-doesnt-start-error-1069"></a>Agent-client niet wordt gestart (fout 1069)

#### <a name="description-and-symptoms"></a>Beschrijving en symptomen

U ontdekken dat de agent wordt niet uitgevoerd op een computer waarop SQL Server wordt gehost. Als u probeert de agent handmatig wordt gestart, krijgt u een foutbericht met het foutbericht ' fout 1069: de service is niet gestart vanwege een aanmeldingsfout. "

![Gegevens synchroniseren 1069 foutdialoogvenster](media/sql-database-troubleshoot-data-sync/sync-error-1069.png)

#### <a name="cause"></a>Oorzaak

Een mogelijke oorzaak van dit probleem is dat het wachtwoord op de lokale server is gewijzigd sinds u de agent gemaakt en heeft deze een aanmeldingswachtwoord gekregen.

#### <a name="solution-or-workaround"></a>Oplossing of oplossing

Wachtwoord van de agent bijwerken naar uw huidige serverwachtwoord.

1. Zoek de clientagent voor synchroniseren van de SQL-gegevens (Preview) Preview service.

    a. Klik op **Start**.

    b. Typ "services.msc" in het zoekvak.

    c. Klik in de zoekresultaten op 'Services'.

    d. In de **Services** venster, blader naar de vermelding voor **synchroniseren van de SQL-gegevens (Preview) Agent Preview**.

2. Met de rechtermuisknop op de vermelding en selecteer **stoppen**.

3. Met de rechtermuisknop op de vermelding en klik vervolgens op **eigenschappen**.

4. In de **synchroniseren van de SQL-gegevens (Preview) Preview Agenteigenschappen** venster, klikt u op de **aanmelden** tabblad.

5. Voer uw wachtwoord in het tekstvak wachtwoord.

6. Bevestig het wachtwoord in het tekstvak Wachtwoord bevestigen.

7. Klik op **Apply** (Toepassen) en op **OK**.

8. In de **Services** venster met de rechtermuisknop op de **synchroniseren van de SQL-gegevens (Preview) Agent Preview** service en klik vervolgens op **Start**.

9. Sluit de **Services** venster.

### <a name="i-cannot-submit-the-agent-key"></a>Ik heb de sleutel van de Agent kan niet worden verzonden

#### <a name="description-and-symptoms"></a>Beschrijving en symptomen

Nadat u hebt gemaakt of opnieuw maken van een sleutel voor een agent, u probeert te verzenden die sleutel door de toepassing SqlAzureDataSyncAgent, maar de verzending niet kan worden voltooid.

![Fout bij het dialoogvenster synchronisatie - sleutel van de agent kan niet worden verzonden](media/sql-database-troubleshoot-data-sync/sync-error-cant-submit-agent-key.png)

Voordat u doorgaat, moet dat een storing van een van de volgende voorwaarden is niet de oorzaak van het probleem.

-   Het synchroniseren van de SQL-gegevens (Preview) Windows-service wordt uitgevoerd.

-   Het serviceaccount voor het synchroniseren van de SQL-gegevens (Preview) Preview Windows-service heeft toegang tot het netwerk.

-   De clientagent is de naamgevingsservice contact kunnen maken. Controleer of de volgende registersleutel de waarde 'https://locator.sync.azure.com/LocatorServiceApi.svc heeft'

    -   Op een x86 computer:`HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Microsoft\\SQL Azure Data Sync\\LOCATORSVCURI`

    -   Op een x64 computer:`HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Wow6432Node\\Microsoft\\SQL Azure Data Sync\\LOCATORSVCURI`

#### <a name="cause"></a>Oorzaak

De sleutel van de agent is een unieke identificatie voor elke lokale agent. De sleutel moet voldoen aan twee voorwaarden te werken:

-   De sleutel van de client-agent op de server synchroniseren van de SQL-gegevens (Preview) en de lokale computer moet identiek zijn.

-   De sleutel van de client-agent kan slechts één keer worden gebruikt.

#### <a name="solution-or-workaround"></a>Oplossing of oplossing

Als de agent niet werkt, is dit omdat een of beide volgende voorwaarden wordt niet voldaan. Ophalen van de agent opnieuw werken:

1. Een nieuwe sleutel te genereren.

2. De nieuwe sleutel van toepassing op de agent.

Als u wilt toepassen de nieuwe sleutel met de agent, het volgende doen:

1. Gebruik Verkenner om te navigeren naar de installatiemap van de agent. De standaardinstallatiemap is `c:\\program files (x86)\\microsoft sql data sync`.

2. Dubbelklik op de `bin` submap.

3. Start de `SqlAzureDataSyncAgent` toepassing.

4. Klik op **indienen Agentcode**.

5. De sleutel van het Klembord plakken in het aanwezige vak.

6. Klik op **OK**.

7. Sluit het programma.

### <a name="client-agent-cannot-be-deleted-from-the-portal-if-its-associated-on-premises-database-is-unreachable"></a>Clientagent kan niet worden verwijderd uit de portal als de bijbehorende lokale-database niet bereikbaar is

#### <a name="description-and-symptoms"></a>Beschrijving en symptomen

Als een lokaal eindpunt (dat wil zeggen, een database) dat is geregistreerd bij een clientagent synchroniseren van de SQL-gegevens (Preview) wordt niet bereikbaar is, kan de clientagent kan niet worden verwijderd.

#### <a name="cause"></a>Oorzaak

De lokale agent kan niet worden verwijderd omdat de database niet bereikbaar is nog steeds geregistreerd met de agent. Wanneer u probeert te verwijderen van de agent, wordt de verwijdering probeert te bereiken van de database mislukt.

#### <a name="resolution-or-workaround"></a>Oplossing of oplossing

Gebruik 'geforceerd verwijderen' verwijderen van de database niet bereikbaar.

> [!NOTE]
> Als u na een 'force verwijderen' sync metagegevenstabellen deprovisioningutil.exe gebruiken om op te schonen ze blijven.

### <a name="local-sync-agent-app-is-unable-to-connect-to-the-local-sync-service"></a>Lokale Sync-Agent-app is geen verbinding maken met de lokale sync-service

#### <a name="solution-or-workaround"></a>Oplossing of oplossing

Voer de volgende stappen uit:

1. De app af te sluiten.

2. Open het deelvenster Component Services.

    a. Typ in het zoekvak op de taakbalk "services.msc".

    b. Dubbelklik op 'Services' in de zoekresultaten.

3. Stop en start de service 'SQL gegevens synchroniseren (Preview) Preview' opnieuw.

4. De app opnieuw starten.

## <a name="troubleshooting-setup-and-maintenance"></a>Het oplossen van de installatie en het onderhoud

### <a name="i-get-a-disk-out-of-space-message"></a>Er verschijnt een bericht 'geen ruimte meer-schijf'

#### <a name="cause"></a>Oorzaak

Het bericht 'geen ruimte meer-schijf' kan worden weergegeven wanneer achterblijven voor bestanden die moeten worden verwijderd. Dit probleem kan worden veroorzaakt door antivirussoftware of omdat er bestanden zijn geopend wanneer de delete-bewerkingen worden uitgevoerd.

#### <a name="solution"></a>Oplossing

De oplossing is handmatig verwijderen van het synchroniseren van bestanden onder `%temp%` (`del \*sync\* /s`), en vervolgens verwijdert u ook de submappen.

> [!IMPORTANT]
> Wacht totdat de synchronisatie is voltooid voordat u bestanden verwijdert.

### <a name="i-cannot-delete-my-sync-group"></a>Ik kan mijn groep voor synchronisatie niet verwijderen.

#### <a name="description-and-symptoms"></a>Beschrijving en symptomen

U niet in uw poging om een synchronisatie-groep te verwijderen.

#### <a name="causes"></a>Oorzaken

De volgende bewerkingen kan leiden tot een fout bij verwijderen van een groep voor synchronisatie.

-   De clientagent is offline.

-   De clientagent is geïnstalleerd of ontbreekt. 

-   Een database is offline. 

-   De groep voor synchronisatie is ingericht of synchroniseert. 

#### <a name="solutions"></a>Oplossingen

De fout wilt verwijderen van een groep voor synchronisatie, controleert u de volgende handelingen oplossen:

-   Zorg ervoor dat de clientagent online is en probeer het opnieuw.

-   Als de clientagent is geïnstalleerd of anderszins ontbreekt:

    a. Verwijder de agent XML-bestand uit de installatiemap voor het synchroniseren van de SQL-gegevens (Preview) als het bestand bestaat.

    b. De agent installeren op dezelfde/een andere on-premises computer, de sleutel van de agent uit de portal gegenereerd voor de agent die wordt weergegeven als offline verzenden.

-   **Het synchroniseren van de SQL-gegevens (Preview)-service is gestopt.**

    a. In de **Start** menu, zoekt u Services.

    b. Klik op Services in de zoekresultaten.

    c. Zoek de **synchroniseren van de SQL-gegevens (Preview) Preview** service.

    d. Als de status van de service **gestopt**, met de rechtermuisknop op de servicenaam en selecteert u **Start** in de vervolgkeuzelijst.

-   Controleer uw SQL-Databases en SQL Server-databases wilt controleren of dat alle online zijn.

-   Wacht totdat het proces provisioning of synchroniseren is voltooid. Probeer de groep voor synchronisatie te verwijderen.

### <a name="i-cant-unregister-an-on-premises-sql-server-database"></a>Ik kan geen Hef de registratie van een lokale SQL Server-database.

#### <a name="cause"></a>Oorzaak

U wilt waarschijnlijk Hef de registratie van een database die al is verwijderd.

#### <a name="solution-or-workaround"></a>Oplossing of oplossing

Selecteer de database om de registratie van een lokale SQL Server-database, en klikt u op **geforceerd verwijderen**.

Als deze bewerking is mislukt voor de database verwijderen uit de groep voor synchronisatie, kunt u het volgende doen:

1. Stop en start de client-agent host-service opnieuw op.

    a. Klik op het menu Start.

    b. Voer *services.msc* in het zoekvak.

    c. Dubbelklik in de programma's sectie van het deelvenster met resultaten op **Services**.

    d. Zoeken en met de rechtermuisknop op de service **synchroniseren van de SQL-gegevens (Preview)**.

    e. Als de service wordt uitgevoerd, stoppen.

    f. Met de rechtermuisknop en selecteer **Start**.

    g. Controleer of de database is niet meer geregistreerd. Als het is niet langer geregistreerd, bent u klaar. Ga anders verder met de volgende stap.

2. Open de app in client-agent (SqlAzureDataSyncAgent).

3. Klik op **referenties bewerken** en geef de referenties voor de database dus bereikbaar is.

4. Doorgaan met verwijderen van de registratie.

### <a name="i-do-not-have-sufficient-privileges-to-start-system-services"></a>Ik heb niet voldoende machtigingen om het starten van systeemservices

#### <a name="cause"></a>Oorzaak

Deze fout treedt op in twee situaties:

-   De gebruikersnaam en/of het wachtwoord zijn onjuist.

-   Het opgegeven gebruikersaccount heeft geen voldoende bevoegdheden als een service aan te melden.

#### <a name="solution-or-workaround"></a>Oplossing of oplossing

Verleen logboek-op-as-a-service-referenties aan de gebruikersaccount.

1. Navigeer naar **Start | Het Configuratiescherm | Systeembeheer |  Lokaal beveiligingsbeleid | Lokaal beleid | Gebruiker Rights Management**.

2. Zoek en klik op de **aanmelden als service** vermelding.

3. Voeg het gebruikersaccount in de **aanmelden als service** **eigenschappen** in het dialoogvenster.

4. Klik op **toepassen** vervolgens **OK**.

5. Sluit de vensters.

### <a name="a-database-has-an-out-of-date-status"></a>Een database heeft een 'Verouderde' status

#### <a name="cause"></a>Oorzaak

Synchroniseren van de SQL-gegevens (Preview) Hiermee verwijdert u de databases die offline van 45 dagen of meer zijn (zoals geteld vanaf het moment dat de database offline ging) van de service. Als een database offline voor een of meer 45 dagen is en vervolgens weer online wordt gezet, is de status ingesteld op 'Verouderde'.

#### <a name="solution-or-workaround"></a>Oplossing of oplossing

U kunt de status van een 'Verouderde' voorkomen door ervoor te zorgen dat geen van uw databases offline van 45 dagen of meer gaat.

Als de status van een database 'Verouderde', moet u het volgende doen:

1. De 'Verouderde' database verwijderen uit de groep voor synchronisatie.

2. Toevoegen van de database weer aan bij de groep voor synchronisatie.

> [!WARNING]
> Alle wijzigingen die in deze database terwijl het offline was gaan verloren.

### <a name="a-sync-group-has-an-out-of-date-status"></a>Een groep voor synchronisatie met heeft een 'Verouderde' status

#### <a name="cause"></a>Oorzaak

Als een of meer wijzigingen niet toepassen voor de hele bewaarperiode van 45 dagen, kunt een groep voor synchronisatie verouderd zijn.

#### <a name="solution-or-workaround"></a>Oplossing of oplossing

Om te voorkomen dat de status van een 'Verouderde', bekijkt u de resultaten van uw synchronisatietaken in de geschiedenisviewer op gezette tijden en onderzoeken en oplossen van eventuele wijzigingen die niet voldoen aan te passen.

Als een groep voor synchronisatie met de status 'Verouderde', moet u de groep voor synchronisatie en maak deze opnieuw.

### <a name="a-sync-group-cannot-be-deleted-within-three-minutes-of-uninstallingstopping-the-agent"></a>Een groep voor synchronisatie kan niet worden verwijderd binnen drie minuten van de Agent verwijderen/stoppen

#### <a name="description-and-symptoms"></a>Beschrijving en symptomen

U bent geen kunt verwijderen van een groep voor synchronisatie binnen drie minuten van de bijbehorende clientagent voor synchroniseren van de SQL-gegevens (Preview) verwijderen/gestopt.

#### <a name="resolution-or-workaround"></a>Oplossing of oplossing

1. Verwijderen van een groep voor synchronisatie, terwijl de agents gekoppeld sync online zijn (aanbevolen).

2. Als de agent is geïnstalleerd, maar offline is, brengt u het online op de lokale computer. Vervolgens wacht u totdat de status van de agent moet worden weergegeven als online in synchroniseren van de SQL-gegevens (Preview)-portal en verwijder de groep voor synchronisatie.

3. Als de agent offline is, omdat deze is verwijderd, kunt u het volgende doen: Probeer vervolgens de groep voor synchronisatie te verwijderen.

    a.  Verwijder de agent XML-bestand uit de installatiemap voor het synchroniseren van de SQL-gegevens (Preview) als het bestand bestaat.

    b.  Installeer de agent op dezelfde of een andere lokale computer, de sleutel van de agent uit de portal gegenereerd voor de agent die wordt weergegeven als offline indienen.

### <a name="what-happens-when-i-restore-a-lost-or-corrupted-database"></a>Wat gebeurt er wanneer ik een verloren of beschadigde database herstellen?

Als u een verloren of beschadigde database vanaf een back-up herstellen, kunnen er non-convergentie van gegevens in de synchronisatie groep(en) waartoe de database behoort.

## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over het synchroniseren van de SQL-gegevens:

-   [Synchronisatie van gegevens over meerdere cloud en on-premises databases met synchroniseren van Azure SQL-gegevens](sql-database-sync-data.md)
-   [Synchroniseren van Azure SQL-gegevens instellen](sql-database-get-started-sql-data-sync.md)
-   [Aanbevolen procedures voor het synchroniseren van Azure SQL-gegevens](sql-database-best-practices-data-sync.md)
-   [Monitor Azure SQL-gegevens synchroniseren met OMS Log Analytics](sql-database-sync-monitor-oms.md)

-   Voer de PowerShell-voorbeelden die laten hoe u zien voor het synchroniseren van de SQL-gegevens configureren:
    -   [PowerShell gebruiken om te synchroniseren tussen meerdere Azure SQL-databases](scripts/sql-database-sync-data-between-sql-databases.md)
    -   [PowerShell gebruiken om te synchroniseren tussen een Azure SQL Database en een lokale SQL Server-database.](scripts/sql-database-sync-data-between-azure-onprem.md)

-   [De SQL-gegevens synchroniseren REST-API-documentatie downloaden](https://github.com/Microsoft/sql-server-samples/raw/master/samples/features/sql-data-sync/Data_Sync_Preview_REST_API.pdf?raw=true)

Zie voor meer informatie over SQL-Database:

-   [Overzicht van de SQL-Database](sql-database-technical-overview.md)
-   [Database-levenscyclusbeheer](https://msdn.microsoft.com/library/jj907294.aspx)
