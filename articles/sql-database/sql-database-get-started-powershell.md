<properties
    pageTitle="Nieuwe SQL Database instellen met PowerShell | Microsoft Azure"
    description="Ontdek hoe u een nieuwe SQL Database maakt met PowerShell. U kunt algemene database-instellingstaken beheren met PowerShell-cmdlets."
    keywords="nieuwe sql-database maken,database instellen"
    services="sql-database"
    documentationCenter=""
    authors="stevestein"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.topic="hero-article"
    ms.tgt_pltfrm="powershell"
    ms.workload="data-management"
    ms.date="05/09/2016"
    ms.author="sstein"/>

# Maak een nieuwe SQL Database aan en voer algemene database-instellingstaken uit met PowerShell-cmdlets


> [AZURE.SELECTOR]
- [Azure-portal](sql-database-get-started.md)
- [PowerShell](sql-database-get-started-powershell.md)
- [C#](sql-database-get-started-csharp.md)



Ontdek hoe u een nieuwe SQL-database maakt met PowerShell-cmdlets. (Voor het maken van de elastische databases raadpleegt u [Een nieuwe groep voor elastische database maken met PowerShell](sql-database-elastic-pool-create-powershell.md).)


[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]

## Database instellen: een resourcegroep, server en firewallregel maken

Wanneer u cmdlets kunt uitvoeren bij uw geselecteerde Azure-abonnement, is de volgende stap om de resourcegroep tot stand te brengen waarin zich de server bevindt waarop de database wordt gemaakt. U kunt de volgende opdracht bewerken zodat u welke geldige locatie dan ook kunt gebruiken. Voer **(Get-AzureRmLocation | Where-Object { $_.Providers -eq "Microsoft.Sql" }).Location** uit om een lijst geldige locaties op te halen.

Voer de volgende opdracht uit om een nieuwe resourcegroep te maken:

    New-AzureRmResourceGroup -Name "resourcegroupsqlgsps" -Location "West US"

Nadat u de nieuwe resourcegroep hebt gemaakt, ziet u het volgende: **ProvisioningState: Geslaagd**.


### Een server maken

SQL-databases worden in Azure SQL-databaseservers gemaakt. Voer **New-AzureRmSqlServer** uit om een nieuwe server te maken. Vervang *ServerName* met de naam voor uw server. Deze naam moet uniek zijn voor alle Azure SQL-databaseservers. U krijgt een foutmelding als de servernaam al in gebruik is. Het is ook handig te weten dat deze opdracht enkele minuten in beslag kan nemen. U kunt de opdracht bewerken en elke geldige locatie naar keuze gebruiken, maar u moet dezelfde locatie gebruiken die u hebt gebruikt voor de resourcegroep die u in de vorige stap hebt gemaakt.

    New-AzureRmSqlServer -ResourceGroupName "resourcegroupsqlgsps" -ServerName "server1" -Location "West US" -ServerVersion "12.0"

Wanneer u deze opdracht uitvoert, wordt u gevraagd naar uw gebruikersnaam en wachtwoord. Voer niet uw Azure-referenties in. Voer in plaats hiervan de gebruikersnaam en het wachtwoord in die de beheerdersreferenties worden die u voor de nieuwe server wilt maken.

De servergegevens worden weergegeven nadat de server is gemaakt.

### Een serverfirewallregel configureren om toegang tot de server toe te staan

Stel een firewallregel in voor toegang tot de server. Voer de volgende opdracht uit, maar vervang het IP-beginadres en het IP-eindadres door geldige waarden voor uw computer.

    New-AzureRmSqlServerFirewallRule -ResourceGroupName "resourcegroupsqlgsps" -ServerName "server1" -FirewallRuleName "rule1" -StartIpAddress "192.168.0.0" -EndIpAddress "192.168.0.0"

De firewallregelgegevens worden weergegeven nadat de regel is gemaakt.

Als u wilt dat andere Azure-services toegang hebben tot de server, voegt u een firewallregel toe en stelt u zowel StartIpAddress als EndIpAddress in op 0.0.0.0. Hiermee staat u Azure-verkeer toe tussen Azure-abonnementen en de server.

Zie [Azure SQL Database-firewall](sql-database-firewall-configure.md) voor meer informatie.


## Een nieuwe SQL-database maken

Nu u een resourcegroep, een server en een firewallregel hebt geconfigureerd, kunt u de server benaderen.

Met de volgende opdracht maakt u een nieuwe (lege) SQL-database op het serviceniveau Standard, met prestatieniveau S1:


    New-AzureRmSqlDatabase -ResourceGroupName "resourcegroupsqlgsps" -ServerName "server1" -DatabaseName "database1" -Edition "Standard" -RequestedServiceObjectiveName "S1"


De databasegegevens worden weergegeven nadat de database is gemaakt.

## Een nieuwe SQL-database maken met een PowerShell-script

Hier volgt een nieuwe PowerShell-script voor SQL-databases:

    $SubscriptionId = "4cac86b0-1e56-bbbb-aaaa-000000000000"
    $ResourceGroupName = "resourcegroupname"
    $Location = "Japan West"

    $ServerName = "uniqueservername"

    $FirewallRuleName = "rule1"
    $FirewallStartIP = "192.168.0.0"
    $FirewallEndIp = "192.168.0.0"

    $DatabaseName = "database1"
    $DatabaseEdition = "Standard"
    $DatabasePerfomanceLevel = "S1"


    Add-AzureRmAccount
    Select-AzureRmSubscription -SubscriptionId $SubscriptionId

    $ResourceGroup = New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location

    $Server = New-AzureRmSqlServer -ResourceGroupName $ResourceGroupName -ServerName $ServerName -Location $Location -ServerVersion "12.0"

    $FirewallRule = New-AzureRmSqlServerFirewallRule -ResourceGroupName $ResourceGroupName -ServerName $ServerName -FirewallRuleName $FirewallRuleName -StartIpAddress $FirewallStartIP -EndIpAddress $FirewallEndIp

    $SqlDatabase = New-AzureRmSqlDatabase -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName -Edition $DatabaseEdition -RequestedServiceObjectiveName $DatabasePerfomanceLevel

    $SqlDatabase



## Volgende stappen
Wanneer u een nieuwe SQL-database hebt gemaakt en u de algemene installatietaken voor de database hebt uitgevoerd, bent u klaar voor het volgende:

- [Maak met SQL Server Management Studio verbinding met een SQL-database en voer een T-SQL-voorbeeldquery uit](sql-database-connect-query-ssms.md).


## Aanvullende resources

- [Azure SQL Database](https://azure.microsoft.com/documentation/services/sql-database/)



<!--HONumber=ago16_HO4-->


