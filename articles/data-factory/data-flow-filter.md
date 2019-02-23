---
title: Azure Data Factory gegevenstransformatie stroom Filter toewijzen
description: Azure Data Factory gegevenstransformatie stroom Filter toewijzen
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/03/2019
ms.openlocfilehash: 1930ca7406b6ef1b9fd20e1651bc031150dd952e
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2019
ms.locfileid: "56729540"
---
# <a name="azure-data-factory-mapping-data-flow-filter-transformation"></a>Azure Data Factory gegevenstransformatie stroom Filter toewijzen

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

De transformaties Filter biedt rij filteren. Een expressie waarin de filtervoorwaarde maken. Klik in het tekstvak in op het starten van de opbouwfunctie voor expressies. Bouw een filterexpressie om te bepalen welke rijen van de stroom voor de huidige gegevens zijn toegestaan (filter) doorgeven aan de volgende transformatie binnen de opbouwfunctie voor expressies.

Dat wil zeggen loan_status kolom filteren:

```
in([‘Default’, ‘Charged Off’, ‘Fully Paid’], loan_status).
```

Filteren op de kolom year in de demo films:

```
year > 1980
```
