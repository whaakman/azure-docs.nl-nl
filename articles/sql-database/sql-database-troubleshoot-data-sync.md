---
title: Problemen met Azure SQL Data Sync oplossen | Microsoft Docs
description: Informatie over het oplossen van veelvoorkomende problemen met Azure SQL Data Sync.
services: sql-database
ms.service: sql-database
ms.subservice: data-movement
ms.custom: data sync
ms.devlang: ''
ms.topic: conceptual
author: allenwux
ms.author: xiwu
ms.reviewer: douglasl
manager: craigg
ms.date: 07/16/2018
ms.openlocfilehash: c08a76711a74f5b0fd119e579c6db54fc13ecfbb
ms.sourcegitcommit: db2cb1c4add355074c384f403c8d9fcd03d12b0c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2018
ms.locfileid: "51685817"
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

Zie voor het oplossen van problemen met de clientagent [Data Sync-Agent oplossen van problemen met](sql-database-data-sync-agent.md#agent-tshoot).

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

-   Overzicht - [gegevens synchroniseren tussen meerdere cloud en on-premises databases met Azure SQL Data Sync](sql-database-sync-data.md)
-   Data Sync instellen
    - In de portal - [zelfstudie: SQL Data Sync instellen om gegevens te synchroniseren tussen Azure SQL Database en SQL Server on-premises](sql-database-get-started-sql-data-sync.md)
    - Met PowerShell
        -  [PowerShell gebruiken om meerdere Azure SQL-databases te synchroniseren](scripts/sql-database-sync-data-between-sql-databases.md)
        -  [PowerShell gebruiken om te synchroniseren tussen een Azure SQL-database en een on-premises database](scripts/sql-database-sync-data-between-azure-onprem.md)
-   Gegevens synchroniseren Agent - [gegevens synchroniseren-Agent voor Azure SQL Data Sync](sql-database-data-sync-agent.md)
-   Aanbevolen procedures - [aanbevolen procedures voor Azure SQL Data Sync](sql-database-best-practices-data-sync.md)
-   Monitor - [SQL Data Sync met Log Analytics controleren](sql-database-sync-monitor-oms.md)
-   Bijwerken van het synchronisatieschema
    -   Met behulp van Transact-SQL - [automatiseren van de replicatie van schemawijzigingen in Azure SQL Data Sync](sql-database-update-sync-schema.md)
    -   Met PowerShell - [Gebruik PowerShell om bij te werken van het synchronisatieschema in een bestaande synchronisatiegroep](scripts/sql-database-sync-update-schema.md)

Zie voor meer informatie over SQL Database:

-   [Wat is de Azure SQL Database-service?](sql-database-technical-overview.md)
-   [Database Lifecycle Management (DLM)](https://msdn.microsoft.com/library/jj907294.aspx)
