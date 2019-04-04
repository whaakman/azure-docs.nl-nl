---
title: Het model en partitie gegevens op Azure Cosmos DB met behulp van een voorbeeld van een echte
description: Meer informatie over het model en Maak partities voor een real-world-voorbeeld met behulp van de Core API van Azure Cosmos DB
author: ThomasWeiss
ms.service: cosmos-db
ms.topic: sample
ms.date: 3/27/2019
ms.author: thweiss
ms.openlocfilehash: ac1b94de4b439aab202d53b23b0d0da616a9f851
ms.sourcegitcommit: f093430589bfc47721b2dc21a0662f8513c77db1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/04/2019
ms.locfileid: "58919614"
---
# <a name="how-to-model-and-partition-data-on-azure-cosmos-db-using-a-real-world-example"></a>Het model en partitie gegevens op Azure Cosmos DB met behulp van een voorbeeld van een echte

In dit artikel is gebaseerd op de verschillende Azure Cosmos DB-concepten, zoals [gegevensmodellering](modeling-data.md), [partitioneren](partitioning-overview.md), en [ingerichte doorvoer](request-units.md) te laten zien hoe een echte aanpakken gegevens ontwerp oefening.

Als u doorgaans met relationele databases werkt, hebt u waarschijnlijk gewoonten en intuitions beschreven voor het ontwerpen van een gegevensmodel gemaakt. Vanwege de specifieke beperkingen, maar ook de unieke voordelen van Azure Cosmos DB, de meeste van deze aanbevolen procedures niet goed vertaald en u naar suboptimale oplossingen kunnen slepen. Het doel van dit artikel is om u te begeleiden u bij het voltooien van het modelleren van een echte use-case op Azure Cosmos DB van modelleren entiteit CO-locatie en de partitionering van container-item.

## <a name="the-scenario"></a>Het scenario

Voor deze oefening, gaan we rekening houden met het domein van een blogplatform dat waar *gebruikers* kunt maken *berichten*. Gebruikers kunnen ook *zoals* en toe te voegen *opmerkingen* naar deze berichten.

> [!TIP]
> We hebben enkele woorden in gemarkeerd *cursief*; deze woorden identificeren van het soort dingen"" ons model hebben om te bewerken.

Meer vereisten aan onze specificatie toe te voegen:

- Een front-pagina wordt weergegeven een feed met recent gemaakte berichten
- We kunnen alle berichten voor een gebruiker, alle opmerkingen voor een bericht en alle likes voor een bericht worden opgehaald
- Berichten worden geretourneerd met de gebruikersnaam van de auteurs en een telling van het aantal opmerkingen en likes die ze hebben,
- Opmerkingen en likes worden ook met de gebruikersnaam van de gebruikers die u hebt gemaakt, geretourneerd
- Wanneer weergegeven als een lijst met moeten berichten alleen een afgekapte samenvatting van de inhoud ervan.

## <a name="identify-the-main-access-patterns"></a>De belangrijkste toegangspatronen identificeren

Als u wilt starten, geven wij bepaalde structuur met onze eerste specificatie door het identificeren van patronen voor databasetoegang van onze oplossing. Bij het ontwerpen van een gegevensmodel voor Azure Cosmos DB, is het belangrijk om te begrijpen welke aanvragen het model dienen moet om ervoor te zorgen dat het model efficiënt deze aanvragen moet fungeren.

Het algehele proces om eenvoudiger te maken om te volgen, we categoriseren deze verschillende aanvragen als opdrachten of query's, sommige woorden van deze items lenen [CQRS](https://en.wikipedia.org/wiki/Command%E2%80%93query_separation#Command_query_responsibility_segregation) waarin opdrachten zijn schrijfaanvragen (dat wil zeggen, intents om bij te werken van het systeem) en query's zijn alleen-lezen aanvragen.

Hier volgt de lijst met aanvragen die ons platform om beschikbaar te stellen:

- **[C1]**  Maken/bewerken van een gebruiker
- **[Q1]**  Een gebruiker ophalen
- **[C2]**  Een bericht maken/bewerken
- **[Q2]**  Een bericht ophalen
- **[Q3]**  Lijst met berichten in verkorte vorm van een gebruiker
- **[C3]**  Een opmerking maken
- **[K4]**  Lijst van een post-opmerkingen
- **[C4]**  , Zoals een bericht
- **VRAAG [5]**  Likes van een bericht weergeven
- **[Q6]**  Lijst de *x* meest recente posts gemaakt in een korte formulier (invoer)

Als deze fase nog niet hebt u gebruikmaakt van over de details van wat elke entiteit (gebruiker, boeken, enzovoort) bevat. Deze stap is meestal tussen de eerste rijen worden aangepakt bij het ontwerpen op basis van een relationele opslagplaats, omdat we hebben om te achterhalen hoe deze entiteiten wordt vertaald in termen van tabellen, kolommen, refererende sleutels enzovoort. Het is veel minder van een probleem met een documentdatabase die een schema op schrijven niet afdwingen.

De belangrijkste reden waarom het is belangrijk om te identificeren van onze patronen voor databasetoegang vanaf het begin is omdat deze lijst met aanvragen is het verstandig om ons testpakket. Telkens wanneer we onze gegevensmodel herhalen, we doorlopen voor elk van de aanvragen en controleren van de prestaties en schaalbaarheid.

## <a name="v1-a-first-version"></a>V1: Een eerste versie

We beginnen met twee containers: `users` en `posts`.

### <a name="users-container"></a>Gebruikerscontainer

Deze container wordt alleen opgeslagen items voor gebruikersgegevens:

    {
      "id": "<user-id>",
      "username": "<username>"
    }

We deze container door partitie `id`, wat betekent dat elke logische partitie in die container wordt slechts één item bevatten.

### <a name="posts-container"></a>Berichten-container

Deze container fungeert als host voor berichten, opmerkingen en likes:

    {
      "id": "<post-id>",
      "type": "post",
      "postId": "<post-id>",
      "userId": "<post-author-id>",
      "title": "<post-title>",
      "content": "<post-content>",
      "creationDate": "<post-creation-date>"
    }

    {
      "id": "<comment-id>",
      "type": "comment",
      "postId": "<post-id>",
      "userId": "<comment-author-id>",
      "content": "<comment-content>",
      "creationDate": "<comment-creation-date>"
    }

    {
      "id": "<like-id>",
      "type": "like",
      "postId": "<post-id>",
      "userId": "<liker-id>",
      "creationDate": "<like-creation-date>"
    }

We deze container door partitie `postId`, wat betekent dat elke logische partitie in die container één bericht, de opmerkingen voor dit item en alle likes voor dit item bevat.

Houd er rekening mee dat we hebben ingevoerd een `type` eigenschap in de items die zijn opgeslagen in deze container onderscheid maken tussen de drie typen entiteiten die deze hosts van de container.

Bovendien hebben we gekozen om te verwijzen naar gerelateerde gegevens in plaats van het insluiten van inhoud (controleren [in deze sectie](modeling-data.md) voor meer informatie over deze concepten) omdat:

- Er is geen bovengrens voor het aantal berichten van een gebruiker maken kunt,
- berichten kunnen worden willekeurig lang
- Er is geen bovengrens voor het aantal opmerkingen en likes een bericht hebben kan,
- We willen kunnen een opmerking of een vergelijkbare toevoegen aan een bericht zonder te hoeven bijwerken van het bericht zelf.

## <a name="how-well-does-our-model-perform"></a>Hoe goed ons model uitvoeren?

Het is nu tijd om de prestaties en schaalbaarheid van onze eerste versie vast te stellen. Voor elk van de eerder vastgestelde aanvragen, we de latentie ervan en het aantal aanvragen meten eenheden verbruikt. Deze meting wordt uitgevoerd op basis van een dummy gegevensset met 100.000 gebruikers met 5 tot 50 berichten per gebruiker en maximaal 25 opmerkingen en 100 likes per bericht.

### <a name="c1-createedit-a-user"></a>[C1] Een gebruiker maken/bewerken

Deze aanvraag is eenvoudig te implementeren, zoals we net maken of bijwerken van een item in de `users` container. De aanvragen zijn netjes wordt verdeeld over alle partities dank aan de `id` partitiesleutel.

![Schrijven van één item naar de gebruikerscontainer](./media/how-to-model-partition-example/V1-C1.png)

| **Latentie** | **RU kosten in rekening gebracht** | **Prestaties** |
| --- | --- | --- |
| 7 ms | 5.71 RU | ✅ |

### <a name="q1-retrieve-a-user"></a>[Q1] Een gebruiker ophalen

Bij het ophalen van een gebruiker wordt uitgevoerd door het lezen van het bijbehorende item uit de `users` container.

![Het ophalen van een item uit de gebruikerscontainer](./media/how-to-model-partition-example/V1-Q1.png)

| **Latentie** | **RU kosten in rekening gebracht** | **Prestaties** |
| --- | --- | --- |
| 2 ms | 1 RU | ✅ |

### <a name="c2-createedit-a-post"></a>[C2] Een bericht maken/bewerken

Op dezelfde manier naar **[C1]**, we net hebben om te schrijven naar de `posts` container.

![Schrijven van één item naar de container berichten](./media/how-to-model-partition-example/V1-C2.png)

| **Latentie** | **RU kosten in rekening gebracht** | **Prestaties** |
| --- | --- | --- |
| 9 ms | 8.76 RU | ✅ |

### <a name="q2-retrieve-a-post"></a>[Q2] Een bericht ophalen

We beginnen bij het ophalen van de bijbehorende document van de `posts` container. Maar dat is niet voldoende, aan de hand van onze specificatie hebben we ook om samen te voegen van de gebruikersnaam van de auteur van het bericht en de aantallen van het aantal opmerkingen en het aantal likes dit bericht heeft, waarvoor 3 aanvullende SQL-query's kunnen worden uitgegeven.

![Bij het ophalen van een bericht en aggregeren van aanvullende gegevens](./media/how-to-model-partition-example/V1-Q2.png)

Elk van de filters extra query's op de partitiesleutel van de desbetreffende container, dit is precies wat we willen prestaties en schaalbaarheid te maximaliseren. Maar we hebben uiteindelijk vier bewerkingen om te retourneren van een enkel bericht, zodat we die in een volgende iteratie verbeteren u uit te voeren.

| **Latentie** | **RU kosten in rekening gebracht** | **Prestaties** |
| --- | --- | --- |
| 9 ms | 19.54 RU | ⚠ |

### <a name="q3-list-a-users-posts-in-short-form"></a>[Q3] Lijst met berichten in verkorte vorm van een gebruiker

We hebben eerst om op te halen van de gewenste berichten met een SQL-query die worden opgehaald van de bijbehorende berichten aan deze bepaalde gebruiker. Maar we ook hebben om uit te geven van extra query's voor het samenvoegen van de auteur gebruikersnaam en het aantal opmerkingen en likes.

![Alle berichten voor een gebruiker ophalen en samenvoegen van hun aanvullende gegevens](./media/how-to-model-partition-example/V1-Q3.png)

Deze implementatie geeft veel nadelen:

- de query's samenvoegen van het aantal opmerkingen en likes moeten worden uitgegeven voor elk bericht dat wordt geretourneerd door de eerste query
- de belangrijkste query niet gefilterd op de partitiesleutel van de `posts` container, leiden tot een fanout en de scan van een partitie in de container.

| **Latentie** | **RU kosten in rekening gebracht** | **Prestaties** |
| --- | --- | --- |
| 130 ms | 619.41 RU | ⚠ |

### <a name="c3-create-a-comment"></a>[C3] Een opmerking maken

Een opmerking is gemaakt door het schrijven van het bijbehorende item in de `posts` container.

![Schrijven van één item naar de container berichten](./media/how-to-model-partition-example/V1-C2.png)

| **Latentie** | **RU kosten in rekening gebracht** | **Prestaties** |
| --- | --- | --- |
| 7 ms | 8.57 RU | ✅ |

### <a name="q4-list-a-posts-comments"></a>[K4] Lijst van een post-opmerkingen

We beginnen met een query waarmee alle opmerkingen voor dit item worden opgehaald en nogmaals, moeten we ook statistische gebruikersnamen afzonderlijk voor elke opmerking.

![Bij het ophalen van alle opmerkingen voor een bericht en de aanvullende gegevens verzamelen](./media/how-to-model-partition-example/V1-Q4.png)

Hoewel de belangrijkste query worden gefilterd op de partitiesleutel van de container, penalizes de gebruikersnamen afzonderlijk aggregeren de algehele prestaties. We hebt die later verbeteren.

| **Latentie** | **RU kosten in rekening gebracht** | **Prestaties** |
| --- | --- | --- |
| 23 ms | 27.72 RU | ⚠ |

### <a name="c4-like-a-post"></a>[C4] Als een bericht

Net als bij **[C3]**, maken we het overeenkomstige item in de `posts` container.

![Schrijven van één item naar de container berichten](./media/how-to-model-partition-example/V1-C2.png)

| **Latentie** | **RU kosten in rekening gebracht** | **Prestaties** |
| --- | --- | --- |
| 6 ms | 7.05 RU | ✅ |

### <a name="q5-list-a-posts-likes"></a>VRAAG [5] Lijst van een bericht likes

Net als bij **[K4]**, we query uitvoeren op de likes voor dit item en klik vervolgens in een statistische hun gebruikersnamen.

![Bij het ophalen van alle leuk vinden voor een bericht en de aanvullende gegevens verzamelen](./media/how-to-model-partition-example/V1-Q5.png)

| **Latentie** | **RU kosten in rekening gebracht** | **Prestaties** |
| --- | --- | --- |
| 59 ms | 58.92 RU | ⚠ |

### <a name="q6-list-the-x-most-recent-posts-created-in-short-form-feed"></a>[Q6] Lijst van de meest recente x-berichten gemaakt in een korte formulier (invoer)

We de meest recente berichten ophalen door het opvragen van de `posts` container gesorteerd op aflopende aanmaakdatum, en vervolgens cumulatieve gebruikersnamen en telt het aantal opmerkingen en likes voor elk van de berichten.

![Het ophalen van de meest recente posts en aggregeren van hun aanvullende gegevens](./media/how-to-model-partition-example/V1-Q6.png)

Nogmaals, de eerste query niet filteren op de partitiesleutel van de `posts` container, zodat een kostbare fanout wordt geactiveerd. Dit is zelfs slechter als we een veel grotere resultatenset als doel en sorteer de resultaten met een `ORDER BY` -component, waardoor het veel duurder in termen van aanvraageenheden.

| **Latentie** | **RU kosten in rekening gebracht** | **Prestaties** |
| --- | --- | --- |
| 306 ms | 2063.54 RU | ⚠ |

## <a name="reflecting-on-the-performance-of-v1"></a>Als gevolg van de prestaties van V1

Kijken naar de prestatieproblemen die we in de vorige sectie hebt gehad, kunnen we twee hoofdcategorieën problemen identificeren:

- Sommige aanvragen vereisen meerdere query's kunnen worden uitgegeven om het verzamelen van alle gegevens moet moeten worden geretourneerd
- aantal query's filteren niet op de partitiesleutel van de containers die ze richten, leidt tot een fanout die onze schaalbaarheid belemmeren.

Laten we elk van deze problemen, beginnen met het eerste item oplossen.

## <a name="v2-introducing-denormalization-to-optimize-read-queries"></a>V2: Maak kennis met denormalisatie lezen query's optimaliseren

De reden waarom we hebben om uit te geven aanvullende aanvragen in sommige gevallen is dat de resultaten van de eerste aanvraag bevat niet alle gegevens die we nodig om terug te keren. Als u werkt met een niet-relationeel gegevensarchief, zoals Azure Cosmos DB, wordt dit soort problemen meestal opgelost door gegevens denormalizing in onze gegevensset.

In ons voorbeeld wijzigen we de postitems om toe te voegen van de gebruikersnaam van de auteur van het bericht, het aantal opmerkingen en het aantal likes:

    {
      "id": "<post-id>",
      "type": "post",
      "postId": "<post-id>",
      "userId": "<post-author-id>",
      "userUsername": "<post-author-username>",
      "title": "<post-title>",
      "content": "<post-content>",
      "commentCount": <count-of-comments>,
      "likeCount": <count-of-likes>,
      "creationDate": "<post-creation-date>"
    }

We ook wijzigen van opmerkingen en items om toe te voegen van de gebruikersnaam van de gebruiker die ze heeft gemaakt, zoals:

    {
      "id": "<comment-id>",
      "type": "comment",
      "postId": "<post-id>",
      "userId": "<comment-author-id>",
      "userUsername": "<comment-author-username>",
      "content": "<comment-content>",
      "creationDate": "<comment-creation-date>"
    }

    {
      "id": "<like-id>",
      "type": "like",
      "postId": "<post-id>",
      "userId": "<liker-id>",
      "userUsername": "<liker-username>",
      "creationDate": "<like-creation-date>"
    }

### <a name="denormalizing-comment-and-like-counts"></a>Opmerking denormalizing en telt als

Wat we willen bereiken, is dat telkens wanneer er een opmerking of een vergelijkbare toevoegt, we ook verhogen de `commentCount` of de `likeCount` in het bijbehorende bericht. Als onze `posts` container is gepartitioneerd op basis van `postId`, het nieuwe item (opmerkingen of zoals) en de bijbehorende post bevinden zich in dezelfde logische partitie. Als gevolg hiervan kunnen worden gebruikt in een [opgeslagen procedure](stored-procedures-triggers-udfs.md) die bewerking uit te voeren.

Nu bij het maken van een opmerking (**[C3]**), in plaats van alleen toe te voegen een nieuw item in de `posts` container noemen we de volgende opgeslagen procedure op die container:

```javascript
function createComment(postId, comment) {
  var collection = getContext().getCollection();

  collection.readDocument(
    `${collection.getAltLink()}/docs/${postId}`,
    function (err, post) {
      if (err) throw err;

      post.commentCount++;
      collection.replaceDocument(
        post._self,
        post,
        function (err) {
          if (err) throw err;

          comment.postId = postId;
          collection.createDocument(
            collection.getSelfLink(),
            comment
          );
        }
      );
    })
}
```

Deze opgeslagen procedure wordt vervolgens de ID van het bericht en de hoofdtekst van de nieuwe opmerking als parameters:

- haalt het bericht
- stappen van de `commentCount`
- Hiermee vervangt u het bericht
- de nieuwe opmerking wordt toegevoegd

Als opgeslagen procedures worden uitgevoerd als een atomic-transacties, is deze gegarandeerd dat de waarde van `commentCount` en het werkelijke aantal opmerkingen wordt altijd gesynchroniseerd blijven.

We noemen natuurlijk een vergelijkbare opgeslagen procedure bij het toevoegen van nieuwe leuk vinden om te verhogen de `likeCount`.

### <a name="denormalizing-usernames"></a>Denormalizing gebruikersnamen

Gebruikersnamen vereisen een andere benadering, zoals gebruikers zich niet alleen in andere partities, maar in een andere container. Wanneer we hoeft te denormaliseren van gegevens in partities en containers, gebruiken we de Broncontainer [wijzigingenfeed](change-feed.md).

In ons voorbeeld gebruiken we de wijzigingenfeed van de `users` container om te reageren wanneer gebruikers hun gebruikersnamen bijwerken. Wanneer dit gebeurt, wordt de wijziging door het aanroepen van een andere opgeslagen procedure op de `posts` container:

![Denormalizing gebruikersnamen in de container berichten](./media/how-to-model-partition-example/denormalization-1.png)

```javascript
function updateUsernames(userId, username) {
  var collection = getContext().getCollection();
  
  collection.queryDocuments(
    collection.getSelfLink(),
    `SELECT * FROM p WHERE p.userId = '${userId}'`,
    function (err, results) {
      if (err) throw err;

      for (var i in results) {
        var doc = results[i];
        doc.userUsername = username;

        collection.upsertDocument(
          collection.getSelfLink(),
          doc);
      }
    });
}
```

Deze opgeslagen procedure wordt vervolgens de ID van de gebruiker en de gebruikersnaam van de nieuwe als parameters:

- haalt alle items die overeenkomen met de `userId` (die kunnen worden berichten, opmerkingen, of likes)
- voor elk van deze items
  - vervangt de `userUsername`
  - Hiermee vervangt u het item

> [!IMPORTANT]
> Met deze bewerking is goedkoper omdat hiervoor deze opgeslagen procedure moet worden uitgevoerd op elke partitie van de `posts` container. Gaan we ervan uit dat de meeste gebruikers Kies een geschikte gebruikersnaam tijdens de registratie en wordt niet ooit wordt gewijzigd, zodat deze update slechts zelden wordt uitgevoerd.

## <a name="what-are-the-performance-gains-of-v2"></a>Wat zijn de prestaties van V2?

### <a name="q2-retrieve-a-post"></a>[Q2] Een bericht ophalen

Onze denormalisatie is voldaan, hebben we slechts één item voor het afhandelen van die aanvraag ophalen.

![Het ophalen van één item uit de container berichten](./media/how-to-model-partition-example/V2-Q2.png)

| **Latentie** | **RU kosten in rekening gebracht** | **Prestaties** |
| --- | --- | --- |
| 2 ms | 1 RU | ✅ |

### <a name="q4-list-a-posts-comments"></a>[K4] Lijst van een post-opmerkingen

Hier opnieuw, we kunnen met reserveonderdelen de extra aanvragen die de gebruikersnamen en het einde van opgehaald met één query filters voor de partitiesleutel.

![Bij het ophalen van alle opmerkingen voor een bericht](./media/how-to-model-partition-example/V2-Q4.png)

| **Latentie** | **RU kosten in rekening gebracht** | **Prestaties** |
| --- | --- | --- |
| 4 ms | 7,72 RU | ✅ |

### <a name="q5-list-a-posts-likes"></a>VRAAG [5] Lijst van een bericht likes

Exact dezelfde situatie wanneer de likes wordt weergegeven.

![Bij het ophalen van alle leuk vinden voor een bericht](./media/how-to-model-partition-example/V2-Q5.png)

| **Latentie** | **RU kosten in rekening gebracht** | **Prestaties** |
| --- | --- | --- |
| 4 ms | 8.92 RU | ✅ |

## <a name="v3-making-sure-all-requests-are-scalable"></a>V3: Zorg dat zijn alle aanvragen schaalbaar

Onze algemene prestatieverbeteringen bekijkt, er zijn nog steeds twee aanvragen die we nog niet volledig geoptimaliseerd: **[Q3]** en **[Q6]**. Ze zijn de aanvragen met betrekking tot query's die niet filteren op de partitiesleutel van de containers die ze richten.

### <a name="q3-list-a-users-posts-in-short-form"></a>[Q3] Lijst met berichten in verkorte vorm van een gebruiker

Deze aanvraag profiteren al van de verbeteringen die zijn geïntroduceerd in V2, waarbij reserveonderdelen van extra query's.

![Bij het ophalen van alle berichten voor een gebruiker](./media/how-to-model-partition-example/V2-Q3.png)

Maar de resterende query is nog steeds niet filteren op de partitiesleutel van de `posts` container.

De manier om na te denken over deze situatie is daadwerkelijk eenvoudig:

1. Deze aanvraag *heeft* om te filteren op de `userId` omdat we willen dat alle berichten voor een bepaalde gebruiker ophalen
1. Er wordt niet uitgevoerd ook omdat deze wordt uitgevoerd op basis de `posts` container, die niet is gepartitioneerd met `userId`
1. Met vermelding van de hand liggende, zouden we het prestatieprobleem oplossen door het uitvoeren van deze aanvraag indient voor een container die *is* gepartitioneerde door `userId`
1. Het blijkt dat we al deze een container hebben: de `users` container!

Daarom we een tweede niveau van denormalisatie stellen door te dupliceren gehele berichten naar de `users` container. Op die manier kunt effectief krijgen we een kopie van de berichten alleen gepartitioneerd langs een andere dimensies, waardoor ze veel efficiënter om op te halen door hun `userId`.

De `users` container bevat nu 2 soorten items:

    {
      "id": "<user-id>",
      "type": "user",
      "userId": "<user-id>",
      "username": "<username>"
    }

    {
      "id": "<post-id>",
      "type": "post",
      "postId": "<post-id>",
      "userId": "<post-author-id>",
      "userUsername": "<post-author-username>",
      "title": "<post-title>",
      "content": "<post-content>",
      "commentCount": <count-of-comments>,
      "likeCount": <count-of-likes>,
      "creationDate": "<post-creation-date>"
    }

Houd rekening met het volgende:

- We hebben ingevoerd een `type` veld in het item voor gebruikersgegevens te onderscheiden van gebruikers van berichten
- We hebben ook hebt toegevoegd een `userId` veld in het item voor gebruikersgegevens, dit redundant zijn met is de `id` veld, maar is vereist als de `users` container is nu gepartitioneerd op basis van `userId` (en niet `id` zoals eerder)

Als u wilt bereiken die denormalisatie, gebruiken we nog een keer de feed wijzigen. Nu we reageren op de wijzigingenfeed van de `posts` container verzending van een nieuwe of bijgewerkte post naar de `users` container. En omdat berichten aanbieding niet vereist om terug te keren van de volledige inhoud, kunnen we ze worden afgekapt in het proces.

![Berichten in de gebruikerscontainer denormalizing](./media/how-to-model-partition-example/denormalization-2.png)

We kunnen nu onze query te routeren de `users` container, filteren op partitiesleutel van de container.

![Bij het ophalen van alle berichten voor een gebruiker](./media/how-to-model-partition-example/V3-Q3.png)

| **Latentie** | **RU kosten in rekening gebracht** | **Prestaties** |
| --- | --- | --- |
| 4 ms | 6.46 RU | ✅ |

### <a name="q6-list-the-x-most-recent-posts-created-in-short-form-feed"></a>[Q6] Lijst van de meest recente x-berichten gemaakt in een korte formulier (invoer)

We hebben hier een soortgelijke situatie zijn getroffen: zelfs nadat de extra query's sparing links onnodige door de denormalisatie die is geïntroduceerd in V2, de resterende query niet gefilterd op de partitiesleutel van de container:

![Het ophalen van de meest recente posts](./media/how-to-model-partition-example/V2-Q6.png)

Na de dezelfde benadering en optimaliseren van de prestaties en schaalbaarheid van deze aanvraag is vereist dat deze alleen treffers in één partitie. Dit is denkbaar omdat we alleen om terug te keren een beperkt aantal items. Als u wilt vullen onze blogplatform startpagina, moeten we net ophalen van de 100 meest recente posts, zonder de noodzaak om te pagineren via de volledige gegevensset.

Dus voor het optimaliseren van deze laatste aanvraag we een derde container naar onze ontwerp introduceren, volledig toegewezen om te voldoen aan deze aanvraag. We onze berichten naar die nieuwe denormaliseren `feed` container:

    {
      "id": "<post-id>",
      "type": "post",
      "postId": "<post-id>",
      "userId": "<post-author-id>",
      "userUsername": "<post-author-username>",
      "title": "<post-title>",
      "content": "<post-content>",
      "commentCount": <count-of-comments>,
      "likeCount": <count-of-likes>,
      "creationDate": "<post-creation-date>"
    }

Deze container wordt geregeld aan de hand `type`, die altijd worden `post` in onze artikelen. Dat zorgt ervoor dat alle items in deze container wordt bevinden zich in dezelfde partitie.

Voor het bereiken van de denormalisatie, hebben we net van een toepassing aansluiten op de pijplijn die we eerder hebben geïntroduceerd voor het verzenden van de berichten op die nieuwe container-wijzigingenfeed. Een belangrijke dingen die met de bare-waarmee u rekening moet is dat we nodig om ervoor te zorgen dat we alleen de 100 meest recente posts; opslaan anders kan de inhoud van de container groter wordt dan de maximale grootte van een partitie. Dit wordt gedaan door het aanroepen van een [na trigger](stored-procedures-triggers-udfs.md#triggers) telkens wanneer een document wordt toegevoegd aan de container:

![Berichten naar de feed container denormalizing](./media/how-to-model-partition-example/denormalization-3.png)

Dit is de hoofdtekst van de na trigger die de verzameling worden afgekapt:

```javascript
function truncateFeed() {
  const maxDocs = 100;
  var context = getContext();
  var collection = context.getCollection();

  collection.queryDocuments(
    collection.getSelfLink(),
    "SELECT VALUE COUNT(1) FROM f",
    function (err, results) {
      if (err) throw err;

      processCountResults(results);
    });

  function processCountResults(results) {
    // + 1 because the query didn't count the newly inserted doc
    if ((results[0] + 1) > maxDocs) {
      var docsToRemove = results[0] + 1 - maxDocs;
      collection.queryDocuments(
        collection.getSelfLink(),
        `SELECT TOP ${docsToRemove} * FROM f ORDER BY f.creationDate`,
        function (err, results) {
          if (err) throw err;

          processDocsToRemove(results, 0);
        });
    }
  }

  function processDocsToRemove(results, index) {
    var doc = results[index];
    if (doc) {
      collection.deleteDocument(
        doc._self,
        function (err) {
          if (err) throw err;

          processDocsToRemove(results, index + 1);
        });
    }
  }
}
```

De laatste stap is het omleiden van onze query aan onze nieuwe `feed` container:

![Het ophalen van de meest recente posts](./media/how-to-model-partition-example/V3-Q6.png)

| **Latentie** | **RU kosten in rekening gebracht** | **Prestaties** |
| --- | --- | --- |
| 9 ms | 16,97 INCH RU | ✅ |

## <a name="conclusion"></a>Conclusie

We hebben een overzicht van de algehele prestaties en schaalbaarheid verbeteringen die ten opzichte van de verschillende versies van onze ontwerp geïntroduceerd.

| | V1 | V2 | V3 |
| --- | --- | --- | --- |
| **[C1]** | 7 ms / 5.71 RU | 7 ms / 5.71 RU | 7 ms / 5.71 RU |
| **[Q1]** | 2 ms / 1 RU | 2 ms / 1 RU | 2 ms / 1 RU |
| **[C2]** | 9 ms / 8.76 RU | 9 ms / 8.76 RU | 9 ms / 8.76 RU |
| **[Q2]** | 9 ms / 19.54 RU | 2 ms / 1 RU | 2 ms / 1 RU |
| **[Q3]** | 130 ms / 619.41 RU | 28 ms / 201.54 RU | 4 ms / 6.46 RU |
| **[C3]** | 7 ms / 8,57 RU | 7 ms / 15.27 RU | 7 ms / 15.27 RU |
| **[Q4]** | 23 ms / 27.72 RU | 4 ms / 7.72 RU | 4 ms / 7.72 RU |
| **[C4]** | 6 ms / 7.05 RU | 7 ms / 14.67 RU | 7 ms / 14.67 RU |
| **[Q5]** | 59 ms / 58.92 RU | 4 ms / 8.92 RU | 4 ms / 8.92 RU |
| **[Q6]** | 306 ms / 2063.54 RU | 83 ms / 532.33 RU | 9 ms / 16.97 RU |

### <a name="we-have-optimized-a-read-heavy-scenario"></a>We hebben een scenario leesintensief geoptimaliseerd

U wellicht hebt opgemerkt dat we onze inspanningen voor het verbeteren van de prestaties van leesaanvragen (query's) ten koste van de schrijfaanvragen (opdrachten) zijn geconcentreerd. In veel gevallen activeren schrijfbewerkingen nu volgende denormalisatie via wijzigingsfeeds, waardoor ze meer rekenintensief duur en meer om te realiseren.

Dit is onderbouwd door het feit dat een blogplatform (zoals meest sociale apps) leesintensief, wat inhoudt dat de hoeveelheid leesaanvragen er dienen meestal orde van grootte hoger is dan de hoeveelheid schrijfaanvragen is. Dus het verstandig is om schrijfaanvragen duurder om uit te voeren om u te laten worden leesaanvragen goedkoper en betere uitvoeren.

Als u op het meest extreme optimalisatie die we hebben gedaan, **[Q6]** ging van 2000 + ru's naar de zojuist 17 ru's; we die door berichten terwijl de kosten van ongeveer 10 ru's per item denormalizing hebben bereikt. Als we nog veel meer feed aanvragen dan worden gemaakt of bijgewerkt van berichten verwerken wilt, is de kosten van deze denormalisatie te verwaarlozen overweegt de algehele besparingen.

### <a name="denormalization-can-be-applied-incrementally"></a>Denormalisatie kan incrementeel worden toegepast

De schaalbaarheid verbeteringen die we in dit artikel hebt verkend hebben betrekking op denormalisatie en duplicatie van gegevens in de gegevensset. Er moet worden opgemerkt dat deze optimalisaties hoeft te worden ingevoerd op 1 dag. Query's die op partitiesleutels filteren beter uitvoeren op grote schaal, maar partitieoverkoepelende query's kunnen worden volledig aanvaardbaar als deze zelden of op basis van een beperkte set van gegevens heten. Als u alleen het bouwen van een prototype, of starten van een product met een klein en gecontroleerde gebruikersgroep, kunt u waarschijnlijk deze verbeteringen reserveonderdelen voor later; Wat is het belangrijk is om te [monitor](use-metrics.md) prestaties van uw model zodat u beslissen kunt of en wanneer is het tijd om deze te brengen.

De wijzigingenfeed die we gebruiken voor het distribueren van updates voor andere containers store alle die voortdurend bijgewerkt. Dit maakt het mogelijk alle updates aanvragen sinds het maken van de container en bootstrap gedenormaliseerde weergaven als een eenmalige bewerking voor bijwerken, zelfs als uw systeem al een grote hoeveelheid gegevens heeft.

## <a name="next-steps"></a>Volgende stappen

Na deze inleiding tot praktische gegevens modelleren en te partitioneren, kunt u om te controleren of de volgende artikelen om te controleren van de concepten besproken:

- [Werken met databases, containers en items](databases-containers-items.md)
- [Partitionering in Azure Cosmos DB](partitioning-overview.md)
- [In Azure Cosmos DB-wijzigingenfeed](change-feed.md)