---
title: Consistentieniveaus en Azure Cosmos DB-API's
description: Informatie over de consistentieniveaus voor API's in Azure Cosmos DB.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 10/23/2018
ms.reviewer: sngun
ms.openlocfilehash: a506c696cdb9ca6c6221b54c63d2446b7cb86a69
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54430553"
---
# <a name="consistency-levels-and-azure-cosmos-db-apis"></a>Consistentieniveaus en Azure Cosmos DB-API's

Vijf consistentiemodellen aangeboden door Azure Cosmos DB worden ondersteund door de Azure Cosmos DB SQL API. Wanneer u Azure Cosmos DB gebruikt, is de SQL-API de standaardinstelling. 

Azure Cosmos DB biedt ook ingebouwde ondersteuning voor wire-protocol-compatibele API's voor populaire-databases. Databases omvatten MongoDB, Apache Cassandra, Gremlin en Azure Table storage. Deze databases bieden niet nauwkeurig omschreven consistentiemodellen of SLA's gesteunde garanties voor consistentieniveaus. Ze bieden doorgaans slechts een subset van de vijf consistentiemodellen aangeboden door Azure Cosmos DB. Voor de SQL-API, Gremlin-API en Table-API, wordt het standaardconsistentieniveau geconfigureerd op het Azure Cosmos DB-account gebruikt. 

De volgende secties ziet u de toewijzing tussen de consistentie van de gegevens aangevraagd door een OSS-client-stuurprogramma voor Apache Cassandra 4.x en MongoDB 3.4. Dit document bevat ook de bijbehorende Azure Cosmos DB-consistentieniveaus voor Apache Cassandra en MongoDB.

## <a id="cassandra-mapping"></a>Toewijzing tussen Apache Cassandra en Azure Cosmos DB-consistentieniveaus

Deze tabel bevat de toewijzing van de consistentie tussen de Apache Cassandra en consistentieniveaus in Azure Cosmos DB. Voor elk van de Cassandra-lezen en schrijven consistentieniveaus, de bijbehorende Consistentieniveau voor Cosmos DB biedt betere, dat wil zeggen, strengere garanties.


<table>
<tr> 
  <th rowspan="2">Cassandra-Consistentieniveau</th> 
  <th rowspan="2">Cosmos DB-Consistentieniveau</th> 
  <th colspan="3">Toewijzing van de consistentie schrijven</th> 
  <th colspan="3">Lezen van consistentie van toewijzing</th> 
</tr> 


 
 <tr> 
  <th>Cassandra</th> 
  <th>Cosmos DB</th> 
  <th>Guarantee</th> 
  <th>Van Cassandra</th> 
  <th>Bij Cosmos DB</th> 
  <th>Guarantee</th> 
 </tr> 
 
  <tr> 
  <td rowspan="6">ALLE</td> 
  <td rowspan="6">Sterk</td> 
  <td>ALLE</td> 
  <td>Sterk</td> 
  <td>Linearisabiliteit</td> 
  <td>ALL, QUORUM, SERIAL, LOCAL_QUORUM, LOCAL_SERIAL, THREE, TWO, ONE, LOCAL_ONE</td> 
  <td>Sterk</td> 
  <td>Linearisabiliteit</td> 
 </tr> 
 
 <tr> 
  <td rowspan="2">EACH_QUORUM</td> 
  <td rowspan="2">Sterk</td> 
  <td rowspan="2">Linearisabiliteit</td> 
  <td>ALLE, QUORUM, SERIENUMMER, LOCAL_QUORUM, LOCAL_SERIAL, DRIE, TWEE</td> 
  <td>Sterk</td> 
  <td >Linearisabiliteit</td> 
 </tr> 
 
 <tr>
 <td>LOCAL_ONE, ÉÉN</td>
  <td>Consistent prefix</td>
   <td>Globale Consistent Prefix</td>
 </tr>
 

 <tr> 
  <td rowspan="2">QUORUM, SERIËLE</td> 
  <td rowspan="2">Sterk</td> 
  <td rowspan="2">Linearisabiliteit</td> 
  <td>ALLE, QUORUM, SERIENUMMER</td> 
  <td>Sterk</td> 
  <td >Linearisabiliteit</td> 
 </tr> 

 <tr>
   <td>LOCAL_ONE, ONE, LOCAL_QUORUM, LOCAL_SERIAL, TWO, THREE</td>
   <td>Consistent prefix</td>
   <td>Globale Consistent Prefix</td>
 </tr>
 
 
 <tr> 
 <td>LOCAL_QUORUM, 3, 2, ÉÉN, LOCAL_ONE, <b>ANY</b></td> 
  <td>Consistent prefix</td> 
  <td>Globale Consistent Prefix</td> 
  <td>LOCAL_ONE, ONE, TWO, THREE, LOCAL_QUORUM, QUORUM</td> 
  <td>Consistent prefix</td> 
  <td>Globale Consistent Prefix</td>
 </tr> 
 
 
  <tr> 
  <td rowspan="6">EACH_QUORUM</td> 
  <td rowspan="6">Sterk</td> 
  <td rowspan="2">EACH_QUORUM</td> 
  <td rowspan="2">Sterk</td> 
  <td rowspan="2">Linearisabiliteit</td> 
  <td>ALLE, QUORUM, SERIENUMMER, LOCAL_QUORUM, LOCAL_SERIAL, DRIE, TWEE</td> 
  <td>Sterk</td> 
  <td>Linearisabiliteit</td> 
 </tr> 
 
 <tr>
 <td>LOCAL_ONE, ÉÉN</td>
  <td>Consistent prefix</td>
   <td>Globale Consistent Prefix</td>
 </tr>
 
 
 
 <tr> 
  <td rowspan="2">QUORUM, SERIËLE</td> 
  <td rowspan="2">Sterk</td> 
  <td rowspan="2">Linearisabiliteit</td> 
  <td>ALLE, QUORUM, SERIENUMMER</td> 
  <td>Sterk</td> 
  <td>Linearisabiliteit</td> 
 </tr> 
 
 <tr>
 <td>LOCAL_ONE, ONE, LOCAL_QUORUM, LOCAL_SERIAL, TWO, THREE</td>
  <td>Consistent prefix</td>
   <td>Globale Consistent Prefix</td>
 </tr>
 
 
  <tr> 
  <td rowspan="2">LOCAL_QUORUM, 3, 2, ÉÉN, LOCAL_ONE,</td> 
  <td rowspan="2">Consistent prefix</td> 
  <td rowspan="2">Globale Consistent Prefix</td> 
  <td>ALLE</td> 
  <td>Sterk</td> 
  <td>Linearisabiliteit</td> 
 </tr> 
 
 <tr>
 <td>LOCAL_ONE, ONE, TWO, THREE, LOCAL_QUORUM, QUORUM</td>
  <td>Consistent prefix</td>
   <td>Globale Consistent Prefix</td>
 </tr>


  <tr> 
  <td rowspan="4">QUORUM</td> 
  <td rowspan="4">Sterk</td> 
  <td rowspan="2">QUORUM, SERIËLE</td> 
  <td rowspan="2">Sterk</td> 
  <td rowspan="2">Linearisabiliteit</td> 
  <td>ALLE, QUORUM, SERIENUMMER</td> 
  <td>Sterk</td> 
  <td>Linearisabiliteit</td> 
 </tr> 
 
 <tr>
 <td>LOCAL_ONE, ONE, LOCAL_QUORUM, LOCAL_SERIAL, TWO, THREE</td>
  <td>Consistent prefix</td>
   <td>Globale Consistent Prefix</td>
 </tr>
 
 
 <tr> 
  <td rowspan="2">LOCAL_QUORUM, 3, 2, ÉÉN, LOCAL_ONE,</td> 
  <td rowspan="2">Consistent prefix </td> 
  <td rowspan="2">Globale Consistent Prefix </td> 
  <td>ALLE</td> 
  <td>Sterk</td> 
  <td>Linearisabiliteit</td> 
 </tr> 
 
 <tr>
 <td>LOCAL_ONE, ONE, TWO, THREE, LOCAL_QUORUM, QUORUM</td>
  <td>Consistent prefix</td>
   <td>Globale Consistent Prefix</td>
 </tr>
 
 <tr> 
  <td rowspan="4">LOCAL_QUORUM, DRIE, TWEE</td> 
  <td rowspan="4">Gebonden veroudering</td> 
  <td rowspan="2">LOCAL_QUORUM, LOCAL_SERIAL, TWO, THREE</td> 
  <td rowspan="2">Gebonden veroudering</td> 
  <td rowspan="2">Gebonden veroudering.<br/>
Maximaal K versies of t keer achter.<br/>
De meest recente toegezegde waarde in de regio lezen. 
</td> 
  
  <td>QUORUM, LOCAL_QUORUM, LOCAL_SERIAL, TWO, THREE</td> 
  <td>Gebonden veroudering</td> 
  <td>Gebonden veroudering.<br/>
Maximaal K versies of t keer achter. <br/>
De meest recente toegezegde waarde in de regio lezen. </td> 
 </tr> 
 
 <tr>
 <td>LOCAL_ONE, ÉÉN</td>
  <td>Consistent prefix</td>
   <td>Consistent Prefix per regio</td>
 </tr>
 
 
 <tr> 
  <td>EEN LOCAL_ONE,</td> 
  <td>Consistent prefix </td> 
  <td >Consistent Prefix per regio </td> 
  <td>LOCAL_ONE, ONE, TWO, THREE, LOCAL_QUORUM, QUORUM</td> 
  <td>Consistent prefix</td> 
  <td>Consistent Prefix per regio</td> 
 </tr> 
</table>

## <a id="mongo-mapping"></a>Toewijzing tussen MongoDB 3.4 en Azure Cosmos DB-consistentieniveaus

De volgende tabel ziet u de toewijzing 'lezen problemen' tussen MongoDB 3.4 en het standaardconsistentieniveau in Azure Cosmos DB. De tabel ziet u implementaties in meerdere regio's en één regio.

| **MongoDB 3.4** | **Azure Cosmos DB (multi-region)** | **Azure Cosmos DB (één regio)** |
| - | - | - |
| Linearizable | Sterk | Sterk |
| Meerderheid | Gebonden veroudering | Sterk |
| Lokaal | Consistent voorvoegsel | Consistent voorvoegsel |

## <a name="next-steps"></a>Volgende stappen

Meer informatie over consistentieniveaus en compatibiliteit tussen Azure Cosmos DB-API's met de open-source-API's. Zie de volgende artikelen:

* [Beschikbaarheid en prestaties van optimalisatie voor verschillende consistentieniveaus](consistency-levels-tradeoffs.md)
* [MongoDB-functies worden ondersteund door de Azure Cosmos DB-API voor MongoDB](mongodb-feature-support.md)
* [Apache Cassandra-functies worden ondersteund door de Cassandra-API van Azure Cosmos DB](cassandra-support.md)