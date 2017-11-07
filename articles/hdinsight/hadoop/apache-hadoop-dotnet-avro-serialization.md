---
title: Gegevens in Azure Hadoop - Library Microsoft Avro - serialiseren | Microsoft Docs
description: Informatie over het serialiseren en deserialiseren van gegevens in Hadoop op HDInsight met behulp van de Microsoft Avro Library om vast te leggen in het geheugen, een database of een bestand.
keywords: avro, hadoop avro
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: c78dc20d-5d8d-4366-94ac-abbe89aaac58
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/06/2017
ms.author: jgao
ms.custom: hdiseo17may2017
ms.openlocfilehash: 04f76d15f12b52d7b47011010a5ce20cc45174ee
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/06/2017
---
# <a name="serialize-data-in-hadoop-with-the-microsoft-avro-library"></a>Gegevens in Hadoop met de Microsoft Avro Library serialiseren

>[!NOTE]
>De Avro-SDK wordt niet langer ondersteund door Microsoft. De bibliotheek is open-source community ondersteund. De bronnen voor de bibliotheek bevinden zich in [Github](https://github.com/Azure/azure-sdk-for-net/tree/master/src/ServiceManagement/HDInsight/Microsoft.Hadoop.Avro).

Dit onderwerp wordt beschreven hoe u de [Microsoft Avro Library](https://github.com/Azure/azure-sdk-for-net/tree/master/src/ServiceManagement/HDInsight/Microsoft.Hadoop.Avro) voor het serialiseren van objecten en andere gegevensstructuren in streams ze ook behouden in het geheugen, een database of een bestand. Ook ziet u hoe deserialiseert om te herstellen van de oorspronkelijke objecten.

[!INCLUDE [windows-only](../../../includes/hdinsight-windows-only.md)]

## <a name="apache-avro"></a>Apache Avro
De <a href="https://hadoopsdk.codeplex.com/wikipage?title=Avro%20Library" target="_blank">Microsoft Avro Library</a> het Apache Avro gegevens serialisatie system voor de Microsoft.NET-omgeving implementeert. Apache Avro biedt een compacte binaire gegevens DIF-indeling voor serialisatie. Hierbij <a href="http://www.json.org" target="_blank">JSON</a> om een taal networkdirect-schema dat interoperabiliteit van talen taalherkenning te definiëren. Gegevens die zijn geserialiseerd in één taal kan worden gelezen in een andere. C, C++ C#, Java, PHP, Python en Ruby worden momenteel ondersteund. Gedetailleerde informatie over deze indeling vindt u in de <a href="http://avro.apache.org/docs/current/spec.html" target="_blank">Apache Avro-specificatie</a>. 

>[!NOTE]
>De Microsoft Avro Library biedt geen ondersteuning voor het deel van de externe procedure aanroepen (RPC's) van deze specificatie.
>

De geserialiseerde representatie van een object in het Avro-systeem bestaat uit twee delen: schema en de werkelijke waarde. De Avro-schema beschrijft het gegevensmodel van taalonafhankelijke van de geserialiseerde gegevens met JSON. Deze functionaliteit wordt weergegeven naast een binaire voorstelling van gegevens. Met het schema gescheiden van de binaire weergave kunt elk object om te worden geschreven met geen per waarde-overhead klein maken voor serialisatie snel en de weergave.

## <a name="the-hadoop-scenario"></a>De Hadoop-scenario
De serialisatie-indeling van Apache Avro wordt veel gebruikt in Azure HDInsight en andere Apache Hadoop-omgevingen. Avro biedt een handige manier om aan te duiden complexe gegevensstructuren binnen een Hadoop-MapReduce-taak. De indeling van het Avro-bestanden (Avro object container-bestand) is ontworpen ter ondersteuning van het gedistribueerde MapReduce-programmeermodel. De belangrijkste functie waarmee de distributie is dat de bestanden 'splittable' in de zin dat één kunt elk in een bestand punt en beginnen met lezen van een bepaald blok zijn.

## <a name="serialization-in-avro-library"></a>Serialisatie in de bibliotheek voor Avro
De .NET-bibliotheek voor Avro ondersteunt serialiseren objecten op twee manieren:

* **Reflectie** -de JSON-schema voor de typen automatisch gebouwd van de gegevens contract kenmerken van de .NET-typen worden geserialiseerd.
* **algemene record** -een JSON-schema expliciet is opgegeven in een record die wordt vertegenwoordigd door de [ **AvroRecord** ](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) klasse wanneer geen .NET-typen aanwezig om te beschrijven van het schema voor de gegevens die zijn moeten worden geserialiseerd.

Wanneer het schema van de bekend is dat de schrijver en de lezer van de stroom, kunnen de gegevens worden verzonden zonder dat het schema. Wanneer een Avro-object container-bestand wordt gebruikt, wordt het schema in gevallen opgeslagen in het bestand. Andere parameters, zoals de codec gebruikt voor de compressie van gegevens kunnen worden opgegeven. Deze scenario's zijn in meer detail beschreven en wordt geïllustreerd in de volgende codevoorbeelden:

## <a name="install-avro-library"></a>Bibliotheek voor Avro installeren
Het volgende is vereist voordat u de bibliotheek installeert:

* <a href="http://www.microsoft.com/download/details.aspx?id=17851" target="_blank">Microsoft .NET Framework 4</a>
* <a href="http://james.newtonking.com/json" target="_blank">Newtonsoft Json.NET</a> (6.0.4 of hoger)

Houd er rekening mee dat de afhankelijkheid Newtonsoft.Json.dll automatisch wordt gedownload met de installatie van de Microsoft Avro Library. De procedure is opgegeven in de volgende sectie:

De Microsoft Avro Library wordt gedistribueerd als een NuGet-pakket van Visual Studio kan worden geïnstalleerd via de volgende procedure:

1. Selecteer de **Project** tabblad -> **NuGet-pakketten beheren...**
2. Zoeken naar 'Microsoft.Hadoop.Avro' in de **Online zoeken** vak.
3. Klik op de **installeren** naast **Avro-bibliotheek van Microsoft Azure HDInsight**.

Houd er rekening mee dat de Newtonsoft.Json.dll (> = 6.0.4) afhankelijkheid ook automatisch wordt gedownload met de Microsoft Avro Library.

De broncode van de Microsoft Avro Library is beschikbaar op [Github](https://github.com/Azure/azure-sdk-for-net/tree/master/src/ServiceManagement/HDInsight/Microsoft.Hadoop.Avro).

## <a name="compile-schemas-using-avro-library"></a>Schema's met behulp van de bibliotheek voor Avro compileren
De Microsoft Avro Library bevat een hulpprogramma voor het genereren van code waarmee het maken van C#-typen automatisch op basis van de eerder gedefinieerde JSON-schema. Het hulpprogramma voor het genereren van code niet als een binaire uitvoerbaar bestand is gedistribueerd, maar kan eenvoudig worden ingebouwd via de volgende procedure:

1. Download het ZIP-bestand met de nieuwste versie van de broncode HDInsight SDK uit <a href="http://hadoopsdk.codeplex.com/SourceControl/latest#" target="_blank">Microsoft .NET SDK voor Hadoop</a>. (Klik op de **downloaden** pictogram, niet de **downloadt** tabblad.)
2. De HDInsight SDK uitpakken naar een map op de machine met .NET Framework 4 is geïnstalleerd en is verbonden met Internet om vereiste afhankelijkheid NuGet-pakketten te downloaden. Hieronder, gaan we ervan uit dat de broncode wordt uitgepakt in C:\SDK.
3. Ga naar de map C:\SDK\src\Microsoft.Hadoop.Avro.Tools en build.bat uitvoeren. (Het bestand aanroepen MSBuild van de 32-bits-distributie van het .NET Framework. Als u wilt de 64-bits versie te gebruiken, bewerk build.bat, de opmerkingen in het bestand te volgen.) Zorg ervoor dat de build geslaagd is. (Op sommige systemen MSBuild kan leiden tot waarschuwingen. Deze waarschuwingen hebben geen invloed op het hulpprogramma, zolang er geen fouten build zijn.)
4. Het gecompileerde hulpprogramma bevindt zich in C:\SDK\Bin\Unsigned\Release\Microsoft.Hadoop.Avro.Tools.

Als u bekend bent met de syntaxis van de opdrachtregel, voer de volgende opdracht uit de map waarin het hulpprogramma voor het genereren van code bevindt:`Microsoft.Hadoop.Avro.Tools help /c:codegen`

U kunt het hulpprogramma testen, kunt u C#-klassen genereren van het JSON-schema-voorbeeldbestand geleverd met de broncode. Voer de volgende opdracht:

    Microsoft.Hadoop.Avro.Tools codegen /i:C:\SDK\src\Microsoft.Hadoop.Avro.Tools\SampleJSON\SampleJSONSchema.avsc /o:

Dit moet voor het produceren van twee C#-bestanden in de huidige map: SensorData.cs en Location.cs.

Zie het bestand GenerationVerification.feature in C:\SDK\src\Microsoft.Hadoop.Avro.Tools\Doc inzicht in de logica die door het hulpprogramma voor het genereren van code wordt gebruikt tijdens het converteren van de JSON-schema voor C#-typen.

Naamruimten worden opgehaald uit de JSON-schema met behulp van de logica in het bestand dat wordt vermeld in de vorige alinea. Naamruimten die worden opgehaald uit het schema hebben voorrang op wat wordt geleverd bij de parameter/n in de opdrachtregel van het hulpprogramma. Als u onderdrukken van de naamruimten die zijn opgenomen in het schema wilt, moet u de parameter /nf gebruiken. Bijvoorbeeld, als u alle naamruimten van de SampleJSONSchema.avsc wilt my.own.nspace, voer de volgende opdracht:

    Microsoft.Hadoop.Avro.Tools codegen /i:C:\SDK\src\Microsoft.Hadoop.Avro.Tools\SampleJSON\SampleJSONSchema.avsc /o:. /nf:my.own.nspace

## <a name="about-the-samples"></a>Over de voorbeelden
Zes voorbeelden in dit onderwerp laten zien dat verschillende scenario's die worden ondersteund door de Microsoft Avro Library. De Microsoft Avro Library is ontworpen voor gebruik met elke andere stroom. In deze voorbeelden wordt gegevens gemanipuleerd via geheugen gegevensstromen in plaats van bestand streams of databases voor eenvoud en consistentie. De aanpak in een productieomgeving, is afhankelijk van de exacte scenariovereisten, gegevensbron en volume, prestaties beperkingen en andere factoren.

De eerste twee voorbeelden laten zien hoe serialiseren en deserialiseren van gegevens in de stroom geheugenbuffers via reflectie en algemene records. Het schema in beide gevallen wordt ervan uitgegaan dat worden gedeeld tussen de en schrijfprogramma out-of-band.

De derde en vierde voorbeelden laten zien hoe serialiseren en deserialiseren van gegevens met behulp van de Avro-object container-bestanden. Wanneer gegevens worden opgeslagen in een Avro-container-bestand, wordt het schema is altijd met het opgeslagen omdat het schema moet worden gedeeld voor deserialisatie.

Het voorbeeld met de eerste vier voorbeelden kan worden gedownload vanuit de <a href="http://code.msdn.microsoft.com/Serialize-data-with-the-86055923" target="_blank">Azure codevoorbeelden</a> site.

De vijfde voorbeeld ziet u hoe u een aangepaste compressiecodec voor containerbestanden Avro-object. Een voorbeeld met de code voor dit voorbeeld kan worden gedownload vanaf de <a href="http://code.msdn.microsoft.com/Serialize-data-with-the-67159111" target="_blank">Azure codevoorbeelden</a> site.

Het zesde voorbeeld laat zien hoe Avro serialisatie gebruiken om te uploaden van gegevens naar Azure Blob-opslag en vervolgens te analyseren met behulp van Hive met een HDInsight (Hadoop)-cluster. Kan worden gedownload vanaf de <a href="https://code.msdn.microsoft.com/Using-Avro-to-upload-data-ae81b1e3" target="_blank">Azure codevoorbeelden</a> site.

Hier vindt u koppelingen naar de zes steekproeven besproken in het onderwerp:

* <a href="#Scenario1">**Serialisatie met reflectie** </a> -de JSON-schema voor typen om te worden geserialiseerd automatisch gebouwd van de gegevens contract kenmerken.
* <a href="#Scenario2">**Serialisatie met algemene record** </a> -de JSON-schema is expliciet worden opgegeven in een record wanneer er geen .NET-type beschikbaar voor reflectie is.
* <a href="#Scenario3">**Gebruik van object containerbestanden met reflectie serialisatie** </a> -de JSON-schema automatisch wordt gemaakt en gedeeld samen met de geserialiseerde gegevens via een Avro-object container-bestand.
* <a href="#Scenario4">**Serialisatie in object containerbestanden met algemene record** </a> -de JSON-schema expliciet is opgegeven voordat de serialisatie en gedeeld samen met de gegevens via een Avro-object container-bestand.
* <a href="#Scenario5">**Gebruik van object containerbestanden met een aangepaste compressiecodec serialisatie** </a> -het voorbeeld ziet u een Avro-object container-bestand maken met een aangepaste .NET-implementatie van de compressiecodec Deflate gegevens.
* <a href="#Scenario6">**Avro met uploaden van gegevens voor de service Microsoft Azure HDInsight** </a> -het voorbeeld ziet u hoe de Avro-serialisatie samenwerkt met de HDInsight-service. Een actief Azure-abonnement en de toegang tot een Azure HDInsight-cluster zijn vereist dit voorbeeld wilt uitvoeren.

## <a name="Scenario1"></a>Voorbeeld 1: Serialisatie met reflectie
De JSON-schema voor de typen kan worden automatisch gebouwd door de Microsoft Avro Library via reflectie van de gegevens contract kenmerken van de C#-objecten moeten worden geserialiseerd. De Microsoft Avro Library maakt een [ **IAvroSeralizer<T>**  ](http://msdn.microsoft.com/library/dn627341.aspx) voor het identificeren van de velden die moeten worden geserialiseerd.

In dit voorbeeld objecten (een **SensorData** klasse met een lid **locatie** struct) worden geserialiseerd naar een stream geheugen en deze stroom op zijn beurt wordt gedeserialiseerd. Het resultaat wordt vervolgens vergeleken met het eerste exemplaar om te bevestigen dat de **SensorData** object hersteld is identiek aan de oorspronkelijke.

Het schema in dit voorbeeld wordt verondersteld kunnen worden gedeeld tussen de en schrijfprogramma, zodat de containerindeling van de Avro-object niet vereist is. Zie voor een voorbeeld van hoe u serialiseren en deserialiseren van gegevens in geheugenbuffers via reflectie met de indeling van de container object als het schema moet worden gedeeld met de gegevens <a href="#Scenario3">serialisatie in object containerbestanden met reflectie</a>.

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


## <a name="sample-2-serialization-with-a-generic-record"></a>Voorbeeld 2: Serialisatie met algemene record
Een JSON-schema kan expliciet worden opgegeven in een algemene record wanneer reflectie kan niet worden gebruikt omdat de gegevens via .NET-klassen met een gegevenscontract kan niet worden weergegeven. Deze methode is langzamer dan met behulp van reflectie. In dergelijke gevallen wordt kan het schema voor de gegevens ook worden dynamisch is, dat wil zeggen, niet bekend bij het compileren. Gegevens die worden vertegenwoordigd als door komma's gescheiden waarden (CSV)-bestanden waarvan het schema onbekend is totdat deze wordt omgezet naar de Avro-indeling tijdens de uitvoering is een voorbeeld van dit type dynamische scenario.

In dit voorbeeld laat zien hoe maken en gebruiken van een [ **AvroRecord** ](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) expliciet opgeven van een JSON-schema, hoe deze vullen met de gegevens en het serialiseren en te deserialiseren. Het resultaat wordt vervolgens vergeleken met het eerste exemplaar om te bevestigen dat de record hersteld identiek aan de oorspronkelijke is.

Het schema in dit voorbeeld wordt verondersteld kunnen worden gedeeld tussen de en schrijfprogramma, zodat de containerindeling van de Avro-object niet vereist is. Zie voor een voorbeeld van hoe u serialiseren en deserialiseren van gegevens in geheugenbuffers met behulp van een algemene record met de indeling van de container object wanneer het schema opgenomen in de geserialiseerde gegevens worden moet, de <a href="#Scenario4">serialisatie in object containerbestanden met algemene record</a> voorbeeld.

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
In dit voorbeeld is vergelijkbaar met het scenario in de <a href="#Scenario1"> eerste voorbeeld</a>, waarbij het schema impliciet met reflectie is opgegeven. Het verschil is dat hier, wordt het schema niet uitgegaan bekend zijn bij de lezer die deze deserializes. De **SensorData** objecten om te worden geserialiseerd en hun impliciet opgegeven schema worden opgeslagen in een Avro-object container-bestand dat wordt vertegenwoordigd door de [ **AvroContainer** ](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.avrocontainer.aspx) klasse.

De gegevens in dit voorbeeld met is geserialiseerd [ **SequentialWriter<SensorData>**  ](http://msdn.microsoft.com/library/dn627340.aspx) en gedeserialiseerd met [ **SequentialReader<SensorData>**](http://msdn.microsoft.com/library/dn627340.aspx). Het resultaat wordt vervolgens vergeleken met de eerste exemplaren om ervoor te zorgen identiteit.

De gegevens in het bestand van de container object via de standaard worden gecomprimeerd [ **Deflate** ] [ deflate-100] compressiecodec van .NET Framework 4. Zie de <a href="#Scenario5"> vijfde voorbeeld</a> in dit onderwerp voor informatie over het gebruik van een meer recente en hogere versie van de [ **Deflate** ] [ deflate-110] compressiecodec beschikbaar in .NET Framework 4.5.

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
In dit voorbeeld is vergelijkbaar met het scenario in de <a href="#Scenario2"> tweede voorbeeld</a>, waarbij het schema expliciet is opgegeven met JSON. Het verschil is dat hier, wordt het schema niet uitgegaan bekend zijn bij de lezer die deze deserializes.

De testgegevensset zijn verzameld in een lijst met [ **AvroRecord** ](http://msdn.microsoft.com/library/microsoft.hadoop.avro.avrorecord.aspx) objecten via een expliciet gedefinieerde JSON-schema en vervolgens wordt opgeslagen in een container-bestand van het object dat wordt vertegenwoordigd door de [ **AvroContainer** ](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.avrocontainer.aspx) klasse. Dit containerbestand maakt een writer die gebruikt wordt voor het serialiseren van de gegevens, ongecomprimeerde op een geheugenstroom die wordt vervolgens naar een bestand opgeslagen. De [ **Codec.Null** ](http://msdn.microsoft.com/library/microsoft.hadoop.avro.container.codec.null.aspx) parameter die wordt gebruikt voor het maken van de lezer geeft aan dat deze gegevens niet worden gecomprimeerd.

De gegevens worden vervolgens lezen uit het bestand en gedeserialiseerd tot een verzameling van objecten. Deze verzameling wordt vergeleken met de initiële lijst met records Avro om te bevestigen dat ze identiek zijn.

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




## <a name="sample-5-serialization-using-object-container-files-with-a-custom-compression-codec"></a>Voorbeeld 5: Serialisatie object container-bestanden gebruiken in een aangepaste compressiecodec
De vijfde voorbeeld ziet u hoe u een aangepaste compressiecodec voor containerbestanden Avro-object. Een voorbeeld met de code voor dit voorbeeld kan worden gedownload vanaf de [Azure codevoorbeelden](http://code.msdn.microsoft.com/Serialize-data-with-the-67159111) site.

De [Avro-specificatie](http://avro.apache.org/docs/current/spec.html#Required+Codecs) kunt u informatie over het gebruik van een optionele compressiecodec (in aanvulling op **Null** en **Deflate** standaardwaarden). In dit voorbeeld wordt een nieuwe codec zoals Snappy niet implementeren (vermeld als een ondersteunde optionele codec in de [Avro-specificatie](http://avro.apache.org/docs/current/spec.html#snappy)). Er wordt weergegeven hoe gebruiken de .NET Framework 4.5-implementatie van de [ **Deflate** ] [ deflate-110] codec een betere compressiealgoritme op basis biedt van de [zlib](http://zlib.net/) compressiebibliotheek dan de standaard .NET Framework 4-versie.

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

## <a name="sample-6-using-avro-to-upload-data-for-the-microsoft-azure-hdinsight-service"></a>Voorbeeld 6: Avro gebruiken voor het uploaden van gegevens voor de Microsoft Azure HDInsight-service
Het zesde voorbeeld ziet u enkele programmeertechnieken die betrekking hebben op de interactie met de Azure HDInsight-service. Een voorbeeld met de code voor dit voorbeeld kan worden gedownload vanaf de [Azure codevoorbeelden](https://code.msdn.microsoft.com/Using-Avro-to-upload-data-ae81b1e3) site.

Het voorbeeld worden de volgende taken:

* Maakt verbinding met een bestaand HDInsight-service-cluster.
* Serialiseert verschillende CSV-bestanden en het resultaat geüpload naar Azure Blob-opslag. (De CSV-bestanden samen met het voorbeeld worden gedistribueerd en vertegenwoordigen een uitpakken uit bijlage aandelen historische gegevens worden verspreid door [Infochimps](http://www.infochimps.com/) voor de periode 1970 2010. Het voorbeeld leest gegevens uit een CSV-bestand, converteert u de records op exemplaren van de **Stock** klasse en serialiseert u ze via reflectie. Voorraad typedefinitie wordt gemaakt van een JSON-schema via het hulpprogramma Microsoft Avro Library code genereren.
* Maakt een nieuwe externe tabel aangeroepen **voorraden** in Hive en koppelingen naar de gegevens in de vorige stap hebt geüpload.
* Een query uitvoert met behulp van Hive via de **voorraden** tabel.

Het voorbeeld voert bovendien een procedure opschonen voor en na het uitvoeren van belangrijke bewerkingen. Alle gerelateerde Azure Blob-gegevens en mappen worden verwijderd tijdens het opschonen, en de Hive-tabel is verwijderd. U kunt ook de procedure opschonen uit vanaf de voorbeeldopdrachtregel aanroepen.

Het voorbeeld heeft de volgende vereisten:

* Een actief Microsoft Azure-abonnement en de abonnement-ID.
* Een beheercertificaat voor het abonnement met de bijbehorende persoonlijke sleutel. Het certificaat moet worden geïnstalleerd in de huidige gebruiker persoonlijke opslag op de computer gebruikt voor het uitvoeren van het voorbeeld.
* Een actief HDInsight-cluster.
* Een Azure Storage-account is gekoppeld aan het HDInsight-cluster van de vorige vereiste, samen met de bijbehorende primaire of secundaire toegangssleutel.

Alle informatie van de vereisten moeten worden ingevoerd in het voorbeeldconfiguratiebestand voordat de steekproef wordt uitgevoerd. Er zijn twee manieren doen:

* Bewerk het bestand app.config in de hoofdmap van het voorbeeld en vervolgens het voorbeeld
* Het voorbeeld eerst te bouwen en bewerk vervolgens AvroHDISample.exe.config in de map build

In beide gevallen alle bewerkingen worden uitgevoerd in de  **<appSettings>**  gedeelte instellingen. Volg de opmerkingen in het bestand.
Het voorbeeld vanaf de opdrachtregel wordt uitgevoerd door het uitvoeren van de volgende opdracht (waarbij het ZIP-bestand met het voorbeeld wordt uitgegaan van een worden uitgepakt naar C:\AvroHDISample; als het pad van het relevante anders gebruiken):

    AvroHDISample run C:\AvroHDISample\Data

Voor het opschonen van het cluster, moet u de volgende opdracht uitvoeren:

    AvroHDISample clean

[deflate-100]: http://msdn.microsoft.com/library/system.io.compression.deflatestream(v=vs.100).aspx
[deflate-110]: http://msdn.microsoft.com/library/system.io.compression.deflatestream(v=vs.110).aspx
