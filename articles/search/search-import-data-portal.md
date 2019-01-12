---
title: Gegevens importeren in een search-index met behulp van Azure portal - Azure Search
description: Informatie over het gebruik van de wizard gegevens importeren in Azure portal voor het verkennen van Azure-gegevens van Cosmos DB, Blob storage, table-opslag, SQL-Database en SQL Server op Azure Virtual machines.
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.topic: conceptual
ms.date: 01/10/2019
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: ee1b2a40dbcbd53a758ac71f30401778ef07e872
ms.sourcegitcommit: a512360b601ce3d6f0e842a146d37890381893fc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/11/2019
ms.locfileid: "54229754"
---
# <a name="import-data-wizard-for-azure-search"></a>De wizard gegevens importeren voor Azure Search

In het Azure Search-dashboard van Azure Portal vindt u de wizard **Gegevens importeren** waarmee u gegevens in een index kunt laden. Achter de schermen, de wizard configureert en roept een *gegevensbron*, *index*, en *indexeerfunctie* -waarmee verschillende stappen van het indexeringsproces worden geautomatiseerd: 

* Maakt verbinding met een externe gegevensbron in hetzelfde Azure-abonnement.
* (Optioneel) kan worden geïntegreerd optische tekenherkenning of natuurlijke taal verwerken om tekst uit niet-gestructureerde gegevens.
* Genereert een index op basis van steekproeven te nemen en metagegevens van de externe gegevensbron.
* Verkent de gegevensbron voor doorzoekbare inhoud samenvoegt, serialiseren en JSON-documenten in de index te laden.

De wizard kan geen verbinding maken met een vooraf gedefinieerde index of een bestaande indexeerfunctie uitvoeren, maar in de wizard kunt u een nieuwe index of de indexeerfunctie ter ondersteuning van de structuur en het gedrag die u moet configureren.

Bent u niet bekend met Azure Search? Doorloop de [Quick Start: Importeren, indexeren en query's uitvoeren met behulp van portal-hulpprogramma's](search-get-started-portal.md) ze kunnen uitproberen importeren en indexeren met **gegevens importeren** en de voorbeeldgegevensset ingebouwde onroerend goed.

## <a name="start-importing-data"></a>Gegevens importeren starten

Deze sectie wordt uitgelegd hoe u de wizard starten en bevat een overzicht van elke stap.

1. In de [Azure-portal](https://portal.azure.com), de zoekpagina service openen vanuit het dashboard of [Zoek uw service](https://ms.portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices) in de lijst met Services.

2. Klik in de overzichtspagina van service aan de bovenkant op **gegevens importeren**.

   ![De gegevensopdracht in de portal importeren](./media/search-import-data-portal/import-data-cmd2.png "Start de wizard gegevens importeren")

   > [!NOTE]
   > U kunt starten **gegevens importeren** van andere Azure-services, met inbegrip van Azure Cosmos DB, Azure SQL Database en Azure Blob-opslag. Zoek naar **Azure Search toevoegen** in het deelvenster navigatie aan de linkerkant op de overzichtspagina van de service.

3. De wizard wordt geopend op **verbinding maken met uw gegevens**, waar u kunt ervoor kiezen een externe gegevensbron moet worden gebruikt voor deze importbewerking. Er zijn verschillende dingen die u moet weten over deze stap, dus zorg ervoor dat u lees de [gegevensbron invoer](#data-source-inputs) sectie voor meer informatie.

   ![De wizard gegevens importeren in de portal](./media/search-import-data-portal/import-data-wizard-startup.png "wizard importeren voor Azure Search")

4. Hierna volgt **toevoegen cognitief zoeken**, als u wilt opnemen van optische tekenherkenning (OCR) van de tekst in afbeeldingsbestanden of tekstanalyse niet-gestructureerde gegevens. AI-algoritmen van Cognitive Services worden opgehaald voor deze taak. Er zijn twee onderdelen met deze stap:
  
   Eerste, [een Cognitive Services-resource koppelen](cognitive-search-attach-cognitive-services.md) naar een Azure Search-vaardigheden.
  
   Ten tweede kiezen welke enrichments AI om op te nemen in de vaardigheden. Zie voor een demonstratie scenario [snelstartgids](cognitive-search-quickstart-blob.md).

   Als u alleen gegevens importeren wilt, sla deze stap over en gaat u rechtstreeks naar de definitie van de index.

5. Hierna volgt **doelindex aanpassen**, waar u kunt Accepteer of wijzig het schema van de index die zijn gepresenteerd in de wizard. De wizard bepaalt de velden en gegevenstypen door steekproeven van gegevens en het lezen van de metagegevens van de externe gegevensbron.

   Voor elk veld [controleren van de indexkenmerken](#index-definition) om in te schakelen specifieke gedrag. Als u niet alle kenmerken selecteert, is de index niet kan worden gebruikt. 

6. Hierna volgt **een indexeerfunctie maken**, dit is een product van deze wizard. Een indexeerfunctie is een Verkenner die doorzoekbare gegevens en metagegevens geëxtraheerd uit een externe Azure-gegevensbron. Door te selecteren van de gegevensbron en koppelen van de kennis en vaardigheden (optioneel) en een index, hebt u zijn configureren van een indexeerfunctie terwijl u elke stap van de wizard hebt doorlopen.

   Geef een naam op voor de indexeerfunctie en klikt u op **indienen** om het importproces te starten. 

U kunt controleren in de portal voor indexering door te klikken op de indexeerfunctie in de **indexeerfuncties** lijst. Naarmate meer documenten worden geladen, neemt het aantal documenten toe voor de index die u hebt gedefinieerd. Soms duurt het enkele minuten voordat de portalpagina de meest recente updates heeft opgehaald.

De index kan worden bevraagd zodra het eerste document wordt geladen. U kunt [Search explorer](search-explorer.md) voor deze taak.

<a name="data-source-inputs"></a>

## <a name="data-source-inputs"></a>Gegevensinvoer bron

De **gegevens importeren** wizard maakt een permanente gegevensbronobject verbindingsgegevens voor een externe gegevensbron op te geven. Het gegevensbronobject wordt gebruikt met [indexeerfuncties](search-indexer-overview.md) en kunnen worden gemaakt voor de volgende gegevensbronnen: 

* [Azure SQL](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)
* [Azure Cosmos DB](search-howto-index-cosmosdb.md)
* [Azure Blob Storage](search-howto-indexing-azure-blob-storage.md)
* [Azure Table Storage](search-howto-indexing-azure-tables.md) (niet ondersteund voor [cognitief zoeken](cognitive-search-concept-intro.md) pijplijnen)

Een platte gegevensset is een vereiste invoer. U kunt slechts importeren uit één tabel, databaseweergave of gelijkwaardige gegevensstructuur. U moet deze gegevensstructuur maken voordat u de wizard uitvoert.

|  Selectie | Description |
| ---------- | ----------- |
| **Bestaande gegevensbron** |Als u al indexeerfuncties hebt gedefinieerd in uw zoekservice, kunt u een bestaande gegevensbrondefinitie voor een andere import selecteren. In Azure Search worden alleen gegevensbronobjecten gebruikt door indexeerfuncties. U kunt object voor een gegevensbron via een programma maken of via de **gegevens importeren** wizard.|
| **Voorbeelden**| Azure Search als host fungeert voor een gratis openbare Azure SQL database die u gebruiken kunt voor meer informatie over aanvragen voor importeren en query's uitvoeren in Azure Search. Zie [Quickstart: Importeren, indexeren en query's uitvoeren met behulp van portal-hulpprogramma's](search-get-started-portal.md) voor een overzicht. |
| **Azure SQL Database** |De servicenaam, referenties voor een databasegebruiker met leesmachtiging en de naam van een database kunnen worden opgegeven op de pagina of via een ADO.NET-verbindingsreeks. Kies de verbindingsreeksoptie om eigenschappen te bekijken of aan te passen. <br/><br/>De tabel of weergave die de rijenset bevat, moet worden opgegeven op de pagina. Deze optie wordt weergegeven nadat de verbinding tot stand is gebracht, waarna een vervolgkeuzelijst wordt weergegeven zodat u een selectie kunt maken. |
| **SQL Server op virtuele Azure-machine** |Geef een volledig gekwalificeerde servicenaam, gebruikers-ID en wachtwoord en -database als een verbindingsreeks. Voor het gebruik van deze gegevensbron moet u eerder een certificaat hebben geïnstalleerd in het lokale archief dat de verbinding versleutelt. Zie [SQL VM-verbinding met Azure Search](search-howto-connecting-azure-sql-iaas-to-azure-search-using-indexers.md) voor instructies. <br/><br/>De tabel of weergave die de rijenset bevat, moet worden opgegeven op de pagina. Deze optie wordt weergegeven nadat de verbinding tot stand is gebracht, waarna een vervolgkeuzelijst wordt weergegeven zodat u een selectie kunt maken. |
| **Azure Cosmos DB** |Vereisten zijn het account, de database en de verzameling. Alle documenten in de verzameling worden opgenomen in de index. U kunt een query definiëren om de rijenset plat te maken of te filteren of voor het detecteren van gewijzigde documenten voor verdere gegevensvernieuwingsbewerkingen. |
| **Azure Blob Storage** |Vereisten zijn het opslagaccount en een container. Als blob-namen een virtuele naamconventie voor groeperingsdoeleinden volgen, kunt u desgewenst het gedeelte van de virtuele map van de naam als een map onder de container opgeven. Zie [Blob Storage indexeren](search-howto-indexing-azure-blob-storage.md) voor meer informatie. |
| **Azure Table Storage** |Vereisten zijn het opslagaccount en een tabelnaam. U kunt desgewenst een query opgeven om een subset van de tabellen op te halen. Zie [Table Storage indexeren](search-howto-indexing-azure-tables.md) voor meer informatie. |


<a name="index-definition"></a>

## <a name="index-attributes"></a>Indexkenmerken

De **gegevens importeren** wizard genereert een index, die wordt gevuld met documenten afkomstig is van de bron van de ingevoerde gegevens. 

Zorg ervoor dat u hebt de volgende elementen die zijn gedefinieerd voor een functionele index.

1. Één veld moet worden gemarkeerd als een **sleutel**, die wordt gebruikt voor het aanduiden van elk document. De **sleutel** moet *Edm.string*. 

   Als veldwaarden spaties of streepjes bevatten, moet u instellen de **base64 coderen sleutel** optie in de **een indexeerfunctie maken** stap onder **geavanceerde opties**, moet worden onderdrukt de de validatiecontrole voor deze tekens.

1. Stel indexkenmerken in voor elk veld. Als u geen kenmerken selecteert, is de index is in feite leeg is, met uitzondering van het vereiste veld. Kies ten minste één of meer van deze kenmerken voor elk veld.
   
   + **Ophalen mogelijk** wordt het veld in de zoekresultaten. Elk veld waarmee inhoud om te zoeken naar resultaten moet dit kenmerk hebben. Instellen van dit veld heeft geen merkbaar indexgrootte invloed.
   + **Filterbaar** kan het veld in filterexpressies naar worden verwezen. Elk veld dat wordt gebruikt een **$filter** -expressie moet dit kenmerk hebben. Er zijn Filterexpressies voor exacte overeenkomsten. Omdat tekenreeksen intact blijven, wordt extra opslag vereist om de verbatim inhoud mogelijk te maken.
   + **Doorzoekbare** zoeken in volledige tekst. Elk veld dat wordt gebruikt in vrije vorm van query's of in query-expressies moet dit kenmerk hebben. Omgekeerde indexen worden gemaakt voor elk veld dat u als markeren **doorzoekbaar**.

1. (Optioneel), stelt u deze kenmerk indien nodig:

   + **Sorteerbaar** kan het veld in een sortering worden gebruikt. Elk veld dat wordt gebruikt een **$Orderby** -expressie moet dit kenmerk hebben.
   + **Geschikt voor facetten** kunt u het veld voor meervoudige navigatie. Alleen velden ook gemarkeerd als **filteren mogelijk** kan worden gemarkeerd als **geschikt voor facetten**.

1. Stel een **Analyzer** als u wilt dat taal uitgebreid indexeren en query's. De standaardwaarde is *Standard Lucene* , maar u kunt ervoor kiezen *Microsoft English* als u wilt voor het gebruik van Microsoft-analyzer voor geavanceerde lexicale verwerkingsbronnen, zoals het oplossen van onregelmatige formulieren voor het zelfstandig naamwoord en bewerking.

   + Selecteer **doorzoekbaar** om in te schakelen de **Analyzer** lijst.
   + Kies een analyzer opgegeven in de lijst. 
   
   Op dit moment kunnen alleen taalanalysefuncties worden opgegeven. Voor het gebruik van een aangepaste analysefunctie of een niet-taal-analysefunctie zoals sleutelwoord, patroon, enzovoort is code vereist. Zie voor meer informatie over de analyzers [maken van een index voor documenten in meerdere talen](search-language-support.md).

1. Selecteer de **suggestie** het selectievakje in om in te schakelen, automatisch aangevulde Querysuggesties op geselecteerde velden.


## <a name="next-steps"></a>Volgende stappen
Bekijk deze koppelingen voor meer informatie over indexeerfuncties:

* [Azure SQL Database indexeren](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)
* [Azure Cosmos DB indexeren](search-howto-index-cosmosdb.md)
* [Blob Storage indexeren](search-howto-indexing-azure-blob-storage.md)
* [Table Storage indexeren](search-howto-indexing-azure-tables.md)