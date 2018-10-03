---
title: Details van Azure Blockchain Workbench-database opvragen
description: Leer hoe u gegevens van een Azure Blockchain Workbench-database en -server kunt opvragen.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 10/1/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: mmercuri
manager: femila
ms.openlocfilehash: 4020500e20973e8818320424833815c1b5224f6c
ms.sourcegitcommit: 1981c65544e642958917a5ffa2b09d6b7345475d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/03/2018
ms.locfileid: "48242649"
---
# <a name="get-information-about-your-azure-blockchain-workbench-database"></a>Informatie opvragen over uw Azure Blockchain Workbench-database

In dit artikel ziet u hoe u gedetailleerde informatie kunt opvragen over eenAzure Blockchain Workbench-database.

## <a name="overview"></a>Overzicht

Informatie over toepassingen, werkstromen de uitvoering van slimme contracten kunt u opvragen met behulp van weergaven van de SQL-database van Blockchain Workbench. Ontwikkelaars kunnen deze informatie gebruiken in combinatie met hulpprogramma's zoals Microsoft Excel, Power BI, Visual Studio en SQL Server Management Studio.

Ontwikkelaars hebben het volgende nodig om verbinding te kunnen maken met de database:

* Externe clienttoegang is toegestaan in de databasefirewall. In dit artikel over het configureren van een databasefirewall wordt uitgelegd hoe u toegang kunt verlenen.
* De naam van de databaseserver en van de database.

## <a name="connect-to-the-blockchain-workbench-database"></a>Verbinding maken met een database van Blockchain Workbench

Verbinding maken met de database:

1. Meld u aan bij Azure Portal met een account met **Eigenaar**-machtigingen voor de resources van Azure Blockchain Workbench.
2. Kies **Resourcegroepen** in het linkernavigatievenster.
3. Kies de naam van de resourcegroep voor uw implementatie van Blockchain Workbench.
4. Selecteer **Type** om de lijst met resources te sorteren en kies vervolgens uw **SQL-server**. De gesorteerde lijst in de volgende schermafbeelding bevat twee SQL-databases, 'master' en een database die 'lhgn' gebruikt als de waarde voor **Voorvoegsel resource**.

   ![Lijst met gesorteerde resources van Azure Blockchain Workbench](./media/getdb-details/sorted-workbench-resource-list.png)

5. Als u gedetailleerde gegevens wilt weergeven voor de database van Blockchain Workbench, selecteert u de link voor de database met het **resourcevoorvoegsel** dat u hebt opgegeven voor het implementeren van Blockchain Workbench.

   ![Databasedetails](./media/getdb-details/workbench-db-details.png)

Met behulp van de naam van de databaseserver en de database kunt u vanuit uw ontwikkel- of rapportageprogramma verbinding met de database Blockchain Workbench.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Databaseweergaven in Azure Blockchain Workbench](database-views.md)