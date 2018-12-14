---
title: Gebeurtenissen filteren voor Azure Event Grid
description: Beschrijft hoe u voor het filteren van gebeurtenissen bij het maken van een Azure Event Grid-abonnement.
services: event-grid
author: tfitzmac
ms.service: event-grid
ms.topic: conceptual
ms.date: 11/05/2018
ms.author: tomfitz
ms.openlocfilehash: 973b5d3f40968790dc6153ae91d1638cb83202ee
ms.sourcegitcommit: 85d94b423518ee7ec7f071f4f256f84c64039a9d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2018
ms.locfileid: "53384374"
---
# <a name="understand-event-filtering-for-event-grid-subscriptions"></a>Informatie over gebeurtenis voor Event Grid-abonnementen filteren

Dit artikel beschrijft de verschillende manieren om te filteren welke gebeurtenissen worden verzonden naar het eindpunt. Bij het maken van een gebeurtenisabonnement, hebt u drie opties voor het filteren:

* Gebeurtenistypen
* Onderwerp met begint of eindigt met
* Geavanceerde velden en operators

## <a name="event-type-filtering"></a>Type gebeurtenisfilters

Standaard alle [gebeurtenistypen](event-schema.md) voor bron van de gebeurtenis naar het eindpunt zijn verzonden. U kunt alleen bepaalde typen gebeurtenissen verzenden naar het eindpunt. Bijvoorbeeld, kunt u krijgen een melding van updates voor uw resources, maar niet gewaarschuwd voor andere bewerkingen zoals het verwijderingen. In dat geval kunt u filteren op de `Microsoft.Resources.ResourceWriteSuccess` gebeurtenistype. Geef een matrix met de typen gebeurtenissen of geef `All` alle types van gebeurtenissen ophalen voor de gebeurtenisbron.

De JSON-syntaxis voor het filteren op gebeurtenistype is:

```json
"filter": {
  "includedEventTypes": [
    "Microsoft.Resources.ResourceWriteFailure",
    "Microsoft.Resources.ResourceWriteSuccess"
  ]
}
```

## <a name="subject-filtering"></a>Onderwerp filteren

Eenvoudig filteren op onderwerp, Geef een begin- of -waarde voor het onderwerp. Bijvoorbeeld, kunt u het onderwerp eindigt met `.txt` om alleen gebeurtenissen met betrekking tot het uploaden van een tekstbestand met storage-account. Of u kunt filteren, het onderwerp begint met `/blobServices/default/containers/testcontainer` om op te halen van alle gebeurtenissen voor die container, maar geen andere containers in het opslagaccount.

Bij het publiceren van gebeurtenissen naar aangepaste onderwerpen, maken van onderwerpen voor de gebeurtenissen die het gemakkelijk voor abonnees om te weten of ze geïnteresseerd zijn in de gebeurtenis. Abonnees gebruiken de eigenschap subject gebeurtenissen filteren en route. U kunt het pad voor waar de gebeurtenis heeft plaatsgevonden, toevoegen, zodat abonnees op segmenten van het opgegeven pad filteren kunnen. Het pad kan abonnees nauwkeurig of ruim om gebeurtenissen te filteren. Als u een pad op drie segment zoals `/A/B/C` in het onderwerp, abonnees kunnen filteren op het eerste segment `/A` om op te halen van een breed scala aan gebeurtenissen. Abonnees die aan de gebeurtenissen met onderwerpen zoals `/A/B/C` of `/A/D/E`. Andere abonnees kunnen filteren op `/A/B` om op te halen van een smaller reeks gebeurtenissen.

De JSON-syntaxis voor het filteren op gebeurtenistype is:

```json
"filter": {
  "subjectBeginsWith": "/blobServices/default/containers/mycontainer/log",
  "subjectEndsWith": ".jpg"
}

```

## <a name="advanced-filtering"></a>Geavanceerd filteren

Als u wilt filteren op de waarden in de gegevensvelden en de vergelijkingsoperator opgeeft, gebruikt u de optie voor Geavanceerd filteren. In Geavanceerd filteren, geeft u de:

* operatortype - het type van de vergelijking.
* sleutel - het veld in de gegevens van de gebeurtenis die u voor het filteren gebruikt. Het kan een getal, een Booleaanse waarde of een tekenreeks zijn.
* de waarde of waarden zijn: de waarde of waarden te vergelijken met de sleutel.

De JSON-syntaxis voor het gebruik van geavanceerde filters is:

```json
"filter": {
  "advancedFilters": [
    {
      "operatorType": "NumberGreaterThanOrEquals",
      "key": "Data.Key1",
      "value": 5
    },
    {
      "operatorType": "StringContains",
      "key": "Subject",
      "values": ["container1", "container2"]
    }
  ]
}
```

### <a name="operator"></a>Operator

De beschikbare operators voor getallen zijn:

* NumberGreaterThan
* NumberGreaterThanOrEquals
* NumberLessThan
* NumberLessThanOrEquals
* NumberIn
* NumberNotIn

De beschikbare operator voor Booleaanse waarden is: BoolEquals

De beschikbare operators voor tekenreeksen zijn:

* StringContains
* StringBeginsWith
* StringEndsWith
* StringIn
* StringNotIn

Alle tekenreeksvergelijkingen zijn geval insensitve.

### <a name="key"></a>Sleutel

Gebruik de volgende waarden voor de sleutel voor gebeurtenissen in het schema voor Event Grid:

* Id
* Onderwerp
* Onderwerp
* Type gebeurtenis
* dataVersion
* De gegevens van de gebeurtenis (zoals Data.key1)

Gebeurtenissen in de Cloud het gebeurtenissenschema, de volgende waarden voor de sleutel te gebruiken:

* Gebeurtenis-id
* Bron
* Type gebeurtenis
* eventTypeVersion
* De gegevens van de gebeurtenis (zoals Data.key1)

Gebruik de gegevensvelden gebeurtenis (zoals Data.key1) voor aangepaste invoerschema.

### <a name="values"></a>Waarden

De waarden zijn:

* getal
* string
* booleaans
* array

### <a name="limitations"></a>Beperkingen

Geavanceerd filteren, heeft de volgende beperkingen:

* Vijf geavanceerde filters per event grid-abonnement
* 512 tekens per string-waarde
* Vijf waarden voor **in** en **niet in** operators
* De sleutel kan slechts één niveau van geneste (zoals data.key1) hebben.

Dezelfde sleutel kan worden gebruikt in meer dan één filter.

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over het filteren van gebeurtenissen met PowerShell en Azure CLI, [gebeurtenissen filteren op Event Grid](how-to-filter-events.md).
* Als u wilt snel aan de slag met Event Grid, Zie [aangepaste gebeurtenissen maken en routeren met Azure Event Grid](custom-event-quickstart.md).
