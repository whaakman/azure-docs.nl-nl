---
title: Gegevens verplaatsen tussen uitgeschaalde clouddatabases | Microsoft Docs
description: Wordt uitgelegd hoe u shards bewerken en verplaatsen van gegevens via een zelf-hostend service gebruik van elastic database API's.
services: sql-database
ms.service: sql-database
ms.subservice: scale-out
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: ''
manager: craigg
ms.date: 12/04/2018
ms.openlocfilehash: 195b513d7cc878045449ed137a2ea72f291a9f6e
ms.sourcegitcommit: 3f4ffc7477cff56a078c9640043836768f212a06
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/04/2019
ms.locfileid: "57308148"
---
# <a name="moving-data-between-scaled-out-cloud-databases"></a>Gegevens verplaatsen tussen uitgeschaalde clouddatabases

Als u een Software als een Service-ontwikkelaar bent en plotseling enorme vraag wanneer er in uw app, moet u de groei. Zodat toevoegen u meer databases (shards). Hoe opnieuw u de gegevens naar de nieuwe databases te distribueren zonder te onderbreken van de integriteit van gegevens? Gebruik de **hulpprogramma voor splitsen en samenvoegen** om gegevens te verplaatsen van beperkte databases naar de nieuwe databases.  

Het hulpprogramma voor splitsen en samenvoegen wordt uitgevoerd als een Azure-web-service. Een beheerder of ontwikkelaar maakt gebruik van het hulpprogramma shardlets (gegevens uit een shard) verplaatsen tussen verschillende databases (shards). Het hulpprogramma maakt gebruik van shard-Toewijzingsbeheer onderhouden van de database-service-metagegevens, en ervoor zorgen dat consistente toewijzingen.

![Overzicht][1]

## <a name="download"></a>Downloaden

[Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge/)

## <a name="documentation"></a>Documentatie

1. [Zelfstudie voor elastische database splitsen en samenvoegen-hulpprogramma](sql-database-elastic-scale-configure-deploy-split-and-merge.md)
2. [Beveiligingsconfiguratie splitsen en samenvoegen](sql-database-elastic-scale-split-merge-security-configuration.md)
3. [Aandachtspunten voor de beveiliging van splitsen en samenvoegen](sql-database-elastic-scale-split-merge-security-configuration.md)
4. [Shard-toewijzingsbeheer](sql-database-elastic-scale-shard-map-management.md)
5. [Bestaande databases migreren voor uitschaling](sql-database-elastic-convert-to-use-elastic-tools.md)
6. [Hulpmiddelen voor elastic database](sql-database-elastic-scale-introduction.md)
7. [Woordenlijst voor elastische Database-hulpprogramma 's](sql-database-elastic-scale-glossary.md)

## <a name="why-use-the-split-merge-tool"></a>Waarom gebruikt u het hulpprogramma voor splitsen en samenvoegen

- **Flexibiliteit**

  Toepassingen moeten flexibel uitbreiden buiten de grenzen van een individuele Azure SQL DB-database. Het hulpprogramma gebruiken om gegevens te verplaatsen naar behoefte nieuwe databases behoudt integriteit.

- **Gesplitste te laten groeien**

  Als u wilt vergroten de totale capaciteit voor het afhandelen van bij een explosieve groei, extra capaciteit maken met de gegevens voor sharding en door te distribueren voor incrementeel meer databases, totdat de capaciteit moet worden voldaan. Dit is een goed voorbeeld van de **splitsen** functie.

- **Als u wilt verkleinen samenvoegen**

  Capaciteit moet verkleinen door de seizoensgebonden aard van een bedrijf. Het hulpprogramma kunt u omlaag schalen naar minder schaaleenheden wanneer bedrijven wordt vertraagd. De functie 'samenvoegen' in de Elastic Scale splitsen en samenvoegen-Service bevat informatie over deze vereiste.

- **Hotspots beheren door te shardlets verplaatsen**

  De toewijzing van shardlets naar shards kan met meerdere tenants per database leiden tot capaciteit knelpunten op sommige shards. Dit is vereist voor opnieuw toewijzen van shardlets of bezet shardlets verplaatsen naar nieuwe of minder gebruikte shards.

## <a name="concepts--key-features"></a>Concepten en belangrijke functies

- **Klant gehoste services**

  De splitsen en samenvoegen wordt geleverd als een klant-gehoste-service. U moet implementeren en de service hosten in uw Microsoft Azure-abonnement. Het pakket dat u vanuit NuGet downloaden bevat een configuratiesjabloon om uit te voeren met de gegevens voor uw specifieke implementatie. Zie de [splitsen en samenvoegen zelfstudie](sql-database-elastic-scale-configure-deploy-split-and-merge.md) voor meer informatie. Omdat de service wordt uitgevoerd in uw Azure-abonnement, kunt u deze kunt beheren en configureren van de meeste beveiligingsaspecten van de service. De standaardsjabloon bevat de opties voor het configureren van SSL, op basis van certificaten voor clientverificatie, versleuteling voor opgeslagen referenties, DoS beveiligen en IP-beperkingen. U vindt meer informatie over de beveiligingsaspecten in het volgende document [beveiligingsconfiguratie splitsen en samenvoegen](sql-database-elastic-scale-split-merge-security-configuration.md).

  De standaardwaarde geïmplementeerd met een Webrol en een werknemer de service wordt uitgevoerd. Gebruik de A1-VM-grootte in Azure Cloud Services. Terwijl u kunt deze instellingen niet wijzigen bij het implementeren van het pakket, kunt u ze kunt wijzigen na een geslaagde implementatie in de actieve cloudservice (via Azure portal). Houd er rekening mee dat de werkrol moet niet worden geconfigureerd voor meer dan één exemplaar voor technische redenen.

- **Integratie van de shard-kaart**

  De service voor splitsen en samenvoegen communiceert met de shard-toewijzing van de toepassing. Wanneer u de service voor splitsen en samenvoegen kunnen worden gesplitst of samengevoegd of shardlets verplaatsen tussen shards, wordt de service automatisch de shard-toewijzing bijgewerkt. Om dit te doen, is de service maakt verbinding met de database voor shard map manager van de toepassing en -bereiken en -toewijzingen onderhoudt als splitsen of samenvoegen/verplaatsen aanvragen uitgevoerd. Dit zorgt ervoor dat de shard-toewijzing een actuele weergave altijd weergegeven wanneer bewerkingen voor splitsen en samenvoegen gaat. Splitsen, worden samenvoegen en shardlet gegevensverplaatsingsbewerkingen geïmplementeerd door een reeks shardlets uit de bron-shard verplaatsen naar de doel-shard. Tijdens de verplaatsing van shardlet de shardlets onderworpen aan de huidige batch zijn gemarkeerd als offline in de shard-toewijzing en zijn niet beschikbaar voor gegevensafhankelijke routering verbindingen met behulp van de **OpenConnectionForKey** API.

- **Consistente shardlet-verbindingen**

  Bij het verplaatsen van gegevens wordt gestart voor een nieuwe batch shardlets, beschikbaar elke shard-toewijzing gegevensafhankelijke routering verbindingen de shard opslaan van de shardlet worden afgesloten en volgende verbindingen van de shard-toewijzing die API 's om de shardlets worden geblokkeerd terwijl de verplaatsing van gegevens is wordt uitgevoerd om te voorkomen dat inconsistenties. Verbindingen met andere shardlets op de dezelfde shard wordt ook ophalen verwijderd, maar is wel mogelijk opnieuw onmiddellijk op opnieuw proberen. Zodra de batch wordt verplaatst, wordt de shardlets online opnieuw uit voor de doel-shard worden gemarkeerd en wordt de brongegevens is verwijderd uit de bron-shard. De service uitvalt tijdens de volgende stappen voor elke batch totdat alle shardlets zijn verplaatst. Dit zal leiden tot meerdere verbinding kill-bewerkingen in de loop van de voltooiingsbewerking splitsen of samenvoegen/verplaatsen.  

- **Shardlet beschikbaarheid beheren**

  Beperken van de verbinding aan de huidige batch van shardlets zoals hierboven wordt beschreven doden beperkt het bereik van niet-beschikbaarheid tot één batch van shardlets tegelijk. Dit heeft de voorkeur boven een benadering waarbij de volledige shard offline voor alle bijbehorende shardlets in de loop van een bewerking splitsen of samenvoegen zouden blijven. De grootte van een batch, gedefinieerd als het aantal verschillende shardlets om te gaan op een tijdstip, is een configuratieparameter. Deze kan worden gedefinieerd voor elke bewerking splitsen en samenvoegen, afhankelijk van de behoeften van de beschikbaarheid en prestaties van de toepassing. Houd er rekening mee dat het bereik dat wordt vergrendeld in de shard-toewijzing mogelijk groter is dan de opgegeven batch. Dit komt doordat de service de grootte van het bereik kiest dat het werkelijke aantal sharding sleutelwaarden in de gegevens ongeveer overeenkomt met de batchgrootte. Dit is belangrijk te onthouden is met name voor spaarzaam ingevuld sharding-sleutels.

- **Opslag van Metagegvens**

  De service voor splitsen en samenvoegen gebruikt een database te onderhouden van de status ervan, en om Logboeken te bewaren tijdens de verwerking van aanvragen. De gebruiker deze database maakt in het abonnement en biedt de verbindingsreeks voor het in het configuratiebestand voor de service-implementatie. Beheerders van de organisatie van de gebruiker kunnen ook verbinding maken met deze database om voortgang van de aanvraag te beoordelen en gedetailleerde informatie over mogelijke problemen te onderzoeken.

- **Sharding-awareness**

  De service voor splitsen en samenvoegen wordt onderscheid gemaakt tussen tussen (1) shard tabellen, (2) verwijzingstabellen en (3) normale tabellen. De semantiek van een bewerking splitsen of samenvoegen/verplaatsen afhankelijk zijn van het type van de tabel die wordt gebruikt en worden als volgt gedefinieerd:

  - **Shard-tabellen**

    Operations verplaatsen shardlets verplaatsen van bron naar doel shard splitsen en samenvoegen. Na voltooiing van de totale aanvraag zijn de shardlets niet meer aanwezig zijn op de bron. Houd er rekening mee dat de doeltabellen moeten aanwezig zijn op de doel-shard en mag geen gegevens in het doelbereik voordat de verwerking van de bewerking.

  - **Naslaginformatie over tabellen**

    Voor de splitsing verwijzingstabellen samen te voegen en verplaats bewerkingen kunt u de gegevens van de bron kopiëren naar de doel-shard. Merk echter op dat er geen wijzigingen doorgevoerd op de doel-shard voor een bepaalde tabel als een rij al aanwezig in deze tabel op de doelcomputer is. De tabel is leeg zijn voor elke bewerking verwijzing tabel kopiëren naar verwerkt.

  - **Andere tabellen**

    Andere tabellen kunnen worden gebruikt op de bron- of het doel van een bewerking voor splitsen en samenvoegen. De service voor splitsen en samenvoegen negeert deze tabellen voor verplaatsing van gegevens of te kopiëren. Merk echter op dat ze deze bewerkingen in het geval van beperkingen kunnen verstoren.

    De informatie van verwijzing versus shard tabellen wordt verstrekt door de `SchemaInfo` API's op de shard-toewijzing. Het volgende voorbeeld wordt het gebruik van deze API's op een object opgegeven shard map manager:

    ```c#
    // Create the schema annotations
    SchemaInfo schemaInfo = new SchemaInfo();

    // Reference tables
    schemaInfo.Add(new ReferenceTableInfo("dbo", "region"));
    schemaInfo.Add(new ReferenceTableInfo("dbo", "nation"));

    // Sharded tables
    schemaInfo.Add(new ShardedTableInfo("dbo", "customer", "C_CUSTKEY"));
    schemaInfo.Add(new ShardedTableInfo("dbo", "orders", "O_CUSTKEY"));
    // Publish
    smm.GetSchemaInfoCollection().Add(Configuration.ShardMapName, schemaInfo);
    ```

    De tabellen 'regio' en 'land' worden gedefinieerd als verwijzingstabellen en met splitsen of samenvoegen/verplaatsen bewerkingen moeten worden gekopieerd. 'klant' en 'orders' zijn op zijn beurt gedefinieerd als shard tabellen. `C_CUSTKEY` en `O_CUSTKEY` fungeren als de sharding-sleutel.

- **Referentiële integriteit aannemen**

  De service voor splitsen en samenvoegen analyseert afhankelijkheden tussen tabellen en refererende-sleutelrelaties van sleutel-primaire gebruikt om te zetten van de bewerkingen voor het verplaatsen van tabellen en shardlets. In het algemeen zijn verwijzingstabellen gekopieerd, eerst in volgorde van de afhankelijkheid, vervolgens shardlets in volgorde van de bijbehorende afhankelijkheden in elke batch worden gekopieerd. Dit is nodig zodat FK PK beperkingen met betrekking tot de doel-shard worden herkend wanneer de nieuwe gegevens worden ontvangen.

- **Consistentie van de shard-toewijzing en de uiteindelijke voltooiing**

  Geval van problemen, de service voor splitsen en samenvoegen operations hervat na een storing voordoet en is erop gericht om uit te voeren in voortgang van aanvragen. Maar mogelijk zijn er niet kan worden hersteld situaties, bijvoorbeeld wanneer de shard doel is zoekgeraakt of niet meer worden hersteld aangetast. Onder deze omstandigheden blijven bepaalde shardlets die zijn mag worden verplaatst, bevinden zich op de bron-shard. De service zorgt ervoor dat er alleen shardlet toewijzingen worden bijgewerkt nadat de gegevens die nodig is gekopieerd naar het doel. Shardlets worden alleen op de broncomputer verwijderd zodra alle hun gegevens zijn gekopieerd naar het doel en de bijbehorende toewijzingen zijn bijgewerkt. De verwijderingsbewerking gebeurt op de achtergrond terwijl het bereik op de doel-shard al online is. De service voor splitsen en samenvoegen is altijd zorgt ervoor dat de juistheid van de toewijzingen die zijn opgeslagen in de shard-toewijzing.

## <a name="the-split-merge-user-interface"></a>De gebruikersinterface van splitsen en samenvoegen

De service voor splitsen en samenvoegen-pakket bevat een werkrol en een Webrol. De Webrol wordt gebruikt voor het verzenden van aanvragen voor splitsen en samenvoegen in een interactieve wijze. De belangrijkste onderdelen van de gebruikersinterface zijn als volgt:

- **Bewerkingstype**

  Het bewerkingstype is een keuzerondje die het soort bewerking die wordt uitgevoerd door de service voor deze aanvraag bepaalt. U kunt kiezen tussen de splitsing, samenvoegen en scenario's te verplaatsen. U kunt ook een eerder ingediende bewerking annuleren. U kunt gebruiken splitsen, samenvoegen en verplaatsen van aanvragen voor bereik-shard-toewijzingen. Lijst-shard-kaarten alleen ondersteuning voor verplaatsingsbewerkingen.

- **Shard-toewijzing**

 De volgende sectie van de parameters van de aanvraag wordt informatie geboden over de shard-toewijzing en de database die als host fungeert voor de shard-toewijzing. In het bijzonder, moet u de naam van de Azure SQL Database-server en de database die als host fungeert de shardmap referenties verbinding maken met de database voor shard, en ten slotte de naam van de shard-toewijzing op te geven. De bewerking accepteert momenteel alleen een enkele set referenties. Deze referenties moeten beschikken over voldoende machtigingen voor het uitvoeren van wijzigingen in de shard-toewijzing evenals garantie voor de gebruikersgegevens op de shards.

- **Bron-bereik (splitsen en samenvoegen)**

  Een bewerking voor splitsen en samenvoegen verwerkt een bereik met de hoge en lage-sleutel. Als u een bewerking met een niet-gebonden hoge sleutelwaarde, schakel het selectievakje 'hoge sleutel is maximaal' in en laat het veld met de hoge sleutel leeg. De bereik-sleutelwaarden die u opgeeft, hoeft niet exact overeenkomt met een toewijzing en de grenzen in de shard-toewijzing. Als u geen begrenzingen bereik helemaal geen opgeeft wordt de service afgeleid het dichtstbijzijnde bereik voor u automatisch. U kunt het GetMappings.ps1 PowerShell-script gebruiken om op te halen van de huidige toewijzingen in een opgegeven shard-toewijzing.

- **Gedrag van de bron splitsen (splitsen)**

  Voor gesplitste bewerkingen, definieert het punt om op te splitsen van het bronbereik. U doen dit door op te geven van de sharding-sleutel waar u de splitsing moet plaatsvinden. Gebruik het keuzerondje opgeven of u het onderste gedeelte van het bereik (met uitzondering van de sleutel splitsen) wilt verplaatsen, of of u wilt dat het bovenste gedeelte verplaatsen (met inbegrip van de split-sleutel).

- **Bron Shardlet (verplaatsen)**

  Verplaatsingsbewerkingen verschillen van splitsen of samenvoegen bewerkingen zoals een bereik voor het beschrijven van de bron is niet vereist. Een bron voor het verplaatsen wordt gewoon geïdentificeerd door de sharding-sleutelwaarde die u van plan bent om te verplaatsen.

- **Doel Shard (splitsen)**

  Nadat u de gegevens hebt opgegeven op de bron van de splitsbewerking, moet u definiëren waar u de gegevens moeten worden gekopieerd naar door te geven van de Azure SQL Db-servernaam en databasenaam voor het doel.

- **Doelbereik (samenvoegen)**

  Bewerkingen verplaatsen shardlets aan een bestaande shard worden samengevoegd. U kunt de bestaande shard identificeren door te geven van de grenzen van het bereik van het bestaande bereik dat u samenvoegen wilt met.

- **Batchgrootte**

  De batchgrootte bepaalt het aantal shardlets die op een moment tijdens de verplaatsing van gegevens offline gaan. Dit is een geheel getal waar u lagere waarden kunt gebruiken wanneer u gevoelig zijn bent voor de lange perioden met uitvaltijd voor shardlets. Hogere waarden verhoogt de tijd die een bepaalde shardlet is offline maar kan de prestaties verbeteren.

- **Bewerkings-ID (annuleren)**

  Als u een lopende bewerking die is niet meer nodig hebt, kunt u de bewerking annuleren door op te geven van de bewerkings-ID in dit veld. U kunt de bewerkings-ID ophalen uit de tabel van de status van aanvraag (Zie de sectie 8.1) of van de uitvoer in de webbrowser waar u de aanvraag heeft ingediend.

## <a name="requirements-and-limitations"></a>Vereisten en beperkingen

De huidige implementatie van de service voor splitsen en samenvoegen is onderworpen aan de volgende vereisten en beperkingen:

- De shards moeten aanwezig zijn en worden geregistreerd in de shard-toewijzing voor een bewerking splitsen en samenvoegen in deze shards kan worden uitgevoerd.
- De service maakt geen tabellen of andere databaseobjecten automatisch als onderdeel van hun werkzaamheden. Dit betekent dat het schema voor alle shard en referentietabellen moet aanwezig zijn op de doel-shard voorafgaand aan elke bewerking splitsen of samenvoegen/verplaatsen. Shard tabellen in het bijzonder moet leeg zijn in het bereik waarin nieuwe shardlets zijn om te worden toegevoegd door een bewerking splitsen of samenvoegen/verplaatsen. Anders mislukt de bewerking de eerste consistentiecontrole op de doel-shard. Let ook op die verwijzen naar gegevens alleen worden gekopieerd als de verwijzing tabel leeg is en of er geen garanties voor consistentie met betrekking tot andere gelijktijdige bewerkingen op tabellen met het schrijven. Dit wordt aangeraden: bij het uitvoeren van splitsen en samenvoegen/bewerkingen, geen andere bewerkingen voor schrijven wijzigingen aanbrengen in de tabellen.
- De service is afhankelijk van rij identiteit die is ingesteld door een unieke index of sleutel die de sharding-sleutel ter verbetering van prestaties en betrouwbaarheid voor grote shardlets bevat. Dit kan de service om gegevens te verplaatsen op een nog betere granulatie dan alleen de sleutelwaarde sharding. Hiermee kunt u de maximale hoeveelheid logboekruimte en vergrendelingen die vereist tijdens de bewerking zijn te verminderen. Houd rekening met het maken van een unieke index of een primaire sleutel, met inbegrip van de sharding-sleutel voor een bepaalde tabel als u wilt gebruiken die tabel met splitsen of samenvoegen/verplaatsen aanvragen. Uit prestatieoverwegingen moet de sharding-sleutel de toonaangevende kolom in de sleutel of de index.
- In de loop van de verwerking van aanvragen, sommige shardlet-gegevens mogelijk aanwezig zijn zowel op de bron en de doel-shard. Dit is nodig om te beveiligen tegen fouten tijdens de verplaatsing shardlet. De integratie van splitsen en samenvoegen met de shard-toewijzing zorgt ervoor dat verbindingen via de gegevensafhankelijke routering API's via de **OpenConnectionForKey** methode voor de shard-toewijzing een inconsistente tussenliggende statussen niet ziet. Echter, bij het verbinden met de bron of de doel-shards zonder gebruik van de **OpenConnectionForKey** methode, inconsistente tussenliggende Staten mogelijk zichtbaar zijn, wanneer splitsen of samenvoegen/verplaatsen aanvragen gaat op. Deze verbindingen mogelijk gedeeltelijk of dubbele resultaten, afhankelijk van de timing of de shard onderliggende de verbinding weergegeven. Deze beperking wordt momenteel de verbindingen gemaakt door Elastic Scale meerdere Shard-query bevat.
- De database met metagegevens voor de service voor splitsen en samenvoegen moet niet worden gedeeld tussen verschillende rollen. Bijvoorbeeld, een functie van de splitsen en samenvoegen-service die wordt uitgevoerd in de faseringsmodus moet verwijzen naar een andere metagegevensdatabase dan de productie-rol.

## <a name="billing"></a>Billing

De service voor splitsen en samenvoegen wordt uitgevoerd als een cloudservice in uw Microsoft Azure-abonnement. Daarom zijn de kosten voor cloudservices van toepassing op uw exemplaar van de service. Tenzij u vaak splitsen of samenvoegen/verplaatsen bewerkingen wilt uitvoeren, wordt u aangeraden verwijderen van uw cloudservice voor splitsen en samenvoegen. Die kosten voor het werken met opgeslagen of cloud service-exemplaren geïmplementeerd. U kunt opnieuw implementeren en de configuratie van uw direct uitvoerbaar starten wanneer u wilt splitsen of samenvoegen bewerkingen uitvoeren.

## <a name="monitoring"></a>Bewaking

### <a name="status-tables"></a>Status van tabellen

De Service voor splitsen en samenvoegen biedt de **RequestStatus** tabel in de database van de store metagegevens voor de bewaking van voltooide en voortdurende aanvragen. De tabel bevat een rij voor elke splitsen en samenvoegen-aanvraag die is ingediend voor dit exemplaar van de service voor splitsen en samenvoegen. Het biedt de volgende informatie voor elke aanvraag:

- **Timestamp**

  De datum en tijd waarop de aanvraag is gestart.

- **OperationId**

  De GUID die de aanvraag wordt aangeduid. Deze aanvraag kan ook worden gebruikt om de bewerking annuleert, terwijl deze wordt nog steeds momenteel is.

- **Status**

  De huidige status van de aanvraag. De tabel voor lopende aanvragen, ook de huidige fase waarin de aanvraag is.

- **CancelRequest**

  Een vlag die aangeeft of de aanvraag is geannuleerd.

- **Wordt uitgevoerd**

  De schatting van een percentage van voor de bewerking is voltooid. Een waarde van 50 geeft aan dat de bewerking ongeveer 50% voltooid is.

- **Details**

  Een XML-waarde waarmee een meer gedetailleerd voortgangsrapport. Het rapport wordt uitgevoerd, wordt regelmatig bijgewerkt als sets rijen worden gekopieerd van bron naar doel. In het geval van fouten of uitzonderingen bevat deze kolom ook meer gedetailleerde informatie over de fout.

### <a name="azure-diagnostics"></a>Azure Diagnostics

De service voor splitsen en samenvoegen maakt gebruik van Azure Diagnostics op basis van de Azure SDK 2.5 voor controle en diagnostische gegevens. U kunt de configuratie van de diagnostische gegevens beheren, zoals hier wordt beschreven: [Inschakelen van diagnostische gegevens in Azure Cloudservices en Virtual Machines](../cloud-services/cloud-services-dotnet-diagnostics.md). Het downloadpakket bevat twee configuraties voor diagnostische gegevens: één voor de Webserverrol en één voor de werkrol. Het bevat de definities voor logboekregistratie van prestatiemeteritems, IIS-logboeken, Windows-gebeurtenislogboeken en logboeken voor splitsen en samenvoegen-toepassing.

## <a name="deploy-diagnostics"></a>Diagnostische gegevens implementeren

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Als u controle en diagnostische gegevens met behulp van de diagnostische configuratie voor de web- en werknemersrollen geleverd door het NuGet-pakket, voert u de volgende opdrachten met behulp van Azure PowerShell:

```powershell
    $storage_name = "<YourAzureStorageAccount>"
    $key = "<YourAzureStorageAccountKey"
    $storageContext = New-AzStorageContext -StorageAccountName $storage_name -StorageAccountKey $key  
    $config_path = "<YourFilePath>\SplitMergeWebContent.diagnostics.xml"
    $service_name = "<YourCloudServiceName>"
    Set-AzureServiceDiagnosticsExtension -StorageContext $storageContext -DiagnosticsConfigurationPath $config_path -ServiceName $service_name -Slot Production -Role "SplitMergeWeb"
    $config_path = "<YourFilePath>\SplitMergeWorkerContent.diagnostics.xml"
    $service_name = "<YourCloudServiceName>"
    Set-AzureServiceDiagnosticsExtension -StorageContext $storageContext -DiagnosticsConfigurationPath $config_path -ServiceName $service_name -Slot Production -Role "SplitMergeWorker"
```

U kunt meer informatie over het configureren en implementeren van de diagnostische instellingen hier vinden: [Inschakelen van diagnostische gegevens in Azure Cloudservices en Virtual Machines](../cloud-services/cloud-services-dotnet-diagnostics.md).

## <a name="retrieve-diagnostics"></a>Diagnostische gegevens ophalen

Eenvoudig kunt u uw diagnostische gegevens van de Visual Studio Server Explorer in het Azure-deel van de structuur van de Server Explorer openen. Open een exemplaar van Visual Studio en in de menubalk op weergave en Server Explorer. Klik op de Azure pictogram verbinding maken met uw Azure-abonnement. Navigeer vervolgens naar Azure Storage -> -> `<your storage account>` -> WADLogsTable-tabellen >. Zie voor meer informatie, [Server Explorer](https://msdn.microsoft.com/library/x603htbk.aspx).

![WADLogsTable][2]

De WADLogsTable gemarkeerd in de bovenstaande afbeelding bevat de gedetailleerde gebeurtenissen uit het toepassingslogboek van de splitsen en samenvoegen-service. Houd er rekening mee dat de standaardconfiguratie van het gedownloade pakket is gericht op een productie-implementatie. Daarom is het interval waarmee logboeken en prestatiemeteritems worden opgehaald uit de service-exemplaren grote (5 minuten). Voor testen en ontwikkeling, verlaagt u het interval door de diagnostische instellingen van de web- of de werkrol aan uw behoeften aan te passen. Met de rechtermuisknop op de rol in de Visual Studio Server Explorer (Zie hierboven) en past u de periode Transfer in het dialoogvenster voor de configuratie-instellingen voor diagnostische gegevens:

![Configuratie][3]

## <a name="performance"></a>Prestaties

Betere prestaties is over het algemeen worden verwacht van de hogere meer goed werkende service-lagen in Azure SQL Database. Hogere i/o-, CPU en geheugen toewijzingen voor de hogere Servicelagen profiteren van de bulksgewijs kopiëren en verwijderen van bewerkingen die gebruikmaakt van de service voor splitsen en samenvoegen. Om die reden, de service tier speciaal voor deze databases voor een gedefinieerde, een beperkte periode te vergroten.

De service wordt ook validatie van query's als onderdeel van de normale bewerkingen uitvoert. Deze validatie van query's controleren op onverwachte aanwezigheid van gegevens in het doelbereik en zorg ervoor dat elke bewerking splitsen of samenvoegen/verplaatsen wordt gestart vanuit een consistente status. Deze alle query's werken via sharding sleutelbereiken gedefinieerd door het bereik van de bewerking en de grootte van de partij geleverd als onderdeel van de definitie van de aanvraag. Deze query's het beste presteren wanneer een index aanwezig is waarvan de sharding-sleutel als de eerste kolom heeft.

Uniekheid van eigenschap met de sharding-sleutel als de eerste kolom kunnen bovendien de service moet gebruiken een geoptimaliseerde benadering waarmee resourceverbruik in termen van logboekruimte en het geheugen wordt beperkt. Deze eigenschap uniekheid is vereist voor het verplaatsen van grote gegevenshoeveelheden (doorgaans meer dan 1GB).

## <a name="how-to-upgrade"></a>Upgrade uitvoeren

1. Volg de stappen in [een service voor splitsen en samenvoegen implementeren](sql-database-elastic-scale-configure-deploy-split-and-merge.md).
2. Wijzig het configuratiebestand van cloud-service voor uw implementatie splitsen en samenvoegen in overeenstemming met de configuratieparameters voor de nieuwe. Een nieuwe vereiste parameter zijn de gegevens van het certificaat dat wordt gebruikt voor versleuteling. Er is een eenvoudige manier om dit te doen om te vergelijken van het nieuwe configuratiebestand voor een sjabloon uit de download op basis van uw bestaande configuratie. Zorg ervoor dat u de instellingen voor 'DataEncryptionPrimaryCertificateThumbprint' en 'DataEncryptionPrimary' voor zowel de web- en de werkrol toevoegen.
3. Voordat u de update implementeert naar Azure, zorg ervoor dat alle lopende splitsen en samenvoegen bewerkingen zijn voltooid. U kunt dit eenvoudig doen door het opvragen van de RequestStatus en PendingWorkflows tabellen in de metagegevensdatabase voor splitsen en samenvoegen voor lopende aanvragen.
4. Werk uw bestaande implementatie van cloud-service voor splitsen en samenvoegen in uw Azure-abonnement met het nieuwe pakket en het bijgewerkte configuratiebestand zijn opgenomen.

U hoeft niet voor het inrichten van een nieuwe database met metagegevens voor splitsen en samenvoegen om bij te werken. De nieuwe versie, de bestaande database met metagegevens automatisch bijgewerkt naar de nieuwe versie.

## <a name="best-practices--troubleshooting"></a>Best practices en probleemoplossing

- Definieer een testtenant en uitoefening van uw belangrijkste splitsen of samenvoegen/verplaatsen-bewerkingen met de testtenant over verschillende shards. Zorg ervoor dat alle metagegevens correct is gedefinieerd in de shard-toewijzing en dat de bewerkingen niet in strijd beperkingen of refererende sleutels zijn.
- Houd de testtenant problemen met betrekking tot de grootte van gegevens boven de maximale grootte van de grootste tenant om te controleren of u niet regelmatig gegevensgrootte. Dit helpt u bij het bepalen van een bovengrens van de tijd die nodig is voor het verplaatsen van één tenant.
- Zorg ervoor dat uw schema verwijderingen geeft. De service voor splitsen en samenvoegen vereist de mogelijkheid om gegevens te verwijderen uit de bron-shard zodra de gegevens is gekopieerd naar het doel. Bijvoorbeeld, **triggers verwijderen** kunt voorkomen dat de service verwijderen van de gegevens op de bron- en kan leiden tot bewerkingen mislukken.
- De sharding-sleutel moet de eerste kolom in uw primaire sleutel of unieke indexdefinitie. Dat zorgt ervoor dat de beste prestaties voor de splitsen of samenvoegen validatie-query's en de werkelijke gegevens verplaatsen en verwijderen van bewerkingen die altijd worden uitgevoerd op sharding sleutelbereiken.
- Plaatsen van uw service voor splitsen en samenvoegen in de regio en Datacenter waar uw databases zich bevinden.

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Anchors-->
<!--Image references-->
[1]:./media/sql-database-elastic-scale-overview-split-and-merge/split-merge-overview.png
[2]:./media/sql-database-elastic-scale-overview-split-and-merge/diagnostics.png
[3]:./media/sql-database-elastic-scale-overview-split-and-merge/diagnostics-config.png
