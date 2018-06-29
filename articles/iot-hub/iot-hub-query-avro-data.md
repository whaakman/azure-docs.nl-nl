---
title: Avro-gegevens opvragen met behulp van Azure Data Lake Analytics | Microsoft Docs
description: Eigenschappen van de hoofdtekst van bericht apparaattelemetrie doorgestuurd naar Blob storage en opvragen van de gegevens van de Avro-indeling die geschreven naar Blob storage gebruiken.
services: iot-hub
documentationcenter: ''
author: ksaye
manager: obloch
ms.service: iot-hub
ms.topic: article
ms.date: 05/29/2018
ms.author: Kevin.Saye
ms.openlocfilehash: c56b567498047ee996018675134c252ec1de7e0c
ms.sourcegitcommit: d1eefa436e434a541e02d938d9cb9fcef4e62604
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2018
ms.locfileid: "37081365"
---
# <a name="query-avro-data-by-using-azure-data-lake-analytics"></a>Avro-gegevens opvragen met behulp van Azure Data Lake Analytics

In dit artikel wordt beschreven hoe een query over gegevens van de Avro om efficiënt routeren van berichten uit Azure IoT Hub tot Azure-services. Als er in het blogbericht aangekondigd [Azure IoT Hub berichtroutering: nu met de routering op berichttekst], IoT-Hub ondersteunt routering op eigenschappen of de berichttekst. Zie voor meer informatie [routering op de berichttekst][Routing on message bodies]. 

De uitdaging is dat wanneer Azure IoT Hub berichten worden doorgestuurd naar Azure Blob-opslag, schrijft IoT Hub de inhoud in de Avro-indeling zowel een eigenschap van de hoofdtekst van bericht en een berichteigenschap heeft. IoT Hub het schrijven van gegevens naar Blob storage ondersteunt alleen in de indeling van het Avro-gegevens en deze indeling wordt niet gebruikt voor alle andere eindpunten. Zie voor meer informatie [bij gebruik van Azure Storage-containers][When using Azure storage containers]. Hoewel de Avro-indeling ideaal voor het behoud van gegevens en het bericht is, is een challenge gebruiken om te gegevens opvragen. Ter vergelijking: JSON- of CSV-indeling, het is veel eenvoudiger is voor het opvragen van gegevens.

Voor niet-relationele big data-behoeften en indelingen op te lossen en deze uitdaging ondervangen, kunt u veel van de big data-patronen voor zowel transformeren en schalen van gegevens. Een van de patronen 'betalen per query' is Azure Data Lake Analytics, de focus van dit artikel. Hoewel eenvoudig u de query in Hadoop of andere oplossingen uitvoeren kunt, is het Data Lake Analytics vaak beter geschikt voor deze benadering 'betalen per query'. 

Er is een zelfstandig 'uitpakken' voor Avro in U-SQL. Zie voor meer informatie [U-SQL-Avro-voorbeeld].

## <a name="query-and-export-avro-data-to-a-csv-file"></a>Vragen en Avro-gegevens exporteren naar een CSV-bestand
In deze sectie Avro-gegevens opvragen en exporteren naar een CSV-bestand in Azure Blob storage, hoewel u eenvoudig de gegevens in andere winkels-opslagplaatsen of gegevens kan plaatsen.

1. Instellen van Azure IoT Hub gegevens routeren naar een Azure Blob storage-eindpunt met behulp van een eigenschap in de berichttekst berichten te selecteren.

    ![De sectie 'Aangepaste eindpunten'][img-query-avro-data-1a]

    ![De opdracht Routes][img-query-avro-data-1b]

2. Zorg ervoor dat uw apparaat de codering, type inhoud en benodigde gegevens in de eigenschappen of de hoofdtekst van het bericht waarnaar wordt verwezen in de productdocumentatie heeft. Wanneer u deze kenmerken in Verkenner apparaat weergeven zoals hier wordt weergegeven, kunt u controleren of ze juist zijn ingesteld.

    ![Het deelvenster Event Hub-gegevens][img-query-avro-data-2]

3. Instellen van een Azure Data Lake Store-exemplaar en een exemplaar van Data Lake Analytics. Azure IoT Hub geen routering naar een Data Lake Store-exemplaar, maar een exemplaar van Data Lake Analytics is een vereist.

    ![Data Lake Store- en Data Lake Analytics-instanties][img-query-avro-data-3]

4. In Data Lake Analytics Azure Blob-opslag te configureren als een extra store, dezelfde Blob-opslag waarmee gegevens naar Azure IoT Hub worden doorgestuurd.

    ![Het deelvenster 'Gegevensbronnen'][img-query-avro-data-4]
 
5. Zoals beschreven in [U-SQL-Avro-voorbeeld], moet u vier dll-bestanden. Deze bestanden uploaden naar een locatie in uw exemplaar van Data Lake Store.

    ![Vier geladen DLL-bestanden][img-query-avro-data-5] 

6. Maak een U-SQL-project in Visual Studio.
 
    ![Een U-SQL-project maken][img-query-avro-data-6]

7. Plak de inhoud van het volgende script in de zojuist gemaakte bestand. Wijzigen van de drie secties van de gemarkeerde: uw Data Lake Analytics-account, de bijbehorende DLL-bestandspaden en het juiste pad voor uw opslagaccount.
    
    ![De drie secties worden gewijzigd][img-query-avro-data-7a]

    Het werkelijke U-SQL-script voor eenvoudige uitvoer naar een CSV-bestand:
    
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

    Het Data Lake Analytics heeft vijf minuten om uit te voeren van het volgende script dat is beperkt tot 10 analytische eenheden en 177 bestanden verwerkt. Het resultaat wordt weergegeven in de uitvoer van de CSV-bestand die wordt weergegeven in de volgende afbeelding:
    
    ![Resultaten van de uitvoer naar CSV-bestand][img-query-avro-data-7b]

    ![Uitvoer geconverteerd naar een CSV-bestand][img-query-avro-data-7c]

    Voor het parseren van de JSON, Ga naar stap 8.
    
8. De meeste IoT berichten zijn in JSON-bestandsindeling. U kunt het bericht in een JSON-bestand waarmee u de component WHERE toevoegen en de uitvoer van alleen de benodigde gegevens parseren door de volgende regels toe te voegen.

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

    De uitvoer geeft aan een kolom voor elk item in de `SELECT` opdracht. 
    
    ![Uitvoer kolommen voor elk item weergeven][img-query-avro-data-8]

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u geleerd hoe u een query over gegevens van de Avro om efficiënt routeren van berichten uit Azure IoT Hub tot Azure-services.

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

[U-SQL-Avro-voorbeeld]:https://github.com/Azure/usql/tree/master/Examples/AvroExamples

[lnk-iot-sa-land]: ../iot-accelerators/index.yml
[Ontwikkelaarshandleiding voor IoT Hub]: iot-hub-devguide.md
[lnk-devguide-messaging]: iot-hub-devguide-messaging.md
