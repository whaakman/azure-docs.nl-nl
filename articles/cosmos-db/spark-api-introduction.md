---
title: Inleiding tot de Spark voor Azure Cosmos DB API
description: Lees hoe u de Spark-API van Azure Cosmos DB kunt gebruiken om uit te voeren operationele analyse en AI
ms.service: cosmos-db
ms.topic: overview
ms.date: 05/06/2019
author: rimman
ms.author: rimman
ms.openlocfilehash: de920f40f2968942b7ac66414170b43bd9317cfb
ms.sourcegitcommit: 0ae3139c7e2f9d27e8200ae02e6eed6f52aca476
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65081092"
---
# <a name="introduction-to-the-azure-cosmos-db-spark-api-preview"></a>Inleiding tot Azure Cosmos DB Spark API (preview) 

De Spark-API in Azure Cosmos DB kunt u analyses van Apache Spark uitvoeren op basis van uw gegevens die zijn opgeslagen in een Azure Cosmos-account.

De Spark-API in Azure Cosmos DB biedt de systeemeigen ondersteuning voor Apache Spark-taken rechtstreeks op uw wereldwijd gedistribueerde Cosmos-databases worden uitgevoerd. Met deze mogelijkheden kunnen ontwikkelaars, gegevenstechnici en gegevenswetenschappers kunnen Azure Cosmos DB gebruiken als een flexibele, schaalbare en goed presterende gegevensplatform om uit te voeren beide **OLTP en OLAP-/ HTAP** werkbelastingen. 

> [!NOTE]
> Azure Cosmos DB Spark API is momenteel in de beperkte Preview-versie. Om u te registreren voor de Preview-versie, gaat u naar [Meld u aan voor de Preview-versie](https://aka.ms/cosmos-spark-preview) pagina. 

De Spark-API in Azure Cosmos DB biedt de volgende voordelen:

* Voor de geografisch verspreide gebruikers en gegevens kunt u de beste tijd om inzicht te krijgen.

* Kunt u de architectuur van de oplossing en de lagere vereenvoudigen de [totale eigendomskosten](total-cost-ownership.md) (TCO). Het systeem heeft het minste aantal onderdelen van de gegevensverwerking en voorkomt u eventuele overbodige gegevensverplaatsing daartussen.

* Hiermee maakt u een [security](secure-access-to-data.md), [naleving](compliance.md), en controle grensgroep die de gegevens onder beheer omvat.

* Biedt 'always on' of [maximaal beschikbare](high-availability.md) analytics voor eindgebruikers die worden ondersteund door strengere Sla's.

![Azure Cosmos DB-API voor Spark-visualisatie](./media/spark-api-introduction/spark-api-visualization.png)
 
Met behulp van de Spark-API van Azure Cosmos DB, kunt u bouwen en implementeren van oplossingen, zoals AI en deep learning-modellen, voorspellende analyses, aanbevelingen, IoT, customer 360, fraude te detecteren, tekst-sentiment, clickstream-analyse. Deze rechtstreeks in uw Azure Cosmos DB-gegevens werken.

U kunt batch instellen en streaming van ETL-taak in Azure Cosmos DB, zonder te hoeven gaan buiten de database-service of voegt u extra compute-services. U kunt de compute-omgeving elastisch schalen wanneer u nodig hebt voor ETL-taak uitvoeren en schalen van het weer omlaag wanneer de taak is voltooid.

De Spark-API in Azure Cosmos DB biedt ingebouwde ondersteuning voor Machine Learning in de Apache Spark-runtimes. De runtimes zijn Spark MLLib, Microsoft Machine Learning voor Spark, Azure Machine Learning en Cognitive Services. Met deze functies, kunnen gegevenswetenschappers, gegevensengineers en gegevensanalisten bouwen en machine learning-modellen rechtstreeks in Azure Cosmos DB, in een fractie van de tijd en met de lage kosten.


## <a name="key-benefits"></a>Belangrijkste voordelen

### <a name="globally-distributed-low-latency-operational-analytics-and-ai"></a>Wereldwijd gedistribueerde, lage latentie operationele analyse en AI

Met Apache Spark in de wereldwijd gedistribueerde Azure Cosmos-database, kunt u nu alles over de hele wereld snel tijd-tot-inzicht te krijgen. Azure Cosmos DB kunt **wereldwijd gedistribueerde, lage latentie, operationele analytics** op elastische schaal met drie belangrijke technieken:

* Aangezien uw Azure Cosmos-database is wereldwijd worden gedistribueerd, is het is dat alle gegevens lokaal opgenomen waar de producenten van de gegevens (bijvoorbeeld gebruikers) zich bevinden. De query's worden aangeboden op basis van de lokale replica's die het dichtst bij zowel de producenten en consumenten van gegevens, ongeacht waar deze in de hele wereld bevinden zich. 

* Uw analytische query's worden rechtstreeks op de geïndexeerde gegevens opgeslagen in de gegevenspartities zonder onnodige gegevens verplaatsingen uitgevoerd. 

* Omdat Spark geplaatst met Azure Cosmos DB is, plaatsvinden minder tussenliggende vertalingen en verplaatsingen van gegevens, wat resulteert in een betere prestaties en schaalbaarheid.

### <a name="unified-serverless-experience-for-apache-spark"></a>Serverloze uniforme voor Apache Spark

Als een multi-modeldatabase, Azure Cosmos DB nu breidt de ondersteuning van OSS-API's door te geven een **serverloze ervaring voor Apache Spark unified** met sleutel-waarde, document, grafiek, kolom-familie gegevensmodellen. Verschillende gegevensmodellen worden ondersteund met behulp van MongoDB, Cassandra, Gremlin, Etcd en SQL-API's - alle besturingssysteem op de onderliggende gegevens. 

Met de API voor Spark, kunt u systeemeigen ondersteuning bieden voor toepassingen die zijn geschreven in Scala, Python, Java en gebruik van verschillende geïntegreerd bibliotheken voor SQL. Deze bibliotheken bevatten ([Spark SQL](https://spark.apache.org/sql/)), machine learning-(Spark [MLlib](https://spark.apache.org/mllib/)), streamverwerking ([Spark Structured Streaming](https://spark.apache.org/streaming/)), en grafische verwerking (Spark [GraphFrames]( https://docs.databricks.com/spark/latest/graph-analysis/graphframes/user-guide-python.html)). Deze hulpprogramma's kunnen gemakkelijker gebruikmaken van de Spark-API voor een verscheidenheid aan gebruiksvoorbeelden. U hoeft te maken met het beheren van Spark of Spark-clusters. U kunt de vertrouwde Apache Spark-API's en **Jupyter-notebooks** voor analyse en SQL-API of een OSS-NoSQL-APIs zoals Cassandra voor transactionele verwerking op de onderliggende gegevens op hetzelfde moment.

### <a name="no-schema-or-index-management"></a>Geen schema- of indexbeheer

In tegenstelling tot traditionele analytische databases met Azure Cosmos DB, gegevenstechnici en gegevenswetenschappers niet meer nodig hebt om op te lossen omslachtig schema- en indexbeheer. De database-engine in Azure Cosmos DB hoeft u niet een expliciete beheer van schema's of indexbeheer en is geschikt voor alle gegevens die deze neemt snel behoeve van de Apache Spark-query's automatisch te indexeren. 

### <a name="consistency-choices"></a>Keuzes in consistentie

Omdat de Apache Spark-taken worden uitgevoerd in de gegevenspartities van uw Azure Cosmos-database, de query's krijgt de [vijf duidelijk gedefinieerde consistentiekeuzen](consistency-levels.md). Deze consistentiemodellen bieden de flexibiliteit om te kiezen strikte consistentie voor de meest nauwkeurige resultaten voor machine learning-algoritmen zonder verlies van de latentie en hoge beschikbaarheid. 

### <a name="slas"></a>SLA's

De Apache Spark-taken heeft de Azure Cosmos DB-voordelen, zoals de toonaangevende uitgebreide [Sla's](https://azure.microsoft.com/support/legal/sla/documentdb/v1_1/) (99,999) zonder eventuele overhead van het beheer van afzonderlijke Apache Spark-clusters... Deze Sla's omvatten doorvoer en latentie in het 99e percentiel, consistentie en beschikbaarheid. 

### <a name="mixed-workloads"></a>Gemengde workloads

De integratie van Apache Spark in Azure Cosmos DB bruggen de scheiding transactionele en analyse, dat is een van de belangrijke klant knelpunten die bij het bouwen van cloud-eigen Apps op wereldwijde schaal. 

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over de voordelen van Azure Cosmos DB, de [overzicht](introduction.md) artikel.
* [Aan de slag met de API van Azure Cosmos DB voor MongoDB](mongodb-introduction.md)
* [Aan de slag met de Cassandra-API van Azure Cosmos DB](cassandra-introduction.md)
* [Aan de slag met de Gremlin-API van Azure Cosmos DB](graph-introduction.md)
* [Aan de slag met de Table-API van Azure Cosmos DB](table-introduction.md)




