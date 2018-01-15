---
title: 'Java SDK: Bestandssysteembewerkingen in Azure Data Lake Store | Microsoft Docs'
description: Gebruik Azure Data Lake Store Java SDK om bestandssysteembewerkingen uit te voeren op Data Lake Store, bijvoorbeeld om mappen te maken enzovoort.
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: d10e09db-5232-4e84-bb50-52efc2c21887
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/09/2018
ms.author: nitinme
ms.openlocfilehash: fdae36c3cbdda16f9392a113502c7c6c62b25534
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2018
---
# <a name="filesystem-operations-on-data-lake-store-using-java-sdk"></a>Bestandssysteembewerkingen - Aan de slag met Azure Data Lake Store met Java SDK
> [!div class="op_single_selector"]
> * [.NET SDK](data-lake-store-data-operations-net-sdk.md)
> * [Java-SDK](data-lake-store-get-started-java-sdk.md)
> * [REST API](data-lake-store-data-operations-rest-api.md)
> * [Python](data-lake-store-data-operations-python.md)
>
> 

Lees hoe u met de Azure Data Lake Store Java SDK basisbewerkingen uitvoert, zoals het maken van mappen, het uploaden en downloaden van gegevensbestanden enzovoort. Zie [Azure Data Lake Store](data-lake-store-overview.md) voor meer informatie over Data Lake.

U kunt de Java SDK API-documenten voor Azure Data Lake Store openen via [Azure Data Lake Store Java API-documenten](https://azure.github.io/azure-data-lake-store-java/javadoc/).

## <a name="prerequisites"></a>Vereisten
* Java Development Kit (JDK 7 of hoger met Java versie 1.7 of hoger)
* Azure Data Lake Store-account. Volg de instructies in [Aan de slag met Azure Data Lake Store met Azure Portal](data-lake-store-get-started-portal.md).
* [Maven](https://maven.apache.org/install.html). In deze zelfstudie wordt Maven gebruikt voor build- en projectafhankelijkheden. Hoewel het mogelijk is te ontwikkelen zonder een buildsysteem als Maven of Gradle, maken deze systemen het veel eenvoudiger om afhankelijkheden te beheren.
* (Optioneel) Een IDE zoals [IntelliJ IDEA](https://www.jetbrains.com/idea/download/), [Eclipse](https://www.eclipse.org/downloads/) of vergelijkbaar.

## <a name="create-a-java-application"></a>Een Java-toepassing maken
Dit codevoorbeeld beschikbaar [in GitHub](https://azure.microsoft.com/documentation/samples/data-lake-store-java-upload-download-get-started/) doorloopt het proces waarin bestanden in het archief worden gemaakt, bestanden worden samengevoegd, een bestand wordt gedownload en een aantal bestanden uit het archief wordt verwijderd. In dit gedeelte van het artikel komen de belangrijkste onderdelen van de code aan bod.

1. Maak een Maven-project met behulp van [mvn archetype](https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html) vanaf de opdrachtregel of met behulp van een IDE. [Hier](https://www.jetbrains.com/help/idea/2016.1/creating-and-running-your-first-java-application.html) vindt u instructies over het maken van een Java-project met behulp van IntelliJ. [Hier](http://help.eclipse.org/mars/index.jsp?topic=%2Forg.eclipse.jdt.doc.user%2FgettingStarted%2Fqs-3.htm) vindt u instructies over het maken van een Java-project met behulp van Eclipse. 

2. Voeg de volgende afhankelijkheden toe aan het **pom.xml**-bestand in Maven. Voeg het volgende codefragment toe vóór de tag **\</project>**:
   
        <dependencies>
          <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>azure-data-lake-store-sdk</artifactId>
            <version>2.1.5</version>
          </dependency>
          <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-nop</artifactId>
            <version>1.7.21</version>
          </dependency>
        </dependencies>
   
    De eerste afhankelijkheid is om Data Lake Store SDK (`azure-data-lake-store-sdk`) vanuit de Maven-opslag te gebruiken. De tweede afhankelijkheid dient om op te geven welk framework voor logboekregistratie (`slf4j-nop`) voor deze toepassing moet worden gebruikt. De Data Lake Store SDK gebruikt een [slf4j](http://www.slf4j.org/)-façade voor logboekregistratie, waarmee u uit een aantal populaire frameworks voor logboekregistratie, zoals log4j, Java logging, logback enzovoort, of voor geen logboekregistratie kunt kiezen. In dit voorbeeld wordt logboekregistratie uitgeschakeld. Daarom wordt de binding **slf4j-nop** gebruikt. [Hier](http://www.slf4j.org/manual.html#projectDep) vindt u andere opties voor logboekregistratie voor uw toepassing.

3. Voeg de volgende importinstructies toe aan uw toepassing.

        import com.microsoft.azure.datalake.store.ADLException;
        import com.microsoft.azure.datalake.store.ADLStoreClient;
        import com.microsoft.azure.datalake.store.DirectoryEntry;
        import com.microsoft.azure.datalake.store.IfExists;
        import com.microsoft.azure.datalake.store.oauth2.AccessTokenProvider;
        import com.microsoft.azure.datalake.store.oauth2.ClientCredsTokenProvider;

        import java.io.*;
        import java.util.Arrays;
        import java.util.List;

## <a name="authentication"></a>Verificatie

* Zie [End-user-authentication with Data Lake Store using Java](data-lake-store-end-user-authenticate-java-sdk.md) (Eindgebruikersverificatie met Data Lake Store met behulp van Java) voor de verificatie van eindgebruikers voor uw toepassing.
* Zie [Service-to-service authentication with Data Lake Store using Java](data-lake-store-service-to-service-authenticate-java.md) (Service-naar-serviceverificatie met Data Lake Store met behulp van Java) voor service-naar-serviceverificatie voor uw toepassing.

## <a name="create-an-azure-data-lake-store-client"></a>Een Azure Data Lake Store-client maken
Voor het maken van een [ADLStoreClient](https://azure.github.io/azure-data-lake-store-java/javadoc/)-object moet u de Data Lake Store-accountnaam en de tokenprovider opgeven die zijn gegenereerd tijdens uw verificatie bij Data Lake Store (zie de sectie [Verificatie](#authentication)). De Data Lake Store-accountnaam moet een volledig gekwalificeerde domeinnaam zijn. Vervang bijvoorbeeld **FILL-IN-HERE** met iets als **mydatalakestore.azuredatalakestore.net**.

    private static String accountFQDN = "FILL-IN-HERE";  // full account FQDN, not just the account name
    ADLStoreClient client = ADLStoreClient.createClient(accountFQDN, provider);

De codefragmenten in de volgende secties bevatten voorbeelden van enkele veelvoorkomende bestandssysteembewerkingen. Bekijk de volledige [Data Lake Store Java SDK API-documenten](https://azure.github.io/azure-data-lake-store-java/javadoc/) van het **ADLStoreClient**-object om andere bewerkingen te bekijken.

## <a name="create-a-directory"></a>Een map maken

In het volgende codefragment wordt een mapstructuur gemaakt in de hoofdmap van het Data Lake Store-account dat u hebt opgegeven.

    // create directory
    client.createDirectory("/a/b/w");
    System.out.println("Directory created.");

## <a name="create-a-file"></a>Een bestand maken

In het volgende codefragment wordt een bestand (c.txt) in de mapstructuur gemaakt en worden er gegevens naar het bestand geschreven.

    // create file and write some content
    String filename = "/a/b/c.txt";
    OutputStream stream = client.createFile(filename, IfExists.OVERWRITE  );
    PrintStream out = new PrintStream(stream);
    for (int i = 1; i <= 10; i++) {
        out.println("This is line #" + i);
        out.format("This is the same line (%d), but using formatted output. %n", i);
    }
    out.close();
    System.out.println("File created.");

U kunt ook een bestand (d.txt) maken met behulp van bytematrices.

    // create file using byte arrays
    stream = client.createFile("/a/b/d.txt", IfExists.OVERWRITE);
    byte[] buf = getSampleContent();
    stream.write(buf);
    stream.close();
    System.out.println("File created using byte array.");

De definitie van functie `getSampleContent` die in het bovenstaande codefragment wordt gebruikt, is beschikbaar als onderdeel van het voorbeeld [op GitHub](https://azure.microsoft.com/documentation/samples/data-lake-store-java-upload-download-get-started/). 

## <a name="append-to-a-file"></a>Toevoegen aan een bestand

In het volgende codefragment wordt inhoud toegevoegd aan een bestaand bestand.

    // append to file
    stream = client.getAppendStream(filename);
    stream.write(getSampleContent());
    stream.close();
    System.out.println("File appended.");

De definitie van functie `getSampleContent` die in het bovenstaande codefragment wordt gebruikt, is beschikbaar als onderdeel van het voorbeeld [op GitHub](https://azure.microsoft.com/documentation/samples/data-lake-store-java-upload-download-get-started/).

## <a name="read-a-file"></a>Een bestand lezen

In het volgende codefragment wordt inhoud gelezen uit een bestand in het Data Lake Store-account.

    // Read File
    InputStream in = client.getReadStream(filename);
    BufferedReader reader = new BufferedReader(new InputStreamReader(in));
    String line;
    while ( (line = reader.readLine()) != null) {
        System.out.println(line);
    }
    reader.close();
    System.out.println();
    System.out.println("File contents read.");

## <a name="concatenate-files"></a>Bestanden samenvoegen

In het volgende codefragment worden twee bestanden in het Data Lake Store-account samengevoegd. Als dit lukt, worden de twee bestaande bestanden vervangen door het samengevoegde bestand.

    // concatenate the two files into one
    List<String> fileList = Arrays.asList("/a/b/c.txt", "/a/b/d.txt");
    client.concatenateFiles("/a/b/f.txt", fileList);
    System.out.println("Two files concatenated into a new file.");

## <a name="rename-a-file"></a>De naam van een bestand wijzigen

In het volgende codefragment wordt de naam gewijzigd van een bestand in het Data Lake Store-account.

    //rename the file
    client.rename("/a/b/f.txt", "/a/b/g.txt");
    System.out.println("New file renamed.");

## <a name="get-metadata-for-a-file"></a>Metagegevens ophalen voor een bestand

In het volgende codefragment worden de metagegevens opgehaald voor een bestand in het Data Lake Store-account.

    // get file metadata
    DirectoryEntry ent = client.getDirectoryEntry(filename);
    printDirectoryInfo(ent);
    System.out.println("File metadata retrieved.");

## <a name="set-permissions-on-a-file"></a>Machtigingen instellen voor een bestand

In het volgende codefragment worden machtigingen ingesteld voor het bestand dat u in de vorige sectie hebt gemaakt.

    // set file permission
    client.setPermission(filename, "744");
    System.out.println("File permission set.");

## <a name="list-directory-contents"></a>Mapinhoud weergeven

In het volgende codefragment wordt recursief de inhoud van een map weergegeven.

    // list directory contents
    List<DirectoryEntry> list = client.enumerateDirectory("/a/b", 2000);
    System.out.println("Directory listing for directory /a/b:");
    for (DirectoryEntry entry : list) {
        printDirectoryInfo(entry);
    }
    System.out.println("Directory contents listed.");

De definitie van functie `printDirectoryInfo` die in het bovenstaande codefragment wordt gebruikt, is beschikbaar als onderdeel van het voorbeeld [op GitHub](https://azure.microsoft.com/documentation/samples/data-lake-store-java-upload-download-get-started/).

## <a name="delete-files-and-folders"></a>Bestanden en mappen verwijderen

In het volgende fragment worden recursief de opgegeven bestanden en mappen in een Data Lake Store-account verwijderd.

    // delete directory along with all the subdirectories and files in it
    client.deleteRecursive("/a");
    System.out.println("All files and folders deleted recursively");
    promptEnterKey();

## <a name="build-and-run-the-application"></a>De toepassing bouwen en uitvoeren.
1. Klik op de knop **Uitvoeren** om de toepassing vanuit een IDE uit te voeren. Gebruik [exec:exec](http://www.mojohaus.org/exec-maven-plugin/exec-mojo.html) om deze vanuit Maven uit te voeren.
2. Als u een afzonderlijke jar wilt maken die u vanuit de opdrachtregel kunt uitvoeren, bouwt u de jar met alle afhankelijkheden geïntegreerd met behulp van de [Maven assembly-invoegtoepassing](http://maven.apache.org/plugins/maven-assembly-plugin/usage.html). De pom.xml in de [voorbeeldbroncode van GitHub](https://github.com/Azure-Samples/data-lake-store-java-upload-download-get-started/blob/master/pom.xml) bevat een voorbeeld.

## <a name="next-steps"></a>Volgende stappen
* [JavaDoc verkennen voor de Java-SDK](https://azure.github.io/azure-data-lake-store-java/javadoc/)
* [Gegevens in Data Lake Store beveiligen](data-lake-store-secure-data.md)


