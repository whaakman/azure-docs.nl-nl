---
title: PowerShell voor DNS-alias Azure SQL | Microsoft Docs
description: PowerShell-cmdlets zoals nieuw AzureRMSqlServerDNSAlias kunt u nieuwe clientverbindingen omleiden naar een andere server in Azure SQL Database, zonder eventuele clientconfiguratie touch.
keywords: DNS-sql-database
services: sql-database
author: MightyPen
manager: craigg
ms.service: sql-database
ms.devlang: PowerShell
ms.topic: conceptual
ms.date: 02/05/2018
ms.reviewer: genemi;amagarwa;maboja
ms.author: dmalik
ms.openlocfilehash: 0353f503ea099b1355b6879efe0748a377115474
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34644311"
---
# <a name="powershell-for-dns-alias-to-azure-sql-database"></a>PowerShell voor DNS-Alias naar Azure SQL Database

Dit artikel bevat een PowerShell-script die u laat zien hoe u een DNS-alias voor Azure SQL Database kunt beheren. Het script wordt uitgevoerd de volgende cmdlets die de volgende acties uitgevoerd:


De cmdlets gebruikt in het voorbeeld zijn de volgende:
- [Nieuwe AzureRMSqlServerDNSAlias](https://docs.microsoft.com/powershell/module/AzureRM.Sql/New-AzureRmSqlServerDnsAlias?view=azurermps-5.1.1): een nieuwe DNS-alias maakt in het systeem van de service Azure SQL Database. De alias die verwijst naar Azure SQL Database-server 1.
- [Get-AzureRMSqlServerDNSAlias](https://docs.microsoft.com/powershell/module/AzureRM.Sql/Get-AzureRmSqlServerDnsAlias?view=azurermps-5.1.1): ophalen en weergeven van alle DNS-aliassen die zijn toegewezen aan SQL-database-server 1.
- [Set-AzureRMSqlServerDNSAlias](https://docs.microsoft.com/powershell/module/AzureRM.Sql/Set-AzureRmSqlServerDnsAlias?view=azurermps-5.1.1): Hiermee wijzigt u de naam van de server die de alias die is geconfigureerd om te verwijzen naar, vanaf server 1 naar 2 van de SQL-database-server.
- [Verwijder AzureRMSqlServerDNSAlias](https://docs.microsoft.com/powershell/module/AzureRM.Sql/Remove-AzureRmSqlServerDnsAlias?view=azurermps-5.1.1): DNS-alias van de SQL-database-server 2, verwijderen met behulp van de naam van de alias.


De voorgaande PowerShell-cmdlets zijn toegevoegd aan de **AzureRm.Sql** module vanaf versie 5.1.1.




#### <a name="dns-alias-in-connection-string"></a>DNS-alias in de verbindingsreeks

Voor een bepaalde Azure SQL Database-server verbinding, krijgt u een client zoals SQL Server Management Studio (SSMS) de naam van de DNS-alias in plaats van de werkelijke servernaam. In het volgende voorbeeld server tekenreeks is de alias *any-unieke-aliasnaam* vervangt het eerste knooppunt van de punt gescheiden van de vier knooppunten server tekenreeks:
- Voorbeeld van een server tekenreeks: `any-unique-alias-name.database.windows.net`.



## <a name="prerequisites"></a>Vereisten

Als u uitvoeren van de demo PowerShell-script is opgegeven in dit artikel wilt, wordt de volgende vereisten gelden:

- Een Azure-abonnement en -account. Voor een gratis proefversie, klikt u op [ https://azure.microsoft.com/free/ ] [ https://azure.microsoft.com/free/].

- Azure PowerShell-module met de cmdlet **nieuw AzureRMSqlServerDNSAlias**.
    - Als u wilt installeren of upgraden, Zie [Installeer Azure PowerShell-module][install-azurerm-ps-84p].
    - Voer `Get-Module -ListAvailable AzureRM;` in powershell\_ise.exe, de versie te vinden.

- Twee Azure SQL Database-servers.

## <a name="code-example"></a>Voorbeeld van code

De volgende PowerShell codevoorbeeld begint met het letterlijke waarden aan verschillende variabelen toewijzen. De code uitvoeren, moet u eerst de tijdelijke aanduiding voor waarden voor de werkelijke waarden vergelijken in uw systeem te bewerken. Of u kunt de code alleen bestuderen. En de uitvoer van de console van de code is ook beschikbaar.


```powershell
################################################################
###    Assign prerequisites.                                 ###
################################################################
cls;

$SubscriptionName             = '<EDIT-your-subscription-name>';
[string]$SubscriptionGuid_Get = '?'; # The script assigns this value, not you.

$SqlServerDnsAliasName = '<EDIT-any-unique-alias-name>';

$1ResourceGroupName = '<EDIT-rg-1>';  # Can be same or different than $2ResourceGroupName.
$1SqlServerName     = '<EDIT-sql-1>'; # Must differ from $2SqlServerName.

$2ResourceGroupName = '<EDIT-rg-2>';
$2SqlServerName     = '<EDIT-sql-2>';

# Login to your Azure subscription, first time per session.
Write-Host "You must log into Azure once per powershell_ise.exe session,";
Write-Host "  thus type 'yes' only the first time.";
Write-Host " ";
$yesno = Read-Host '[yes/no]  Do you need to log into Azure now?';
if ('yes' -eq $yesno)
{
    Connect-AzureRmAccount -SubscriptionName $SubscriptionName;
}

$SubscriptionGuid_Get = Get-AzureRmSubscription `
    -SubscriptionName $SubscriptionName;

################################################################
###    Working with DNS aliasing for Azure SQL DB server.    ###
################################################################

Write-Host '[1] Assign a DNS alias to SQL DB server 1.';
New-AzureRMSqlServerDNSAlias `
    –ResourceGroupName  $1ResourceGroupName `
    -ServerName         $1SqlServerName `
    -ServerDNSAliasName $SqlServerDnsAliasName;

Write-Host '[2] Get and list all the DNS aliases that are assigned to SQL DB server 1.';
Get-AzureRMSqlServerDNSAlias `
    –ResourceGroupName $1ResourceGroupName `
    -ServerName        $1SqlServerName;

Write-Host '[3] Move the DNS alias from 1 to SQL DB server 2.';
Set-AzureRMSqlServerDNSAlias `
    –ResourceGroupName  $2ResourceGroupName `
    -NewServerName      $2SqlServerName `
    -ServerDNSAliasName $SqlServerDnsAliasName `
    -OldServerResourceGroup  $1ResourceGroupName `
    -OldServerName           $1SqlServerName `
    -OldServerSubscriptionId $SubscriptionGuid_Get;

# Here your client, such as SSMS, can connect to your "$2SqlServerName"
# by using "$SqlServerDnsAliasName" in the server name.
# For example, server:  "any-unique-alias-name.database.windows.net".

# Remove-AzureRMSqlServerDNSAlias  - would fail here for SQL DB server 1.

Write-Host '[4] Remove the DNS alias from SQL DB server 2.';
Remove-AzureRMSqlServerDNSAlias `
    –ResourceGroupName  $2ResourceGroupName `
    -ServerName         $2SqlServerName `
    -ServerDNSAliasName $SqlServerDnsAliasName;
```


#### <a name="actual-console-output-from-the-powershell-example"></a>Werkelijke console-uitvoer van het PowerShell-voorbeeld

De volgende console-uitvoer is gekopieerd en geplakt van een werkelijke uitvoeren.


```
You must log into Azure once per powershell_ise.exe session,
  thus type 'yes' only the first time.
 
[yes/no]  Do you need to log into Azure now?: yes


Environment           : AzureCloud
Account               : gm@acorporation.com
TenantId              : 72f988bf-1111-1111-1111-111111111111
SubscriptionId        : 45651c69-2222-2222-2222-222222222222
SubscriptionName      : mysubscriptionname
CurrentStorageAccount : 

 
[1] Assign a DNS alias to SQL DB server 1.
[2] Get the DNS alias that is assigned to SQL DB server 1.
[3] Move the DNS alias from 1 to SQL DB server 2.
[4] Remove the DNS alias from SQL DB server 2.
ResourceGroupName ServerName         ServerDNSAliasName    
----------------- ----------         ------------------    
gm-rg-dns-1       gm-sqldb-dns-1     unique-alias-name-food
gm-rg-dns-1       gm-sqldb-dns-1     unique-alias-name-food
gm-rg-dns-2       gm-sqldb-dns-2     unique-alias-name-food


[C:\windows\system32\]
>> 
```

## <a name="next-steps"></a>Volgende stappen

Zie voor een volledige beschrijving van de functie DNS-Alias voor SQL-Database, [DNS-alias voor Azure SQL Database][dns-alias-overview-37v].



<!-- Article links. -->

[https://azure.microsoft.com/free/]: https://azure.microsoft.com/free/

[install-azurerm-ps-84p]: https://docs.microsoft.com/powershell/azure/install-azurerm-ps

[dns-alias-overview-37v]: dns-alias-overview.md

