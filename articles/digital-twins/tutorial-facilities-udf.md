---
title: Een ruimte controleren met Azure Digital Twins | Microsoft Docs
description: Leer hoe u uw ruimtelijke resources kunt inrichten en de werkomstandigheden kunt controleren met Azure Digital Twins door de stappen in deze zelfstudie te volgen.
services: digital-twins
author: dsk-2015
ms.service: digital-twins
ms.topic: tutorial
ms.date: 10/15/2018
ms.author: dkshir
ms.openlocfilehash: 1e5cb18b4e526cd0a0607f5bc93788fcf07430e1
ms.sourcegitcommit: 6361a3d20ac1b902d22119b640909c3a002185b3
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/17/2018
ms.locfileid: "49364231"
---
# <a name="tutorial-provision-your-building-and-monitor-working-conditions-with-azure-digital-twins"></a>Zelfstudie: Uw gebouw inrichten en uw werkomstandigheden controleren met Azure Digital Twins

Deze zelfstudie laat zien hoe u Azure Digital Twins gebruikt voor het controleren van ruimten zodat daar de gewenste temperatuur en het gewenste comfortniveau heerst. Zodra u een [voorbeeldgebouw hebt geconfigureerd](tutorial-facilities-setup.md), kunt u een gebouw inrichten en aangepaste functies uitvoeren op uw sensorgegevens met behulp van de stappen in deze zelfstudie.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Werkomstandigheden definiëren die gecontroleerd moeten worden
> * Een door de gebruiker gedefinieerde functie maken
> * Sensorgegevens simuleren
> * Resultaten ophalen van een door de gebruiker gedefinieerde functie

## <a name="prerequisites"></a>Vereisten

In deze zelfstudie wordt ervan uitgegaan dat u de [Azure Digital Twins-installatie hebt geconfigureerd](tutorial-facilities-setup.md). Voordat u doorgaat, moet u ervoor zorgen voor dat u beschikt over:
- Een [Azure-account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).
- Een actieve instantie van Digital Twins. 
- De gedownloade en uitgepakte [Digital Twins C#-voorbeelden](https://github.com/Azure-Samples/digital-twins-samples-csharp) op een werkcomputer. 
- [.NET Core SDK-versie 2.1.403 of hoger](https://www.microsoft.com/net/download) op een ontwikkelcomputer om het voorbeeld te bouwen en uit te voeren. Voer `dotnet --version` uit om te controleren of de juiste versie is geïnstalleerd. 
- [Visual Studio Code](https://code.visualstudio.com/) om de voorbeeldcode mee te verkennen. 

## <a name="define-conditions-to-monitor"></a>Werkomstandigheden definiëren die gecontroleerd moeten worden
U kunt een reeks specifieke omstandigheden in het apparaat of sensorgegevens definiëren die moeten worden gecontroleerd. Deze worden **Matchers** genoemd. Vervolgens kunt u functies definiëren die de *door de gebruiker gedefinieerde functies* worden genoemd, waarbij aangepaste logica wordt uitgevoerd op gegevens die afkomstig zijn van uw ruimten en apparaten, wanneer de omstandigheden zich voordoen die overeenkomen met wat er als matchers is opgegeven. Lees [Gegevensverwerking en door gebruikers gedefinieerde functies](concepts-user-defined-functions.md) voor meer informatie. 

Open vanuit het voorbeeldproject **_occupancy--quickstart_** het bestand **_src\actions\provisionSample.yaml_**  in Visual Studio Code. Let op de sectie die begint met het type **matchers**. Met elk item onder dit type wordt een matcher gemaakt met de opgegeven **naam**, waarmee een sensor wordt gecontroleerd van het type **dataTypeValue**. Kijk hoe deze zich verhoudt tot de ruimte met de naam *Focus Room A1*, die een knooppunt **devices** heeft waarin zich een aantal **sensors** bevinden. Als u een matcher wilt inrichten om een van deze sensoren te volgen, moet de bijbehorende **dataTypeValue** overeenkomen met het **dataType** van de sensor. 

Voeg de volgende matcher toe onder aan de huidige matchers, en zorg er daarbij voor dat de sleutels zijn uitgelijnd en de ruimten niet door tabs zijn vervangen:

```yaml
      - name: Matcher Temperature
        dataTypeValue: Temperature
```

Als gevolg hiervan wordt de sensor *SAMPLE_SENSOR_TEMPERATURE* gevolgd die u in [de eerste zelfstudie](tutorial-facilities-setup.md) hebt toegevoegd. Houd er rekening mee dat deze regels ook aanwezig zijn in *provisionSample.yaml* in de vorm van regels met opmerkingen; u kunt er eenvoudig voor zorgen dat deze niet als opmerkingen worden gezien, door het teken '#' aan het begin van elke regel te verwijderen. 

<a id="udf" />

## <a name="create-a-user-defined-function"></a>Een door de gebruiker gedefinieerde functie maken
Met behulp van door gebruikers gedefinieerde functies kunt u aanpassen hoe uw sensorgegevens worden verwerkt. Ze bestaan uit aangepaste JavaScript-code die in uw Digital Twins-instantie wordt uitgevoerd als er zich omstandigheden voordoen die door de matchers worden beschreven. U kunt *matchers* en *door gebruikers gedefinieerde functies* maken voor elke sensor die u wilt controleren. Lees [Gegevensverwerking en door gebruikers gedefinieerde functies](concepts-user-defined-functions.md) voor meer informatie. 

Zoek in het voorbeeldbestand *provisionSample.yaml* naar een sectie die begint met het type **userdefinedfunctions**. In deze sectie wordt een door de gebruiker gedefinieerde functie met een bepaalde **naam** ingericht, die wordt geactiveerd op basis van de lijst met matchers onder **matcherNames**. Kijk ook hoe u een eigen JavaScript-kunt gebruiken als **script** voor de door de gebruiker gedefinieerde functie. Let ook op de sectie met de naam **roleassignments**. Hiermee wordt de rol van *Ruimtebeheerder* toegewezen aan de door de gebruiker gedefinieerde functie, waardoor deze toegang krijgt tot gebeurtenissen die afkomstig zijn uit een van de ingerichte ruimten. 

1. Configureer de door de gebruiker gedefinieerde functie zodanig dat de temperatuurmatcher erin wordt opgenomen door de volgende regel in het knooppunt `matcherNames` aan het bestand *provisionSample.yaml* toe te voegen, of door ervoor te zorgen dat deze regel niet als opmerking wordt gezien:

    ```yaml
            - Matcher Temperature
    ```

1. Open het bestand  **_src\actions\userDefinedFunctions\availability.js_** in een editor. Dit is het bestand waarnaar wordt verwezen door het element **script** in het bestand *provisionSample.yaml*. Met de door gebruiker gedefinieerde functie in dit bestand wordt gezocht naar omstandigheden waarbij geen beweging in de kamer wordt gedetecteerd en waarbij het koolstofdioxideniveau zich onder 1000 ppm bevindt. Wijzig het JavaScript-bestand zodat naast andere omstandigheden ook op temperatuur wordt gecontroleerd. Voeg de volgende regels code toe om te zoeken naar omstandigheden waarbij geen beweging wordt gedetecteerd in de kamer, het koolstofdioxideniveau lager is dan 1000 ppm en de temperatuur lager is dan 78 graden Fahrenheit.

   > [!NOTE]
   > In deze sectie wordt het bestand *src\actions\userDefinedFunctions\availability.js* gewijzigd, zodat u in detail ervaart hoe u een door de gebruiker gedefinieerde functie kunt schrijven. U kunt er echter voor kiezen om het bestand [src\actions\userDefinedFunctions\availabilityForTutorial.js](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/occupancy-quickstart/src/actions/userDefinedFunctions/availabilityForTutorial.js) in uw installatie direct te gebruiken. Dit bestand bevat alle wijzigingen die u voor deze zelfstudie nodig hebt. Als u in de plaats daarvan dit bestand gebruikt, moet u ervoor zorgen dat u de juiste bestandsnaam gebruikt voor de **_script_**-sleutel in [src\actions\provisionSample.yaml](https://github.com/Azure-Samples/digital-twins-samples-csharp/blob/master/occupancy-quickstart/src/actions/provisionSample.yaml).

    1. Boven in het bestand moet u onder de opmerking `// Add your sensor type here` de volgende regels voor de temperatuur toevoegen:

        ```JavaScript
            var temperatureType = "Temperature";
            var temperatureThreshold = 78;
        ```
   
    1. Voeg onder de opmerking `// Add your sensor variable here` de volgende regels toe na de instructie waarmee `var motionSensor` wordt gedefinieerd:

        ```JavaScript
            var temperatureSensor = otherSensors.find(function(element) {
                return element.DataType === temperatureType;
            });
        ```
    
    1. Voeg onder de opmerking `// Add your sensor latest value here` de volgende regel toe na de instructie waarmee `var carbonDioxideValue` wordt gedefinieerd:

        ```JavaScript
            var temperatureValue = getFloatValue(temperatureSensor.Value().Value);
        ```
    
    1. Verwijder de volgende regels code onder de opmerking `// Modify this line to monitor your sensor value`: 

        ```JavaScript
            if(carbonDioxideValue === null || motionValue === null) {
                sendNotification(telemetry.SensorId, "Sensor", "Error: Carbon dioxide or motion are null, returning");
                return;
            }
        ```
       
       Vervang deze door de volgende regels:

        ```JavaScript
            if(carbonDioxideValue === null || motionValue === null || temperatureValue === null){
                sendNotification(telemetry.SensorId, "Sensor", "Error: Carbon dioxide, motion, or temperature are null, returning");
                return;
            }
        ```
    
    1. Verwijder de volgende regels code onder de opmerking `// Modify these lines as per your sensor`:

        ```JavaScript
            var availableFresh = "Room is available and air is fresh";
            var noAvailableOrFresh = "Room is not available or air quality is poor";
        ```

       Vervang deze door de volgende regels:

        ```JavaScript
            var alert = "Room with fresh air and comfortable temperature is available.";
            var noAlert = "Either room is occupied, or working conditions are not right.";
        ```
    
    1. Verwijder het volgende *if-else*-codeblok na de opmerking `// Modify this code block for your sensor`:

        ```JavaScript
            // If carbonDioxide less than threshold and no presence in the room => log, notify and set parent space computed value
            if(carbonDioxideValue < carbonDioxideThreshold && !presence) {
                log(`${availableFresh}. Carbon Dioxide: ${carbonDioxideValue}. Presence: ${presence}.`);
                setSpaceValue(parentSpace.Id, spaceAvailFresh, availableFresh);

                // Set up custom notification for air quality
                parentSpace.Notify(JSON.stringify(availableFresh));
            }
            else {
                log(`${noAvailableOrFresh}. Carbon Dioxide: ${carbonDioxideValue}. Presence: ${presence}.`);
                setSpaceValue(parentSpace.Id, spaceAvailFresh, noAvailableOrFresh);

                // Set up custom notification for air quality
                parentSpace.Notify(JSON.stringify(noAvailableOrFresh));
            }
        ```
      
       En vervang dit door het volgende *if-else*-codeblok:

        ```JavaScript
            // If sensor values are within range and room is available
            if(carbonDioxideValue < carbonDioxideThreshold && temperatureValue < temperatureThreshold && !presence) {
                log(`${alert}. Carbon Dioxide: ${carbonDioxideValue}. Temperature: ${temperatureValue}. Presence: ${presence}.`);

                // log, notify and set parent space computed value
                setSpaceValue(parentSpace.Id, spaceAvailFresh, alert);

                // Set up notification for this alert
                parentSpace.Notify(JSON.stringify(alert));
            }
            else {
                log(`${noAlert}. Carbon Dioxide: ${carbonDioxideValue}. Temperature: ${temperatureValue}. Presence: ${presence}.`);
    
                // log, notify and set parent space computed value
                setSpaceValue(parentSpace.Id, spaceAvailFresh, noAlert);
            }
        ```
        
        Met de gewijzigde door de gebruiker gedefinieerde functie wordt gezocht naar omstandigheden waarbij een kamer beschikbaar wordt en het koolstofdioxideniveau en de temperatuur ervan binnen de toegestane limieten vallen. Er wordt een melding met de instructie `parentSpace.Notify(JSON.stringigy(alert));` gegenereerd als deze omstandigheid zich voordoet. Met het bijbehorende bericht wordt de waarde van de gecontroleerde ruimte ingesteld, ongeacht of de omstandigheid zich voordoet of niet.
    
    1. Sla het bestand op. 
    
1. Open het opdrachtvenster en navigeer naar de map **_occupancy-quickstart\src_**. Voer de volgende opdracht uit om de grafiek met gegevens over de ruimte en de door de gebruiker gedefinieerde functie in te richten. 

    ```cmd/sh
    dotnet run ProvisionSample
    ```

   > [!IMPORTANT]
   > Om te voorkomen dat onbevoegden toegang hebben tot uw beheer-API van Digital Twins, wordt u door de toepassing **_occupancy-quickstart_** gedwongen u aan te melden met uw Azure-accountreferenties. Uw referenties worden gedurende een korte periode bewaard, zodat u zich niet steeds hoeft aan te melden als u deze wilt uitvoeren. De eerste keer dat dit programma wordt uitgevoerd, en wanneer uw opgeslagen referenties kort daarna zijn verlopen, wordt u omgeleid naar een aanmeldingspagina en ontvangt u een code die u op die pagina moet invoeren. Volg de aanwijzingen om u aan te melden met uw Azure-account.


1. Als uw account is geverifieerd, start de toepassing door een ruimtelijke grafiek te maken zoals is geconfigureerd in *provisionSample.yaml*. Wacht totdat het inrichten is voltooid; dit duurt een paar minuten. Nadat het inrichten is voltooid, bekijkt u de berichten in het opdrachtvenster en u ziet hoe uw ruimtelijke grafiek wordt gemaakt. Observeer hoe een IoT-hub wordt gemaakt op het hoofdknooppunt of de `Venue`. 

1. Kopieer de waarde van de `ConnectionString` uit de uitvoer in het opdrachtvenster onder de sectie `Devices` naar het Klembord. U hebt deze waarde nodig om de apparaatverbinding in de volgende sectie te simuleren.

    ![Voorbeeld van inrichting](./media/tutorial-facilities-udf/run-provision-sample.png)

> [!TIP]
> Als er tijdens het inrichtingsproces een foutbericht wordt weergegeven dat overeenkomt met ‘De I/O-bewerking is afgebroken vanwege een afgesloten thread of een toepassingsaanvraag', moet u de opdracht opnieuw uitvoeren. Dit kan gebeuren als er een time-out van de HTTP-client heeft plaatsgevonden vanwege een netwerkprobleem.

<a id="simulate" />

## <a name="simulate-sensor-data"></a>Sensorgegevens simuleren
In deze sectie gaat u een project met de naam *device-connectivity* in het voorbeeld gebruiken voor het simuleren van sensorgegevens voor het detecteren van beweging, temperatuur en koolstofdioxide. Door dit project worden willekeurige waarden voor de sensors gegenereerd en worden deze naar de IoT-hub verzonden met behulp van de verbindingsreeks van het apparaat.

1. Ga in een afzonderlijke opdrachtvenster naar het Digital Twins-voorbeeld en ga van daaruit naar de map **_device-connectivity_**.

1. Voer de volgende opdracht uit om te controleren of de afhankelijkheden voor het project juist zijn:

    ```cmd/sh
    dotnet restore
    ```

1. Open het bestand *appSettings.json* in een editor en bewerk de volgende waarden:
    1. *DeviceConnectionString*: wijs de waarde toe van `ConnectionString` in het uitvoervenster uit de vorige sectie. Zorg ervoor dat u de volledige tekenreeks (tussen de aanhalingstekens) kopieert, zodat de simulator correct verbinding maakt met de IoT-hub.

    1. *HardwareId* binnen de matrix *Sensoren*: aangezien u gebeurtenissen van sensoren simuleert die zijn ingericht voor uw instantie van Digital Twins, moeten de hardware-ID en de namen van de sensoren in dit bestand overeenkomen met het knooppunt `sensors` van het bestand *provisionSample.yaml*. Voeg een nieuw gegeven voor de temperatuursensor toe; het knooppunt **Sensoren** in *appSettings.json* moet er als volgt uitzien:

        ```JSON
        "Sensors": [{
          "DataType": "Motion",
          "HardwareId": "SAMPLE_SENSOR_MOTION"
        },{
          "DataType": "CarbonDioxide",
          "HardwareId": "SAMPLE_SENSOR_CARBONDIOXIDE"
        },{
          "DataType": "Temperature",
          "HardwareId": "SAMPLE_SENSOR_TEMPERATURE"
        }]
        ```

1. Voer de volgende opdracht uit om te beginnen met het simuleren van apparaatgebeurtenissen voor temperatuur, beweging en kooldioxide:

    ```cmd/sh
    dotnet run
    ```

   > [!NOTE] 
   > Omdat het simulatievoorbeeld niet rechtstreeks met uw instantie van Digital Twin communiceert, hoeft u zich niet te verifiëren.

## <a name="get-results-of-user-defined-function"></a>Resultaten ophalen van een door de gebruiker gedefinieerde functie
De door de gebruiker gedefinieerde functie wordt telkens uitgevoerd als uw instantie gegevens van het apparaat en de sensor ontvangt. In deze sectie voert de Digital Twins-instantie een query uit om de resultaten van de door de gebruiker gedefinieerde functie op te halen. In bijna realtime ziet u wanneer een kamer beschikbaar is, de lucht vers is en temperatuur klopt. 

1. Open het opdrachtvenster dat u heb gebruikt voor het inrichten van het voorbeeld, of open een nieuw opdrachtvenster, en ga weer naar de map **_occupancy-quickstart\src_** van het voorbeeld. 

1. Voer de volgende opdracht uit en meld u aan wanneer u hierom wordt gevraagd:

    ```cmd/sh
    dotnet run GetAvailableAndFreshSpaces
    ```

In het uitvoervenster ziet u hoe de door gebruiker gedefinieerde functie wordt uitgevoerd en ziet u wanneer er gebeurtenissen van de apparaatsimulatie worden onderschept. 

   ![Een door de gebruiker gedefinieerde functie uitvoeren](./media/tutorial-facilities-udf/udf-running.png)

Afhankelijk van of de controleerde omstandigheid zich voordoet, wordt met de door de gebruiker gedefinieerde functie de waarde van de ruimte ingesteld met behulp van het relevante bericht, zoals u hebt gezien in [de bovenstaande sectie](#udf), en dit wordt door de functie `GetAvailableAndFreshSpaces` op de console weergegeven. 

## <a name="clean-up-resources"></a>Resources opschonen

Als u Azure Digital Twins niet verder wilt verkennen, kunt u de resources die in deze zelfstudie zijn gemaakt, gerust verwijderen:

1. Klik in het linkermenu in de [Azure-portal](http://portal.azure.com) op **Alle resources**, selecteer de Digital Twins-resourcegroep en kies **Verwijderen**.
2. Als u wilt, kunt u ook de voorbeeldtoepassingen op de werkcomputer verwijderen. 


## <a name="next-steps"></a>Volgende stappen

Nu dat u uw ruimten hebt ingericht en een framework voor het activeren van aangepaste meldingen hebt gemaakt, kunt u doorgaan met een van de volgende zelfstudies. 

> [!div class="nextstepaction"]
> [Zelfstudie: Meldingen ontvangen uit uw Azure Digital Twins-ruimten met behulp van logische apps](tutorial-facilities-events.md)

Of

> [!div class="nextstepaction"]
> [Zelfstudie: Gebeurtenissen uit Azure Digital Twins-ruimten visualiseren en analyseren met Time Series Insights](tutorial-facilities-analyze.md)
