---
title: Transparante gegevensversleuteling inschakelt voor Stretch Database - Azure | Microsoft Docs
description: Transparent Data Encryption (TDE) voor SQL Server Stretch Database in Azure inschakelen
services: sql-server-stretch-database
documentationcenter: ''
author: douglaslMS
manager: craigg
ms.assetid: a44ed8f5-b416-4c41-9b1e-b7271f10bdc3
ms.service: sql-server-stretch-database
ms.workload: data-management
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 06/14/2016
ms.author: douglasl
ms.openlocfilehash: 1e40e3d9eb1231666acda89c752ebc8f517e8fc6
ms.sourcegitcommit: 21466e845ceab74aff3ebfd541e020e0313e43d9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2018
ms.locfileid: "53741535"
---
# <a name="enable-transparent-data-encryption-tde-for-stretch-database-on-azure"></a>Transparent Data Encryption (TDE) inschakelen voor Stretch Database in Azure
> [!div class="op_single_selector"]
> * [Azure Portal](sql-server-stretch-database-encryption-tde.md)
> * [TSQL](sql-server-stretch-database-tde-tsql.md)
>
>

Transparante gegevensversleuteling (TDE) helpt te beschermen tegen de dreiging van schadelijke activiteiten door het uitvoeren van real-time versleuteling en ontsleuteling van de database, gekoppelde back-ups en transactielogboekbestanden in rust zonder wijzigingen aan de toepassing.

De opslag van een volledige database versleutelt TDE met behulp van een symmetrische sleutel met de naam de databaseversleutelingssleutel. De versleutelingssleutel van de database wordt beveiligd door een ingebouwde servercertificaat. Het certificaat van de ingebouwde is uniek voor elke Azure-server. Microsoft draait automatisch deze certificaten ten minste elke 90 dagen. Zie voor een algemene beschrijving van TDE [transparante gegevensversleuteling (TDE)].

## <a name="enabling-encryption"></a>Versleuteling is ingeschakeld
Als u wilt TDE inschakelen voor een Azure-database die de gegevens opslaat gemigreerd van een Stretch-functionaliteit van SQL Server-database, het volgende doen:

1. Openen van de database in de [Azure-portal](https://portal.azure.com)
2. Klik in de databaseblade op de **instellingen** knop
3. Selecteer de **transparante gegevensversleuteling** optie ![][1]
4. Selecteer de **op** instellen en selecteer vervolgens **opslaan**
   ![][2]

## <a name="disabling-encryption"></a>Als u versleuteling uitschakelt
Als u wilt TDE uitschakelen voor een Azure-database die de gegevens opslaat gemigreerd van een Stretch-functionaliteit van SQL Server-database, het volgende doen:

1. Openen van de database in de [Azure-portal](https://portal.azure.com)
2. Klik in de databaseblade op de **instellingen** knop
3. Selecteer de **transparante gegevensversleuteling** optie
4. Selecteer de **uit** instellen en selecteer vervolgens **opslaan**

<!--Anchors-->
[Transparante gegevensversleuteling (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx


<!--Image references-->
[1]: ./media/sql-server-stretch-database-encryption-tde/stretchtde1.png
[2]: ./media/sql-server-stretch-database-encryption-tde/stretchtde2.png


<!--Link references-->
