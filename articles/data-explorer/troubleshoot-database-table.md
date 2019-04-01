---
title: Fout bij maken of verwijderen van een database of tabel in Azure Data Explorer
description: In dit artikel wordt beschreven welke stappen voor het maken en verwijderen van databases en tabellen in Azure Data Explorer.
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
services: data-explorer
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 62eccebab81745f85450390f64b6b22f3c17b32e
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/01/2019
ms.locfileid: "58758323"
---
# <a name="troubleshoot-failure-to-create-or-delete-a-database-or-table-in-azure-data-explorer"></a>Problemen oplossen: Fout bij maken of verwijderen van een database of tabel in Azure Data Explorer

In Azure Data Explorer werkt u regelmatig met databases en tabellen. Dit artikel bevat stappen voor probleemoplossing voor problemen die naar behoren.

## <a name="creating-a-database"></a>Een database maken

1. Zorg ervoor dat u voldoende machtigingen hebt. Voor het maken van een database, moet u lid zijn van de *Inzender* of *eigenaar* rol voor het Azure-abonnement. Indien nodig, werken met de beheerder van het abonnement, zodat ze u aan de juiste rol toevoegen kunnen.

1. Zorg ervoor dat er geen validatiefouten naam voor de databasenaam. De naam moet bestaan uit alfanumerieke tekens, met een maximale lengte van 260 tekens lang zijn.

1. Zorg ervoor dat de database-retentie en opslaan in cache waarden binnen de toegestane bereiken. Bewaarperiode moet liggen tussen 1 en 36500 dagen (100 jaar). Opslaan in cache moet tussen 1 en de maximumwaarde voor de bewaarperiode instellen.

## <a name="deleting-or-renaming-a-database"></a>Verwijderen of hernoemen van een database

Zorg ervoor dat u voldoende machtigingen hebt. Als u wilt verwijderen of wijzig de naam van een database, moet u lid zijn van de *Inzender* of *eigenaar* rol voor het Azure-abonnement. Indien nodig, werken met de beheerder van het abonnement, zodat ze u aan de juiste rol toevoegen kunnen.

## <a name="creating-a-table"></a>Het maken van een tabel

1. Zorg ervoor dat u voldoende machtigingen hebt. Voor het maken van een tabel, moet u lid zijn van de *database admin* of *databasegebruiker* rol in de database of de *Inzender* of *eigenaar* rol voor het Azure-abonnement. Indien nodig, werken met uw abonnement of Clusterbeheer, zodat ze u aan de juiste rol toevoegen kunnen.

    Zie voor meer informatie over machtigingen [databasemachtigingen beheren](manage-database-permissions.md).

1. Zorg ervoor dat een tabel met dezelfde naam al bestaat niet. Als deze bestaat, kunt u: Maak een tabel met een andere naam; Wijzig de naam van de bestaande tabel (vereist *tabel admin* rol); of verwijdert u de bestaande tabel (vereist *database admin* rol). Gebruik de volgende opdrachten.

    ```Kusto
    .drop table <TableName>

   .rename table <OldTableName> to <NewTableName>
    ```

## <a name="deleting-or-renaming-a-table"></a>Verwijderen of hernoemen van een tabel

Zorg ervoor dat u voldoende machtigingen hebt. Als u wilt verwijderen of wijzig de naam van een tabel, moet u lid zijn van de *database admin* of *tabel admin* rol in de database. Indien nodig, werken met uw abonnement of Clusterbeheer, zodat ze u aan de juiste rol toevoegen kunnen.

Zie voor meer informatie over machtigingen [databasemachtigingen beheren](manage-database-permissions.md).

## <a name="general-guidance"></a>Algemene richtlijnen

1. Controleer de [Azure service health-dashboard](https://azure.microsoft.com/status/). Zoek naar de status van Azure Data Explorer in de regio waar u wilt werken met een database of tabel.

    Als de status niet **goede** (groen vinkje), probeer het opnieuw nadat de status wordt verbeterd.

1. Als u nog steeds hulp bij het oplossen van uw probleem, opent u een ondersteuningsaanvraag in de [Azure-portal](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview).

## <a name="next-steps"></a>Volgende stappen

[Clusterstatus controleren](check-cluster-health.md)