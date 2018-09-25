---
title: Gegevens verwijderen uit Azure Data Explorer
description: Dit artikel wordt beschreven scenario's voor bulksgewijs verwijderen in Azure gegevens verkennen, met inbegrip van opschonen van Logboeken en retentie op basis van wordt verwijderd.
author: orspod
ms.author: v-orspod
ms.reviewer: mblythe
ms.service: data-explorer
services: data-explorer
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 571a005dd3f50690f291a7ffa3c1174ea15cb0ed
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47047609"
---
# <a name="delete-data-from-azure-data-explorer"></a>Gegevens verwijderen uit Azure Data Explorer

Azure Data Explorer biedt ondersteuning voor verschillende bulksgewijs verwijderen benaderingen, die we in dit artikel behandelen. Het biedt geen ondersteuning voor per record verwijderen in realtime, omdat deze geoptimaliseerd voor snelle toegang voor lezen.

* Als de database niet meer nodig hebt is, verwijderen met behulp van de opdracht drop database.

    ```Kusto
    .drop database <DatabaseName>
    ```

* Als een of meer tabellen niet meer nodig is, verwijdert u deze met behulp van de tabel verwijderen of tabellen opdracht verwijderen.

    ```Kusto
    .drop table <TableName>

    .drop tables (<TableName1>, <TableName2>,...)
    ```

* Als de oude gegevens is niet meer nodig hebt, kunt u deze door het veranderen van de bewaarperiode op het niveau van de database of tabel verwijderen.

    Houd rekening met een database of tabel die is ingesteld op 90 dagen bewaard. Zakelijke moet wijzigen, zodat alleen 60 dagen aan gegevens nodig is. In dit geval verwijderen van de oudere gegevens in een van de volgende manieren.

    ```Kusto
    .alter-merge database <DatabaseName> policy retention softdelete = 60d

    .alter-merge table <TableName> policy retention softdelete = 60d
    ```

    Zie voor meer informatie, [bewaarbeleid](https://docs.microsoft.com/azure/kusto/concepts/retentionpolicy).

* U kunt afzonderlijke records met behulp van verwijderen de *opschonen* bewerking, op basis van een predicaat achtige `where CustomerName == 'contoso'`. Ze eerst een opschonen is een bulk-verwijderen die niet is ontworpen voor realtime verwijderen. Het volgende voorbeeld ziet een leegmaken.

    ```Kusto
    .purge table Customer records
    | where CustomerName =='contoso'
    ```

Als u hulp nodig hebt bij problemen met het verwijderen van gegevens, opent u een ondersteuningsaanvraag in de [Azure-portal](https://portal.azure.com).