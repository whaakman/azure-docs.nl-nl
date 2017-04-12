---
title: De Java-SDK gebruiken om toepassingen te ontwikkelen in Azure Data Lake Store | Microsoft Docs
description: De Java-SDK van Azure Data Lake Store gebruiken om een Data Lake Store-account te maken en basisbewerkingen in Data Lake Store uit te voeren
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
ms.date: 04/03/2017
ms.author: nitinme
translationtype: Human Translation
ms.sourcegitcommit: 988e7fe2ae9f837b661b0c11cf30a90644085e16
ms.openlocfilehash: 7e1d596739e6c548349827ff79b76cc0312bc4df
ms.lasthandoff: 04/06/2017


---
# <a name="get-started-with-azure-data-lake-store-using-java"></a>Aan de slag met Azure Data Lake Store met Java
> [!div class="op_single_selector"]
> * [Portal](data-lake-store-get-started-portal.md)
> * [PowerShell](data-lake-store-get-started-powershell.md)
> * [.NET-SDK](data-lake-store-get-started-net-sdk.md)
> * [Java-SDK](data-lake-store-get-started-java-sdk.md)
> * [REST-API](data-lake-store-get-started-rest-api.md)
> * [Azure CLI](data-lake-store-get-started-cli.md)
> * [Azure CLI 2.0](data-lake-store-get-started-cli-2.0.md)
> * [Node.js](data-lake-store-manage-use-nodejs.md)
> * [Python](data-lake-store-get-started-python.md)
>
> 

Lees hoe u met de Azure Data Lake Store Java SDK basisbewerkingen uitvoert, zoals het maken van mappen, het uploaden en downloaden van gegevensbestanden enzovoort. Zie [Azure Data Lake Store](data-lake-store-overview.md) voor meer informatie over Data Lake.

U kunt de Java SDK API-documenten voor Azure Data Lake Store openen via [Azure Data Lake Store Java API-documenten](https://azure.github.io/azure-data-lake-store-java/javadoc/).

## <a name="prerequisites"></a>Vereisten
* Java Development Kit (JDK 7 of hoger met Java versie 1.7 of hoger)
* Azure Data Lake Store-account. Volg de instructies in [Aan de slag met Azure Data Lake Store met Azure Portal](data-lake-store-get-started-portal.md).
* [Maven](https://maven.apache.org/install.html). In deze zelfstudie wordt Maven gebruikt voor build- en projectafhankelijkheden. Hoewel het mogelijk is te ontwikkelen zonder een buildsysteem als Maven of Gradle, maken deze systemen het veel eenvoudiger om afhankelijkheden te beheren.
* (Optioneel) Een IDE zoals [IntelliJ IDEA](https://www.jetbrains.com/idea/download/), [Eclipse](https://www.eclipse.org/downloads/) of vergelijkbaar.

## <a name="how-do-i-authenticate-using-azure-active-directory"></a>Hoe verifieer ik met Azure Active Directory?
In deze zelfstudie wordt een clientgeheim in de Azure AD-toepassing gebruikt om een Azure Active Directory-token (service-naar-serviceverificatie) op te halen. Dit token wordt gebruikt om een Data Lake Store-clientobject te maken voor het uitvoeren van bestand- en mapbewerkingen. Voer de volgende high-level stappen uit om de verificatie uit te voeren met het clientgeheim in Azure Data Lake Store:

1. Een Azure AD-webtoepassing maken
2. Haal de client-id, het client-geheim en het tokeneindpunt voor de Azure AD-webtoepassing op.
3. Configureer de toegang tot de Azure AD-webtoepassing in het/de Data Lake Store-bestand/-map dat/die u wilt openen vanuit de Java-toepassing die u maakt.

Zie [Een Active Directory-toepassing maken](data-lake-store-authenticate-using-active-directory.md) voor instructies om deze stappen uit te voeren.

Azure Active Directory biedt andere opties naast het ophalen van een token. U kunt uit verschillende verificatiemechanismen kiezen aan de hand van uw scenario, bijvoorbeeld een toepassing die wordt uitgevoerd in een browser, een toepassing die wordt gedistribueerd als een bureaubladtoepassing of een servertoepassing die on-premises of in een virtuele Azure-machine wordt uitgevoerd. U kunt ook uit verschillende soorten referenties kiezen, zoals wachtwoorden, certificaten, tweeledige authenticatie enzovoort. Daarnaast stelt Azure Active Directory u in staat uw on-premises Active Directory-gebruikers te synchroniseren met de cloud. Zie [Verificatiescenario's voor Azure Active Directory](../active-directory/active-directory-authentication-scenarios.md) voor meer informatie. 

## <a name="create-a-java-application"></a>Een Java-toepassing maken
Dit codevoorbeeld beschikbaar [in GitHub](https://azure.microsoft.com/documentation/samples/data-lake-store-java-upload-download-get-started/) doorloopt het proces waarin bestanden in het archief worden gemaakt, bestanden worden samengevoegd, een bestand wordt gedownload en een aantal bestanden uit het archief wordt verwijderd. In dit gedeelte van het artikel komen de belangrijkste onderdelen van de code aan bod.

1. Maak een Maven-project met behulp van [mvn archetype](https://maven.apache.org/guides/getting-started/maven-in-five-minutes.html) vanuit de opdrachtregel of met behulp van een IDE. [Hier](https://www.jetbrains.com/help/idea/2016.1/creating-and-running-your-first-java-application.html) vindt u instructies over het maken van een Java-project met behulp van IntelliJ. [Hier](http://help.eclipse.org/mars/index.jsp?topic=%2Forg.eclipse.jdt.doc.user%2FgettingStarted%2Fqs-3.htm) vindt u instructies over het maken van een Java-project met behulp van Eclipse. 
2. Voeg de volgende afhankelijkheden toe aan het **pom.xml**-bestand in Maven. Voeg het volgende tekstfragment toe tussen de tag **\</version>** en de tag **\</project>**:
   
        <dependencies>
          <dependency>
            <groupId>com.microsoft.azure</groupId>
            <artifactId>azure-data-lake-store-sdk</artifactId>
            <version>2.1.4</version>
          </dependency>
          <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-nop</artifactId>
            <version>1.7.21</version>
          </dependency>
        </dependencies>
   
    De eerste afhankelijkheid is om Data Lake Store SDK (`azure-data-lake-store-sdk`) vanuit de Maven-opslag te gebruiken. De tweede afhankelijkheid (`slf4j-nop`) is om aan te geven welk framework voor logboekregistratie moet worden gebruikt voor deze toepassing. De Data Lake Store SDK gebruikt een [slf4j](http://www.slf4j.org/)-façade voor logboekregistratie, waarmee u uit een aantal populaire frameworks voor logboekregistratie, zoals log4j, Java logging, logback enzovoort, of voor geen logboekregistratie kunt kiezen. In dit voorbeeld wordt logboekregistratie uitgeschakeld. Daarom wordt de **slf4j-nop**-binding gebruikt. [Hier](http://www.slf4j.org/manual.html#projectDep) vindt u andere opties voor logboekregistratie voor uw toepassing.

### <a name="add-the-application-code"></a>De toepassingscode toevoegen
Er zijn drie belangrijke onderdelen voor het toevoegen van de code.

1. De Azure Active Directory-token verkrijgen
2. Het token gebruiken om een Data Lake Store-client te maken.
3. De Data Lake Store-client gebruiken om bewerkingen uit te voeren.

#### <a name="step-1-obtain-an-azure-active-directory-token"></a>Stap 1: het Azure Active Directory-token verkrijgen.
De Data Lake Store SDK biedt handige methoden om de beveiligingstokens te beheren die nodig zijn om te communiceren met het Data Lake Store-account. Dit zijn echter niet de enige methoden die met SDK kunnen worden gebruikt. U kunt elke andere methode voor het verkrijgen van een token gebruiken. Zo kunt u de [Azure Active Directory SDK](https://github.com/AzureAD/azure-activedirectory-library-for-java) gebruiken, of uw persoonlijke code.

Als u de Data Lake Store SDK wilt gebruiken om het token te verkrijgen voor de Active Directory-webtoepassing die u eerder hebt gemaakt, gebruikt u een van de subklassen `AccessTokenProvider` (in het onderstaande voorbeeld wordt `ClientCredsTokenProvider` gebruikt). De tokenprovider slaat de referenties die worden gebruikt voor het ophalen van het token, op in de cache en vernieuwt het token automatisch als het bijna is verlopen. Het is mogelijk om uw eigen subklassen te maken van `AccessTokenProvider`, zodat tokens worden opgehaald door de code van de klant. In dit voorbeeld gebruiken we voor het gemak het token dat is opgegeven in de SDK.

Vervang **FILL-IN-HERE** met de daadwerkelijke waarden voor de Azure Active Directory-webtoepassing.

    private static String clientId = "FILL-IN-HERE";
    private static String authTokenEndpoint = "FILL-IN-HERE";
    private static String clientKey = "FILL-IN-HERE";

    AccessTokenProvider provider = new ClientCredsTokenProvider(authTokenEndpoint, clientId, clientKey);

#### <a name="step-2-create-an-azure-data-lake-store-client-adlstoreclient-object"></a>Stap 2: een Azure Data Lake Store-clientobject (ADLStoreClient-object) verkrijgen
Voor het maken van een [ADLStoreClient](https://azure.github.io/azure-data-lake-store-java/javadoc/)-object moet u de Data Lake Store-accountnaam en de tokenprovider opgeven die u in de vorige stap hebt gemaakt. De Data Lake Store-accountnaam moet een volledig gekwalificeerde domeinnaam zijn. Vervang bijvoorbeeld **FILL-IN-HERE** met iets als **mydatalakestore.azuredatalakestore.net**.

    private static String accountFQDN = "FILL-IN-HERE";  // full account FQDN, not just the account name
    ADLStoreClient client = ADLStoreClient.createClient(accountFQDN, provider);

### <a name="step-3-use-the-adlstoreclient-to-perform-file-and-directory-operations"></a>Stap 3: de ADLStoreClient gebruiken om bestand- en mapbewerkingen uit te voeren
De code hieronder bevat voorbeelden van tekstfragmenten van een aantal veelvoorkomende bewerkingen. Bekijk de volledige [Data Lake Store Java SDK API-documenten](https://azure.github.io/azure-data-lake-store-java/javadoc/) van het **ADLStoreClient**-object om andere bewerkingen te bekijken.

Bestanden worden gelezen en geschreven met behulp van standaard Java-streams. Dit betekent dat u een willekeurige Java-stream gelaagd bovenop de Data Lake Store-streams kunt plaatsen om van standaard Java-functionaliteit te profiteren (bijv. printstreams voor uitvoer met opmaak, of een van de compressie- of versleutelingsstreams voor extra functionaliteit enzovoort).

     // create file and write some content
     String filename = "/a/b/c.txt";
     OutputStream stream = client.createFile(filename, IfExists.OVERWRITE  );
     PrintStream out = new PrintStream(stream);
     for (int i = 1; i <= 10; i++) {
         out.println("This is line #" + i);
         out.format("This is the same line (%d), but using formatted output. %n", i);
     }
     out.close();
    
    // set file permission
    client.setPermission(filename, "744");

    // append to file
    stream = client.getAppendStream(filename);
    stream.write(getSampleContent());
    stream.close();

    // Read File
    InputStream in = client.getReadStream(filename);
    byte[] b = new byte[64000];
    while (in.read(b) != -1) {
        System.out.write(b);
    }
    in.close();

    // concatenate the two files into one
    List<String> fileList = Arrays.asList("/a/b/c.txt", "/a/b/d.txt");
    client.concatenateFiles("/a/b/f.txt", fileList);

    //rename the file
    client.rename("/a/b/f.txt", "/a/b/g.txt");

    // list directory contents
    List<DirectoryEntry> list = client.enumerateDirectory("/a/b", 2000);
    System.out.println("Directory listing for directory /a/b:");
    for (DirectoryEntry entry : list) {
        printDirectoryInfo(entry);
    }

    // delete directory along with all the subdirectories and files in it
    client.deleteRecursive("/a");

#### <a name="step-4-build-and-run-the-application"></a>Stap 4: de toepassing bouwen en uitvoeren
1. Klik op de knop **Uitvoeren** om de toepassing vanuit een IDE uit te voeren. Gebruik [exec:exec](http://www.mojohaus.org/exec-maven-plugin/exec-mojo.html) om deze vanuit Maven uit te voeren.
2. Als u een afzonderlijke jar wilt maken die u vanuit de opdrachtregel kunt uitvoeren, bouwt u de jar met alle afhankelijkheden geïntegreerd met behulp van de [Maven assembly-invoegtoepassing](http://maven.apache.org/plugins/maven-assembly-plugin/usage.html). De pom.xml in de [voorbeeldbroncode van GitHub](https://github.com/Azure-Samples/data-lake-store-java-upload-download-get-started/blob/master/pom.xml) bevat een voorbeeld van hoe u dat doet.

## <a name="next-steps"></a>Volgende stappen
* [JavaDoc verkennen voor de Java-SDK](https://azure.github.io/azure-data-lake-store-java/javadoc/)
* [Gegevens in Data Lake Store beveiligen](data-lake-store-secure-data.md)
* [Azure Data Lake Analytics gebruiken met Data Lake Store](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Azure HDInsight gebruiken met Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md)


