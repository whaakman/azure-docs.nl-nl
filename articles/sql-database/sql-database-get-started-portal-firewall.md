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
ms.date: 12/01/2018
ms.openlocfilehash: cab92539b5019d4807ddefb2b84279c844f53016
ms.sourcegitcommit: 333d4246f62b858e376dcdcda789ecbc0c93cd92
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/01/2018
ms.locfileid: "52721898"
---
# <a name="quickstart-create-a-server-level-firewall-rule-for-your-sql-database-using-the-azure-portal"></a>Snelstart: een firewallregel op serverniveau maken voor uw SQL-database met behulp van de Azure-portal

Deze snelstart laat zien hoe u een firewallregel op serverniveau kunt maken voor een Azure SQL-database, zodat u er verbinding mee kunt maken vanuit een on-premises resource.

## <a name="prerequisites"></a>Vereisten

Deze quickstart gebruikt de resources die zijn gemaakt in [Een Azure SQL-database maken in de Azure-portal](sql-database-get-started-portal.md) als uitgangspunt.

## <a name="sign-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Meld u aan bij [Azure Portal](https://portal.azure.com/).

## <a name="create-a-server-level-firewall-rule"></a>Een serverfirewallregel maken

De service SQL Database maakt een firewall op serverniveau. De firewall voorkomt dat toepassingen en hulpprogramma's verbinding maken met de server of serverdatabases, tenzij u een firewallregel maakt om de firewall te openen. Voor een verbinding vanaf een IP-adres buiten Azure maakt u een firewallregel voor een specifiek IP-adres of bereik van adressen. Zie [Firewallregel op SQL Database-serverniveau](sql-database-firewall-configure.md) voor meer informatie over firewallregels.

> [!NOTE]
> SQL Database communiceert via poort 1433. Als u verbinding probeert te maken vanuit een bedrijfsnetwerk, is uitgaand verkeer via poort 1433 mogelijk niet toegestaan vanwege de firewall van het netwerk. In dat geval kunt u geen verbinding maken met uw Azure SQL Database-server, tenzij de IT-afdeling poort 1433 openstelt.
>

Volg deze stappen om een firewallregel op serverniveau te maken voor het IP-adres van de client en connectiviteit via de SQL Database-firewall alleen voor uw IP-adres toe te staan.

1. Wanneer de implementatie van [Vereiste Azure SQL-database](#prerequisites) is voltooid, selecteert u **SQL-databases** in het menu links en kiest u **mySampleDatabase** op de pagina **SQL-databases**. De overzichtspagina voor uw database wordt geopend, met de volledig gekwalificeerde servernaam (zoals **mynewserver20170824.database.windows.net**) en opties voor verdere configuratie.

2. Kopieer deze volledig gekwalificeerde servernaam zodat u deze kunt gebruiken wanneer u in andere quickstarts verbinding maakt met de server en de bijbehorende databases.

   ![servernaam](./media/sql-database-get-started-portal/server-name.png)

3. Selecteer **Serverfirewall instellen** op de werkbalk. De pagina **Firewallinstellingen** voor de SQL Database-server wordt geopend.

   ![serverfirewallregel](./media/sql-database-get-started-portal/server-firewall-rule.png)

4. Kies **IP van client toevoegen** op de werkbalk om uw huidige IP-adres aan een nieuwe firewallregel toe te voegen. Een firewallregel kan poort 1433 openen voor een afzonderlijk IP-adres of voor een aantal IP-adressen.

   > [!IMPORTANT]
   > Voor alle Azure-services is toegang via de SQL Database-firewall standaard ingeschakeld. Kies **UIT** op deze pagina om dit voor alle Azure-services uit te schakelen.
   >

5. Selecteer **Opslaan**. Er wordt een firewallregel op serverniveau gemaakt voor uw huidige IP-adres waarbij poort 1433 op de logische server wordt geopend.

6. Sluit de pagina **Firewallinstellingen**.

Nu kunt u SQL Server Management Studio of een ander hulpprogramma naar keuze gebruiken om vanaf dit IP-adres via het serverbeheerdersaccount dat eerder is gemaakt verbinding te maken met de SQL Database-server en de bijbehorende databases.

## <a name="clean-up-resources"></a>Resources opschonen

Sla deze resources op als u verder wilt gaan met [Volgende stappen](#next-steps) en wilt leren hoe u op verschillende manieren verbinding kunt maken met uw database en deze kunt bevragen. Als u echter de resources wilt verwijderen die u hebt gemaakt in deze quickstart, voert u de volgende stappen uit.


1. Selecteer in het menu links in de Azure-portal **Resourcegroepen** en selecteer vervolgens **myResourceGroup**.
2. Selecteer op de pagina van uw resourcegroep de optie **Verwijderen**, typ **myResourceGroup** in het tekstvak en selecteer **Verwijderen**.

## <a name="next-steps"></a>Volgende stappen

- Nu u een database hebt, kunt u [verbinding maken met een hulpprogramma of taal naar keuze en daarmee query's uitvoeren](sql-database-connect-query.md), waaronder
  - [Verbinding maken en query's uitvoeren met behulp van SQL Server Management Studio](sql-database-connect-query-ssms.md)
  - [Verbinding maken en query's uitvoeren met behulp van Azure Data Studio](https://docs.microsoft.com/sql/azure-data-studio/quickstart-sql-database?toc=/azure/sql-database/toc.json)
- Voor informatie over het ontwerpen van uw eerste database, het maken van tabellen en het invoegen van gegevens kunt u een van deze zelfstudies gebruiken:
  - [Uw eerste Azure SQL-database ontwerpen met SQL Server Management Studio](sql-database-design-first-database.md)
  - [Een Azure SQL-database ontwerpen en verbinding maken met C# en ADO.NET](sql-database-design-first-database-csharp.md)
