---
title: Python UDF met Apache Hive en varkens - Azure HDInsight | Microsoft Docs
description: Informatie over het gebruik van Python gebruiker gedefinieerde functies (UDF) van Hive en Pig in HDInsight, het Hadoop-technologiestack in Azure.
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: c44d6606-28cd-429b-b535-235e8f34a664
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 12/05/2017
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: 002072c8eac37ffb1548b44627ec08e941c96a1d
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2017
---
# <a name="use-python-user-defined-functions-udf-with-hive-and-pig-in-hdinsight"></a>Gebruik Python gebruiker gedefinieerde functies (UDF) met Hive en Pig in HDInsight

Informatie over het gebruik van Python gebruiker gedefinieerde functies (UDF) met Apache Hive en Pig in Hadoop op Azure HDInsight.

## <a name="python"></a>Python in HDInsight

Python2.7 is standaard op HDInsight 3.0 en hoger geïnstalleerd. Apache Hive kan met deze versie van Python gebruikt voor de verwerking van de stroom. Verwerking van de stroom maakt gebruik van STDOUT en STDIN doorgeven van gegevens tussen Hive en de UDF.

HDInsight bevat ook Jython een Python-implementatie die is geschreven in Java. Jython rechtstreeks op de virtuele Java-Machine wordt uitgevoerd en geen streaming gebruikt. Jython is de aanbevolen Python-interpreter bij het gebruik van Python met Pig.

> [!WARNING]
> De stappen in dit document moeten de volgende veronderstellingen: 
>
> * U kunt de Python-scripts maken op uw lokale ontwikkelingsomgeving.
> * U kunt de scripts uploaden naar HDInsight met behulp van de `scp` opdracht vanuit een lokale Bash-sessie of het opgegeven PowerShell-script.
>
> Als u wilt gebruiken de [Azure Cloud-Shell (bash)](https://docs.microsoft.com/azure/cloud-shell/overview) preview werken met HDInsight, moet u:
>
> * De scripts in de cloud shell-omgeving maken.
> * Gebruik `scp` voor het uploaden van bestanden vanuit de cloud-shell uit op HDInsight.
> * Gebruik `ssh` vanuit de cloud-shell verbinding maken met HDInsight en het uitvoeren van de voorbeelden.

## <a name="hivepython"></a>Hive UDF

Python kunnen worden gebruikt als een UDF van Hive via de HiveQL `TRANSFORM` instructie. Bijvoorbeeld de volgende HiveQL roept de `hiveudf.py` bestand is opgeslagen in de Azure Storage-standaardaccount voor het cluster.

**HDInsight op basis van Linux**

```hiveql
add file wasb:///hiveudf.py;

SELECT TRANSFORM (clientid, devicemake, devicemodel)
    USING 'python hiveudf.py' AS
    (clientid string, phoneLable string, phoneHash string)
FROM hivesampletable
ORDER BY clientid LIMIT 50;
```

**HDInsight op basis van Windows**

```hiveql
add file wasb:///hiveudf.py;

SELECT TRANSFORM (clientid, devicemake, devicemodel)
    USING 'D:\Python27\python.exe hiveudf.py' AS
    (clientid string, phoneLable string, phoneHash string)
FROM hivesampletable
ORDER BY clientid LIMIT 50;
```

> [!NOTE]
> Op Windows gebaseerde HDInsight-clusters, de `USING` component moet het volledige pad naar python.exe opgeven.

Dit is wat in dit voorbeeld doet:

1. De `add file` instructie aan het begin van het bestand voegt u de `hiveudf.py` bestand naar de gedistribueerde cache, zodat deze toegankelijk is voor alle knooppunten in het cluster is.
2. De `SELECT TRANSFORM ... USING` -instructie selecteert u gegevens uit de `hivesampletable`. Geeft ook de clientid, devicemake en devicemodel waarden voor de `hiveudf.py` script.
3. De `AS` component beschrijft de velden die zijn geretourneerd door `hiveudf.py`.

<a name="streamingpy"></a>

### <a name="create-the-hiveudfpy-file"></a>Het bestand hiveudf.py maken


Maak een tekstbestand met de naam op uw ontwikkelomgeving `hiveudf.py`. De volgende code gebruiken als de inhoud van het bestand:

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

Dit script voert de volgende handelingen uit:

1. Een line-of gegevens lezen uit STDIN.
2. Het afsluitende teken wordt verwijderd met behulp van `string.strip(line, "\n ")`.
3. Bij het uitvoeren van de stroom verwerken, bevat een enkele regel de waarden met een teken tabblad tussen elke waarde. Dus `string.split(line, "\t")` kan worden gebruikt voor de invoer op elk tabblad retourneert alleen de velden splitsen.
4. Bij het verwerken is voltooid, moet de uitvoer naar STDOUT worden geschreven als één regel, met een tabblad tussen elk veld. Bijvoorbeeld `print "\t".join([clientid, phone_label, hashlib.md5(phone_label).hexdigest()])`.
5. De `while` lus wordt herhaald totdat Nee `line` lezen is.

De uitvoer van het script is een samenvoeging van de invoerwaarden voor `devicemake` en `devicemodel`, en een hash van de samengevoegde waarde.

Zie [uitgevoerd in de voorbeelden](#running) voor het uitvoeren van dit voorbeeld op uw HDInsight-cluster.

## <a name="pigpython"></a>Pig UDF

Een pythonscript kan worden gebruikt als een UDF van Pig via de `GENERATE` instructie. U kunt het script met behulp van Jython of C Python uitvoeren.

* Jython wordt uitgevoerd op de JVM en systeemeigen kan worden aangeroepen vanuit Pig.
* C Python is een extern proces, zodat de gegevens van Pig op de JVM wordt verzonden naar het script uitgevoerd in een Python-proces. De uitvoer van het Python-script wordt verzonden naar Pig.

Gebruik het opgeven van de Python-interpreter `register` bij verwijzingen naar het Python-script. De volgende voorbeelden scripts registreren bij Pig als `myfuncs`:

* **Jython gebruiken**:`register '/path/to/pigudf.py' using jython as myfuncs;`
* **Gebruik C Python**:`register '/path/to/pigudf.py' using streaming_python as myfuncs;`

> [!IMPORTANT]
> Wanneer u Jython, het pad naar het bestand pig_jython mag een lokaal pad of een WASB: / / pad. Echter, wanneer u C Python gebruikt, u moet verwijzen naar een bestand op het lokale bestandssysteem van het knooppunt dat u gebruikt voor het verzenden van de Pig-taak.

Eenmaal uit het verleden registratie, de Pig Latin voor dit voorbeeld is hetzelfde voor beide:

```pig
LOGS = LOAD 'wasb:///example/data/sample.log' as (LINE:chararray);
LOG = FILTER LOGS by LINE is not null;
DETAILS = FOREACH LOG GENERATE myfuncs.create_structure(LINE);
DUMP DETAILS;
```

Dit is wat in dit voorbeeld doet:

1. De eerste regel wordt het voorbeeldgegevensbestand geladen `sample.log` in `LOGS`. Ook wordt gedefinieerd hoe elke record als een `chararray`.
2. De volgende regel gefilterd met een null-waarden voor het opslaan van het resultaat van de bewerking in `LOG`.
3. Vervolgens wordt het via de records in doorloopt `LOG` en maakt gebruik van `GENERATE` aan te roepen de `create_structure` methode opgenomen in het script Python/Jython geladen als `myfuncs`. `LINE`wordt gebruikt voor het doorgeven van de huidige record aan de functie.
4. Ten slotte wordt de uitvoer STDOUT worden gedumpt met behulp van de `DUMP` opdracht. Met deze opdracht worden de resultaten weergegeven nadat de bewerking is voltooid.

### <a name="create-the-pigudfpy-file"></a>Het bestand pigudf.py maken

Maak een tekstbestand met de naam op uw ontwikkelomgeving `pigudf.py`. De volgende code gebruiken als de inhoud van het bestand:

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

In het voorbeeld Pig Latin de `LINE` invoer is gedefinieerd als een chararray omdat er geen consistente schema voor de invoer is. Het Python-script transformeert de gegevens in een consistente schema voor uitvoer.

1. De `@outputSchema` instructie de indeling van de gegevens die wordt geretourneerd naar Pig definieert. In dit geval heeft een **gegevens eigenschappenverzameling**, namelijk een Pig-gegevenstype. De verzameling bevat de volgende velden, dit allemaal chararray (tekenreeksen zijn):

   * datum - de datum waarop die de logboekvermelding is gemaakt
   * tijd - de tijd waarop die de logboekvermelding is gemaakt
   * ClassName - de naam van de klasse de vermelding is gemaakt voor
   * Level - het logboekniveau
   * detail - uitgebreide gegevens op voor de logboekvermelding

2. Vervolgens moet de `def create_structure(input)` definieert de functie waarin Pig regel items die moeten worden doorgegeven.

3. De voorbeeldgegevens, `sample.log`, voornamelijk voldoet aan de datum, tijd, classname, niveau, toegelicht en schema. Het document bevat echter een paar regels die met beginnen `*java.lang.Exception*`. Deze regels moeten worden gewijzigd zodat deze overeenkomt met het schema. De `if` instructie die controleert en vervolgens de invoergegevens verplaatsen massages de `*java.lang.Exception*` tekenreeks aan het einde, de gegevens in-line met het schema van de verwachte uitvoer te brengen.

4. Vervolgens moet de `split` opdracht wordt gebruikt voor het splitsen van de gegevens op de eerste vier spaties. De uitvoer is toegewezen in `date`, `time`, `classname`, `level`, en `detail`.

5. Ten slotte worden de waarden geretourneerd voor Pig.

Wanneer de gegevens worden geretourneerd voor Pig, heeft een consistente schema zoals gedefinieerd in de `@outputSchema` instructie.

## <a name="running"></a>Uploaden en de voorbeelden uitvoeren

> [!IMPORTANT]
> De **SSH** stappen werken alleen met een Linux gebaseerde HDInsight-cluster. De **PowerShell** stappen werken met een Linux- of Windows gebaseerde HDInsight-cluster, maar vereisen dat een Windows-client.

### <a name="ssh"></a>SSH

Zie voor meer informatie over het gebruik van SSH [SSH gebruiken met HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).

1. Gebruik `scp` de bestanden naar uw HDInsight-cluster te kopiëren. Bijvoorbeeld de volgende opdracht worden de bestanden gekopieerd naar een cluster met de naam **mijncluster**.

    ```bash
    scp hiveudf.py pigudf.py myuser@mycluster-ssh.azurehdinsight.net:
    ```

2. SSH gebruiken voor verbinding met het cluster.

    ```bash
    ssh myuser@mycluster-ssh.azurehdinsight.net
    ```

    Zie het document [SSH gebruiken met HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie.

3. Toevoegen van de SSH-sessie de python-bestanden die eerder hebt geüpload naar de opslag WASB voor het cluster.

    ```bash
    hdfs dfs -put hiveudf.py /hiveudf.py
    hdfs dfs -put pigudf.py /pigudf.py
    ```

Gebruik de volgende stappen de Hive en Pig-taken uitvoeren na het uploaden van de bestanden.

#### <a name="use-the-hive-udf"></a>Gebruik de component UDF

1. Voor verbinding met Hive, gebruik de volgende opdracht:

    ```bash
    beeline -u 'jdbc:hive2://headnodehost:10001/;transportMode=http'
    ```

    Deze opdracht start de client Beeline.

2. Voer de volgende query op de `0: jdbc:hive2://headnodehost:10001/>` prompt:

   ```hive
   add file wasb:///hiveudf.py;
   SELECT TRANSFORM (clientid, devicemake, devicemodel)
       USING 'python hiveudf.py' AS
       (clientid string, phoneLabel string, phoneHash string)
   FROM hivesampletable
   ORDER BY clientid LIMIT 50;
   ```

3. De taak na het invoeren van de laatste regel moet worden gestart. Zodra de taak is voltooid, retourneert deze uitvoer lijkt op het volgende voorbeeld:

        100041    RIM 9650    d476f3687700442549a83fac4560c51c
        100041    RIM 9650    d476f3687700442549a83fac4560c51c
        100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9
        100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9
        100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9

4. Gebruik de volgende opdracht Beeline om af te sluiten:

    ```hive
    !q
    ```

#### <a name="use-the-pig-udf"></a>De Pig UDF gebruiken

1. Voor verbinding met pig, gebruik de volgende opdracht:

    ```bash
    pig
    ```

2. Voer de volgende instructies uit op de `grunt>` prompt:

   ```pig
   Register wasb:///pigudf.py using jython as myfuncs;
   LOGS = LOAD 'wasb:///example/data/sample.log' as (LINE:chararray);
   LOG = FILTER LOGS by LINE is not null;
   DETAILS = foreach LOG generate myfuncs.create_structure(LINE);
   DUMP DETAILS;
   ```

3. Na het invoeren van de volgende regel de taak moet worden gestart. Zodra de taak is voltooid, retourneert deze uitvoer vergelijkbaar met de volgende gegevens:

        ((2012-02-03,20:11:56,SampleClass5,[TRACE],verbose detail for id 990982084))
        ((2012-02-03,20:11:56,SampleClass7,[TRACE],verbose detail for id 1560323914))
        ((2012-02-03,20:11:56,SampleClass8,[DEBUG],detail for id 2083681507))
        ((2012-02-03,20:11:56,SampleClass3,[TRACE],verbose detail for id 1718828806))
        ((2012-02-03,20:11:56,SampleClass3,[INFO],everything normal for id 530537821))

4. Gebruik `quit` afsluiten van de shell knorvis en gebruikt u de volgende om het bestand pigudf.py op het lokale bestandssysteem te bewerken:

    ```bash
    nano pigudf.py
    ```

5. Eenmaal in de editor, verwijder de opmerkingen in de volgende regel door het verwijderen van de `#` teken vanaf het begin van de regel:

    ```bash
    #from pig_util import outputSchema
    ```

    Deze regel wordt het script Python werken met C Python in plaats van Jython gewijzigd. Zodra de wijziging is aangebracht, gebruikt u **Ctrl + X** naar de editor te sluiten. Selecteer **Y**, en vervolgens **Enter** de wijzigingen wilt opslaan.

6. Gebruik de `pig` opdracht waarmee de shell opnieuw wordt gestart. Zodra u zich op de `grunt>` gevraagd, gebruikt u de volgende de Python-script met behulp van de C-Python-interpreter uit te voeren.

   ```pig
   Register 'pigudf.py' using streaming_python as myfuncs;
   LOGS = LOAD 'wasb:///example/data/sample.log' as (LINE:chararray);
   LOG = FILTER LOGS by LINE is not null;
   DETAILS = foreach LOG generate myfuncs.create_structure(LINE);
   DUMP DETAILS;
   ```

    Zodra deze taak is voltooid, ziet u hetzelfde resultaat als wanneer u het script met behulp van Jython eerder is uitgevoerd.

### <a name="powershell-upload-the-files"></a>PowerShell: De bestanden uploaden

PowerShell kunt u de bestanden uploaden naar de HDInsight-server. Het volgende script gebruiken om de Python-bestanden te uploaden:

> [!IMPORTANT] 
> De stappen in deze sectie gebruikt Azure PowerShell. Zie voor meer informatie over het gebruik van Azure PowerShell [installeren en configureren van Azure PowerShell](/powershell/azure/overview).

[!code-powershell[main](../../../powershell_scripts/hdinsight/run-python-udf/run-python-udf.ps1?range=5-41)]

> [!IMPORTANT]
> Wijzig de `C:\path\to` waarde naar het pad waar de bestanden op uw ontwikkelomgeving.

Dit script wordt informatie opgehaald voor uw HDInsight-cluster en vervolgens haalt de account en de sleutel voor het standaardopslagaccount en uploadt de bestanden naar de hoofdmap van de container.

> [!NOTE]
> Zie voor meer informatie over het uploaden van bestanden de [gegevens voor Hadoop-taken in HDInsight uploaden](../hdinsight-upload-data.md) document.

#### <a name="powershell-use-the-hive-udf"></a>PowerShell: De Hive UDF gebruiken

PowerShell kan ook worden gebruikt voor het extern uitvoeren van Hive-query's. Gebruik de volgende PowerShell-script uitvoeren van een Hive-query die gebruikmaakt van **hiveudf.py** script:

> [!IMPORTANT]
> Voordat u, het script wordt u gevraagd de accountgegevens/HTTPs-beheerder voor uw HDInsight-cluster.

[!code-powershell[main](../../../powershell_scripts/hdinsight/run-python-udf/run-python-udf.ps1?range=45-94)]

De uitvoer voor de **Hive** taak ziet er ongeveer als volgt uitzien:

    100041    RIM 9650    d476f3687700442549a83fac4560c51c
    100041    RIM 9650    d476f3687700442549a83fac4560c51c
    100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9
    100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9
    100042    Apple iPhone 4.2.x    375ad9a0ddc4351536804f1d5d0ea9b9

#### <a name="pig-jython"></a>Pig (Jython)

PowerShell kan ook worden gebruikt om uit te voeren taken Pig Latin. Uitvoeren van een taak Pig Latin die gebruikmaakt van de **pigudf.py** script, gebruikt u de volgende PowerShell-script:

> [!NOTE]
> Wanneer een taak met PowerShell op afstand wordt verzonden, is het niet mogelijk Python C gebruiken als de interpreter.

[!code-powershell[main](../../../powershell_scripts/hdinsight/run-python-udf/run-python-udf.ps1?range=98-144)]

De uitvoer voor de **Pig** taak lijkt op de volgende gegevens:

    ((2012-02-03,20:11:56,SampleClass5,[TRACE],verbose detail for id 990982084))
    ((2012-02-03,20:11:56,SampleClass7,[TRACE],verbose detail for id 1560323914))
    ((2012-02-03,20:11:56,SampleClass8,[DEBUG],detail for id 2083681507))
    ((2012-02-03,20:11:56,SampleClass3,[TRACE],verbose detail for id 1718828806))
    ((2012-02-03,20:11:56,SampleClass3,[INFO],everything normal for id 530537821))

## <a name="troubleshooting"></a>Problemen oplossen

### <a name="errors-when-running-jobs"></a>Fout bij het uitvoeren van taken

Wanneer de hive-taak wordt uitgevoerd, kunt u een vergelijkbaar met de volgende tekst fout kan optreden:

    Caused by: org.apache.hadoop.hive.ql.metadata.HiveException: [Error 20001]: An error occurred while reading or writing to your custom script. It may have crashed with an error.

Dit probleem kan worden veroorzaakt door de regeleinden in het Python-bestand. Veel editors standaard Windows CRLF meestal gebruiken als het beëindigen van de regel, maar de Linux-toepassingen verwachten LF.

U kunt de volgende PowerShell-instructies gebruiken de CR-tekens te verwijderen voordat u het bestand uploadt naar HDInsight:

[!code-powershell[main](../../../powershell_scripts/hdinsight/run-python-udf/run-python-udf.ps1?range=148-150)]

### <a name="powershell-scripts"></a>PowerShell-scripts

Beide van de voorbeeld-PowerShell-scripts gebruikt voor het uitvoeren van de voorbeelden bevatten een opmerkingen regel die de foutuitvoer voor de taak wordt weergegeven. Als u de verwachte uitvoer voor de taak niet ziet, de volgende regel en zien als de informatie over de fout duidt op een probleem.

[!code-powershell[main](../../../powershell_scripts/hdinsight/run-python-udf/run-python-udf.ps1?range=135-139)]

De informatie over de fout (STDERR) en het resultaat van de taak (STDOUT), worden ook geregistreerd in uw HDInsight-opslag.

| Voor deze taak... | Bekijk deze bestanden in de blob-container |
| --- | --- |
| Hive |/ HivePython/stderr<p>/ HivePython/stdout |
| Pig |/ PigPython/stderr<p>/ PigPython/stdout |

## <a name="next"></a>Volgende stappen

Als u laden Python-modules die standaard zijn niet beschikbaar wilt, Zie [implementeren van een module op Azure HDInsight](http://blogs.msdn.com/b/benjguin/archive/2014/03/03/how-to-deploy-a-python-module-to-windows-azure-hdinsight.aspx).

Varkens voor andere manieren om te gebruiken, Hive, en voor meer informatie over het gebruik van MapReduce, Zie de volgende documenten:

* [Hive gebruiken met HDInsight](hdinsight-use-hive.md)
* [Pig gebruiken met HDInsight](hdinsight-use-pig.md)
* [MapReduce gebruiken met HDInsight](hdinsight-use-mapreduce.md)
