---
title: Transparante gegevensversleuteling inschakelt voor Stretch Database - Azure | Microsoft Docs
description: Transparante gegevensversleuteling (TDE) voor SQL Server Stretch Database in Azure inschakelen
services: sql-server-stretch-database
documentationcenter: 
author: douglaslMS
manager: barbkess
editor: 
ms.assetid: a44ed8f5-b416-4c41-9b1e-b7271f10bdc3
ms.service: sql-server-stretch-database
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2016
ms.author: douglasl
ms.openlocfilehash: ceb355d2ba872ed5d3886c6dc82ca75b1854db0a
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="enable-transparent-data-encryption-tde-for-stretch-database-on-azure"></a>Transparante gegevensversleuteling (TDE) inschakelen voor Stretch Database in Azure
> [!div class="op_single_selector"]
> * [Azure Portal](sql-server-stretch-database-encryption-tde.md)
> * [TSQL](sql-server-stretch-database-tde-tsql.md)
>
>

Transparent Data Encryption (TDE) beschermt tegen de dreiging van schadelijke activiteiten door realtime versleuteling en ontsleuteling van de database, gekoppelde back-ups en transactielogboekbestanden in rust in te voeren zonder dat wijzigingen in de toepassing.

TDE versleutelt de opslag van een volledige database met behulp van een symmetrische sleutel, naam van de databaseversleutelingssleutel. De databaseversleutelingssleutel is beveiligd met een ingebouwde servercertificaat. Het ingebouwde servercertificaat is uniek voor elke Azure-server. Microsoft draait automatisch deze certificaten ten minste om de 90 dagen. Zie voor een algemene beschrijving van TDE [Transparent Data Encryption (TDE)].

## <a name="enabling-encryption"></a>Codering inschakelen
Zodat TDE voor een Azure-database die de gegevens opslaat die zijn gemigreerd vanuit een Stretch geschikte SQL Server-database, het volgende doen:

1. Openen van de database in de [Azure-portal](https://portal.azure.com)
2. Klik in de databaseblade op de **instellingen** knop
3. Selecteer de **transparante gegevensversleuteling** optie![][1]
4. Selecteer de **op** instelling en selecteer vervolgens **opslaan**
   ![][2]

## <a name="disabling-encryption"></a>Codering uitschakelen
Schakel TDE voor een Azure-database die de gegevens worden opgeslagen die zijn gemigreerd vanuit Stretch geschikte SQL Server-database, het volgende doen:

1. Openen van de database in de [Azure-portal](https://portal.azure.com)
2. Klik in de databaseblade op de **instellingen** knop
3. Selecteer de **transparante gegevensversleuteling** optie
4. Selecteer de **uit** instelling en selecteer vervolgens **opslaan**

<!--Anchors-->
[Transparent Data Encryption (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx


<!--Image references-->
[1]: ./media/sql-server-stretch-database-encryption-tde/stretchtde1.png
[2]: ./media/sql-server-stretch-database-encryption-tde/stretchtde2.png


<!--Link references-->
