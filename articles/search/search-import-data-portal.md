---
title: Gegevens importeren in search-index met behulp van Azure portal - Azure Search
description: Informatie over het gebruik van de wizard gegevens importeren in Azure portal voor het verkennen van Azure-gegevens van Cosmos DB, Blob storage, table-opslag, SQL-Database en SQL Server op Azure Virtual machines.
author: HeidiSteen
manager: cgronlun
services: search
ms.service: search
ms.topic: conceptual
ms.date: 07/10/2018
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: ceca9b8e89a963cd9a9226be143d24ed5429747b
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53316469"
---
# <a name="how-to-import-data-into-azure-search-index-using-the-azure-portal"></a>Gegevens importeren in Azure Search-index met behulp van de Azure portal

In het Azure Search-dashboard van Azure Portal vindt u de wizard **Gegevens importeren** waarmee u gegevens in een index kunt laden. 

  ![Gegevens importeren in de opdrachtbalk][1]

Intern configureert de wizard een *indexeerfunctie* en roept deze aan, waarmee verschillende stappen van het indexeringsproces worden geautomatiseerd: 

* Verbinding maken met een externe gegevensbron in hetzelfde Azure-abonnement
* Een te wijzigen indexschema maken op basis van de brongegevensstructuur
* JSON-documenten in een index laden met behulp van een rijenset die is opgehaald uit de gegevensbron

> [!NOTE]
> U kunt de wizard **Gegevens importeren** laden vanuit het dashboard Azure Cosmos DB om indexering voor die gegevensbron te vereenvoudigen. Ga in het linkernavigatiedeelvenster naar **Verzamelingen** > **Azure Search toevoegen** om aan de slag te gaan.

## <a name="data-sources-supported-by-the-import-data-wizard"></a>Gegevensbronnen die worden ondersteund door de wizard Gegevens importeren
De wizard Gegevens importeren ondersteunt de volgende gegevensbronnen: 

* Azure SQL Database
* Relationele SQL Server-gegevens op een virtuele machine van Azure
* Azure Cosmos DB
* Azure Blob Storage
* Azure Table Storage

Een platte gegevensset is een vereiste invoer. U kunt slechts importeren uit één tabel, databaseweergave of gelijkwaardige gegevensstructuur. U moet deze gegevensstructuur maken voordat u de wizard uitvoert.

## <a name="connect-to-your-data"></a>Verbinding maken met uw gegevens
1. Meld u aan bij [Azure Portal](https://portal.azure.com) en open het servicedashboard. Klik in de koppelingsbalk op **Alle services** als u in het huidige abonnement naar bestaande 'zoekservices' wilt zoeken. 

1. Klik op **Gegevens importeren** in de opdrachtbalk om de blade Gegevens importeren te openen.

1. Klik op **Verbinden met uw gegevens** om een gegevensbrondefinitie op te geven die door een indexeerfunctie wordt gebruikt. Voor gegevensbronnen binnen het abonnement kan de wizard meestal verbindingsgegevens detecteren en lezen, zodat de algehele configuratievereisten minimaal zijn.

|  |  |
| --- | --- |
| **Bestaande gegevensbron** |Als u al indexeerfuncties hebt gedefinieerd in uw zoekservice, kunt u een bestaande gegevensbrondefinitie voor een andere import selecteren. |
| **Azure SQL Database** |De servicenaam, referenties voor een databasegebruiker met leesmachtiging en de naam van een database kunnen worden opgegeven op de pagina of via een ADO.NET-verbindingsreeks. Kies de verbindingsreeksoptie om eigenschappen te bekijken of aan te passen. <br/><br/>De tabel of weergave die de rijenset bevat, moet worden opgegeven op de pagina. Deze optie wordt weergegeven nadat de verbinding tot stand is gebracht, waarna een vervolgkeuzelijst wordt weergegeven zodat u een selectie kunt maken. |
| **SQL Server op virtuele Azure-machine** |Geef een FQDN-servicenaam, gebruikers-ID en wachtwoord en de database als een verbindingsreeks op. Voor het gebruik van deze gegevensbron moet u eerder een certificaat hebben geïnstalleerd in het lokale archief dat de verbinding versleutelt. Zie [SQL VM-verbinding met Azure Search](search-howto-connecting-azure-sql-iaas-to-azure-search-using-indexers.md) voor instructies. <br/><br/>De tabel of weergave die de rijenset bevat, moet worden opgegeven op de pagina. Deze optie wordt weergegeven nadat de verbinding tot stand is gebracht, waarna een vervolgkeuzelijst wordt weergegeven zodat u een selectie kunt maken. |
| **Azure Cosmos DB** |Vereisten zijn het account, de database en de verzameling. Alle documenten in de verzameling worden opgenomen in de index. U kunt een query definiëren om de rijenset plat te maken of te filteren of voor het detecteren van gewijzigde documenten voor verdere gegevensvernieuwingsbewerkingen. |
| **Azure Blob Storage** |Vereisten zijn het opslagaccount en een container. Als blob-namen een virtuele naamconventie voor groeperingsdoeleinden volgen, kunt u desgewenst het gedeelte van de virtuele map van de naam als een map onder de container opgeven. Zie [Blob Storage indexeren](search-howto-indexing-azure-blob-storage.md) voor meer informatie. |
| **Azure Table Storage** |Vereisten zijn het opslagaccount en een tabelnaam. U kunt desgewenst een query opgeven om een subset van de tabellen op te halen. Zie [Table Storage indexeren](search-howto-indexing-azure-tables.md) voor meer informatie. |

## <a name="customize-target-index"></a>Doelindex aanpassen
Een voorlopige index is meestal afgeleid van de gegevensset. U kunt velden toevoegen, bewerken of verwijderen om het schema te voltooien. Stel daarnaast kenmerken op het veldniveau in om het daaropvolgende zoekgedrag te bepalen.

1. Geef in **Doelindex aanpassen** de naam en een **sleutel** op die als unieke identificatie van elk document worden gebruikt. De sleutel moet een tekenreeks zijn. Als veldwaarden spaties of streepjes bevatten, moet u geavanceerde opties instellen in **Gegevens importeren** om de validatiecontrole voor deze tekens te onderdrukken.

1. Controleer en wijzig indien nodig de resterende velden. De veldnaam en het veldtype worden doorgaans automatisch ingevuld. U kunt het gegevenstype wijzigen totdat de index is gemaakt. Als u het daarna nog verandert, moet het opnieuw worden opgebouwd.

1. Stel indexkenmerken in voor elk veld:
   
   * Met Ophalen mogelijk wordt het veld in de zoekresultaten weergegeven.
   * Met Filterbaar kunt u in filterexpressies naar het veld verwijzen.
   * Met Sorteerbaar kan het veld in een sortering worden gebruikt.
   * Met Facetable (geschikt voor facetten) kunt u het veld gebruiken voor meervoudige navigatie.
   * Met Doorzoekbaar kunt u in het veld zoeken in volledige tekst.

1. Klik op het tabblad **Analyse** als u een taalanalyse op veldniveau wilt opgeven. Op dit moment kunnen alleen taalanalysefuncties worden opgegeven. Voor het gebruik van een aangepaste analysefunctie of een niet-taal-analysefunctie zoals sleutelwoord, patroon, enzovoort is code vereist.
   
   * Klik op **Doorzoekbaar** om te zoeken in volledige tekst in het veld en de vervolgkeuzelijst met analysefuncties in te schakelen.
   * Kies de gewenste analysefunctie. Zie [Een index voor documenten in meerdere talen maken](search-language-support.md) voor meer informatie.

1. Klik op **Suggesties** om aangevulde zoeksuggesties in de geselecteerde velden inschakelen.

## <a name="import-your-data"></a>Uw gegevens importeren
1. Geef in **Gegevens importeren** een naam op voor de indexeerfunctie. Vergeet niet dat het product van de wizard Gegevens importeren een indexeerfunctie is. Als u deze later wilt bekijken of bewerken, selecteert u deze vanuit de portal in plaats van de wizard opnieuw uit te voeren. 

1. Geef het schema op, dat is gebaseerd op de regionale tijdzone waarin de service is geïmplementeerd.

1. Stel geavanceerde opties in om drempelwaarden op te geven over of het indexeren kan worden voortgezet als een document is verwijderd. Daarnaast kunt u opgeven of **Sleutel**-velden spaties en slashes mogen bevatten.  

1. Klik op **OK** om de index te maken en de gegevens te importeren.

U kunt de indexering in de portal bewaken. Naarmate meer documenten worden geladen, neemt het aantal documenten toe voor de index die u hebt gedefinieerd. Soms duurt het enkele minuten voordat de portalpagina de meest recente updates heeft opgehaald.

De index kan worden bevraagd zodra alle documenten zijn geladen.

## <a name="query-an-index-using-search-explorer"></a>Query uitvoeren in een index met behulp van Search explorer

De portal bevat **Search explorer** zodat u een index opvragen kunt zonder code te schrijven. U kunt [Search explorer](search-explorer.md) op elke index toepassen.

De zoekervaring is gebaseerd op standaardinstellingen, zoals de [eenvoudige syntaxis](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) en de standaard-queryparameter [searchMode](https://docs.microsoft.com/rest/api/searchservice/search-documents). 

Resultaten worden geretourneerd in JSON, in uitgebreide vorm, zodat u het hele document kunt inspecteren.

## <a name="edit-an-existing-indexer"></a>Een bestaande indexeerfunctie bewerken
Zoals aangegeven, maakt de wizard Gegevens importeren een **indexeerfunctie**, die u als een zelfstandige constructie in de portal kunt wijzigen.

Dubbelklik in het servicedashboard op de tegel Indexeerfunctie om een lijst met alle indexeerfuncties voor uw abonnement weer te geven. Dubbelklik op een van de indexeerfuncties om deze uit te voeren, te bewerken of te verwijderen. U kunt de index vervangen door een andere bestaande index, de gegevensbron wijzigen en opties instellen voor de drempelwaarden van fouten tijdens het indexeren.

## <a name="edit-an-existing-index"></a>Een bestaande index bewerken
De wizard heeft ook een **index** gemaakt. In Azure Search moet een index opnieuw worden opgebouwd na structurele updates van die index. Het opnieuw opbouwen van een index houdt in dat de index opnieuw wordt gemaakt aan de hand van een herzien schema dat de gewenste wijzigingen bevat, en dat gegevens opnieuw worden geladen. Structurele updates zijn onder andere het wijzigen van een gegevenstype en het wijzigen van de naam of verwijderen van een veld.

Bewerkingen waarvoor de index niet opnieuw hoeft te worden gemaakt, zijn onder andere het toevoegen van een nieuw veld, wijzigen van scoringsprofielen, wijziging van de suggestiefunctie en wijzigen van de taalanalysefunctie. Zie [Update Index](https://msdn.microsoft.com/library/azure/dn800964.aspx) (Index bijwerken) voor meer informatie.


## <a name="next-steps"></a>Volgende stappen
Bekijk deze koppelingen voor meer informatie over indexeerfuncties:

* [Azure SQL Database indexeren](search-howto-connecting-azure-sql-database-to-azure-search-using-indexers.md)
* [Azure Cosmos DB indexeren](search-howto-index-cosmosdb.md)
* [Blob Storage indexeren](search-howto-indexing-azure-blob-storage.md)
* [Table Storage indexeren](search-howto-indexing-azure-tables.md)

<!--Image references-->
[1]: ./media/search-import-data-portal/search-import-data-command.png

