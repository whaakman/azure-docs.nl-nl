---
title: Azure Data Factory-gegevensstroom afgeleide kolom transformatie
description: Azure Data Factory-gegevensstroom afgeleide kolom transformatie
author: kromerm
ms.author: makromer
ms.reviewer: douglasl
ms.service: data-factory
ms.topic: conceptual
ms.date: 10/08/2018
ms.openlocfilehash: 1335b86431bcf345f452ead0c95926d295ae185f
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2019
ms.locfileid: "56728757"
---
# <a name="azure-data-factory-data-flow-derived-column-transformation"></a>Azure Data Factory-gegevensstroom afgeleide kolom transformatie

[!INCLUDE [notes](../../includes/data-factory-data-flow-preview.md)]

Gebruik de transformatie afgeleide kolom voor het genereren van nieuwe kolommen in de gegevensstroom of om bestaande velden wijzigen.

![kolom afleiden](media/data-flow/dc1.png "afgeleide kolom")

U kunt meerdere afgeleide kolom acties uitvoeren in een enkele afgeleide kolom-transformatie. Klik op 'Kolom toevoegen' voor het transformeren van meer dan 1 kolom in de transformatiestap één.

Selecteer een bestaande kolom te overschrijven met een nieuwe waarde van de afgeleide in het veld kolom, of klikt u op 'Nieuwe kolom maken' voor het genereren van een nieuwe kolom met de zojuist afgeleide waarde.

De opbouwfunctie voor expressies waar u de expressie voor de afgeleide kolommen met behulp van functies van de expressie kunt bouwen, het tekstvak voor expressie wordt geopend.
