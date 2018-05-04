---
title: Java-HBase-client - Azure HDInsight | Microsoft Docs
description: Informatie over het gebruik van Apache Maven een Apache HBase op basis van Java-toepassing bouwen en vervolgens implementeren op HBase op Azure HDInsight.
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: cgronlun
editor: ''
ms.assetid: 1d1ed180-e0f4-4d1c-b5ea-72e0eda643bc
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: conceptual
ms.date: 04/30/2018
ms.author: larryfr
ms.openlocfilehash: 564bfb9c80fe835bd5defb4321607e0970c9f0aa
ms.sourcegitcommit: 6e43006c88d5e1b9461e65a73b8888340077e8a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/01/2018
---
# <a name="build-java-applications-for-apache-hbase"></a>Java-toepassingen voor Apache HBase bouwen

Meer informatie over het maken van een [Apache HBase](http://hbase.apache.org/) toepassing in Java. Gebruik vervolgens de toepassing met HBase op Azure HDInsight.

De stappen in dit document gebruik [Maven](http://maven.apache.org/) maken en bouw het project. Maven is een begrip hulpprogramma waarmee u software, documentatie en rapporten voor Java-projecten bouwen en beheer van software-project.

> [!NOTE]
> De stappen in dit document zijn meest recent getest met HDInsight 3.6.

> [!IMPORTANT]
> De stappen in dit document moet een HDInsight-cluster dat gebruik maakt van Linux. Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](../hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.

## <a name="requirements"></a>Vereisten

* [Java-platform JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 8 of hoger.

    > [!NOTE]
    > HDInsight 3.5 en groter vereist Java 8. Eerdere versies van HDInsight vereisen Java 7.

* [Maven](http://maven.apache.org/)

* [Een Azure HDInsight op basis van Linux-cluster met HBase](apache-hbase-tutorial-get-started-linux.md#create-hbase-cluster)

## <a name="create-the-project"></a>Het project maken

1. Vanaf de opdrachtregel in uw ontwikkelomgeving, wijzig de mappen naar de locatie waar u wilt maken van het project, bijvoorbeeld `cd code\hbase`.

2. Gebruik de **mvn** opdracht, die is geïnstalleerd met Maven, genereren de steiger voor het project.

    ```bash
    mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=hbaseapp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

    > [!NOTE]
    > Als u met behulp van PowerShell, moet u tussen de `-D` parameters tussen dubbele aanhalingstekens.
    >
    > `mvn archetype:generate "-DgroupId=com.microsoft.examples" "-DartifactId=hbaseapp" "-DarchetypeArtifactId=maven-archetype-quickstart" "-DinteractiveMode=false"`

    Met deze opdracht maakt u een map met dezelfde naam als de **artefact-id** parameter (**hbaseapp** in dit voorbeeld.) Deze map bevat de volgende items:

   * **pom.XML**: de Project-objectmodel ([POM](http://maven.apache.org/guides/introduction/introduction-to-the-pom.html)) bevat informatie en configuratie-informatie gebruikt om het project te bouwen.
   * **SRC**: de map waarin zich de **main/java/com/microsoft/voorbeelden** directory, waar u de toepassing schrijven.

3. Verwijder de `src/test/java/com/microsoft/examples/apptest.java` bestand. Het is niet in dit voorbeeld worden gebruikt.

## <a name="update-the-project-object-model"></a>Het Project objectmodel bijwerken

1. Bewerk de `pom.xml` -bestand en voeg de volgende code in de `<dependencies>` sectie:

   ```xml
    <dependency>
        <groupId>org.apache.hbase</groupId>
        <artifactId>hbase-client</artifactId>
        <version>1.1.2</version>
    </dependency>
    <dependency>
        <groupId>org.apache.phoenix</groupId>
        <artifactId>phoenix-core</artifactId>
        <version>4.4.0-HBase-1.1</version>
    </dependency>
   ```

    Deze sectie geeft aan dat het project moet **hbase-client** en **phoenix-core** onderdelen. Tijdens de compilatie, worden deze afhankelijkheden gedownload vanuit de opslagplaats met Maven standaard. U kunt de [Maven centrale opslagplaats zoeken](http://search.maven.org/#artifactdetails%7Corg.apache.hbase%7Chbase-client%7C0.98.4-hadoop2%7Cjar) voor meer informatie over deze afhankelijkheid.

   > [!IMPORTANT]
   > Het versienummer van de hbase-client moet overeenkomen met de versie van HBase die beschikbaar is in uw HDInsight-cluster. Gebruik de volgende tabel om te zoeken naar het juiste versienummer.

   | HDInsight-cluster versie | HBase-versie moet worden gebruikt |
   | --- | --- |
   | 3.2 |0.98.4-hadoop2 |
   | 3.3, 3.4 3.5 en 3.6 |1.1.2 |

    Zie voor meer informatie over de versies van HDInsight en -onderdelen [wat de verschillende beschikbaar met HDInsight Hadoop-onderdelen zijn](../hdinsight-component-versioning.md).

3. Voeg de volgende code naar de **pom.xml** bestand. Deze tekst moet binnen het `<project>...</project>` codes in het bestand bijvoorbeeld tussen `</dependencies>` en `</project>`.

   ```xml
    <build>
        <sourceDirectory>src</sourceDirectory>
        <resources>
        <resource>
            <directory>${basedir}/conf</directory>
            <filtering>false</filtering>
            <includes>
            <include>hbase-site.xml</include>
            </includes>
        </resource>
        </resources>
        <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
                    <version>3.3</version>
            <configuration>
                <source>1.8</source>
                <target>1.8</target>
            </configuration>
            </plugin>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-shade-plugin</artifactId>
            <version>2.3</version>
            <configuration>
            <transformers>
                <transformer implementation="org.apache.maven.plugins.shade.resource.ApacheLicenseResourceTransformer">
                </transformer>
            </transformers>
            </configuration>
            <executions>
            <execution>
                <phase>package</phase>
                <goals>
                <goal>shade</goal>
                </goals>
            </execution>
            </executions>
        </plugin>
        </plugins>
    </build>
   ```

    Deze sectie configureert u een resource (`conf/hbase-site.xml`) die configuratie-informatie voor HBase bevat.

   > [!NOTE]
   > U kunt ook configuratiewaarden via code instellen. Zie de opmerkingen in de `CreateTable` voorbeeld.

    Deze sectie configureert ook de [Maven-Compiler-invoegtoepassing](http://maven.apache.org/plugins/maven-compiler-plugin/) en [Maven grijs-invoegtoepassing](http://maven.apache.org/plugins/maven-shade-plugin/). De invoegtoepassing compiler wordt gebruikt voor het compileren van de topologie. De tint van de invoegtoepassing wordt gebruikt om te voorkomen dat de licentie duplicatie in het JAR-pakket dat wordt gebouwd door Maven. Deze invoegtoepassing wordt gebruikt om te voorkomen dat een 'dubbele licentiebestanden'-Fout tijdens de uitvoering van het HDInsight-cluster. Met behulp van maven-schaduw-invoegtoepassing met de `ApacheLicenseResourceTransformer` implementatie wordt voorkomen dat de fout.

    Maven-schaduw-invoegtoepassing produceert ook een jar uber die de afhankelijkheden die zijn vereist voor de toepassing bevat.

4. Sla het bestand `pom.xml` op.

5. Maak een map met de naam `conf` in de `hbaseapp` directory. Deze map wordt gebruikt om configuratie-informatie voor het verbinden met HBase.

6. Gebruik de volgende opdracht om te kopiëren van de HBase-configuratie in het HBase-cluster om de `conf` directory. Vervang `USERNAME` met de naam van uw SSH-aanmelding. Vervang `CLUSTERNAME` met de naam van uw HDInsight-cluster:

    ```bash
    scp USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:/etc/hbase/conf/hbase-site.xml ./conf/hbase-site.xml
    ```

   Voor meer informatie over het gebruik van `ssh` en `scp`, Zie [SSH gebruiken met HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).

## <a name="create-the-application"></a>De toepassing maken

1. Ga naar de `hbaseapp/src/main/java/com/microsoft/examples` map en wijzig de naam van de app.java-bestand in `CreateTable.java`.

2. Open de `CreateTable.java` -bestand en vervang de bestaande inhoud met de volgende tekst:

   ```java
    package com.microsoft.examples;
    import java.io.IOException;

    import org.apache.hadoop.conf.Configuration;
    import org.apache.hadoop.hbase.HBaseConfiguration;
    import org.apache.hadoop.hbase.client.HBaseAdmin;
    import org.apache.hadoop.hbase.HTableDescriptor;
    import org.apache.hadoop.hbase.TableName;
    import org.apache.hadoop.hbase.HColumnDescriptor;
    import org.apache.hadoop.hbase.client.HTable;
    import org.apache.hadoop.hbase.client.Put;
    import org.apache.hadoop.hbase.util.Bytes;

    public class CreateTable {
        public static void main(String[] args) throws IOException {
        Configuration config = HBaseConfiguration.create();

        // Example of setting zookeeper values for HDInsight
        // in code instead of an hbase-site.xml file
        //
        // config.set("hbase.zookeeper.quorum",
        //            "zookeepernode0,zookeepernode1,zookeepernode2");
        //config.set("hbase.zookeeper.property.clientPort", "2181");
        //config.set("hbase.cluster.distributed", "true");
        //
        //NOTE: Actual zookeeper host names can be found using Ambari:
        //curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/hosts"

        //Linux-based HDInsight clusters use /hbase-unsecure as the znode parent
        config.set("zookeeper.znode.parent","/hbase-unsecure");

        // create an admin object using the config
        HBaseAdmin admin = new HBaseAdmin(config);

        // create the table...
        HTableDescriptor tableDescriptor = new HTableDescriptor(TableName.valueOf("people"));
        // ... with two column families
        tableDescriptor.addFamily(new HColumnDescriptor("name"));
        tableDescriptor.addFamily(new HColumnDescriptor("contactinfo"));
        admin.createTable(tableDescriptor);

        // define some people
        String[][] people = {
            { "1", "Marcel", "Haddad", "marcel@fabrikam.com"},
            { "2", "Franklin", "Holtz", "franklin@contoso.com" },
            { "3", "Dwayne", "McKee", "dwayne@fabrikam.com" },
            { "4", "Rae", "Schroeder", "rae@contoso.com" },
            { "5", "Rosalie", "burton", "rosalie@fabrikam.com"},
            { "6", "Gabriela", "Ingram", "gabriela@contoso.com"} };

        HTable table = new HTable(config, "people");

        // Add each person to the table
        //   Use the `name` column family for the name
        //   Use the `contactinfo` column family for the email
        for (int i = 0; i< people.length; i++) {
            Put person = new Put(Bytes.toBytes(people[i][0]));
            person.add(Bytes.toBytes("name"), Bytes.toBytes("first"), Bytes.toBytes(people[i][1]));
            person.add(Bytes.toBytes("name"), Bytes.toBytes("last"), Bytes.toBytes(people[i][2]));
            person.add(Bytes.toBytes("contactinfo"), Bytes.toBytes("email"), Bytes.toBytes(people[i][3]));
            table.put(person);
        }
        // flush commits and close the table
        table.flushCommits();
        table.close();
        }
    }
   ```

    Deze code wordt de **CreateTable** klasse, die u een tabel met de naam maakt **mensen** en deze vullen met een aantal vooraf gedefinieerde gebruikers.

3. Sla het bestand `CreateTable.java` op.

4. In de `hbaseapp/src/main/java/com/microsoft/examples` directory, maakt u een bestand met de naam `SearchByEmail.java`. Gebruik de volgende tekst als de inhoud van dit bestand:

   ```java
    package com.microsoft.examples;
    import java.io.IOException;

    import org.apache.hadoop.conf.Configuration;
    import org.apache.hadoop.hbase.HBaseConfiguration;
    import org.apache.hadoop.hbase.client.HTable;
    import org.apache.hadoop.hbase.client.Scan;
    import org.apache.hadoop.hbase.client.ResultScanner;
    import org.apache.hadoop.hbase.client.Result;
    import org.apache.hadoop.hbase.filter.RegexStringComparator;
    import org.apache.hadoop.hbase.filter.SingleColumnValueFilter;
    import org.apache.hadoop.hbase.filter.CompareFilter.CompareOp;
    import org.apache.hadoop.hbase.util.Bytes;
    import org.apache.hadoop.util.GenericOptionsParser;

    public class SearchByEmail {
        public static void main(String[] args) throws IOException {
        Configuration config = HBaseConfiguration.create();

        // Use GenericOptionsParser to get only the parameters to the class
        // and not all the parameters passed (when using WebHCat for example)
        String[] otherArgs = new GenericOptionsParser(config, args).getRemainingArgs();
        if (otherArgs.length != 1) {
            System.out.println("usage: [regular expression]");
            System.exit(-1);
        }

        // Open the table
        HTable table = new HTable(config, "people");

        // Define the family and qualifiers to be used
        byte[] contactFamily = Bytes.toBytes("contactinfo");
        byte[] emailQualifier = Bytes.toBytes("email");
        byte[] nameFamily = Bytes.toBytes("name");
        byte[] firstNameQualifier = Bytes.toBytes("first");
        byte[] lastNameQualifier = Bytes.toBytes("last");

        // Create a regex filter
        RegexStringComparator emailFilter = new RegexStringComparator(otherArgs[0]);
        // Attach the regex filter to a filter
        //   for the email column
        SingleColumnValueFilter filter = new SingleColumnValueFilter(
            contactFamily,
            emailQualifier,
            CompareOp.EQUAL,
            emailFilter
        );

        // Create a scan and set the filter
        Scan scan = new Scan();
        scan.setFilter(filter);

        // Get the results
        ResultScanner results = table.getScanner(scan);
        // Iterate over results and print  values
        for (Result result : results ) {
            String id = new String(result.getRow());
            byte[] firstNameObj = result.getValue(nameFamily, firstNameQualifier);
            String firstName = new String(firstNameObj);
            byte[] lastNameObj = result.getValue(nameFamily, lastNameQualifier);
            String lastName = new String(lastNameObj);
            System.out.println(firstName + " " + lastName + " - ID: " + id);
            byte[] emailObj = result.getValue(contactFamily, emailQualifier);
            String email = new String(emailObj);
            System.out.println(firstName + " " + lastName + " - " + email + " - ID: " + id);
        }
        results.close();
        table.close();
        }
    }
   ```

    De **SearchByEmail** klasse kan worden gebruikt voor de query voor rijen door e-mailadres. Omdat een filter reguliere expressie worden gebruikt, kunt u een tekenreeks of een reguliere expressie opgeven bij het gebruik van de klasse.

5. Sla het bestand `SearchByEmail.java` op.

6. In de `hbaseapp/src/main/hava/com/microsoft/examples` directory, maakt u een bestand met de naam `DeleteTable.java`. Gebruik de volgende tekst als de inhoud van dit bestand:

   ```java
    package com.microsoft.examples;
    import java.io.IOException;

    import org.apache.hadoop.conf.Configuration;
    import org.apache.hadoop.hbase.HBaseConfiguration;
    import org.apache.hadoop.hbase.client.HBaseAdmin;

    public class DeleteTable {
        public static void main(String[] args) throws IOException {
        Configuration config = HBaseConfiguration.create();

        // Create an admin object using the config
        HBaseAdmin admin = new HBaseAdmin(config);

        // Disable, and then delete the table
        admin.disableTable("people");
        admin.deleteTable("people");
        }
    }
   ```

    Deze klasse de HBase-tabellen in dit voorbeeld is gemaakt door uitschakelen en neer te zetten in de tabel die is gemaakt door schoont de `CreateTable` klasse.

7. Sla het bestand `DeleteTable.java` op.

## <a name="build-and-package-the-application"></a>Bouwen en de toepassing van het pakket

1. Van de `hbaseapp` directory, gebruik de volgende opdracht voor het bouwen van een JAR-bestand dat de toepassing bevat:

    ```bash
    mvn clean package
    ```

    Met deze opdracht maakt en pakketten van de toepassing naar een JAR-bestand.

2. Wanneer de opdracht is voltooid, de `hbaseapp/target` map bevat een bestand met de naam `hbaseapp-1.0-SNAPSHOT.jar`.

   > [!NOTE]
   > De `hbaseapp-1.0-SNAPSHOT.jar` bestand is een jar uber. Het bevat de afhankelijkheden die zijn vereist voor het uitvoeren van de toepassing.


## <a name="upload-the-jar-and-run-jobs-ssh"></a>Uploaden van de JAR-taken en uitvoeren (SSH)

De volgende stappen uitvoeren om `scp` de JAR kopiëren naar de primaire hoofdknooppunt van uw HBase op HDInsight-cluster. De `ssh` opdracht wordt vervolgens gebruikt voor verbinding met het cluster en voer in het voorbeeld rechtstreeks op het hoofdknooppunt.

1. Als u wilt de jar uploaden naar het cluster, moet u de volgende opdracht gebruiken:

    ```bash
    scp ./target/hbaseapp-1.0-SNAPSHOT.jar USERNAME@CLUSTERNAME-ssh.azurehdinsight.net:hbaseapp-1.0-SNAPSHOT.jar
    ```

    Vervang `USERNAME` met de naam van uw SSH-aanmelding. Vervang `CLUSTERNAME` met de naam van uw HDInsight-cluster.

2. Voor verbinding met de HBase-cluster, moet u de volgende opdracht gebruiken:

    ```bash
    ssh USERNAME@CLUSTERNAME-ssh.azurehdinsight.net
    ```

    Vervang `USERNAME` de naam van uw SSH-aanmelding. Vervang `CLUSTERNAME` met de naam van uw HDInsight-cluster.

3. Gebruik de volgende opdracht voor het maken van een HBase-tabel met de Java-toepassing:

    ```bash
    yarn jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.CreateTable
    ```

    Deze opdracht maakt u een HBase-tabel met de naam **mensen**, en gevuld met gegevens.

4. Als u wilt zoeken naar e-mailadressen die zijn opgeslagen in de tabel, moet u de volgende opdracht gebruiken:

    ```bash
    yarn jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.SearchByEmail contoso.com
    ```

    U ontvangt de volgende resultaten:

        Franklin Holtz - ID: 2
        Franklin Holtz - franklin@contoso.com - ID: 2
        Rae Schroeder - ID: 4
        Rae Schroeder - rae@contoso.com - ID: 4
        Gabriela Ingram - ID: 6
        Gabriela Ingram - gabriela@contoso.com - ID: 6

5. Gebruik de volgende opdracht voor het verwijderen van de tabel:

    ```bash
    yarn jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.DeleteTable
    ```

## <a name="upload-the-jar-and-run-jobs-powershell"></a>Uploaden van de JAR-taken en uitvoeren (PowerShell)

De volgende stappen gebruikt Azure PowerShell de JAR uploaden naar de standaard-opslag voor uw HBase-cluster. HDInsight-cmdlets worden gebruikt voor het extern uitvoeren van de voorbeelden.

1. Nadat het installeren en configureren van Azure PowerShell, maakt u een bestand met de naam `hbase-runner.psm1`. Gebruik de volgende tekst als de inhoud van dit bestand:

   ```powershell
    <#
    .SYNOPSIS
    Copies a file to the primary storage of an HDInsight cluster.
    .DESCRIPTION
    Copies a file from a local directory to the blob container for
    the HDInsight cluster.
    .EXAMPLE
    Start-HBaseExample -className "com.microsoft.examples.CreateTable"
    -clusterName "MyHDInsightCluster"

    .EXAMPLE
    Start-HBaseExample -className "com.microsoft.examples.SearchByEmail"
    -clusterName "MyHDInsightCluster"
    -emailRegex "contoso.com"

    .EXAMPLE
    Start-HBaseExample -className "com.microsoft.examples.SearchByEmail"
    -clusterName "MyHDInsightCluster"
    -emailRegex "^r" -showErr
    #>

    function Start-HBaseExample {
    [CmdletBinding(SupportsShouldProcess = $true)]
    param(
    #The class to run
    [Parameter(Mandatory = $true)]
    [String]$className,

    #The name of the HDInsight cluster
    [Parameter(Mandatory = $true)]
    [String]$clusterName,

    #Only used when using SearchByEmail
    [Parameter(Mandatory = $false)]
    [String]$emailRegex,

    #Use if you want to see stderr output
    [Parameter(Mandatory = $false)]
    [Switch]$showErr
    )

    Set-StrictMode -Version 3

    # Is the Azure module installed?
    FindAzure

    # Get the login for the HDInsight cluster
    $creds=Get-Credential -Message "Enter the login for the cluster" -UserName "admin"

    # The JAR
    $jarFile = "wasb:///example/jars/hbaseapp-1.0-SNAPSHOT.jar"

    # The job definition
    $jobDefinition = New-AzureRmHDInsightMapReduceJobDefinition `
        -JarFile $jarFile `
        -ClassName $className `
        -Arguments $emailRegex

    # Get the job output
    $job = Start-AzureRmHDInsightJob `
        -ClusterName $clusterName `
        -JobDefinition $jobDefinition `
        -HttpCredential $creds
    Write-Host "Wait for the job to complete ..." -ForegroundColor Green
    Wait-AzureRmHDInsightJob `
        -ClusterName $clusterName `
        -JobId $job.JobId `
        -HttpCredential $creds
    if($showErr)
    {
    Write-Host "STDERR"
    Get-AzureRmHDInsightJobOutput `
                -Clustername $clusterName `
                -JobId $job.JobId `
                -HttpCredential $creds `
                -DisplayOutputType StandardError
    }
    Write-Host "Display the standard output ..." -ForegroundColor Green
    Get-AzureRmHDInsightJobOutput `
                -Clustername $clusterName `
                -JobId $job.JobId `
                -HttpCredential $creds
    }

    <#
    .SYNOPSIS
    Copies a file to the primary storage of an HDInsight cluster.
    .DESCRIPTION
    Copies a file from a local directory to the blob container for
    the HDInsight cluster.
    .EXAMPLE
    Add-HDInsightFile -localPath "C:\temp\data.txt"
    -destinationPath "example/data/data.txt"
    -ClusterName "MyHDInsightCluster"
    .EXAMPLE
    Add-HDInsightFile -localPath "C:\temp\data.txt"
    -destinationPath "example/data/data.txt"
    -ClusterName "MyHDInsightCluster"
    -Container "MyContainer"
    #>

    function Add-HDInsightFile {
        [CmdletBinding(SupportsShouldProcess = $true)]
        param(
            #The path to the local file.
            [Parameter(Mandatory = $true)]
            [String]$localPath,

            #The destination path and file name, relative to the root of the container.
            [Parameter(Mandatory = $true)]
            [String]$destinationPath,

            #The name of the HDInsight cluster
            [Parameter(Mandatory = $true)]
            [String]$clusterName,

            #If specified, overwrites existing files without prompting
            [Parameter(Mandatory = $false)]
            [Switch]$force
        )

        Set-StrictMode -Version 3

        # Is the Azure module installed?
        FindAzure

        # Get authentication for the cluster
        $creds=Get-Credential

        # Does the local path exist?
        if (-not (Test-Path $localPath))
        {
            throw "Source path '$localPath' does not exist."
        }

        # Get the primary storage container
        $storage = GetStorage -clusterName $clusterName

        # Upload file to storage, overwriting existing files if -force was used.
        Set-AzureStorageBlobContent -File $localPath `
            -Blob $destinationPath `
            -force:$force `
            -Container $storage.container `
            -Context $storage.context
    }

    function FindAzure {
        # Is there an active Azure subscription?
        $sub = Get-AzureRmSubscription -ErrorAction SilentlyContinue
        if(-not($sub))
        {
            throw "No active Azure subscription found! If you have a subscription, use the Connect-AzureRmAccount cmdlet to login to your subscription."
        }
    }

    function GetStorage {
        param(
            [Parameter(Mandatory = $true)]
            [String]$clusterName
        )
        $hdi = Get-AzureRmHDInsightCluster -ClusterName $clusterName
        # Does the cluster exist?
        if (!$hdi)
        {
            throw "HDInsight cluster '$clusterName' does not exist."
        }
        # Create a return object for context & container
        $return = @{}
        $storageAccounts = @{}

        # Get storage information
        $resourceGroup = $hdi.ResourceGroup
        $storageAccountName=$hdi.DefaultStorageAccount.split('.')[0]
        $container=$hdi.DefaultStorageContainer
        $storageAccountKey=(Get-AzureRmStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value
        # Get the resource group, in case we need that
        $return.resourceGroup = $resourceGroup
        # Get the storage context, as we can't depend
        # on using the default storage context
        $return.context = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
        # Get the container, so we know where to
        # find/store blobs
        $return.container = $container
        # Return storage accounts to support finding all accounts for
        # a cluster
        $return.storageAccount = $storageAccountName
        $return.storageAccountKey = $storageAccountKey

        return $return
    }
    # Only export the verb-phrase things
    export-modulemember *-*
   ```

    Dit bestand bevat twee modules:

   * **Voeg HDInsightFile** : wordt gebruikt om bestanden te uploaden naar het cluster
   * **Start HBaseExample** : wordt gebruikt om uit te voeren van de klassen die eerder hebt gemaakt

2. Sla het bestand `hbase-runner.psm1` op.

3. Open een nieuw Azure PowerShell-venster, wijzig de mappen op de `hbaseapp` directory en voer de volgende opdracht:

    ```powershell
    PS C:\ Import-Module c:\path\to\hbase-runner.psm1
    ```

    Wijzig het pad naar de locatie van de `hbase-runner.psm1` bestand eerder hebt gemaakt. Deze opdracht wordt de module geregistreerd met Azure PowerShell.

4. Gebruik de volgende opdracht voor het uploaden van de `hbaseapp-1.0-SNAPSHOT.jar` met uw cluster.

    ```powershell
    Add-HDInsightFile -localPath target\hbaseapp-1.0-SNAPSHOT.jar -destinationPath example/jars/hbaseapp-1.0-SNAPSHOT.jar -clusterName hdinsightclustername
    ```

    Vervang `hdinsightclustername` door de naam van uw cluster. Wanneer u wordt gevraagd, typt u de cluster-aanmeldingsnaam (admin) en het wachtwoord. De opdracht uploadt de `hbaseapp-1.0-SNAPSHOT.jar` naar de `example/jars` locatie in de primaire opslag voor uw cluster.

5. Maken van een tabel met de `hbaseapp`, gebruik de volgende opdracht:

    ```powershell
    Start-HBaseExample -className com.microsoft.examples.CreateTable -clusterName hdinsightclustername
    ```

    Vervang `hdinsightclustername` door de naam van uw cluster. Wanneer u wordt gevraagd, typt u de cluster-aanmeldingsnaam (admin) en het wachtwoord.

    Met deze opdracht maakt een tabel met de naam **mensen** in HBase op uw HDInsight-cluster. Met deze opdracht wordt geen uitvoer niet weergegeven in het consolevenster.

6. Als u wilt zoeken naar vermeldingen in de tabel, moet u de volgende opdracht gebruiken:

    ```powershell
    Start-HBaseExample -className com.microsoft.examples.SearchByEmail -clusterName hdinsightclustername -emailRegex contoso.com
    ```

    Vervang `hdinsightclustername` door de naam van uw cluster. Wanneer u wordt gevraagd, typt u de cluster-aanmeldingsnaam (admin) en het wachtwoord.

    Deze opdracht wordt de `SearchByEmail` klasse om te zoeken naar alle rijen waarin de `contactinformation` kolomfamilie en de `email` kolom bevat de tekenreeks `contoso.com`. U ontvangt de volgende resultaten:

          Franklin Holtz - ID: 2
          Franklin Holtz - franklin@contoso.com - ID: 2
          Rae Schroeder - ID: 4
          Rae Schroeder - rae@contoso.com - ID: 4
          Gabriela Ingram - ID: 6
          Gabriela Ingram - gabriela@contoso.com - ID: 6

    Met behulp van **fabrikam.com** voor de `-emailRegex` waarde retourneert de gebruikers die hebben **fabrikam.com** in het e-veld. U kunt ook reguliere expressies gebruiken als de zoekterm. Bijvoorbeeld: **^ r** retourneert e-mailadressen die met de letter 'r beginnen'.

### <a name="no-results-or-unexpected-results-when-using-start-hbaseexample"></a>Er zijn geen resultaten of onverwachte resultaten optreden wanneer u Start HBaseExample

Gebruik de `-showErr` parameter om de standaardfout (STDERR) dat wordt gegenereerd tijdens het uitvoeren van de taak weer te geven.

## <a name="delete-the-table"></a>De tabel verwijderen

Wanneer u klaar bent met het voorbeeld, gebruikt u de volgende verwijderen van de **mensen** tabel die wordt gebruikt in dit voorbeeld:

__Van een `ssh` sessie__:

`yarn jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.DeleteTable`

__Vanuit Azure PowerShell__:

`Start-HBaseExample -className com.microsoft.examples.DeleteTable -clusterName hdinsightclustername`

## <a name="next-steps"></a>Volgende stappen

[Informatie over het gebruik van SQuirreL SQL met HBase](apache-hbase-phoenix-squirrel-linux.md)
