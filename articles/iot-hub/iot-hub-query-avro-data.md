---
title: Opvragen van Avro-gegevens met Azure Data Lake Analytics | Microsoft Docs
description: Hoofdtekst berichteigenschappen om telemetriegegevens route blob-opslag en opvragen van de Avro-indeling gegevens geschreven naar de blob storage gebruiken.
services: iot-hub
documentationcenter: ''
author: ksaye
manager: obloch
ms.service: iot-hub
ms.topic: article
ms.date: 05/29/2018
ms.author: Kevin.Saye
ms.openlocfilehash: 98a30155c73a937042b4bea6568543fb5152d748
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34727739"
---
# <a name="query-avro-data-using-azure-data-lake-analytics"></a>Avro-gegevens opvragen met Azure Data Lake Analytics

Dit artikel is over het Avro-gegevens opvragen voor het efficiënt doorsturen van berichten van Azure IoT Hub tot Azure-services. Na de blogberichtaankondiging:[Azure IoT Hub berichtroutering: nu met de routering op berichttekst], IoT-Hub ondersteunt routering op eigenschappen of de berichttekst. Zie ook [routering op de berichttekst][Routing on message bodies]. 

IoT Hub schrijft dat de uitdaging is dat wanneer Azure IoT Hub berichten routeert naar de blob storage, de inhoud in de Avro-indeling berichttekst en eigenschappen van berichten heeft. Houd er rekening mee dat IoT Hub biedt alleen ondersteuning voor schrijven van gegevens naar de blob storage in de indeling van het Avro-gegevens en deze indeling niet voor alle andere eindpunten gebruikt wordt. Zie [bij gebruik van Azure Storage-containers][When using Azure storage containers]. Hoewel de Avro-indeling ideaal voor het behoud van gegevens/bericht is, is het lastig voor query's in de gegevens. Ter vergelijking: JSON- of CSV-indeling, het is veel eenvoudiger is voor het opvragen van gegevens.

U verhelpt dit, kunt u veel van de big data-patronen voor zowel transformeren en schalen van gegevens voor het oplossen van niet-relationele big data-behoeften en indelingen. Een van de patronen, een 'betaalde per query'-patroon, is Azure Data Lake Analytics (ADLA). De focus van dit artikel is. Hoewel de query kan eenvoudig worden uitgevoerd in Hadoop of andere oplossingen, wordt ADLA vaak beter geschikt voor deze benadering 'betalen per query'. Er is een zelfstandig 'uitpakken' voor Avro in U-SQL. Zie [U-SQL Avro-voorbeeld].

## <a name="query-and-export-avro-data-to-a-csv-file"></a>Vragen en Avro-gegevens exporteren naar een CSV-bestand
De sectie leidt u door de Avro-gegevens opvragen en te exporteren naar een CSV-bestand in Azure Blob Storage, hoewel u eenvoudig de gegevens in andere winkels-opslagplaatsen of gegevens kan plaatsen.

1. Instellen van Azure IoT Hub gegevens routeren naar een Azure Blob Storage-eindpunt met een eigenschap in de berichttekst berichten te selecteren.

    ![Schermopname voor stap 1a][img-query-avro-data-1a]

    ![Schermopname voor stap 1b][img-query-avro-data-1b]

2. Zorg ervoor dat uw apparaat heeft de codering, het type inhoud en de benodigde gegevens in de eigenschappen of de hoofdtekst van het bericht waarnaar wordt verwezen in de productdocumentatie. Wanneer in apparaat Explorer (Zie hieronder) bekeken, kunt u controleren of deze kenmerken juist zijn ingesteld.

    ![Schermopname voor stap 2][img-query-avro-data-2]

3. Een Azure Data Lake Store (ADLS) en een Azure Data Lake Analytics-instantie instellen. Terwijl Azure IoT Hub niet naar een Azure Data Lake Store doorsturen, is een ADLA vereist.

    ![Schermopname voor stap 3][img-query-avro-data-3]

4. In ADLA, configureert u de Azure Blob Storage als een extra store, dezelfde Blob-opslag waarmee gegevens naar Azure IoT Hub worden doorgestuurd.

    ![Schermopname voor stap 4][img-query-avro-data-4]
 
5. Zoals beschreven in [U-SQL Avro-voorbeeld], er zijn 4 dll-bestanden die nodig zijn.  Deze bestanden uploaden naar een locatie in uw ADLS.

    ![Schermopname voor stap 5][img-query-avro-data-5] 

6. Maak in Visual Studio een U-SQL-Project
 
    ![Schermopname voor stap 6][img-query-avro-data-6]

7. De inhoud van het volgende script Kopieer en plak deze in de zojuist gemaakte bestand. Wijzigen van de 3 gemarkeerde secties: uw account ADLA, de bijbehorende dll paden en het juiste pad voor uw Opslagaccount.
    
    ![Schermopname voor stap 7][img-query-avro-data-7a]

    Het werkelijke U-SQL-script voor eenvoudige uitvoer naar CSV:
    
    ```sql
        DROP ASSEMBLY IF EXISTS [Avro];
        CREATE ASSEMBLY [Avro] FROM @"/Assemblies/Avro/Avro.dll";
        DROP ASSEMBLY IF EXISTS [Microsoft.Analytics.Samples.Formats];
        CREATE ASSEMBLY [Microsoft.Analytics.Samples.Formats] FROM @"/Assemblies/Avro/Microsoft.Analytics.Samples.Formats.dll";
        DROP ASSEMBLY IF EXISTS [Newtonsoft.Json];
        CREATE ASSEMBLY [Newtonsoft.Json] FROM @"/Assemblies/Avro/Newtonsoft.Json.dll";
        DROP ASSEMBLY IF EXISTS [log4net];
        CREATE ASSEMBLY [log4net] FROM @"/Assemblies/Avro/log4net.dll";

        REFERENCE ASSEMBLY [Newtonsoft.Json];
        REFERENCE ASSEMBLY [log4net];
        REFERENCE ASSEMBLY [Avro];
        REFERENCE ASSEMBLY [Microsoft.Analytics.Samples.Formats];

        // Blob container storage account filenames, with any path
        DECLARE @input_file string = @"wasb://hottubrawdata@kevinsayazstorage/kevinsayIoT/{*}/{*}/{*}/{*}/{*}/{*}";
        DECLARE @output_file string = @"/output/output.csv";

        @rs =
        EXTRACT
        EnqueuedTimeUtc string,
        Body byte[]
        FROM @input_file

        USING new Microsoft.Analytics.Samples.Formats.ApacheAvro.AvroExtractor(@"
        {
        ""type"":""record"",
        ""name"":""Message"",
        ""namespace"":""Microsoft.Azure.Devices"",
        ""fields"":[{
        ""name"":""EnqueuedTimeUtc"",
        ""type"":""string""
        },
        {
        ""name"":""Properties"",
        ""type"":{
        ""type"":""map"",
        ""values"":""string""
        }
        },
        {
        ""name"":""SystemProperties"",
        ""type"":{
        ""type"":""map"",
        ""values"":""string""
        }
        },
        {
        ""name"":""Body"",
        ""type"":[""null"",""bytes""]
        }
        ]
        }");

        @cnt =
        SELECT EnqueuedTimeUtc AS time, Encoding.UTF8.GetString(Body) AS jsonmessage
        FROM @rs;

        OUTPUT @cnt TO @output_file USING Outputters.Text(); 
    ```    

    Het onderstaande script wordt uitgevoerd, ADLA 5 minuten duurde wanneer tot 10 analytische eenheden beperkt en 177 bestanden verwerkt, samenvatting van de uitvoer naar een CSV-bestand.
    
    ![Schermopname voor stap 7 ter][img-query-avro-data-7b]

    De uitvoer bekijkt, ziet u dat de Avro-inhoud is geconverteerd naar een CSV-bestand. Doorgaan naar stap 8 als u wilt de JSON parseren.
    
    ![Schermopname voor stap 7c][img-query-avro-data-7c]

    
8. De meeste IoT berichten zijn in JSON-indeling.  De volgende regels toe te voegen, kunt u parseren het bericht naar JSON, zodat u kunt de component WHERE toevoegen en uitvoer alleen de benodigde gegevens.

    ```sql
       @jsonify = SELECT Microsoft.Analytics.Samples.Formats.Json.JsonFunctions.JsonTuple(Encoding.UTF8.GetString(Body)) AS message FROM @rs;
    
        /*
        @cnt =
            SELECT EnqueuedTimeUtc AS time, Encoding.UTF8.GetString(Body) AS jsonmessage
            FROM @rs;
        
        OUTPUT @cnt TO @output_file USING Outputters.Text();
        */
        
        @cnt =
            SELECT message["message"] AS iotmessage,
                   message["event"] AS msgevent,
                   message["object"] AS msgobject,
                   message["status"] AS msgstatus,
                   message["host"] AS msghost
            FROM @jsonify;
            
        OUTPUT @cnt TO @output_file USING Outputters.Text();
    ```

9. De uitvoer bekijkt, ziet u nu kolommen voor elk item in de select-opdracht. 
    
    ![Schermopname voor stap 8][img-query-avro-data-8]

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u geleerd hoe Avro-gegevens voor het doorsturen van berichten van Azure IoT Hub tot Azure-services efficiënt opvragen.

Zie voor voorbeelden van volledige end-to-end-oplossingen die gebruikmaken van IoT Hub [oplossingsverbetering Azure IoT externe controle][lnk-iot-sa-land].

Zie voor meer informatie over het ontwikkelen van oplossingen met IoT Hub, de [Ontwikkelaarshandleiding voor IoT Hub].

Zie voor meer informatie over het routeren van berichten van IoT-Hub, [berichten verzenden en ontvangen met IoT Hub][lnk-devguide-messaging].

<!-- Images -->
[img-query-avro-data-1a]: ./media/iot-hub-query-avro-data/query-avro-data-1a.png
[img-query-avro-data-1b]: ./media/iot-hub-query-avro-data/query-avro-data-1b.png
[img-query-avro-data-2]: ./media/iot-hub-query-avro-data/query-avro-data-2.png
[img-query-avro-data-3]: ./media/iot-hub-query-avro-data/query-avro-data-3.png
[img-query-avro-data-4]: ./media/iot-hub-query-avro-data/query-avro-data-4.png
[img-query-avro-data-5]: ./media/iot-hub-query-avro-data/query-avro-data-5.png
[img-query-avro-data-6]: ./media/iot-hub-query-avro-data/query-avro-data-6.png
[img-query-avro-data-7a]: ./media/iot-hub-query-avro-data/query-avro-data-7a.png
[img-query-avro-data-7b]: ./media/iot-hub-query-avro-data/query-avro-data-7b.png
[img-query-avro-data-7c]: ./media/iot-hub-query-avro-data/query-avro-data-7c.png
[img-query-avro-data-8]: ./media/iot-hub-query-avro-data/query-avro-data-8.png

<!-- Links -->
[Azure IoT Hub berichtroutering: nu met de routering op berichttekst]: https://azure.microsoft.com/blog/iot-hub-message-routing-now-with-routing-on-message-body/

[Routing on message bodies]: iot-hub-devguide-query-language.md#routing-on-message-bodies
[When using Azure storage containers]:iot-hub-devguide-endpoints.md#when-using-azure-storage-containers

[U-SQL Avro-voorbeeld]:https://github.com/Azure/usql/tree/master/Examples/AvroExamples

[lnk-iot-sa-land]: ../iot-accelerators/index.md
[Ontwikkelaarshandleiding voor IoT Hub]: iot-hub-devguide.md
[lnk-devguide-messaging]: iot-hub-devguide-messaging.md
