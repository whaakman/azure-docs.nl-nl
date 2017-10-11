---
title: Modeling documentgegevens voor een NoSQL-database | Microsoft Docs
description: Meer informatie over het modelleren van gegevens voor NoSQL-databases
keywords: modellering van gegevens
services: cosmos-db
author: arramac
manager: jhubbard
editor: mimig1
documentationcenter: 
ms.assetid: 69521eb9-590b-403c-9b36-98253a4c88b5
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/29/2016
ms.author: arramac
ms.openlocfilehash: 16c387fe574243544cf54cf283c7713ddcaa1942
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="modeling-document-data-for-nosql-databases"></a>Modeling documentgegevens voor NoSQL-databases
Terwijl databases zonder schema, zoals Azure Cosmos DB, u kunnen heel eenvoudig om af te spelen op wijzigingen in het gegevensmodel u moet nog steeds hoeven te besteden aan bepaalde tijd na te denken over uw gegevens. 

Hoe er gegevens worden opgeslagen? Hoe uw toepassing gaat op te halen en het opvragen van gegevens? Is uw toepassing dikke lezen of schrijven zwaar? 

Na het lezen van dit artikel kunt u zich de volgende vragen beantwoorden:

* Hoe moet ik denken dat zij een document in een documentdatabase?
* Wat is er gegevens modelleren en waarom moet ik het belangrijkst? 
* Hoe wordt modellering gegevens in een documentdatabase in een relationele database?
* Hoe ik gegevensrelaties in een niet-relationele database express?
* Wanneer insluiten gegevens en wanneer kan ik gegevens koppelen?

## <a name="embedding-data"></a>Gegevens insluiten
Wanneer u gegevens in een store, document, zoals Azure Cosmos DB modelleren start probeer uw entiteiten behandelen als **zelfstandig documenten** vertegenwoordigd in JSON.

Voordat we meteen te veel meer laat het ons Neem een paar stappen terug en bekijk hoe wij iets in een relationele database, een onderwerp dat velen van ons al bekend met bent mogelijk model. Het volgende voorbeeld ziet hoe een persoon kan worden opgeslagen in een relationele database. 

![Relationele database-model](./media/documentdb-modeling-data/relational-data-model.png)

Als u werkt met relationele databases, hebben we geleerd jaar normaliseren, normaliseren, normaliseren.

Doorgaans normaliseren van uw gegevens omvat het maken van een entiteit, zoals een persoon, en deze te splitsen op discrete gegevens. In het bovenstaande voorbeeld hebben van een persoon meerdere details van contactpersonen records, alsmede meerdere adresrecords. We zelfs nog een stapje verder te gaan en contactgegevens onderverdelen per verdere uitpakken algemene velden, zoals een type. Hetzelfde adres, heeft elke record hier een type zoals *Start* of *Business* 

Lokale wanneer de normalisatie van gegevens wordt de wegwijs **voorkomen gegevensopslag voor redundante** op elke registreren en in plaats van te verwijzen naar gegevens. In dit voorbeeld om te lezen van een persoon, met hun contactgegevens en adressen, moet u JOINS te gebruiken voor het cumuleren van uw gegevens effectief tijdens de uitvoering.

    SELECT p.FirstName, p.LastName, a.City, cd.Detail
    FROM Person p
    JOIN ContactDetail cd ON cd.PersonId = p.Id
    JOIN ContactDetailType on cdt ON cdt.Id = cd.TypeId
    JOIN Address a ON a.PersonId = p.Id

Bijwerken van één persoon met hun contactgegevens en adressen vereist schrijfbewerkingen over veel afzonderlijke tabellen. 

Nu eens kijken hoe we dezelfde gegevens als een zelfstandig entiteit in de documentdatabase van een zou model.

    {
        "id": "1",
        "firstName": "Thomas",
        "lastName": "Andersen",
        "addresses": [
            {            
                "line1": "100 Some Street",
                "line2": "Unit 1",
                "city": "Seattle",
                "state": "WA",
                "zip": 98012
            }
        ],
        "contactDetails": [
            {"email: "thomas@andersen.com"},
            {"phone": "+1 555 555-5555", "extension": 5555}
        ] 
    }

Met de bovenstaande benadering, we nu hebben **gedenormaliseerd** degene vastleggen waar we **ingesloten** alle informatie met betrekking tot deze persoon, zoals hun contactgegevens en -adressen naar een enkele JSON document.
Aangezien we niet beperkt naar een vast schema blijven hebben we bovendien de flexibiliteit voor handelingen zoals de contactgegevens van verschillende vormen volledig hebben. 

Een persoonsrecord voltooid ophalen uit de database is nu één bewerking met betrekking tot één verzameling en voor één document te lezen. Bijwerken van een persoonsrecord met hun contactgegevens en -adressen is ook een schrijfbewerking voor één voor één document.

Door denormalizing gegevens, moet uw toepassing mogelijk minder query's en updates algemene bewerkingen worden voltooid. 

### <a name="when-to-embed"></a>Wanneer voor het insluiten van
In het algemeen gebruikt ingesloten gegevens modellen wanneer:

* Er zijn **bevat** relaties tussen entiteiten.
* Er zijn **één aan enkele** relaties tussen entiteiten.
* Er zijn ingesloten gegevens die **niet vaak veranderen**.
* Er is ingesloten gegevens won't groeien **zonder gebonden**.
* Er is ingesloten gegevens **integraal** tot gegevens in een document.

> [!NOTE]
> Doorgaans data-modellen beter bieden gedenormaliseerd **lezen** prestaties.
> 
> 

### <a name="when-not-to-embed"></a>Wanneer u niet in te sluiten
De databasebestandsgrootte in een documentdatabase is denormalize alles en alle gegevens in één document insluiten, kan dit leiden tot een aantal situaties die moeten worden vermeden.

Deze JSON-fragment in beslag nemen.

    {
        "id": "1",
        "name": "What's new in the coolest Cloud",
        "summary": "A blog post by someone real famous",
        "comments": [
            {"id": 1, "author": "anon", "comment": "something useful, I'm sure"},
            {"id": 2, "author": "bob", "comment": "wisdom from the interwebs"},
            …
            {"id": 100001, "author": "jane", "comment": "and on we go ..."},
            …
            {"id": 1000000001, "author": "angry", "comment": "blah angry blah angry"},
            …
            {"id": ∞ + 1, "author": "bored", "comment": "oh man, will this ever end?"},
        ]
    }

Dit is mogelijk wat een post-entiteit met ingesloten opmerkingen eruit zou als we een typische blog of CMS, systeem zijn modelleren. Het probleem met dit voorbeeld is dat de matrix opmerkingen **unbounded**, wat betekent dat er geen limiet (praktische) aan het aantal opmerkingen eventuele één post kan hebben. Hiermee worden een probleem omdat de grootte van het document kan aanzienlijk toenemen.

Wanneer de grootte van het document de mogelijkheid om te verzenden van de gegevens over de kabel, evenals met het lezen en bijwerken van het document, op schaal, groeit zullen worden beïnvloed.

In dit geval zou het beter rekening houden met het volgende model zijn.

    Post document:
    {
        "id": "1",
        "name": "What's new in the coolest Cloud",
        "summary": "A blog post by someone real famous",
        "recentComments": [
            {"id": 1, "author": "anon", "comment": "something useful, I'm sure"},
            {"id": 2, "author": "bob", "comment": "wisdom from the interwebs"},
            {"id": 3, "author": "jane", "comment": "....."}
        ]
    }

    Comment documents:
    {
        "postId": "1"
        "comments": [
            {"id": 4, "author": "anon", "comment": "more goodness"},
            {"id": 5, "author": "bob", "comment": "tails from the field"},
            ...
            {"id": 99, "author": "angry", "comment": "blah angry blah angry"}
        ]
    },
    {
        "postId": "1"
        "comments": [
            {"id": 100, "author": "anon", "comment": "yet more"},
            ...
            {"id": 199, "author": "bored", "comment": "will this ever end?"}
        ]
    }

Dit model heeft de meest recente drie opmerkingen die zijn ingesloten in het bericht, hetgeen een matrix met een vaste ditmaal gebonden. De andere opmerkingen zijn gegroepeerd in naar batches van 100 opmerkingen en opgeslagen in afzonderlijke documenten. De grootte van de batch is gekozen als 100 omdat onze fictieve toepassing kan de gebruiker het laden van 100 opmerkingen tegelijk.  

Een andere aanvraag ingesloten gegevens waar niet een goed idee is is wanneer de ingesloten gegevens vaak op documenten gebruikt wordt en vaak worden gewijzigd. 

Deze JSON-fragment in beslag nemen.

    {
        "id": "1",
        "firstName": "Thomas",
        "lastName": "Andersen",
        "holdings": [
            {
                "numberHeld": 100,
                "stock": { "symbol": "zaza", "open": 1, "high": 2, "low": 0.5 }
            },
            {
                "numberHeld": 50,
                "stock": { "symbol": "xcxc", "open": 89, "high": 93.24, "low": 88.87 }
            }
        ]
    }

Dit kan leiden tot slot portfolio van een persoon. We hebben ervoor gekozen voor het insluiten van de vooraf gedefinieerde informatie in voor elk document portfolio. In een omgeving waar gerelateerde gegevens vaak wordt gewijzigd, gaat zoals een stock handel van toepassing, het insluiten van gegevens die regelmatig veranderen betekenen dat u elk document portfolio voortdurend wordt bijgewerkt telkens wanneer een bestand wordt verhandeld.

Stock *zaza* mogen worden verhandeld veel honderden keren in een enkele dag en duizenden gebruikers kunnen hebben *zaza* op hun portfolio. Met een gegevensmodel als de bovenstaande we moet bijwerken duizenden portfolio documenten vaak elke dag leidt tot een systeem dat won't schalen uitstekend. 

## <a id="Refer"></a>Verwijst naar gegevens
Dus mooi gegevens insluiten werkt voor veel gevallen maar duidelijk is dat er scenario's zijn wanneer uw gegevens denormalizing meer problemen dan het verdient aanbeveling. Dus wat we doen nu? 

Relationele databases zijn niet de enige plaats waar u de relaties tussen entiteiten kunt maken. U kunt de informatie in een document dat daadwerkelijk is gekoppeld aan gegevens in andere documenten hebben in een documentdatabase. Nu, ik ben niet pleiten zelfs een minuut dat wij systemen die worden beter voor een relationele database in Azure Cosmos DB of een andere documentdatabase geschikt zouden maken, maar eenvoudige relaties geen en kunnen zeer nuttig zijn. 

In de JSON we kiest voor het gebruik van het voorbeeld van een voorraad portfolio van eerder, maar deze tijd is de voorraad item in de portfolio in plaats van te sluiten. Wanneer het slot item vaak wijzigen gedurende de dag het enige document dat moet worden bijgewerkt, is enkele voorraad document. 

    Person document:
    {
        "id": "1",
        "firstName": "Thomas",
        "lastName": "Andersen",
        "holdings": [
            { "numberHeld":  100, "stockId": 1},
            { "numberHeld":  50, "stockId": 2}
        ]
    }

    Stock documents:
    {
        "id": "1",
        "symbol": "zaza",
        "open": 1,
        "high": 2,
        "low": 0.5,
        "vol": 11970000,
        "mkt-cap": 42000000,
        "pe": 5.89
    },
    {
        "id": "2",
        "symbol": "xcxc",
        "open": 89,
        "high": 93.24,
        "low": 88.87,
        "vol": 2970200,
        "mkt-cap": 1005000,
        "pe": 75.82
    }


Een onmiddellijke nadeel voor deze benadering is echter als uw toepassing vereist om aan te tonen informatie over elke voorraad die wordt veroorzaakt is bij het weergeven van een persoon portfolio; in dit geval moet u meerdere reizen aanbrengen in de database laden van de gegevens voor elk document voorraad. Hier hebt wordt een beslissing voor het verbeteren van de efficiëntie van schrijfbewerkingen die vaak gedurende de dag gebeuren, maar op zijn beurt inbreuk op de leesbewerkingen die mogelijk minder invloed op de prestaties van dit bepaalde systeem hebben genomen.

> [!NOTE]
> Genormaliseerd gegevensmodellen **kunt vereisen meer retouren** naar de server.
> 
> 

### <a name="what-about-foreign-keys"></a>Hoe zit het met refererende sleutels?
Omdat er momenteel geen concept van een beperking, refererende sleutel of anderszins, de relaties tussen document die u in documenten hebt zijn effectief 'zwakke koppelingen' en kunnen niet worden gecontroleerd door de database zelf. Als u zorgen dat de gegevens die een document verwijst wilt naar het daadwerkelijk bestaat, moet u dit doen in uw toepassing of door het gebruik van serverzijde triggers of opgeslagen procedures op Azure Cosmos DB.

### <a name="when-to-reference"></a>Wanneer u om te verwijzen naar
In het algemeen gebruikt genormaliseerde gegevens modellen wanneer:

* Voor **een-op-veel** relaties.
* Voor **veel-op-veel** relaties.
* Gerelateerde gegevens **regelmatig wordt gewijzigd**.
* Gegevens waarnaar wordt verwezen, kan worden **unbounded**.

> [!NOTE]
> Doorgaans normaliseren biedt betere **schrijven** prestaties.
> 
> 

### <a name="where-do-i-put-the-relationship"></a>Waar ik de relatie plaatsen?
De groei van de relatie kunt u bepalen in welke document voor het opslaan van de referentie.

Als we de onderstaande JSON modellen uitgevers en rapporten bekijken.

    Publisher document:
    {
        "id": "mspress",
        "name": "Microsoft Press",
        "books": [ 1, 2, 3, ..., 100, ..., 1000]
    }

    Book documents:
    {"id": "1", "name": "Azure Cosmos DB 101" }
    {"id": "2", "name": "Azure Cosmos DB for RDBMS Users" }
    {"id": "3", "name": "Taking over the world one JSON doc at a time" }
    ...
    {"id": "100", "name": "Learn about Azure Cosmos DB" }
    ...
    {"id": "1000", "name": "Deep Dive in to Azure Cosmos DB" }

Als het nummer van de boeken per uitgever met beperkte groei klein is, kan klikt u vervolgens de adresboek naslaginformatie voor de publisher-document opslaan nuttig zijn. Echter als het aantal boeken per uitgever unbounded is, zou klikt u vervolgens deze gegevensmodel leiden tot veranderlijke, groeiende matrices, zoals in het bovenstaande voorbeeld publisher document. 

Overschakelen van dingen rond een bits zou leiden tot een model die nog steeds vertegenwoordigt dezelfde gegevens, maar deze grote veranderlijke verzamelingen nu voorkomt.

    Publisher document: 
    {
        "id": "mspress",
        "name": "Microsoft Press"
    }

    Book documents: 
    {"id": "1","name": "Azure Cosmos DB 101", "pub-id": "mspress"}
    {"id": "2","name": "Azure Cosmos DB for RDBMS Users", "pub-id": "mspress"}
    {"id": "3","name": "Taking over the world one JSON doc at a time"}
    ...
    {"id": "100","name": "Learn about Azure Cosmos DB", "pub-id": "mspress"}
    ...
    {"id": "1000","name": "Deep Dive in to Azure Cosmos DB", "pub-id": "mspress"}

In het bovenstaande voorbeeld hebben we de unbounded verzameling verwijderd op de publisher-document. In plaats daarvan we zojuist hebben een verwijzing naar de uitgever op elk document adresboek.

### <a name="how-do-i-model-manymany-relationships"></a>Hoe ik veel: veel-relaties model?
In een relationele database *veel:* relaties zijn vaak gemodelleerd met join-tabellen die records uit andere tabellen NET samenvoegen. 

![Tabellen samenvoegen](./media/documentdb-modeling-data/join-table.png)

U kunt mogelijk geneigd te repliceren van hetzelfde met behulp van documenten en produceren van een gegevensmodel dat op het volgende lijkt.

    Author documents: 
    {"id": "a1", "name": "Thomas Andersen" }
    {"id": "a2", "name": "William Wakefield" }

    Book documents:
    {"id": "b1", "name": "Azure Cosmos DB 101" }
    {"id": "b2", "name": "Azure Cosmos DB for RDBMS Users" }
    {"id": "b3", "name": "Taking over the world one JSON doc at a time" }
    {"id": "b4", "name": "Learn about Azure Cosmos DB" }
    {"id": "b5", "name": "Deep Dive in to Azure Cosmos DB" }

    Joining documents: 
    {"authorId": "a1", "bookId": "b1" }
    {"authorId": "a2", "bookId": "b1" }
    {"authorId": "a1", "bookId": "b2" }
    {"authorId": "a1", "bookId": "b3" }

Dit zou werken. Echter, ofwel een auteur met hun books laden of het laden van een rapport met de auteur zou moet altijd ten minste twee extra query's op de database. Een query voor het gekoppelde document en vervolgens een andere query voor het ophalen van het document wordt toegevoegd. 

Als deze tabel join wordt alleen is lijmen samen twee soorten gegevens waarom niet vermeld het dan niet volledig?
Overweeg het volgende.

    Author documents:
    {"id": "a1", "name": "Thomas Andersen", "books": ["b1, "b2", "b3"]}
    {"id": "a2", "name": "William Wakefield", "books": ["b1", "b4"]}

    Book documents: 
    {"id": "b1", "name": "Azure Cosmos DB 101", "authors": ["a1", "a2"]}
    {"id": "b2", "name": "Azure Cosmos DB for RDBMS Users", "authors": ["a1"]}
    {"id": "b3", "name": "Learn about Azure Cosmos DB", "authors": ["a1"]}
    {"id": "b4", "name": "Deep Dive in to Azure Cosmos DB", "authors": ["a2"]}

Nu als ik had een auteur, ik onmiddellijk weet welke boeken deze geschreven en als u daarentegen als ik een boekdocument geladen had ik weet de id's van de auteur (s). Dit bespaart die tussenliggende-query op het verminderen van join tabel het nummer van server-retouren uw toepassing heeft om te nemen. 

## <a id="WrapUp"></a>Hybride gegevensmodellen
We nu hebt bekeken insluiten (of denormalizing) en gegevens van verwijzende (of normaliseren), hebben elk hun upsides en elk compromissen hebben, zoals we hebben gezien. 

Heeft geen altijd te zijn of niet worden Blazende naar dingen enigszins van elkaar. 

Op basis van de specifieke gebruikspatronen en werkbelastingen die mogelijk zijn er gevallen waarbij de combinatie van ingesloten van uw toepassing en gegevens waarnaar wordt verwezen is wel zinvol en kan leiden tot eenvoudiger toepassingslogica met minder server retouren tegelijkertijd nog steeds een goede niveau van de prestaties.

Houd rekening met de volgende JSON. 

    Author documents: 
    {
        "id": "a1",
        "firstName": "Thomas",
        "lastName": "Andersen",        
        "countOfBooks": 3,
         "books": ["b1", "b2", "b3"],
        "images": [
            {"thumbnail": "http://....png"}
            {"profile": "http://....png"}
            {"large": "http://....png"}
        ]
    },
    {
        "id": "a2",
        "firstName": "William",
        "lastName": "Wakefield",
        "countOfBooks": 1,
        "books": ["b1"],
        "images": [
            {"thumbnail": "http://....png"}
        ]
    }

    Book documents:
    {
        "id": "b1",
        "name": "Azure Cosmos DB 101",
        "authors": [
            {"id": "a1", "name": "Thomas Andersen", "thumbnailUrl": "http://....png"},
            {"id": "a2", "name": "William Wakefield", "thumbnailUrl": "http://....png"}
        ]
    },
    {
        "id": "b2",
        "name": "Azure Cosmos DB for RDBMS Users",
        "authors": [
            {"id": "a1", "name": "Thomas Andersen", "thumbnailUrl": "http://....png"},
        ]
    }

Hier hebt we het ingesloten model, waarbij gegevens uit andere entiteiten zijn ingesloten in het document op het hoogste niveau, maar andere gegevens wordt verwezen (voornamelijk) gevolgd. 

Als u het document adresboek bekijkt, ziet u enkele interessante velden bij de matrix van auteurs. Er is een *id* veld dit is het veld we gebruiken terug te verwijzen naar een auteur-document standaardprocedure in een genormaliseerde model, maar vervolgens we hebben ook *naam* en *thumbnailUrl*. Kan we zojuist hebt vastgelopen met *id* en links van de toepassing eventuele aanvullende informatie nodig ophalen uit het respectieve auteur-document met behulp van de 'koppeling', maar omdat onze toepassing de naam van de auteur en een miniatuur geeft met elke adresboek weergegeven kunnen we een round trip opslaan op de server per rapport in een lijst door denormalizing **sommige** gegevens van de auteur.

Ervoor dat, als de naam van de auteur gewijzigd of dat ze hun photo die zou moeten we een update gaat bijwerken wilden elk rapport ze ooit gepubliceerd maar voor onze toepassing, op basis van de veronderstelling dat auteurs niet hun namen vaak veranderen, is dit een acceptabele ontwerpbeslissing.  

In het voorbeeld zijn er **vooraf berekende aggregaties** waarden op te slaan dure verwerking op een leesbewerking. In het voorbeeld is enkele van de gegevens die zijn ingesloten in het document de auteur van gegevens die tijdens de uitvoering wordt berekend. Telkens wanneer een nieuw rapport is gepubliceerd, wordt een boekdocument gemaakt **en** countOfBooks veld is ingesteld voor een berekende waarde op basis van het aantal documenten adresboek die beschikbaar zijn voor een bepaalde auteur. Deze optimalisatie zou zijn goede in lezen zware systemen waar we doen van berekeningen bij het schrijven om te optimaliseren leesbewerkingen toestaan.

De mogelijkheid om een model met vooraf berekende velden wordt mogelijk gemaakt omdat Azure Cosmos DB ondersteunt **transacties met meerdere documenten**. Veel NoSQL-opslag kunnen doen transacties meerdere documenten en daarom pleiten ontwerpbeslissingen, zoals 'altijd insluiten alles', vanwege deze beperking. U kunt met Azure Cosmos DB serverzijde triggers of opgeslagen procedures, die books invoegen en auteurs allemaal binnen een ACID-transactie bijwerken gebruiken. Nu u geen **hebben** voor het insluiten van alles in één document net om ervoor te zorgen dat de gegevens consistent blijft.

## <a name="NextSteps"></a>Volgende stappen
De grootste takeaways uit dit artikel is om te begrijpen dat modelleren in een wereld schemavrije gegevens even belangrijk is als ooit. 

Net als er geen enkele manier een stukje informatie op een scherm vertegenwoordigt is, is er geen eenduidige manier om uw gegevens te modelleren. U moet uw toepassing en hoe deze wordt produceert, gebruiken en de gegevens verwerken. Vervolgens kunt u door het toepassen van enkele van de richtlijnen die hier wordt gepresenteerd ook instellen over het maken van een model die zijn gericht op de onmiddellijke behoeften van uw toepassing. Als uw toepassingen wijzigen wilt, kunt u gebruikmaken van de flexibiliteit van een schema gratis database over te gaan die wijzigen en het gegevensmodel van uw eenvoudig ontwikkelen. 

Raadpleeg voor meer informatie over Azure Cosmos DB, van de service [documentatie](https://azure.microsoft.com/documentation/services/cosmos-db/) pagina. 

Om te begrijpen hoe naar shard van uw gegevens over meerdere partities verwijzen naar [partitioneren van gegevens in Azure Cosmos DB](documentdb-partition-data.md). 
