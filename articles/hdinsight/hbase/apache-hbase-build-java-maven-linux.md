---
title: Apache Maven gebruiken om te maken van een Java-HBase-client voor Azure HDInsight
description: Informatie over het gebruik van Apache Maven in een Apache HBase op basis van een Java-toepassing bouwen en vervolgens implementeert u deze in HBase op Azure HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive,seodec18
ms.topic: conceptual
ms.date: 04/16/2019
ms.openlocfilehash: 1ec4e9cbfd1d70c128f530bd996793a49c8a7d00
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/01/2019
ms.locfileid: "67484120"
---
# <a name="build-java-applications-for-apache-hbase"></a>Java-toepassingen voor Apache HBase bouwen

Meer informatie over het maken van een [Apache HBase](https://hbase.apache.org/) toepassing in Java. Gebruik vervolgens de toepassing met HBase op Azure HDInsight.

De stappen in dit document gebruiken [Apache Maven](https://maven.apache.org/) maken en bouw het project. Maven is een begrip hulpprogramma waarmee u software, documentatie en rapporten voor Java-projecten bouwen en beheer van software-project.

## <a name="prerequisites"></a>Vereisten

* Een Apache HBase-cluster in HDInsight. Zie [aan de slag met Apache HBase](./apache-hbase-tutorial-get-started-linux.md).

* [Java Developer Kit (JDK) versie 8](https://aka.ms/azure-jdks).

* [Apache Maven](https://maven.apache.org/download.cgi) goed [geïnstalleerd](https://maven.apache.org/install.html) op basis van Apache.  Maven is een project maken voor Java-projecten.

* Een SSH-client. Zie voor meer informatie [Verbinding maken met HDInsight (Apache Hadoop) via SSH](../hdinsight-hadoop-linux-use-ssh-unix.md).

* Als u PowerShell gebruikt, moet u de [AZ Module](https://docs.microsoft.com/powershell/azure/overview).

* Een teksteditor. In dit artikel maakt gebruik van Microsoft Notepad.

## <a name="test-environment"></a>Testomgeving
De omgeving moet worden gebruikt voor dit artikel is een computer met Windows 10.  De opdrachten zijn uitgevoerd in een opdrachtprompt en de verschillende bestanden zijn bewerkt met Kladblok. Aanpassen voor uw omgeving.

Voer de onderstaande opdrachten om een werkende-omgeving te maken vanaf een opdrachtprompt:

```cmd
IF NOT EXIST C:\HDI MKDIR C:\HDI
cd C:\HDI
```

## <a name="create-a-maven-project"></a>Maak een Maven-project

1. Voer de volgende opdracht om te maken van een Maven-project met de naam **hbaseapp**:

    ```cmd
    mvn archetype:generate -DgroupId=com.microsoft.examples -DartifactId=hbaseapp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false

    cd hbaseapp
    mkdir conf
    ```

    Deze opdracht maakt u een map met de naam `hbaseapp` op de huidige locatie, die een eenvoudige Maven-project bevat. De tweede opdracht wijzigt de werkmap naar `hbaseapp`. De derde opdracht maakt u een nieuwe map `conf`, die later worden gebruikt. De `hbaseapp` map bevat de volgende items:

    * `pom.xml`:  De Project-objectmodel ([POM](https://maven.apache.org/guides/introduction/introduction-to-the-pom.html)) bevat informatie en configuratie die wordt gebruikt om het project te bouwen.
    * `src\main\java\com\microsoft\examples`: Bevat de code van uw toepassing.
    * `src\test\java\com\microsoft\examples`: Tests voor de toepassing bevat.

2. Het van de gegenereerde voorbeeldcode verwijderen. Verwijderen van de gegenereerde Testscenario's en toepassingsbestanden `AppTest.java`, en `App.java` met de onderstaande opdrachten:

    ```cmd
    DEL src\main\java\com\microsoft\examples\App.java
    DEL src\test\java\com\microsoft\examples\AppTest.java
    ```

## <a name="update-the-project-object-model"></a>Het objectmodel Project bijwerken

Zie voor een volledig overzicht van het bestand pom.xml https://maven.apache.org/pom.html.  Open `pom.xml` met de onderstaande opdracht:

```cmd
notepad pom.xml
```

### <a name="add-dependencies"></a>Afhankelijkheden toevoegen

In `pom.xml`, voeg de volgende tekst in de `<dependencies>` sectie:

```xml
<dependency>
    <groupId>org.apache.hbase</groupId>
    <artifactId>hbase-client</artifactId>
    <version>1.1.2</version>
</dependency>
<dependency>
    <groupId>org.apache.phoenix</groupId>
    <artifactId>phoenix-core</artifactId>
    <version>4.14.1-HBase-1.1</version>
</dependency>
```  

In deze sectie geeft aan dat het project moet **hbase-client** en **phoenix-core** onderdelen. Bij het compileren, worden deze afhankelijkheden gedownload vanuit de opslagplaats van de Maven standaard. U kunt de [Maven Central Repository zoeken](https://search.maven.org/artifact/org.apache.hbase/hbase-client/1.1.2/jar) voor meer informatie over deze afhankelijkheid.

> [!IMPORTANT]  
> Het versienummer van de hbase-client moet overeenkomen met de versie van Apache HBase die beschikbaar is in uw HDInsight-cluster. Gebruik de volgende tabel om te vinden van het juiste versienummer.

| HDInsight-clusterversie | Apache HBase-versie moet worden gebruikt |
| --- | --- |
| 3.6 | 1.1.2 |
| 4.0 | 2.0.0 |

Zie voor meer informatie over HDInsight-versies en onderdelen [wat de verschillende beschikbaar met HDInsight Apache Hadoop-onderdelen zijn](../hdinsight-component-versioning.md).

### <a name="build-configuration"></a>Configuratie samenstellen

Maven-invoegtoepassingen kunt u het aanpassen van de fasen van de build van het project. In deze sectie wordt gebruikt om toe te voegen-invoegtoepassingen, resources en andere configuratieopties build.

Voeg de volgende code aan de `pom.xml` -bestand, en vervolgens opslaan en sluiten van het bestand. Deze tekst moet zich binnen in de `<project>...</project>` labels in het bestand, bijvoorbeeld tussen `</dependencies>` en `</project>`.

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
                <version>3.8.0</version>
        <configuration>
            <source>1.8</source>
            <target>1.8</target>
        </configuration>
        </plugin>
    <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-shade-plugin</artifactId>
        <version>3.2.1</version>
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

In deze sectie configureert u een resource (`conf/hbase-site.xml`) die configuratie-informatie voor HBase bevat.

> [!NOTE]  
> U kunt ook instellen-configuratiewaarden via code. Zie de opmerkingen in de `CreateTable` voorbeeld.

In deze sectie configureert ook de [Apache Maven-Compiler invoegtoepassing](https://maven.apache.org/plugins/maven-compiler-plugin/) en [Apache Maven-invoegtoepassing voor tint](https://maven.apache.org/plugins/maven-shade-plugin/). De invoegtoepassing compiler wordt gebruikt voor het compileren van de topologie. De tint van de invoegtoepassing wordt gebruikt om te voorkomen dat de licentie duplicatie in het JAR-pakket dat is gebouwd met Maven. Deze invoegtoepassing wordt gebruikt om te voorkomen dat een 'dubbele licentiebestanden'-Fout tijdens de uitvoering van het HDInsight-cluster. Met behulp van maven-tint-invoegtoepassing met de `ApacheLicenseResourceTransformer` implementatie voorkomt dat de fout.

De maven-tint-invoegtoepassing levert ook een uber jar die de afhankelijkheden die zijn vereist voor de toepassing bevat.

### <a name="download-the-hbase-sitexml"></a>De hbase-site.xml downloaden

De volgende opdracht gebruiken om te kopiëren van de HBase-configuratie van de HBase-cluster naar de `conf` directory. Vervang `CLUSTERNAME` met uw HDInsight-clusternaam en voer vervolgens de opdracht:

```cmd
scp sshuser@CLUSTERNAME-ssh.azurehdinsight.net:/etc/hbase/conf/hbase-site.xml ./conf/hbase-site.xml
```

## <a name="create-the-application"></a>De toepassing maken

### <a name="implement-a-createtable-class"></a>Een klasse CreateTable implementeren

Voer de opdracht hieronder om te maken en open een nieuw bestand `CreateTable.java`. Selecteer **Ja** bij de prompt voor het maken van een nieuw bestand.

```cmd
notepad src\main\java\com\microsoft\examples\CreateTable.java
```

Kopieer en plak de volgende java-code naar het nieuwe bestand. Sluit het bestand.

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

Deze code wordt de `CreateTable` klasse, maakt u een tabel met de naam `people` en deze vullen met bepaalde vooraf gedefinieerde gebruikers.

### <a name="implement-a-searchbyemail-class"></a>Een klasse SearchByEmail implementeren

Voer de opdracht hieronder om te maken en open een nieuw bestand `SearchByEmail.java`. Selecteer **Ja** bij de prompt voor het maken van een nieuw bestand.

```cmd
notepad src\main\java\com\microsoft\examples\SearchByEmail.java
```

Kopieer en plak de volgende java-code naar het nieuwe bestand. Sluit het bestand.

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

De `SearchByEmail` klasse kan worden gebruikt om op te vragen rijen door e-mailadres. Omdat deze gebruikmaakt van een reguliere expressie-filter, kunt u een tekenreeks of een reguliere expressie opgeven bij het gebruik van de klasse.

### <a name="implement-a-deletetable-class"></a>Een klasse DeleteTable implementeren

Voer de opdracht hieronder om te maken en open een nieuw bestand `DeleteTable.java`. Selecteer **Ja** bij de prompt voor het maken van een nieuw bestand.

```cmd
notepad src\main\java\com\microsoft\examples\DeleteTable.java
```

Kopieer en plak de volgende java-code naar het nieuwe bestand. Sluit het bestand.

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

De `DeleteTable` klasse ruimt de HBase-tabellen in dit voorbeeld is gemaakt door te schakelen en neer te zetten in de tabel die zijn gemaakt door de `CreateTable` klasse.

## <a name="build-and-package-the-application"></a>Bouwen en de toepassing verpakken

1. Uit de `hbaseapp` directory, gebruik de volgende opdracht om te maken van een JAR-bestand dat de toepassing bevat:

    ```cmd
    mvn clean package
    ```

    Met deze opdracht bouwt en pakketten van de toepassing in een JAR-bestand.

2. Wanneer de opdracht is voltooid, de `hbaseapp/target` map bevat een bestand met de naam `hbaseapp-1.0-SNAPSHOT.jar`.

   > [!NOTE]  
   > De `hbaseapp-1.0-SNAPSHOT.jar` bestand is een jar uber. Het bevat de afhankelijkheden die zijn vereist voor het uitvoeren van de toepassing.

## <a name="upload-the-jar-and-run-jobs-ssh"></a>Uploaden van de JAR en voert u taken (SSH)

De volgende stappen `scp` de JAR kopiëren naar het primaire hoofdknooppunt van het Apache HBase op HDInsight-cluster. De `ssh` opdracht wordt vervolgens gebruikt om verbinding met het cluster en voer het voorbeeld rechtstreeks op het hoofdknooppunt.

1. Upload de jar met het cluster. Vervang `CLUSTERNAME` met uw HDInsight-clusternaam en voer vervolgens de volgende opdracht uit:

    ```cmd
    scp ./target/hbaseapp-1.0-SNAPSHOT.jar sshuser@CLUSTERNAME-ssh.azurehdinsight.net:hbaseapp-1.0-SNAPSHOT.jar
    ```

2. Verbinding maken met de HBase-cluster. Vervang `CLUSTERNAME` met uw HDInsight-clusternaam en voer vervolgens de volgende opdracht uit:

    ```cmd
    ssh sshuser@CLUSTERNAME-ssh.azurehdinsight.net
    ```

 3. Voor het maken van een HBase-tabel met behulp van de Java-toepassing met de volgende opdracht in uw open ssh-verbinding gebruiken:

    ```bash
    yarn jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.CreateTable
    ```

    Met deze opdracht maakt u een HBase-tabel met de naam **mensen**, en gevuld met gegevens.

4. Als u wilt zoeken naar e-mailadressen die zijn opgeslagen in de tabel, gebruik de volgende opdracht:

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

5. Als u wilt verwijderen in de tabel, gebruikt u de volgende opdracht uit:

    ```bash
    yarn jar hbaseapp-1.0-SNAPSHOT.jar com.microsoft.examples.DeleteTable
    ```

## <a name="upload-the-jar-and-run-jobs-powershell"></a>Uploaden van de JAR en voert u taken (PowerShell)

De volgende stappen uit gebruik van Azure PowerShell [AZ module](https://docs.microsoft.com/powershell/azure/new-azureps-module-az) de JAR uploaden naar de standaardopslag voor het Apache HBase-cluster. HDInsight-cmdlets worden gebruikt voor het extern uitvoeren van de voorbeelden.

1. Nadat het installeren en configureren van de AZ-module, maakt een bestand met de naam `hbase-runner.psm1`. Gebruik de volgende tekst als de inhoud van dit bestand:

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
    $jobDefinition = New-AzHDInsightMapReduceJobDefinition `
        -JarFile $jarFile `
        -ClassName $className `
        -Arguments $emailRegex

    # Get the job output
    $job = Start-AzHDInsightJob `
        -ClusterName $clusterName `
        -JobDefinition $jobDefinition `
        -HttpCredential $creds
    Write-Host "Wait for the job to complete ..." -ForegroundColor Green
    Wait-AzHDInsightJob `
        -ClusterName $clusterName `
        -JobId $job.JobId `
        -HttpCredential $creds
    if($showErr)
    {
    Write-Host "STDERR"
    Get-AzHDInsightJobOutput `
                -Clustername $clusterName `
                -JobId $job.JobId `
                -HttpCredential $creds `
                -DisplayOutputType StandardError
    }
    Write-Host "Display the standard output ..." -ForegroundColor Green
    Get-AzHDInsightJobOutput `
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
        Set-AzStorageBlobContent -File $localPath `
            -Blob $destinationPath `
            -force:$force `
            -Container $storage.container `
            -Context $storage.context
    }

    function FindAzure {
        # Is there an active Azure subscription?
        $sub = Get-AzSubscription -ErrorAction SilentlyContinue
        if(-not($sub))
        {
            Connect-AzAccount
        }
    }

    function GetStorage {
        param(
            [Parameter(Mandatory = $true)]
            [String]$clusterName
        )
        $hdi = Get-AzHDInsightCluster -ClusterName $clusterName
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
        $storageAccountKey=(Get-AzStorageAccountKey `
            -Name $storageAccountName `
        -ResourceGroupName $resourceGroup)[0].Value
        # Get the resource group, in case we need that
        $return.resourceGroup = $resourceGroup
        # Get the storage context, as we can't depend
        # on using the default storage context
        $return.context = New-AzStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey
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

   * **Voeg HDInsightFile** : wordt gebruikt voor bestanden uploaden naar het cluster
   * **Start-HBaseExample** : wordt gebruikt voor het uitvoeren van de klassen die eerder hebt gemaakt

2. Sla de `hbase-runner.psm1` -bestand in de `hbaseapp` directory.

3. De modules registreren met Azure PowerShell. Een nieuw Azure PowerShell-venster openen en bewerken van de onderstaande opdracht door te vervangen `CLUSTERNAME` met de naam van uw cluster. Voer de volgende opdrachten:

    ```powershell
    cd C:\HDI\hbaseapp
    $myCluster = "CLUSTERNAME"
    Import-Module .\hbase-runner.psm1
    ```

4. Gebruik de volgende opdracht om te uploaden de `hbaseapp-1.0-SNAPSHOT.jar` met uw cluster.

    ```powershell
    Add-HDInsightFile -localPath target\hbaseapp-1.0-SNAPSHOT.jar -destinationPath example/jars/hbaseapp-1.0-SNAPSHOT.jar -clusterName $myCluster
    ```

    Wanneer u wordt gevraagd, typt u de clusternaam van het aanmeldingswachtwoord (beheerder) en het wachtwoord. De opdracht uploadt de `hbaseapp-1.0-SNAPSHOT.jar` naar de `example/jars` locatie in de primaire opslag voor uw cluster.

5. Het maken van een tabel met de `hbaseapp`, gebruikt u de volgende opdracht:

    ```powershell
    Start-HBaseExample -className com.microsoft.examples.CreateTable -clusterName $myCluster
    ```

    Wanneer u wordt gevraagd, typt u de clusternaam van het aanmeldingswachtwoord (beheerder) en het wachtwoord.

    Deze opdracht maakt u een tabel met de naam **mensen** in HBase op uw HDInsight-cluster. Met deze opdracht weergegeven niet eventuele uitvoer in het consolevenster.

6. Als u wilt zoeken naar vermeldingen in de tabel, gebruik de volgende opdracht:

    ```powershell
    Start-HBaseExample -className com.microsoft.examples.SearchByEmail -clusterName $myCluster -emailRegex contoso.com
    ```

    Wanneer u wordt gevraagd, typt u de clusternaam van het aanmeldingswachtwoord (beheerder) en het wachtwoord.

    Deze opdracht wordt de `SearchByEmail` klasse om te zoeken naar alle rijen waarin de `contactinformation` kolomfamilie en de `email` kolom bevat de tekenreeks `contoso.com`. U ontvangt de volgende resultaten:

          Franklin Holtz - ID: 2
          Franklin Holtz - franklin@contoso.com - ID: 2
          Rae Schroeder - ID: 4
          Rae Schroeder - rae@contoso.com - ID: 4
          Gabriela Ingram - ID: 6
          Gabriela Ingram - gabriela@contoso.com - ID: 6

    Met behulp van **fabrikam.com** voor de `-emailRegex` waarde als resultaat geeft de gebruikers die hebben **fabrikam.com** in het veld e-mailbericht. U kunt ook reguliere expressies gebruiken als de zoekterm. Bijvoorbeeld, **^ r** retourneert e-mailadressen die met de letter 'r beginnen'.

7. Als u wilt verwijderen in de tabel, gebruikt u de volgende opdracht uit:

    ```PowerShell
    Start-HBaseExample -className com.microsoft.examples.DeleteTable -clusterName $myCluster
    ```

### <a name="no-results-or-unexpected-results-when-using-start-hbaseexample"></a>Er zijn geen resultaten of onverwachte resultaten bij het gebruik van Start-HBaseExample

Gebruik de `-showErr` parameter om de standaardfout (STDERR) dat wordt gegenereerd tijdens het uitvoeren van de taak weer te geven.

## <a name="next-steps"></a>Volgende stappen

[Informatie over het gebruik van SQuirreL SQL met Apache HBase](apache-hbase-phoenix-squirrel-linux.md)
