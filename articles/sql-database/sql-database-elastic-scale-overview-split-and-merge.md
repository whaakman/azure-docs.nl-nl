---
title: Verplaatsen van gegevens tussen cloud uitgebreide databases | Microsoft Docs
description: Legt uit hoe manipuleren shards en verplaatsen van gegevens via een zelf gehoste service met behulp van API's van elastische database.
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
ms.assetid: 204fd902-0397-4185-985a-dea3ed7c7d9f
ms.service: sql-database
ms.custom: scale out apps
ms.workload: Inactive
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: 328989c4fc1f9a404d4c048eb148a95e9105bdf5
ms.sourcegitcommit: dfd49613fce4ce917e844d205c85359ff093bb9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2017
---
# <a name="moving-data-between-scaled-out-cloud-databases"></a>Gegevens verplaatsen tussen uitgeschaalde clouddatabases
Als u een Software als een Service-ontwikkelaar bent en plotseling uw app grote vraag ondergaat, moet u ruimte is voor de groei. Zodat toevoegen u meer databases (shards). Hoe u opnieuw distribueren de gegevens naar de nieuwe databases zonder te onderbreken van de integriteit van gegevens? Gebruik de **gesplitste merge tool** gegevens van beperkte databases naar de nieuwe databases verplaatsen.  

Het hulpprogramma gesplitste samenvoegen wordt uitgevoerd als een Azure-web-service. Een beheerder of ontwikkelaar gebruikt het hulpprogramma shardlets (gegevens uit een shard) verplaatsen tussen verschillende databases (shards). Het hulpprogramma maakt gebruik van beheer van shard-toewijzing te onderhouden de metagegevens-database en zorg ervoor dat de consistente toewijzingen.

![Overzicht][1]

## <a name="download"></a>Downloaden
[Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Service.SplitMerge/)

## <a name="documentation"></a>Documentatie
1. [Elastische database gesplitste Merge tool-zelfstudie](sql-database-elastic-scale-configure-deploy-split-and-merge.md)
2. [Gesplitste samenvoegen Beveiligingsconfiguratie](sql-database-elastic-scale-split-merge-security-configuration.md)
3. [Beveiligingsoverwegingen gesplitste samenvoegen](sql-database-elastic-scale-split-merge-security-configuration.md)
4. [Shard-toewijzingsbeheer](sql-database-elastic-scale-shard-map-management.md)
5. [Bestaande databases migreren voor uitschaling](sql-database-elastic-convert-to-use-elastic-tools.md)
6. [Hulpprogramma's voor elastische database](sql-database-elastic-scale-introduction.md)
7. [Elastische Database extra verklarende woordenlijst](sql-database-elastic-scale-glossary.md)

## <a name="why-use-the-split-merge-tool"></a>Waarom het hulpprogramma gesplitste samenvoegen gebruiken?
**Flexibiliteit**

Toepassingen moeten worden uitgerekt flexibel vallen buiten de grenzen van één Azure SQL DB-database. Het hulpprogramma gebruiken om gegevens te verplaatsen naar behoefte nieuwe databases terwijl de integriteit te behouden.

**Gesplitste groeien** 

U moet de algehele capaciteit voor het afhandelen van explosief gegroeid verhogen. Om dit te doen, extra capaciteit te maken door sharding de gegevens en over stapsgewijs meer databases verdeeld totdat de capaciteitsbehoeften is voldaan. Dit is een goed voorbeeld van de functie 'splitsen'. 

**Samenvoegen verkleinen**

Capaciteit moet verkleinen vanwege de seizoensgebonden aard van een bedrijf. Het hulpprogramma kunt u omlaag schalen naar minder schaaleenheden wanneer bedrijven wordt vertraagd. De functie 'samenvoegen' in de elastische Schaalfunctionaliteit gesplitste samenvoegen Service bevat informatie over deze vereiste. 

**Hotspots beheren door shardlets verplaatsen**

De toewijzing van shardlets naar shards kan met meerdere tenants per database leiden tot capaciteit knelpunten op sommige shards. Dit is vereist voor opnieuw toewijzen van shardlets of bezet shardlets verplaatsen naar nieuwe of minder gebruikte shards. 

## <a name="concepts--key-features"></a>Concepten & belangrijke functies
**Klant gehoste services**

De split-samenvoegen wordt geleverd als een klant gehoste service. U moet implementeren en de service hosten in uw Microsoft Azure-abonnement. Het u van het NuGet downloaden-pakket bevat een configuratiesjabloon voltooid met de informatie voor uw specifieke implementatie. Zie de [gesplitste samenvoegen zelfstudie](sql-database-elastic-scale-configure-deploy-split-and-merge.md) voor meer informatie. Omdat de service wordt uitgevoerd in uw Azure-abonnement, kunt u instellen en configureren van de meeste beveiligingsaspecten van de service. De standaardsjabloon bevat de opties voor het configureren van SSL, op basis van certificaten voor clientverificatie en versleuteling van opgeslagen referenties, DoS beveiligen en IP-beperkingen. U vindt meer informatie over de beveiligingsaspecten in het volgende document [gesplitste samenvoegen Beveiligingsconfiguratie](sql-database-elastic-scale-split-merge-security-configuration.md).

De standaardwaarde geïmplementeerd met een Webrol en een werknemer de service wordt uitgevoerd. Gebruik de A1-VM-grootte in Azure Cloud Services. Terwijl u kunt deze instellingen niet wijzigen wanneer u het pakket implementeert, kunt u ze kunt wijzigen na een geslaagde implementatie in de actieve cloudservice (via de Azure portal). Houd er rekening mee dat de werkrol moet niet worden geconfigureerd voor meer dan één exemplaar voor technische redenen. 

**Integratie van shard-kaart**

De splitsing merge-service communiceert met de shard-toewijzing van de toepassing. Wanneer u de splitsing merge-service om te splitsen of samenvoegen van bereiken of shardlets verplaatsen tussen shards, blijft de service automatisch de shard-toewijzing up-to-date te houden. Om dit te doen, wordt de service verbindt met de shard kaart manager-database van de toepassing en onderhoudt bereiken en toewijzingen als voortgang merge-gesplitste/move-aanvragen. Dit zorgt ervoor dat de shard-toewijzing een actuele weergave altijd weergegeven wanneer de splitsing samenvoegbewerkingen gaat op. Splitsen, zijn samenvoegen en shardlet movement-bewerkingen door het verplaatsen van een batch shardlets van de bron-shard naar de doel-shard geïmplementeerd. Tijdens de bewerking van de verplaatsing shardlet de shardlets onderworpen aan de huidige batch zijn gemarkeerd als offline in de shard-toewijzing en zijn niet beschikbaar voor gegevensafhankelijke routering verbindingen met behulp van de **OpenConnectionForKey** API. 

**Consistente shardlet-verbindingen**

Wanneer gegevensverplaatsing wordt gestart voor een nieuwe batch shardlets, shard-toewijzing opgegeven gegevensafhankelijke routering verbindingen met het opslaan van de shardlet shard gedode en volgende verbindingen vanaf de shard-toewijzing API's naar deze zijn shardlets tijdens de gegevens worden geblokkeerd verkeer wordt uitgevoerd om te voorkomen dat inconsistenties. Verbindingen met andere shardlets op de dezelfde shard wordt ook ophalen afgesloten, maar opnieuw slaagt onmiddellijk taak opnieuw wordt uitgevoerd. Zodra de batch wordt verplaatst, de shardlets online opnieuw voor de shard doel zijn gemarkeerd en de brongegevens is verwijderd uit de bron-shard. De service gaat door deze stappen voor elke batch totdat alle shardlets zijn verplaatst. Dit leidt tot verschillende kill-bewerkingen in de loop van de volledige merge-gesplitste/move-bewerking.  

**Shardlet beschikbaarheid van beheren**

Beperken van de verbinding als de huidige batch van shardlets zoals hierboven wordt beschreven beëindigt, kan beperkt het bereik van ontbreken tot één batch van shardlets tegelijk. Dit heeft de voorkeur boven een benadering waarbij de volledige shard blijft offline voor alle bijbehorende shardlets in de loop van een gesplitste of merge-bewerking. De grootte van een batch, gedefinieerd als het aantal unieke shardlets verplaatsen op een tijdstip is een configuratieparameter. Het kan worden gedefinieerd voor elke bewerking samenvoegen en splitsen afhankelijk van de beschikbaarheid en prestaties behoeften van de toepassing. Houd er rekening mee dat het bereik dat wordt vergrendeld in de shard-toewijzing mogelijk groter is dan de opgegeven batch. Dit komt doordat de service de grootte van het bereik kiest dat het werkelijke aantal sharding sleutelwaarden in de gegevens ongeveer overeenkomt met de batchgrootte van de. Dit is belangrijk te weten met name voor zeer ingevuld sharding-sleutels. 

**Metagegevensopslag**

De splitsing merge-service gebruikt een database om de status ervan onderhouden en logboeken tijdens de verwerking van aanvragen. De gebruiker deze database wordt gemaakt in het abonnement en voorziet de verbindingsreeks in het configuratiebestand voor de service-implementatie. Beheerders van de organisatie van de gebruiker kunnen ook verbinding maken met deze database om de voortgang van de aanvraag te nemen en voor het onderzoeken van gedetailleerde informatie over mogelijke fouten.

**Sharding-awareness**

De service gesplitste samenvoegen onderscheidt tussen (1) de shard-tabellen, (2) verwijzingsdimensies en (3) de normale tabellen. De semantiek van een merge-gesplitste/move-bewerking afhankelijk van het type van de tabel die wordt gebruikt en wordt als volgt gedefinieerd: 

* **Shard tabellen**: gesplitste, samenvoegen en migratiebewerkingen verplaatsen van shardlets van bron naar doel shard. Nadat de algehele aanvraag is voltooid zijn die shardlets niet langer aanwezig op de bron. Houd er rekening mee dat de doeltabellen moeten op de doel-shard bestaan en mag geen gegevens in het doelbereik voordat de verwerking van de bewerking. 
* **Verwijzen naar tabellen**: voor verwijzingsdimensies, de splitsing samen te voegen en bewerkingen kunt u de gegevens van de bron kopiëren naar de doel-shard te verplaatsen. Let echter op dat er geen wijzigingen doorgevoerd op de doel-shard voor een bepaalde tabel als een rij al aanwezig in deze tabel op het doel is. De tabel is leeg voor een verwijzing naar tabel kopieerbewerking op verwerkt.
* **Andere tabellen**: andere tabellen kunnen worden gebruikt op de bron- of het doel van een bewerking voor samenvoegen en splitsen. De service gesplitste samenvoegen negeert deze tabellen voor gegevensverplaatsing of te kopiëren. Let echter op dat ze deze bewerkingen in het geval van beperkingen kunnen verstoren.

De informatie van verwijzing versus shard tabellen wordt verstrekt door de **SchemaInfo** API's op de shard-kaart. Het volgende voorbeeld ziet u het gebruik van deze API's op een gegeven shard kaart manager object smm: 

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

De tabellen 'regio' en 'land' worden gedefinieerd als verwijzingstabellen en met gesplitste of merge/verplaatsen bewerkingen moeten worden gekopieerd. 'klant' en 'orders' worden op zijn beurt gedefinieerd als shard tabellen. C_CUSTKEY en O_CUSTKEY fungeren als de sharding-sleutel. 

**Referentiële integriteit**

De service gesplitste samenvoegen analyseert afhankelijkheden tussen de tabellen en sleutel-primaire refererende sleutels gebruikt voor het Faseren van de bewerkingen voor het verplaatsen van verwijzingsdimensies en shardlets. In het algemeen verwijzingsdimensies gekopieerd eerst in volgorde van de afhankelijkheid, worden in volgorde van hun afhankelijkheden in elke batch shardlets gekopieerd. Dit is noodzakelijk om FK PK beperkingen op de doel-shard worden gehonoreerd als de nieuwe gegevens aankomen. 

**Consistentie van shard-toewijzing en de uiteindelijke voltooiing**

In geval van problemen, de service gesplitste samenvoegen operations hervat na een storing en uitvoeren in voortgang aanvragen is erop gericht. Echter mogelijk zijn er onherstelbare situaties, bijvoorbeeld wanneer de doel-shard zoekraakt of wordt geknoeid niet meer worden hersteld. In deze omstandigheden kan sommige shardlets die moesten worden verplaatst op de bron-shard blijven. De service zorgt ervoor dat er alleen shardlet toewijzingen worden bijgewerkt nadat de gegevens die nodig is gekopieerd naar het doel. Shardlets zijn alleen op de bron verwijderd nadat alle bijbehorende gegevens is gekopieerd naar het doel en de bijbehorende toewijzingen zijn bijgewerkt. De verwijderingsbewerking wordt op de achtergrond terwijl het bereik op de doel-shard al online is. De service gesplitste samenvoegen zorgt altijd juistheid van de toewijzingen opgeslagen in de shard-toewijzing.

## <a name="the-split-merge-user-interface"></a>De gebruikersinterface van de gesplitste samenvoegen
Het pakket gesplitste samenvoegen service omvat een werkrol en een Webrol. De Webrol wordt gebruikt voor het splitsen merge-aanvragen indienen op interactieve wijze. De belangrijkste onderdelen van de gebruikersinterface zijn als volgt:

* Type voor bewerking: Het bewerkingstype is een keuzerondje die het soort bewerking uitgevoerd door de service voor deze aanvraag bepaalt. U kunt kiezen tussen de splitsing samenvoegen en scenario's te verplaatsen. U kunt ook een eerder ingediende bewerking annuleren. U kunt gesplitste gebruiken, samenvoegen en aanvragen voor de shard-kaarten bereik verplaatsen. Lijst shard wijst alleen ondersteuning migratiebewerkingen.
* Shard-toewijzing: Informatie over de shard-toewijzing en de database die als host fungeert voor de shard-kaart hebben betrekking op de volgende sectie van de parameters van de aanvraag. In het bijzonder, moet u de naam van de Azure SQL Database-server en database die als host fungeert de shardmap referenties voor verbinding met de shard-toewijzing-database en ten slotte de naam van de shard-toewijzing. Op dit moment de bewerking kan slechts één set referenties. Deze referenties moeten voldoende rechten hebt voor het uitvoeren van wijzigingen in de shard kaart ook garantie voor de gebruikersgegevens op de shards.
* Bron-bereik (splitsen en samenvoegen): een bewerking voor samenvoegen en splitsen een bereik met behulp van de lage en hoge sleutel verwerkt. Geef een bewerking met een hoge sleutelwaarde unbounded door het selectievakje 'hoge sleutel is max' en hoge sleutel veld leeg laten. De belangrijkste waarden die u opgeeft hoeft niet exact overeenkomen met een toewijzing en de grenzen in de shard-toewijzing. Als u geen geen begrenzingen bereik op alle opgeeft wordt de service afgeleid het dichtstbijzijnde bereik voor u automatisch. U kunt het GetMappings.ps1 PowerShell-script gebruiken om op te halen van de huidige toewijzingen in een bepaalde shard-toewijzing.
* Gesplitste bron gedrag (splitsen): voor gesplitste bewerkingen, definiëren op het punt om te splitsen van het bronbereik. U doen dit door de waar u de splitsing sharding-sleutel. Gebruik het keuzerondje opgeven of u het onderste gedeelte van het bereik (met uitzondering van de sleutel splitsen) wilt verplaatsen, of of u wilt dat het bovenste gedeelte verplaatsen (inclusief de split-sleutel).
* Bron Shardlet (verplaatsen): verplaatsen bewerkingen zijn verschilt van de gesplitste of merge-bewerkingen zoals een bereik te beschrijven van de bron is niet vereist. Een bron voor het verplaatsen wordt gewoon geïdentificeerd door de sleutelwaarde van sharding die u van plan bent te verplaatsen.
* Shard (splitsen) als doel: nadat u de gegevens hebt opgegeven voor de bron van de splitsbewerking, moet u definiëren waar u de gegevens worden gekopieerd naar door de Azure SQL Db-server en database de naam voor het doel te geven.
* Doelbereik (samenvoegen): samenvoegbewerkingen shardlets verplaatsen naar een bestaande shard. U identificeren de bestaande shard door te geven van het bestaande bereik dat u samenvoegen wilt met de grenzen van het bereik.
* Batchgrootte: De batchgrootte bepaalt het aantal shardlets die op een moment tijdens de gegevensverplaatsing offline gaan. Dit is een integerwaarde waar u lagere waarden kunt gebruiken wanneer u gevoelig zijn voor lange perioden met uitvaltijd voor shardlets. Hogere waarden verhoogt de tijd die een bepaalde shardlet offline maar de prestaties mogelijk verbeterd.
* Bewerking-Id (annuleren): Als u er een bewerking die niet meer nodig hebt, kunt u de bewerking annuleren door op te geven van de bewerkings-ID in dit veld. U kunt de bewerkings-ID ophalen uit de tabel van de status van aanvraag (Zie de sectie 8.1) of van de uitvoer in de webbrowser waar u de aanvraag ingediend.

## <a name="requirements-and-limitations"></a>Vereisten en beperkingen
De huidige implementatie van de gesplitste merge-service is onderworpen aan de volgende vereisten en beperkingen: 

* De shards moeten voorkomen en zijn geregistreerd in de shard-toewijzing voordat een gesplitste merge-bewerking op deze shards kan worden uitgevoerd. 
* De service maakt geen tabellen of andere databaseobjecten automatisch als onderdeel van bewerkingen. Dit betekent dat het schema voor alle shard-tabellen en verwijzingsdimensies moet bestaan op de doel-shard voordat een merge-gesplitste/move-bewerking. Shard tabellen zijn met name vereist voor het niet leeg zijn in het bereik waarin nieuwe shardlets zijn moeten worden toegevoegd door een merge-gesplitste/move-bewerking. Anders mislukt de bewerking uit de eerste consistentiecontrole op de doel-shard. Let ook op die verwijzen naar gegevens alleen worden gekopieerd als de verwijzing tabel leeg is en of er geen consistentiecontrole garanties met betrekking tot andere gelijktijdige bewerkingen op tabellen met het schrijven. We raden u dit: wanneer gesplitste/merge-bewerkingen wordt uitgevoerd, geen schrijfbewerkingen wijzigingen aanbrengen in de tabellen.
* De service is afhankelijk van de identiteit van de rij is ingesteld door een unieke index of de sleutel met de sleutel sharding ter verbetering van prestaties en betrouwbaarheid voor grote shardlets. Hierdoor kan de service om gegevens te verplaatsen op een zelfs weer specifieker dan alleen de sleutelwaarde sharding. Dit helpt bij het verminderen van de maximale hoeveelheid logboekruimte en vergrendelingen die vereist tijdens de bewerking zijn. Houd rekening met het maken van een unieke index of een primaire sleutel, met inbegrip van de sharding-sleutel op een bepaalde tabel als u wilt gebruiken die tabel met merge-gesplitste/move-aanvragen. Uit prestatieoverwegingen moet de sleutel sharding de toonaangevende kolom in de sleutel of de index.
* Tijdens de verwerking van aanvragen mogelijk bepaalde gegevens shardlet aanwezig op de bron en de doel-shard. Dit is nodig om te beschermen tegen fouten tijdens het shardlet-verkeer. De integratie van split-samenvoegen met de shard-toewijzing zorgt ervoor dat verbindingen via de gegevens afhankelijke routering API's met de **OpenConnectionForKey** methode voor de shard-toewijzing een inconsistente tussenliggende statussen niet ziet. Echter, bij het verbinden met de bron- of de doel-shards zonder gebruik van de **OpenConnectionForKey** methode inconsistente tussenliggende statussen mogelijk zichtbaar wanneer merge-gesplitste/move-aanvragen zijn die aan. Deze verbindingen gedeeltelijke of dubbele resultaten, afhankelijk van de timing of de shard onderliggende de verbinding kunnen worden weergegeven. Deze beperking bevat momenteel de verbindingen gemaakt door de elastische Schaalfunctionaliteit van meerdere Shard-query.
* De database met metagegevens voor de service gesplitste merge moet niet worden gedeeld tussen verschillende rollen. Een rol van de service gesplitste samenvoegen in fasering moet verwijzen naar een andere metagegevensdatabase dan de productie-rol.

## <a name="billing"></a>Facturering
De splitsing merge-service wordt uitgevoerd als een cloudservice in uw Microsoft Azure-abonnement. Kosten voor cloudservices zijn daarom van toepassing op uw exemplaar van de service. Tenzij u vaak split of merge/verplaatsen bewerkingen uitvoert, wordt u aangeraden verwijderen van uw cloudservice gesplitste samenvoegen. Die kosten voor het werken met opgeslagen of geïmplementeerd cloud service-exemplaren. U kunt opnieuw implementeren en de configuratie van uw gemakkelijk uitvoerbare start wanneer u wilt splitsen of samenvoegen bewerkingen uitvoeren. 

## <a name="monitoring"></a>Bewaking
### <a name="status-tables"></a>Status tabellen
De splitsing merge-Service biedt de **RequestStatus** tabel in de store-database van de metagegevens voor de bewaking van voltooide en lopende aanvragen. De tabel bevat een rij voor elke aanvraag gesplitste samenvoegen die met dit exemplaar van de gesplitste merge-service is verzonden. Dit biedt de volgende informatie voor elke aanvraag:

* **Tijdstempel**: de tijd en datum waarop de aanvraag is gestart.
* **OperationId**: een GUID die de aanvraag wordt aangeduid. Deze aanvraag kan ook worden gebruikt voor de bewerking annuleert, terwijl deze nog steeds uitgevoerd wordt.
* **Status**: de huidige status van de aanvraag. Voor actieve aanvragen, ook geeft de huidige fase waarin de aanvraag is.
* **CancelRequest**: een vlag die aangeeft of de aanvraag is geannuleerd.
* **Voortgang**: een percentage van de schatting van voor de bewerking is voltooid. Een waarde van 50 geeft aan dat de bewerking ongeveer 50% voltooid.
* **Details**: een XML-waarde die een gedetailleerdere voortgangsrapport biedt. Het voortgangsrapport wordt regelmatig bijgewerkt als sets van rijen worden gekopieerd van bron naar doel. In geval van fouten en uitzonderingen in deze kolom ook meer gedetailleerde informatie over de fout bevat.

### <a name="azure-diagnostics"></a>Azure Diagnostics
De service gesplitste samenvoegen gebruikt Azure Diagnostics op basis van de Azure SDK 2.5 voor controle en diagnostische gegevens. U de configuratie van diagnostische beheren, zoals hier wordt beschreven: [diagnostische gegevens inschakelen in Azure Cloud Services en virtuele Machines](../cloud-services/cloud-services-dotnet-diagnostics.md). Het downloadpakket bevat twee diagnostics configuraties, één voor de Webserverrol en één voor de werkrol. Deze configuraties voor diagnostische gegevens voor de service Volg de richtlijnen van [Cloud Service grondbeginselen in Microsoft Azure](https://code.msdn.microsoft.com/windowsazure/Cloud-Service-Fundamentals-4ca72649). Het bevat de definities voor logboekregistratie van prestatiemeteritems, IIS-logboeken, Windows-gebeurtenislogboeken en gebeurtenislogboeken van toepassingen gesplitste samenvoegen. 

## <a name="deploy-diagnostics"></a>Diagnostische gegevens implementeren
Als u de controle en diagnostische gegevens met behulp van de diagnostische configuratie voor de web- en werkrollen rollen geleverd door het NuGet-pakket, voert u de volgende met Azure PowerShell-opdrachten: 

    $storage_name = "<YourAzureStorageAccount>" 

    $key = "<YourAzureStorageAccountKey" 

    $storageContext = New-AzureStorageContext -StorageAccountName $storage_name -StorageAccountKey $key  


    $config_path = "<YourFilePath>\SplitMergeWebContent.diagnostics.xml" 

    $service_name = "<YourCloudServiceName>" 

    Set-AzureServiceDiagnosticsExtension -StorageContext $storageContext -DiagnosticsConfigurationPath $config_path -ServiceName $service_name -Slot Production -Role "SplitMergeWeb" 


    $config_path = "<YourFilePath>\SplitMergeWorkerContent.diagnostics.xml" 

    $service_name = "<YourCloudServiceName>" 

    Set-AzureServiceDiagnosticsExtension -StorageContext $storageContext -DiagnosticsConfigurationPath $config_path -ServiceName $service_name -Slot Production -Role "SplitMergeWorker" 

U vindt meer informatie over het configureren en implementeren van diagnostische instellingen hier: [diagnostische gegevens inschakelen in Azure Cloud Services en virtuele Machines](../cloud-services/cloud-services-dotnet-diagnostics.md). 

## <a name="retrieve-diagnostics"></a>Diagnostische gegevens ophalen
U kunt eenvoudig uw diagnostics openen vanaf Visual Studio Server Explorer in de Azure, deel van de Server Explorer-structuur. Open een exemplaar van de Visual Studio en in de menubalk op weergeven en Server Explorer. Klik op het pictogram voor verbinding maken met uw Azure-abonnement in Azure. Ga naar Azure Storage -> -> <your storage account> -> tabellen-WADLogsTable >. Zie voor meer informatie [opslagbronnen bladeren met Server Explorer](http://msdn.microsoft.com/library/azure/ff683677.aspx). 

![WADLogsTable][2]

De WADLogsTable gemarkeerd in de bovenstaande afbeelding bevat de gedetailleerde gebeurtenissen uit het toepassingslogboek van de gesplitste merge-service. Houd er rekening mee dat de standaardconfiguratie van het gedownloade pakket is gericht op een productie-implementatie. Daarom is het interval waarmee logboeken en prestatiemeteritems worden opgehaald uit de service-exemplaren grote (5 minuten). Voor testen en ontwikkeling, het interval te verlagen door de instellingen voor diagnostische gegevens van de web- of de werkrol aan uw behoeften aan te passen. Met de rechtermuisknop op de rol in de Visual Studio Server Explorer (Zie hierboven) en past u de periode Transfer in het dialoogvenster voor de configuratie-instellingen voor diagnostische gegevens: 

![Configuratie][3]

## <a name="performance"></a>Prestaties
Betere prestaties is over het algemeen des te hoger meer zodat Servicelagen in Azure SQL Database worden verwacht. Hogere i/o, CPU en geheugen toewijzingen voor de hogere Servicelagen profiteren van de bulksgewijs kopiëren en verwijderen van bewerkingen die gebruikmaakt van de service gesplitste samenvoegen. Daarom moet de servicelaag slechts voor deze databases voor een gedefinieerde, beperkte periode te vergroten.

De service worden ook de validatie van query's als onderdeel van de normale bewerkingen uitgevoerd. Deze validatie van query's controleren op onverwachte aanwezigheid van gegevens in het doelbereik en zorg ervoor dat een merge-gesplitste/move-bewerking wordt gestart van een consistente status. Deze alle query's werken via sharding sleutelbereiken gedefinieerd door het bereik van de bewerking en de grootte van de partij geleverd als onderdeel van de definitie van de aanvraag. Deze query's werken het beste als een index aanwezig is waarvan de sharding sleutel als de toonaangevende kolom heeft. 

Uniekheid van eigenschap met de sleutel sharding als de toonaangevende kolom kan bovendien de service moet een geoptimaliseerde aanpak die brongebruik in termen van logboekruimte en geheugen beperkt gebruiken. Deze eigenschap uniekheid is vereist voor het verplaatsen van grote hoeveelheden gegevens grootten (meestal meer dan 1GB). 

## <a name="how-to-upgrade"></a>Bijwerken
1. Volg de stappen in [implementeren van een service gesplitste samenvoegen](sql-database-elastic-scale-configure-deploy-split-and-merge.md).
2. Wijzig het configuratiebestand voor cloud-service voor uw implementatie gesplitste samenvoegen in overeenstemming met de configuratieparameters voor de nieuwe. Een nieuwe vereiste parameter is de informatie over het certificaat dat wordt gebruikt voor versleuteling. Een eenvoudige manier om dit te doen, is het nieuwe bestand van de sjabloon configuratie van de download op basis van uw bestaande configuratie te vergelijken. Zorg ervoor dat u de instellingen voor 'DataEncryptionPrimaryCertificateThumbprint' en 'DataEncryptionPrimary' voor de web- en de werkrol toevoegen.
3. Zorg ervoor dat alle lopende gesplitste merge-bewerkingen zijn voltooid voordat u Azure implementeert de update. U kunt dit eenvoudig doen door het opvragen van de RequestStatus en PendingWorkflows tabellen in de metagegevensdatabase gesplitste samenvoegen voor actieve aanvragen.
4. Werk uw bestaande cloud service-implementatie voor split-samenvoegen in uw Azure-abonnement met het nieuwe pakket en uw bijgewerkte serviceconfiguratiebestand.

U hoeft niet voor het inrichten van een nieuwe metagegevensdatabase voor split-samenvoegen om bij te werken. De nieuwe versie wordt automatisch de bestaande metagegevensdatabase van een upgrade naar de nieuwe versie. 

## <a name="best-practices--troubleshooting"></a>Aanbevolen procedures en probleemoplossing
* Definieer een testtenant en uw belangrijkste split of merge/verplaatsen bewerkingen met de testtenant uitoefenen over meerdere shards. Zorg ervoor dat alle metagegevens correct is gedefinieerd in de shard-toewijzing en dat de bewerkingen niet-beperkingen of refererende sleutels schenden.
* Houd de testtenant problemen met betrekking tot de omvang van gegevens boven de maximale grootte van uw grootste tenant zodat u niet gegevensgrootte doe. Zo kunt u een bovengrens van de tijd die nodig is voor het verplaatsen van een enkele tenant te beoordelen. 
* Zorg ervoor dat uw schema verwijderingen toestaat. De service gesplitste samenvoegen vereist de mogelijkheid om gegevens te verwijderen uit de bron-shard nadat de gegevens is gekopieerd naar het doel. Bijvoorbeeld: **triggers verwijderen** kunt voorkomen dat de service verwijderen van de gegevens op de bron- en kan leiden tot bewerkingen mislukken.
* De sharding-sleutel moet de toonaangevende kolom in de primaire sleutel of unieke indexdefinitie. Die ervoor zorgt voor de beste prestaties voor de splitsen of samenvoegen validatie-query's en de bewerkingen voor de actuele gegevens verplaatsing en verwijderen die altijd werken op sharding sleutelbereiken.
* Plaatsen van uw service gesplitste samenvoegen in de regio en Datacenter waar uw database zich bevinden. 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Anchors-->
<!--Image references-->
[1]:./media/sql-database-elastic-scale-overview-split-and-merge/split-merge-overview.png
[2]:./media/sql-database-elastic-scale-overview-split-and-merge/diagnostics.png
[3]:./media/sql-database-elastic-scale-overview-split-and-merge/diagnostics-config.png

