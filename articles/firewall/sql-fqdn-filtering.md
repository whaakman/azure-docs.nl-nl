---
title: Azure-toepassing firewallregels configureren met FQDN's van SQL
description: In dit artikel leert u hoe u SQL-FQDN's configureren in Azure-Firewall-regels van toepassing.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 7/11/2019
ms.author: victorh
ms.openlocfilehash: e188a5dda8f936ad369aa2b9222bc726bb0d6a5e
ms.sourcegitcommit: 1572b615c8f863be4986c23ea2ff7642b02bc605
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2019
ms.locfileid: "67786591"
---
# <a name="configure-azure-firewall-application-rules-with-sql-fqdns"></a>Azure-toepassing firewallregels configureren met FQDN's van SQL

> [!IMPORTANT]
> Toepassing firewallregels voor Azure met SQL-FQDN's is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt.
> Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

U kunt nu Azure-toepassing firewallregels configureren met SQL-FQDN's. Hiermee kunt u de toegang van uw virtuele netwerken om alleen de opgegeven SQL server-exemplaren te beperken.

Met SQL-FQDN's, kunt u verkeer filteren:

- Vanaf uw VNets met een Azure SQL-Database of een Azure SQL datawarehouse. Bijvoorbeeld: Alleen toegang geven tot *sql-server1.database.windows.net*.
- Van on-premises naar Azure SQL Managed Instances of SQL IaaS die worden uitgevoerd in uw VNets.
- Van knooppunt-naar-spoke Azure SQL Managed Instances of SQL IaaS die worden uitgevoerd in uw VNets.

Tijdens de preview-versie, de FQDN van de SQL-filtering wordt ondersteund in [proxymodus](https://docs.microsoft.com/azure/sql-database/sql-database-connectivity-architecture#connection-policy) alleen (poort 1433). Als u SQL in de standaardmodus voor de omleiding gebruiken, kunt u filteren toegang met behulp van de SQL-servicetag als onderdeel van [regels](overview.md#network-traffic-filtering-rules).
Als u niet-standaard poorten voor SQL IaaS-verkeer gebruikt, kunt u deze poorten configureren in de firewallregels voor de toepassing.

> [!NOTE]
> Regels van de toepassing met SQL FQDN's is momenteel beschikbaar in alle regio's via Azure CLI, REST en sjablonen. De portal-gebruikersinterface incrementeel aan regio's wordt toegevoegd en is beschikbaar in alle regio's wanneer de implementatie is voltooid.

## <a name="configure-using-azure-cli"></a>Configureren met behulp van Azure CLI

1. Implementeer een [Firewall van Azure met behulp van Azure CLI](deploy-cli.md).
2. Als u verkeer naar Azure SQL Database, SQL Data Warehouse of SQL Managed Instance filteren, controleert u of de SQL-verbindingsmodus is ingesteld op **Proxy**. Zie voor meer informatie over het overschakelen op SQL-verbindingsmodus, [Azure SQL-Connectiviteitsarchitectuur](https://docs.microsoft.com/azure/sql-database/sql-database-connectivity-architecture#change-azure-sql-database-connection-policy). 

   > [!NOTE]
   > SQL *proxy* modus kan leiden tot meer latentie in vergelijking met *omleiden*. Als u doorgaan met het gebruik van de omleidingsmodus, dit de standaardinstelling voor clients die verbinding maken in Azure wilt is, kunt u toegang met behulp van de SQL filteren [servicetag](service-tags.md) in de firewall [regels](tutorial-firewall-deploy-portal.md#configure-a-network-rule).

3. Een regel voor een toepassing met SQL-FQDN voor toegang tot een SQL-server configureren:

   ```azurecli
   az network firewall application-rule create \
   -g FWRG \
   -f azfirewall \
   -c FWAppRules \
   -n srule \
   --protocols mssql=1433 \
   --source-addresses 10.0.0.0/24 \
   --target-fqdns sql-serv1.database.windows.net
   ```

## <a name="configure-using-the-azure-portal"></a>Configureren met behulp van de Azure-portal
1. Implementeer een [Firewall van Azure met behulp van Azure CLI](deploy-cli.md).
2. Als u verkeer naar Azure SQL Database, SQL Data Warehouse of SQL Managed Instance filteren, controleert u of de SQL-verbindingsmodus is ingesteld op **Proxy**. Zie voor meer informatie over het overschakelen op SQL-verbindingsmodus, [Azure SQL-Connectiviteitsarchitectuur](../sql-database/sql-database-connectivity-architecture.md#change-azure-sql-database-connection-policy). 

   > [!NOTE]
   > SQL *proxy* modus kan leiden tot meer latentie in vergelijking met *omleiden*. Als u doorgaan met het gebruik van de omleidingsmodus, dit de standaardinstelling voor clients die verbinding maken in Azure wilt is, kunt u toegang met behulp van de SQL filteren [servicetag](service-tags.md) in de firewall [regels](tutorial-firewall-deploy-portal.md#configure-a-network-rule).
3. De toepassing-regel met het juiste protocol, de poort en de FQDN van de SQL toevoegen en selecteer vervolgens **opslaan**.
   ![toepassing-regel met de FQDN van de SQL](media/sql-fqdn-filtering/application-rule-sql.png)
4. Toegang SQL via een virtuele machine in een VNet dat het verkeer via de firewall filtert. 
5. Valideren dat [Azure Firewall-logboeken](log-analytics-samples.md) tonen het verkeer is toegestaan.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over SQL-proxy en omleidings-modi, [Azure SQL database-connectiviteitsarchitectuur](../sql-database/sql-database-connectivity-architecture.md).