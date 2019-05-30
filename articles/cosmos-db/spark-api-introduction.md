---
title: Inleiding tot de ingebouwde operational analytics in Azure Cosmos DB met Apache Spark
description: Lees hoe u de ingebouwde ondersteuning voor Apache Spark in Azure Cosmos DB kunt gebruiken om uit te voeren operationele analyse en AI
ms.service: cosmos-db
ms.topic: overview
ms.date: 05/23/2019
author: rimman
ms.author: rimman
ms.openlocfilehash: b392f7fd6438b25a741aecb86a72f142d785f0e3
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2019
ms.locfileid: "66237897"
---
# <a name="built-in-operational-analytics-in-azure-cosmos-db-with-apache-spark-preview"></a>Ingebouwde operational analytics in Azure Cosmos DB met Apache Spark (preview) 

De ingebouwde ondersteuning voor Apache Spark in Azure Cosmos DB kunt u analyses van Apache Spark uitvoeren op basis van uw gegevens die zijn opgeslagen in een Azure Cosmos-account. Het biedt de systeemeigen ondersteuning voor Apache Spark-taken rechtstreeks op uw wereldwijd gedistribueerde Cosmos-databases worden uitgevoerd. Met deze mogelijkheden kunnen ontwikkelaars, gegevenstechnici en gegevenswetenschappers kunnen Azure Cosmos DB gebruiken als een flexibele, schaalbare en goed presterende gegevensplatform om uit te voeren beide **OLTP en OLAP-/ HTAP** werkbelastingen. 

Spark compute is automatisch beschikbaar in alle Azure-regio's die zijn gekoppeld aan uw Azure Cosmos-account. Spark-taken met meerdere masters mogelijkheid van de Azure Cosmos DB gebruiken en ze kunnen schrijven of query op basis van de lokale replica's in elke regio. 

> [!NOTE]
> De ingebouwde ondersteuning voor Apache Spark in Azure Cosmos DB is momenteel in de beperkte Preview-versie. Om u te registreren voor de Preview-versie, gaat u naar [Meld u aan voor de preview-pagina](https://portal.azure.com/?feature.customportal=false#create/Microsoft.DocumentDB). 

De ondersteuning voor Apache Spark in Azure Cosmos DB biedt de volgende voordelen:

* Voor de geografisch verspreide gebruikers en gegevens kunt u de beste tijd om inzicht te krijgen.

* Kunt u de architectuur van de oplossing en de lagere vereenvoudigen de [totale eigendomskosten](total-cost-ownership.md) (TCO). Het systeem heeft het minste aantal onderdelen van de gegevensverwerking en voorkomt u eventuele overbodige gegevensverplaatsing daartussen.

* Hiermee maakt u een [security](secure-access-to-data.md), [naleving](compliance.md), en controle grensgroep die de gegevens onder beheer omvat.

* Biedt 'always on' of [maximaal beschikbare](high-availability.md) analytics voor eindgebruikers die worden ondersteund door strengere Sla's.

![Ondersteuning voor Apache Spark in Azure Cosmos DB-visualisatie](./media/spark-api-introduction/spark-api-visualization.png)
 
Met de ondersteuning voor Apache Spark in Azure Cosmos DB, kunt u bouwen en implementeren van oplossingen, zoals AI en deep learning-modellen, voorspellende analyses, aanbevelingen, IoT, customer 360, fraude te detecteren, tekst-sentiment, clickstream-analyse. Deze oplossingen worden toegepast rechtstreeks op uw Azure Cosmos DB-gegevens.

U kunt batch instellen en streaming van ETL-taak in Azure Cosmos DB, zonder te hoeven gaan buiten de database-service of voegt u extra compute-services. U kunt de compute-omgeving elastisch schalen wanneer u nodig hebt voor ETL-taak uitvoeren en schalen van het weer omlaag wanneer de taak is voltooid.

De ondersteuning voor Apache Spark in Azure Cosmos DB biedt ingebouwde ondersteuning voor Machine Learning in de Apache Spark-runtimes. De runtimes zijn Spark MLLib, Microsoft Machine Learning voor Spark, Azure Machine Learning en Cognitive Services. Met deze functies, kunnen gegevenswetenschappers, gegevensengineers en gegevensanalisten bouwen en machine learning-modellen rechtstreeks in Azure Cosmos DB, in een fractie van de tijd en met de lage kosten.


## <a name="key-benefits"></a>Belangrijkste voordelen

### <a name="globally-distributed-low-latency-operational-analytics-and-ai"></a>Wereldwijd gedistribueerde, lage latentie operationele analyse en AI

Met Apache Spark in de wereldwijd gedistribueerde Azure Cosmos-database, kunt u nu alles over de hele wereld snel tijd-tot-inzicht te krijgen. Azure Cosmos DB kunt **wereldwijd gedistribueerde, lage latentie, operationele analytics** op elastische schaal met drie belangrijke technieken:

* Aangezien uw Azure Cosmos-database is wereldwijd worden gedistribueerd, is het is dat alle gegevens lokaal opgenomen waar de producenten van de gegevens (bijvoorbeeld gebruikers) zich bevinden. De query's worden aangeboden op basis van de lokale replica's die het dichtst bij zowel de producenten en consumenten van gegevens, ongeacht waar deze in de hele wereld bevinden zich. 

* Uw analytische query's worden rechtstreeks op de geïndexeerde gegevens opgeslagen in de gegevenspartities zonder onnodige gegevens verplaatsingen uitgevoerd. 

* Omdat Spark geplaatst met Azure Cosmos DB is, plaatsvinden minder tussenliggende vertalingen en verplaatsingen van gegevens, wat resulteert in een betere prestaties en schaalbaarheid.

### <a name="unified-serverless-experience-for-apache-spark"></a>Serverloze uniforme voor Apache Spark

Als een multi-modeldatabase, Azure Cosmos DB nu breidt de ondersteuning van OSS-API's door te geven een **serverloze ervaring voor Apache Spark unified** met sleutel-waarde, document, grafiek, kolom-familie gegevensmodellen. Verschillende gegevensmodellen worden ondersteund met behulp van MongoDB, Cassandra, Gremlin, Etcd en SQL-API's - alle besturingssysteem op de onderliggende gegevens. 

Met de ondersteuning voor Apache Spark in Azure Cosmos DB, kunt u systeemeigen ondersteuning bieden voor toepassingen die zijn geschreven in Scala, Python, Java en gebruik van verschillende geïntegreerd bibliotheken voor SQL. Deze bibliotheken bevatten ([Spark SQL](https://spark.apache.org/sql/)), machine learning-(Spark [MLlib](https://spark.apache.org/mllib/)), streamverwerking ([Spark Structured Streaming](https://spark.apache.org/streaming/)), en grafische verwerking (Spark [GraphFrames]( https://docs.databricks.com/spark/latest/graph-analysis/graphframes/user-guide-python.html)). Deze hulpprogramma's kunnen gemakkelijker gebruikmaken van de Apache Spark voor een verscheidenheid aan gebruiksvoorbeelden. U hoeft te maken met het beheren van Spark of Spark-clusters. U kunt de vertrouwde Apache Spark-API's en **Jupyter-notebooks** voor analyse en SQL-API of een OSS-NoSQL-APIs zoals Cassandra voor transactionele verwerking op de onderliggende gegevens op hetzelfde moment.

### <a name="no-schema-or-index-management"></a>Geen schema- of indexbeheer

In tegenstelling tot traditionele analytische databases met Azure Cosmos DB, gegevenstechnici en gegevenswetenschappers niet meer nodig hebt om op te lossen omslachtig schema- en indexbeheer. De database-engine in Azure Cosmos DB hoeft u niet een expliciete beheer van schema's of indexbeheer en is geschikt voor alle gegevens die deze neemt snel behoeve van de Apache Spark-query's automatisch te indexeren. 

### <a name="consistency-choices"></a>Keuzes in consistentie

Omdat de Apache Spark-taken worden uitgevoerd in de gegevenspartities van uw Azure Cosmos-database, de query's krijgt de [vijf duidelijk gedefinieerde consistentiekeuzen](consistency-levels.md). Deze consistentiemodellen bieden de flexibiliteit om te kiezen strikte consistentie voor de meest nauwkeurige resultaten voor machine learning-algoritmen zonder verlies van de latentie en hoge beschikbaarheid. 

### <a name="comprehensive-slas"></a>Uitgebreide SLA's

De Apache Spark-taken heeft de Azure Cosmos DB-voordelen, zoals de toonaangevende uitgebreide [Sla's](https://azure.microsoft.com/support/legal/sla/documentdb/v1_1/) (99,999) zonder eventuele overhead van het beheer van afzonderlijke Apache Spark-clusters... Deze Sla's omvatten doorvoer en latentie in het 99e percentiel, consistentie en beschikbaarheid. 

### <a name="mixed-workloads"></a>Gemengde workloads

De integratie van Apache Spark in Azure Cosmos DB bruggen de scheiding transactionele en analyse, dat is een van de belangrijke klant knelpunten die bij het bouwen van cloud-eigen Apps op wereldwijde schaal. 

## <a name="built-in-jupyter-notebooks-support"></a>Ingebouwde ondersteuning voor Jupyter-notebooks

Azure Cosmos DB biedt ondersteuning voor ingebouwde Jupyter-notebooks voor alle API's, zoals Cassandra, MongoDB, SQL, Gremlin en tabel. De Jupyter-notebooks uitgevoerd in de Azure Cosmos-accounts en ze verbeteren de ervaring voor ontwikkelaars. De laptop ingebouwde ondersteuning voor alle Azure Cosmos DB-API's en gegevensmodellen kunt u interactief uitvoeren van query's. U kunt ook machine learning-modellen uitvoeren en analyseren van de gegevens die zijn opgeslagen in uw Azure Cosmos-databases. Met behulp van de Jupyter-notebook-ervaring, kunt u de opgeslagen gegevens te analyseren, maken en machine learning-modellen trainen en inferentietaken uitvoeren voor de gegevens in Azure portal, zoals wordt weergegeven in de volgende afbeelding:

![Ondersteuning voor Jupyter-notebooks in Azure Cosmos DB](./media/spark-api-introduction/jupyter-notebooks-portal.png)

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over de voordelen van Azure Cosmos DB, de [overzicht](introduction.md) artikel.
* [Aan de slag met de API van Azure Cosmos DB voor MongoDB](mongodb-introduction.md)
* [Aan de slag met de Cassandra-API van Azure Cosmos DB](cassandra-introduction.md)
* [Aan de slag met de Gremlin-API van Azure Cosmos DB](graph-introduction.md)
* [Aan de slag met de Table-API van Azure Cosmos DB](table-introduction.md)




