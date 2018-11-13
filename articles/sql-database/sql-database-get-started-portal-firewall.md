---
title: 'Azure Portal: een firewallregel voor een SQL-database maken | Microsoft Docs'
description: Een firewallregel maken op SQL Database-serverniveau
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: quickstart
author: sachinpMSFT
ms.author: sachinp
ms.reviewer: vanto, carlrab
manager: craigg
ms.date: 11/01/2018
ms.openlocfilehash: 3b9968fa0349a7c68a598681a1d6d5aad230055b
ms.sourcegitcommit: 799a4da85cf0fec54403688e88a934e6ad149001
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/02/2018
ms.locfileid: "50913084"
---
# <a name="quickstart-create-a-server-level-firewall-rule-for-your-sql-database-using-the-azure-portal"></a>Snelstart: een firewallregel op serverniveau maken voor uw SQL-database met behulp van de Azure-portal

Deze snelstart laat zien hoe u een firewallregel op serverniveau kunt maken voor een Azure SQL-database, zodat u er verbinding mee kunt maken vanuit een on-premises resource.

## <a name="prerequisites"></a>Vereisten

Deze snelstart gebruikt als uitgangspunt de resources die in deze snelstart zijn gemaakt: [Een Azure SQL-database maken in Azure Portal](sql-database-get-started-portal.md)

## <a name="log-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Meld u aan bij [Azure Portal](https://portal.azure.com/).

## <a name="create-a-server-level-firewall-rule"></a>Een serverfirewallregel maken

De service SQL Database maakt een firewall op serverniveau die voorkomt dat toepassingen en hulpprogramma’s verbinding maken met de server of databases op de server, tenzij er een firewallregel is gemaakt om de firewall te openen. Maak voor een verbinding vanaf een IP-adres buiten Azure een firewallregel voor een specifiek IP-adres of bereik met adressen. Volg deze stappen om een [SQL Database-firewallregel op serverniveau](sql-database-firewall-configure.md) te maken voor het IP-adres van de client en connectiviteit via de SQL Database-firewall alleen voor uw IP-adres toe te staan.

> [!NOTE]
> SQL Database communiceert via poort 1433. Als u verbinding probeert te maken vanuit een bedrijfsnetwerk, wordt uitgaand verkeer via poort 1433 mogelijk niet toegestaan door de firewall van uw netwerk. In dat geval kunt u geen verbinding maken met uw Azure SQL Database-server, tenzij de IT-afdeling poort 1433 openstelt.
>

1. Wanneer de implementatie is voltooid, klikt u op **SQL Databases** in het menu aan de linkerkant. Klik vervolgens op de pagina **SQL Databases** op **mySampleDatabase**. De overzichtspagina voor uw database wordt geopend, met de volledig gekwalificeerde servernaam (zoals **mynewserver20170824.database.windows.net**) en opties voor verdere configuratie.

2. Kopieer deze volledig gekwalificeerde servernaam om in volgende snelstartgidsen verbinding te maken met de server en de bijbehorende databases.

   ![servernaam](./media/sql-database-get-started-portal/server-name.png)

3. Klik op de werkbalk op **Serverfirewall instellen** zoals in de vorige afbeelding is weergegeven. De pagina **Firewallinstellingen** voor de SQL Database-server wordt geopend.

   ![serverfirewallregel](./media/sql-database-get-started-portal/server-firewall-rule.png)

4. Klik op **IP van client toevoegen** op de werkbalk om uw huidige IP-adres aan een nieuwe firewallregel toe te voegen. Een firewallregel kan poort 1433 openen voor een afzonderlijk IP-adres of voor een aantal IP-adressen.

5. Klik op **Opslaan**. Er wordt een firewallregel op serverniveau gemaakt voor uw huidige IP-adres waarbij poort 1433 op de logische server wordt geopend.

6. Klik op **OK** en sluit de pagina **Firewallinstellingen**.

U kunt nu verbinding maken met de SQL Database-server en de bijbehorende databases met behulp van SQL Server Management Studio of een ander hulpprogramma naar keuze. Dit doet u vanaf dit IP-adres via het serverbeheerdersaccount dat eerder is gemaakt.

> [!IMPORTANT]
> Voor alle Azure-services is toegang via de SQL Database-firewall standaard ingeschakeld. Klik op **UIT** op deze pagina om dit voor alle Azure-services uit te schakelen.
>

## <a name="clean-up-resources"></a>Resources opschonen

Sla deze resources op als u verder wilt gaan met [Volgende stappen](#next-steps) en wilt leren hoe u op verschillende manieren verbinding kunt maken met uw database en deze kunt bevragen. Als u echter de resources wilt verwijderen die u hebt gemaakt in deze snelstartgids, voert u de volgende stappen uit.


1. Klik in het menu links in Azure Portal op **Resourcegroepen** en klik vervolgens op **myResourceGroup**.
2. Klik op de pagina van uw resourcegroep op **Verwijderen**, typ **myResourceGroup** in het tekstvak en klik vervolgens op **Verwijderen**.

## <a name="next-steps"></a>Volgende stappen

- Nu u een database hebt, kunt u [verbinding maken met een hulpprogramma of taal naar keuze en daarmee query's uitvoeren](sql-database-connect-query.md), waaronder
  - [Verbinding maken en query's uitvoeren met behulp van SQL Server Management Studio](sql-database-connect-query-ssms.md)
  - [Verbinding maken en query's uitvoeren met behulp van Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/quickstart-sql-database?toc=/azure/sql-database/toc.json)
- Voor informatie over het ontwerpen van uw eerste database, het maken van tabellen en het invoegen van gegevens kunt u een van deze zelfstudies gebruiken:
 - [Uw eerste Azure SQL-database ontwerpen met SQL Server Management Studio](sql-database-design-first-database.md)
 - [Een Azure SQL-database ontwerpen en verbinding maken met C# en ADO.NET](sql-database-design-first-database-csharp.md)
