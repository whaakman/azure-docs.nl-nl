---
title: Tips voor probleemoplossing voor cognitief zoeken - Azure Search
description: Tips en probleemoplossing voor het instellen van cognitieve zoeken in pijplijnen in Azure Search.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: luisca
ms.custom: seodec2018
ms.openlocfilehash: 5f21fe3c65e37d3fee4043526762a7fafdea5cc4
ms.sourcegitcommit: a8948ddcbaaa22bccbb6f187b20720eba7a17edc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2019
ms.locfileid: "56593961"
---
# <a name="troubleshooting-tips-for-cognitive-search"></a>Tips voor probleemoplossing voor cognitief zoeken

In dit artikel bevat een lijst met tips en trucs om te voorkomen dat u verplaatst terwijl u aan de slag met cognitieve zoekmogelijkheden in Azure Search. 

Als u dit nog niet hebt gedaan, doorloopt u de [zelfstudie: Informatie over het aanroepen van de cognitief zoeken-API's](cognitive-search-quickstart-blob.md) voor praktijk bij het toepassen van enrichments cognitief zoeken naar een blob-gegevensbron.

## <a name="tip-1-start-with-a-small-dataset"></a>Tip 1: Beginnen met een kleine gegevensset
De beste manier om problemen snel te vinden is om te verhogen de snelheid waarmee u problemen kunt oplossen. Er is de beste manier om de indexering tijd te verkorten door het aantal documenten worden geïndexeerd te verminderen. 

Beginnen met het maken van een gegevensbron met slechts een handvol documenten/records. Voorbeeld van uw document moet een goede representatie van het gebruik van verschillende van documenten die worden geïndexeerd. 

Uw document-voorbeeld uitvoeren via de end-to-end-pijplijn en controleer de resultaten aan uw behoeften voldoet. Zodra u tevreden met de resultaten bent, kunt u meer bestanden toevoegen aan uw gegevensbron.

## <a name="tip-2-make-sure-your-data-source-credentials-are-correct"></a>Tip 2: Zorg ervoor dat uw referenties juist zijn
De verbinding met de gegevensbron is niet gevalideerd totdat u definieert een indexeerfunctie die wordt gebruikt. Als u fouten vermelden dat de indexeerfunctie geen toegang hebt tot de gegevens ziet, controleert u of:
- De verbindingsreeks is juist. Speciaal wanneer u SAS-tokens maakt, zorg ervoor dat u de indeling die door Azure Search wordt verwacht. Zie [het opgeven van referenties sectie](
https://docs.microsoft.com/azure/search/search-howto-indexing-azure-blob-storage#how-to-specify-credentials) voor meer informatie over de verschillende indelingen ondersteund.
- De containernaam van uw in de indexeerfunctie is juist.

## <a name="tip-3-see-what-works-even-if-there-are-some-failures"></a>Tip 3: Zie wat werkt, zelfs als er fouten opgetreden zijn
Een kleine fout stopt soms een indexeerfunctie in sporen te wissen. Dat is prima als u van plan bent een door een problemen op te lossen. Echter, kunt u een bepaald type fout negeren zodat de indexeerfunctie om door te gaan, zodat u kunt zien wat stromen daadwerkelijk werkt.

In dat geval kunt u zien van de indexeerfunctie worden fouten genegeerd. Dat doen door in te stellen *maxFailedItems* en *maxFailedItemsPerBatch* als -1 als onderdeel van de definitie van de indexeerfunctie.

```
{
  "// rest of your indexer definition
   "parameters":
   {
      "maxFailedItems":-1,
      "maxFailedItemsPerBatch":-1
   }
}
```
## <a name="tip-4-looking-at-enriched-documents-under-the-hood"></a>Tip 4: Kijken naar verrijkt documenten achter de schermen 
Verrijkt documenten zijn tijdelijke structuren die zijn gemaakt tijdens de verrijking en vervolgens verwijderd wanneer de verwerking is voltooid.

Als u een momentopname van het verrijkte document wilt vastleggen tijdens het indexeren, voegt u een veld met de naam ```enriched``` toe aan uw index. De indexeerfunctie dumpt automatisch een tekenreeksrepresentatie van de verrijkingen voor het document in het veld.

Het veld ```enriched``` bevat dan een tekenreeks die een logische representatie vormt van het verrijkte document in het geheugen in JSON.  De veldwaarde is echter een geldig JSON-document. Aanhalingstekens worden geïdentificeerd met een escape-teken, waardoor u `\"` met `"` moet vervangen als u het document als geformatteerde JSON wilt weergeven. 

Het verrijkt veld is bedoeld voor foutopsporing worden gebruikt, zodat u inzicht in de logische vorm van de inhoud die op basis van expressies worden geëvalueerd. U moet niet afhankelijk zijn van dit veld voor het indexeren van toepassing.

Voeg een ```enriched``` veld als onderdeel van de definitie van de index voor foutopsporing:

#### <a name="request-body-syntax"></a>Syntaxis aanvraagbody
```json
{
  "fields": [
    // other fields go here.
    {
      "name": "enriched",
      "type": "Edm.String",
      "searchable": false,
      "sortable": false,
      "filterable": false,
      "facetable": false
    }
  ]
}
```

## <a name="tip-5-expected-content-fails-to-appear"></a>Tip 5: Verwacht inhoud niet kan worden weergegeven

Inhoud ontbreekt mogelijk het gevolg van documenten geplaatst tijdens het indexeren. Gratis en Basic-lagen hebben lage beperkingen op de documentgrootte van het. Elk bestand overschrijdt de limiet is verwijderd tijdens het indexeren. U kunt verwijderde documenten in Azure portal controleren. Dubbelklik in het dashboard van de zoekopdracht, op de tegel indexeerfuncties. Bekijk de verhouding van geslaagde geïndexeerde documenten. Als deze niet 100% is, kunt u de verhouding als u meer informatie. 

Als het probleem is gerelateerd aan de grootte, ziet u mogelijk een fout als volgt: "De blob < bestandsnaam >' heeft de grootte van < bestandsgrootte > bytes, die groter is dan de maximale grootte voor het ophalen van het document voor uw huidige servicelaag." Zie voor meer informatie over limieten van de indexeerfunctie [Servicelimieten](search-limits-quotas-capacity.md).

Een tweede reden voor de inhoud niet kan worden weergegeven mogelijk gerelateerde van i/o-toewijzingsfouten. Bijvoorbeeld de naam van een uitvoer-doel is 'Gebruikers', maar het veldnaam van de index is kleine 'gebruikers'. Het systeem kan 201 bericht voor de hele pijplijn retourneren, zodat u denkt indexering zijn voltooid dat, voordat er in feite een veld leeg is. 

## <a name="tip-6-extend-processing-beyond-maximum-run-time-24-hour-window"></a>Tip 6: Verwerking van meer dan de maximale uitvoeringstijd (24-uurs tijdvenster) uitbreiden

Analyse van de afbeelding is rekenintensief-intensieve voor zelfs voor eenvoudige gevallen, dus wanneer afbeeldingen met name groot of complex zijn, verwerkingstijden kunnen groter zijn dan de maximale tijd die is toegestaan. 

Maximale uitvoeringstijd is afhankelijk van de laag: enkele minuten in de gratis laag, 24-uurs indexering voor factureerbare laag. Als de verwerking niet voltooid binnen een periode van 24 uur voor verwerking op aanvraag, overschakelen naar een schema dat de indexeerfunctie pikken verwerking waar het afgebroken. 

Indexeren wordt hervat volgens schema op de laatste bekende goede document voor geplande indexeerfuncties. Met behulp van een terugkerend schema, kan de indexeerfunctie eraan via de achterstand van de installatiekopie werken gedurende een reeks uren of dagen, waarna alle niet-verwerkte afbeeldingen worden verwerkt. Zie voor meer informatie over de syntaxis van de planning, [stap 3: Een indexeerfunctie maken](search-howto-indexing-azure-blob-storage.md#step-3-create-an-indexer).

Voor het indexeren van portal-gebaseerd (zoals beschreven in de Quick Start), kiezen van de indexeerfunctie 'één keer uitvoeren' optie limieten voor verwerking naar één uur (`"maxRunTime": "PT1H"`). Het is raadzaam om uit te breiden het segmenteringsmodel iets langer.

## <a name="tip-7-increase-indexing-throughput"></a>Tip 7: Doorvoer voor indexering verhogen

Voor [parallelle indexeren](search-howto-large-index.md), plaatst u uw gegevens in meerdere containers of meerdere virtuele mappen in dezelfde container. Vervolgens maakt u meerdere paren van de gegevensbron en indexeerfunctie. Alle indexeerfuncties met de dezelfde vaardigheden en schrijven naar de dezelfde doelzoekindex, zodat uw app zoeken hoeft niet te worden op de hoogte van dit partitioneren.
Zie voor meer informatie, [grote gegevenssets indexeren](search-howto-indexing-azure-blob-storage.md#indexing-large-datasets).

## <a name="see-also"></a>Zie ook
+ [Snelstart: In de portal maakt een pijplijn cognitief zoeken](cognitive-search-quickstart-blob.md)
+ [Zelfstudie: Informatie over cognitive search REST-API 's](cognitive-search-tutorial-blob.md)
+ [Referenties van de gegevensbron op te geven](search-howto-indexing-azure-blob-storage.md#how-to-specify-credentials)
+ [Indexeren van grote gegevenssets](search-howto-indexing-azure-blob-storage.md#indexing-large-datasets)
+ [Hoe u een set vaardigheden definiëren](cognitive-search-defining-skillset.md)
+ [Verrijkt velden toewijzen aan een index](cognitive-search-output-field-mapping.md)