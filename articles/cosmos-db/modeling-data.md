---
title: Documentgegevens modelleren in een NoSQL-database
titleSuffix: Azure Cosmos DB
description: Meer informatie over het modelleren van de gegevens in NoSQL-databases, verschillen tussen het modelleren van gegevens in een relationele database en een documentdatabase.
author: aliuy
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/06/2018
ms.author: andrl
ms.custom: seodec18
ms.openlocfilehash: a6781c3a94789b26beb85a9a3df3166ec47622bb
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54041574"
---
# <a name="modeling-document-data-for-nosql-databases"></a>Modellering van documentgegevens van het voor NoSQL-databases

Terwijl databases zonder schema, zoals Azure Cosmos DB, u kunnen heel eenvoudig om te spelen op wijzigingen in uw gegevensmodel moet nog steeds besteedt u bepaalde tijd na te denken over uw gegevens. 

Hoe er gegevens worden opgeslagen? Hoe uw toepassing gaat ophalen en gegevens op te vragen? Is uw toepassing dikke lezen of schrijven zware? 

Na het lezen van dit artikel, kunt u zich de volgende vragen beantwoorden:

* Wat moet ik een document in een documentdatabase doen?
* Wat is gegevensmodellering en waarom moet ik care? 
* Hoe verschilt modellering van gegevens in een documentdatabase voor een relationele database?
* Hoe ik de relaties tussen gegevens in een niet-relationele database express?
* Wanneer insluiten gegevens en wanneer kan ik koppelen aan gegevens?

## <a name="embedding-data"></a>Gegevens insluiten
Wanneer u begint met het modelleren van gegevens in een document-store, zoals Azure Cosmos DB, probeert te behandelen van uw entiteiten als **zichzelf documenten** in JSON weergegeven.

Voordat we dieper te veel verder, laat het ons weer een paar stappen uitvoeren en kijken hoe we iets in een relationele database, een onderwerp veel mensen al bekend met bent mogelijk model. Het volgende voorbeeld laat zien hoe een persoon kan worden opgeslagen in een relationele database. 

![Relationele database-model](./media/sql-api-modeling-data/relational-data-model.png)

Bij het werken met relationele databases, hebben we geleerd jaar te normaliseren, normaliseren, normaliseren.

Doorgaans normaliseren van uw gegevens omvat het maken van een entiteit, zoals een persoon en splitsen in afzonderlijke stukken van gegevens. In het bovenstaande voorbeeld hebben van een persoon meerdere records van de details van contactpersonen, evenals de meerdere-adresrecords. We nog een stap verder gaan en contactgegevens onderverdeeld per verdere extraheren algemene velden, zoals een type. Hetzelfde adres, heeft elke record hier een type, zoals *Start* of *Business* 

De begeleiden premises wanneer de gegevens normaliseren wordt **te voorkomen dat opslaan van redundante gegevens** op elk vastleggen en in plaats daarvan verwijst naar gegevens. In dit voorbeeld om te lezen van een persoon, met al hun gegevens van de contactpersoon en -adressen, moet u met JOINS effectief Verzamel uw gegevens tijdens de uitvoering.

    SELECT p.FirstName, p.LastName, a.City, cd.Detail
    FROM Person p
    JOIN ContactDetail cd ON cd.PersonId = p.Id
    JOIN ContactDetailType on cdt ON cdt.Id = cd.TypeId
    JOIN Address a ON a.PersonId = p.Id

Een en dezelfde persoon bijwerken met hun contactgegevens en adressen van vereist schrijfbewerkingen voor veel afzonderlijke tabellen. 

Nu eens kijken hoe we dezelfde gegevens als een zelfstandige entiteit in een documentdatabase zou model.

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

Met behulp van de bovenstaande benadering, nu we hebben **gedenormaliseerde** de persoon vastleggen waar we **ingesloten** alle informatie met betrekking tot deze persoon, zoals hun contactgegevens en -adressen in een enkel JSON het document.
Bovendien hebben we de flexibiliteit voor handelingen zoals met gegevens van de contactpersoon van verschillende vormen volledig omdat we niet tot een vast schema beperkt bent. 

Het ophalen van een die volledige persoonsrecord uit de database is nu één bewerking op basis van één verzameling en voor één document lezen. Bijwerken van de persoonsrecord van een, met hun contactgegevens en -adressen, is ook een één schrijfbewerking voor één document.

Door denormalizing gegevens, moet uw toepassing mogelijk minder query's en updates voor algemene bewerkingen worden voltooid. 

### <a name="when-to-embed"></a>Bij het insluiten
In het algemeen gebruikt ingesloten gegevens modellen wanneer:

* Er zijn opgenomen ** relaties tussen entiteiten.
* Er zijn **één-op-paar** relaties tussen entiteiten.
* Er zijn ingesloten gegevens die **niet vaak worden gewijzigd**.
* Er is ingesloten gegevens wordt niet groeien **zonder gebonden**.
* Er zijn ingesloten gegevens die **integraal** met gegevens in een document.

> [!NOTE]
> Gewoonlijk gedenormaliseerd gegevens modellen betere bieden **lezen** prestaties.
> 
> 

### <a name="when-not-to-embed"></a>Als niet in te sluiten
Terwijl de databasebestandsgrootte in een documentdatabase is te denormaliseren alles en alle gegevens in één document insluiten, kan dit leiden tot bepaalde situaties die moeten worden vermeden.

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

Dit wordt mogelijk wat een post-entiteit met ingesloten opmerkingen zou er als volgt uitzien als we een typische blog of CMS, system zijn modelleren. Het probleem met dit voorbeeld is dat de matrix opmerkingen **niet-gebonden**, wat betekent dat er is geen (praktische) limiet voor het aantal opmerkingen een enkel bericht kan hebben. Hiermee wordt een probleem als de grootte van het document kan aanzienlijk toenemen.

Wanneer de grootte van het document de mogelijkheid voor het verzenden van de gegevens via de kabel, evenals met het lezen en bijwerken van het document, op schaal, groeit wordt beïnvloed.

In dit geval zou het beter om te overwegen het volgende model zijn.

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

Dit model heeft de drie meest recente opmerkingen ingesloten in het bericht, dit een matrix met een vaste is afhankelijk van dit moment. De andere opmerkingen worden gegroepeerd in batches van 100 opmerkingen en opgeslagen in afzonderlijke documenten. De grootte van de batch is gekozen als 100 omdat onze fictieve toepassing kan de gebruiker 100 opmerkingen tegelijk worden geladen.  

Een andere aanvraag ingesloten gegevens waar niet een goed idee is is wanneer de ingesloten gegevens vaak voor documenten gebruikt wordt en vaak worden gewijzigd. 

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

Dit kan leiden tot slot portfolio van een persoon. We hebben gekozen om in te sluiten van de voorraad gegevens in elk document portfolio. In een omgeving waar de gerelateerde gegevens vaak worden gewijzigd, gaat zoals een voorraad trading van toepassing, voor het insluiten van gegevens die vaak veranderen betekenen dat u elk document portfolio voortdurend wordt bijgewerkt telkens wanneer een bestand wordt verhandeld.

Voorraad *zaza* mogen worden verhandeld veel honderd keer in één dag en duizenden gebruikers kunnen hebben *zaza* op hun portfolio. Met een gegevensmodel, zoals de bovenstaande we zelf zou moeten werken duizenden portfolio documenten aantal keren dat elke dag leidt tot een systeem dat wordt niet goed worden geschaald. 

## <a id="Refer"></a>Verwijst naar gegevens
Dus gegevens insluiten goed werkt voor veel gevallen maar het is duidelijk dat er scenario's zijn wanneer uw gegevens denormalizing meer problemen dan de moeite waard. Dus wat we doen nu? 

Relationele databases zijn niet de enige plaats waar u relaties tussen entiteiten kunt maken. U kunt de informatie in één document die daadwerkelijk zijn gekoppeld aan gegevens in andere documenten hebben in een documentdatabase. Nu ik ben niet dat pleit voor zelfs één minuut dat we bij het bouwen van systemen die beter zou zijn geschikt voor een relationele database in Azure Cosmos DB, of een andere documentdatabase, maar eenvoudig relaties in orde zijn en kunnen nuttig zijn. 

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
> 
> 

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
> 
> 

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

Nu, als ik had een auteur, ik direct weten welke boeken die ze hebben geschreven en als ik een van geladen boekdocument zou ik daarentegen de id van de auteur (s) kent. Hiermee slaat u deze tussenliggende query op basis van het join-tabel verminderen het aantal servers netwerkretouren uw toepassing heeft om te maken. 

## <a id="WrapUp"></a>Hybride gegevensmodellen
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

Als u het boekdocument bekijkt, zien we enkele interessante velden als we kijken naar de matrix van auteurs. Er is een *id* veld dat wil zeggen het veld we gebruiken terug te verwijzen naar een auteur-document standaardprocedure in een genormaliseerde model, maar vervolgens hebben we ook *naam* en *thumbnailUrl*. We kunnen zijn vastgelopen met *id* en links van de toepassing om op te halen van eventuele aanvullende informatie die nodig is van het respectieve auteur-document met behulp van de 'koppeling', maar omdat onze toepassing de naam van de auteur en een miniatuur met geeft elk rapport weergegeven we kunnen een retour op de server opslaan per rapport in een lijst met door denormalizing **sommige** gegevens van de auteur.

Zorg ervoor dat, als de naam van de auteur gewijzigd of zij wil bijwerken hun foto zelf zou moeten we gaan een update elke book ze ooit gepubliceerd, maar voor de toepassing die is gebaseerd op de veronderstelling dat auteurs hun namen vaak niet wijzigen, dit is een acceptabele ontwerpbesluit.  

In het voorbeeld zijn er **vooraf berekende aggregaties** waarden dure verwerking opslaan op een leesbewerking. In het voorbeeld zijn sommige van de gegevens die zijn ingesloten in het document de auteur van gegevens die tijdens de uitvoering wordt berekend. Telkens wanneer een nieuw rapport is gepubliceerd, wordt een boekdocument gemaakt **en** het veld countOfBooks is ingesteld op een berekende waarde op basis van het aantal book documenten die beschikbaar zijn voor een bepaalde auteur. Deze optimalisatie is goed in lezen zware systemen waar we erg berekeningen op schrijfbewerkingen doen om het optimaliseren van leesbewerkingen.

De mogelijkheid om een model met vooraf berekende velden wordt mogelijk gemaakt omdat Azure Cosmos DB biedt ondersteuning voor **transacties met meerdere documenten**. Veel NoSQL-archieven kunnen pas transacties meerdere documenten en daarom advocate ontwerpbeslissingen, zoals "altijd insluiten van alles wat' vanwege deze beperking. Met Azure Cosmos DB, kunt u server-side-triggers of opgeslagen procedures, die rapporten invoeg- en auteurs allemaal binnen een ACID-transactie. Nu u dit niet doet **hebt** alles in één document alleen om ervoor te zorgen dat uw gegevens consistent blijft insluiten.

## <a name="NextSteps"></a>Volgende stappen
De grootste takeaways uit dit artikel zijn om te begrijpen dat gegevens modelleren in een wereld schema's kunnen net zo belangrijk als ooit worden. 

Net zoals er geen enkele manier om weer te geven van een hoeveelheid gegevens op een scherm is, is er geen eenduidige manier om uw gegevens modelleren. U moet voor informatie over uw toepassing en hoe deze wordt produceren, gebruiken en de gegevens te verwerken. Vervolgens kunt u door het toepassen van enkele van de richtlijnen die hier wordt gepresenteerd ook instellen over het maken van een model dat de onmiddellijke behoeften van uw toepassing adressen. Wanneer u uw toepassingen wilt wijzigen, kunt u gebruikmaken van de flexibiliteit van een schema's database over te gaan die wijzigen en uw gegevensmodel eenvoudig kunt veranderen. 

Raadpleeg voor meer informatie over Azure Cosmos DB, van de service [documentatie](https://azure.microsoft.com/documentation/services/cosmos-db/) pagina. 

Om te begrijpen hoe sharden uw gegevens over meerdere partities verwijzen naar [partitioneren van gegevens in Azure Cosmos DB](sql-api-partition-data.md). 
