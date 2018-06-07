---
title: Tips voor het cognitieve zoeken in Azure Search | Microsoft Docs
description: Tips en probleemoplossing voor het instellen van cognitieve zoeken pijplijnen in Azure Search.
services: search
manager: pablocas
author: luiscabrer
ms.service: search
ms.devlang: NA
ms.workload: search
ms.topic: conceptual
ms.date: 05/01/2018
ms.author: luisca
ms.openlocfilehash: 3c3f9a0d0dc40de6c62c21dab0f11a501829ef11
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34640962"
---
# <a name="troubleshooting-tips-for-cognitive-search"></a>Tips voor probleemoplossing voor cognitieve zoeken

Dit artikel bevat een lijst met tips en trucs zorgen ervoor dat u verplaatst terwijl u aan de slag met cognitieve zoekmogelijkheden in Azure Search. 

Als u dit nog niet hebt gedaan, doorloopt u de [zelfstudie: informatie over het aanroepen van de cognitieve zoeken-API's](cognitive-search-quickstart-blob.md) voor de praktijk bij het toepassen van enrichments cognitieve zoeken naar een blob-gegevensbron.

## <a name="tip-1-start-with-a-small-dataset"></a>Tip 1: Beginnen met een kleine gegevensset
De beste manier om problemen snel zoeken is om te verhogen van de snelheid waarmee u problemen kunt oplossen. De beste manier om de indexering tijd te beperken is door het verminderen van het aantal documenten te indexeren. 

Begint met het maken van een gegevensbron met slechts een handvol documenten/records. Uw voorbeeld document moet een goede weergave van de verscheidenheid van documenten die worden geïndexeerd. 

Uw document-voorbeeld uitvoeren via de end-to-end-pijplijn en controleer de resultaten aan uw behoeften voldoet. Zodra u tevreden met de resultaten bent, kunt u meer bestanden toevoegen aan uw gegevensbron.

## <a name="tip-2-make-sure-your-data-source-credentials-are-correct"></a>Tip 2: Zorg ervoor dat uw referenties voor gegevensbron juist zijn.
Verbinding met de gegevensbron is niet gevalideerd totdat u een indexeerfunctie die gebruikmaakt van het definieert. Als u fouten vermelding dat de indexeerfunctie geen toegang hebben tot de gegevens ziet, zorg ervoor dat:
- De verbindingsreeks is juist. Speciaal tijdens het maken van SAS-tokens, zorg er dan voor dat de indeling die werd verwacht door de Azure Search. Zie [het opgeven van referenties sectie](
https://docs.microsoft.com/en-us/azure/search/search-howto-indexing-azure-blob-storage#how-to-specify-credentials) voor meer informatie over de verschillende indelingen ondersteund.
- De containernaam van de in de indexeerfunctie is juist.

## <a name="tip-3-see-what-works-even-if-there-are-some-failures"></a>Tip 3: Zie wat werkt er zelfs als er een aantal fouten
Een kleine fout stopt soms een indexeerfunctie in de nummers. Dat is goed als u van plan bent om op te lossen problemen één voor één. Echter, u mogelijk wilt negeren een bepaald type fout, zodat de indexeerfunctie om door te gaan, zodat u kunt zien wat stromen daadwerkelijk werkt.

In dat geval wilt u de indexeerfunctie voor het negeren van fouten zien. Doen door in te stellen *maxFailedItems* en *maxFailedItemsPerBatch* als -1 als onderdeel van de definitie van de indexeerfunctie.

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
Verrijkt documenten zijn tijdelijke structuren tijdens verrijking gemaakt en vervolgens verwijderd bij het verwerken is voltooid.

Als u een momentopname van het verrijkte document wilt vastleggen tijdens het indexeren, voegt u een veld met de naam ```enriched``` toe aan uw index. De indexeerfunctie dumpt automatisch een tekenreeksrepresentatie van de verrijkingen voor het document in het veld.

Het veld ```enriched``` bevat dan een tekenreeks die een logische representatie vormt van het verrijkte document in het geheugen in JSON.  De veldwaarde is echter een geldig JSON-document. Aanhalingstekens worden geïdentificeerd met een escape-teken, waardoor u `\"` met `"` moet vervangen als u het document als geformatteerde JSON wilt weergeven. 

Het veld verrijkt is bedoeld voor foutopsporing, zodat u een overzicht van de logische vorm van de inhoud die op basis van expressies worden geëvalueerd. U moet niet afhankelijk zijn van dit veld voor indexeren doeleinden.

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

## <a name="tip-5-expected-content-fails-to-appear"></a>Tip 5: Verwachte inhoud niet weergegeven

Ontbrekende inhoud is mogelijk het gevolg van documenten steeds verbroken tijdens het indexeren. Gratis en Basic niveaus hebben lage beperkingen op de documentgrootte van het. Elk bestand overschrijdt de limiet is verwijderd tijdens het indexeren. U kunt controleren voor de verwijderde documenten in de Azure-portal. Dubbelklik in het dashboard search-service op de tegel indexeerfuncties. Bekijk de verhouding van geslaagde geïndexeerde documenten. Als dit niet 100% is, kunt u de verhouding tussen het als u meer informatie. 

Als het probleem is gerelateerd aan de grootte, ziet u mogelijk een fout als volgt: 'De blob < bestandsnaam >' heeft de grootte van < bestandsgrootte > bytes die groter is dan de maximale grootte voor het uitpakken van het document voor uw huidige servicelaag. " Zie voor meer informatie over limieten van de indexeerfunctie [Servicelimieten](search-limits-quotas-capacity.md).

Een tweede reden voor inhoud die niet kan worden weergegeven mogelijk gerelateerde toewijzingsfouten input/output. Bijvoorbeeld, de naam van een uitvoer-doel is 'Gebruikers', maar het veldnaam van de index is kleine 'gebruikers'. Het systeem kan 201 succesberichten voor de hele pijplijn retourneren, zodat u denkt dat het indexeren is voltooid, wanneer u in feite een veld is leeg. 

## <a name="tip-6-extend-processing-beyond-maximum-run-time-24-hour-window"></a>Tip 6: Uitbreiden verwerking voorbij de maximale uitvoeringstijd (venster 24 uur)

Analyse van de installatiekopie is rekenkundig intensief zelfs eenvoudige gevallen, dus als afbeeldingen vooral groot of complex is of zijn verwerkingstijden kunnen overschrijden de maximale toegestane tijd. 

Maximale uitvoeringstijd, hangt af van de laag: enkele minuten op de gratis laag, 24-uurs op factureerbare lagen te indexeren. Als de verwerking is mislukt binnen een periode van 24 uur voor verwerking op aanvraag voltooien, overschakelen naar een schema dat de indexeerfunctie worden opgepikt verwerking waar deze was gebleven. 

Voor geplande indexeerfuncties hervat volgens schema op de laatste bekende goede document te indexeren. Met behulp van een terugkerend schema, kan de indexeerfunctie gedurende een reeks uren of dagen, totdat alle niet-verwerkte afbeeldingen worden verwerkt door de achterstand van de installatiekopie werken. Zie voor meer informatie over planning syntaxis [stap 3: Maak een indexeerfunctie](search-howto-indexing-azure-blob-storage.md#step-3-create-an-indexer).

Voor de portal gebaseerde indexering (zoals beschreven in de Quick Start), het kiezen van de indexeerfunctie 'één keer uitvoeren' optie limieten voor verwerking naar 1 uur (`"maxRunTime": "PT1H"`). Mogelijk wilt het venster verwerking op iets langer uitbreiden.

## <a name="tip-7-increase-indexing-throughput"></a>Tip 7: Vergroten doorvoer voor indexering

Voor [parallelle indexeren](search-howto-large-index.md), uw gegevens in meerdere containers of meerdere virtuele mappen in dezelfde container te plaatsen. Vervolgens maakt u meerdere paren van datasource en indexeerfunctie. Alle indexeerfuncties toegang tot de dezelfde vaardigheden en schrijven naar het hetzelfde doel search-index, zodat uw app zoeken niet hoeft te worden op de hoogte van deze partitioneren.
Zie voor meer informatie [grote gegevenssets indexeren](search-howto-indexing-azure-blob-storage.md#indexing-large-datasets).

## <a name="see-also"></a>Zie ook
+ [Snelstartgids: Een pijplijn cognitieve zoeken in de portal maken](cognitive-search-quickstart-blob.md)
+ [Zelfstudie: Meer informatie over cognitieve search REST-API 's](cognitive-search-tutorial-blob.md)
+ [Het opgeven van referenties voor gegevensbron](search-howto-indexing-azure-blob-storage.md#how-to-specify-credentials)
+ [Indexeren van grote gegevenssets](search-howto-indexing-azure-blob-storage.md#indexing-large-datasets)
+ [Het definiëren van een vaardigheden](cognitive-search-defining-skillset.md)
+ [Het verrijkt velden toewijzen aan een index](cognitive-search-output-field-mapping.md)