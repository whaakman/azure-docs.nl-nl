---
title: Azure Data Factory gegevenstransformatie stroom Filter toewijzen
description: Azure Data Factory gegevenstransformatie stroom Filter toewijzen
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/03/2019
ms.openlocfilehash: e0b41850c149ff7095333cf77b780dec1f03b882
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66234414"
---
# <a name="azure-data-factory-filter-transformation"></a>Azure data factory filter transformatie

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
