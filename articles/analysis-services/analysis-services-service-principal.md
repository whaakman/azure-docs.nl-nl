---
title: Azure Analysis Services-taken automatiseren met service-principals | Microsoft Docs
description: Informatie over het maken van service-principals voor het automatiseren van Azure Analysis Services-taken.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 04/23/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: c034ed7164e67183b9a848d5210dcaf377476c6a
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65518164"
---
# <a name="automation-with-service-principals"></a>Automatisering met service-principals

Service-principals zijn een Azure Active Directory-toepassingsresource die u in uw tenant maakt om onbeheerde bewerkingen op resource- en serviceniveau uit te voeren. Ze een uniek soort *gebruikersidentiteit* met een toepassings-ID en wachtwoord of certificaat. Een service-principal heeft alleen de machtigingen die nodig zijn voor het uitvoeren van taken die zijn gedefinieerd door de rollen en machtigingen waarvoor deze toegewezen. 

In Analysis Services, service-principals met Azure Automation, PowerShell installatie zonder toezicht, aangepaste toepassingen en web-apps gebruikt om algemene taken te automatiseren. Bijvoorbeeld, inrichting servers, het implementeren van modellen, het vernieuwen van gegevens, schaal omhoog/omlaag en onderbreken/hervatten kan alle worden geautomatiseerd met behulp van service-principals. Machtigingen zijn toegewezen aan service-principals via rollidmaatschap, net als normale UPN van Azure AD-accounts.

Analyseservices biedt ook ondersteuning voor bewerkingen die worden uitgevoerd op beheerde identiteiten met behulp van service-principals. Zie voor meer informatie, [beheerde identiteiten voor een Azure-resources](../active-directory/managed-identities-azure-resources/overview.md) en [Azure-services die ondersteuning voor Azure AD-verificatie](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md#azure-analysis-services).

## <a name="create-service-principals"></a>Service-principals maken
 
Service-principals kunnen worden gemaakt in Azure portal of met behulp van PowerShell. Voor meer informatie zie:

[Service-principal - Azure portal maken](../active-directory/develop/howto-create-service-principal-portal.md)   
[Service-principal maken - PowerShell](../active-directory/develop/howto-authenticate-service-principal-powershell.md)

## <a name="store-credential-and-certificate-assets-in-azure-automation"></a>Referentie en certificaat activa Store in Azure Automation

Referenties voor service-principal en certificaten kunnen worden veilig opgeslagen in Azure Automation voor runbook-bewerkingen. Voor meer informatie zie:

[Referentieassets in Azure Automation](../automation/automation-credentials.md)   
[Verbindingsassets in Azure Automation](../automation/automation-certificates.md)

## <a name="add-service-principals-to-server-admin-role"></a>Service-principals toevoegen aan de rol admin server

Voordat u een service-principal voor beheerbewerkingen voor Analysis Services-server gebruiken kunt, moet u deze toevoegen aan de beheerdersrol van de server. Zie voor meer informatie, [een service-principal toevoegen aan de serverbeheerdersrol](analysis-services-addservprinc-admins.md).

## <a name="service-principals-in-connection-strings"></a>Service-principals in verbindingsreeksen

App service principal-id en wachtwoord of certificaat kan worden gebruikt in connection strings bijna hetzelfde als een UPN.

### <a name="powershell"></a>PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

#### <a name="a-nameazmodule-using-azanalysisservices-module"></a><a name="azmodule" />Met behulp van de module Az.AnalysisServices

Bij het gebruik van een service-principal voor resource management-bewerkingen met de [Az.AnalysisServices](/powershell/module/az.analysisservices) -module, gebruik `Connect-AzAccount` cmdlet. 

Toepassings-id en het wachtwoord in het volgende voorbeeld worden gebruikt voor het uitvoeren van bewerkingen voor de controlelaag voor synchronisatie met alleen-lezen replica's en schalen omhoog/uit:

```powershell
Param (
        [Parameter(Mandatory=$true)] [String] $AppId,
        [Parameter(Mandatory=$true)] [String] $PlainPWord,
        [Parameter(Mandatory=$true)] [String] $TenantId
       )
$PWord = ConvertTo-SecureString -String $PlainPWord -AsPlainText -Force
$Credential = New-Object -TypeName "System.Management.Automation.PSCredential" -ArgumentList $AppId, $PWord

# Connect using Az module
Connect-AzAccount -Credential $Credential -SubscriptionId "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxx"

# Syncronize a database for query scale out
Sync-AzAnalysisServicesInstance -Instance "asazure://westus.asazure.windows.net/testsvr" -Database "testdb"

# Scale up the server to an S1, set 2 read-only replicas, and remove the primary from the query pool. The new replicas will hydrate from the synchronized data.
Set-AzAnalysisServicesServer -Name "testsvr" -ResourceGroupName "testRG" -Sku "S1" -ReadonlyReplicaCount 2 -DefaultConnectionMode Readonly
```

#### <a name="using-sqlserver-module"></a>Met behulp van SQL Server-module

Toepassings-id en het wachtwoord in het volgende voorbeeld worden gebruikt om uit te voeren van een model-bewerking voor het vernieuwen van database:

```powershell
Param (
        [Parameter(Mandatory=$true)] [String] $AppId,
        [Parameter(Mandatory=$true)] [String] $PlainPWord,
        [Parameter(Mandatory=$true)] [String] $TenantId
       )
$PWord = ConvertTo-SecureString -String $PlainPWord -AsPlainText -Force

$Credential = New-Object -TypeName "System.Management.Automation.PSCredential" -ArgumentList $AppId, $PWord

Invoke-ProcessTable -Server "asazure://westcentralus.asazure.windows.net/myserver" -TableName "MyTable" -Database "MyDb" -RefreshType "Full" -ServicePrincipal -ApplicationId $AppId -TenantId $TenantId -Credential $Credential
```

### <a name="amo-and-adomd"></a>AMO en ADOMD 

Wanneer u verbinding maakt met client-toepassingen en web-apps, [AMO en ADOMD-clientbibliotheken](analysis-services-data-providers.md) versie 15.0.2 en hogere installeerbare pakketten vanuit NuGet ondersteuning voor service-principals in de verbindingsreeksen die gebruikmaken van de volgende syntaxis: `app:AppID` en het wachtwoord of `cert:thumbprint`. 

In het volgende voorbeeld `appID` en een `password` worden gebruikt voor het uitvoeren van een model-bewerking voor het vernieuwen van database:

```C#
string appId = "xxx";
string authKey = "yyy";
string connString = $"Provider=MSOLAP;Data Source=asazure://westus.asazure.windows.net/<servername>;User ID=app:{appId};Password={authKey};";
Server server = new Server();
server.Connect(connString);
Database db = server.Databases.FindByName("adventureworks");
Table tbl = db.Model.Tables.Find("DimDate");
tbl.RequestRefresh(RefreshType.Full);
db.Model.SaveChanges();
```

## <a name="next-steps"></a>Volgende stappen
[Meld u aan met Azure PowerShell](https://docs.microsoft.com/powershell/azure/authenticate-azureps)   
[Een service-principal toevoegen aan de rol van de server-beheerder](analysis-services-addservprinc-admins.md)   
