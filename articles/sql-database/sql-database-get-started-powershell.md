<properties
    pageTitle="Nieuwe SQL-database instellen met PowerShell | Microsoft Azure"
    description="Ontdek hoe u een SQL-database maakt met PowerShell. U kunt algemene database-instellingstaken beheren met PowerShell-cmdlets."
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
    ms.date="08/19/2016"
    ms.author="sstein"/>


# Maak een SQL-database aan en voer algemene database-instellingstaken uit met PowerShell-cmdlets


> [AZURE.SELECTOR]
- [Azure Portal](sql-database-get-started.md)
- [PowerShell](sql-database-get-started-powershell.md)
- [C#](sql-database-get-started-csharp.md)



Ontdek hoe u een SQL-database maakt met PowerShell-cmdlets. (Voor het maken van de elastische databases raadpleegt u [Een nieuwe pool voor elastische database maken met PowerShell](sql-database-elastic-pool-create-powershell.md).)


[AZURE.INCLUDE [Start your PowerShell session](../../includes/sql-database-powershell.md)]

## Database instellen: een resourcegroep, server en firewallregel maken

Wanneer u cmdlets kunt uitvoeren bij uw geselecteerde Azure-abonnement, is de volgende stap om de resourcegroep tot stand te brengen waarin zich de server bevindt waarop de database wordt gemaakt. U kunt de volgende opdracht bewerken zodat u welke geldige locatie dan ook kunt gebruiken. Voer **(Get-AzureRmLocation | Where-Object { $_.Providers -eq "Microsoft.Sql" }).Location** uit om een lijst geldige locaties op te halen.

Voer de volgende opdracht uit om een resourcegroep te maken:

    New-AzureRmResourceGroup -Name "resourcegroupsqlgsps" -Location "westus"


### Een server maken

SQL-databases worden in Azure SQL-databaseservers gemaakt. Voer **New-AzureRmSqlServer** uit om een server te maken. De naam voor uw server moet uniek zijn voor alle Azure SQL Databaseservers. U krijgt een foutmelding als de servernaam al in gebruik is. Het is ook handig te weten dat deze opdracht enkele minuten in beslag kan nemen. U kunt de opdracht bewerken en elke geldige locatie naar keuze gebruiken, maar u moet dezelfde locatie gebruiken die u hebt gebruikt voor de resourcegroep die u in de vorige stap hebt gemaakt.

    New-AzureRmSqlServer -ResourceGroupName "resourcegroupsqlgsps" -ServerName "server1" -Location "westus" -ServerVersion "12.0"

Wanneer u deze opdracht uitvoert, wordt u gevraagd naar uw gebruikersnaam en wachtwoord. Voer niet uw Azure-referenties in. Voer in plaats daarvan de gebruikersnaam en het wachtwoord in die u wilt maken voor de serverbeheerder. In het script onder aan dit artikel ziet u hoe u de serverreferenties in de code instelt.

De servergegevens worden weergegeven nadat de server is gemaakt.

### Een serverfirewallregel configureren om toegang tot de server toe te staan

Als u toegang wilt tot de server, moet u een firewallregel instellen. Voer de volgende opdracht uit, maar vervang het IP-beginadres en het IP-eindadres door geldige waarden voor uw computer.

    New-AzureRmSqlServerFirewallRule -ResourceGroupName "resourcegroupsqlgsps" -ServerName "server1" -FirewallRuleName "rule1" -StartIpAddress "192.168.0.0" -EndIpAddress "192.168.0.0"

De firewallregelgegevens worden weergegeven nadat de regel is gemaakt.

Als u wilt dat andere Azure-services toegang hebben tot de server, voegt u een firewallregel toe en stelt u zowel StartIpAddress als EndIpAddress in op 0.0.0.0. Met deze regel u Azure-verkeer toe tussen Azure-abonnementen en de server.

Zie [Azure SQL Database-firewall](sql-database-firewall-configure.md) voor meer informatie.


## Een SQL-database maken

Nu u een resourcegroep, een server en een firewallregel hebt geconfigureerd, kunt u de server benaderen.

Met de volgende opdracht maakt u een (lege) SQL-database op het serviceniveau Standard, met prestatieniveau S1:


    New-AzureRmSqlDatabase -ResourceGroupName "resourcegroupsqlgsps" -ServerName "server1" -DatabaseName "database1" -Edition "Standard" -RequestedServiceObjectiveName "S1"


De databasegegevens worden weergegeven nadat de database is gemaakt.

## Een SQL-database maken met een PowerShell-script

Met het volgende PowerShell-script maakt u een SQL-database met alle afhankelijke resources. Vervang alle `{variables}` door waarden die specifiek zijn voor uw abonnement en resources (verwijder de **{}** wanneer u de waarden instelt).

    # Sign in to Azure and set the subscription to work with
    $SubscriptionId = "{subscription-id}"

    Add-AzureRmAccount
    Set-AzureRmContext -SubscriptionId $SubscriptionId

    # CREATE A RESOURCE GROUP
    $resourceGroupName = "{group-name}"
    $rglocation = "{Azure-region}"
    
    New-AzureRmResourceGroup -Name $resourceGroupName -Location $rglocation
    
    # CREATE A SERVER
    $serverName = "{server-name}"
    $serverVersion = "12.0"
    $serverLocation = "{Azure-region}"
    
    $serverAdmin = "{server-admin}"
    $serverPassword = "{server-password}" 
    $securePassword = ConvertTo-SecureString –String $serverPassword –AsPlainText -Force
    $serverCreds = New-Object –TypeName System.Management.Automation.PSCredential –ArgumentList $serverAdmin, $securePassword
    
    $sqlDbServer = New-AzureRmSqlServer -ResourceGroupName $resourceGroupName -ServerName $serverName -Location $serverLocation -ServerVersion $serverVersion -SqlAdministratorCredentials $serverCreds
    
    # CREATE A SERVER FIREWALL RULE
    $ip = (Test-Connection -ComputerName $env:COMPUTERNAME -Count 1 -Verbose).IPV4Address.IPAddressToString
    $firewallRuleName = '{rule-name}'
    $firewallStartIp = $ip
    $firewallEndIp = $ip
    
    $fireWallRule = New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourceGroupName -ServerName $serverName -FirewallRuleName $firewallRuleName -StartIpAddress $firewallStartIp -EndIpAddress $firewallEndIp
    
    
    # CREATE A SQL DATABASE
    $databaseName = "{database-name}"
    $databaseEdition = "{Standard}"
    $databaseSlo = "{S0}"
    
    $sqlDatabase = New-AzureRmSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName -Edition $databaseEdition -RequestedServiceObjectiveName $databaseSlo
    
   
    # REMOVE ALL RESOURCES THE SCRIPT JUST CREATED
    #Remove-AzureRmResourceGroup -Name $resourceGroupName






## Volgende stappen
Nadat u een SQL-database hebt gemaakt en de algemene installatietaken voor de database hebt uitgevoerd, bent u klaar voor het volgende:

- [SQL Database beheren met PowerShell](sql-database-manage-powershell.md)
- [Verbinding maken met SQL Database met SQL Server Management Studio en een voorbeeld-T-SQL-query uitvoeren](sql-database-connect-query-ssms.md)


## Aanvullende resources

- [Azure SQL Database](https://azure.microsoft.com/documentation/services/sql-database/)



<!--HONumber=Sep16_HO5-->


