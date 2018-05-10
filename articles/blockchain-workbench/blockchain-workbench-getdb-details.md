---
title: Details van de database Azure Blockchain Workbench ophalen
description: Lees hoe u Azure Blockchain Workbench database en informatie over de database.
services: azure-blockchain
keywords: ''
author: PatAltimore
ms.author: patricka
ms.date: 5/4/2018
ms.topic: article
ms.service: azure-blockchain
ms.reviewer: mmercuri
manager: femila
ms.openlocfilehash: bf7cc85e823e6630dbd3278bc91fba85f404059f
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/08/2018
---
# <a name="get-information-about-your-azure-blockchain-workbench-database"></a>Informatie ophalen over uw Azure Blockchain Workbench-database

In dit artikel laat zien hoe voor gedetailleerde informatie over uw Azure Blockchain Workbench-database.

## <a name="overview"></a>Overzicht

Informatie over toepassingen en werkstromen smartcard contract kan worden uitgevoerd wordt verstrekt met behulp van weergaven in de Blockchain Workbench SQL-database. Ontwikkelaars kunnen deze informatie gebruiken bij gebruik van hulpprogramma's, zoals Microsoft Excel, Power BI, Visual Studio en SQL Server Management Studio.

Voordat een ontwikkelaar verbinding met de database maken kan, die ze nodig:

* Externe clienttoegang is toegestaan in de databasefirewall. In dit artikel over het configureren van een database firewall artikel wordt uitgelegd hoe u toegang te verlenen.
* De database-servernaam en databasenaam op.

## <a name="connect-to-the-blockchain-workbench-database"></a>Verbinding maken met de database Blockchain Workbench

Verbinding maken met de database:

1. Meld u aan bij de Azure-Portal met een account met **eigenaar** machtigingen voor de bronnen Azure Blockchain Workbench.
2. Kies in het navigatiedeelvenster links **resourcegroepen**.
3. Kies de naam van de resourcegroep voor uw implementatie-Blockchain Workbench.
4. Selecteer **Type** sorteer de lijst met resources en klik vervolgens op uw **SQL server**. De gesorteerde lijst in de volgende schermafbeelding ziet u twee SQL-databases 'master' en 'lhgn' gebruikt als de **Resource voorvoegsel**.

   ![Lijst met gesorteerde Blockchain Workbench resources](media/blockchain-workbench-getdb-details/sorted-workbench-resource-list.png)

5. Gedetailleerde gegevens over de database Blockchain Workbench wilt bekijken, selecteer de koppeling voor de database met de **Resource voorvoegsel** u hebt opgegeven voor het implementeren van Blockchain Workbench.

   ![Informatie over de database](media/blockchain-workbench-getdb-details/workbench-db-details.png)

De database-servernaam en databasenaam kunt u verbinding met de database Blockchain Workbench met behulp van de ontwikkeling of rapportage.

## <a name="next-steps"></a>Volgende stappen

* [Databaseweergaven in Azure Blockchain Workbench](blockchain-workbench-database-views.md)