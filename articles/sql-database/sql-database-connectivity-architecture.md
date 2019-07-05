---
title: Azure SQL Database en SQL Data Warehouse Connectiviteitsarchitectuur | Microsoft Docs
description: Dit document wordt uitgelegd dat de architectuur van de Azure SQL-connectiviteit voor databaseverbindingen in Azure of van buiten Azure.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: rohitnayakmsft
ms.author: rohitna
ms.reviewer: carlrab, vanto
manager: craigg
ms.date: 07/02/2019
ms.openlocfilehash: 8441e64981b7157e91a56124a08c0aa02a9b1db0
ms.sourcegitcommit: 084630bb22ae4cf037794923a1ef602d84831c57
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2019
ms.locfileid: "67537925"
---
# <a name="azure-sql-connectivity-architecture"></a>Architectuur van Azure SQL-connectiviteit

In dit artikel wordt de Azure SQL Database en SQL Data Warehouse connectiviteitsarchitectuur ook uitgelegd hoe de verschillende onderdelen functie verkeer naar uw Azure SQL-exemplaar. Deze functie van de onderdelen verbinding om te leiden van netwerkverkeer naar de Azure SQL Database of SQL Data Warehouse met clients die verbinding maken vanuit Azure en met clients die verbinding maakt vanaf buiten Azure. Dit artikel bevat ook voorbeelden van scripts om te wijzigen hoe de verbinding plaatsvindt, evenals de overwegingen met betrekking tot het wijzigen van de standaardinstellingen van de verbinding.

## <a name="connectivity-architecture"></a>Connectiviteitsarchitectuur

Het volgende diagram biedt een overzicht op hoog niveau van de architectuur van Azure SQL Database-connectiviteit.

![overzicht van netwerkarchitectuur](./media/sql-database-connectivity-architecture/connectivity-overview.png)

De volgende stappen wordt beschreven hoe een verbinding met een Azure SQL-database tot stand is gebracht:

- Clients verbinding maken met de gateway, die een openbaar IP-adres is en luistert op poort 1433.
- De gateway, afhankelijk van de effectieve verbindingsbeleid, omleidingen of proxy het verkeer naar de juiste database-cluster.
- In de database is clusterverkeer doorgestuurd naar de juiste Azure SQL-database.

## <a name="connection-policy"></a>Verbindingsbeleid voor

Azure SQL Database ondersteunt de volgende drie opties voor de beleidsinstelling voor de verbinding van een SQL Database-server:

- **Omleiding (aanbevolen):** Clients tot stand brengen van verbindingen rechtstreeks naar het knooppunt waarop de database wordt gehost. Als connectiviteit wilt inschakelen, moeten de clients de uitgaande firewallregels voor alle Azure-IP-adressen in de regio met behulp van de Netwerkbeveiligingsgroep groepen (NSG) met toestaan [servicetags](../virtual-network/security-overview.md#service-tags)) voor poorten 11000-11999, niet alleen de Azure SQL Database-gateway-IP adressen op poort 1433. Omdat de pakketten Ga rechtstreeks naar de database, zijn latentie en doorvoer prestaties beter.
- **Proxy:** In deze modus worden alle verbindingen via proxy via de Azure SQL Database-gateways. Als connectiviteit wilt inschakelen, moet de client uitgaande firewallregels waarmee alleen de Azure SQL Database-gateway op een IP-adressen (doorgaans twee IP-adressen per regio) hebben. In deze modus kiezen kan leiden tot hogere latentie en lagere doorvoer, afhankelijk van de aard van de werkbelasting. We raden u aan de `Redirect` verbindingsbeleid via de `Proxy` verbindingsbeleid voor de laagste latentie en de hoogste doorvoer.
- **Standaard:** Dit is de verbindingsbeleid van kracht op alle servers na het maken, tenzij u expliciet het verbindingsbeleid naar een alter `Proxy` of `Redirect`. De effectieve beleid is afhankelijk van of verbindingen zijn afkomstig uit in Azure (`Redirect`) en buiten Azure (`Proxy`).

## <a name="connectivity-from-within-azure"></a>Connectiviteit van in Azure

Als u in Azure verbinding vanaf maakt uw verbindingen hebben een verbindingsbeleid van `Redirect` standaard. Een beleid van `Redirect` betekent dat nadat de TCP-sessie met de Azure SQL database tot stand is gebracht, de clientsessie wordt vervolgens omgeleid naar de juiste database-cluster met een wijziging in de bestemming virtueel IP-adres van die van de Azure SQL Database-gateway met die van de cluster. Daarna worden alle volgende pakketten stromen rechtstreeks aan het cluster, overslaan van de Azure SQL Database-gateway. Het volgende diagram illustreert dit netwerkverkeer.

![overzicht van netwerkarchitectuur](./media/sql-database-connectivity-architecture/connectivity-azure.png)

## <a name="connectivity-from-outside-of-azure"></a>De connectiviteit van buiten Azure

Als u verbinding vanaf buiten Azure maakt, uw verbindingen hebben een verbindingsbeleid van `Proxy` standaard. Een beleid van `Proxy` betekent dat de TCP-sessie tot stand is gebracht via de Azure SQL Database-gateway en alle volgende pakketten stromen via de gateway. Het volgende diagram illustreert dit netwerkverkeer.

![overzicht van netwerkarchitectuur](./media/sql-database-connectivity-architecture/connectivity-onprem.png)

## <a name="azure-sql-database-gateway-ip-addresses"></a>Azure SQL Database-gateway-IP-adressen

De volgende tabel bevat de IP-adressen van Gateways per regio. Voor verbinding met een Azure SQL Database, die u wilt toestaan dat het netwerkverkeer naar en van **alle** Gateways voor de regio.

In de toekomst zullen we meer Gateways toevoegen in elke regio en buiten gebruik stellen van de Gateways in de kolom uit bedrijf genomen Gateway-IP-adres van de onderstaande tabel. Meer informatie over het proces dat is opgegeven in het volgende artikel buiten gebruik stellen: [Azure SQL Database-verkeer-migratie naar nieuwere Gateways](sql-database-gateway-migration.md)


| Naam regio          | IP-adres van gateway | Uit bedrijf genomen Gateway </br> IP-adres| Opmerkingen bij buiten gebruik stellen | 
| --- | --- | --- | --- |
| Australië - oost       | 13.75.149.87, 40.79.161.1 | | |
| Australië - zuidoost | 191.239.192.109, 13.73.109.251 | | |
| Brazilië - zuid         | 104.41.11.5        |                 | |
| Canada - midden       | 40.85.224.249      |                 | |
| Canada - oost          | 40.86.226.166      |                 | |
| US - centraal           | 13.67.215.62, 52.182.137.15 | 23.99.160.139 | Er zijn geen verbindingen na 1 September 2019 |
| China Oost 1         | 139.219.130.35     |                 | |
| China - oost 2         | 40.73.82.1         |                 | |
| China North 1        | 139.219.15.17      |                 | |
| China - noord 2        | 40.73.50.0         |                 | |
| Azië - oost            | 191.234.2.139, 52.175.33.150 |       | |
| VS-Oost 1            | 40.121.158.30, 40.79.153.12 | 191.238.6.43 | Er zijn geen verbindingen na 1 September 2019 |
| US - oost 2            | 40.79.84.180, 52.177.185.181, 52.167.104.0 | 191.239.224.107    | Er zijn geen verbindingen na 1 September 2019 |
| Frankrijk - centraal       | 40.79.137.0, 40.79.129.1 |           | |
| Duitsland - centraal      | 51.4.144.100       |                 | |
| Duitsland-Noordoost   | 51.5.144.179       |                 | |
| India - centraal        | 104.211.96.159     |                 | |
| India - zuid          | 104.211.224.146    |                 | |
| India - west           | 104.211.160.80     |                 | |
| Japan - oost           | 13.78.61.196, 40.79.184.8, 13.78.106.224 | 191.237.240.43 | Er zijn geen verbindingen na 1 September 2019 |
| Japan - west           | 104.214.148.156, 40.74.100.192 | 191.238.68.11 | Er zijn geen verbindingen na 1 September 2019 |
| Korea - centraal        | 52.231.32.42       |                 | |
| Korea - zuid          | 52.231.200.86      |                 | |
| US - noord-centraal     | 23.96.178.199      | 23.98.55.75     | Er zijn geen verbindingen na 1 September 2019 |
| Europa - noord         | 40.113.93.91       | 191.235.193.75  | Er zijn geen verbindingen na 1 September 2019 |
| US - zuid-centraal     | 13.66.62.124       | 23.98.162.75    | Er zijn geen verbindingen na 1 September 2019 |
| Azië - zuidoost      | 104.43.15.0        | 23.100.117.95   | Er zijn geen verbindingen na 1 September 2019 |
| Verenigd Koninkrijk Zuid             | 51.140.184.11      |                 | |
| Verenigd Koninkrijk West              | 51.141.8.11        |                 | |
| US - west-centraal      | 13.78.145.25       |                 | |
| Europa -west          | 191.237.232.75, 40.68.37.158 |       | |
| VS-West 1            | 23.99.34.75, 104.42.238.205 |        | |
| US - west 2            | 13.66.226.202      |                 | |
|                      |                    |                 | |

## <a name="change-azure-sql-database-connection-policy"></a>Azure SQL Database-verbindingsbeleid wijzigen

U kunt het beleid van de Azure SQL Database-verbinding voor een Azure SQL Database-server wijzigen met de [connectiviteit beleid](https://docs.microsoft.com/cli/azure/sql/server/conn-policy) opdracht.

- Als het verbindingsbeleid is ingesteld op `Proxy`, alle netwerkverbindingen stroom van pakketten via de Azure SQL Database-gateway. Voor deze instelling moet u uitgaand verkeer op alleen de Azure SQL Database-gateway-IP toestaan. Met behulp van een instelling van `Proxy` heeft meer latentie dan een instelling van `Redirect`.
- Als het instellen van het verbindingsbeleid `Redirect`, alle netwerkverbindingen pakketten stroom rechtstreeks naar de databasecluster. Voor deze instelling is moet u uitgaand verkeer naar meerdere IP-adressen toestaan.

## <a name="script-to-change-connection-settings-via-powershell"></a>Script voor het wijzigen van instellingen van de verbinding via PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
> [!IMPORTANT]
> De PowerShell Azure Resource Manager-module nog steeds wordt ondersteund door Azure SQL Database, maar alle toekomstige ontwikkeling is voor de module Az.Sql. Zie voor deze cmdlets [AzureRM.Sql](https://docs.microsoft.com/powershell/module/AzureRM.Sql/). De argumenten voor de opdrachten in de Az-module en de AzureRm-modules zijn vrijwel identiek zijn. Het volgende script vereist de [Azure PowerShell-module](/powershell/azure/install-az-ps).

De volgende PowerShell-script laat zien hoe het verbindingsbeleid wijzigen.

```powershell
# Get SQL Server ID
$sqlserverid=(Get-AzSqlServer -ServerName sql-server-name -ResourceGroupName sql-server-group).ResourceId

# Set URI
$id="$sqlserverid/connectionPolicies/Default"

# Get current connection policy
(Get-AzResource -ResourceId $id).Properties.connectionType

# Update connection policy
Set-AzResource -ResourceId $id -Properties @{"connectionType" = "Proxy"} -f
```

## <a name="script-to-change-connection-settings-via-azure-cli"></a>Script voor het wijzigen van instellingen van de verbinding via Azure CLI

> [!IMPORTANT]
> Dit script moet de [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli).

### <a name="azure-cli-in-a-bash-shell"></a>Azure CLI in een bash-shell

> [!IMPORTANT]
> Dit script moet de [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli).

De volgende CLI-script laat zien hoe het verbindingsbeleid in een bash-shell wijzigen.

```azurecli-interactive
# Get SQL Server ID
sqlserverid=$(az sql server show -n sql-server-name -g sql-server-group --query 'id' -o tsv)

# Set URI
ids="$sqlserverid/connectionPolicies/Default"

# Get current connection policy
az resource show --ids $ids

# Update connection policy
az resource update --ids $ids --set properties.connectionType=Proxy
```

### <a name="azure-cli-from-a-windows-command-prompt"></a>Azure CLI vanaf een opdrachtprompt van Windows

> [!IMPORTANT]
> Dit script moet de [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli).

De volgende CLI-script laat zien hoe het verbindingsbeleid wijzigen vanaf een opdrachtprompt van Windows (met Azure CLI is geïnstalleerd).

```azurecli
# Get SQL Server ID and set URI
FOR /F "tokens=*" %g IN ('az sql server show --resource-group myResourceGroup-571418053 --name server-538465606 --query "id" -o tsv') do (SET sqlserverid=%g/connectionPolicies/Default)

# Get current connection policy
az resource show --ids %sqlserverid%

# Update connection policy
az resource update --ids %sqlserverid% --set properties.connectionType=Proxy
```

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over het wijzigen van het beleid van de Azure SQL Database-verbinding voor een Azure SQL Database-server [connectiviteit beleid](https://docs.microsoft.com/cli/azure/sql/server/conn-policy).
- Zie voor meer informatie over het gedrag van Azure SQL Database-verbindingen voor clients die gebruikmaken van ADO.NET 4.5 of hoger [poorten boven 1433 voor ADO.NET 4.5](sql-database-develop-direct-route-ports-adonet-v12.md).
- Zie voor informatie over algemene ontwikkeling overzicht, [overzicht van ontwikkeling op SQL-databases](sql-database-develop-overview.md).