---
title: Gebruik C# met Apache Hive en Apache Pig op Apache Hadoop in HDInsight - Azure
description: Meer informatie over het gebruik van C# gebruikersgedefinieerde functies (UDF's) met Apache Hive en Apache Pig, streaming in Azure HDInsight.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 02/15/2019
ms.author: hrasheed
ms.openlocfilehash: 139fe946b013b1c86404bdef6ed06314af349c89
ms.sourcegitcommit: fcb674cc4e43ac5e4583e0098d06af7b398bd9a9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/18/2019
ms.locfileid: "56343133"
---
# <a name="use-c-user-defined-functions-with-apache-hive-and-apache-pig-streaming-on-apache-hadoop-in-hdinsight"></a>Gebruik C# door de gebruiker gedefinieerde functies met Apache Hive en Apache Pig streaming van Apache Hadoop in HDInsight

Meer informatie over het gebruik van C# gebruikersgedefinieerde functies (UDF's) met Apache Hive en Apache Pig in HDInsight.

> [!IMPORTANT]
> De stappen in dit document werken met zowel op basis van Linux en Windows-gebaseerde HDInsight-clusters. Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie voor meer informatie, [versiebeheer van HDInsight-onderdeel](../hdinsight-component-versioning.md).

Beide Hive en Pig gegevens kan worden doorgegeven in externe toepassingen voor verwerking. Dit proces staat bekend als _streaming_. Wanneer u een .NET-toepassing, de gegevens op STDIN wordt doorgegeven aan de toepassing en de toepassing worden de resultaten op STDOUT geretourneerd. U kunt gebruiken om te lezen en schrijven van STDIN en STDOUT, `Console.ReadLine()` en `Console.WriteLine()` vanuit een consoletoepassing.

## <a name="prerequisites"></a>Vereisten

* Een vertrouwd zijn met het schrijven en het bouwen van C#-code die gericht is op .NET Framework 4.5.

    * Welke IDE u wilt gebruiken. Het is raadzaam [Visual Studio](https://www.visualstudio.com/vs) 2015, 2017, of [Visual Studio Code](https://code.visualstudio.com/). De stappen in dit document gebruikt Visual Studio 2017.

* Een manier om het .exe-bestanden uploaden naar het cluster en voer Pig en Hive-taken. We raden het Data Lake Tools voor Visual Studio, Azure PowerShell en klassieke Azure-CLI. De stappen in dit document Data Lake Tools voor Visual Studio om de bestanden uploaden en een voorbeeld van de Hive-query.

    Hive-query's en Pig-taken, raadpleegt u de volgende documenten voor meer informatie over andere manieren om uit te voeren:

    * [Apache Hive gebruiken met HDInsight](hdinsight-use-hive.md)

    * [Apache Pig gebruiken met HDInsight](hdinsight-use-pig.md)

* Een Hadoop op HDInsight-cluster. Zie voor meer informatie over het maken van een cluster [maken van een HDInsight-cluster](../hdinsight-hadoop-provision-linux-clusters.md).

## <a name="net-on-hdinsight"></a>.NET in HDInsight

* __HDInsight op basis van Linux__ -clusters met [Mono (https://mono-project.com) ](https://mono-project.com) .NET-toepassingen uit te voeren. Mono versie 4.2.1 is opgenomen in HDInsight versie 3.6.

    Zie voor meer informatie over de Mono-compatibiliteit met versies van .NET Framework [Mono-compatibiliteit](https://www.mono-project.com/docs/about-mono/compatibility/).

    Voor het gebruik van een specifieke versie van Mono, Zie de [installeren of bijwerken Mono](../hdinsight-hadoop-install-mono.md) document.

* __HDInsight op basis van Windows__ -clusters gebruiken de Microsoft .NET CLR .NET-toepassingen uit te voeren.

Zie voor meer informatie over de versie van het .NET framework en Mono deel uitmaakt van HDInsight-versies [HDInsight onderdeel versies](../hdinsight-component-versioning.md).

## <a name="create-the-c-projects"></a>Maken van de C\# projecten

### <a name="apache-hive-udf"></a>Apache Hive UDF

1. Open Visual Studio en maak een oplossing. Selecteer voor het projecttype **Console-App (.NET Framework)**, en noem het nieuwe project **HiveCSharp**.

    > [!IMPORTANT]
    > Selecteer __.NET Framework 4.5__ als u een Linux gebaseerde HDInsight-cluster. Zie voor meer informatie over de Mono-compatibiliteit met versies van .NET Framework [Mono-compatibiliteit](https://www.mono-project.com/docs/about-mono/compatibility/).

2. Vervang de inhoud van **Program.cs** door de volgende code:

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

### <a name="apache-pig-udf"></a>Apache Pig UDF

1. Open Visual Studio en maak een oplossing. Selecteer voor het projecttype **consoletoepassing**, en noem het nieuwe project **PigUDF**.

2. Vervang de inhoud van de **Program.cs** -bestand met de volgende code:

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

    Deze code parseert de regels die zijn verzonden vanaf Pig en zet de regels die met beginnen `java.lang.Exception`.

3. Sla **Program.cs**, en bouw het project.

## <a name="upload-to-storage"></a>Uploaden naar storage

1. Open in Visual Studio, **Server Explorer**.

2. Vouw **Azure** uit en vouw vervolgens **HDInsight** uit.

3. Als u hierom wordt gevraagd, voert u de referenties van uw Azure-abonnement en klik vervolgens op **aanmelden**.

4. Vouw het HDInsight-cluster dat u wilt deze toepassing te implementeren. Een item met de tekst __(Storage-standaardaccount)__ wordt vermeld.

    ![Server Explorer met de storage-account voor het cluster](./media/apache-hadoop-hive-pig-udf-dotnet-csharp/storage.png)

    * Als dit item kan worden uitgebreid, gebruikt u een __Azure Storage-Account__ als standaardopslag voor het cluster. Als u de bestanden op de standaardopslag voor het cluster, vouw de vermelding en dubbelklik vervolgens op de __(standaardcontainer)__.

    * Als dit item kan niet worden uitgebreid, gebruikt u __Azure Data Lake Storage__ als de standaardopslag voor het cluster. Als u de bestanden op de standaardopslag voor het cluster, dubbelklikt u op de __(Standaardopslagaccount)__ vermelding.

6. Als u wilt het .exe-bestanden uploaden, moet u een van de volgende methoden gebruiken:

    * Als een __Azure Storage-Account__, klik op het uploadpictogram en blader vervolgens naar de **bin\debug** map voor de **HiveCSharp** project. Selecteer ten slotte de **HiveCSharp.exe** -bestand en klik op **Ok**.

        ![pictogram uploaden](./media/apache-hadoop-hive-pig-udf-dotnet-csharp/upload.png)
    
    * Als u __Azure Data Lake Storage__, met de rechtermuisknop op een leeg gebied in de lijst en selecteer vervolgens __uploaden__. Selecteer ten slotte de **HiveCSharp.exe** -bestand en klik op **Open**.

    Zodra de __HiveCSharp.exe__ uploaden is voltooid, herhaalt u het uploadproces voor de __PigUDF.exe__ bestand.

## <a name="run-an-apache-hive-query"></a>Een Apache Hive-query uitvoeren

1. Open in Visual Studio, **Server Explorer**.

2. Vouw **Azure** uit en vouw vervolgens **HDInsight** uit.

3. Met de rechtermuisknop op het cluster dat u hebt geïmplementeerd de **HiveCSharp** toepassing aan en selecteer vervolgens **een Hive-Query schrijven**.

4. Gebruik de volgende tekst voor de Hive-query:

    ```hiveql
    -- Uncomment the following if you are using Azure Storage
    -- add file wasb:///HiveCSharp.exe;
    -- Uncomment the following if you are using Azure Data Lake Storage Gen1
    -- add file adl:///HiveCSharp.exe;
    -- Uncomment the following if you are using Azure Data Lake Storage Gen2
    -- add file abfs:///HiveCSharp.exe;

    SELECT TRANSFORM (clientid, devicemake, devicemodel)
    USING 'HiveCSharp.exe' AS
    (clientid string, phoneLabel string, phoneHash string)
    FROM hivesampletable
    ORDER BY clientid LIMIT 50;
    ```

    > [!IMPORTANT]
    > Verwijder de opmerking bij de `add file` -instructie die overeenkomt met het type standaardopslag die wordt gebruikt voor uw cluster.

    Deze query selecteert de `clientid`, `devicemake`, en `devicemodel` velden uit `hivesampletable`, en geeft u de velden aan de toepassing HiveCSharp.exe. De query wordt verwacht dat de toepassing om terug te keren drie velden die worden opgeslagen als `clientid`, `phoneLabel`, en `phoneHash`. De query verwacht ook HiveCSharp.exe vinden in de hoofdmap van de standaard-storage-container.

5. Klik op **indienen** om de taak voor het HDInsight-cluster te verzenden. De **samenvatting van Hive-taak** venster wordt geopend.

6. Klik op **vernieuwen** vernieuwen van de samenvatting tot **taakstatus** wordt gewijzigd in **voltooid**. Als u wilt de taakuitvoer weergeven, klikt u op **Taakuitvoer**.

## <a name="run-an-apache-pig-job"></a>Een Apache Pig-taak uitvoeren

1. Gebruik een van de volgende methoden voor het verbinding maken met uw HDInsight-cluster:

    * Als u een __op basis van Linux__ HDInsight-cluster, gebruikmaken van SSH. Bijvoorbeeld `ssh sshuser@mycluster-ssh.azurehdinsight.net`. Zie voor meer informatie, [SSH gebruiken withHDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md)
    
    * Als u een __op basis van Windows__ HDInsight-cluster, [verbinding maken met het cluster met behulp van extern bureaublad](../hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp)

2. Gebruik een de volgende opdracht uit om te beginnen de Pig-opdrachtregel:

        pig

    > [!IMPORTANT]
    > Als u een cluster op basis van Windows gebruikt, in plaats daarvan de volgende opdrachten gebruiken:
    > ```
    > cd %PIG_HOME%
    > bin\pig
    > ```

    Een `grunt>` prompt wordt weergegeven.

3. Voer de volgende voor het uitvoeren van een Pig-taak die gebruikmaakt van de .NET Framework-toepassing:

        DEFINE streamer `PigUDF.exe` CACHE('/PigUDF.exe');
        LOGS = LOAD '/example/data/sample.log' as (LINE:chararray);
        LOG = FILTER LOGS by LINE is not null;
        DETAILS = STREAM LOG through streamer as (col1, col2, col3, col4, col5);
        DUMP DETAILS;

    De `DEFINE` instructie maakt u een alias van `streamer` voor de toepassingen pigudf.exe en `CACHE` geladen vanuit de standaardopslag voor het cluster. Later, `streamer` wordt gebruikt met de `STREAM` operator voor het verwerken van de regels die zijn opgenomen in het logboek en de gegevens als een reeks kolommen retourneren.

    > [!NOTE]
    > De naam van de toepassing die wordt gebruikt voor het streamen van moet tussen de \` (backtick) teken als alias, en ' (enkel aanhalingsteken) gebruikt in combinatie met `SHIP`.

4. Na het invoeren van de laatste regel, moet de taak starten. Deze uitvoer die lijkt op de volgende tekst:

        (2012-02-03 20:11:56 SampleClass5 [WARN] problem finding id 1358451042 - java.lang.Exception)
        (2012-02-03 20:11:56 SampleClass5 [DEBUG] detail for id 1976092771)
        (2012-02-03 20:11:56 SampleClass5 [TRACE] verbose detail for id 1317358561)
        (2012-02-03 20:11:56 SampleClass5 [TRACE] verbose detail for id 1737534798)
        (2012-02-03 20:11:56 SampleClass7 [DEBUG] detail for id 1475865947)

## <a name="next-steps"></a>Volgende stappen

In dit document hebt u geleerd hoe u een .NET Framework-toepassing uit Hive en Pig gebruiken op HDInsight. Als u wilt meer informatie over het gebruik van Python met Hive en Pig, Zie [Python gebruiken met Apache Hive en Apache Pig in HDInsight](python-udf-hdinsight.md).

Voor andere manieren Pig en Hive gebruiken, en voor informatie over het gebruik van MapReduce, Zie de volgende documenten:

* [Apache Hive gebruiken met HDInsight](hdinsight-use-hive.md)
* [Apache Pig gebruiken met HDInsight](hdinsight-use-pig.md)
* [MapReduce gebruiken met HDInsight](hdinsight-use-mapreduce.md)
