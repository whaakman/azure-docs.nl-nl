---
title: Azure verkeer doorsturen naar Azure SQL Database en SQL Data Warehouse | Microsoft Docs
description: Dit document wordt uitgelegd dat de Azure SQL Database en SQL Data Warehouse connectiviteitsarchitectuur uit in Azure of van buiten Azure.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: srdan-bozovic-msft
ms.author: srbozovi
ms.reviewer: carlrab
manager: craigg
ms.date: 02/25/2019
ms.openlocfilehash: dde54894867811a34340f6dd4d0236b6b54e8144
ms.sourcegitcommit: 3f4ffc7477cff56a078c9640043836768f212a06
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/04/2019
ms.locfileid: "57307714"
---
# <a name="azure-sql-connectivity-architecture"></a>Architectuur van Azure SQL-connectiviteit

In dit artikel wordt de Azure SQL Database en SQL Data Warehouse connectiviteitsarchitectuur ook uitgelegd hoe de verschillende onderdelen functie verkeer naar uw Azure SQL-exemplaar. Deze functie van de onderdelen verbinding om te leiden van netwerkverkeer naar de Azure SQL Database of SQL Data Warehouse met clients die verbinding maken vanuit Azure en met clients die verbinding maakt vanaf buiten Azure. Dit artikel bevat ook voorbeelden van scripts om te wijzigen hoe de verbinding plaatsvindt, evenals de overwegingen met betrekking tot het wijzigen van de standaardinstellingen van de verbinding.

> [!IMPORTANT]
> **[Toekomstige wijzigingen] Voor service-eindpunt-verbindingen met Azure SQL-servers, een `Default` connectiviteit gedrag wordt gewijzigd in `Redirect`.**
> Klanten wordt aangeraden om nieuwe servers en set aandeel bestaande klanten met verbindingstype expliciet zijn ingesteld op omleiding (voorkeur) of Proxy, afhankelijk van hun architectuur, verbinding te maken.
>
> Om te voorkomen dat connectiviteit via een service-eindpunt belangrijke in bestaande omgevingen als gevolg van deze wijziging, gebruiken we telemetrie Doe het volgende:
>
> - Voor servers die worden gedetecteerd die zijn toegankelijk via service-eindpunten voor de wijziging, schakelen we het verbindingstype voor `Proxy`.
> - Voor alle andere servers, schakelen we de verbinding type zal worden overgeschakeld naar de `Redirect`.
>
> De invloed van service-eindpuntgebruikers in de volgende scenario's mogelijk nog steeds:
>
> - Toepassing verbinding maakt met een bestaande server niet regelmatig worden, zodat onze telemetrie is niet de informatie over deze toepassingen vastleggen
> - Logica voor automatische implementatie maakt u een SQL-databaseserver ervan uitgaande dat het standaardgedrag voor verbindingen met de service-eindpunt `Proxy`
>
> Als verbindingen met de service-eindpunt kunnen niet worden gemaakt met Azure SQL-server, en u een vermoeden bestaat dat u worden beïnvloed door deze wijziging, Controleer of dat de verbindingstype expliciet is ingesteld op `Redirect`. Als dit het geval is, hebt u het openen van VM-firewallregels en Netwerkbeveiligingsgroep groepen (NSG) voor alle Azure-IP-adressen in de regio die deel uitmaken van Sql [servicetag](../virtual-network/security-overview.md#service-tags) voor poorten 11000 12000. Als dit niet een optie voor u is, schakelt u over server expliciet aan `Proxy`.
> [!NOTE]
> In dit onderwerp is van toepassing op Azure SQL Database-servers die als host fungeert voor individuele databases en elastische pools en databases van SQL Data Warehouse. Voor het gemak wordt de term 'SQL Database' gebruikt wanneer er wordt verwezen naar zowel SQL Database als SQL Data Warehouse.

## <a name="connectivity-architecture"></a>Connectiviteitsarchitectuur

Het volgende diagram biedt een overzicht op hoog niveau van de architectuur van Azure SQL Database-connectiviteit.

![overzicht van netwerkarchitectuur](./media/sql-database-connectivity-architecture/connectivity-overview.png)

De volgende stappen wordt beschreven hoe een verbinding met een Azure SQL-database tot stand is gebracht:

- Clients verbinding maken met de gateway, die een openbaar IP-adres is en luistert op poort 1433.
- De gateway, afhankelijk van de effectieve verbindingsbeleid, omleidingen of proxy het verkeer naar de juiste database-cluster.
- In de database is clusterverkeer doorgestuurd naar de juiste Azure SQL-database.

## <a name="connection-policy"></a>Verbindingsbeleid voor

Azure SQL Database ondersteunt de volgende drie opties voor de beleidsinstelling voor de verbinding van een SQL Database-server:

- **Omleiding (aanbevolen):** Clients tot stand brengen van verbindingen rechtstreeks naar het knooppunt waarop de database wordt gehost. Als connectiviteit wilt inschakelen, moeten de clients de uitgaande firewallregels voor alle Azure-IP-adressen in de regio met behulp van de Netwerkbeveiligingsgroep groepen (NSG) met toestaan [servicetags](../virtual-network/security-overview.md#service-tags)) voor poorten 11000-12000, niet alleen de Azure SQL Database-gateway-IP adressen op poort 1433. Omdat de pakketten Ga rechtstreeks naar de database, zijn latentie en doorvoer prestaties beter.
- **Proxy:** In deze modus worden alle verbindingen via proxy via de Azure SQL Database-gateways. Als connectiviteit wilt inschakelen, moet de client uitgaande firewallregels waarmee alleen de Azure SQL Database-gateway op een IP-adressen (doorgaans twee IP-adressen per regio) hebben. In deze modus kiezen kan leiden tot hogere latentie en lagere doorvoer, afhankelijk van de aard van de werkbelasting. We raden u aan de `Redirect` verbindingsbeleid via de `Proxy` verbindingsbeleid voor de laagste latentie en de hoogste doorvoer.
- **Standaard:** Dit is de verbindingsbeleid van kracht op alle servers na het maken, tenzij u expliciet het verbindingsbeleid naar een alter `Proxy` of `Redirect`. De effectieve beleid is afhankelijk van of verbindingen zijn afkomstig uit in Azure (`Redirect`) en buiten Azure (`Proxy`).

## <a name="connectivity-from-within-azure"></a>Connectiviteit van in Azure

Als u in Azure verbinding vanaf maakt uw verbindingen hebben een verbindingsbeleid van `Redirect` standaard. Een beleid van `Redirect` betekent dat nadat de TCP-sessie met de Azure SQL database tot stand is gebracht, de clientsessie wordt vervolgens omgeleid naar de juiste database-cluster met een wijziging in de bestemming virtueel IP-adres van die van de Azure SQL Database-gateway met die van de cluster. Daarna worden alle volgende pakketten stromen rechtstreeks aan het cluster, overslaan van de Azure SQL Database-gateway. Het volgende diagram illustreert dit netwerkverkeer.

![overzicht van netwerkarchitectuur](./media/sql-database-connectivity-architecture/connectivity-azure.png)

## <a name="connectivity-from-outside-of-azure"></a>De connectiviteit van buiten Azure

Als u verbinding vanaf buiten Azure maakt, uw verbindingen hebben een verbindingsbeleid van `Proxy` standaard. Een beleid van `Proxy` betekent dat de TCP-sessie tot stand is gebracht via de Azure SQL Database-gateway en alle volgende pakketten stromen via de gateway. Het volgende diagram illustreert dit netwerkverkeer.

![overzicht van netwerkarchitectuur](./media/sql-database-connectivity-architecture/connectivity-onprem.png)

## <a name="azure-sql-database-gateway-ip-addresses"></a>Azure SQL Database-gateway-IP-adressen

Als u wilt verbinding maken met een Azure SQL database vanuit on-premises bronnen, die u wilt toestaan dat uitgaand netwerkverkeer naar de Azure SQL Database-gateway voor uw Azure-regio. Uw verbindingen alleen verlopen via de gateway wanneer u verbinding maakt `Proxy` modus, dit de standaardinstelling is bij verbinding maakt vanaf on-premises resources.

De volgende tabel geeft een lijst van de primaire en secundaire IP-adressen van de Azure SQL Database-gateway voor alle regio's van gegevens. Voor sommige regio's zijn er twee IP-adressen. In deze regio's, het primaire IP-adres is het huidige IP-adres van de gateway en het tweede IP-adres is een failover-IP-adres. De failover-adres is het adres waarop we uw server te houden van de van hoge servicebeschikbaarheid mogelijk verplaatsen. Voor deze regio's raden wij u uitgaand naar beide IP-adressen toestaat. Het tweede IP-adres is eigendom van Microsoft en luistert niet op alle services totdat deze is geactiveerd door Azure SQL Database om verbindingen te accepteren.

| Naam regio | Primaire IP-adres | Secundaire IP-adres |
| --- | --- |--- |
| Australië - oost | 13.75.149.87 | 40.79.161.1 |
| Australië - zuidoost | 191.239.192.109 | 13.73.109.251 |
| Brazilië - zuid | 104.41.11.5 | |
| Canada - midden | 40.85.224.249 | |
| Canada - oost | 40.86.226.166 | |
| US - centraal | 23.99.160.139 | 13.67.215.62 |
| China Oost 1 | 139.219.130.35 | |
| China - oost 2 | 40.73.82.1 | |
| China North 1 | 139.219.15.17 | |
| China - noord 2 | 40.73.50.0 | |
| Azië - oost | 191.234.2.139 | 52.175.33.150 |
| VS-Oost 1 | 191.238.6.43 | 40.121.158.30 |
| US - oost 2 | 191.239.224.107 | 40.79.84.180 * |
| Frankrijk - centraal | 40.79.137.0 | 40.79.129.1 |
| Duitsland - centraal | 51.4.144.100 | |
| Duitsland-Noordoost | 51.5.144.179 | |
| India - centraal | 104.211.96.159 | |
| India - zuid | 104.211.224.146 | |
| India - west | 104.211.160.80 | |
| Japan - oost | 191.237.240.43 | 13.78.61.196 |
| Japan - west | 191.238.68.11 | 104.214.148.156 |
| Korea - centraal | 52.231.32.42 | |
| Korea - zuid | 52.231.200.86 |  |
| US - noord-centraal | 23.98.55.75 | 23.96.178.199 |
| Europa - noord | 191.235.193.75 | 40.113.93.91 |
| US - zuid-centraal | 23.98.162.75 | 13.66.62.124 |
| Azië - zuidoost | 23.100.117.95 | 104.43.15.0 |
| Verenigd Koninkrijk Zuid | 51.140.184.11 | |
| US - west-centraal | 13.78.145.25 | |
| Europa -west | 191.237.232.75 | 40.68.37.158 |
| VS-West 1 | 23.99.34.75 | 104.42.238.205 |
| US - west 2 | 13.66.226.202 | |
||||

\* **OPMERKING:** *VS-Oost 2* heeft ook een tertiaire IP-adres van `52.167.104.0`.

## <a name="change-azure-sql-database-connection-policy"></a>Azure SQL Database-verbindingsbeleid wijzigen

U kunt het beleid van de Azure SQL Database-verbinding voor een Azure SQL Database-server wijzigen met de [connectiviteit beleid](https://docs.microsoft.com/cli/azure/sql/server/conn-policy) opdracht.

- Als het verbindingsbeleid is ingesteld op `Proxy`, alle netwerkverbindingen stroom van pakketten via de Azure SQL Database-gateway. Voor deze instelling moet u uitgaand verkeer op alleen de Azure SQL Database-gateway-IP toestaan. Met behulp van een instelling van `Proxy` heeft meer latentie dan een instelling van `Redirect`.
- Als het instellen van het verbindingsbeleid `Redirect`, alle netwerkverbindingen pakketten stroom rechtstreeks naar de databasecluster. Voor deze instelling is moet u uitgaand verkeer naar meerdere IP-adressen toestaan.

## <a name="script-to-change-connection-settings-via-powershell"></a>Script voor het wijzigen van instellingen van de verbinding via PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

> [!IMPORTANT]
> Dit script moet de [Azure PowerShell-module](/powershell/azure/install-az-ps).

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
> Dit script moet de [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).

De volgende CLI-script laat zien hoe het verbindingsbeleid wijzigen.

```azurecli-interactive
# Get SQL Server ID
sqlserverid=$(az sql server show -n sql-server-name -g sql-server-group --query 'id' -o tsv)

# Set URI
id="$sqlserverid/connectionPolicies/Default"

# Get current connection policy
az resource show --ids $id

# Update connection policy
az resource update --ids $id --set properties.connectionType=Proxy
```

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over het wijzigen van het beleid van de Azure SQL Database-verbinding voor een Azure SQL Database-server [connectiviteit beleid](https://docs.microsoft.com/cli/azure/sql/server/conn-policy).
- Zie voor meer informatie over het gedrag van Azure SQL Database-verbindingen voor clients die gebruikmaken van ADO.NET 4.5 of hoger [poorten boven 1433 voor ADO.NET 4.5](sql-database-develop-direct-route-ports-adonet-v12.md).
- Zie voor informatie over algemene ontwikkeling overzicht, [overzicht van ontwikkeling op SQL-databases](sql-database-develop-overview.md).
