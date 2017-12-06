---
title: Gebruik C# met Hive en Pig met Hadoop in HDInsight - Azure | Microsoft Docs
description: Informatie over het gebruik van C# gebruiker gedefinieerde functies (UDF) met Hive en Pig streaming in Azure HDInsight.
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: d83def76-12ad-4538-bb8e-3ba3542b7211
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 12/05/2017
ms.author: larryfr
ms.openlocfilehash: 1ad6ba7126b210ddc671026244c4c614d7010000
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2017
---
# <a name="use-c-user-defined-functions-with-hive-and-pig-streaming-on-hadoop-in-hdinsight"></a>C# gebruiker gedefinieerde functies gebruiken met Hive en Pig streaming van Hadoop in HDInsight

Informatie over het gebruik van C# gebruiker gedefinieerde functies (UDF) met Apache Hive en Pig op HDInsight.

> [!IMPORTANT]
> De stappen in dit document werken met op basis van Linux en Windows-HDInsight-clusters. Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie voor meer informatie [versiebeheer van HDInsight-onderdeel](../hdinsight-component-versioning.md).

Beide Hive en Pig gegevens kan doorgeven bij externe toepassingen voor verwerking. Dit proces wordt ook wel _streaming_. Wanneer u een .NET-toepassing, de gegevens op STDIN wordt doorgegeven aan de toepassing en de toepassing de resultaten weer die op STDOUT. U kunt gebruiken om te lezen en schrijven op STDIN en STDOUT, `Console.ReadLine()` en `Console.WriteLine()` vanuit een consoletoepassing.

## <a name="prerequisites"></a>Vereisten

* U moet bekend zijn met schrijven en het bouwen van C#-code die gericht is op .NET Framework 4.5.

    * Ongeacht IDE die u wilt gebruiken. Het is raadzaam [Visual Studio](https://www.visualstudio.com/vs) 2015, 2017, of [Visual Studio Code](https://code.visualstudio.com/). De stappen in dit document gebruikt u Visual Studio 2017.

* Een manier .exe-bestanden uploaden naar het cluster en Pig en Hive-taken uitvoeren. Beter Data Lake Tools voor Visual Studio, Azure PowerShell en Azure CLI. De stappen in dit document Data Lake Tools voor Visual Studio voor het uploaden van de bestanden en het voorbeeld uitvoert Hive-query.

    Hive-query's en Pig-taken, raadpleegt u de volgende documenten voor meer informatie over andere manieren om uit te voeren:

    * [Apache Hive gebruiken met HDInsight](hdinsight-use-hive.md)

    * [Apache Pig gebruiken met HDInsight](hdinsight-use-pig.md)

* Een Hadoop op HDInsight-cluster. Zie voor meer informatie over het maken van een cluster [maken van een HDInsight-cluster](../hdinsight-hadoop-provision-linux-clusters.md).

## <a name="net-on-hdinsight"></a>.NET in HDInsight

* __HDInsight op basis van Linux__ opslagclusters die gebruikmaken van [Mono (https://mono-project.com)](https://mono-project.com) .NET-toepassingen uit te voeren. Mono versie 4.2.1 is opgenomen in HDInsight versie 3.5.

    Zie voor meer informatie over de Mono compatibiliteit met versies van .NET Framework [Mono compatibiliteit](http://www.mono-project.com/docs/about-mono/compatibility/).

    Zie voor het gebruik van een specifieke versie van Mono de [installeren of update Mono](../hdinsight-hadoop-install-mono.md) document.

* __HDInsight op basis van Windows__ clusters gebruiken de Microsoft .NET CLR .NET-toepassingen uit te voeren.

Zie voor meer informatie over de versie van .NET framework en Mono opgenomen met versies van HDInsight [HDInsight onderdeel versies](../hdinsight-component-versioning.md).

## <a name="create-the-c-projects"></a>Maken van de C\# projecten

### <a name="hive-udf"></a>Hive UDF

1. Open Visual Studio en maak een oplossing. Selecteer voor het projecttype **Console-App (.NET Framework)**, en de naam van het nieuwe project **HiveCSharp**.

    > [!IMPORTANT]
    > Selecteer __.NET Framework 4.5__ als u een Linux gebaseerde HDInsight-cluster. Zie voor meer informatie over de Mono compatibiliteit met versies van .NET Framework [Mono compatibiliteit](http://www.mono-project.com/docs/about-mono/compatibility/).

2. Vervang de inhoud van **Program.cs** met de volgende code:

    ```csharp
    using System;
    using System.Security.Cryptography;
    using System.Text;
    using System.Threading.Tasks;

    namespace HiveCSharp
    {
        class Program
        {
            static void Main(string[] args)
            {
                string line;
                // Read stdin in a loop
                while ((line = Console.ReadLine()) != null)
                {
                    // Parse the string, trimming line feeds
                    // and splitting fields at tabs
                    line = line.TrimEnd('\n');
                    string[] field = line.Split('\t');
                    string phoneLabel = field[1] + ' ' + field[2];
                    // Emit new data to stdout, delimited by tabs
                    Console.WriteLine("{0}\t{1}\t{2}", field[0], phoneLabel, GetMD5Hash(phoneLabel));
                }
            }
            /// <summary>
            /// Returns an MD5 hash for the given string
            /// </summary>
            /// <param name="input">string value</param>
            /// <returns>an MD5 hash</returns>
            static string GetMD5Hash(string input)
            {
                // Step 1, calculate MD5 hash from input
                MD5 md5 = System.Security.Cryptography.MD5.Create();
                byte[] inputBytes = System.Text.Encoding.ASCII.GetBytes(input);
                byte[] hash = md5.ComputeHash(inputBytes);

                // Step 2, convert byte array to hex string
                StringBuilder sb = new StringBuilder();
                for (int i = 0; i < hash.Length; i++)
                {
                    sb.Append(hash[i].ToString("x2"));
                }
                return sb.ToString();
            }
        }
    }
    ```

3. Maak het project.

### <a name="pig-udf"></a>Pig UDF

1. Open Visual Studio en maak een oplossing. Selecteer voor het projecttype **consoletoepassing**, en de naam van het nieuwe project **PigUDF**.

2. Vervang de inhoud van de **Program.cs** bestand met de volgende code:

    ```csharp
    using System;

    namespace PigUDF
    {
        class Program
        {
            static void Main(string[] args)
            {
                string line;
                // Read stdin in a loop
                while ((line = Console.ReadLine()) != null)
                {
                    // Fix formatting on lines that begin with an exception
                    if(line.StartsWith("java.lang.Exception"))
                    {
                        // Trim the error info off the beginning and add a note to the end of the line
                        line = line.Remove(0, 21) + " - java.lang.Exception";
                    }
                    // Split the fields apart at tab characters
                    string[] field = line.Split('\t');
                    // Put fields back together for writing
                    Console.WriteLine(String.Join("\t",field));
                }
            }
        }
    }
    ```

    Deze code parseert de regels die zijn verzonden vanaf de Pig en zet de regels die met beginnen `java.lang.Exception`.

3. Sla **Program.cs**, en vervolgens het project te bouwen.

## <a name="upload-to-storage"></a>Uploaden naar de opslag

1. Open in Visual Studio **Server Explorer**.

2. Vouw **Azure** uit en vouw vervolgens **HDInsight** uit.

3. Als u wordt gevraagd, Voer uw referenties in Azure-abonnement en klik vervolgens op **aanmelden**.

4. Vouw het HDInsight-cluster die u wilt deze toepassing te implementeren. Een item met de tekst __(Default Storage Account)__ wordt vermeld.

    ![Server Explorer met de storage-account voor het cluster](./media/apache-hadoop-hive-pig-udf-dotnet-csharp/storage.png)

    * Als dit item kan worden uitgebreid, gebruikt u een __Azure Storage-Account__ als standaard opslag voor het cluster. Als u de bestanden op de standaard-opslag voor het cluster, vouw de vermelding en dubbelklikt u vervolgens op de __(standaardcontainer)__.

    * Als dit item kan niet worden uitgebreid, gebruikt u __Azure Data Lake Store__ als de opslag van de standaard voor het cluster. Als u wilt weergeven van de bestanden op de standaard-opslag voor het cluster, dubbelklikt u op de __(Standaardopslagaccount)__ vermelding.

6. Als u wilt de .exe-bestanden uploaden, moet u een van de volgende methoden gebruiken:

    * Als u een __Azure Storage-Account__, klik op het pictogram uploaden en blader vervolgens naar de **bin\debug** map voor de **HiveCSharp** project. Tot slot selecteert u de **HiveCSharp.exe** -bestand en klik op **Ok**.

        ![pictogram uploaden](./media/apache-hadoop-hive-pig-udf-dotnet-csharp/upload.png)
    
    * Als u __Azure Data Lake Store__, met de rechtermuisknop op een leeg gebied in de lijst en selecteer vervolgens __uploaden__. Tot slot selecteert u de **HiveCSharp.exe** -bestand en klik op **Open**.

    Eenmaal de __HiveCSharp.exe__ upload is voltooid, herhaalt het uploadproces voor de __PigUDF.exe__ bestand.

## <a name="run-a-hive-query"></a>Een Hive-query uitvoeren

1. Open in Visual Studio **Server Explorer**.

2. Vouw **Azure** uit en vouw vervolgens **HDInsight** uit.

3. Met de rechtermuisknop op het cluster dat u hebt geïmplementeerd de **HiveCSharp** naar een toepassing en selecteer vervolgens **een Hive-Query schrijven**.

4. Gebruik de volgende tekst voor de Hive-query:

    ```hiveql
    -- Uncomment the following if you are using Azure Storage
    -- add file wasb:///HiveCSharp.exe;
    -- Uncomment the following if you are using Azure Data Lake Store
    -- add file adl:///HiveCSharp.exe;

    SELECT TRANSFORM (clientid, devicemake, devicemodel)
    USING 'HiveCSharp.exe' AS
    (clientid string, phoneLabel string, phoneHash string)
    FROM hivesampletable
    ORDER BY clientid LIMIT 50;
    ```

    > [!IMPORTANT]
    > Opmerkingen bij de `add file` instructie die overeenkomt met het type standaard opslag gebruikt voor uw cluster.

    Deze query selecteert de `clientid`, `devicemake`, en `devicemodel` velden uit `hivesampletable`, en geeft u de velden aan de toepassing HiveCSharp.exe. De query verwacht dat de toepassing om terug te keren drie velden die worden opgeslagen als `clientid`, `phoneLabel`, en `phoneHash`. De query verwacht ook HiveCSharp.exe vinden in de hoofdmap van de standaard storage-container.

5. Klik op **indienen** voor het verzenden van de taak met de HDInsight-cluster. De **taakoverzicht Hive** venster wordt geopend.

6. Klik op **vernieuwen** vernieuwen van de samenvatting tot **taakstatus** wijzigingen in **voltooid**. Als u wilt weergeven van de taakuitvoer **Taakuitvoer**.

## <a name="run-a-pig-job"></a>Pig-taak uitgevoerd

1. Gebruik een van de volgende methoden om verbinding maken met uw HDInsight-cluster:

    * Als u een __op basis van Linux__ HDInsight-cluster, SSH gebruiken. Bijvoorbeeld `ssh sshuser@mycluster-ssh.azurehdinsight.net`. Zie voor meer informatie [SSH gebruiken withHDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md)
    
    * Als u een __Windows gebaseerde__ HDInsight-cluster, [verbinding maken met het cluster met behulp van extern bureaublad](../hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp)

2. Gebruik een de volgende opdracht om te beginnen de Pig-opdrachtregel:

        pig

    > [!IMPORTANT]
    > Als u een cluster met Windows gebruikt, kunt u de volgende opdrachten gebruiken:
    > ```
    > cd %PIG_HOME%
    > bin\pig
    > ```

    Een `grunt>` prompt wordt weergegeven.

3. Voer de volgende als u wilt uitvoeren van een Pig-taak die gebruikmaakt van de .NET Framework-toepassing:

        DEFINE streamer `PigUDF.exe` CACHE('/PigUDF.exe');
        LOGS = LOAD '/example/data/sample.log' as (LINE:chararray);
        LOG = FILTER LOGS by LINE is not null;
        DETAILS = STREAM LOG through streamer as (col1, col2, col3, col4, col5);
        DUMP DETAILS;

    De `DEFINE` instructie maakt u een alias van `streamer` voor de toepassingen pigudf.exe en `CACHE` geladen uit opslag standaard voor het cluster. Later, `streamer` wordt gebruikt met de `STREAM` operator voor het verwerken van de regels die zijn opgenomen in een logboek en de gegevens worden geretourneerd als een reeks kolommen.

    > [!NOTE]
    > De naam van de toepassing die wordt gebruikt voor streaming moet worden omgeven door de \` (backtick) teken als alias, en ' (enkel aanhalingsteken) gebruikt in combinatie met `SHIP`.

4. De taak na het invoeren van de laatste regel moet worden gestart. Deze retourneert uitvoer vergelijkbaar met de volgende tekst:

        (2012-02-03 20:11:56 SampleClass5 [WARN] problem finding id 1358451042 - java.lang.Exception)
        (2012-02-03 20:11:56 SampleClass5 [DEBUG] detail for id 1976092771)
        (2012-02-03 20:11:56 SampleClass5 [TRACE] verbose detail for id 1317358561)
        (2012-02-03 20:11:56 SampleClass5 [TRACE] verbose detail for id 1737534798)
        (2012-02-03 20:11:56 SampleClass7 [DEBUG] detail for id 1475865947)

## <a name="next-steps"></a>Volgende stappen

In dit document hebt u geleerd hoe een toepassing .NET Framework via Hive en Pig op HDInsight gebruiken. Als u meer informatie over het wilt gebruik van Python met Hive en Pig, Zie [Python gebruiken met Hive en Pig in HDInsight](python-udf-hdinsight.md).

Zie de volgende documenten voor andere manieren om te gebruiken, Pig en Hive, en voor meer informatie over het gebruik van MapReduce:

* [Hive gebruiken met HDInsight](hdinsight-use-hive.md)
* [Pig gebruiken met HDInsight](hdinsight-use-pig.md)
* [MapReduce gebruiken met HDInsight](hdinsight-use-mapreduce.md)
