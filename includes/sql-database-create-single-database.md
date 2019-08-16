---
author: MashaMSFT
ms.service: sql-database
ms.subservice: single-database
ms.topic: include
ms.date: 06/19/2019
ms.author: mathoma
ms.openlocfilehash: eff121cfaf4473607110de4553a9bb8021990caf
ms.sourcegitcommit: 0e59368513a495af0a93a5b8855fd65ef1c44aac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/15/2019
ms.locfileid: "68444680"
---
In deze stap maakt u de resource groep en een Azure SQL Database afzonderlijke data base. 

> [!IMPORTANT]
> Zorg ervoor dat u firewall regels instelt voor het gebruik van het open bare IP-adres van de computer waarop u de stappen in dit artikel uitvoert. Firewall regels op database niveau worden automatisch gerepliceerd naar de secundaire server.
>
> Zie voor meer informatie [een firewall regel op database niveau maken](/sql/relational-databases/system-stored-procedures/sp-set-database-firewall-rule-azure-sql-database) of om het IP-adres te bepalen dat wordt gebruikt voor de firewall regel op server niveau voor uw computer Zie [een firewall op server niveau maken](../articles/sql-database/sql-database-server-level-firewall-rule.md).  

# <a name="azure-portaltabazure-portal"></a>[Azure-portal](#tab/azure-portal)
Maak uw resource groep en één data base met behulp van de Azure Portal. 

1. Selecteer in de linkerbovenhoek van Azure Portal **Een resource maken**.
2. Selecteer **data bases** en selecteer vervolgens **SQL database** om de pagina **SQL database maken** te openen.

   ![Individuele database maken](../articles/sql-database/media/sql-database-get-started-portal/create-database-1.png)

3. Typ of selecteer de volgende waarden in het gedeelte **Project Details** van het tabblad **basis beginselen** :

   - **Abonnement**: Selecteer het juiste abonnement in de vervolgkeuzelijst als deze niet wordt weergegeven.
   - **Resourcegroep**: Selecteer **nieuwe maken**, type `myResourceGroup`en selecteer **OK**.

     ![Tabblad Nieuw SQL database-basis](../articles/sql-database/media/sql-database-get-started-portal/new-sql-database-basics.png)

4. Typ of selecteer de volgende waarden in de sectie **database Details** :

   - **Databasenaam**: Voer `mySampleDatabase` in.
   - **Server**: Selecteer **nieuwe maken** en voer de volgende waarden in en selecteer **selecteren**.
       - **Servernaam**: Typ `mysqlserver`; en een aantal getallen voor uniekheid.
       - **Aanmeldgegevens van serverbeheerder**: Typ `azureuser`.
       - **Wachtwoord**: Typ een complex wacht woord dat voldoet aan de wachtwoord vereisten.
       - **Locatie**: Kies een locatie in de vervolg keuzelijst, zoals `West US 2`.

         ![Nieuwe server](../articles/sql-database/media/sql-database-get-started-portal/new-server.png)

      > [!IMPORTANT]
      > Vergeet niet de aanmeldgegevens en het wachtwoord van de server te noteren zodat u zich bij de server en databases voor deze en andere quickstarts kunt aanmelden. Als u uw aanmeldgegevens of wachtwoord vergeet, kunt u de aanmeldnaam ophalen of het wachtwoord opnieuw instellen op de pagina **SQL Server**. U kunt de pagina **SQL Server** openen door de servernaam te selecteren op de **Overzichtspagina** van de database nadat u de database hebt gemaakt.

        ![SQL Database Details](../articles/sql-database/media/sql-database-get-started-portal/sql-db-basic-db-details.png)

   - **Elastische SQL-pool wilt gebruiken**: Selecteer de optie **Nee** .
   - **Reken-en opslag**: Selecteer **Data Base configureren** en selecteer voor deze Quick Start **vCore inkoop opties**

     ![Op vCore gebaseerde aanschafopties](../articles/sql-database/media/sql-database-get-started-portal/create-database-vcore.png)

   - Selecteer **ingericht** en **Gen4**.

     ![serverloze Compute-laag](../articles/sql-database/media/sql-database-get-started-portal/create-database-serverless.png)

   - Controleer de instellingen voor **Max vCores**, **min vCores**, **interval voor automatisch onderbreken**en **maximale grootte van gegevens**. Wijzig deze naar wens.
   - Accepteer de voor waarden van de preview-versie en klik op **OK**.
   - Selecteer **Toepassen**.

5. Selecteer het tabblad **extra instellingen** . 
6. Selecteer in de sectie **gegevens bron** onder **bestaande gegevens gebruiken**de optie `Sample`. 

   ![Aanvullende SQL data base-instellingen](../articles/sql-database/media/sql-database-get-started-portal/create-sql-database-additional-settings.png)

   > [!IMPORTANT]
   > Selecteer de gegevens uit **Voorbeeld (AdventureWorksLT)** zodat u deze en andere quickstarts voor Azure SQL Database gemakkelijk kunt volgen waarbij deze gegevens ook worden gebruikt.

7. Laat de rest van de waarden als standaard staan en selecteer onder aan het formulier de optie **controleren + maken** .
8. Controleer de laatste instellingen en selecteer **maken**.

9. Selecteer in het **SQL Database**-formulier de optie **Maken** om de resourcegroep, server en database te implementeren en in te richten.

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)

[!INCLUDE [updated-for-az](updated-for-az.md)]

Maak uw resource groep en één data base met behulp van Power shell. 

   ```powershell-interactive
   # Set variables for your server and database
   $SubscriptionId = '<Your Azure subscription ID>'
   $ResourceGroupName = "myResourceGroup" # to randomize: "myResourceGroup-$(Get-Random)"
   $Location = "westus2"
   $AdminLogin = "azureuser"
   $Password = "ChangeYourAdminPassword1"
   $ServerName = "mysqlserver" # to randomize: "mysqlserver-$(Get-Random)"
   $DatabaseName = "mySampleDatabase"
   
   
   # The ip address range that you want to allow to access your server 
   #(leaving at 0.0.0.0 will prevent outside-of-azure connections to your DB)
   $startIp = "0.0.0.0"
   $endIp = "0.0.0.0"
   
   # Connect to Azure
   Connect-AzAccount

   # Set subscription ID
   Set-AzContext -SubscriptionId $subscriptionId 
   
   # Create a resource group
   $resourceGroup = New-AzResourceGroup -Name $ResourceGroupName -Location $Location
   
   # Create a server with a system wide unique server name
   $server = New-AzSqlServer -ResourceGroupName $ResourceGroupName `
      -ServerName $ServerName `
      -Location $Location `
      -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $AdminLogin, $(ConvertTo-SecureString -String $Password -AsPlainText -Force))
   
   # Create a server firewall rule that allows access from the specified IP range
   $serverFirewallRule = New-AzSqlServerFirewallRule -ResourceGroupName $ResourceGroupName `
      -ServerName $ServerName `
      -FirewallRuleName "AllowedIPs" -StartIpAddress $startIp -EndIpAddress $endIp
   
   # Create General Purpose Gen4 database with 1 vCore
   $database = New-AzSqlDatabase  -ResourceGroupName $ResourceGroupName `
      -ServerName $ServerName `
      -DatabaseName $DatabaseName `
      -Edition GeneralPurpose `
      -VCore 1 `
      -ComputeGeneration Gen4  `
      -MinimumCapacity 1 `
      -SampleName "AdventureWorksLT"
   ```

# <a name="az-clitabbash"></a>[AZ CLI](#tab/bash)
Maak uw resource groep en één data base met behulp van AZ CLI. 


   ```azurecli-interactive
   #!/bin/bash
   # Set variables
   export subscriptionID=<Your Subscription ID>
   export ResourceGroupName=myResourceGroup # to randomize: myResourceGroup-$RANDOM
   export Location=WestUS2
   export AdminLogin=azureuser
   export Password=ChangeYourAdminPassword1
   export ServerName="mysqlserver" # to randomize: mysqlserver-$RANDOM
   export DatabaseName=mySampleDatabase
   
   # The ip address range that you want to allow to access your DB. 
   # (leaving at 0.0.0.0 will prevent outside-of-azure connections to your DB)
   export startip=0.0.0.0
   export endip=0.0.0.0
  
   # Connect to Azure
   az login

   $ Set subscription ID
   az account set --subscription $SubscriptionID
   
   # Create a resource group
   az group create \
      --name $ResourceGroupName \
      --location $Location
   
   # Create a logical server in the resource group
   az sql server create \
      --name $ServerName \
      --resource-group $ResourceGroupName \
      --location $Location  \
      --admin-user $AdminLogin\
      --admin-password $Password
   
   # Configure a firewall rule for the server
   az sql server firewall-rule create \
      --resource-group $ResourceGroupName \
      --server $ServerName \
      -n AllowYourIp \
      --start-ip-address $startip \
      --end-ip-address $endip
   
   # Create a database in the server 
   az sql db create \
      --resource-group $ResourceGroupName \
      --server $ServerName \
      --name $DatabaseName \
      --sample-name AdventureWorksLT \
      --edition GeneralPurpose \
      --family Gen4 \
      --capacity 1 \
   ```

---