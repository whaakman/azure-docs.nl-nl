---
title: Afgeleide kolom transformatie in het toewijzen van gegevensstroom - Azure Data Factory | Microsoft Docs
description: Informatie over het transformeren van gegevens op schaal in Azure Data Factory met de toewijzing van Flow afgeleide kolom-transformatie.
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.topic: conceptual
ms.date: 10/08/2018
ms.openlocfilehash: 941c629fd8359edc7fc1cf364a6735314044d95e
ms.sourcegitcommit: 5cb0b6645bd5dff9c1a4324793df3fdd776225e4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/21/2019
ms.locfileid: "67312210"
---
# <a name="derived-column-transformation-in-mapping-data-flow"></a>Afgeleide kolom transformatie in de gegevensstroom toewijzen

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Gebruik de transformatie afgeleide kolom voor het genereren van nieuwe kolommen in de gegevensstroom of om bestaande velden wijzigen.

## <a name="derived-column-settings"></a>Afgeleide kolom-instellingen

Als u wilt een bestaande kolom overschrijven, selecteert u deze via de kolom-vervolgkeuzelijst. Gebruik anders de kolomveld als een tekstvak en typ de naam van de nieuwe kolom. Als u wilt maken van de afgeleide kolom expressie, klikt u op het vak 'Expressie invoeren' te openen de [gegevens Flow opbouwfunctie voor expressies](concepts-data-flow-expression-builder.md).

![Kolominstellingen afgeleid](media/data-flow/dc1.png "afgeleide kolominstellingen")

Als u wilt toevoegen van extra afgeleide kolommen, Beweeg de muisaanwijzer over een bestaande afgeleide kolom en klik op '+'. Kies vervolgens 'Kolom toevoegen' of 'Patroon voor kolom toevoegen'. Kolom patronen kunnen van pas komen als uw kolomnamen variabele van uw bronnen. Zie voor meer informatie, [kolom patronen](concepts-data-flow-column-pattern.md).

![Nieuw afgeleide kolom selecteren](media/data-flow/columnpattern.png "nieuw afgeleide kolom selecteren")

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over de [expressietaal gegevensstroom toewijzing](data-flow-expression-functions.md).
