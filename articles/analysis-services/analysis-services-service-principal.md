---
title: Azure Analysis Services-taken automatiseren met service-principals | Microsoft Docs
author: minewiskan
manager: kfile
ms.service: analysis-services
ms.topic: conceptual
ms.date: 05/15/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 2b49b85d39f55052e112fd9f4f0e28bdc6c91637
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
---
# <a name="automation-with-service-principals"></a>Automatisering met service-principals

Service-principals zijn de bron van een Azure Active Directory-toepassing u binnen uw tenant om uit te voeren zonder toezicht resource en niveau servicebewerkingen maken. Ze zijn van een uniek type *gebruikersidentiteit* met een toepassings-ID en wachtwoord of certificaat. Een service-principal heeft alleen de machtigingen die nodig zijn voor het uitvoeren van taken die zijn gedefinieerd door de rollen en machtigingen waarvoor deze toegewezen. 

In Analysis Services, service-principals met Azure Automation, PowerShell zonder toezicht, aangepaste clienttoepassingen en web-apps gebruikt om algemene taken te automatiseren. Bijvoorbeeld: inrichting servers, het implementeren van modellen, gegevens vernieuwen, schaal omhoog/omlaag en onderbreken/hervatten kan alle worden geautomatiseerd met behulp van de service-principals. Machtigingen worden toegewezen aan de service-principals via het rollidmaatschap, net als normale UPN van Azure AD-accounts.

## <a name="create-service-principals"></a>Service-principals maken
 
Service-principals kunnen worden gemaakt in de Azure portal of met behulp van PowerShell. Voor meer informatie zie:

[Maken van de service-principal - Azure-portal](../azure-resource-manager/resource-group-create-service-principal-portal.md)   
[Service-principal maken - PowerShell](../azure-resource-manager/resource-group-authenticate-service-principal.md)

## <a name="store-credential-and-certificate-assets-in-azure-automation"></a>Opslaan van referenties en certificaat activa in Azure Automation

Principal-Servicereferenties en certificaten kunnen worden veilig opgeslagen in Azure Automation voor runbook-bewerkingen. Voor meer informatie zie:

[Referentieassets in Azure Automation](../automation/automation-credentials.md)   
[Certificaatassets in Azure Automation](../automation/automation-certificates.md)

## <a name="add-service-principals-to-server-admin-role"></a>Service-principals toevoegen aan de rol admin server

Voordat u een service-principal voor beheerbewerkingen voor Analysis Services-server gebruiken kunt, moet u deze toevoegen aan de server-rol Administrator. Zie voor meer informatie, [een service-principal toevoegen aan de serverbeheerdersrol](analysis-services-addservprinc-admins.md).

## <a name="service-principals-in-connection-strings"></a>Service-principals in verbindingsreeksen

Service-principal appID en wachtwoord of certificaat kan worden gebruikt in verbinding tekenreeksen grotendeels hetzelfde als een UPN.

### <a name="powershell"></a>PowerShell

Wanneer een service-principal wordt gebruikt voor bewerkingen van de resource management met de [AzureRM.AnalysisServices](https://www.powershellgallery.com/packages/AzureRM.AnalysisServices) module, gebruik `Login-AzureRmAccount` cmdlet. Wanneer een service-principal wordt gebruikt voor serverbewerkingen met de [SQLServer](https://www.powershellgallery.com/packages/SqlServer) module, gebruik `Add-AzureAnalysisServicesAccount` cmdlet. 

Toepassings-id en een wachtwoord in het volgende voorbeeld worden gebruikt om een model database vernieuwen uit te voeren:

```PowerShell
Param (

        [Parameter(Mandatory=$true)] [String] $AppId,
        [Parameter(Mandatory=$true)] [String] $PlainPWord,
        [Parameter(Mandatory=$true)] [String] $TenantId
       )
$PWord = ConvertTo-SecureString -String $PlainPWord -AsPlainText -Force

$Credential = New-Object -TypeName "System.Management.Automation.PSCredential" -ArgumentList $AppId, $PWord

Add-AzureAnalysisServicesAccount -Credential $Credential -ServicePrincipal -TenantId $TenantId -RolloutEnvironment "westcentralus.asazure.windows.net"

Invoke-ProcessTable -Server "asazure://westcentralus.asazure.windows.net/myserver" -TableName "MyTable" -Database "MyDb" -RefreshType "Full"
```

### <a name="amo-and-adomd"></a>AMO en ADOMD 

Wanneer u verbinding maakt met clienttoepassingen en web-apps, [AMO en ADOMD clientbibliotheken](analysis-services-data-providers.md) versie 15.0.2 en hogere installeerbare pakketten vanuit NuGet ondersteuning voor service-principals in verbindingsreeksen met de volgende syntaxis: `app:AppID` en het wachtwoord of `cert:thumbprint`. 

In het volgende voorbeeld `appID` en een `password` worden gebruikt om een model database vernieuwen uit te voeren:

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
[Een service-principal toevoegen aan de rol van server](analysis-services-addservprinc-admins.md)   