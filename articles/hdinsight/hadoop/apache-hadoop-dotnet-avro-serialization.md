---
title: Serialiseer gegevens in Hadoop - Library Microsoft Avro - Azure
description: Informatie over het serialiseren en deserialiseren van gegevens in Hadoop op HDInsight met behulp van de Microsoft Avro Library om vast te leggen met geheugen, een database of bestand.
keywords: avro, hadoop avro
services: hdinsight
author: jasonwhowell
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 05/16/2018
ms.author: jasonh
ms.custom: hdiseo17may2017
ms.openlocfilehash: ad8d0392b4b0e1e9e8d94e0d5a7a0c72455ecfcb
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/05/2018
ms.locfileid: "43700994"
---
# <a name="serialize-data-in-hadoop-with-the-microsoft-avro-library"></a>Gegevens in Hadoop met het Microsoft Avro Library serialiseren

>[!NOTE]
>De Avro-SDK wordt niet meer ondersteund door Microsoft. De bibliotheek is open source-community ondersteund. De bronnen voor de bibliotheek bevinden zich in [Github](https://github.com/Azure/azure-sdk-for-net/tree/master/src/ServiceManagement/HDInsight/Microsoft.Hadoop.Avro).

In dit onderwerp ziet u hoe u de [Microsoft Avro Library](https://github.com/Azure/azure-sdk-for-net/tree/master/src/ServiceManagement/HDInsight/Microsoft.Hadoop.Avro) aan objecten en andere gegevensstructuren serialiseren in stromen om vast te leggen ze naar geheugen, een database of een bestand. Ook ziet u hoe ze om te herstellen van de oorspronkelijke objecten deserialiseren.

[!INCLUDE [windows-only](../../../includes/hdinsight-windows-only.md)]

## <a name="apache-avro"></a>Apache Avro
De <a href="https://hadoopsdk.codeplex.com/wikipage?title=Avro%20Library" target="_blank">Microsoft Avro Library</a> het Apache Avro gegevens serialisatie system voor de omgeving Microsoft.NET implementeert. Apache Avro biedt een compacte binaire gegevens DIF-indeling voor de serialisatie. Hierbij <a href="http://www.json.org" target="_blank">JSON</a> voor het definiëren van een taal-agnostisch schema waarmee taalherkenning interoperabiliteit van talen. Gegevens die zijn geserialiseerd in één taal kunnen worden gelezen in een andere. C, C++, C#, Java, PHP, Python en Ruby worden momenteel ondersteund. Gedetailleerde informatie over deze indeling vindt u de <a href="http://avro.apache.org/docs/current/spec.html" target="_blank">Apache Avro-specificatie</a>. 

>[!NOTE]
>De Microsoft Avro Library biedt geen ondersteuning voor het deel van de externe procedure aanroepen (RPC's) van deze specificatie.
>

De geserialiseerde representatie van een object in het Avro-systeem bestaat uit twee delen: schema en de werkelijke waarde. De Avro-schema beschrijving van het gegevensmodel van taal-onafhankelijk van de geserialiseerde gegevens met JSON. Deze functionaliteit wordt weergegeven naast een binaire weergave van gegevens. Met het schema gescheiden van de binaire weergave van toestaat elk object dat moet worden geschreven met geen overhead per waarde, waardoor snelle serialisatie en de weergave kleine.

## <a name="the-hadoop-scenario"></a>Het Hadoop-scenario
De Apache Avro serialisatie-indeling wordt veel gebruikt in Azure HDInsight en andere Apache Hadoop-omgevingen. Avro biedt een handige manier om weer te geven complexe gegevensstructuren binnen een Hadoop-MapReduce-taak. De indeling van Avro-bestanden (Avro-object container-bestand) is ontworpen ter ondersteuning van het gedistribueerde MapReduce-programmeermodel. De belangrijkste functie waarmee de distributie is dat de bestanden "splitsbare' in de zin dat één kunt elk in een bestand punt en beginnen met lezen vanaf een bepaald blok.

## <a name="serialization-in-avro-library"></a>Serialisatie in Avro-bibliotheek
De .NET-bibliotheek voor Avro ondersteunt twee manieren om serialiseren objecten:

* **Reflectie** -de JSON-schema voor de typen automatisch gebouwd van de gegevens contract kenmerken van de .NET-typen worden geserialiseerd.
* **algemene record** -een JSON-schema is expliciet worden opgegeven in een record die wordt vertegenwoordigd door de [ **AvroRecord** ](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) klasse wanneer er geen .NET-typen zijn aanwezig om te beschrijven van het schema voor de gegevens worden geserialiseerd.

Wanneer het schema is bekend bij de schrijver en de lezer van de stroom, kunnen de gegevens worden verzonden zonder dat bijbehorende schema. Wanneer een Avro-object container-bestand wordt gebruikt, wordt het schema in gevallen opgeslagen in het bestand. Andere parameters, zoals de codec gebruikt voor de compressie van gegevens kunnen worden opgegeven. Deze scenario's worden in meer detail beschreven en weergegeven in de volgende codevoorbeelden:

## <a name="install-avro-library"></a>Avro-bibliotheek installeren
De onderstaande onderdelen vereist voordat u de bibliotheek installeert:

* <a href="http://www.microsoft.com/download/details.aspx?id=17851" target="_blank">Microsoft .NET Framework 4</a>
* <a href="http://james.newtonking.com/json" target="_blank">Newtonsoft Json.NET</a> (6.0.4 of hoger)

Houd er rekening mee dat de afhankelijkheid Newtonsoft.Json.dll automatisch worden gedownload met de installatie van de Microsoft Avro Library. De procedure is opgegeven in de volgende sectie:

De Microsoft Avro Library wordt gedistribueerd als een NuGet-pakket dat kan worden geïnstalleerd vanuit Visual Studio via de volgende procedure:

1. Selecteer de **Project** tabblad -> **NuGet-pakketten beheren...**
2. Zoeken naar "Microsoft.Hadoop.Avro" in de **Online zoeken** vak.
3. Klik op de **installeren** naast **Avro-bibliotheek van Microsoft Azure HDInsight**.

Houd er rekening mee dat de Newtonsoft.Json.dll (> = 6.0.4) afhankelijkheid is ook automatisch gedownload met de Microsoft Avro Library.

De Microsoft Avro Library-broncode is beschikbaar op [Github](https://github.com/Azure/azure-sdk-for-net/tree/master/src/ServiceManagement/HDInsight/Microsoft.Hadoop.Avro).

## <a name="compile-schemas-using-avro-library"></a>Schema's met Avro-bibliotheek gecompileerd
De Microsoft Avro Library bevat een hulpprogramma voor het genereren van code waarmee het maken van C#-typen automatisch op basis van de vooraf gedefinieerde JSON-schema. Het hulpprogramma voor het genereren van code niet wordt gedistribueerd als een binaire kan worden uitgevoerd, maar kan eenvoudig worden gemaakt via de volgende procedure:

1. Download het ZIP-bestand met de meest recente versie van HDInsight SDK-broncode uit de <a href="http://hadoopsdk.codeplex.com/SourceControl/latest#" target="_blank">Microsoft .NET SDK voor Hadoop</a>. (Klik op de **downloaden** pictogram, niet de **Downloads** tabblad.)
2. De HDInsight SDK uitpakken naar een map op de computer met .NET Framework 4 geïnstalleerd en die zijn verbonden met Internet om vereiste afhankelijkheid NuGet-pakketten te downloaden. Hieronder ziet, gaan we ervan uit dat de broncode wordt uitgepakt naar C:\SDK.
3. Ga naar de map C:\SDK\src\Microsoft.Hadoop.Avro.Tools en build.bat uitvoeren. (Het bestand aanroepen MSBuild van de 32-bits-distributie van het .NET Framework. Als u wilt de 64-bits versie te gebruiken, bewerk build.bat, de opmerkingen in het bestand te volgen.) Zorg ervoor dat de build voltooid is. (Op sommige systemen MSBuild kan leiden tot waarschuwingen. Deze waarschuwingen hebben geen invloed op het hulpprogramma, zolang er geen fouten build zijn.)
4. Het gecompileerde hulpprogramma bevindt zich in C:\SDK\Bin\Unsigned\Release\Microsoft.Hadoop.Avro.Tools.

Als u bekend bent met de syntaxis van de opdrachtregel, dan de volgende opdracht uit de map waar het hulpprogramma voor het genereren van code zich bevindt: `Microsoft.Hadoop.Avro.Tools help /c:codegen`

Als u wilt testen van het hulpprogramma, kunt u C#-klassen genereren van de JSON-schema-voorbeeldbestand geleverd met de broncode. Voer de volgende opdracht uit:

    Microsoft.Hadoop.Avro.Tools codegen /i:C:\SDK\src\Microsoft.Hadoop.Avro.Tools\SampleJSON\SampleJSONSchema.avsc /o:

Dit moet voor het produceren van twee C#-bestanden in de huidige map: SensorData.cs en Location.cs.

Voor inzicht in de logica die door het hulpprogramma voor het genereren van code wordt gebruikt tijdens het converteren van het JSON-schema voor C#-typen, raadpleegt u het bestand GenerationVerification.feature in C:\SDK\src\Microsoft.Hadoop.Avro.Tools\Doc.

Naamruimten worden geëxtraheerd uit de JSON-schema met behulp van de logica in het bestand dat is vermeld in de vorige alinea. Naamruimten die zijn geëxtraheerd uit het schema hebben voorrang op de wat is opgegeven met de parameter/n in de opdrachtregel hulpprogramma. Als u overschrijven van de naamruimten die zijn opgenomen in het schema wilt, moet u de parameter /nf gebruiken. Bijvoorbeeld als u wilt alle naamruimten van de SampleJSONSchema.avsc my.own.nspace, voert u de volgende opdracht uit:

    Microsoft.Hadoop.Avro.Tools codegen /i:C:\SDK\src\Microsoft.Hadoop.Avro.Tools\SampleJSON\SampleJSONSchema.avsc /o:. /nf:my.own.nspace

## <a name="about-the-samples"></a>Over de voorbeelden
Zes voorbeelden vindt u in dit onderwerp ziet u verschillende scenario's ondersteund door de Microsoft Avro Library. De Microsoft Avro Library is ontworpen voor gebruik met elke gewenste gegevensstroom. In deze voorbeelden wordt gegevens gemanipuleerd via geheugen gegevensstromen in plaats van bestand streams of databases voor eenvoud en consistentie. De benadering die zijn uitgevoerd in een productie-omgeving, is afhankelijk van de scenariovereisten voor de exacte, gegevensbron en volume, prestaties beperkingen en andere factoren.

De eerste twee voorbeelden laten zien hoe te serialiseren en deserialiseren van gegevens in stream geheugenbuffers via reflectie en algemene records. Het schema in deze twee gevallen wordt aangenomen dat moet worden gedeeld tussen de lezers en schrijvers out-of-band.

De derde en vierde voorbeelden laten zien hoe te serialiseren en deserialiseren van gegevens met behulp van de geselecteerde container van de avro-object. Wanneer gegevens worden opgeslagen in een Avro-container-bestand, worden altijd waarvan het schema is opgeslagen met het omdat het schema voor de deserialisatie moet worden gedeeld.

Het voorbeeld met de eerste vier voorbeelden kan worden gedownload vanaf de <a href="http://code.msdn.microsoft.com/Serialize-data-with-the-86055923" target="_blank">Azure-codevoorbeelden</a> site.

Het vijfde voorbeeld ziet hoe u een aangepaste compressiecodec voor container van de avro-object. Een voorbeeld met de code voor dit voorbeeld kan worden gedownload vanaf de <a href="http://code.msdn.microsoft.com/Serialize-data-with-the-67159111" target="_blank">Azure-codevoorbeelden</a> site.

Het zesde voorbeeld toont hoe u gegevens uploaden naar Azure Blob-opslag en analyseer ze met behulp van Hive met een HDInsight (Hadoop)-cluster met Avro-serialisatie. Deze kan worden gedownload vanaf de <a href="https://code.msdn.microsoft.com/Using-Avro-to-upload-data-ae81b1e3" target="_blank">Azure-codevoorbeelden</a> site.

Hier vindt u koppelingen naar de zes voorbeelden die in het onderwerp worden besproken:

* <a href="#Scenario1">**Serialisatie met reflectie** </a> -de JSON-schema voor typen worden geserialiseerd automatisch gebouwd van de gegevens contract kenmerken.
* <a href="#Scenario2">**Serialisatie met algemene record** </a> -de JSON-schema is expliciet worden opgegeven in een record wanneer er geen .NET-type beschikbaar voor reflectie is.
* <a href="#Scenario3">**Met behulp van object containerbestanden met reflectie serialisatie** </a> -de JSON-schema is automatisch gemaakt en heeft gedeeld, samen met de geserialiseerde gegevens via een Avro-object container-bestand.
* <a href="#Scenario4">**Met behulp van object containerbestanden met algemene record serialisatie** </a> -de JSON-schema expliciet is opgegeven voor de serialisatie en gedeeld, samen met de gegevens via een Avro-object container-bestand.
* <a href="#Scenario5">**Met behulp van containerbestanden van object met een aangepaste compressiecodec serialisatie** </a> -het voorbeeld ziet u hoe u een Avro-object kontejner soubor maakt met een aangepaste .NET-implementatie van de compressiecodec Deflate-gegevens.
* <a href="#Scenario6">**Het uploaden van gegevens voor de Microsoft Azure HDInsight-service met behulp van Avro** </a> -het voorbeeld ziet u hoe de Avro-serialisatie communiceert met de HDInsight-service. Een actief Azure-abonnement en de toegang tot een Azure HDInsight-cluster zijn vereist om uit te voeren in het volgende voorbeeld.

## <a name="Scenario1"></a>Voorbeeld 1: Serialisatie met reflectie
De JSON-schema voor de typen kan worden automatisch gebouwd door de Microsoft Avro Library via reflectie van de gegevens contract kenmerken van de C#-objecten worden geserialiseerd. Hiermee maakt u de Microsoft Avro Library een [ **IAvroSeralizer<T>**  ](http://msdn.microsoft.com/library/dn627341.aspx) voor het identificeren van de velden die moeten worden geserialiseerd.

In dit voorbeeld-objecten (een **SensorData** klasse met een lid **locatie** struct) naar een stream geheugen worden geserialiseerd en deze stroom op zijn beurt is gedeserialiseerd. Het resultaat wordt vervolgens vergeleken met het eerste exemplaar om te bevestigen dat de **SensorData** object hersteld is identiek aan de oorspronkelijke.

Het schema in dit voorbeeld wordt aangenomen dat worden gedeeld tussen de lezers en schrijvers, zodat de container-indeling van Avro-object niet vereist is. Zie voor een voorbeeld van hoe u serialiseren en deserialiseren van gegevens in het, geheugenbuffers met behulp van reflectie met de indeling van de container object wanneer het schema moet worden gedeeld met de gegevens, <a href="#Scenario3">serialisatie met behulp van object containerbestanden met reflectie</a>.

    namespace Microsoft.Hadoop.Avro.Sample
    {
        using System;
        using System.Collections.Generic;
        using System.IO;
        using System.Linq;
        using System.Runtime.Serialization;
        using Microsoft.Hadoop.Avro.Container;
        using Microsoft.Hadoop.Avro;

        //Sample class used in serialization samples
        [DataContract(Name = "SensorDataValue", Namespace = "Sensors")]
        internal class SensorData
        {
            [DataMember(Name = "Location")]
            public Location Position { get; set; }

            [DataMember(Name = "Value")]
            public byte[] Value { get; set; }
        }

        //Sample struct used in serialization samples
        [DataContract]
        internal struct Location
        {
            [DataMember]
            public int Floor { get; set; }

            [DataMember]
            public int Room { get; set; }
        }

        //This class contains all methods demonstrating
        //the usage of Microsoft Avro Library
        public class AvroSample
        {

            //Serialize and deserialize sample data set represented as an object using reflection.
            //No explicit schema definition is required - schema of serialized objects is automatically built.
            public void SerializeDeserializeObjectUsingReflection()
            {

                Console.WriteLine("SERIALIZATION USING REFLECTION\n");
                Console.WriteLine("Serializing Sample Data Set...");

                //Create a new AvroSerializer instance and specify a custom serialization strategy AvroDataContractResolver
                //for serializing only properties attributed with DataContract/DateMember
                var avroSerializer = AvroSerializer.Create<SensorData>();

                //Create a memory stream buffer
                using (var buffer = new MemoryStream())
                {
                    //Create a data set by using sample class and struct
                    var expected = new SensorData { Value = new byte[] { 1, 2, 3, 4, 5 }, Position = new Location { Room = 243, Floor = 1 } };

                    //Serialize the data to the specified stream
                    avroSerializer.Serialize(buffer, expected);


                    Console.WriteLine("Deserializing Sample Data Set...");

                    //Prepare the stream for deserializing the data
                    buffer.Seek(0, SeekOrigin.Begin);

                    //Deserialize data from the stream and cast it to the same type used for serialization
                    var actual = avroSerializer.Deserialize(buffer);

                    Console.WriteLine("Comparing Initial and Deserialized Data Sets...");

                    //Finally, verify that deserialized data matches the original one
                    bool isEqual = this.Equal(expected, actual);

                    Console.WriteLine("Result of Data Set Identity Comparison is {0}", isEqual);

                }
            }

            //
            //Helper methods
            //

            //Comparing two SensorData objects
            private bool Equal(SensorData left, SensorData right)
            {
                return left.Position.Equals(right.Position) && left.Value.SequenceEqual(right.Value);
            }



            static void Main()
            {

                string sectionDivider = "---------------------------------------- ";

                //Create an instance of AvroSample Class and invoke methods
                //illustrating different serializing approaches
                AvroSample Sample = new AvroSample();

                //Serialization to memory using reflection
                Sample.SerializeDeserializeObjectUsingReflection();

                Console.WriteLine(sectionDivider);
                Console.WriteLine("Press any key to exit.");
                Console.Read();
            }
        }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING REFLECTION
    //
    // Serializing Sample Data Set...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // Result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.


## <a name="sample-2-serialization-with-a-generic-record"></a>Voorbeeld 2: Serialisatie met een algemene record
Een JSON-schema kan expliciet worden opgegeven in een algemene record wanneer reflectie kan niet worden gebruikt omdat de gegevens kan niet worden weergegeven via .NET-klassen aan een gegevenscontract. Deze methode is langzamer dan met behulp van reflectie. In dergelijke gevallen kan kan het schema voor de gegevens ook worden dynamisch is, dat wil zeggen, niet bekend bij het compileren. Gegevens die worden weergegeven als door komma's gescheiden waarden (CSV)-bestanden waarvan het schema onbekend is totdat deze wordt omgezet naar de Avro-indeling tijdens de uitvoering is een voorbeeld van dit soort dynamische scenario.

In dit voorbeeld laat zien hoe het maken en gebruiken een [ **AvroRecord** ](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) expliciet opgeven van een JSON-schema, hoe u deze vullen met de gegevens en hoe u serialiseren en deserialiseren. Het resultaat wordt vervolgens vergeleken met het eerste exemplaar om te bevestigen dat de record hersteld identiek aan de oorspronkelijke is.

Het schema in dit voorbeeld wordt aangenomen dat worden gedeeld tussen de lezers en schrijvers, zodat de container-indeling van Avro-object niet vereist is. Zie voor een voorbeeld van hoe u serialiseren en deserialiseren van gegevens in het, geheugenbuffers met behulp van een algemene record met de indeling van de container object wanneer het schema opgenomen in de geserialiseerde gegevens worden moet, de <a href="#Scenario4">serialisatie met behulp van de containerbestanden object met algemene record</a> voorbeeld.

    namespace Microsoft.Hadoop.Avro.Sample
    {
    using System;
    using System.Collections.Generic;
    using System.IO;
    using System.Linq;
    using System.Runtime.Serialization;
    using Microsoft.Hadoop.Avro.Container;
    using Microsoft.Hadoop.Avro.Schema;
    using Microsoft.Hadoop.Avro;

    //This class contains all methods demonstrating
    //the usage of Microsoft Avro Library
    public class AvroSample
    {

        //Serialize and deserialize sample data set by using a generic record.
        //A generic record is a special class with the schema explicitly defined in JSON.
        //All serialized data should be mapped to the fields of the generic record,
        //which in turn is then serialized.
        public void SerializeDeserializeObjectUsingGenericRecords()
        {
            Console.WriteLine("SERIALIZATION USING GENERIC RECORD\n");
            Console.WriteLine("Defining the Schema and creating Sample Data Set...");

            //Define the schema in JSON
            const string Schema = @"{
                                ""type"":""record"",
                                ""name"":""Microsoft.Hadoop.Avro.Specifications.SensorData"",
                                ""fields"":
                                    [
                                        {
                                            ""name"":""Location"",
                                            ""type"":
                                                {
                                                    ""type"":""record"",
                                                    ""name"":""Microsoft.Hadoop.Avro.Specifications.Location"",
                                                    ""fields"":
                                                        [
                                                            { ""name"":""Floor"", ""type"":""int"" },
                                                            { ""name"":""Room"", ""type"":""int"" }
                                                        ]
                                                }
                                        },
                                        { ""name"":""Value"", ""type"":""bytes"" }
                                    ]
                            }";

            //Create a generic serializer based on the schema
            var serializer = AvroSerializer.CreateGeneric(Schema);
            var rootSchema = serializer.WriterSchema as RecordSchema;

            //Create a memory stream buffer
            using (var stream = new MemoryStream())
            {
                //Create a generic record to represent the data
                dynamic location = new AvroRecord(rootSchema.GetField("Location").TypeSchema);
                location.Floor = 1;
                location.Room = 243;

                dynamic expected = new AvroRecord(serializer.WriterSchema);
                expected.Location = location;
                expected.Value = new byte[] { 1, 2, 3, 4, 5 };

                Console.WriteLine("Serializing Sample Data Set...");

                //Serialize the data
                serializer.Serialize(stream, expected);

                stream.Seek(0, SeekOrigin.Begin);

                Console.WriteLine("Deserializing Sample Data Set...");

                //Deserialize the data into a generic record
                dynamic actual = serializer.Deserialize(stream);

                Console.WriteLine("Comparing Initial and Deserialized Data Sets...");

                //Finally, verify the results
                bool isEqual = expected.Location.Floor.Equals(actual.Location.Floor);
                isEqual = isEqual && expected.Location.Room.Equals(actual.Location.Room);
                isEqual = isEqual && ((byte[])expected.Value).SequenceEqual((byte[])actual.Value);
                Console.WriteLine("Result of Data Set Identity Comparison is {0}", isEqual);
            }
        }

        static void Main()
        {

            string sectionDivider = "---------------------------------------- ";

            //Create an instance of AvroSample class and invoke methods
            //illustrating different serializing approaches
            AvroSample Sample = new AvroSample();

            //Serialization to memory using generic record
            Sample.SerializeDeserializeObjectUsingGenericRecords();

            Console.WriteLine(sectionDivider);
            Console.WriteLine("Press any key to exit.");
            Console.Read();
        }
    }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING GENERIC RECORD
    //
    // Defining the Schema and creating Sample Data Set...
    // Serializing Sample Data Set...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // Result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.


## <a name="sample-3-serialization-using-object-container-files-and-serialization-with-reflection"></a>Voorbeeld 3: Serialisatie object containerbestanden en serialisatie gebruiken met reflectie
In dit voorbeeld is vergelijkbaar met het scenario in de <a href="#Scenario1"> eerste voorbeeld</a>, waarbij het schema impliciet met weerspiegeling is opgegeven. Het verschil is dat hier, het schema wordt niet aangenomen dat bekend zijn bij de lezer die gedeserialiseerd. De **SensorData** objecten worden geserialiseerd en hun impliciet opgegeven schema worden opgeslagen in een containerbestand van een Avro-object dat wordt vertegenwoordigd door de [ **AvroContainer** ](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.avrocontainer.aspx) klasse.

De gegevens is geserialiseerd in dit voorbeeld met [ **SequentialWriter<SensorData>**  ](http://msdn.microsoft.com/library/dn627340.aspx) en gedeserialiseerd met [ **SequentialReader<SensorData>**  ](http://msdn.microsoft.com/library/dn627340.aspx). Het resultaat wordt vervolgens vergeleken met de eerste exemplaren om te controleren of de identiteit.

De gegevens in het bestand van de container object via de standaard wordt gecomprimeerd [ **Deflate** ] [ deflate-100] compressiecodec van .NET Framework 4. Zie de <a href="#Scenario5"> vijfde voorbeeld</a> in dit onderwerp voor informatie over het gebruik van een superieure en meer recente versie van de [ **Deflate** ] [ deflate-110] compressiecodec beschikbaar in .NET Framework 4.5.

    namespace Microsoft.Hadoop.Avro.Sample
    {
        using System;
        using System.Collections.Generic;
        using System.IO;
        using System.Linq;
        using System.Runtime.Serialization;
        using Microsoft.Hadoop.Avro.Container;
        using Microsoft.Hadoop.Avro;

        //Sample class used in serialization samples
        [DataContract(Name = "SensorDataValue", Namespace = "Sensors")]
        internal class SensorData
        {
            [DataMember(Name = "Location")]
            public Location Position { get; set; }

            [DataMember(Name = "Value")]
            public byte[] Value { get; set; }
        }

        //Sample struct used in serialization samples
        [DataContract]
        internal struct Location
        {
            [DataMember]
            public int Floor { get; set; }

            [DataMember]
            public int Room { get; set; }
        }

        //This class contains all methods demonstrating
        //the usage of Microsoft Avro Library
        public class AvroSample
        {

            //Serializes and deserializes the sample data set by using reflection and Avro object container files.
            //Serialized data is compressed with the Deflate codec.
            public void SerializeDeserializeUsingObjectContainersReflection()
            {

                Console.WriteLine("SERIALIZATION USING REFLECTION AND AVRO OBJECT CONTAINER FILES\n");

                //Path for Avro object container file
                string path = "AvroSampleReflectionDeflate.avro";

                //Create a data set by using sample class and struct
                var testData = new List<SensorData>
                        {
                            new SensorData { Value = new byte[] { 1, 2, 3, 4, 5 }, Position = new Location { Room = 243, Floor = 1 } },
                            new SensorData { Value = new byte[] { 6, 7, 8, 9 }, Position = new Location { Room = 244, Floor = 1 } }
                        };

                //Serializing and saving data to file.
                //Creating a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Serializing Sample Data Set...");

                    //Create a SequentialWriter instance for type SensorData, which can serialize a sequence of SensorData objects to stream.
                    //Data is compressed using the Deflate codec.
                    using (var w = AvroContainer.CreateWriter<SensorData>(buffer, Codec.Deflate))
                    {
                        using (var writer = new SequentialWriter<SensorData>(w, 24))
                        {
                            // Serialize the data to stream by using the sequential writer
                            testData.ForEach(writer.Write);
                        }
                    }

                    //Save stream to file
                    Console.WriteLine("Saving serialized data to file...");
                    if (!WriteFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }
                }

                //Reading and deserializing data.
                //Creating a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Reading data from file...");

                    //Reading data from object container file
                    if (!ReadFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }

                    Console.WriteLine("Deserializing Sample Data Set...");

                    //Prepare the stream for deserializing the data
                    buffer.Seek(0, SeekOrigin.Begin);

                    //Create a SequentialReader instance for type SensorData, which deserializes all serialized objects from the given stream.
                    //It allows iterating over the deserialized objects because it implements the IEnumerable<T> interface.
                    using (var reader = new SequentialReader<SensorData>(
                        AvroContainer.CreateReader<SensorData>(buffer, true)))
                    {
                        var results = reader.Objects;

                        //Finally, verify that deserialized data matches the original one
                        Console.WriteLine("Comparing Initial and Deserialized Data Sets...");
                        int count = 1;
                        var pairs = testData.Zip(results, (serialized, deserialized) => new { expected = serialized, actual = deserialized });
                        foreach (var pair in pairs)
                        {
                            bool isEqual = this.Equal(pair.expected, pair.actual);
                            Console.WriteLine("For Pair {0} result of Data Set Identity Comparison is {1}", count, isEqual);
                            count++;
                        }
                    }
                }

                //Delete the file
                RemoveFile(path);
            }

            //
            //Helper methods
            //

            //Comparing two SensorData objects
            private bool Equal(SensorData left, SensorData right)
            {
                return left.Position.Equals(right.Position) && left.Value.SequenceEqual(right.Value);
            }

            //Saving memory stream to a new file with the given path
            private bool WriteFile(MemoryStream InputStream, string path)
            {
                if (!File.Exists(path))
                {
                    try
                    {
                        using (FileStream fs = File.Create(path))
                        {
                            InputStream.Seek(0, SeekOrigin.Begin);
                            InputStream.CopyTo(fs);
                        }
                        return true;
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during creation and writing to the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                        return false;
                    }
                }
                else
                {
                    Console.WriteLine("Can not create file \"{0}\". File already exists", path);
                    return false;

                }
            }

            //Reading a file content by using the given path to a memory stream
            private bool ReadFile(MemoryStream OutputStream, string path)
            {
                try
                {
                    using (FileStream fs = File.Open(path, FileMode.Open))
                    {
                        fs.CopyTo(OutputStream);
                    }
                    return true;
                }
                catch (Exception e)
                {
                    Console.WriteLine("The following exception was thrown during reading from the file \"{0}\"", path);
                    Console.WriteLine(e.Message);
                    return false;
                }
            }

            //Deleting file by using given path
            private void RemoveFile(string path)
            {
                if (File.Exists(path))
                {
                    try
                    {
                        File.Delete(path);
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during deleting the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                    }
                }
                else
                {
                    Console.WriteLine("Can not delete file \"{0}\". File does not exist", path);
                }
            }

            static void Main()
            {

                string sectionDivider = "---------------------------------------- ";

                //Create an instance of AvroSample class and invoke methods
                //illustrating different serializing approaches
                AvroSample Sample = new AvroSample();

                //Serialization using reflection to Avro object container file
                Sample.SerializeDeserializeUsingObjectContainersReflection();

                Console.WriteLine(sectionDivider);
                Console.WriteLine("Press any key to exit.");
                Console.Read();
            }
        }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING REFLECTION AND AVRO OBJECT CONTAINER FILES
    //
    // Serializing Sample Data Set...
    // Saving serialized data to file...
    // Reading data from file...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // For Pair 1 result of Data Set Identity Comparison is True
    // For Pair 2 result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.


## <a name="sample-4-serialization-using-object-container-files-and-serialization-with-generic-record"></a>Voorbeeld 4: Serialisatie object containerbestanden en serialisatie gebruiken met algemene record
In dit voorbeeld is vergelijkbaar met het scenario in de <a href="#Scenario2"> tweede voorbeeld</a>, waarbij het schema expliciet is opgegeven met JSON. Het verschil is dat hier, het schema wordt niet aangenomen dat bekend zijn bij de lezer die gedeserialiseerd.

De testgegevensset zijn verzameld in een lijst met [ **AvroRecord** ](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) objecten via een expliciet gedefinieerde JSON-schema en vervolgens opgeslagen in een container-bestand van het object wordt vertegenwoordigd door de [  **AvroContainer** ](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.avrocontainer.aspx) klasse. Dit containerbestand maakt een writer dat wordt gebruikt voor het serialiseren van de gegevens, niet-gecomprimeerd, naar een geheugenstroom die vervolgens naar een bestand wordt opgeslagen. De [ **Codec.Null** ](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.codec.null.aspx) parameter die wordt gebruikt voor het maken van de lezer geeft aan dat deze gegevens niet worden gecomprimeerd.

De gegevens worden vervolgens uit het bestand gelezen en gedeserialiseerd naar een verzameling van objecten. Deze verzameling wordt vergeleken met de eerste lijst met Avro-records om te bevestigen dat ze identiek zijn.

    namespace Microsoft.Hadoop.Avro.Sample
    {
        using System;
        using System.Collections.Generic;
        using System.IO;
        using System.Linq;
        using System.Runtime.Serialization;
        using Microsoft.Hadoop.Avro.Container;
        using Microsoft.Hadoop.Avro.Schema;
        using Microsoft.Hadoop.Avro;

        //This class contains all methods demonstrating
        //the usage of Microsoft Avro Library
        public class AvroSample
        {

            //Serializes and deserializes a sample data set by using a generic record and Avro object container files.
            //Serialized data is not compressed.
            public void SerializeDeserializeUsingObjectContainersGenericRecord()
            {
                Console.WriteLine("SERIALIZATION USING GENERIC RECORD AND AVRO OBJECT CONTAINER FILES\n");

                //Path for Avro object container file
                string path = "AvroSampleGenericRecordNullCodec.avro";

                Console.WriteLine("Defining the Schema and creating Sample Data Set...");

                //Define the schema in JSON
                const string Schema = @"{
                                ""type"":""record"",
                                ""name"":""Microsoft.Hadoop.Avro.Specifications.SensorData"",
                                ""fields"":
                                    [
                                        {
                                            ""name"":""Location"",
                                            ""type"":
                                                {
                                                    ""type"":""record"",
                                                    ""name"":""Microsoft.Hadoop.Avro.Specifications.Location"",
                                                    ""fields"":
                                                        [
                                                            { ""name"":""Floor"", ""type"":""int"" },
                                                            { ""name"":""Room"", ""type"":""int"" }
                                                        ]
                                                }
                                        },
                                        { ""name"":""Value"", ""type"":""bytes"" }
                                    ]
                            }";

                //Create a generic serializer based on the schema
                var serializer = AvroSerializer.CreateGeneric(Schema);
                var rootSchema = serializer.WriterSchema as RecordSchema;

                //Create a generic record to represent the data
                var testData = new List<AvroRecord>();

                dynamic expected1 = new AvroRecord(rootSchema);
                dynamic location1 = new AvroRecord(rootSchema.GetField("Location").TypeSchema);
                location1.Floor = 1;
                location1.Room = 243;
                expected1.Location = location1;
                expected1.Value = new byte[] { 1, 2, 3, 4, 5 };
                testData.Add(expected1);

                dynamic expected2 = new AvroRecord(rootSchema);
                dynamic location2 = new AvroRecord(rootSchema.GetField("Location").TypeSchema);
                location2.Floor = 1;
                location2.Room = 244;
                expected2.Location = location2;
                expected2.Value = new byte[] { 6, 7, 8, 9 };
                testData.Add(expected2);

                //Serializing and saving data to file.
                //Create a MemoryStream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Serializing Sample Data Set...");

                    //Create a SequentialWriter instance for type SensorData, which can serialize a sequence of SensorData objects to stream.
                    //Data is not compressed (Null compression codec).
                    using (var writer = AvroContainer.CreateGenericWriter(Schema, buffer, Codec.Null))
                    {
                        using (var streamWriter = new SequentialWriter<object>(writer, 24))
                        {
                            // Serialize the data to stream by using the sequential writer
                            testData.ForEach(streamWriter.Write);
                        }
                    }

                    Console.WriteLine("Saving serialized data to file...");

                    //Save stream to file
                    if (!WriteFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }
                }

                //Reading and deserializing the data.
                //Create a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Reading data from file...");

                    //Reading data from object container file
                    if (!ReadFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }

                    Console.WriteLine("Deserializing Sample Data Set...");

                    //Prepare the stream for deserializing the data
                    buffer.Seek(0, SeekOrigin.Begin);

                    //Create a SequentialReader instance for type SensorData, which deserializes all serialized objects from the given stream.
                    //It allows iterating over the deserialized objects because it implements the IEnumerable<T> interface.
                    using (var reader = AvroContainer.CreateGenericReader(buffer))
                    {
                        using (var streamReader = new SequentialReader<object>(reader))
                        {
                            var results = streamReader.Objects;

                            Console.WriteLine("Comparing Initial and Deserialized Data Sets...");

                            //Finally, verify the results
                            var pairs = testData.Zip(results, (serialized, deserialized) => new { expected = (dynamic)serialized, actual = (dynamic)deserialized });
                            int count = 1;
                            foreach (var pair in pairs)
                            {
                                bool isEqual = pair.expected.Location.Floor.Equals(pair.actual.Location.Floor);
                                isEqual = isEqual && pair.expected.Location.Room.Equals(pair.actual.Location.Room);
                                isEqual = isEqual && ((byte[])pair.expected.Value).SequenceEqual((byte[])pair.actual.Value);
                                Console.WriteLine("For Pair {0} result of Data Set Identity Comparison is {1}", count, isEqual.ToString());
                                count++;
                            }
                        }
                    }
                }

                //Delete the file
                RemoveFile(path);
            }

            //
            //Helper methods
            //

            //Saving memory stream to a new file with the given path
            private bool WriteFile(MemoryStream InputStream, string path)
            {
                if (!File.Exists(path))
                {
                    try
                    {
                        using (FileStream fs = File.Create(path))
                        {
                            InputStream.Seek(0, SeekOrigin.Begin);
                            InputStream.CopyTo(fs);
                        }
                        return true;
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during creation and writing to the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                        return false;
                    }
                }
                else
                {
                    Console.WriteLine("Can not create file \"{0}\". File already exists", path);
                    return false;

                }
            }

            //Reading a file content by using the given path to a memory stream
            private bool ReadFile(MemoryStream OutputStream, string path)
            {
                try
                {
                    using (FileStream fs = File.Open(path, FileMode.Open))
                    {
                        fs.CopyTo(OutputStream);
                    }
                    return true;
                }
                catch (Exception e)
                {
                    Console.WriteLine("The following exception was thrown during reading from the file \"{0}\"", path);
                    Console.WriteLine(e.Message);
                    return false;
                }
            }

            //Deleting file by using the given path
            private void RemoveFile(string path)
            {
                if (File.Exists(path))
                {
                    try
                    {
                        File.Delete(path);
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during deleting the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                    }
                }
                else
                {
                    Console.WriteLine("Can not delete file \"{0}\". File does not exist", path);
                }
            }

            static void Main()
            {

                string sectionDivider = "---------------------------------------- ";

                //Create an instance of the AvroSample class and invoke methods
                //illustrating different serializing approaches
                AvroSample Sample = new AvroSample();

                //Serialization using generic record to Avro object container file
                Sample.SerializeDeserializeUsingObjectContainersGenericRecord();

                Console.WriteLine(sectionDivider);
                Console.WriteLine("Press any key to exit.");
                Console.Read();
            }
        }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING GENERIC RECORD AND AVRO OBJECT CONTAINER FILES
    //
    // Defining the Schema and creating Sample Data Set...
    // Serializing Sample Data Set...
    // Saving serialized data to file...
    // Reading data from file...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // For Pair 1 result of Data Set Identity Comparison is True
    // For Pair 2 result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.




## <a name="sample-5-serialization-using-object-container-files-with-a-custom-compression-codec"></a>Voorbeeld 5: Serialisatie met behulp van containerbestanden van object met een aangepaste compressiecodec
Het vijfde voorbeeld ziet hoe u een aangepaste compressiecodec voor container van de avro-object. Een voorbeeld met de code voor dit voorbeeld kan worden gedownload vanaf de [Azure-codevoorbeelden](http://code.msdn.microsoft.com/Serialize-data-with-the-67159111) site.

De [Avro-specificatie](http://avro.apache.org/docs/current/spec.html#Required+Codecs) kunt u het gebruik van een optioneel compressiecodec (in aanvulling op **Null** en **Deflate** standaardinstellingen). In dit voorbeeld is het niet implementeren van een nieuwe codec zoals Snappy (die worden vermeld als een ondersteunde optioneel compressiecodec in de [Avro-specificatie](http://avro.apache.org/docs/current/spec.html#snappy)). Het laat zien hoe u de .NET Framework 4.5-implementatie van de [ **Deflate** ] [ deflate-110] verbeterde codecs, waarmee u een betere compressiealgoritme op basis van de [zlib ](http://zlib.net/) compressiebibliotheek dan de standaard .NET Framework 4-versie.

    //
    // This code needs to be compiled with the parameter Target Framework set as ".NET Framework 4.5"
    // to ensure the desired implementation of the Deflate compression algorithm is used.
    // Ensure your C# project is set up accordingly.
    //

    namespace Microsoft.Hadoop.Avro.Sample
    {
        using System;
        using System.Collections.Generic;
        using System.Diagnostics;
        using System.IO;
        using System.IO.Compression;
        using System.Linq;
        using System.Runtime.Serialization;
        using Microsoft.Hadoop.Avro.Container;
        using Microsoft.Hadoop.Avro;

        #region Defining objects for serialization
        //Sample class used in serialization samples
        [DataContract(Name = "SensorDataValue", Namespace = "Sensors")]
        internal class SensorData
        {
            [DataMember(Name = "Location")]
            public Location Position { get; set; }

            [DataMember(Name = "Value")]
            public byte[] Value { get; set; }
        }

        //Sample struct used in serialization samples
        [DataContract]
        internal struct Location
        {
            [DataMember]
            public int Floor { get; set; }

            [DataMember]
            public int Room { get; set; }
        }
        #endregion

        #region Defining custom codec based on .NET Framework V.4.5 Deflate
        //Avro.NET codec class contains two methods,
        //GetCompressedStreamOver(Stream uncompressed) and GetDecompressedStreamOver(Stream compressed),
        //which are the key ones for data compression.
        //To enable a custom codec, one needs to implement these methods for the required codec.

        #region Defining Compression and Decompression Streams
        //DeflateStream (class from System.IO.Compression namespace that implements Deflate algorithm)
        //cannot be directly used for Avro because it does not support vital operations like Seek.
        //Thus one needs to implement two classes inherited from stream
        //(one for compressed and one for decompressed stream)
        //that use Deflate compression and implement all required features.
        internal sealed class CompressionStreamDeflate45 : Stream
        {
            private readonly Stream buffer;
            private DeflateStream compressionStream;

            public CompressionStreamDeflate45(Stream buffer)
            {
                Debug.Assert(buffer != null, "Buffer is not allowed to be null.");

                this.compressionStream = new DeflateStream(buffer, CompressionLevel.Fastest, true);
                this.buffer = buffer;
            }

            public override bool CanRead
            {
                get { return this.buffer.CanRead; }
            }

            public override bool CanSeek
            {
                get { return true; }
            }

            public override bool CanWrite
            {
                get { return this.buffer.CanWrite; }
            }

            public override void Flush()
            {
                this.compressionStream.Close();
            }

            public override long Length
            {
                get { return this.buffer.Length; }
            }

            public override long Position
            {
                get
                {
                    return this.buffer.Position;
                }

                set
                {
                    this.buffer.Position = value;
                }
            }

            public override int Read(byte[] buffer, int offset, int count)
            {
                return this.buffer.Read(buffer, offset, count);
            }

            public override long Seek(long offset, SeekOrigin origin)
            {
                return this.buffer.Seek(offset, origin);
            }

            public override void SetLength(long value)
            {
                throw new NotSupportedException();
            }

            public override void Write(byte[] buffer, int offset, int count)
            {
                this.compressionStream.Write(buffer, offset, count);
            }

            protected override void Dispose(bool disposed)
            {
                base.Dispose(disposed);

                if (disposed)
                {
                    this.compressionStream.Dispose();
                    this.compressionStream = null;
                }
            }
        }

        internal sealed class DecompressionStreamDeflate45 : Stream
        {
            private readonly DeflateStream decompressed;

            public DecompressionStreamDeflate45(Stream compressed)
            {
                this.decompressed = new DeflateStream(compressed, CompressionMode.Decompress, true);
            }

            public override bool CanRead
            {
                get { return true; }
            }

            public override bool CanSeek
            {
                get { return true; }
            }

            public override bool CanWrite
            {
                get { return false; }
            }

            public override void Flush()
            {
                this.decompressed.Close();
            }

            public override long Length
            {
                get { return this.decompressed.Length; }
            }

            public override long Position
            {
                get
                {
                    return this.decompressed.Position;
                }

                set
                {
                    throw new NotSupportedException();
                }
            }

            public override int Read(byte[] buffer, int offset, int count)
            {
                return this.decompressed.Read(buffer, offset, count);
            }

            public override long Seek(long offset, SeekOrigin origin)
            {
                throw new NotSupportedException();
            }

            public override void SetLength(long value)
            {
                throw new NotSupportedException();
            }

            public override void Write(byte[] buffer, int offset, int count)
            {
                throw new NotSupportedException();
            }

            protected override void Dispose(bool disposing)
            {
                base.Dispose(disposing);

                if (disposing)
                {
                    this.decompressed.Dispose();
                }
            }
        }
        #endregion

        #region Define Codec
        //Define the actual codec class containing the required methods for manipulating streams:
        //GetCompressedStreamOver(Stream uncompressed) and GetDecompressedStreamOver(Stream compressed).
        //Codec class uses classes for compressed and decompressed streams defined above.
        internal sealed class DeflateCodec45 : Codec
        {

            //We merely use different IMPLEMENTATIONS of Deflate, so CodecName remains "deflate"
            public static readonly string CodecName = "deflate";

            public DeflateCodec45()
                : base(CodecName)
            {
            }

            public override Stream GetCompressedStreamOver(Stream decompressed)
            {
                if (decompressed == null)
                {
                    throw new ArgumentNullException("decompressed");
                }

                return new CompressionStreamDeflate45(decompressed);
            }

            public override Stream GetDecompressedStreamOver(Stream compressed)
            {
                if (compressed == null)
                {
                    throw new ArgumentNullException("compressed");
                }

                return new DecompressionStreamDeflate45(compressed);
            }
        }
        #endregion

        #region Define modified Codec Factory
        //Define modified codec factory to be used in the reader.
        //It catches the attempt to use "Deflate" and provide  a custom codec.
        //For all other cases, it relies on the base class (CodecFactory).
        internal sealed class CodecFactoryDeflate45 : CodecFactory
        {

            public override Codec Create(string codecName)
            {
                if (codecName == DeflateCodec45.CodecName)
                    return new DeflateCodec45();
                else
                    return base.Create(codecName);
            }
        }
        #endregion

        #endregion

        #region Sample Class with demonstration methods
        //This class contains methods demonstrating
        //the usage of Microsoft Avro Library
        public class AvroSample
        {

            //Serializes and deserializes sample data set by using reflection and Avro object container files.
            //Serialized data is compressed with the custom compression codec (Deflate of .NET Framework 4.5).
            //
            //This sample uses memory stream for all operations related to serialization, deserialization and
            //object container manipulation, though file stream could be easily used.
            public void SerializeDeserializeUsingObjectContainersReflectionCustomCodec()
            {

                Console.WriteLine("SERIALIZATION USING REFLECTION, AVRO OBJECT CONTAINER FILES AND CUSTOM CODEC\n");

                //Path for Avro object container file
                string path = "AvroSampleReflectionDeflate45.avro";

                //Create a data set by using sample class and struct
                var testData = new List<SensorData>
                        {
                            new SensorData { Value = new byte[] { 1, 2, 3, 4, 5 }, Position = new Location { Room = 243, Floor = 1 } },
                            new SensorData { Value = new byte[] { 6, 7, 8, 9 }, Position = new Location { Room = 244, Floor = 1 } }
                        };

                //Serializing and saving data to file.
                //Creating a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Serializing Sample Data Set...");

                    //Create a SequentialWriter instance for type SensorData, which can serialize a sequence of SensorData objects to stream.
                    //Here the custom codec is introduced. For convenience, the next commented code line shows how to use built-in Deflate.
                    //Note that because the sample deals with different IMPLEMENTATIONS of Deflate, built-in and custom codecs are interchangeable
                    //in read-write operations.
                    //using (var w = AvroContainer.CreateWriter<SensorData>(buffer, Codec.Deflate))
                    using (var w = AvroContainer.CreateWriter<SensorData>(buffer, new DeflateCodec45()))
                    {
                        using (var writer = new SequentialWriter<SensorData>(w, 24))
                        {
                            // Serialize the data to stream using the sequential writer
                            testData.ForEach(writer.Write);
                        }
                    }

                    //Save stream to file
                    Console.WriteLine("Saving serialized data to file...");
                    if (!WriteFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }
                }

                //Reading and deserializing data.
                //Creating a memory stream buffer.
                using (var buffer = new MemoryStream())
                {
                    Console.WriteLine("Reading data from file...");

                    //Reading data from object container file
                    if (!ReadFile(buffer, path))
                    {
                        Console.WriteLine("Error during file operation. Quitting method");
                        return;
                    }

                    Console.WriteLine("Deserializing Sample Data Set...");

                    //Prepare the stream for deserializing the data
                    buffer.Seek(0, SeekOrigin.Begin);

                    //Because of SequentialReader<T> constructor signature, an AvroSerializerSettings instance is required
                    //when codec factory is explicitly specified.
                    //You may comment the line below if you want to use built-in Deflate (see next comment).
                    AvroSerializerSettings settings = new AvroSerializerSettings();

                    //Create a SequentialReader instance for type SensorData, which deserializes all serialized objects from the given stream.
                    //It allows iterating over the deserialized objects because it implements the IEnumerable<T> interface.
                    //Here the custom codec factory is introduced.
                    //For convenience, the next commented code line shows how to use built-in Deflate
                    //(no explicit Codec Factory parameter is required in this case).
                    //Note that because the sample deals with different IMPLEMENTATIONS of Deflate, built-in and custom codecs are interchangeable
                    //in read-write operations.
                    //using (var reader = new SequentialReader<SensorData>(AvroContainer.CreateReader<SensorData>(buffer, true)))
                    using (var reader = new SequentialReader<SensorData>(
                        AvroContainer.CreateReader<SensorData>(buffer, true, settings, new CodecFactoryDeflate45())))
                    {
                        var results = reader.Objects;

                        //Finally, verify that deserialized data matches the original one
                        Console.WriteLine("Comparing Initial and Deserialized Data Sets...");
                        bool isEqual;
                        int count = 1;
                        var pairs = testData.Zip(results, (serialized, deserialized) => new { expected = serialized, actual = deserialized });
                        foreach (var pair in pairs)
                        {
                            isEqual = this.Equal(pair.expected, pair.actual);
                            Console.WriteLine("For Pair {0} result of Data Set Identity Comparison is {1}", count, isEqual.ToString());
                            count++;
                        }
                    }
                }

                //Delete the file
                RemoveFile(path);
            }
        #endregion

            #region Helper Methods

            //Comparing two SensorData objects
            private bool Equal(SensorData left, SensorData right)
            {
                return left.Position.Equals(right.Position) && left.Value.SequenceEqual(right.Value);
            }

            //Saving memory stream to a new file with the given path
            private bool WriteFile(MemoryStream InputStream, string path)
            {
                if (!File.Exists(path))
                {
                    try
                    {
                        using (FileStream fs = File.Create(path))
                        {
                            InputStream.Seek(0, SeekOrigin.Begin);
                            InputStream.CopyTo(fs);
                        }
                        return true;
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during creation and writing to the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                        return false;
                    }
                }
                else
                {
                    Console.WriteLine("Can not create file \"{0}\". File already exists", path);
                    return false;

                }
            }

            //Reading file content by using the given path to a memory stream
            private bool ReadFile(MemoryStream OutputStream, string path)
            {
                try
                {
                    using (FileStream fs = File.Open(path, FileMode.Open))
                    {
                        fs.CopyTo(OutputStream);
                    }
                    return true;
                }
                catch (Exception e)
                {
                    Console.WriteLine("The following exception was thrown during reading from the file \"{0}\"", path);
                    Console.WriteLine(e.Message);
                    return false;
                }
            }

            //Deleting file by using given path
            private void RemoveFile(string path)
            {
                if (File.Exists(path))
                {
                    try
                    {
                        File.Delete(path);
                    }
                    catch (Exception e)
                    {
                        Console.WriteLine("The following exception was thrown during deleting the file \"{0}\"", path);
                        Console.WriteLine(e.Message);
                    }
                }
                else
                {
                    Console.WriteLine("Can not delete file \"{0}\". File does not exist", path);
                }
            }
            #endregion

            static void Main()
            {

                string sectionDivider = "---------------------------------------- ";

                //Create an instance of AvroSample Class and invoke methods
                //illustrating different serializing approaches
                AvroSample Sample = new AvroSample();

                //Serialization using reflection to Avro object container file using custom codec
                Sample.SerializeDeserializeUsingObjectContainersReflectionCustomCodec();

                Console.WriteLine(sectionDivider);
                Console.WriteLine("Press any key to exit.");
                Console.Read();
            }
        }
    }
    // The example is expected to display the following output:
    // SERIALIZATION USING REFLECTION, AVRO OBJECT CONTAINER FILES AND CUSTOM CODEC
    //
    // Serializing Sample Data Set...
    // Saving serialized data to file...
    // Reading data from file...
    // Deserializing Sample Data Set...
    // Comparing Initial and Deserialized Data Sets...
    // For Pair 1 result of Data Set Identity Comparison is True
    //For Pair 2 result of Data Set Identity Comparison is True
    // ----------------------------------------
    // Press any key to exit.

## <a name="sample-6-using-avro-to-upload-data-for-the-microsoft-azure-hdinsight-service"></a>Voorbeeld 6: Met behulp van Avro het uploaden van gegevens voor de Microsoft Azure HDInsight-service
Het zesde voorbeeld ziet u enkele programmeertechnieken die betrekking hebben op de interactie met de Azure HDInsight-service. Een voorbeeld met de code voor dit voorbeeld kan worden gedownload vanaf de [Azure-codevoorbeelden](https://code.msdn.microsoft.com/Using-Avro-to-upload-data-ae81b1e3) site.

Het voorbeeld heeft de volgende taken:

* Maakt verbinding met een bestaand HDInsight-service-cluster.
* Serialiseert verschillende CSV-bestanden en het resultaat geüpload naar Azure Blob storage. (De CSV-bestanden samen met het voorbeeld worden gedistribueerd en uitpakken van AMEX voorraad historische gegevens worden verspreid door vertegenwoordigen [Infochimps](http://www.infochimps.com/) voor de periode 1970 2010. Het voorbeeld leest gegevens uit een CSV-bestand, converteert u de records voor exemplaren van de **voorraad** klasse en serialiseert u ze via reflectie. Aandelen typedefinitie is gemaakt op basis van een JSON-schema via het Microsoft Avro Library-hulpprogramma voor het genereren van code.
* Hiermee maakt u een nieuwe externe tabel met de naam **voorraden** in Hive en koppelingen naar de gegevens in de vorige stap hebt geüpload.
* Een query uitvoert met behulp van Hive via de **voorraden** tabel.

Het voorbeeld voert bovendien een procedure opschonen voor en na het uitvoeren van belangrijke bewerkingen. Tijdens het opschonen, alle verwante Azure Blob-gegevens en mappen worden verwijderd en de Hive-tabel is verwijderd. U kunt ook de procedure opschonen vanaf de voorbeeldopdrachtregel aanroepen.

Het voorbeeld heeft de volgende vereisten:

* Een actief abonnement op Microsoft Azure en de abonnements-ID.
* Een beheercertificaat voor het abonnement met de bijbehorende persoonlijke sleutel. Het certificaat moet worden geïnstalleerd in de huidige gebruiker persoonlijke opslag op de computer die wordt gebruikt om uit te voeren van het voorbeeld.
* Een actief HDInsight-cluster.
* Een Azure Storage-account is gekoppeld aan het HDInsight-cluster uit de vorige vereiste, samen met de bijbehorende primaire of secundaire toegangssleutel.

Alle informatie met betrekking tot de vereisten moet worden ingevoerd in het voorbeeldconfiguratiebestand voordat het voorbeeld wordt uitgevoerd. Er zijn twee manieren om dit te doen:

* Bewerk het bestand app.config in de hoofdmap van het voorbeeld en bouw het voorbeeld
* Het voorbeeld eerst maken en bewerk vervolgens AvroHDISample.exe.config in de build-map

In beide gevallen moeten alle bewerkingen worden uitgevoerd de **<appSettings>** gedeelte instellingen. Voer de opmerkingen in het bestand.
Het voorbeeld wordt uitgevoerd vanaf de opdrachtregel door de volgende opdracht wordt uitgevoerd (waar het ZIP-bestand met het voorbeeld wordt uitgegaan van een worden uitgepakt naar C:\AvroHDISample; als het pad van het relevante anders gebruiken):

    AvroHDISample run C:\AvroHDISample\Data

Als u wilt opschonen van het cluster, moet u de volgende opdracht uitvoeren:

    AvroHDISample clean

[deflate-100]: http://msdn.microsoft.com/library/system.io.compression.deflatestream(v=vs.100).aspx
[deflate-110]: http://msdn.microsoft.com/library/system.io.compression.deflatestream(v=vs.110).aspx
