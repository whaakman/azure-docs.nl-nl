---
title: Werken met projecties in een knowledge store (preview) - Azure Search
description: Opslaan en vorm te geven uw verrijkt gegevens uit de indexering AI-pijplijn voor gebruik in scenario's dan zoeken
manager: eladz
author: vkurpad
services: search
ms.service: search
ms.devlang: NA
ms.topic: conceptual
ms.date: 05/02/2019
ms.author: vikurpad
ms.custom: seomay2019
ms.openlocfilehash: f1c7278909557dc92f86c5dfc1f190fddf33f607
ms.sourcegitcommit: f6c85922b9e70bb83879e52c2aec6307c99a0cac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/11/2019
ms.locfileid: "65540812"
---
# <a name="working-with-projections-in-a-knowledge-store-in-azure-search"></a>Werken met projecties in een winkel kennis in Azure Search

> [!Note]
> Kennis store is in preview en niet bedoeld voor gebruik in productieomgevingen. De [2019 in de REST-API-versie-05-06-Preview](search-api-preview.md) biedt deze functie. Er is geen .NET SDK-ondersteuning op dit moment.
>

Azure Search kunt inhoud verrijking door middel van cognitieve vaardigheden AI en aangepaste vaardigheden als onderdeel van het indexeren. Enrichments structuur toevoegen aan uw documenten en effectiever maken door te zoeken. In veel gevallen zijn de verrijkt documenten handig voor scenario's dan zoeken, bijvoorbeeld voor knowledge gegevensanalyse.

Projecties, een onderdeel van [kennis store](knowledge-store-concept-intro.md), weergaven van verrijkt documenten die kunnen worden opgeslagen op de fysieke opslag voor knowledge analysestructuur doeleinden. Een projectie kunt u 'project' uw gegevens in een vorm die bij uw behoeften, relaties te behouden, zodat de hulpprogramma's zoals Power BI de gegevens met geen extra moeite kunnen lezen. 

Projecties kunnen in tabelvorm, met gegevens die zijn opgeslagen in rijen en kolommen in de Azure-tabelopslag of JSON-objecten die zijn opgeslagen in Azure Blob-opslag zijn. U kunt meerdere projecties van uw gegevens definiëren als deze wordt gehandhaafd. Dit is handig als u wilt dat dezelfde gegevens vormgegeven anders voor afzonderlijke gebruiksvoorbeelden. 

De knowledge-store ondersteunt twee typen projecties:

+ **Tabellen**: Voor gegevens die het beste wordt weergegeven als rijen en kolommen, kunnen tabel schattingen u een Geschematiseerde vorm of een projectie definiëren in de tabelopslag. 

+ **Objecten**: Wanneer u een JSON-weergave van uw gegevens en enrichments nodig hebt, wordt object projecties worden opgeslagen als blobs.

Als u wilt zien projecties gedefinieerd in de context, doorlopen [aan de slag met kennis store](knowledge-store-howto.md)

## <a name="projection-groups"></a>Projectie-groepen

In sommige gevallen moet u uw verrijkt projectgegevens in verschillende vormen om te voldoen aan verschillende doelstellingen. De knowledge-store kunt u meerdere groepen van projecties definiëren. Projectie groepen hebben de volgende belangrijke kenmerken van wederzijdse exclusiviteit en relatedness.

### <a name="mutually-exclusivity"></a>Sluiten elkaar wederzijds exclusiviteit

Alle inhoud geprojecteerd in één groep is onafhankelijk van gegevens in andere groepen projectie geprojecteerd. Dit betekent dat u dezelfde gegevens vormgegeven anders, maar worden herhaald in elke groep projectie kan hebben. 

Een beperking afgedwongen in groepen van de projectie is de wederzijdse exclusiviteit typen projectie met een projectie-groep. U kunt alleen definiëren projecties van de tabel of object projecties binnen één groep. Als u wilt dat beide tabellen en objecten, definieert u een projectie-groep voor tabellen en een tweede projectie-groep voor objecten.

### <a name="relatedness"></a>Relatedness

Alle inhoud binnen een groep één projectie geprojecteerd bewaart relaties binnen de gegevens. Relaties zijn gebaseerd op een gegenereerde sleutel en elk onderliggend knooppunt behoudt een verwijzing naar het bovenliggende knooppunt. Relaties zijn niet verdeeld projectie groepen en tabellen of objecten die zijn gemaakt in een projectie-groep hebben geen relatie met gegevens die in andere groepen projectie gegenereerd.

## <a name="input-shaping"></a>Voer vormgeven
Ophalen van uw gegevens in de juiste vorm of de structuur op effectieve gebruik is, worden deze tabellen of objecten. De mogelijkheid om te vorm te geven of de structuur van uw gegevens op basis van hoe u van plan bent om te openen en gebruiken is een belangrijke functie die beschikbaar zijn gemaakt als de **Shaper** vaardigheid binnen de vaardigheden.  

Projecties zijn gemakkelijker te definiëren wanneer u een object in de structuur van verrijking die overeenkomt met het schema van de projectie. De bijgewerkte [Shaper vaardigheid](cognitive-search-skill-shaper.md) kunt u een object uit verschillende knooppunten in de boomstructuur verrijking opstellen en ze onder een nieuw knooppunt bovenliggende. De **Shaper** vaardigheden kunt u complexe typen met geneste objecten te definiëren.

Wanneer u een nieuwe shape gedefinieerd die alle elementen die u bevat aan het project uit nodig hebt, kunt u deze vorm nu gebruiken als bron voor uw projecties of als invoer voor een andere kwalificatie.

## <a name="table-projections"></a>Tabel projecties

Omdat deze eenvoudiger te importeren, wordt aangeraden tabel schattingen voor gegevens verkennen met Power BI. Bovendien kunnen tabel schattingen voor wijzigen van het wijzigen van de kardinaliteit tussen tabelrelatie. 

U kunt één document in de index in meerdere tabellen projecteren de relaties te behouden. Bij de projectie van meerdere tabellen, wordt de volledige vorm worden geprojecteerd in elke tabel, tenzij een onderliggend knooppunt de bron van een andere tabel binnen dezelfde groep is.

### <a name="defining-a-table-projection"></a>De projectie van een tabel definiëren

Bij het definiëren van de projectie van een tabel in de `knowledgeStore` element van uw vaardigheden uit, start u via het toewijzen van een knooppunt in de structuur in verrijking naar de tabelbron. Dit knooppunt is meestal de uitvoer van een **Shaper** vaardigheden die u hebt toegevoegd aan de lijst met vaardigheden voor het produceren van een bepaalde vorm die u nodig hebt om te projecteren in tabellen. Het knooppunt dat u aan het project kan worden gesegmenteerd aan project in meerdere tabellen. De definitie van de tabellen is een lijst met tabellen die u wilt projecteren. 

Elke tabel vereist drie eigenschappen:

+ Tabelnaam: De naam van de tabel in Azure Storage.

+ generatedKeyName: De naam van de kolom voor de sleutel die uniek voor deze rij.

+ Bron: Het knooppunt uit de structuur verrijking u sourcing uw enrichments uit. Dit is meestal de uitvoer van een shaper, maar de uitvoer van een van de vaardigheden die kan worden.

Hier volgt een voorbeeld van de tabel projecties.

```json
{
    "name": "your-skillset",
    "skills": [
      …your skills
      
    ],
"cognitiveServices": {
… your cognitive services key info
    },

    "knowledgeStore": {
      "storageConnectionString": "an Azure storage connection string",
      "projections" : [
        {
          "tables": [
            { "tableName": "MainTable", "generatedKeyName": "SomeId", "source": "/document/EnrichedShape" },
            { "tableName": "KeyPhrases", "generatedKeyName": "KeyPhraseId", "source": "/document/EnrichedShape/*/KeyPhrases/*" },
            { "tableName": "Entities", "generatedKeyName": "EntityId", "source": "/document/EnrichedShape/*/Entities/*" }
          ]
        },
        {
          "objects": [
            
          ]
        }
      ]
    }
}
```
Zoals in dit voorbeeld wordt gedemonstreerd, sleuteltermen en entiteiten zijn gemodelleerd in verschillende tabellen en een verwijzing naar de bovenliggende (%{maintable/) voor elke rij bevat. 

De volgende afbeelding is een verwijzing naar de oefening Caselaw in [aan de slag met kennis store](knowledge-store-howto.md). In een scenario waarbij een aanvraag heeft meerdere adviezen en elke advies is uitgebreid door het identificeren van entiteiten die hierin zijn opgenomen, kunt u de projecties model zoals hier wordt weergegeven.

![Entiteiten en relaties in tabellen](media/knowledge-store-projection-overview/TableRelationships.png "relaties in de tabel projecties modelleren")

## <a name="object-projections"></a>Object projecties

Object projecties zijn JSON-weergaven van de structuur van de verrijking die afkomstig is van een willekeurig knooppunt. In veel gevallen hetzelfde **Shaper** dat wordt gemaakt van de projectie van een tabel kan worden gebruikt voor het genereren van de projectie van een object. 

```json
{
 
    "name": "your-skillset",
    "skills": [
      …your skills
      
    ],
"cognitiveServices": {
… your cognitive services key info
    },

    "knowledgeStore": {
      "storageConnectionString": "an Azure storage connection string",
      "projections" : [
        {
          "tables": [ ]
        },
        {
          "objects": [
            {
              "storageContainer": "Reviews", 
              "format": "json", 
              "source": "/document/Review", 
              "key": "/document/Review/Id" 
            }
          ]
        }
      ]
    }
}
```

Genereren van de projectie van een object zijn een paar object-specifieke kenmerken vereist:

+ storageContainer: De container waar de objecten moeten worden opgeslagen
+ Bron: Het pad naar het knooppunt van de structuur van de verrijking is de hoofdmap van de projectie
+ Sleutel: Een pad dat staat voor een unieke sleutel voor het object moet worden opgeslagen. Het zal worden gebruikt voor het maken van de naam van de blob in de container.

## <a name="projection-lifecycle"></a>Projectie levenscyclus

Uw projecties hebben een levenscyclus dat is gekoppeld aan de brongegevens in uw gegevensbron. Als uw gegevens is bijgewerkt en opnieuw geïndexeerd, worden uw projecties bijgewerkt met de resultaten van de enrichments ervoor te zorgen dat uw projecties uiteindelijk consistent zijn met de gegevens in uw gegevensbron. De projecties nemen de delete-beleid dat u hebt geconfigureerd voor de index. 

## <a name="using-projections"></a>Met behulp van projecties

Nadat de indexeerfunctie wordt uitgevoerd, kunt u de verwachte gegevens in de containers of tabellen die u hebt opgegeven via projecties kunt lezen. 

Verkennen in Power BI is voor analyse, net zo eenvoudig als het instellen van Azure Table storage als de gegevensbron. U kunt heel eenvoudig een set visualisaties maken op uw gebruik te maken van de relaties binnen gegevens.

U kunt ook als u gebruikmaken van de geavanceerde gegevens in een pijplijn van data science wilt, kunt u [laden van de gegevens van de blobs in een Pandas DataFrame](../machine-learning/team-data-science-process/explore-data-blob.md).

Ten slotte, als u nodig hebt voor het exporteren van uw gegevens uit de store kennis, heeft Azure Data Factory connectors voor de gegevens exporteren en het land in de database van uw keuze. 

## <a name="next-steps"></a>Volgende stappen

Maak uw eerste knowledge-archief met behulp van voorbeeldgegevens en instructies als volgende stap.

> [!div class="nextstepaction"]
> [Over het maken van een winkel kennis](knowledge-store-howto.md).