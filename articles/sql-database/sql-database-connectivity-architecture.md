---
title: Azure SQL-Database connectivity-architectuur | Microsoft Docs
description: Dit document wordt uitgelegd dat de Azure-SQLDB connectiviteitsarchitectuur uit in Azure of van buiten Azure.
services: sql-database
author: DhruvMsft
manager: craigg
ms.service: sql-database
ms.custom: DBs & servers
ms.topic: conceptual
ms.date: 01/24/2018
ms.author: dhruv
ms.openlocfilehash: 6066462d0a7f31698745275c3c6d65c4e09d9cc5
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/19/2018
ms.locfileid: "46364122"
---
# <a name="azure-sql-database-connectivity-architecture"></a>Azure SQL Database Connectivity-architectuur 

Dit artikel worden de architectuur van Azure SQL Database-connectiviteit en wordt uitgelegd hoe de verschillende onderdelen functie voor het verkeer naar uw exemplaar van Azure SQL Database. Onderdelen functie van deze Azure SQL Database-verbinding om te leiden van netwerkverkeer naar de Azure-database met clients die verbinding maken vanuit Azure en met clients die verbinding maakt vanaf buiten Azure. Dit artikel bevat ook voorbeelden van scripts om te wijzigen hoe de verbinding plaatsvindt, evenals de overwegingen met betrekking tot het wijzigen van de standaardinstellingen van de verbinding. 

## <a name="connectivity-architecture"></a>Connectiviteitsarchitectuur

Het volgende diagram biedt een overzicht op hoog niveau van de architectuur van Azure SQL Database-connectiviteit.

![overzicht van netwerkarchitectuur](./media/sql-database-connectivity-architecture/architecture-overview.png)


De volgende stappen wordt beschreven hoe een verbinding met een Azure SQL-database via de Azure SQL Database-software load balancer (SLB) en de gateway van Azure SQL Database tot stand is gebracht.

- Clients in Azure en buiten Azure verbinding met de SLB, die een openbaar IP-adres is en luistert op poort 1433.
- De SLB zorgt ervoor dat verkeer naar de Azure SQL Database-gateway.
- De gateway stuurt het verkeer naar de juiste proxy-middleware.
- De proxy-middleware leidt het verkeer naar de juiste Azure SQL-database.

> [!IMPORTANT]
> Elk van deze onderdelen is denial-of-service (DDoS) beveiliging ingebouwde op het netwerk en de app-laag gedistribueerd.
>

## <a name="connectivity-from-within-azure"></a>Connectiviteit van in Azure

Als u verbinding vanaf in Azure maakt, uw verbindingen hebben een verbindingsbeleid van **omleiden** standaard. Een beleid van **omleiden** betekent dat verbindingen wanneer de TCP-sessie is ingesteld op de Azure SQL-database, de clientsessie wordt vervolgens omgeleid naar de proxy-middleware met een wijziging in de bestemming virtueel IP-adres van die van de Azure De gateway van de SQL-Database met die van de proxy-middleware. Daarna worden alle volgende pakketten stromen rechtstreeks via de proxy-middleware, overslaan van de Azure SQL Database-gateway. Het volgende diagram illustreert dit netwerkverkeer.

![overzicht van netwerkarchitectuur](./media/sql-database-connectivity-architecture/connectivity-from-within-azure.png)

## <a name="connectivity-from-outside-of-azure"></a>De connectiviteit van buiten Azure

Als u verbinding vanaf buiten Azure maakt, uw verbindingen hebben een verbindingsbeleid van **Proxy** standaard. Een beleid van **Proxy** betekent dat de TCP-sessie tot stand is gebracht via de Azure SQL Database-gateway en alle volgende pakketten stromen via de gateway. Het volgende diagram illustreert dit netwerkverkeer.

![overzicht van netwerkarchitectuur](./media/sql-database-connectivity-architecture/connectivity-from-outside-azure.png)

> [!IMPORTANT]
> Wanneer u service-eindpunten met Azure SQL Database is het uw beleid **Proxy** standaard. Als u wilt inschakelen in uw VNet verbinding, moet u uitgaande verbindingen naar de Azure SQL Database-Gateway-IP-adressen die is opgegeven in de onderstaande lijst toestaan. Bij het gebruik van service-eindpunten is het raadzaam het verbindingsbeleid naar wijzigen **omleiden** voor betere prestaties. Als u het verbindingsbeleid naar wijzigt **omleiden** dient niet voldoende zijn voor uitgaand verkeer toestaan op uw NSG aan Azure-SQLDB-gateway IP-adressen die hieronder worden vermeld, moet u toestaan uitgaand naar alle Azure SQLDB IP-adressen. Dit kan worden bewerkstelligd met behulp van de servicetags NSG (Netwerkbeveiligingsgroepen). Zie voor meer informatie, [servicetags](https://docs.microsoft.com/azure/virtual-network/security-overview#service-tags).

## <a name="azure-sql-database-gateway-ip-addresses"></a>Azure SQL Database-gateway-IP-adressen

Als u wilt verbinding maken met een Azure SQL database vanuit on-premises bronnen, die u wilt toestaan dat uitgaand netwerkverkeer naar de Azure SQL Database-gateway voor uw Azure-regio. Uw verbindingen worden alleen via de gateway gaan wanneer u verbinding maakt in de Proxy-modus, dit de standaardinstelling is bij het verbinden van on-premises bronnen.

De volgende tabel geeft een lijst van de primaire en secundaire IP-adressen van de Azure SQL Database-gateway voor alle regio's van gegevens. Voor sommige regio's zijn er twee IP-adressen. In deze regio's, het primaire IP-adres is het huidige IP-adres van de gateway en het tweede IP-adres is een failover-IP-adres. De failover-adres is het adres waarop we uw server te houden van de van hoge servicebeschikbaarheid mogelijk verplaatsen. Voor deze regio's raden wij u uitgaand naar beide IP-adressen toestaat. Het tweede IP-adres is eigendom van Microsoft en luistert niet op alle services totdat deze is geactiveerd door Azure SQL Database om verbindingen te accepteren.

> [!IMPORTANT]
> Als u verbinding vanaf binnen Azure maakt het verbindingsbeleid zich **omleiden** standaard (met uitzondering van de opdracht als u van service-eindpunten gebruikmaakt). Het wordt niet voldoende om toe te staan de volgende IP-adressen zijn. U moet alle Azure SQL Database IP-adressen toestaan. Als u verbinding vanaf binnen een VNet maakt, kan dit worden bewerkstelligd met behulp van de servicetags NSG (Netwerkbeveiligingsgroepen). Zie voor meer informatie, [servicetags](https://docs.microsoft.com/azure/virtual-network/security-overview#service-tags).

| Naam regio | Primaire IP-adres | Secundaire IP-adres |
| --- | --- |--- |
| Australië - oost | 191.238.66.109 | 13.75.149.87 |
| Australië - zuidoost | 191.239.192.109 | 13.73.109.251 |
| Brazilië - zuid | 104.41.11.5 | |
| Canada - midden | 40.85.224.249 | |
| Canada - oost | 40.86.226.166 | |
| US - centraal | 23.99.160.139 | 13.67.215.62 |
| Azië - oost | 191.234.2.139 | 52.175.33.150 |
| VS-Oost 1 | 191.238.6.43 | 40.121.158.30 |
| US - oost 2 | 191.239.224.107 | 40.79.84.180 * |
| India - centraal | 104.211.96.159  | |
| India - zuid | 104.211.224.146  | |
| India - west | 104.211.160.80 | |
| Japan - oost | 191.237.240.43 | 13.78.61.196 |
| Japan - west | 191.238.68.11 | 104.214.148.156 |
| Korea - centraal | 52.231.32.42 | |
| Korea - zuid | 52.231.200.86 |  |
| US - noord-centraal | 23.98.55.75 | 23.96.178.199 |
| Europa - noord | 191.235.193.75 | 40.113.93.91 |
| US - zuid-centraal | 23.98.162.75 | 13.66.62.124 |
| Azië - zuidoost | 23.100.117.95 | 104.43.15.0 |
| VK, noord | 13.87.97.210 | |
| UK-Zuid 1 | 51.140.184.11 | |
| VK, zuid 2 | 13.87.34.7 | |
| Verenigd Koninkrijk West | 51.141.8.11  | |
| US - west-centraal | 13.78.145.25 | |
| Europa -west | 191.237.232.75 | 40.68.37.158 |
| VS-West 1 | 23.99.34.75 | 104.42.238.205 |
| US - west 2 | 13.66.226.202  | |
||||

\* **Opmerking:** *VS-Oost 2* heeft ook een tertiaire IP-adres van `52.167.104.0`.

## <a name="change-azure-sql-database-connection-policy"></a>Azure SQL Database-verbindingsbeleid wijzigen

U kunt het beleid van de Azure SQL Database-verbinding voor een Azure SQL Database-server wijzigen met de [connectiviteit beleid](https://docs.microsoft.com/cli/azure/sql/server/conn-policy) opdracht.

- Als het verbindingsbeleid is ingesteld op **Proxy**, alle netwerkverbindingen stroom van pakketten via de Azure SQL Database-gateway. Voor deze instelling moet u uitgaand verkeer op alleen de Azure SQL Database-gateway-IP toestaan. Met behulp van een instelling van **Proxy** heeft meer latentie dan een instelling van **omleiden**.
- Als het instellen van het verbindingsbeleid **omleiden**, alle pakketten stroom rechtstreeks naar de proxy middleware netwerkverbindingen. Voor deze instelling is moet u uitgaand verkeer naar meerdere IP-adressen toestaan.

## <a name="script-to-change-connection-settings-via-powershell"></a>Script voor het wijzigen van instellingen van de verbinding via PowerShell

> [!IMPORTANT]
> Dit script moet de [Azure PowerShell-module](/powershell/azure/install-azurerm-ps).
>

De volgende PowerShell-script laat zien hoe het verbindingsbeleid wijzigen.

```powershell
Connect-AzureRmAccount
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

## <a name="script-to-change-connection-settings-via-azure-cli-20"></a>Script voor het wijzigen van instellingen van de verbinding via Azure CLI 2.0

> [!IMPORTANT]
> Dit script moet de [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).
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

- Zie voor meer informatie over het wijzigen van het beleid van de Azure SQL Database-verbinding voor een Azure SQL Database-server [connectiviteit beleid](https://docs.microsoft.com/cli/azure/sql/server/conn-policy).
- Zie voor meer informatie over het gedrag van Azure SQL Database-verbindingen voor clients die gebruikmaken van ADO.NET 4.5 of hoger [poorten boven 1433 voor ADO.NET 4.5](sql-database-develop-direct-route-ports-adonet-v12.md).
- Zie voor informatie over algemene ontwikkeling overzicht, [overzicht van ontwikkeling op SQL-databases](sql-database-develop-overview.md).
