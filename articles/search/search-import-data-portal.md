<properties
    pageTitle="Gegevens importeren in Azure Search met de indexeerfuncties in de Azure-portal | Microsoft Azure | Gehoste service voor zoeken in de cloud"
    description="De wizard Gegevens importeren van Azure Search in de Azure Portal gebruiken om gegevens te verkennen in Azure Blob Storage, Table Storage, SQL Database en SQL Server op virtuele Azure-machines."
    services="search"
    documentationCenter=""
    authors="HeidiSteen"
    manager="jhubbard"
    editor=""
    tags="Azure Portal"/>

<tags
    ms.service="search"
    ms.devlang="na"
    ms.workload="search"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.date="08/29/2016"
    ms.author="heidist"/>

# Gegevens importeren in Azure Search via de portal

De Azure Portal bevat de wizard **Gegevens importeren** op het Azure Search-dashboard om gegevens in een index te laden. 

  ![Gegevens importeren in de opdrachtbalk][1]

Intern configureert de wizard een *indexeerfunctie* en roept deze aan, waarmee verschillende stappen van het indexeringsproces worden geautomatiseerd: 

- Verbinding maken met een externe gegevensbron in het huidige Azure-abonnement
- Automatisch genereren van een indexschema op basis van de brongegevensstructuur
- Documenten maken op basis van een rijenset opgehaald uit de gegevensbron
- Documenten uploaden naar de index in uw zoekservice

U kunt deze werkstroom met voorbeeldgegevens uitproberen in DocumentDB. Ga naar [Aan de slag met Azure Search in de Azure Portal](search-get-started-portal.md) voor instructies.

## Gegevensbronnen die worden ondersteund door de wizard Gegevens importeren

Indexeren van Automation en hulpprogramma's is beschikbaar voor de volgende gegevensbronnen: 

- Azure SQL Database
- Relationele SQL Server-gegevens op een virtuele machine van Azure
- Azure DocumentDB
- Azure Blob Storage (in de preview-versie)
- Azure Table Storage (in de preview-versie)

Een platte gegevensset is een vereiste invoer. U kunt slechts importeren uit één tabel, databaseweergave of gelijkwaardige gegevensstructuur. U moet deze gegevensstructuur maken voordat u de wizard uitvoert.

Houd er rekening mee dat een paar van de indexeerfuncties nog steeds in de preview-versie zijn, wat betekent dat de definitie van de indexeerfunctie wordt ondersteund door de preview-versie van de API. Zie [Overzicht van indexeerfuncties](search-indexer-overview.md) voor meer informatie en koppelingen.

## Verbinding maken met uw gegevens

1. Meld u aan bij de [Azure Portal](https://portal.azure.com) en open het servicedashboard. U kunt klikken op **Services zoeken** in de koppelingsbalk om de bestaande services in het huidige abonnement weer te geven. 

2. Klik op **Gegevens importeren** in de opdrachtbalk om de blade Gegevens importeren te openen.  

3. Klik op **Verbinden met uw gegevens** om een gegevensbrondefinitie op te geven die door een indexeerfunctie wordt gebruikt. Voor gegevensbronnen binnen het abonnement kan de wizard meestal verbindingsgegevens detecteren en lezen, zodat de algehele configuratievereisten minimaal zijn.

| | |
|--------|------------|
|**Bestaande gegevensbron** | Als u al indexeerfuncties hebt gedefinieerd in uw zoekservice, kunt u een bestaande gegevensbrondefinitie voor een andere import selecteren.|
|**Azure SQL Database** | De servicenaam, referenties voor een databasegebruiker met leesmachtiging en de naam van een database kunnen worden opgegeven op de pagina of via een ADO.NET-verbindingsreeks. Kies de verbindingsreeksoptie om eigenschappen te bekijken of aan te passen. <br/><br/>De tabel of weergave die de rijenset bevat, moet worden opgegeven op de pagina. Deze optie wordt weergegeven nadat de verbinding tot stand is gebracht, waarna een vervolgkeuzelijst wordt weergegeven zodat u een selectie kunt maken.|
|**SQL Server op virtuele Azure-machine** | Geef een FQDN-servicenaam, gebruikers-ID en wachtwoord en de database als een verbindingsreeks op. Voor het gebruik van deze gegevensbron moet u eerder een certificaat hebben geïnstalleerd in het lokale archief dat de verbinding versleutelt. <br/><br/>De tabel of weergave die de rijenset bevat, moet worden opgegeven op de pagina. Deze optie wordt weergegeven nadat de verbinding tot stand is gebracht, waarna een vervolgkeuzelijst wordt weergegeven zodat u een selectie kunt maken.
|**DocumentDB** |Vereisten zijn het account, de database en de verzameling. Alle documenten in de verzameling worden opgenomen in de index. U kunt een query definiëren om de rijenset plat te maken of te filteren of voor het detecteren van gewijzigde documenten voor verdere gegevensvernieuwingsbewerkingen.|
|**Azure Blob Storage** | Vereisten zijn het opslagaccount en een container. Als blob-namen een virtuele naamconventie voor groeperingsdoeleinden volgen, kunt u desgewenst het gedeelte van de virtuele map van de naam als een map onder de container opgeven. Zie [Blob Storage indexeren (in preview-versie)](search-howto-indexing-azure-blob-storage.md) voor meer informatie. |
|**Azure-tabelopslag** | Vereisten zijn het opslagaccount en een tabelnaam. U kunt desgewenst een query opgeven om een subset van de tabellen op te halen. Zie [Table Storage indexeren (in preview-versie)](search-howto-indexing-azure-tables.md) voor meer informatie. |

## Doelindex aanpassen

Een voorlopige index is meestal afgeleid van de gegevensset. U kunt velden toevoegen, bewerken of verwijderen om het schema te voltooien. Stel daarnaast kenmerken op het veldniveau in om het daaropvolgende zoekgedrag te bepalen.

1. Geef in **Doelindex aanpassen** de naam en een **sleutel** op die als unieke identificatie van elk document worden gebruikt. De sleutel moet een tekenreeks zijn. Als veldwaarden spaties of streepjes bevatten, moet u geavanceerde opties instellen in **Gegevens importeren** om de validatiecontrole voor deze tekens te onderdrukken.

2. Controleer en wijzig indien nodig de resterende velden. De veldnaam en het veldtype worden doorgaans automatisch ingevuld. U kunt het gegevenstype wijzigen.

3. Stel indexkenmerken in voor elk veld:

 - Met Ophalen mogelijk wordt het veld in de zoekresultaten weergegeven.
 - Met Filterbaar kunt u in filterexpressies naar het veld verwijzen.
 - Met Sorteerbaar kan het veld in een sortering worden gebruikt.
 - Met Facetable (geschikt voor facetten) kunt u het veld gebruiken voor meervoudige navigatie.
 - Met Doorzoekbaar kunt u in het veld zoeken in volledige tekst.
  
4. Klik op het tabblad **Analyse** als u een taalanalyse op veldniveau wilt opgeven. Op dit moment kunnen alleen taalanalysefuncties worden opgegeven. Voor het gebruik van een aangepaste analysefunctie of een niet-taal-analysefunctie zoals sleutelwoord, patroon, enzovoort is code vereist.

 - Klik op **Doorzoekbaar** om te zoeken in volledige tekst in het veld en de vervolgkeuzelijst met analysefuncties in te schakelen.
 - Kies de gewenste analysefunctie. Zie [Een index voor documenten in meerdere talen maken](search-language-support.md) voor meer informatie.

5. Klik op **Suggesties** om aangevulde zoeksuggesties in de geselecteerde velden inschakelen.


## Uw gegevens importeren

1. Geef in **Gegevens importeren** een naam op voor de indexeerfunctie. Vergeet niet dat het product van de wizard Gegevens importeren een indexeerfunctie is. Als u deze later wilt bekijken of bewerken, selecteert u deze vanuit de portal in plaats van de wizard opnieuw uit te voeren. 

2. Geef het schema op, dat is gebaseerd op de regionale tijdzone waarin de service is geïmplementeerd.

3. Stel geavanceerde opties in om drempelwaarden op te geven over of het indexeren kan worden voortgezet als een document is verwijderd. Daarnaast kunt u opgeven of **Sleutel**-velden spaties en slashes mogen bevatten.  

## Een bestaande indexeerfunctie bewerken

Dubbelklik in het servicedashboard op de tegel Indexeerfunctie om een lijst met alle indexeerfuncties voor uw abonnement weer te geven. Dubbelklik op een van de indexeerfuncties om deze uit te voeren, te bewerken of te verwijderen. U kunt de index vervangen door een andere bestaande index, de gegevensbron wijzigen en opties instellen voor de drempelwaarden van fouten tijdens het indexeren.

## Een bestaande index bewerken

In Azure Search moet de index opnieuw worden gemaakt bij structurele updates voor de index. Het opnieuw maken van de index bestaat uit het verwijderen van de index, het opnieuw samenstellen van de index en het opnieuw laden van de gegevens. Structurele updates zijn onder andere het wijzigen van een gegevenstype en het wijzigen van de naam of verwijderen van een veld.

Bewerkingen waarvoor de index niet opnieuw hoeft te worden gemaakt, zijn onder andere het toevoegen van een nieuw veld, wijzigen van scoringsprofielen, wijziging van de suggestiefunctie en wijzigen van de taalanalysefunctie. Zie [Update Index](https://msdn.microsoft.com/library/azure/dn800964.aspx) (Index bijwerken) voor meer informatie.

## Volgende stap

Bekijk deze koppelingen voor meer informatie over indexeerfuncties:

- [Azure SQL Database indexeren](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers-2015-02-28.md)
- [DocumentDB indexeren](../documentdb/documentdb-search-indexer.md)
- [Blob Storage indexeren (in preview-versie)](search-howto-indexing-azure-blob-storage.md)
- [Table Storage indexeren (in preview-versie)](search-howto-indexing-azure-tables.md)



<!--Image references-->
[1]: ./media/search-import-data-portal/search-import-data-command.png




<!--HONumber=sep16_HO2-->


