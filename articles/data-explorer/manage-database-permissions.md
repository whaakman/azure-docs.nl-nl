---
title: Azure Data Explorer databasemachtigingen beheren
description: Dit artikel wordt beschreven op rollen gebaseerd toegangsbeheer voor databases en tabellen in Azure Data Explorer.
author: orspod
ms.author: v-orspod
ms.reviewer: mblythe
ms.service: data-explorer
services: data-explorer
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: fbbc5a199116e46aac0874f3dc6d6d9aa18c60cd
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46954014"
---
# <a name="manage-azure-data-explorer-database-permissions"></a>Azure Data Explorer databasemachtigingen beheren

Azure Data Explorer kunt u toegang tot databases en tabellen, beheren met behulp van een *op rollen gebaseerd toegangsbeheer* model. Bij dit model *principals* (gebruikers, groepen en apps) zijn toegewezen aan *rollen*. Beveiligings-principals hebben toegang tot resources op basis van de rollen die ze zijn toegewezen.

Dit artikel beschrijft de beschikbare rollen en beveiligings-principals toewijzen aan deze rollen.

## <a name="roles-and-permissions"></a>Rollen en machtigingen

Azure Data Explorer heeft de volgende rollen:

|Rol                       |Machtigingen                                                                        |
|---------------------------|-----------------------------------------------------------------------------------|
|Database-beheerder             |Alles binnen het bereik van een bepaalde database kunt doen.|
|Databasegebruiker              |Kan lezen alle gegevens en metagegevens in de database. Ze kunnen bovendien tabellen (om de beheerder van de tabel voor deze tabel) en -functies in de database maken.|
|Database-viewer            |Kan lezen alle gegevens en metagegevens in de database.|
|Database ingestor          |Kan opnemen van gegevens op alle bestaande tabellen in de database, maar niet de gegevens op te vragen.|
|Database unrestrictedviewer|Query kunt uitvoeren op tabellen waarvoor de **RestrictedViewAccess** beleid is ingeschakeld. Kan geen query uitvoeren op andere tabellen.|
|Database-monitor           |Kan '.show...' opdrachten uitvoeren in de context van de database en de onderliggende entiteiten.|
|Tabel-beheerder                |Alles binnen het bereik van een bepaalde tabel kunnen doen. |
|Tabel ingestor             |Kan opnemen van gegevens in het bereik van een bepaalde tabel, maar niet de gegevens op te vragen.|

## <a name="manage-permissions-in-the-azure-portal"></a>Machtigingen in de Azure-portal beheren

1. Meld u aan bij [Azure Portal](https://portal.azure.com/).

1. Navigeer naar uw Azure Data Explorer-cluster.

1. In de **overzicht** sectie, selecteer de database waar u om machtigingen te beheren.

    ![database selecteren](media/manage-database-permissions/select-database.png)

1. Selecteer **machtigingen** vervolgens **toevoegen**.

    ![Machtigingen voor database](media/manage-database-permissions/database-permissions.png)

1. Onder **toevoegen databasemachtigingen**, selecteer de rol die u wilt de principal toewijzen aan, klikt u vervolgens **principals Selecteer**.

    ![Databasemachtigingen toevoegen](media/manage-database-permissions/add-permission.png)

1. De principal opzoeken, selecteren, klikt u vervolgens **Selecteer**.

    ![Machtigingen in de Azure-portal beheren](media/manage-database-permissions/new-principals.png)

1. Selecteer **Opslaan**.

    ![Machtigingen in de Azure-portal beheren](media/manage-database-permissions/save-permission.png)

## <a name="manage-permissions-with-management-commands"></a>Met de opdrachten voor het beheer-machtigingen beheren

1. Aanmelden bij [ https://dataexplorer.azure.com ](https://dataexplorer.azure.com), en het cluster toevoegen als deze nog niet beschikbaar.

1. Selecteer de juiste database in het linkerdeelvenster.

1. Gebruik de `.add` opdracht principals toewijzen aan rollen: `.add database databasename rolename ('aaduser | aadgroup=user@domain.com')`. Als u wilt een gebruiker toevoegen aan de gebruikersrol van de Database, de volgende opdracht, waarbij de databasenaam en de gebruiker worden uitgevoerd.

    ```Kusto
    .add database <TestDatabase> users ('aaduser=<user@contoso.com>')
    ```

    De uitvoer van de opdracht bevat de lijst met bestaande gebruikers en de rollen waaraan ze zijn toegewezen in de database.

## <a name="next-steps"></a>Volgende stappen

[Query's schrijven](write-queries.md)