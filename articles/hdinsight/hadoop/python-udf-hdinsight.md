---
title: Python-UDF met Apache Hive en Apache Pig - Azure HDInsight
description: Informatie over het gebruik van Python gebruiker gedefinieerde functies (UDF's) van Apache Hive en Apache Pig in HDInsight, de Apache Hadoop-technologiestack in Azure.
services: hdinsight
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.topic: conceptual
ms.date: 03/15/2019
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: adcfb308bbbc8e3de456c4e7a71c543f988db02a
ms.sourcegitcommit: f24fdd1ab23927c73595c960d8a26a74e1d12f5d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/27/2019
ms.locfileid: "58497989"
---
# <a name="use-python-user-defined-functions-udf-with-apache-hive-and-apache-pig-in-hdinsight"></a>Gebruik Python-gebruiker gedefinieerde functies (UDF's) met Apache Hive en Apache Pig in HDInsight

Informatie over het gebruik van Python gebruikersgedefinieerde functies (UDF's) met Apache Hive en Apache Pig in Apache Hadoop op Azure HDInsight.

## <a name="python"></a>Python op HDInsight

Python2.7 is standaard op HDInsight 3.0 en hoger geïnstalleerd. Apache Hive kan worden gebruikt met deze versie van Python voor de verwerking. Verwerking van de Stream maakt gebruik van STDOUT en STDIN om door te geven van gegevens tussen Hive en de UDF.

HDInsight bevat ook Jython, een Python-implementatie die zijn geschreven in Java. Jython rechtstreeks op de virtuele Java-Machine wordt uitgevoerd en maakt geen gebruik van streaming. Jython is de aanbevolen Python-interpreter als Python gebruiken met Pig.

## <a name="prerequisites"></a>Vereisten

* **Een Hadoop-cluster op HDInsight**. Zie [aan de slag met HDInsight op Linux](apache-hadoop-linux-tutorial-get-started.md).
* **Een SSH-client**. Zie voor meer informatie [Verbinding maken met HDInsight (Apache Hadoop) via SSH](../hdinsight-hadoop-linux-use-ssh-unix.md).
* De [URI-schema](../hdinsight-hadoop-linux-information.md#URI-and-scheme) voor uw clusters primaire opslag. Zou dit wasb: / / voor Azure Storage, abfs: / / voor Azure Data Lake Storage Gen2 of adl: / / voor Azure Data Lake Storage Gen1. Als veilige overdracht is ingeschakeld voor Azure Storage of Data Lake Storage Gen2, zou de URI wasbs: / / of abfss: / /, respectievelijk Zie ook [veilige overdracht](../../storage/common/storage-require-secure-transfer.md).
* **Mogelijke wijziging in de opslagconfiguratie.**  Zie [opslagconfiguratie](#storage-configuration) als type opslagaccount `BlobStorage`.
* Optioneel.  Als u van plan het gebruik van PowerShell, moet u de [AZ module](https://docs.microsoft.com/powershell/azure/new-azureps-module-az) geïnstalleerd.

> [!NOTE]  
> Het opslagaccount die wordt gebruikt in dit artikel is Azure Storage met [veilige overdracht](../../storage/common/storage-require-secure-transfer.md) ingeschakeld en dus `wasbs` wordt gebruikt in het artikel.

## <a name="storage-configuration"></a>Opslagconfiguratie
Er is geen actie is vereist als het opslagaccount dat wordt gebruikt van het type `Storage (general purpose v1)` of `StorageV2 (general purpose v2)`.  Het proces in dit artikel wordt uitvoer naar ten minste produceren `/tezstaging`.  Een standaard hadoop-configuratie bevat `/tezstaging` in de `fs.azure.page.blob.dir` configuratievariabele van de in `core-site.xml` voor service `HDFS`.  Deze configuratie zorgt ervoor dat de uitvoer naar de map op de pagina-blobs worden niet ondersteund voor het type opslagaccount worden `BlobStorage`.  Gebruik `BlobStorage` voor dit artikel, verwijdert u `/tezstaging` uit de `fs.azure.page.blob.dir` configuratievariabele.  De configuratie is toegankelijk vanuit de [Ambari UI](../hdinsight-hadoop-manage-ambari.md).  Anders ontvangt u het foutbericht weergegeven: `Page blob is not supported for this account type.`

> [!WARNING]  
> De stappen in dit document moeten u de volgende veronderstellingen:  
>
> * U kunt de Python-scripts maken op uw lokale ontwikkelomgeving.
> * U de scripts uploaden naar HDInsight met behulp van de `scp` opdracht of het opgegeven PowerShell-script.
>
> Als u wilt gebruiken de [Azure Cloud Shell (bash)](https://docs.microsoft.com/azure/cloud-shell/overview) om te werken met HDInsight, moet u:
>
> * De scripts in de cloud shell-omgeving maken.
> * Gebruik `scp` voor het uploaden van de bestanden in de cloudshell naar HDInsight.
> * Gebruik `ssh` vanuit de cloudshell verbinding maken met HDInsight en het uitvoeren van de voorbeelden.

## <a name="hivepython"></a>Apache Hive UDF

Python kan worden gebruikt als een UDF uit Hive via de HiveQL `TRANSFORM` instructie. Bijvoorbeeld, de volgende HiveQL roept de `hiveudf.py` bestand wordt opgeslagen in de standaard Azure Storage-account voor het cluster.

```hiveql
add file wasbs:///hiveudf.py;

SELECT TRANSFORM (clientid, devicemake, devicemodel)
    USING 'python hiveudf.py' AS
    (clientid string, phoneLabel string, phoneHash string)
FROM hivesampletable
ORDER BY clientid LIMIT 50;
```

Dit is wat in dit voorbeeld doet:

1. De `add file` instructie aan het begin van het bestand wordt toegevoegd de `hiveudf.py` bestand naar de gedistribueerde cache, zodat deze beschikbaar voor alle knooppunten in het cluster is.
2. De `SELECT TRANSFORM ... USING` instructie selecteert u gegevens uit de `hivesampletable`. Bovendien worden de clientid, devicemake en devicemodel waarden die moeten worden doorgestuurd de `hiveudf.py` script.
3. De `AS` component beschrijft de velden die zijn geretourneerd door `hiveudf.py`.

<a name="streamingpy"></a>

### <a name="create-file"></a>Bestand maken

Maak een tekstbestand met de naam op uw ontwikkelomgeving `hiveudf.py`. Gebruik de volgende code als de inhoud van het bestand:

```python
#!/usr/bin/env python
import sys
import string
import hashlib

while True:
    line = sys.stdin.readline()
    if not line:
        break

    line = string.strip(line, "\n ")
    clientid, devicemake, devicemodel = string.split(line, "\t")
    phone_label = devicemake + ' ' + devicemodel
    print "\t".join([clientid, phone_label, hashlib.md5(phone_label).hexdigest()])
```

Met dit script worden de volgende acties uitgevoerd:

1. Een line-of-gegevens leest uit STDIN.
2. De afsluitende nieuwe-regelteken wordt verwijderd met behulp van `string.strip(line, "\n ")`.
3. Bij het uitvoeren van de verwerking van stromen, bevat een enkele regel alle waarden met een tabteken tussen elke waarde. Dus `string.split(line, "\t")` kan worden gebruikt voor het splitsen van de invoer op elk tabblad, alleen de velden geretourneerd.
4. Wanneer de verwerking is voltooid, moet de uitvoer worden geschreven naar de STDOUT als één regel, met een tabblad tussen elk veld. Bijvoorbeeld `print "\t".join([clientid, phone_label, hashlib.md5(phone_label).hexdigest()])`.
5. De `while` lus wordt herhaald tot en met Nee `line` wordt gelezen.

De uitvoer van het script is een samenvoeging van de invoerwaarden voor `devicemake` en `devicemodel`, en een hash van de samengevoegde waarde.

### <a name="upload-file-shell"></a>(Shell)-bestand uploaden
Vervang in de onderstaande opdrachten `sshuser` met de werkelijke gebruikersnaam als deze verschilt.  Vervang `mycluster` met de naam van het daadwerkelijke cluster.  Zorg ervoor dat uw werkmap is waar het bestand zich bevindt.

1. Gebruik `scp` de bestanden naar uw HDInsight-cluster te kopiëren. Bewerken en voer de onderstaande opdracht:

    ```cmd
    scp hiveudf.py sshuser@mycluster-ssh.azurehdinsight.net:
    ```

2. Gebruik SSH verbinding maken met het cluster.  Bewerken en voer de onderstaande opdracht:

    ```cmd
    ssh sshuser@mycluster-ssh.azurehdinsight.net
    ```

3. Toevoegen van de SSH-sessie, het python-bestanden die eerder hebt geüpload naar de opslag voor het cluster.

    ```bash
    hdfs dfs -put hiveudf.py /hiveudf.py
    ```

### <a name="use-hive-udf-shell"></a>Gebruik Hive UDF (shell)

1. Als u wilt verbinding maken met Hive, gebruik de volgende opdracht uit uw SSH-sessie openen:

    ```bash
    beeline -u 'jdbc:hive2://headnodehost:10001/;transportMode=http'
    ```

    Met deze opdracht start de client Beeline.

2. Voer de volgende query uit op de `0: jdbc:hive2://headnodehost:10001/>` prompt:

   ```hive
   add file wasbs:///hiveudf.py;
   SELECT TRANSFORM (clientid, devicemake, devicemodel)
       USING 'python hiveudf.py' AS
       (clientid string, phoneLabel string, phoneHash string)
   FROM hivesampletable
   ORDER BY clientid LIMIT 50;
   ```

3. Na het invoeren van de laatste regel, moet de taak starten. Nadat de taak is voltooid, retourneert deze uitvoer die vergelijkbaar is met het volgende voorbeeld:

        100041    RIM 9650    d476f3687700442549a83fac4560c51c
        100041    RIM 9650    d476f3687700442549a83fac4560c51c
        100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9
        100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9
        100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9

4. Voer de volgende opdracht om Beeline:

    ```hive
    !q
    ```

### <a name="upload-file-powershell"></a>Uploaden van bestand (PowerShell)

> [!IMPORTANT]  
> Deze PowerShell-scripts werken niet als [veilige overdracht](../../storage/common/storage-require-secure-transfer.md) is ingeschakeld.  Shell-opdrachten te gebruiken of veilige overdracht uitschakelen.

PowerShell kan ook worden gebruikt om Hive-query's op afstand worden uitgevoerd. Zorg ervoor dat uw werkmap is de locatie waar `hiveudf.py` zich bevindt.  Gebruik de volgende PowerShell-script om uit te voeren van een Hive-query die gebruikmaakt van de `hiveudf.py` script:

```PowerShell
# Login to your Azure subscription
# Is there an active Azure subscription?
$sub = Get-AzSubscription -ErrorAction SilentlyContinue
if(-not($sub))
{
    Connect-AzAccount
}

# Revise file path as needed
$pathToStreamingFile = ".\hiveudf.py"

# Get cluster info
$clusterName = Read-Host -Prompt "Enter the HDInsight cluster name"
$clusterInfo = Get-AzHDInsightCluster -ClusterName $clusterName
$resourceGroup = $clusterInfo.ResourceGroup
$storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
$container=$clusterInfo.DefaultStorageContainer
$storageAccountKey=(Get-AzStorageAccountKey `
   -ResourceGroupName $resourceGroup `
   -Name $storageAccountName)[0].Value

# Create an Azure Storage context
$context = New-AzStorageContext `
    -StorageAccountName $storageAccountName `
    -StorageAccountKey $storageAccountKey

# Upload local files to an Azure Storage blob
Set-AzStorageBlobContent `
    -File $pathToStreamingFile `
    -Blob "hiveudf.py" `
    -Container $container `
    -Context $context
```

> [!NOTE]  
> Zie voor meer informatie over het uploaden van bestanden, de [gegevens uploaden voor Apache Hadoop-taken in HDInsight](../hdinsight-upload-data.md) document.


#### <a name="use-hive-udf"></a>Use-Hive UDF


```PowerShell
# Script should stop on failures
$ErrorActionPreference = "Stop"

# Login to your Azure subscription
# Is there an active Azure subscription?
$sub = Get-AzSubscription -ErrorAction SilentlyContinue
if(-not($sub))
{
    Connect-AzAccount
}

# Get cluster info
$clusterName = Read-Host -Prompt "Enter the HDInsight cluster name"
$creds=Get-Credential -UserName "admin" -Message "Enter the login for the cluster"

$HiveQuery = "add file wasbs:///hiveudf.py;" +
                "SELECT TRANSFORM (clientid, devicemake, devicemodel) " +
                "USING 'python hiveudf.py' AS " +
                "(clientid string, phoneLabel string, phoneHash string) " +
                "FROM hivesampletable " +
                "ORDER BY clientid LIMIT 50;"

# Create Hive job object
$jobDefinition = New-AzHDInsightHiveJobDefinition `
    -Query $HiveQuery

# For status bar updates
$activity="Hive query"

# Progress bar (optional)
Write-Progress -Activity $activity -Status "Starting query..."

# Start defined Azure HDInsight job on specified cluster.
$job = Start-AzHDInsightJob `
    -ClusterName $clusterName `
    -JobDefinition $jobDefinition `
    -HttpCredential $creds

# Progress bar (optional)
Write-Progress -Activity $activity -Status "Waiting on query to complete..."

# Wait for completion or failure of specified job
Wait-AzHDInsightJob `
    -JobId $job.JobId `
    -ClusterName $clusterName `
    -HttpCredential $creds

# Uncomment the following to see stderr output
<#
Get-AzHDInsightJobOutput `
   -Clustername $clusterName `
   -JobId $job.JobId `
   -HttpCredential $creds `
   -DisplayOutputType StandardError
#>

# Progress bar (optional)
Write-Progress -Activity $activity -Status "Retrieving output..."

# Gets the log output
Get-AzHDInsightJobOutput `
    -Clustername $clusterName `
    -JobId $job.JobId `
    -HttpCredential $creds
```

De uitvoer voor de **Hive** taak moet worden weergegeven die vergelijkbaar is met het volgende voorbeeld:

    100041    RIM 9650    d476f3687700442549a83fac4560c51c
    100041    RIM 9650    d476f3687700442549a83fac4560c51c
    100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9
    100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9
    100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9


## <a name="pigpython"></a>Apache Pig UDF

Een Python-script kan worden gebruikt als een UDF uit Pig via de `GENERATE` instructie. U kunt het script met behulp van Jython of C Python kunt uitvoeren.

* Jython wordt uitgevoerd op de JVM. systeemeigen kan worden aangeroepen vanuit Pig.
* C Python is een extern proces, zodat de gegevens van Pig op de JVM is verzonden naar het script dat wordt uitgevoerd in een Python-proces. De uitvoer van het Python-script wordt verzonden naar Pig.

Gebruik het opgeven van de Python-interpreter `register` wanneer u verwijst naar het Python-script. De volgende voorbeelden scripts registreren met Pig als `myfuncs`:

* **Gebruik Jython**: `register '/path/to/pigudf.py' using jython as myfuncs;`
* **Gebruik C Python**: `register '/path/to/pigudf.py' using streaming_python as myfuncs;`

> [!IMPORTANT]  
> Wanneer u Jython, het pad naar het bestand pig_jython mag bestaan uit een lokaal pad of een WASBS: / / pad. Echter, wanneer u C Python, u moet verwijzen naar een bestand op het lokale bestandssysteem van het knooppunt dat u gebruikt voor het verzenden van de Pig-taak.

Eenmaal na registratie, de Pig Latin voor dit voorbeeld is hetzelfde voor beide:

```pig
LOGS = LOAD 'wasbs:///example/data/sample.log' as (LINE:chararray);
LOG = FILTER LOGS by LINE is not null;
DETAILS = FOREACH LOG GENERATE myfuncs.create_structure(LINE);
DUMP DETAILS;
```

Dit is wat in dit voorbeeld doet:

1. De eerste regel wordt het bestand met voorbeeldgegevens geladen `sample.log` in `LOGS`. Het definieert ook elke record als een `chararray`.
2. De volgende regel filtert met een null-waarden opslaan van het resultaat van de bewerking in `LOG`.
3. Vervolgens wordt een iteratie over de records in `LOG` en maakt gebruik van `GENERATE` om aan te roepen de `create_structure` methode die is opgenomen in het script Python/Jython geladen als `myfuncs`. `LINE` wordt gebruikt om de huidige record doorgeven aan de functie.
4. Ten slotte de resultaten die in STDOUT zijn gedumpt met behulp van de `DUMP` opdracht. Met deze opdracht worden de resultaten weergegeven nadat de bewerking is voltooid.

### <a name="create-file"></a>Bestand maken

Maak een tekstbestand met de naam op uw ontwikkelomgeving `pigudf.py`. Gebruik de volgende code als de inhoud van het bestand:

<a name="streamingpy"></a>

```python
# Uncomment the following if using C Python
#from pig_util import outputSchema

@outputSchema("log: {(date:chararray, time:chararray, classname:chararray, level:chararray, detail:chararray)}")
def create_structure(input):
    if (input.startswith('java.lang.Exception')):
        input = input[21:len(input)] + ' - java.lang.Exception'
    date, time, classname, level, detail = input.split(' ', 4)
    return date, time, classname, level, detail
```

In het voorbeeld Pig Latin de `LINE` invoer wordt gedefinieerd als een chararray omdat er geen consistent schema voor de invoer is. Het Python-script transformeert de gegevens naar een consistent schema voor uitvoer.

1. De `@outputSchema` instructie de indeling van de gegevens die wordt geretourneerd naar Pig definieert. In dit geval heeft een **gegevens eigenschappenverzameling**, dit is een Pig-gegevenstype. De eigenschappenverzameling bevatten de volgende velden, die allemaal chararray (tekenreeksen zijn):

   * datum - de datum waarop die de logboekvermelding is gemaakt
   * tijd - de tijd waarop die de logboekvermelding is gemaakt
   * ClassName - de naam van de klasse de vermelding is gemaakt
   * -niveau van het logboek-niveau
   * detail - uitgebreide gegevens voor de logboekvermelding

2. Vervolgens worden de `def create_structure(input)` definieert de functie die Pig regelitems aan wordt doorgegeven.

3. De voorbeeldgegevens `sample.log`, voornamelijk voldoet aan de datum, tijd, classname, niveau, en de details van schema. Het bevat echter een paar regels die met beginnen `*java.lang.Exception*`. Deze regels moeten worden gewijzigd zodat deze overeenkomt met het schema. De `if` instructie wordt gecontroleerd op die vervolgens massages van de ingevoerde gegevens te verplaatsen de `*java.lang.Exception*` tekenreeks aan het einde, waardoor de gegevens in-regel met de verwachte uitvoer-schema.

4. Vervolgens worden de `split` opdracht wordt gebruikt voor het splitsen van de gegevens op de eerste vier spaties. De uitvoer is toegewezen in `date`, `time`, `classname`, `level`, en `detail`.

5. Ten slotte worden de waarden geretourneerd naar Pig.

Wanneer de gegevens wordt geretourneerd naar Pig, heeft een consistent schema zoals gedefinieerd in de `@outputSchema` instructie.



### <a name="upload-file-shell"></a>(Shell)-bestand uploaden

Vervang in de onderstaande opdrachten `sshuser` met de werkelijke gebruikersnaam als deze verschilt.  Vervang `mycluster` met de naam van het daadwerkelijke cluster.  Zorg ervoor dat uw werkmap is waar het bestand zich bevindt.

1. Gebruik `scp` de bestanden naar uw HDInsight-cluster te kopiëren. Bewerken en voer de onderstaande opdracht:

    ```cmd
    scp pigudf.py sshuser@mycluster-ssh.azurehdinsight.net:
    ```

2. Gebruik SSH verbinding maken met het cluster.  Bewerken en voer de onderstaande opdracht:

    ```cmd
    ssh sshuser@mycluster-ssh.azurehdinsight.net
    ```

3. Toevoegen van de SSH-sessie, het python-bestanden die eerder hebt geüpload naar de opslag voor het cluster.

    ```bash
    hdfs dfs -put pigudf.py /pigudf.py
    ```


### <a name="use-pig-udf-shell"></a>Gebruik Pig UDF (shell)

1. Voor verbinding met pig, gebruik de volgende opdracht uit uw SSH-sessie openen:

    ```bash
    pig
    ```

2. Voer de volgende instructies toe aan de `grunt>` prompt:

   ```pig
   Register wasbs:///pigudf.py using jython as myfuncs;
   LOGS = LOAD 'wasb:///example/data/sample.log' as (LINE:chararray);
   LOG = FILTER LOGS by LINE is not null;
   DETAILS = foreach LOG generate myfuncs.create_structure(LINE);
   DUMP DETAILS;
   ```

3. Na het invoeren van de volgende regel, moet de taak starten. Nadat de taak is voltooid, retourneert deze uitvoer die vergelijkbaar is met de volgende gegevens:

        ((2012-02-03,20:11:56,SampleClass5,[TRACE],verbose detail for id 990982084))
        ((2012-02-03,20:11:56,SampleClass7,[TRACE],verbose detail for id 1560323914))
        ((2012-02-03,20:11:56,SampleClass8,[DEBUG],detail for id 2083681507))
        ((2012-02-03,20:11:56,SampleClass3,[TRACE],verbose detail for id 1718828806))
        ((2012-02-03,20:11:56,SampleClass3,[INFO],everything normal for id 530537821))

4. Gebruik `quit` om te sluiten van de Grunt-shell en gebruikt u het volgende om het bestand pigudf.py op het lokale bestandssysteem te bewerken:

    ```bash
    nano pigudf.py
    ```

5. Eenmaal in de editor Verwijder opmerkingen bij de volgende regel door het verwijderen van de `#` tekens vanaf het begin van de regel:

    ```bash
    #from pig_util import outputSchema
    ```

    Deze regel wordt het Python-script om te werken met Python C in plaats van Jython gewijzigd. Zodra de wijzigingen zijn gemaakt, gebruikt u **Ctrl + X** om de editor af te sluiten. Selecteer **Y**, en vervolgens **Enter** de wijzigingen op te slaan.

6. Gebruik de `pig` opdracht de shell opnieuw start. Wanneer u zich bij de `grunt>` wordt gevraagd, gebruikt u het volgende om uit te voeren van het Python-script met behulp van de C-Python-interpreter.

   ```pig
   Register 'pigudf.py' using streaming_python as myfuncs;
   LOGS = LOAD 'wasbs:///example/data/sample.log' as (LINE:chararray);
   LOG = FILTER LOGS by LINE is not null;
   DETAILS = foreach LOG generate myfuncs.create_structure(LINE);
   DUMP DETAILS;
   ```

    Zodra deze taak is voltooid, ziet u hetzelfde resultaat als wanneer u het script met behulp van Jython eerder hebt uitgevoerd.


### <a name="upload-file-powershell"></a>Uploaden van bestand (PowerShell)

> [!IMPORTANT]  
> Deze PowerShell-scripts werken niet als [veilige overdracht](../../storage/common/storage-require-secure-transfer.md) is ingeschakeld.  Shell-opdrachten te gebruiken of veilige overdracht uitschakelen.

PowerShell kan ook worden gebruikt om Hive-query's op afstand worden uitgevoerd. Zorg ervoor dat uw werkmap is de locatie waar `pigudf.py` zich bevindt.  Gebruik de volgende PowerShell-script om uit te voeren van een Hive-query die gebruikmaakt van de `pigudf.py` script:

```PowerShell
# Login to your Azure subscription
# Is there an active Azure subscription?
$sub = Get-AzSubscription -ErrorAction SilentlyContinue
if(-not($sub))
{
    Connect-AzAccount
}

# Revise file path as needed
$pathToJythonFile = ".\pigudf.py"


# Get cluster info
$clusterName = Read-Host -Prompt "Enter the HDInsight cluster name"
$clusterInfo = Get-AzHDInsightCluster -ClusterName $clusterName
$resourceGroup = $clusterInfo.ResourceGroup
$storageAccountName=$clusterInfo.DefaultStorageAccount.split('.')[0]
$container=$clusterInfo.DefaultStorageContainer
$storageAccountKey=(Get-AzStorageAccountKey `
   -ResourceGroupName $resourceGroup `
   -Name $storageAccountName)[0].Value

# Create an Azure Storage context
$context = New-AzStorageContext `
    -StorageAccountName $storageAccountName `
    -StorageAccountKey $storageAccountKey

# Upload local files to an Azure Storage blob
Set-AzStorageBlobContent `
    -File $pathToJythonFile `
    -Blob "pigudf.py" `
    -Container $container `
    -Context $context
```

### <a name="use-pig-udf-powershell"></a>Gebruik Pig UDF (PowerShell)

> [!NOTE]  
> Bij het op afstand indienen van een taak met behulp van PowerShell, is het niet mogelijk Python C gebruiken als de-interpreter.

PowerShell kan ook worden gebruikt Pig Latin-taken uit te voeren. Om uit te voeren een Pig Latin-taak die gebruikmaakt van de `pigudf.py` script, gebruikt u de volgende PowerShell-script:

```PowerShell
# Script should stop on failures
$ErrorActionPreference = "Stop"

# Login to your Azure subscription
# Is there an active Azure subscription?
$sub = Get-AzSubscription -ErrorAction SilentlyContinue
if(-not($sub))
{
    Connect-AzAccount
}

# Get cluster info
$clusterName = Read-Host -Prompt "Enter the HDInsight cluster name"
$creds=Get-Credential -UserName "admin" -Message "Enter the login for the cluster"


$PigQuery = "Register wasbs:///pigudf.py using jython as myfuncs;" +
            "LOGS = LOAD 'wasbs:///example/data/sample.log' as (LINE:chararray);" +
            "LOG = FILTER LOGS by LINE is not null;" +
            "DETAILS = foreach LOG generate myfuncs.create_structure(LINE);" +
            "DUMP DETAILS;"

# Create Pig job object
$jobDefinition = New-AzHDInsightPigJobDefinition -Query $PigQuery

# For status bar updates
$activity="Pig job"

# Progress bar (optional)
Write-Progress -Activity $activity -Status "Starting job..."

# Start defined Azure HDInsight job on specified cluster.
$job = Start-AzHDInsightJob `
    -ClusterName $clusterName `
    -JobDefinition $jobDefinition `
    -HttpCredential $creds

# Progress bar (optional)
Write-Progress -Activity $activity -Status "Waiting for the Pig job to complete..."

# Wait for completion or failure of specified job
Wait-AzHDInsightJob `
    -Job $job.JobId `
    -ClusterName $clusterName `
    -HttpCredential $creds

# Uncomment the following to see stderr output
<#
Get-AzHDInsightJobOutput `
    -Clustername $clusterName `
    -JobId $job.JobId `
    -HttpCredential $creds `
    -DisplayOutputType StandardError
#>

# Progress bar (optional)
Write-Progress -Activity $activity "Retrieving output..."

# Gets the log output
Get-AzHDInsightJobOutput `
    -Clustername $clusterName `
    -JobId $job.JobId `
    -HttpCredential $creds
```

De uitvoer voor de **Pig** taak moet worden weergegeven die vergelijkbaar is met de volgende gegevens:

    ((2012-02-03,20:11:56,SampleClass5,[TRACE],verbose detail for id 990982084))
    ((2012-02-03,20:11:56,SampleClass7,[TRACE],verbose detail for id 1560323914))
    ((2012-02-03,20:11:56,SampleClass8,[DEBUG],detail for id 2083681507))
    ((2012-02-03,20:11:56,SampleClass3,[TRACE],verbose detail for id 1718828806))
    ((2012-02-03,20:11:56,SampleClass3,[INFO],everything normal for id 530537821))

## <a name="troubleshooting"></a>Problemen oplossen

### <a name="errors-when-running-jobs"></a>Fouten bij het uitvoeren van taken

Wanneer de hive-taak wordt uitgevoerd, kunt u een vergelijkbaar met de volgende tekst fout kan optreden:

    Caused by: org.apache.hadoop.hive.ql.metadata.HiveException: [Error 20001]: An error occurred while reading or writing to your custom script. It may have crashed with an error.

Dit probleem kan worden veroorzaakt door de regeleinden in de Python-bestand. Groot aantal Windows-editors maken standaard gebruik CRLF als het einde van regel, maar Linux-toepassingen meestal verwachten LF.

U kunt de volgende PowerShell-instructies gebruiken de CR tekens verwijderen voordat u het bestand te uploaden naar HDInsight:

[!code-powershell[main](../../../powershell_scripts/hdinsight/run-python-udf/run-python-udf.ps1?range=148-150)]

### <a name="powershell-scripts"></a>PowerShell-scripts

Beide van de voorbeeld-PowerShell-scripts gebruikt voor het uitvoeren van de voorbeelden bevatten een opmerkingen regel die de foutuitvoer voor de taak wordt weergegeven. Als u de verwachte uitvoer voor de taak niet ziet, verwijder opmerkingen bij de volgende regel en zien als gegevens van de fout duidt op een probleem.

[!code-powershell[main](../../../powershell_scripts/hdinsight/run-python-udf/run-python-udf.ps1?range=135-139)]

De gegevens van de fout (STDERR) en het resultaat van de taak (STDOUT), worden ook geregistreerd in uw HDInsight-opslag.

| Voor deze taak... | Bekijk deze bestanden in de blob-container |
| --- | --- |
| Hive |/HivePython/stderr<p>/HivePython/stdout |
| Pig |/PigPython/stderr<p>/PigPython/stdout |

## <a name="next"></a>Volgende stappen

Als u laden van Python-modules die niet worden geboden door standaard wilt, Zie [een module implementeren op Azure HDInsight](https://blogs.msdn.com/b/benjguin/archive/2014/03/03/how-to-deploy-a-python-module-to-windows-azure-hdinsight.aspx).

Voor andere manieren om te gebruiken Pig, Hive, en voor meer informatie over het gebruik van MapReduce, Zie de volgende documenten:

* [Apache Hive gebruiken met HDInsight](hdinsight-use-hive.md)
* [Apache Pig gebruiken met HDInsight](hdinsight-use-pig.md)
* [MapReduce gebruiken met HDInsight](hdinsight-use-mapreduce.md)
