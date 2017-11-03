---
title: XEvent-gebeurtenisbestand code voor de SQL-Database | Microsoft Docs
description: PowerShell en Transact-SQL biedt voor een in twee fasen voorbeeldcode die laat zien van het doel van de gebeurtenisbestand in een uitgebreide gebeurtenis op Azure SQL Database. Azure Storage is een vereist onderdeel van dit scenario.
services: sql-database
documentationcenter: 
author: MightyPen
manager: jhubbard
editor: 
tags: 
ms.assetid: bbb10ecc-739f-4159-b844-12b4be161231
ms.service: sql-database
ms.custom: monitor & tune
ms.workload: Inactive
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/05/2017
ms.author: genemi
ms.openlocfilehash: abf660e3fafd1a5020cdf9a6beb5b73252b72cfc
ms.sourcegitcommit: e5355615d11d69fc8d3101ca97067b3ebb3a45ef
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2017
---
# <a name="event-file-target-code-for-extended-events-in-sql-database"></a>Bestand doel gebeurteniscode voor uitgebreide gebeurtenissen in SQL-Database

[!INCLUDE [sql-database-xevents-selectors-1-include](../../includes/sql-database-xevents-selectors-1-include.md)]

U wilt een compleet codevoorbeeld voor een krachtige manier om te worden vastgelegd en rapport informatie voor een uitgebreide gebeurtenis.

In Microsoft SQL Server, de [gebeurtenisbestand doel](http://msdn.microsoft.com/library/ff878115.aspx) wordt gebruikt voor het opslaan van de uitvoer van de gebeurtenis naar een lokale vaste schijf-bestand. Maar deze bestanden zijn niet beschikbaar voor Azure SQL Database. We gebruiken in plaats daarvan de Azure Storage-service voor de ondersteuning van het doel van de gebeurtenisbestand.

In dit onderwerp wordt een codevoorbeeld in twee fasen:

* PowerShell voor het maken van een Azure Storage-container in de cloud.
* Transact-SQL:
  
  * De Azure Storage-container toewijzen aan een doel voor het bestand van de gebeurtenis.
  * Maken en de event-sessie te starten, enzovoort.

## <a name="prerequisites"></a>Vereisten

* Een Azure-account en -abonnement. U  kunt zich aanmelden voor een [gratis proefversie](https://azure.microsoft.com/pricing/free-trial/). 
* De database die kunt u een tabel in.
  
  * Desgewenst kunt u [maken een **AdventureWorksLT** demonstratiedatabase](sql-database-get-started.md) in minuten.
* SQL Server Management Studio (ssms.exe) in het ideale geval de meest recente maandelijkse updateversie. 
  U kunt de meest recente ssms.exe van downloaden:
  
  * Onderwerp [SQL Server Management Studio downloaden](http://msdn.microsoft.com/library/mt238290.aspx).
  * [Een directe koppeling naar de download.](http://go.microsoft.com/fwlink/?linkid=616025)
* U moet hebben de [Azure PowerShell-modules](http://go.microsoft.com/?linkid=9811175) geïnstalleerd.
  
  * De modules bieden opdrachten zoals - **nieuw AzureStorageAccount**.

## <a name="phase-1-powershell-code-for-azure-storage-container"></a>Fase 1: PowerShell-code voor Azure Storage-container

Deze PowerShell is de fase 1 van de voorbeeldcode van de in twee fasen.

Het script wordt gestart met opdrachten om op te schonen nadat een vorige mogelijk uitgevoerd en is rerunnable.

1. Plak het PowerShell-script in een teksteditor zoals Notepad.exe en sla het script op als een bestand met de extensie **.ps1**.
2. Start PowerShell ISE als beheerder.
3. Typ bij de opdrachtprompt<br/>`Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser`<br/>en druk op Enter.
4. Open in PowerShell ISE uw **.ps1** bestand. Voer het script uit.
5. Het script voor het eerst opstart een nieuw venster waarin u zich bij Azure aanmelden.
   
   * Als u het script opnieuw zonder uw sessie verstoren, hebt u de mogelijkheid handige commentaarteken de **Add-AzureAccount** opdracht.

![PowerShell ISE met Azure-module is geïnstalleerd, klaar script uit te voeren.][30_powershell_ise]

### <a name="powershell-code"></a>PowerShell-code

Deze PowerShell-script wordt ervan uitgegaan dat u de cmdlet Import-Module voor de module AzureRm al hebt uitgevoerd. Zie voor documentatie, [PowerShell-Module Browser](https://docs.microsoft.com/powershell/module/).

```powershell
## TODO: Before running, find all 'TODO' and make each edit!!

cls;

#--------------- 1 -----------------------

'Script assumes you have already logged your PowerShell session into Azure.
But if not, run  Add-AzureRmAccount (or  Login-AzureRmAccount), just one time.';
#Add-AzureRmAccount;   # Same as  Login-AzureRmAccount.

#-------------- 2 ------------------------

'
TODO: Edit the values assigned to these variables, especially the first few!
';

# Ensure the current date is between
# the Expiry and Start time values that you edit here.

$subscriptionName    = 'YOUR_SUBSCRIPTION_NAME';
$resourceGroupName   = 'YOUR_RESOURCE-GROUP-NAME';

$policySasExpiryTime = '2018-08-28T23:44:56Z';
$policySasStartTime  = '2017-10-01';

$storageAccountLocation = 'West US';
$storageAccountName     = 'YOUR_STORAGE_ACCOUNT_NAME';
$contextName            = 'YOUR_CONTEXT_NAME';
$containerName          = 'YOUR_CONTAINER_NAME';
$policySasToken         = ' ? ';

$policySasPermission = 'rwl';  # Leave this value alone, as 'rwl'.

#--------------- 3 -----------------------

# The ending display lists your Azure subscriptions.
# One should match the $subscriptionName value you assigned
#   earlier in this PowerShell script. 

'Choose an existing subscription for the current PowerShell environment.';

Select-AzureRmSubscription -Subscription $subscriptionName;

#-------------- 4 ------------------------

'
Clean up the old Azure Storage Account after any previous run, 
before continuing this new run.';

If ($storageAccountName)
{
    Remove-AzureRmStorageAccount `
        -Name              $storageAccountName `
        -ResourceGroupName $resourceGroupName;
}

#--------------- 5 -----------------------

[System.DateTime]::Now.ToString();

'
Create a storage account. 
This might take several minutes, will beep when ready.
  ...PLEASE WAIT...';

New-AzureRmStorageAccount `
    -Name              $storageAccountName `
    -Location          $storageAccountLocation `
    -ResourceGroupName $resourceGroupName `
    -SkuName           'Standard_LRS';

[System.DateTime]::Now.ToString();
[System.Media.SystemSounds]::Beep.Play();

'
Get the access key for your storage account.
';

$accessKey_ForStorageAccount = `
    (Get-AzureRmStorageAccountKey `
        -Name              $storageAccountName `
        -ResourceGroupName $resourceGroupName
        ).Value[0];

"`$accessKey_ForStorageAccount = $accessKey_ForStorageAccount";

'Azure Storage Account cmdlet completed.
Remainder of PowerShell .ps1 script continues.
';

#--------------- 6 -----------------------

# The context will be needed to create a container within the storage account.

'Create a context object from the storage account and its primary access key.
';

$context = New-AzureStorageContext `
    -StorageAccountName $storageAccountName `
    -StorageAccountKey  $accessKey_ForStorageAccount;

'Create a container within the storage account.
';

$containerObjectInStorageAccount = New-AzureStorageContainer `
    -Name    $containerName `
    -Context $context;

'Create a security policy to be applied to the SAS token.
';

New-AzureStorageContainerStoredAccessPolicy `
    -Container  $containerName `
    -Context    $context `
    -Policy     $policySasToken `
    -Permission $policySasPermission `
    -ExpiryTime $policySasExpiryTime `
    -StartTime  $policySasStartTime;

'
Generate a SAS token for the container.
';
Try
{
    $sasTokenWithPolicy = New-AzureStorageContainerSASToken `
        -Name    $containerName `
        -Context $context `
        -Policy  $policySasToken;
}
Catch 
{
    $Error[0].Exception.ToString();
}

#-------------- 7 ------------------------

'Display the values that YOU must edit into the Transact-SQL script next!:
';

"storageAccountName: $storageAccountName";
"containerName:      $containerName";
"sasTokenWithPolicy: $sasTokenWithPolicy";

'
REMINDER: sasTokenWithPolicy here might start with "?" character, which you must exclude from Transact-SQL.
';

'
(Later, return here to delete your Azure Storage account. See the preceding  Remove-AzureRmStorageAccount -Name $storageAccountName)';

'
Now shift to the Transact-SQL portion of the two-part code sample!';

# EOFile
```


Let op de enkele naamwaarden die het PowerShell-script wordt afgedrukt wanneer deze wordt beëindigd. In de Transact-SQL-script dat volgt op fase 2, moet u deze waarden bewerken.

## <a name="phase-2-transact-sql-code-that-uses-azure-storage-container"></a>Fase 2: Transact-SQL-code die gebruikmaakt van Azure Storage-container

* U hebt een PowerShell-script voor het maken van een Azure Storage-container fase 1 van dit voorbeeld wordt uitgevoerd.
* De volgende Transact-SQL-script moet vervolgens in de fase 2 gebruikt de container.

Het script wordt gestart met opdrachten om op te schonen nadat een vorige mogelijk uitgevoerd en is rerunnable.

Het PowerShell-script afgedrukt enkele naamwaarden geëindigd. U moet de Transact-SQL-script voor het gebruik van deze waarden bewerken. Zoeken naar **TODO** in de Transact-SQL-script de punten vinden.

1. Open SQL Server Management Studio (ssms.exe).
2. Verbinding maken met uw Azure SQL Database-database.
3. Klik om een nieuwe Querydeelvenster te openen.
4. Plak de volgende Transact-SQL-script in het Querydeelvenster.
5. Zoeken naar elke **TODO** in het script en breng de gewenste wijzigingen.
6. Sla op en voer het script.


> [!WARNING]
> De SAS-sleutelwaarde gegenereerd door de voorgaande PowerShell-script kan beginnen met een '?' (vraagteken). Wanneer u de SAS-sleutel in het volgende T-SQL-script gebruikt, moet u *de voorloopspaties verwijderen '?'* . Anders wordt uw inspanningen mogelijk geblokkeerd door security.


### <a name="transact-sql-code"></a>Transact-SQL-code

```sql
---- TODO: First, run the earlier PowerShell portion of this two-part code sample.
---- TODO: Second, find every 'TODO' in this Transact-SQL file, and edit each.

---- Transact-SQL code for Event File target on Azure SQL Database.


SET NOCOUNT ON;
GO


----  Step 1.  Establish one little table, and  ---------
----  insert one row of data.


IF EXISTS
    (SELECT * FROM sys.objects
        WHERE type = 'U' and name = 'gmTabEmployee')
BEGIN
    DROP TABLE gmTabEmployee;
END
GO


CREATE TABLE gmTabEmployee
(
    EmployeeGuid         uniqueIdentifier   not null  default newid()  primary key,
    EmployeeId           int                not null  identity(1,1),
    EmployeeKudosCount   int                not null  default 0,
    EmployeeDescr        nvarchar(256)          null
);
GO


INSERT INTO gmTabEmployee ( EmployeeDescr )
    VALUES ( 'Jane Doe' );
GO


------  Step 2.  Create key, and  ------------
------  Create credential (your Azure Storage container must already exist).


IF NOT EXISTS
    (SELECT * FROM sys.symmetric_keys
        WHERE symmetric_key_id = 101)
BEGIN
    CREATE MASTER KEY ENCRYPTION BY PASSWORD = '0C34C960-6621-4682-A123-C7EA08E3FC46' -- Or any newid().
END
GO


IF EXISTS
    (SELECT * FROM sys.database_scoped_credentials
        -- TODO: Assign AzureStorageAccount name, and the associated Container name.
        WHERE name = 'https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent')
BEGIN
    DROP DATABASE SCOPED CREDENTIAL
        -- TODO: Assign AzureStorageAccount name, and the associated Container name.
        [https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent] ;
END
GO


CREATE
    DATABASE SCOPED
    CREDENTIAL
        -- use '.blob.',   and not '.queue.' or '.table.' etc.
        -- TODO: Assign AzureStorageAccount name, and the associated Container name.
        [https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent]
    WITH
        IDENTITY = 'SHARED ACCESS SIGNATURE',  -- "SAS" token.
        -- TODO: Paste in the long SasToken string here for Secret, but exclude any leading '?'.
        SECRET = 'sv=2014-02-14&sr=c&si=gmpolicysastoken&sig=EjAqjo6Nu5xMLEZEkMkLbeF7TD9v1J8DNB2t8gOKTts%3D'
    ;
GO


------  Step 3.  Create (define) an event session.  --------
------  The event session has an event with an action,
------  and a has a target.

IF EXISTS
    (SELECT * from sys.database_event_sessions
        WHERE name = 'gmeventsessionname240b')
BEGIN
    DROP
        EVENT SESSION
            gmeventsessionname240b
        ON DATABASE;
END
GO


CREATE
    EVENT SESSION
        gmeventsessionname240b
    ON DATABASE

    ADD EVENT
        sqlserver.sql_statement_starting
            (
            ACTION (sqlserver.sql_text)
            WHERE statement LIKE 'UPDATE gmTabEmployee%'
            )
    ADD TARGET
        package0.event_file
            (
            -- TODO: Assign AzureStorageAccount name, and the associated Container name.
            -- Also, tweak the .xel file name at end, if you like.
            SET filename =
                'https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent/anyfilenamexel242b.xel'
            )
    WITH
        (MAX_MEMORY = 10 MB,
        MAX_DISPATCH_LATENCY = 3 SECONDS)
    ;
GO


------  Step 4.  Start the event session.  ----------------
------  Issue the SQL Update statements that will be traced.
------  Then stop the session.

------  Note: If the target fails to attach,
------  the session must be stopped and restarted.

ALTER
    EVENT SESSION
        gmeventsessionname240b
    ON DATABASE
    STATE = START;
GO


SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM gmTabEmployee;

UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 2
    WHERE EmployeeDescr = 'Jane Doe';

UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 13
    WHERE EmployeeDescr = 'Jane Doe';

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM gmTabEmployee;
GO


ALTER
    EVENT SESSION
        gmeventsessionname240b
    ON DATABASE
    STATE = STOP;
GO


-------------- Step 5.  Select the results. ----------

SELECT
        *, 'CLICK_NEXT_CELL_TO_BROWSE_ITS_RESULTS!' as [CLICK_NEXT_CELL_TO_BROWSE_ITS_RESULTS],
        CAST(event_data AS XML) AS [event_data_XML]  -- TODO: In ssms.exe results grid, double-click this cell!
    FROM
        sys.fn_xe_file_target_read_file
            (
                -- TODO: Fill in Storage Account name, and the associated Container name.
                'https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent/anyfilenamexel242b',
                null, null, null
            );
GO


-------------- Step 6.  Clean up. ----------

DROP
    EVENT SESSION
        gmeventsessionname240b
    ON DATABASE;
GO

DROP DATABASE SCOPED CREDENTIAL
    -- TODO: Assign AzureStorageAccount name, and the associated Container name.
    [https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent]
    ;
GO

DROP TABLE gmTabEmployee;
GO

PRINT 'Use PowerShell Remove-AzureStorageAccount to delete your Azure Storage account!';
GO
```


Als het doel niet koppelen als u uitvoert, moet u stoppen en opnieuw starten van de event-sessie:

```sql
ALTER EVENT SESSION ... STATE = STOP;
GO
ALTER EVENT SESSION ... STATE = START;
GO
```


## <a name="output"></a>Uitvoer

Wanneer de Transact-SQL-script is voltooid, klikt u op een cel onder de **event_data_XML** kolomkop. Een  **<event>**  element wordt weergegeven waarin u een UPDATE-instructie.

Hier volgt een  **<event>**  element dat is gegenereerd tijdens het testen:


```xml
<event name="sql_statement_starting" package="sqlserver" timestamp="2015-09-22T19:18:45.420Z">
  <data name="state">
    <value>0</value>
    <text>Normal</text>
  </data>
  <data name="line_number">
    <value>5</value>
  </data>
  <data name="offset">
    <value>148</value>
  </data>
  <data name="offset_end">
    <value>368</value>
  </data>
  <data name="statement">
    <value>UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 2
    WHERE EmployeeDescr = 'Jane Doe'</value>
  </data>
  <action name="sql_text" package="sqlserver">
    <value>

SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM gmTabEmployee;

UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 2
    WHERE EmployeeDescr = 'Jane Doe';

UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 13
    WHERE EmployeeDescr = 'Jane Doe';

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM gmTabEmployee;
</value>
  </action>
</event>
```


De voorgaande Transact-SQL-script gebruikt de volgende systeemfunctie om te lezen van de event_file:

* [sys.fn_xe_file_target_read_file (Transact-SQL)](http://msdn.microsoft.com/library/cc280743.aspx)

Er is een uitleg van de geavanceerde opties voor het bekijken van gegevens van uitgebreide gebeurtenissen beschikbaar op:

* [Geavanceerde van doelgegevens van uitgebreide gebeurtenissen weergeven](http://msdn.microsoft.com/library/mt752502.aspx)


## <a name="converting-the-code-sample-to-run-on-sql-server"></a>Converteren van het voorbeeld uit te voeren op de SQL Server

Stel dat u wilt de voorgaande Transact-SQL-voorbeeld uitvoeren op Microsoft SQL Server.

* Voor de eenvoud, u zou willen volledig gebruik van de Azure Storage-container, zoals met een eenvoudige bestand vervangen **C:\myeventdata.xel**. Het bestand zou worden geschreven naar de lokale vaste schijf van de computer die als host fungeert voor SQL Server.
* U hoeft niet elk soort Transact-SQL-instructies voor **CREATE MASTER KEY** en **referentie maken**.
* In de **maken GEBEURTENIS sessie** instructie in de **doel toevoegen** -component bevat, vervangt u de waarde van de Http toegewezen aangebracht **filename =** met een volledig padtekenreeks zoals **C:\myfile.xel**.
  
  * Er is geen Azure Storage-account moet worden uitgevoerd.

## <a name="more-information"></a>Meer informatie

Zie voor meer informatie over accounts en containers in de Azure Storage-service:

* [Het Blob storage gebruiken met .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md)
* [Naamgeving en verwijzen naar Containers, Blobs en metagegevens](http://msdn.microsoft.com/library/azure/dd135715.aspx)
* [Werken met het Root-Container](http://msdn.microsoft.com/library/azure/ee395424.aspx)
* [Les 1: Een toegangsbeleid opgeslagen en een shared access signature maken op een Azure-container](http://msdn.microsoft.com/library/dn466430.aspx)
  * [Les 2: Een SQL Server-referentie met een shared access signature maken](http://msdn.microsoft.com/library/dn466435.aspx)
* [Uitgebreide gebeurtenissen voor Microsoft SQL Server](https://docs.microsoft.com/sql/relational-databases/extended-events/extended-events)

<!--
Image references.
-->

[30_powershell_ise]: ./media/sql-database-xevent-code-event-file/event-file-powershell-ise-b30.png

