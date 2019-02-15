---
title: Azure Data Factory gegevenstransformatie stroom Filter toewijzen
description: Azure Data Factory gegevenstransformatie stroom Filter toewijzen
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 02/03/2019
ms.openlocfilehash: b7e7b123560aae3a2d3086c8536969297d31f7ba
ms.sourcegitcommit: f715dcc29873aeae40110a1803294a122dfb4c6a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/14/2019
ms.locfileid: "56271724"
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
