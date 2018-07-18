---
title: De verbindingsreeks ophalen met de Azure-portal
description: De verbindingsreeks ophalen met de Azure-portal
keywords: SQL-verbinding, verbindingsreeks
services: sql-database
author: dalechen
manager: craigg
ms.service: sql-database
ms.custom: develop apps
ms.topic: include
ms.date: 07/13/2018
ms.author: ninarn
ms.openlocfilehash: dab7623c86bea4e562313e618f238b9b33c0fdc5
ms.sourcegitcommit: 7827d434ae8e904af9b573fb7c4f4799137f9d9b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/18/2018
ms.locfileid: "39117287"
---
### <a name="obtain-the-connection-string-from-the-azure-portal"></a>De verbindingsreeks ophalen met de Azure-portal
Gebruik de [Azure-portal](https://portal.azure.com/) om op te halen van de verbindingsreeks die nodig is voor uw clientprogramma om te communiceren met Azure SQL Database.

1. Selecteer **alle services** > **SQL-databases**.

2. Voer de naam van uw database in het filtertekstvak in de buurt van de linkerbovenhoek van de **SQL-databases** blade.

3. Selecteer de rij voor uw database.

4. Nadat de blade wordt weergegeven voor uw database, selecteer visual gemak de **minimaliseren** knoppen om samen te vouwen de blades die u hebt gebruikt voor het bladeren door en filteren van de database.

5. Selecteer op de blade voor uw database **databaseverbindingsreeksen tonen**.

6. Kopieer de juiste verbindingsreeks. dat wil zeggen als u van plan bent de ADO.NET-verbinding-clientbibliotheek gebruiken, kopieert u de juiste tekenreeks van de **ADO.NET** tabblad.

    ![Kopieer de ADO-verbindingsreeks voor uw database][20-CopyAdoConnectionString]

7. Indien nodig, kunt u de verbindingsreeks bewerken. dat wil zeggen uw wachtwoord in de connection string invoegen of verwijderen "@&lt;servername&gt;' van de gebruikersnaam als naam van de gebruikersnaam of de server te lang zijn.

8. In de ene indeling of een andere, de verbindingsinformatie in uw programma clientcode te plakken.

Zie voor meer informatie, [tekenreeksen voor databaseverbindingen en -configuratiebestanden](http://msdn.microsoft.com/library/ms254494.aspx).

<!-- Image references. -->



[20-CopyAdoConnectionString]: ./media/sql-database-include-connection-string-20-portalshots/connqry-connstr-b.png


<!--
These three includes/ files are a sequenced set, but you can pick and choose:

includes/sql-database-include-connection-string-20-portalshots.md
includes/sql-database-include-connection-string-30-compare.md
includes/sql-database-include-connection-string-40-config.md
-->
