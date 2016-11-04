---
title: Aan de slag met apparaatbeheer via IoT Hub | Microsoft Docs
description: Zelfstudie voor apparaatbeheer met C# via Azure IoT Hub. Implementeer apparaatbeheer met behulp van Azure IoT Hub en C# in combinatie met de Microsoft Azure IoT SDK's.
services: iot-hub
documentationcenter: .net
author: juanjperez
manager: timlt
editor: ''

ms.service: iot-hub
ms.devlang: dotnet
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/11/2016
ms.author: juanpere

---
# Aan de slag met apparaatbeheer met node.js via Azure IoT Hub (preview)
[!INCLUDE [iot-hub-device-management-get-started-selector](../../includes/iot-hub-device-management-get-started-selector.md)]

## Inleiding
Als u apparaten wilt gaan beheren met apparaatbeheer via Azure IoT Hub, moet u een Azure IoT Hub maken, apparaten in de IoT Hub inrichten, meerdere gesimuleerde apparaten starten en deze apparaten weergeven in de voorbeeld-UI voor apparaatbeheer. In deze zelfstudie ontdekt u stapsgewijs hoe u dit moet doen.

> [!NOTE]
> U moet een nieuwe IoT Hub maken om apparaatbeheerfuncties in te kunnen schakelen, zelfs als u al een IoT Hub hebt. Dit is omdat bestaande IoT Hubs nog niet over deze functies beschikken. Zodra apparaatbeheer algemeen beschikbaar is, worden alle bestaande IoT Hubs bijgewerkt met apparaatbeheerfuncties.
> 
> 

## Vereisten
In deze zelfstudie wordt ervan uitgegaan dat u een Ubuntu Linux-ontwikkelingscomputer hebt.

De volgende software moet zijn geïnstalleerd om de stappen te kunnen doorlopen:

* Git
* gcc (versie 4.9 of hoger). Met de opdracht `gcc --version` kunt u controleren welke versie momenteel is geïnstalleerd in uw omgeving. Zie <http://askubuntu.com/questions/466651/how-do-i-use-the-latest-gcc-4-9-on-ubuntu-14-04> voor meer informatie over het uitvoeren van een upgrade voor uw versie van gcc op Ubuntu 14.04.
* [CMake](https://cmake.org/download/) (versie 2.8 of hoger). Met de opdracht `cmake --version` kunt u controleren welke versie momenteel is geïnstalleerd in uw omgeving.
* Node.js 6.1.0 of hoger.  Installeer op <https://nodejs.org/> Node.js voor uw platform.
* Een actief Azure-abonnement. Als u geen account hebt, kunt u binnen een paar minuten een account voor de gratis proefversie maken. Zie [Gratis proefversie van Azure][lnk-free-trial] voor meer informatie.

## Een IoT Hub met apparaatbeheerfunctie maken
U moet een IoT Hub met apparaatbeheerfunctie maken waarmee uw gesimuleerde apparaten verbinding kunnen maken. In de volgende stappen ziet u hoe u dit moet doen met behulp van de Azure Portal.

1. Meld u aan bij de [Azure Portal].
2. Klik in de snelbalk op **New**. Klik vervolgens op **Internet of Things** en klik vervolgens op **Azure IoT Hub**.
   
   ![][img-new-hub]
3. In de blade **IoT Hub** kiest u de configuratie voor uw IoT Hub.
   
   ![][img-configure-hub]
   
   * In het vak **Name** voert u een naam voor uw IoT Hub in. Als de **naam** geldig en beschikbaar is, verschijnt er een groen vinkje in het vak **Name**.
   * Selecteer een **prijs- en schaalcategorie**. Voor deze zelfstudie is geen bepaalde categorie vereist.
   * Geef in **Resource group** een nieuwe resourcegroep op of selecteer een bestaande. Zie [Resourcegroepen gebruiken om Azure-resources te beheren] voor meer informatie.
   * Vink het selectievakje **Enable Device Management** aan. Als u het selectievakje **Enable Device Management** niet aanvinkt, werken de voorbeelden niet. Als u **Enable Device Management** inschakelt, maakt u een voorbeeld van een IoT Hub dat alleen wordt ondersteund in het oostelijk deel van de VS, Noord-Europa en Oost-Azië en dat niet is bedoeld voor productiescenario‘s. U kunt apparaten niet migreren naar en vanuit hubs waarvoor apparaatbeheer is ingeschakeld.
   * Bij **Location** selecteert u de locatie waar uw IoT Hub gehost wordt. Apparaatbeheer via IoT Hub is alleen beschikbaar in het oostelijk deel van de VS, Noord-Europa en Oost-Azië tijdens de openbare previewfase. In de toekomst zal de functie beschikbaar zijn voor alle regio's.
4. Wanneer u de configuratieopties voor uw IoT Hub hebt gekozen, klikt u op **Create**. Het kan enkele minuten duren voordat Azure uw IoT Hub heeft gemaakt. Als u de status wilt zien, kunt u de voortgang bekijken via het **Startboard** of in het venster **Notifications**.
   
   ![][img-monitor]
5. Wanneer het gelukt is om een IoT Hub te maken, opent u de blade van de nieuwe IoT Hub. Noteer de **hostnaam** en klik vervolgens op **Shared access policies**.
   
   ![][img-keys]
6. Klik op het beleid **iothubowner**. Kopieer en noteer de verbindingsreeks in de blade **iothubowner**. Kopieer de verbindingsreeks naar een locatie die u later kunt terugvinden. U hebt deze nodig voor de rest van deze zelfstudie.
   
   > [!NOTE]
   > Zorg ervoor dat u in productiescenario's de **iothubowner**-referenties niet gebruikt.
   > 
   > 
   
   ![][img-connection]

U hebt nu een IoT Hub met apparaatbeheerfunctie gemaakt. U hebt een verbindingsreeks nodig voor de rest van deze zelfstudie.

## Voorbeelden bouwen en apparaten inrichten in uw IoT Hub
In deze sectie voert u een script uit dat het gesimuleerde apparaat en de voorbeelden bouwt en een aantal nieuwe apparaat-id’s inricht in het apparatenregister van de IoT Hub. Een apparaat kan geen verbinding met IoT Hub maken, tenzij het vermeld staat in het apparatenregister.

Volg deze stappen om de voorbeelden te bouwen en apparaten in te richten in de IoT Hub:

1. Open een shell.
2. Kloon de github-opslagplaats. **Zorg ervoor dat u de opslagplaats kloont naar een map zonder spaties.**
   
     ```
     git clone --recursive --branch dmpreview https://github.com/Azure/azure-iot-sdks.git
     ```
3. Navigeer vanuit de hoofdmap waarin u de opslagplaats**azure-iot-sdks** hebt gekloond naar de map **azure-iot-sdks/c/build_all/linux** en voer de volgende opdracht uit om de vereiste pakketten en de afhankelijke bibliotheken te installeren:
   
     ```
     ./setup.sh
     ```
4. Navigeer vanuit de hoofdmap waarin u de opslagplaats **azure-iot-sdks** hebt gekloond naar de map **azure-iot-sdks/node/service/samples** en voer de volgende opdracht uit, waarbij u de aanduidingswaarde vervangt door de verbindingsreeks uit de vorige sectie:
   
     ```
     ./setup.sh <IoT Hub Connection String>
     ```

Dit script doet het volgende:

1. **cmake** wordt uitgevoerd om de bestanden te maken die nodig zijn voor het creëren van het gesimuleerde apparaat. Het uitvoerbare bestand bevindt zich in **azure-iot-sdks/node/service/samples/cmake/iotdm\_client/samples/iotdm\_simple\_sample**. Let op: de bronbestanden staan in de map **azure-iot-sdks/c/iotdm\_client/samples/iotdm\_simple\_sample**.
2. Het gesimuleerde uitvoerbare apparaatbestand **iotdm\_simple\_sample** wordt gemaakt.
3. `npm install` wordt uitgevoerd om de benodigde pakketten te installeren.
4. `node generate_devices.js` wordt uitgevoerd om apparaat-id’s in de IoT Hub in te richten. De apparaten worden beschreven in **sampledevices.json**. Nadat de apparaten zijn ingericht, worden de referenties opgeslagen in het bestand **devicecreds.txt** (dit bestand bevindt zich in de map **azure-iot-sdks/node/service/samples**).

## Uw gesimuleerde apparaten starten
Wanneer de apparaten zijn toegevoegd aan het apparatenregister, kunt u de gesimuleerde beheerde apparaten starten. U moet één gesimuleerd apparaat starten per apparaat-id die is ingericht in de Azure IoT Hub.

Navigeer met behulp van een shell naar de map **azure-iot-sdks/node/service/samples** en voer het volgende uit:

  ```
  ./simulate.sh
  ```

Dit script levert de opdrachten die u moet uitvoeren om  **iotdm\_simple\_sample** te starten voor elk apparaat dat wordt weergegeven in het bestand **devicecreds.txt**. Voer de opdrachten voor elk gesimuleerd apparaat afzonderlijk uit vanuit een apart terminalvenster. Het gesimuleerde apparaat is actief totdat u het opdrachtvenster sluit.

De toepassing **iotdm\_simple\_sample** is gemaakt met de apparaatbeheerclientbibliotheek voor C van de Azure IoT Hub. Hiermee kunnen IoT-apparaten worden gemaakt die beheerd kunnen worden door de Azure IoT Hub. Apparatenmakers kunnen met behulp van deze bibliotheek apparaateigenschappen melden en de uitgevoerde acties implementeren die vereist zijn door apparaattaken. Deze bibliotheek is een component dat wordt geleverd als onderdeel van de open source Azure IoT Hub SDK's.

Wanneer u **simulate.sh** uitvoert, ziet u een stream met gegevens in het uitvoervenster. Deze uitvoer toont het binnenkomende en uitgaande verkeer, en de **printf**-instructies in de callback-functies die specifiek zijn voor de toepassing. Met deze uitvoer kunt u binnenkomend en uitgaand verkeer bekijken en ziet u hoe de voorbeeldtoepassing de gedecodeerde pakketten verwerkt. Als het apparaat verbinding maakt met de IoT Hub, wordt de service automatisch gestart om resources op het apparaat te observeren. De DM-clientbibliotheek van IoT Hub roept vervolgens de callbacks van het apparaat op om de meest recente waarden van het apparaat op te halen.

Hierna volgt de uitvoer van de voorbeeldtoepassing **iotdm\_simple\_sample**. Bovenaan ziet u het bericht dat de **REGISTRATIE** is geslaagd en dat het apparaat met id **Device11-7ce4a850** verbinding maakt met IoT Hub.

> [!NOTE]
> Voor een minder uitgebreide uitvoer maakt u de retailconfiguratie en voert u deze uit.
> 
> 

![][img-output]

Zorg ervoor dat alle gesimuleerde apparaten actief blijven wanneer u de volgende secties voltooit.

## Voorbeeld-UI voor apparaatbeheer uitvoeren
Nu u een IoT hub hebt ingericht en er verschillende gesimuleerde apparaten worden uitgevoerd en zijn geregistreerd voor beheer, kunt u de voorbeeld-UI voor apparaatbeheer implementeren. De voorbeeld-UI voor apparaatbeheer biedt een werkvoorbeeld van hoe u de apparaatbeheer-API‘s kunt gebruiken om een interactieve UI te bouwen.  Zie [Azure loT-UI voor apparaatbeheer][lnk-dm-github] GitHub-opslagplaats voor meer informatie over de voorbeeld-UI voor apparaatbeheer, inclusief [bekende problemen](https://github.com/Azure/azure-iot-device-management#knownissues).

Als u de voorbeeld-UI voor apparaatbeheer wilt ophalen, bouwen en uitvoeren, volgt u deze stappen:

1. Open een shell.
2. Bevestig dat u Node.js 6.1.0 of hoger hebt geïnstalleerd volgens de sectie Vereisten door `node --version` te typen.
3. Kloon de GitHub-opslagplaats voor de Azure loT-IU voor apparaatbeheer door de volgende opdracht uit te voeren in de shell:
   
    ```
    git clone https://github.com/Azure/azure-iot-device-management.git
    ```
4. Voer in de hoofdmap van de gekloonde kopie van de opslagplaats voor de Azure loT-UI voor apparaatbeheer de volgende opdracht uit om de afhankelijke pakketten op te halen:
   
    ```
    npm install
    ```
5. Wanneer de installatieopdracht nmp is voltooid, voert u de volgende opdracht uit in de shell om de code te bouwen:
   
    ```
    npm run build
    ```
6. Gebruik een teksteditor om het bestand user-config.json te openen in de hoofdmap van de gekloonde map. Vervang de tekst &lt;YOUR CONNECTION STRING HERE&gt; met de loT Hub-verbindingsreeks uit de vorige sectie en sla het bestand op.
7. Voer in de shell de volgende opdracht uit om de UX-app voor apparaatbeheer te starten:
   
    ```
    npm run start
    ```
8. Als de opdrachtprompt aangeeft dat de services zijn gestart, opent u een webbrowser en navigeert u naar de app voor apparaatbeheer op de volgende URL om de gesimuleerde apparaten te bekijken: <http://127.0.0.1:3003>.
   
    ![][img-dm-ui]

Laat de gesimuleerde apparaten en de app voor apparaatbeheer actief terwijl u verdergaat naar de volgende zelfstudie voor apparaatbeheer.

## Volgende stappen
Zie [Getting started with the Gateway SDK][Ink-gateway-SDK] (Aan de slag gaan met de Gateway SDK) om verder te gaan met IoT Hub.

Zie de zelfstudie [Explore Azure IoT Hub device management using the sample UI][lnk-sample-ui] (Apparaatbeheer via Azure IoT Hub verkennen met behulp van de voorbeeld-UI) voor meer informatie over de functies voor apparaatbeheer via Azure IoT Hub.

<!-- images and links -->
[img-new-hub]: media/iot-hub-device-management-get-started-node/image1.png
[img-configure-hub]: media/iot-hub-device-management-get-started-node/image2.png
[img-monitor]: media/iot-hub-device-management-get-started-node/image3.png
[img-keys]: media/iot-hub-device-management-get-started-node/image4.png
[img-connection]: media/iot-hub-device-management-get-started-node/image5.png
[img-output]: media/iot-hub-device-management-get-started-node/image6.png
[img-dm-ui]: media/iot-hub-device-management-get-started-node/dmui.png

[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[Azure Portal]: https://portal.azure.com/
[Resourcegroepen gebruiken om Azure-resources te beheren]: ../azure-portal/resource-group-portal.md
[lnk-dm-github]: https://github.com/Azure/azure-iot-device-management
[lnk-sample-ui]: iot-hub-device-management-ui-sample.md
[Ink-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md


<!--HONumber=ago16_HO4-->


