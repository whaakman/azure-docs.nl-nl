---
title: Avro-gegevens op te vragen met behulp van Azure Data Lake Analytics | Microsoft Docs
description: Eigenschappen van de hoofdtekst van bericht gebruiken voor het doorsturen van telemetrie van apparaten naar Blob-opslag en de Avro-indeling gegevens die worden geschreven naar Blob-opslag op te vragen.
author: ash2017
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: asrastog
ms.openlocfilehash: 69c890cfc3db04fe625ed7ad008f545c01844834
ms.sourcegitcommit: fd488a828465e7acec50e7a134e1c2cab117bee8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2019
ms.locfileid: "53993481"
---
# <a name="query-avro-data-by-using-azure-data-lake-analytics"></a>Avro-gegevens opvragen met behulp van Azure Data Lake Analytics

In dit artikel wordt beschreven hoe u Avro-gegevens voor het efficiënt te routeren van berichten van Azure IoT Hub met Azure-services op te vragen. [Message Routing](iot-hub-devguide-messages-d2c.md) kunt u voor het filteren van gegevens met behulp van uitgebreide query's op basis van eigenschappen van berichten, hoofdtekst van het bericht apparaat apparaatdubbel-tags en apparaatdubbeleigenschappen. Zie het artikel over bericht routering query-syntaxis voor meer informatie over de mogelijkheden bij het controleren van het bericht doorsturen. 
<!--[Message Routing Query Syntax](iot-hub-devguide-routing-query-syntax.md). I don't have this article yet. -->

IoT Hub schrijft dat de uitdaging is dat wanneer u Azure IoT Hub berichten naar Azure Blob-opslag worden doorgestuurd, de inhoud in Avro-indeling, die zowel een eigenschap van de hoofdtekst van bericht als een berichteigenschap is. IoT Hub schrijven van gegevens naar Blob-opslag alleen in de indeling van de Avro ondersteunt, en deze indeling wordt niet gebruikt voor alle andere eindpunten. Zie voor meer informatie, een artikel over het gebruik van Azure Storage-containers. Hoewel de Avro-indeling ideaal voor het behoud van gegevens en het bericht is, is een uitdaging om te gebruiken om gegevens te doorzoeken. Ter vergelijking: JSON- of CSV-indeling, het is veel eenvoudiger is voor het opvragen van gegevens.

<!-- https://review.docs.microsoft.com/azure/iot-hub/iot-hub-devguide-messages-d2c?branch=pr-en-us-51566#azure-blob-storage  NEW LINK FOR 'WHEN USING STORAGE CONTAINERS' -->

Voor het adres van de behoeften van niet-relationele big data en indelingen en strijden tegen deze uitdaging, kunt u veel van de big data-patronen voor zowel transformeren en schalen van gegevens. Een van de patronen 'betalen per query', is Azure Data Lake Analytics, wat het hoofdonderwerp van dit artikel. Hoewel u kunt eenvoudig de query worden uitgevoerd in Hadoop- of andere oplossingen, Data Lake Analytics is vaak beter geschikt zijn voor deze benadering 'betalen per query'. 

Er is een 'extractor' voor Avro in U-SQL. Zie voor meer informatie, [U-SQL-Avro voorbeeld](https://github.com/Azure/usql/tree/master/Examples/AvroExamples).

## <a name="query-and-export-avro-data-to-a-csv-file"></a>Query's uitvoeren en Avro-gegevens exporteren naar een CSV-bestand
In deze sectie maakt u Avro-gegevens op te vragen en exporteren naar een CSV-bestand in Azure Blob-opslag, hoewel u eenvoudig de gegevens in andere opslagplaatsen of gegevensarchieven kan plaatsen.

1. Instellen van Azure IoT Hub gegevens routeren naar een Azure Blob storage-eindpunt met behulp van een eigenschap in de berichttekst berichten te selecteren.

   ![De sectie "Aangepaste eindpunten"](./media/iot-hub-query-avro-data/query-avro-data-1a.png)

   ![De regels voor doorsturen](./media/iot-hub-query-avro-data/query-avro-data-1b.png)

   Zie voor meer informatie over instellingen van routes en aangepaste eindpunten [voor een IoT-hub-berichtroutering](iot-hub-create-through-portal.md#message-routing-for-an-iot-hub).

2. Zorg ervoor dat uw apparaat de codering, inhoudstype en benodigde gegevens in de eigenschappen of de berichttekst, waarnaar wordt verwezen in de productdocumentatie heeft. Wanneer u deze kenmerken in Device Explorer, weergeven zoals hier wordt weergegeven, kunt u controleren of ze correct zijn ingesteld.

   ![Het deelvenster gegevens van Event Hub](./media/iot-hub-query-avro-data/query-avro-data-2.png)

3. Instellen van een Azure Data Lake Store-exemplaar en een Data Lake Analytics-exemplaar. Azure IoT Hub niet gerouteerd naar een Data Lake Store-exemplaar, maar een exemplaar van Data Lake Analytics is een vereist.

   ![Data Lake Store en Data Lake Analytics-instanties](./media/iot-hub-query-avro-data/query-avro-data-3.png)

4. In Data Lake Analytics, Azure Blob-opslag te configureren als een aanvullende store, de Blob-opslag waarmee gegevens naar Azure IoT Hub worden doorgestuurd.

   ![Het deelvenster "Gegevensbronnen"](./media/iot-hub-query-avro-data/query-avro-data-4.png)
 
5. Zoals beschreven in de [U-SQL-Avro voorbeeld](https://github.com/Azure/usql/tree/master/Examples/AvroExamples), moet u vier DLL-bestanden. Deze bestanden uploaden naar een locatie in uw Data Lake Store-exemplaar.

   ![Vier geladen DLL-bestanden](./media/iot-hub-query-avro-data/query-avro-data-5.png)

6. Maak een U-SQL-project in Visual Studio.
 
   ! Maken van een U-SQL-project](./media/iot-hub-query-avro-data/query-avro-data-6.png)

7. Plak de inhoud van het volgende script in het zojuist gemaakte bestand. Wijzigen van de drie gemarkeerde secties: uw Data Lake Analytics-account, de bijbehorende DLL-bestandspaden en het juiste pad voor uw opslagaccount.
    
   ![De drie secties om te worden gewijzigd](./media/iot-hub-query-avro-data/query-avro-data-7a.png)

   De werkelijke U-SQL-script voor eenvoudige uitvoer naar een CSV-bestand:
    
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
            ""fields"":
           [{
                ""name"":""EnqueuedTimeUtc"",
                ""type"":""string""
            },
            {
                ""name"":""Properties"",
                ""type"":
                {
                    ""type"":""map"",
                    ""values"":""string""
                }
            },
            {
                ""name"":""SystemProperties"",
                ""type"":
                {
                    ""type"":""map"",
                    ""values"":""string""
                }
            },
            {
                ""name"":""Body"",
                ""type"":[""null"",""bytes""]
            }]
        }"
        );

        @cnt =
        SELECT EnqueuedTimeUtc AS time, Encoding.UTF8.GetString(Body) AS jsonmessage
        FROM @rs;

        OUTPUT @cnt TO @output_file USING Outputters.Text(); 
    ```    

    Het Data Lake Analytics heeft vijf minuten om uit te voeren van het volgende script, dat is beperkt tot 10 Analytics units en 177 bestanden wordt verwerkt. Het resultaat wordt weergegeven in de uitvoer van CSV-bestand dat wordt weergegeven in de volgende afbeelding:
    
    ![Resultaten van de uitvoer naar CSV-bestand](./media/iot-hub-query-avro-data/query-avro-data-7b.png)

    ![Uitvoer geconverteerd naar een CSV-bestand](./media/iot-hub-query-avro-data/query-avro-data-7c.png)

    Voor het parseren van de JSON, verder met stap 8.
    
8. De meeste IoT-berichten zijn in JSON-bestand-indeling. Door de volgende regels toe te voegen, kunt u het bericht in een JSON-bestand, waarmee u de component WHERE toevoegen en voert u alleen de benodigde gegevens parseren.

    ```sql
       @jsonify = 
         SELECT Microsoft.Analytics.Samples.Formats.Json.JsonFunctions.JsonTuple(Encoding.UTF8.GetString(Body)) 
           AS message FROM @rs;
    
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
    
    ![Uitvoer van een kolom voor elk item](./media/iot-hub-query-avro-data/query-avro-data-8.png)

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe u query's Avro-gegevens om efficiënt te routeren van berichten van Azure IoT Hub met Azure-services.

Zie voor meer voorbeelden van volledige end-to-end-oplossingen die gebruikmaken van IoT Hub, de [documentatie voor IoT-oplossingsversnellers Azure](/azure/iot-accelerators).

Zie voor meer informatie over het ontwikkelen van oplossingen met IoT Hub, de [Ontwikkelaarshandleiding voor IoT Hub](iot-hub-devguide.md).

Zie voor meer informatie over berichtroutering in IoT Hub, [berichten verzenden en ontvangen met IoT Hub](iot-hub-devguide-messaging.md).
