---
title: Azure SQL Database connectivity-architectuur | Microsoft Docs
description: Dit document wordt uitgelegd dat de Azure SQLDB connectiviteit architectuur van Azure of vanuit buiten Azure.
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: monicar
ms.assetid: 
ms.service: sql-database
ms.custom: DBs & servers
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: On Demand
ms.date: 01/24/2018
ms.author: carlrab
ms.openlocfilehash: 81f96c223fb5ad2c37bd0679743f14980a5885b0
ms.sourcegitcommit: 28178ca0364e498318e2630f51ba6158e4a09a89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="azure-sql-database-connectivity-architecture"></a>Azure SQL Database Connectivity-architectuur 

Dit artikel wordt uitgelegd van de Azure SQL Database connectivity-architectuur en wordt uitgelegd hoe de andere onderdelen functie voor het verkeer naar uw Azure SQL Database-exemplaar. Deze Azure SQL Database connectivity onderdelen functie om netwerkverkeer te regelen met de Azure-database met clients die verbinding maken vanuit Azure en clients die verbinding maakt vanaf buiten Azure. In dit artikel biedt ook scriptvoorbeelden wijzigen hoe de verbinding plaatsvindt, evenals de overwegingen die betrekking hebben op de standaardinstellingen van de verbinding wijzigen. 

## <a name="connectivity-architecture"></a>Connectiviteitsarchitectuur

Het volgende diagram biedt een overzicht van de Azure SQL Database connectivity-architectuur.

![overzicht van de architectuur](./media/sql-database-connectivity-architecture/architecture-overview.png)


De volgende stappen wordt beschreven hoe een verbinding is gemaakt met een Azure SQL database via de Azure SQL Database software load balancer (SLB) en de Azure SQL Database-gateway.

- Clients in Azure of buiten Azure verbinding met de SLB dat een openbare IP-adres en luistert op poort 1433.
- De SLB stuurt het verkeer naar de Azure SQL Database-gateway.
- De gateway stuurt het verkeer naar de juiste proxy-middleware.
- De proxy-middleware leidt het verkeer naar de juiste Azure SQL-database.

> [!IMPORTANT]
> Elk van deze componenten heeft gedistribueerde denial-of-service (DDoS) beveiliging ingebouwde op het netwerk en de app-laag.
>

## <a name="connectivity-from-within-azure"></a>Verbinding tussen in Azure

Als u verbinding vanaf in Azure maakt, uw verbindingen hebben een beleid van **omleiden** standaard. Een beleid van **omleiden** betekent dat verbindingen nadat de TCP-sessie tot stand is gebracht met de Azure SQL database, de clientsessie wordt vervolgens omgeleid naar de proxy-middleware met een wijziging in het virtuele IP-adres voor doel van die van de Azure SQL Database-gateway met die van de proxy-middleware. Daarna alle latere pakketten stromen rechtstreeks via de proxy-middleware voor het omzeilen van de Azure SQL Database-gateway. Het volgende diagram illustreert dit netwerkverkeer.

![overzicht van de architectuur](./media/sql-database-connectivity-architecture/connectivity-from-within-azure.png)

## <a name="connectivity-from-outside-of-azure"></a>Connectiviteit van buiten Azure

Als u verbinding vanaf buiten Azure maakt, uw verbindingen hebben een beleid van **Proxy** standaard. Een beleid van **Proxy** betekent dat de TCP-sessie tot stand is gebracht via de Azure SQL Database-gateway en alle latere pakketten stromen via de gateway. Het volgende diagram illustreert dit netwerkverkeer.

![overzicht van de architectuur](./media/sql-database-connectivity-architecture/connectivity-from-outside-azure.png)

## <a name="azure-sql-database-gateway-ip-addresses"></a>IP-adressen van Azure SQL Database-gateway

Voor verbinding met een Azure SQL database van de lokale bronnen, moet u uitgaand netwerkverkeer naar de Azure SQL Database-gateway voor uw Azure-regio toestaan. Uw verbindingen gaat alleen via de gateway om verbinding te maken in de Proxy-modus de standaardinstelling is bij het verbinden van lokale bronnen.

De volgende tabel bevat de primaire en secundaire IP-adressen van de Azure SQL Database-gateway voor alle regio's van gegevens. Er zijn twee IP-adressen voor bepaalde gebieden. In deze regio's, de primaire IP-adres is het huidige IP-adres van de gateway en het tweede IP-adres is een failover-IP-adres. De failover-adres is het adres waarop we uw server te houden van hoge beschikbaarheid van de service mogelijk verplaatsen. Voor deze regio's, is het raadzaam dat u uitgaand verkeer naar de IP-adressen toestaan. Het tweede IP-adres is het eigendom van Microsoft en luistert niet op alle services totdat deze is geactiveerd door Azure SQL Database om verbindingen te accepteren.

| Naam regio | Primaire IP-adres | Secundaire IP-adres |
| --- | --- |--- |
| Australië - oost | 191.238.66.109 | 13.75.149.87 |
| Australië - zuidoost | 191.239.192.109 | 13.73.109.251 |
| Brazilië - zuid | 104.41.11.5 | |
| Canada - midden | 40.85.224.249 | |
| Canada - oost | 40.86.226.166 | |
| VS - midden | 23.99.160.139 | 13.67.215.62 |
| Oost-Azië | 191.234.2.139 | 52.175.33.150 |
| VS-Oost 1 | 191.238.6.43 | 40.121.158.30 |
| VS - oost 2 | 191.239.224.107 | 40.79.84.180 * |
| India - midden | 104.211.96.159  | |
| India - zuid | 104.211.224.146  | |
| India - west | 104.211.160.80 | |
| Japan - oost | 191.237.240.43 | 13.78.61.196 |
| Japan - west | 191.238.68.11 | 104.214.148.156 |
| Korea - centraal | 52.231.32.42 | |
| Korea - zuid | 52.231.200.86 |  |
| Noord-centraal VS | 23.98.55.75 | 23.96.178.199 |
| Noord-Europa | 191.235.193.75 | 40.113.93.91 |
| Zuid-centraal VS | 23.98.162.75 | 13.66.62.124 |
| Zuidoost-Azië | 23.100.117.95 | 104.43.15.0 |
| VK, noord | 13.87.97.210 | |
| VK Zuid 1 | 51.140.184.11 | |
| VK, zuid 2 | 13.87.34.7 | |
| Verenigd Koninkrijk West | 51.141.8.11  | |
| West-centraal VS | 13.78.145.25 | |
| West-Europa | 191.237.232.75 | 40.68.37.158 |
| VS-West 1 | 23.99.34.75 | 104.42.238.205 |
| VS - west 2 | 13.66.226.202  | |
||||

\***Opmerking:** *VS-Oost 2* heeft ook een tertiaire IP-adres `52.167.104.0`.

## <a name="change-azure-sql-database-connection-policy"></a>Azure SQL Database verbindingsbeleid wijzigen

Om te wijzigen van het beleid van de Azure SQL Database-verbinding voor een Azure SQL Database-server, gebruiken de [REST-API](https://msdn.microsoft.com/library/azure/mt604439.aspx).

- Als uw verbindingsbeleid voor de is ingesteld op **Proxy**, alle netwerkapparaten stroom van pakketten via de Azure SQL Database-gateway. Voor deze instelling moet u uitgaand verkeer op alleen de Azure SQL Database gateway IP toestaan. Met behulp van een instelling van **Proxy** heeft latentie van meer dan een instelling van **omleiden**.
- Als uw verbindingsbeleid voor de tot stand **omleiden**, alle pakketten stroom rechtstreeks naar de proxy middleware netwerkapparaten. Voor deze instelling moet u uitgaand verkeer naar meerdere IP-adressen toestaan.

## <a name="script-to-change-connection-settings-via-powershell"></a>Script verbindingsinstellingen via PowerShell wijzigen

> [!IMPORTANT]
> Dit script vereist de [Azure PowerShell-module](/powershell/azure/install-azurerm-ps).
>

De volgende PowerShell-script laat zien hoe het verbindingsbeleid wijzigen.

```powershell
Add-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName <Subscription Name>

# Azure Active Directory ID
$tenantId = "<Azure Active Directory GUID>"
$authUrl = "https://login.microsoftonline.com/$tenantId"

# Subscription ID
$subscriptionId = "<Subscription GUID>"

# Create an App Registration in Azure Active Directory.  Ensure the application type is set to NATIVE
# Under Required Permissions, add the API:  Windows Azure Service Management API

# Specify the redirect URL for the app registration
$uri = "<NATIVE APP - REDIRECT URI>"

# Specify the application id for the app registration
$clientId = "<NATIVE APP - APPLICATION ID>"

# Logical SQL Server Name
$serverName = "<LOGICAL DATABASE SERVER - NAME>"

# Resource Group where the SQL Server is located
$resourceGroupName= "<LOGICAL DATABASE SERVER - RESOURCE GROUP NAME>"


# Login and acquire a bearer token
$AuthContext = [Microsoft.IdentityModel.Clients.ActiveDirectory.AuthenticationContext]$authUrl
$result = $AuthContext.AcquireToken(
"https://management.core.windows.net/",
$clientId,
[Uri]$uri,
[Microsoft.IdentityModel.Clients.ActiveDirectory.PromptBehavior]::Auto
)

$authHeader = @{
'Content-Type'='application\json; '
'Authorization'=$result.CreateAuthorizationHeader()
}

#Get current connection Policy
Invoke-RestMethod -Uri "https://management.azure.com/subscriptions/$subscriptionId/resourceGroups/$resourceGroupName/providers/Microsoft.Sql/servers/$serverName/connectionPolicies/Default?api-version=2014-04-01-preview" -Method GET -Headers $authHeader

#Set connection policy to Proxy
$connectionType="Proxy" <#Redirect / Default are other options#>
$body = @{properties=@{connectionType=$connectionType}} | ConvertTo-Json

# Apply Changes
Invoke-RestMethod -Uri "https://management.azure.com/subscriptions/$subscriptionId/resourceGroups/$resourceGroupName/providers/Microsoft.Sql/servers/$serverName/connectionPolicies/Default?api-version=2014-04-01-preview" -Method PUT -Headers $authHeader -Body $body -ContentType "application/json"
```

## <a name="script-to-change-connection-settings-via-azure-cli-20"></a>Script verbindingsinstellingen via Azure CLI 2.0 wijzigen

> [!IMPORTANT]
> Dit script vereist de [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).
>

De volgende CLI-script laat zien hoe het verbindingsbeleid wijzigen.

<pre>
# Get SQL Server ID
sqlserverid=$(az sql server show -n <b>sql-server-name</b> -g <b>sql-server-group</b> --query 'id' -o tsv)

# Set URI
id="$sqlserverid/connectionPolicies/Default"

# Get current connection policy 
az resource show --ids $id

# Update connection policy 
az resource update --ids $id --set properties.connectionType=Proxy

</pre>

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over het wijzigen van het beleid van de Azure SQL Database-verbinding voor een Azure SQL Database-server [maken of bijwerken Server verbindingsbeleid met de REST API](https://msdn.microsoft.com/library/azure/mt604439.aspx).
- Zie voor informatie over de werking van Azure SQL Database-verbinding voor clients die gebruikmaken van ADO.NET 4.5 of hoger, [poorten buiten 1433 ADO.NET 4.5](sql-database-develop-direct-route-ports-adonet-v12.md).
- Zie voor informatie over algemene toepassing ontwikkeling overzicht, [SQL Database ontwikkelen-overzicht](sql-database-develop-overview.md).
