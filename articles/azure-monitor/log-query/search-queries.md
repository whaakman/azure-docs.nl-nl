---
title: Zoekopdrachten in Logboeken van Azure Monitor | Microsoft Docs
description: In dit artikel bevat een zelfstudie voor aan de slag met zoeken in Logboeken-query's van Azure Monitor.
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 08/06/2018
ms.author: bwren
ms.openlocfilehash: 2df4cf994e118fef9048504daf40fabc1625c375
ms.sourcegitcommit: f715dcc29873aeae40110a1803294a122dfb4c6a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/14/2019
ms.locfileid: "56267680"
---
# <a name="search-queries-in-azure-monitor-logs"></a>Zoekopdrachten in Logboeken van Azure Monitor

> [!NOTE]
> U moet voltooien [aan de slag met Azure Monitor logboeken-query's](get-started-queries.md) voordat het voltooien van deze les gaat uitvoeren.

[!INCLUDE [log-analytics-demo-environment](../../../includes/log-analytics-demo-environment.md)]

Azure Monitor logboeken-query's kunnen beginnen met een tabelnaam wordt opgegeven of een zoekopdracht uitvoeren. In deze zelfstudie bevat informatie over query's op basis van zoeken. Er zijn voordelen voor elke methode.

Query's op basis van een tabel begin met het bereik van de query en daarom meestal efficiënter zijn dan zoekquery's. Zoekquery's zijn dat minder gestructureerd waardoor ze beter bij het zoeken naar een specifieke waarde over kolommen of tabellen. **Search** kunt scannen alle kolommen in een bepaalde tabel of in alle tabellen, voor de opgegeven waarde. De hoeveelheid gegevens die worden verwerkt mogelijk enorme, dit is de reden waarom deze query's het langer duren kunnen om en kunnen zeer grote resultatensets retourneren.

## <a name="search-a-term"></a>Een term zoeken
De **zoeken** opdracht wordt doorgaans gebruikt om te zoeken naar een specifieke term. In het volgende voorbeeld worden alle kolommen in alle tabellen worden gescand op de term "error":

```Kusto
search "error"
| take 100
```

Terwijl ze eenvoudig te gebruiken, wordt buiten het bereik vallen query's zoals bleek hierboven zijn niet efficiënt en waarschijnlijk veel irrelevante resultaten worden geretourneerd. Uit veiligheidsoverwegingen beter zou zijn om te zoeken in de betreffende tabel, of zelfs een specifieke kolom.

### <a name="table-scoping"></a>Tabel scoping
Als u wilt een zoekterm in een specifieke tabel toevoegen `in (table-name)` direct na de **zoeken** operator:

```Kusto
search in (Event) "error"
| take 100
```

of in meerdere tabellen:
```Kusto
search in (Event, SecurityEvent) "error"
| take 100
```

### <a name="table-and-column-scoping"></a>Tabel en kolom scoping
Standaard **zoeken** evalueren alle kolommen in de gegevensset. Als u wilt zoeken naar alleen een specifieke kolom, gebruik de volgende syntaxis:

```Kusto
search in (Event) Source:"error"
| take 100
```

> [!TIP]
> Als u `==` in plaats van `:`, zou bevatten de resultaten records waarin de *bron* kolom bevat de exacte waarde "error" in dit geval exacte. Met behulp van ':' bevat geen records waarin *bron* waarden zoals "404-foutcode" of "Error" bevat.

## <a name="case-sensitivity"></a>Hoofdlettergevoeligheid
Term zoeken is standaard niet-hoofdlettergevoelig, dus resultaten zoals "DNS", "dns" of "Dns" zoeken "dns" kan opleveren. Als u de zoekopdracht hoofdlettergevoelig, gebruikt u de `kind` optie:

```Kusto
search kind=case_sensitive in (Event) "DNS"
| take 100
```

## <a name="use-wild-cards"></a>Jokertekens gebruiken
De **zoeken** opdracht ondersteunt jokertekens, aan het begin, einde of het midden van een term.

Om te zoeken naar termen die met 'win' beginnen:
```Kusto
search in (Event) "win*"
| take 100
```

Om te zoeken naar termen die eindigen op '.com':
```Kusto
search in (Event) "*.com"
| take 100
```

Om te zoeken naar termen die 'www' bevatten:
```Kusto
search in (Event) "*www*"
| take 100
```

Om te zoeken naar termen die begint met "corp" en eindigt op '.com', zoals "corp.mydomain.com" "

```Kusto
search in (Event) "corp*.com"
| take 100
```

U kunt ook opvragen alles in een tabel met alleen een jokerteken: `search in (Event) *`, maar dat zou zijn hetzelfde als het schrijven van alleen `Event`.

> [!TIP]
> U kunt gebruiken `search *` voor elke kolom van elke tabel, wordt aanbevolen dat u altijd het bereik van uw query's op specifieke tabellen. Buiten het bereik vallen query's kunnen even duren om uit te voeren en mogelijk te veel resultaten worden geretourneerd.

## <a name="add-and--or-to-search-queries"></a>Voeg *en* / *of* om te zoeken naar query's
Gebruik **en** om te zoeken naar records die meerdere termen bevatten:

```Kusto
search in (Event) "error" and "register"
| take 100
```

Gebruik **of** om op te halen records met ten minste één van de voorwaarden:

```Kusto
search in (Event) "error" or "register"
| take 100
```

Als u meerdere zoekvoorwaarden hebt, kunt u ze kunt combineren in dezelfde query met behulp van haakjes:

```Kusto
search in (Event) "error" and ("register" or "marshal*")
| take 100
```

De resultaten van dit voorbeeld zou de records die de term "error" bevatten en ook 'registreren' of iets dat met 'marshal begint' zijn.

## <a name="pipe-search-queries"></a>Doorgeven van zoekquery 's
Net als bij een andere opdracht **zoeken** kan worden doorgesluisd, zodat de zoekresultaten kunnen worden gefilterd, gesorteerd en samengevoegd. Bijvoorbeeld, om het aantal *gebeurtenis* records met 'win':

```Kusto
search in (Event) "win"
| count
```




## <a name="next-steps"></a>Volgende stappen

- Zie meer zelfstudies over het [Kusto-query language site](/azure/kusto/query/).