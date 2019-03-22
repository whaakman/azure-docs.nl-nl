---
title: Azure Data Factory gegevenstransformatie stroom Filter toewijzen
description: Azure Data Factory gegevenstransformatie stroom Filter toewijzen
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/03/2019
ms.openlocfilehash: d1751c47ad4507260d9f8d6ea44fcb32ed0e7338
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57864725"
---
# <a name="azure-data-factoryfilter-transformation"></a>Azure Data FactoryFilter Transformation

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

De transformaties Filter biedt rij filteren. Een expressie waarin de filtervoorwaarde maken. Klik in het tekstvak in op het starten van de opbouwfunctie voor expressies. Bouw een filterexpressie om te bepalen welke rijen van de stroom voor de huidige gegevens zijn toegestaan (filter) doorgeven aan de volgende transformatie binnen de opbouwfunctie voor expressies. Denk aan de transformatie Filter als de component WHERE van een SQL-instructie.

## <a name="filter-on-loanstatus-column"></a>Filteren op loan_status kolom:

```
in([‘Default’, ‘Charged Off’, ‘Fully Paid’], loan_status).
```

Filteren op de kolom year in de demo films:

```
year > 1980
```

## <a name="next-steps"></a>Volgende stappen

Een kolom filteren, transformeren, probeert de [transformatie selecteren](data-flow-select.md)
