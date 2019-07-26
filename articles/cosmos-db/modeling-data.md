---
title: Model gegevens in Azure Cosmos DB
titleSuffix: Azure Cosmos DB
description: Meer informatie over het modelleren van de gegevens in NoSQL-databases, verschillen tussen het modelleren van gegevens in een relationele database en een documentdatabase.
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 07/23/2019
ms.author: rimman
ms.custom: rimman
ms.openlocfilehash: da119b2858c6b6c7bbc99b40d340f79964e0fae3
ms.sourcegitcommit: c72ddb56b5657b2adeb3c4608c3d4c56e3421f2c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/24/2019
ms.locfileid: "68467883"
---
# <a name="data-modeling-in-azure-cosmos-db"></a>Gegevens modellering in Azure Cosmos DB

U kunt zonder schema-Free data bases, zoals Azure Cosmos DB, het eenvoudig maken om ongestructureerde en semi-gestructureerde gegevens op te slaan en op te vragen, maar u kunt het beste even best Eden aan uw gegevens model om optimaal te profiteren van de prestaties en schaal baarheid en de laagste goedkope.

Hoe er gegevens worden opgeslagen? Hoe uw toepassing gaat ophalen en gegevens op te vragen? Is uw toepassing lezen/zwaar of schrijven-zwaar?

Na het lezen van dit artikel, kunt u zich de volgende vragen beantwoorden:

* Wat is gegevensmodellering en waarom moet ik care?
* Hoe worden model gegevens in Azure Cosmos DB afwijkend van een relationele data base?
* Hoe ik de relaties tussen gegevens in een niet-relationele database express?
* Wanneer insluiten gegevens en wanneer kan ik koppelen aan gegevens?

## <a name="embedding-data"></a>Gegevens insluiten

Wanneer u gegevens modelleert in Azure Cosmos DB probeert u uw entiteiten te behandelen als **zelfstandige items** die worden weer gegeven als JSON-documenten.

Laten we eerst zien hoe we gegevens in een relationele data base kunnen model leren. Het volgende voorbeeld laat zien hoe een persoon kan worden opgeslagen in een relationele database.

![Relationele database-model](./media/sql-api-modeling-data/relational-data-model.png)

Bij het werken met relationele data bases is de strategie het normaliseren van al uw gegevens. Het normaliseren van uw gegevens omvat meestal het nemen van een entiteit, zoals een persoon, en het opsplitsen daarvan in discrete onderdelen. In het bovenstaande voor beeld kan een persoon meerdere contact gegevens records hebben, evenals meerdere adres records. De contact gegevens kunnen verder worden opgesplitst door algemene velden, zoals een type, verder uit te pakken. Hetzelfde geldt voor het adres, elke record kan van het type *thuis* of *zakelijk*zijn.

De begeleiden premises wanneer de gegevens normaliseren wordt **te voorkomen dat opslaan van redundante gegevens** op elk vastleggen en in plaats daarvan verwijst naar gegevens. In dit voor beeld, om een persoon te lezen met al hun contact gegevens en-adressen, moet u samen voegingen gebruiken om uw gegevens in runtime effectief te maken (of te denormaliseren).

    SELECT p.FirstName, p.LastName, a.City, cd.Detail
    FROM Person p
    JOIN ContactDetail cd ON cd.PersonId = p.Id
    JOIN ContactDetailType cdt ON cdt.Id = cd.TypeId
    JOIN Address a ON a.PersonId = p.Id

Een en dezelfde persoon bijwerken met hun contactgegevens en adressen van vereist schrijfbewerkingen voor veel afzonderlijke tabellen.

Laten we nu eens kijken hoe we dezelfde gegevens als een op zichzelf staande entiteit in Azure Cosmos DB model leren.

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
            {"email": "thomas@andersen.com"},
            {"phone": "+1 555 555-5555", "extension": 5555}
        ]
    }

Met behulp van de bovenstaande  benadering hebben we de persoons record genormaliseerd door alle informatie met betrekking tot deze persoon, zoals de contact gegevens en-adressen, in te **sluiten** in *één JSON* -document.
Bovendien hebben we de flexibiliteit voor handelingen zoals met gegevens van de contactpersoon van verschillende vormen volledig omdat we niet tot een vast schema beperkt bent.

Het ophalen van een volledige persoons record uit de data base is nu een **enkele Lees bewerking** voor één container en voor één item. Het bijwerken van een persoons record, met hun contact gegevens en-adressen, is ook een **enkele schrijf bewerking** voor één item.

Door denormalizing gegevens, moet uw toepassing mogelijk minder query's en updates voor algemene bewerkingen worden voltooid.

### <a name="when-to-embed"></a>Bij het insluiten

In het algemeen gebruikt ingesloten gegevens modellen wanneer:

* Er zijn **relaties** tussen entiteiten.
* Er zijn **één-op-paar** relaties tussen entiteiten.
* Er zijn ingesloten gegevens die **niet vaak worden gewijzigd**.
* Er zijn Inge sloten gegevens die niet worden uitgebreid **zonder gebonden**.
* Er zijn Inge sloten gegevens die **vaak samen worden opgevraagd**.

> [!NOTE]
> Gewoonlijk gedenormaliseerd gegevens modellen betere bieden **lezen** prestaties.

### <a name="when-not-to-embed"></a>Als niet in te sluiten

De regel van de vuist in Azure Cosmos DB is om alles te denormaliseren en alle gegevens in één item in te sluiten, maar dit kan leiden tot enkele situaties die moeten worden vermeden.

Deze JSON-codefragment duren.

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

Dit wordt mogelijk wat een post-entiteit met ingesloten opmerkingen zou er als volgt uitzien als we een typische blog of CMS, system zijn modelleren. Het probleem met dit voorbeeld is dat de matrix opmerkingen **niet-gebonden**, wat betekent dat er is geen (praktische) limiet voor het aantal opmerkingen een enkel bericht kan hebben. Dit kan een probleem zijn omdat de grootte van het item oneindig groot kan worden uitgebreid.

Naarmate de grootte van het item groeit, is de mogelijkheid om gegevens over de kabel te verzenden en om het item op de juiste schaal te lezen en bij te werken, van invloed.

In dit geval is het beter om rekening te houden met het volgende gegevens model.

    Post item:
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

    Comment items:
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

Dit model bevat de drie meest recente opmerkingen die zijn Inge sloten in de post-container, een matrix met een vaste set kenmerken. De overige opmerkingen worden gegroepeerd in batches van 100 opmerkingen en opgeslagen als afzonderlijke items. De grootte van de batch is gekozen als 100 omdat onze fictieve toepassing kan de gebruiker 100 opmerkingen tegelijk worden geladen.  

Een andere geval waarin het insluiten van gegevens geen goed idee is wanneer de Inge sloten gegevens vaak worden gebruikt in verschillende items en regel matig worden gewijzigd.

Deze JSON-codefragment duren.

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

Dit kan leiden tot slot portfolio van een persoon. We hebben ervoor gekozen de aandelen informatie in elk portfolio document in te sluiten. In een omgeving waar de gerelateerde gegevens vaak worden gewijzigd, gaat zoals een voorraad trading van toepassing, voor het insluiten van gegevens die vaak veranderen betekenen dat u elk document portfolio voortdurend wordt bijgewerkt telkens wanneer een bestand wordt verhandeld.

Voorraad *zaza* mogen worden verhandeld veel honderd keer in één dag en duizenden gebruikers kunnen hebben *zaza* op hun portfolio. Met een gegevensmodel, zoals de bovenstaande we zelf zou moeten werken duizenden portfolio documenten aantal keren dat elke dag leidt tot een systeem dat wordt niet goed worden geschaald.

## <a name="referencing-data"></a>Verwijzen naar gegevens

Het insluiten van gegevens werkt goed voor veel gevallen, maar er zijn scenario's bij het denormaliseren van uw gegevens, waardoor er meer problemen zijn dan het waard is. Dus wat we doen nu?

Relationele databases zijn niet de enige plaats waar u relaties tussen entiteiten kunt maken. In een document database kunt u gegevens in één document hebben die betrekking hebben op gegevens in andere documenten. We raden u aan om geen systemen te bouwen die beter geschikt zijn voor een relationele data base in Azure Cosmos DB of een andere document database, maar eenvoudige relaties zijn prima en kunnen nuttig zijn.

In de onderstaande JSON, we hebben gekozen voor het voorbeeld van een voorraad portfolio van eerder gebruik, maar deze keer verwijzen we naar het aandeel item in de portfolio in plaats van het insluiten van inhoud. Deze manier wordt de voorraad item vaak wijzigen gedurende de dag het enige document dat moet worden bijgewerkt, is één aandelen document.

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

Een onmiddellijke nadeel deze benadering is echter als uw toepassing vereist voor het weergeven van informatie over elke voorraad die wordt bewaard is bij het weergeven van een persoon portfolio; in dit geval moet u meerdere trips aanbrengen in de database om de informatie voor elk document dat aandelen te laden. We hebben hier een beslissing om de efficiëntie van schrijfbewerkingen die vaak gedurende de dag gebeuren, maar op zijn beurt voor de leesbewerkingen die mogelijk minder invloed op de prestaties van deze bepaalde systeem hebben worden aangetast gemaakt.

> [!NOTE]
> Gegevensmodellen genormaliseerd **kan vereisen meer retouren** naar de server.

### <a name="what-about-foreign-keys"></a>Hoe zit het met refererende sleutels?

Omdat er momenteel geen concept van een beperking, refererende sleutel of anderszins, eventuele relaties tussen document dat u in documenten hebt zijn effectief "zwakke links" en wordt niet gecontroleerd door de database zelf. Als u zorgen dat de gegevens die een document wordt verwezen wilt naar het daadwerkelijk bestaat, moet u dit doen in uw toepassing, of door het gebruik van server-side-triggers of opgeslagen procedures op Azure Cosmos DB.

### <a name="when-to-reference"></a>Wanneer u om te verwijzen naar

In het algemeen gebruikt genormaliseerde gegevens modellen wanneer:

* Voor **een-op-veel** relaties.
* Voor **veel-op-veel** relaties.
* Gerelateerde gegevens **regelmatig wordt gewijzigd**.
* Waarnaar wordt verwezen, gegevens kan worden **niet-gebonden**.

> [!NOTE]
> Normaal gesproken normaliseren biedt betere **schrijven** prestaties.

### <a name="where-do-i-put-the-relationship"></a>Waar ik de relatie moet plaatsen?

De groei van de relatie kunt u bepalen in welke document voor het opslaan van de referentie.

Als we kijken naar de onderstaande JSON van uitgevers en boeken modellen.

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
    {"id": "1000", "name": "Deep Dive into Azure Cosmos DB" }

Als het aantal boeken per uitgever kleine met een groei van beperkt is, kan klikt u vervolgens opslaan van het boek naslaginformatie voor de publisher-document nuttig zijn. Echter, als het aantal boeken per publisher niet-gebonden is, vervolgens dit gegevensmodel zou leiden tot veranderlijke, groeiende matrices, zoals in het bovenstaande voorbeeld van de uitgever document.

Schakelen tussen dingen rond een bits zou leiden tot een model dat u nog steeds vertegenwoordigt de dezelfde gegevens, maar nu voorkomt deze grote veranderlijke verzamelingen.

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
    {"id": "1000","name": "Deep Dive into Azure Cosmos DB", "pub-id": "mspress"}

In het bovenstaande voorbeeld hebben we de niet-gebonden verzameling verwijderd op de publisher-document. In plaats daarvan hebben we net een verwijzing naar de uitgever op elk boekdocument.

### <a name="how-do-i-model-manymany-relationships"></a>Hoe ik veel: veel-relaties model?

In een relationele database *veel:* relaties worden vaak gebaseerd met join-tabellen, die alleen records uit andere tabellen samen samenvoegen.

![Tabellen samenvoegen](./media/sql-api-modeling-data/join-table.png)

Het is mogelijk dat er geneigd te repliceren van hetzelfde te doen met behulp van documenten en produceren van een gegevensmodel dat op het volgende lijkt.

    Author documents:
    {"id": "a1", "name": "Thomas Andersen" }
    {"id": "a2", "name": "William Wakefield" }

    Book documents:
    {"id": "b1", "name": "Azure Cosmos DB 101" }
    {"id": "b2", "name": "Azure Cosmos DB for RDBMS Users" }
    {"id": "b3", "name": "Taking over the world one JSON doc at a time" }
    {"id": "b4", "name": "Learn about Azure Cosmos DB" }
    {"id": "b5", "name": "Deep Dive into Azure Cosmos DB" }

    Joining documents:
    {"authorId": "a1", "bookId": "b1" }
    {"authorId": "a2", "bookId": "b1" }
    {"authorId": "a1", "bookId": "b2" }
    {"authorId": "a1", "bookId": "b3" }

Dit zou moeten werken. Echter, het laden van een van beide een auteur met hun boeken, of het laden van een rapport met de auteur, zou altijd ten minste twee extra query's voor de database. Een query naar het lid te worden document en vervolgens een andere query voor het ophalen van het document wordt toegevoegd.

Als alle gaat met deze join-tabel is lijmen samen twee soorten gegevens, waarom niet vermeld het dan niet volledig?
Overweeg het volgende.

    Author documents:
    {"id": "a1", "name": "Thomas Andersen", "books": ["b1, "b2", "b3"]}
    {"id": "a2", "name": "William Wakefield", "books": ["b1", "b4"]}

    Book documents:
    {"id": "b1", "name": "Azure Cosmos DB 101", "authors": ["a1", "a2"]}
    {"id": "b2", "name": "Azure Cosmos DB for RDBMS Users", "authors": ["a1"]}
    {"id": "b3", "name": "Learn about Azure Cosmos DB", "authors": ["a1"]}
    {"id": "b4", "name": "Deep Dive into Azure Cosmos DB", "authors": ["a2"]}

Nu als ik een auteur had, weet ik dan onmiddellijk welke boeken ze hebben geschreven en omgekeerd als ik een boek document had geladen ik weet wat de Id's van de auteur (s) zijn. Hiermee slaat u deze tussenliggende query op basis van het join-tabel verminderen het aantal servers netwerkretouren uw toepassing heeft om te maken.

## <a name="hybrid-data-models"></a>Hybride gegevens modellen

We nu hebt bekeken insluiten (of denormalizing) en gegevens van verwijzende (of normaliseren), hebben elk hun upsides en elk compromissen hebben, zoals we hebben gezien.

Dit hoeft altijd te zijn of niet worden Blazende om dingen van iets.

Op basis van de specifieke gebruikspatronen van uw toepassing en de werkbelastingen die mogelijk zijn er gevallen waarbij een combinatie van ingesloten en zinvol is waarnaar wordt verwezen, gegevens en kan leiden tot eenvoudiger toepassingslogica met de server minder netwerkretouren terwijl een goede prestatieniveau gehandhaafd blijft.

Houd rekening met de volgende JSON.

    Author documents:
    {
        "id": "a1",
        "firstName": "Thomas",
        "lastName": "Andersen",
        "countOfBooks": 3,
        "books": ["b1", "b2", "b3"],
        "images": [
            {"thumbnail": "https://....png"}
            {"profile": "https://....png"}
            {"large": "https://....png"}
        ]
    },
    {
        "id": "a2",
        "firstName": "William",
        "lastName": "Wakefield",
        "countOfBooks": 1,
        "books": ["b1"],
        "images": [
            {"thumbnail": "https://....png"}
        ]
    }

    Book documents:
    {
        "id": "b1",
        "name": "Azure Cosmos DB 101",
        "authors": [
            {"id": "a1", "name": "Thomas Andersen", "thumbnailUrl": "https://....png"},
            {"id": "a2", "name": "William Wakefield", "thumbnailUrl": "https://....png"}
        ]
    },
    {
        "id": "b2",
        "name": "Azure Cosmos DB for RDBMS Users",
        "authors": [
            {"id": "a1", "name": "Thomas Andersen", "thumbnailUrl": "https://....png"},
        ]
    }

Hier hebben we de ingesloten model, waarbij gegevens uit andere entiteiten zijn ingesloten in het document op het hoogste niveau, maar andere gegevens wordt verwezen (meestal) gevolgd.

Als u het boekdocument bekijkt, zien we enkele interessante velden als we kijken naar de matrix van auteurs. Er is een `id` veld dat het veld wordt gebruikt om terug te verwijzen naar een auteur-document, de standaard procedure in een genormaliseerd model, maar we `name` hebben `thumbnailUrl`ook en. We hebben de toepassing kunnen `id` blijven gebruiken om aanvullende informatie op te halen uit het respectieve auteurs document met behulp van de ' koppeling ', maar omdat in de toepassing de naam van de auteur en een miniatuur afbeelding worden weer gegeven voor elk boek weer gegeven we kunnen een retour naar de server per boek in een lijst opslaan door **bepaalde** gegevens van de auteur te denormaliseren.

Als de naam van de auteur is gewijzigd of als hij of zij de foto wil bijwerken, moeten we dan ook elk boek dat ze ooit hebben gepubliceerd, maar voor onze toepassing bijwerken, op basis van de veronderstelling dat auteurs hun namen niet vaak wijzigen, is dit een acceptabel ontwerp besluit.  

In het voorbeeld zijn er **vooraf berekende aggregaties** waarden dure verwerking opslaan op een leesbewerking. In het voorbeeld zijn sommige van de gegevens die zijn ingesloten in het document de auteur van gegevens die tijdens de uitvoering wordt berekend. Telkens wanneer een nieuw rapport is gepubliceerd, wordt een boekdocument gemaakt **en** het veld countOfBooks is ingesteld op een berekende waarde op basis van het aantal book documenten die beschikbaar zijn voor een bepaalde auteur. Deze optimalisatie is goed in lezen zware systemen waar we erg berekeningen op schrijfbewerkingen doen om het optimaliseren van leesbewerkingen.

De mogelijkheid om een model met vooraf berekende velden wordt mogelijk gemaakt omdat Azure Cosmos DB biedt ondersteuning voor **transacties met meerdere documenten**. Veel NoSQL-archieven kunnen pas transacties meerdere documenten en daarom advocate ontwerpbeslissingen, zoals "altijd insluiten van alles wat' vanwege deze beperking. Met Azure Cosmos DB, kunt u server-side-triggers of opgeslagen procedures, die rapporten invoeg- en auteurs allemaal binnen een ACID-transactie. Nu hoeft u **niet alles** in één document in te sluiten om er zeker van te zijn dat uw gegevens consistent blijven.

## <a name="distinguishing-between-different-document-types"></a>Onderscheiden van verschillende document typen

In sommige scenario's wilt u mogelijk verschillende document typen combi neren in dezelfde verzameling. Dit is meestal het geval wanneer u wilt dat er meerdere, gerelateerde documenten zich in dezelfde [partitie](partitioning-overview.md)bevinden. U kunt bijvoorbeeld zowel boeken als boek beoordelingen in dezelfde verzameling plaatsen en partitioneren door `bookId`. In dat geval wilt u meestal aan uw documenten toevoegen met een veld dat het type identificeert om ze te onderscheiden.

    Book documents:
    {
        "id": "b1",
        "name": "Azure Cosmos DB 101",
        "bookId": "b1",
        "type": "book"
    }

    Review documents:
    {
        "id": "r1",
        "content": "This book is awesome",
        "bookId": "b1",
        "type": "review"
    },
    {
        "id": "r2",
        "content": "Best book ever!",
        "bookId": "b1",
        "type": "review"
    }

## <a name="next-steps"></a>Volgende stappen

De grootste takeaways uit dit artikel zijn om te begrijpen dat gegevens modelleren in een wereld schema's kunnen net zo belangrijk als ooit worden.

Net zoals er geen enkele manier om weer te geven van een hoeveelheid gegevens op een scherm is, is er geen eenduidige manier om uw gegevens modelleren. U moet voor informatie over uw toepassing en hoe deze wordt produceren, gebruiken en de gegevens te verwerken. Vervolgens kunt u door het toepassen van enkele van de richtlijnen die hier wordt gepresenteerd ook instellen over het maken van een model dat de onmiddellijke behoeften van uw toepassing adressen. Wanneer u uw toepassingen wilt wijzigen, kunt u gebruikmaken van de flexibiliteit van een schema's database over te gaan die wijzigen en uw gegevensmodel eenvoudig kunt veranderen.

Raadpleeg voor meer informatie over Azure Cosmos DB, van de service [documentatie](https://azure.microsoft.com/documentation/services/cosmos-db/) pagina.

Om te begrijpen hoe sharden uw gegevens over meerdere partities verwijzen naar [partitioneren van gegevens in Azure Cosmos DB](sql-api-partition-data.md).

Raadpleeg voor meer informatie over het model leren en partitioneren van gegevens op Azure Cosmos DB met behulp van een echt voor beeld van [gegevens modellering en partitionering: een praktijk voorbeeld](how-to-model-partition-example.md).
