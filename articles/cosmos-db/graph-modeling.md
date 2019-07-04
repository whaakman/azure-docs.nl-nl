---
title: Graph-gegevensmodellering voor Gremlin-API van Azure Cosmos DB
description: Leer hoe u een grafiekdatabase met behulp van Gremlin-API voor Cosmos DB model.
author: LuisBosquez
ms.service: cosmos-db
ms.subservice: cosmosdb-graph
ms.topic: overview
ms.date: 06/24/2019
ms.author: lbosq
ms.openlocfilehash: c6ae23efa90874bbefc2aff35f8798aa6c0da791
ms.sourcegitcommit: 837dfd2c84a810c75b009d5813ecb67237aaf6b8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2019
ms.locfileid: "67503418"
---
# <a name="graph-data-modeling-for-azure-cosmos-db-gremlin-api"></a>Graph-gegevensmodellering voor Gremlin-API van Azure Cosmos DB

Het volgende document is zodanig ontworpen dat de grafiekgegevens zijn aanbevelingen modelleren. Deze stap is het essentieel om ervoor te zorgen de schaalbaarheid en prestaties van een systeem graph-database als de gegevens zich verder ontwikkelt. Een efficiënte gegevensmodel is vooral belangrijk bij grootschalige grafieken.

## <a name="requirements"></a>Vereisten

Het proces wordt beschreven in deze handleiding is gebaseerd op de volgende veronderstellingen:
 * De **entiteiten** in de probleem-ruimte worden geïdentificeerd. Deze entiteiten zijn bedoeld om te worden verbruikt _atomisch_ voor elke aanvraag. Met andere woorden, is niet de database-systeem bedoeld voor het ophalen van gegevens van een enkele entiteit in meerdere queryaanvragen.
 * Er is een goed begrip van **lezen en schrijven van vereisten** voor de database-systeem. Deze vereisten leidt de optimalisaties die nodig zijn voor het gegevensmodel van grafiek.
 * De beginselen van het [Apache Tinkerpop eigenschap graph standard](http://tinkerpop.apache.org/docs/current/reference/#graph-computing) goed worden begrepen.

## <a name="when-do-i-need-a-graph-database"></a>Wanneer moet ik een grafiekdatabase?

Een grafiek database-oplossing kan optimaal worden toegepast als de entiteiten en relaties in een domein gegevens hebt een van de volgende kenmerken: 

* De entiteiten zijn **maximaal verbonden** via beschrijvende relaties. Het voordeel in dit scenario is het feit dat de relaties zijn opgeslagen in opslag.
* Er zijn **cyclische relaties** of **zelf waarnaar wordt verwezen entiteiten**. Dit patroon wordt vaak een hele uitdaging wanneer met behulp van relationele of document van databases.
* Er zijn **dynamisch zich ontwikkelende relaties** tussen entiteiten. Dit patroon is vooral van toepassing op hiërarchische of boomstructuur gegevens met veel niveaus.
* Er zijn **veel-op-veel-relaties** tussen entiteiten.
* Er zijn **vereisten voor beide entiteiten en relaties te lezen en schrijven**. 

Als de bovenstaande criteria wordt voldaan, is het waarschijnlijk dat een benadering van de graph-database biedt voordelen voor **query complexiteit**, **gegevensmodel schaalbaarheid**, en **queryprestaties**.

De volgende stap is om te bepalen als de grafiek moet worden gebruikt voor analyse- of transactionele gaat. Als de grafiek is bedoeld om te worden gebruikt voor zware berekeningen en gegevensverwerking werkbelastingen, het waard is om normaal zou zijn om te verkennen de [Cosmos DB Spark-connector](https://docs.microsoft.com/azure/cosmos-db/spark-connector) en het gebruik van de [GraphX bibliotheek](https://spark.apache.org/graphx/). 

## <a name="how-to-use-graph-objects"></a>Het gebruik van de graph-objecten

De [Apache Tinkerpop eigenschap graph standard](http://tinkerpop.apache.org/docs/current/reference/#graph-computing) definieert u twee soorten objecten **hoekpunten** en **randen**. 

Hier volgen de aanbevolen procedures voor de eigenschappen in de graph-objecten:

| Object | Eigenschap | Type | Opmerkingen |
| --- | --- | --- |  --- |
| Vertex | id | String | Een unieke afgedwongen per partitie. Als een waarde niet opgegeven bij het invoegen en automatisch gegenereerde is worden GUID opgeslagen. |
| Vertex | label | String | Deze eigenschap wordt gebruikt voor het definiëren van het type entiteit die het hoekpunt vertegenwoordigt. Als een waarde niet is opgegeven, wordt een standaardwaarde 'hoekpunt' worden gebruikt. |
| Vertex | properties | Tekenreeks, Boole-waarde, numerieke | Een lijst met afzonderlijke eigenschappen die zijn opgeslagen als sleutel / waarde-paren in elk hoekpunt. |
| Vertex | Partitiesleutel | Tekenreeks, Boole-waarde, numerieke | Deze eigenschap bepaalt waar het hoekpunt en de uitgaande randen worden opgeslagen. Meer informatie over [graph-partitionering](graph-partitioning.md). |
| Microsoft Edge | id | String | Een unieke afgedwongen per partitie. Automatisch gegenereerde standaard. Randen gewoonlijk niet beschikken over de noodzaak om te worden unieke opgehaald door een ID. |
| Microsoft Edge | label | String | Deze eigenschap wordt gebruikt voor het definiëren van het type van de relatie die twee hoekpunten hebben. |
| Microsoft Edge | properties | Tekenreeks, Boole-waarde, numerieke | Een lijst met afzonderlijke eigenschappen die zijn opgeslagen als sleutel / waarde-paren in elke edge. |

> [!NOTE]
> Randen vereisen een waarde voor de partitiesleutel, omdat de waarde automatisch toegewezen op basis van hun bronhoekpunt is wordt. Meer informatie in de [graph-partitionering](graph-partitioning.md) artikel.

## <a name="entity-and-relationship-modeling-guidelines"></a>Entiteits- en relatie richtlijnen voor modellering

Hier volgen een aantal richtlijnen om de benadering van gegevensmodellering voor een Gremlin-API van Azure Cosmos DB graph-database. Deze richtlijnen wordt ervan uitgegaan dat er een bestaande definitie van een domein gegevens en query's voor het is.

> [!NOTE]
> De onderstaande stappen uit worden als aanbevelingen weergegeven. Het uiteindelijke model moet worden geëvalueerd en getest voordat de overwegen als gereed is voor productie. Bovendien zijn de onderstaande aanbevelingen, specifiek voor Azure Cosmos DB Gremlin-API-implementatie. 

### <a name="modeling-vertices-and-properties"></a>Hoekpunten en eigenschappen modelleren 

De eerste stap voor het gegevensmodel van een grafiek is om toe te wijzen elke geïdentificeerde entiteit op een **object vertex**. Een één-op-een-toewijzing van alle entiteiten aan hoekpunten moet een eerste stap en onderhevig aan wijzigingen.

Er is een algemene zich eigenschappen van een enkele entiteit als afzonderlijke hoekpunten toe te wijzen. Bekijk het voorbeeld hieronder, waar dezelfde entiteit wordt weergegeven op twee verschillende manieren:

* **Op basis van een hoekpunt eigenschappen**: De entiteit wordt in deze benadering drie afzonderlijke hoekpunten en randen twee gebruikt om te beschrijven van de eigenschappen ervan. Terwijl deze benadering redundantie te verminderen kan, toeneemt model complexiteit. Een toename in model complexiteit kan leiden tot extra latentie, complexiteit en kosten berekening. Dit model kan ook uitdagingen bij het partitioneren van opleveren.

![Entiteitsmodel met hoekpunten voor eigenschappen.](./media/graph-modeling/graph-modeling-1.png)

* **De eigenschap ingesloten hoekpunten**: Deze methode maakt gebruik van de lijst met sleutel / waarde-paar om weer te geven van alle eigenschappen van de entiteit binnen een hoekpunt. Deze benadering biedt beperkte model complexiteit, die tot eenvoudiger query's en meer betaalbare doorkruisingen leidt.

![Entiteitsmodel met hoekpunten voor eigenschappen.](./media/graph-modeling/graph-modeling-2.png)

> [!NOTE]
> De bovenstaande voorbeelden laten zien een vereenvoudigde graph-model om alleen de vergelijking tussen de twee manieren van het delen van de entiteitseigenschappen van de weer te geven.

De **eigenschap ingesloten hoekpunten** patroon biedt doorgaans een meer goed presterende en schaalbare manier. De standaard-benadering tot een nieuwe grafiek gegevensmodel moet gravitate voor dit patroon.

Er zijn echter scenario's waarin verwijzen naar een eigenschap voordelen kan opgeven. Bijvoorbeeld: als de eigenschap waarnaar wordt verwezen, regelmatig wordt bijgewerkt. Met behulp van een afzonderlijke hoekpunt om weer te geven van een eigenschap die voortdurend wordt gewijzigd, zou het bedrag van schrijfbewerkingen waarvoor de update wilt minimaliseren.

### <a name="relationship-modeling-with-edge-directions"></a>Relatie modellen met edge-instructies

Nadat de hoekpunten zijn gemodelleerd, kan de randen kunnen worden toegevoegd om de relaties tussen deze aan te geven. De eerste aspect dat moet worden geëvalueerd is de **richting van de relatie**. 

Edge-objecten hebben een standaardrichting die wordt gevolgd door een transport bij het gebruik van de `out()` of `outE()` functie. Met behulp van deze natuurlijke richting resulteert in een efficiënte bewerking, omdat alle hoekpunten worden opgeslagen met de uitgaande randen. 

Echter, in de tegengestelde richting van een rand doorlopen, met behulp van de `in()` functie, altijd zal leiden tot een partitieoverkoepelende query uitvoert. Meer informatie over [graph-partitionering](graph-partitioning.md). Als er behoefte aan voortdurend bladeren met behulp van de `in()` functie, het is raadzaam om toe te voegen randen in beide richtingen.

U kunt de edge-richting bepalen met behulp van de `.to()` of `.from()` predikaten naar de `.addE()` Gremlin-stap. Of met behulp van de [BulkExecutor-bibliotheek voor Gremlin-API](bulk-executor-graph-dotnet.md).

> [!NOTE]
> Edge-objecten hebben een richting standaard.

### <a name="relationship-labeling"></a>Relatie labels

Met behulp van de beschrijvende relatielabels, kan de efficiëntie van edge resolutie operations verbeteren. Dit patroon kan worden toegepast op de volgende manieren:
* Niet-algemene voorwaarden gebruiken voor het labelen van een relatie.
* Het label van het bronhoekpunt naar het label van de doelhoekpunt koppelen met de naam van de relatie.

![Relatie labels voorbeelden.](./media/graph-modeling/graph-modeling-3.png)

Hoe meer specifieke het label dat de traverser wordt gebruikt voor het filteren van de randen, hoe beter. Deze beslissing kan een aanzienlijke invloed hebben op ook de kosten voor de query. U kunt de kosten van de query evalueren op elk gewenst moment [met behulp van de stap executionProfile](graph-execution-profile.md).


## <a name="next-steps"></a>Volgende stappen: 
* Bekijk de lijst met ondersteunde [Gremlin-stappen](gremlin-support.md).
* Meer informatie over [graph Databasepartitionering](graph-partitioning.md) te bekommeren om grootschalige grafieken.
* Evalueren van de Gremlin-query's met behulp van de [profiel van de uitvoering van stap](graph-execution-profile.md).
