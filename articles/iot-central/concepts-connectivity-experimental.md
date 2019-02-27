---
title: Connectiviteit van apparaten in Azure IoT Central | Microsoft Docs
description: In dit artikel bevat belangrijke concepten met betrekking tot connectiviteit van apparaten in Azure IoT Central
author: dominicbetts
ms.author: dobett
ms.date: 11/30/2017
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: timlt
ms.openlocfilehash: edffc6677609537d8a07aeae45a57c5e88449099
ms.sourcegitcommit: 50ea09d19e4ae95049e27209bd74c1393ed8327e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56882651"
---
# <a name="device-connectivity-in-azure-iot-central"></a>Connectiviteit van apparaten in Azure IoT Central

In dit artikel bevat belangrijke concepten die betrekking op de connectiviteit van apparaten in Microsoft Azure IoT Central hebben.

Maakt gebruik van Azure IoT Central [Azure IoT Hub Device Provisioning Service](https://docs.microsoft.com/azure/iot-dps/about-iot-dps) te registreren en verbinding maken met apparaten op schaal.

Met Device Provisioning Service:

- Klanten kunnen nu apparaatreferenties genereren en offline-apparaten configureren zonder eerst de om apparaten te registreren in Azure IoT Central.
- Azure IoT Central ondersteunt apparaatverbinding met X.509-certificaten, maar blijft ondersteunen en de connectiviteit met handtekeningen voor gedeelde toegang te verbeteren.
- Azure IoT Central-klanten kunnen nu hun eigen apparaat-id's gebruiken voor het registreren van apparaten in Azure IoT Central, waarmee de eenvoudige integratie met bestaande back-officesystemen uitvoeren.
- Er is een consistente manier apparaten kunt verbinden met Azure IoT Central.

>[!NOTE]
>Azure IoT Central maakt gebruik van IoT Hub Device Provisioning Service voor alle device Registration service en -verbinding. [Meer informatie](https://docs.microsoft.com/azure/iot-dps/about-iot-dps).

Op basis van uw situatie, volgt u de juiste instructies voor het verbinden van apparaten met Azure IoT Central:

- [Verbinding maken met een enkel apparaat snel (met behulp van handtekeningen voor gedeelde toegang)](#connect-a-single-device)
- [Verbinding maken met apparaten op schaal met behulp van handtekeningen voor gedeelde toegang](#connect-devices-at-scale-using-shared-access-signatures)
- [Verbinding maken met apparaten op schaal met behulp van X.509-certificaten](#connect-devices-using-x509-certificates) (aanbevolen voor productieworkloads)
- [Verbinding maken zonder eerst registreren apparaten](#connect-without-first-registering-devices) 

>[!NOTE]
>Het globaal eindpunt voor apparaten verbinding maken en inrichten is **global.azure-apparaten-provisioning.net**.

## <a name="connect-a-single-device"></a>Verbinding maken met een enkel apparaat

Verbinding met het maken van een enkel apparaat met Azure IoT Central met behulp van handtekeningen voor gedeelde toegang:

1. Voeg een **echt apparaat** van **Device Explorer**. Selecteer **+ nieuw** > **echte** om toe te voegen een echt apparaat.
    - Voer de **apparaat-ID** (moet kleine letters) of gebruikt u de voorgestelde apparaat-ID.
    - Voer de **apparaatnaam** of de voorgestelde naam gebruiken.

    ![Apparaat toevoegen](media/concepts-connectivity/add-device.png)

1. Ophalen van de gegevens van de verbinding, zoals de **bereik-ID**, **apparaat-ID**, en **primaire sleutel**, voor het apparaat door het selecteren van **Connect** op de Apparaatpagina.

    - **[Scope-ID](https://docs.microsoft.com/azure/iot-dps/concepts-device#id-scope)**  per Azure IoT Central-app en wordt gegenereerd door de Device Provisioning Service. Deze ID wordt gebruikt om te controleren of een unieke apparaat-ID in een app.
    - **Apparaat-ID** is de unieke apparaat-ID per app. Het apparaat moet de apparaat-ID als onderdeel van de registratie-aanroep verzenden.
    - **Primaire sleutel** een token shared access signature is gegenereerd door Azure IoT Central voor dit specifieke apparaat.
 
    ![Verbindingsdetails](media/concepts-connectivity/device-connect.png)

1. Details van de verbinding gebruiken **apparaat-id**, **apparaatnaam**, en van het apparaat **primaire sleutel** in uw apparaatcode inrichten en verbinding maken met uw apparaat en weer te geven de gegevensstroom via onmiddellijk. Als u een MXChip IoT DevKit (DevKit)-apparaat gebruikt, voert u de [de stapsgewijze instructies](howto-connect-devkit-experimental.md#add-a-real-device), vanaf de sectie 'Het apparaat DevKit voorbereiden'.

    Hier vindt u de referenties voor andere talen die u wilt gebruiken.

    - **De taal C:** Ga als volgt [deze client voor C voorbeeld apparaten](https://github.com/Azure/azure-iot-sdk-c/blob/master/provisioning_client/devdoc/using_provisioning_client.md) om een voorbeeld-apparaat te verbinden. Gebruik de volgende instellingen in het voorbeeld:

         ```c
         hsm_type = SECURE_DEVICE_TYPE_SYMMETRIC_KEY;

         ## Enter the Device Id and Symmetric keys 
         prov_dev_set_symmetric_key_info("<Device Id>", "<Enter Primary Symmetric key here>");
        ```

    - **Node.js:**  Ga als volgt [de stapsgewijze instructies](tutorial-add-device-experimental.md#prepare-the-client-code), vanaf de sectie 'De clientcode voorbereiden'.

## <a name="connect-devices-at-scale-by-using-shared-access-signatures"></a>Verbinding maken met apparaten op schaal met behulp van handtekeningen voor gedeelde toegang

Voor apparaten op schaal met Azure IoT Central verbinding met behulp van handtekeningen voor gedeelde toegang, zijn er twee algemene stappen:

1. **Het registreren van apparaten:** Registratie van apparaten door deze te importeren in Azure IoT Central via een CSV-bestand. Gebruik vervolgens de **exporteren** actie om te exporteren van uw apparaten en hun apparaat-Verbindingsdetails ophalen.
1. **Configuratie van het apparaat:** De apparaten met de verbindingsgegevens programma (**bereik-ID**, **apparaat-ID**, en **primaire sleutel**). Wanneer elk apparaat wordt ingeschakeld, wordt deze Device Provisioning Service als u de verbindingsgegevens of de toewijzing van Azure IoT Central-app aanroepen.

>[!NOTE]
>Een geavanceerde optie is ook beschikbaar waar u apparaten kunt verbinden zonder eerst de om apparaten te registreren in Azure IoT Central. [Meer informatie](https://docs.microsoft.com/azure/iot-dps/about-iot-dps).

### <a name="device-registration"></a>Apparaatregistratie

Voor een groot aantal apparaten verbinding met uw toepassing, ondersteunt Azure IoT Central bulkbewerkingen voor importeren van apparaten via een CSV-bestand. 

1. Importeren van apparaten te registreren in uw toepassing:

   1. Selecteer **Device Explorer** in het menu links.
   1. Selecteer in het linkerdeelvenster de apparaat-sjabloon voor dat u bulksgewijs wilt maken de apparaten. 
   1. Selecteer **importeren**, en selecteer vervolgens het CSV-bestand met de lijst van apparaat-id's moeten worden geïmporteerd. Het CSV-bestand moet hebben de volgende kolommen (en headers):

       - IOTC_DeviceID (moet kleine letters)
       - IOTC_DeviceName (optioneel)

   1. Nadat het importeren is voltooid, wordt een bericht wordt weergegeven op het raster apparaat.

1. Apparaten voor hun Verbindingsdetails exporteren:

   De **exporteren** actie wordt een CSV-bestand gemaakt met de apparaat-ID, naam van het apparaat en de apparaatsleutels. Aan de hand van deze gegevens naar het apparaat verbinding te maken met Azure IoT Central. Bulk-apparaten van uw toepassing exporteren:

   1. Selecteer **Device Explorer** in het menu links.
   1. Selecteer de apparaten die u wilt exporteren, en selecteer vervolgens de **exporteren** actie.
   1. Nadat de export is voltooid, wordt een bericht weergegeven samen met een koppeling voor het downloaden van het gegenereerde bestand.
   1. Klik op het bericht weergegeven voor het downloaden van het bestand naar een lokale map op de schijf.
   1. Het geëxporteerde CSV-bestand heeft de volgende kolommen met gegevens van de verbinding **apparaat-ID**, **apparaatnaam**, **apparaat primaire of secundaire sleutels**, en  **Primaire of secundaire certificaatvingerafdrukken**:

       - IOTC_DEVICEID
       - IOTC_DEVICENAME
       - IOTC_SASKEY_PRIMARY
       - IOTC_SASKEY_SECONDARY
       - IOTC_X509THUMBPRINT_PRIMARY
       - IOTC_X509THUMBPRINT_SECONDARY

### <a name="device-setup"></a>Apparaatinstallatie

 Als u wilt inrichten en uw apparaat te verbinden, gebruiken de verbindingsdetails **apparaat-id (IOTC_DEVICEID)**, **apparaat primaire sleutel (IOTC_SASKEY_PRIMARY)**, en **bereik-ID** in de apparaatcode van uw. Als u dit nog niet hebt gedaan, krijgen de **bereik-ID** van uw app door te selecteren in Azure IoT Central **beheer** > **apparaatverbinding**  >  **Scope-ID**.

Als u een DevKit apparaat verbinding maakt, voert u de [de stapsgewijze instructies](howto-connect-devkit-experimental.md#add-a-real-device), vanaf de sectie 'Het apparaat DevKit voorbereiden'.

Hier vindt u de referenties voor andere talen die u wilt gebruiken.

- **De taal C:** Ga als volgt [deze client voor C voorbeeld apparaten](https://github.com/Azure/azure-iot-sdk-c/blob/master/provisioning_client/devdoc/using_provisioning_client.md) om een voorbeeld-apparaat te verbinden. Gebruik de volgende instellingen in het voorbeeld:

     ```c
     hsm_type = SECURE_DEVICE_TYPE_SYMMETRIC_KEY;

     ## Enter the Device Id and Symmetric keys 
     prov_dev_set_symmetric_key_info("<Device Id>", "<Enter Primary Symmetric key here>");
    ```

- **Node.js:** Ga als volgt [de stapsgewijze instructies](tutorial-add-device-experimental.md#prepare-the-client-code), vanaf de sectie 'De clientcode voorbereiden'.

## <a name="connect-devices-by-using-x509-certificates"></a>Verbind apparaten met behulp van X.509-certificaten

Met behulp van X.509-certificaten als een attestation-mechanisme is een uitstekende manier om te schalen productie en vereenvoudigen apparaatinrichting. X.509-certificaten zijn doorgaans gerangschikt in een certificaatketen van de vertrouwensrelatie, waarbij elk certificaat in de keten is ondertekend door de persoonlijke sleutel van de volgende hogere certificaat, enzovoort, hebben die eindigt op een zelfondertekend basiscertificaat. Deze structuur het vastleggen van een gedelegeerde vertrouwensketen van het basiscertificaat dat wordt gegenereerd door een vertrouwde basiscertificaat (certificeringsinstantie) omlaag via elke tussenliggende CA aan het einde-entiteit 'leaf'-certificaat dat geïnstalleerd op een apparaat. Zie voor meer informatie, [apparaatverificatie met behulp van x.509-CA-certificaten](https://docs.microsoft.com/azure/iot-hub/iot-hub-x509ca-overview). 

Voor apparaten verbinding met Azure IoT Central met behulp van X.509-certificaten, zijn er drie belangrijke stappen die betrokken zijn:
 
1. **Configureren van de verbindingsinstellingen** in de Azure IoT Central-app door het toevoegen of controleren van het x.509-toegangspunt of de tussenliggende certificaat dat wordt gebruikt voor het genereren van de certificaten voor apparaten. Er zijn drie stappen voor het configureren van de verbindingsinstellingen voor X.509-certificaten:  

    - **Toevoegen van de x.509-basis- of tussencertificaat** dat u gebruikt voor het genereren van de certificaten voor de leaf-apparaten. Ga naar **beheer** > **apparaatverbinding** > **certificaten**. 
    
       ![Verbindingsinstellingen](media/concepts-connectivity/connection-settings.png)

    - **Controleer of het certificaat.** Eigendom van het certificaat te verifiëren, zorgt u ervoor dat de uploader van het certificaat een persoonlijke sleutel van het certificaat heeft. Om te controleren of het certificaat:

        - De verificatiecode genereren. Selecteer de knop naast de **verificatiecode** veld om deze code te genereren. 
        - Maak een X.509-certificaat voor verificatie met de verificatiecode. Sla het certificaat als een cer-bestand. 
        - Upload het ondertekende verificatiecertificaat en selecteer **controleren**.

        ![Verbindingsinstellingen](media/concepts-connectivity/verify-cert.png)

    - **Een secundair certificaat toevoegen.** Tijdens de levenscyclus van uw IoT-oplossing moet u certificaten implementeren. Twee van de belangrijkste redenen voor het verlengen van de certificaten zijn schendingen van de beveiliging en certificaten verlopen. Secundaire certificaten voor minder uitvaltijd voor apparaten die u om in te richten probeert terwijl u het primaire certificaat voor bijwerkt.

      **Voor het testen alleen gebruikt:**
    
      Hier volgen enkele hulpprogramma opdrachtregelprogramma's die u gebruiken kunt voor het genereren van CA-certificaten en certificaten voor apparaten.

      - Als u een apparaat DevKit gebruikt, is dit een [opdrachtregelprogramma](https://aka.ms/iotcentral-docs-dicetool) voor het genereren van CA-certificaten. Toe te voegen aan uw app in Azure IoT Central en controleer of de certificaten. 

      - Gebruik [deze opdrachtregel-hulpprogramma](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md) aan:

          - Maak de certificaatketen (Volg stap 2 in de GitHub-documenten). De certificaten als cer-bestanden opslaan en ze uploaden naar Azure IoT Central (als primaire certificaten).
          - De verificatiecode ontvangen vanuit de Azure IoT Central-app, genereren van het certificaat (Voer stap 3 in de GitHub-documenten) en upload het certificaat om deze te verifiëren. 
          - Leaf-certificaten maken met uw apparaat-ID als een parameter voor het hulpprogramma (Volg stap 4). Sla het certificaat en op uw apparaat te gebruiken.

1. **Apparaten registreren** door deze te importeren in Azure IoT Central via een CSV-bestand.

1. **Configuratie van het apparaat**: De leaf-certificaten met behulp van de geüploade basiscertificaat genereren. Zorg ervoor dat u de apparaat-ID gebruiken als de CNAME in de leaf-certificaten en zorgt ervoor dat deze in kleine letters. Hier volgt een [opdrachtregelprogramma](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md) voor het genereren van certificaten voor leaf/apparaat **testen uitsluitend**.

1. **Het apparaat met de service-informatie voor het inrichten programma**, waardoor het ophalen van de gegevens van de verbinding en de toewijzing van de Azure IoT Central-app wanneer deze ingeschakeld.

    Raadpleeg voor meer informatie de volgende artikelen:

    - [Voorbeeldimplementatie voor Raspberry Pi](https://aka.ms/iotcentral-docs-Raspi-releases)  

    - [Voorbeeld van apparaatclient in C](https://github.com/Azure/azure-iot-sdk-c/blob/master/provisioning_client/devdoc/using_provisioning_client.md)

## <a name="connect-without-first-registering-devices"></a>Verbinding maken zonder eerst registreren apparaten

Een van de belangrijkste scenario's die biedt ondersteuning voor Azure IoT Central is bedoeld voor OEM's voor massaopslag-vervaardiging apparaten, het genereren van referenties en configureert u de apparaten in de fabriek zonder eerst de om apparaten te registreren in Azure IoT Central. Nadat de apparaten zijn ingeschakeld en ze proberen verbinding maken met Azure IoT Central, keurt de operator de apparaten verbinding maken met de Azure IoT Central-app.

Dit is de stroom om apparaten te verbinden met deze functie:

![Verbindingsinstellingen](media/concepts-connectivity/device-connection-flow.png)

Volg de stappen op basis van uw keuze van apparaat-verificatieschema (X.509-certificaten of handtekeningen voor gedeelde toegang):

1. **Configureren of u de verbindingsinstellingen:**

    - **X.509-certificaten:** [Toevoegen en controleer of de basis- of tussencertificaat](#connect-devices-using-x509-certificates) en deze gebruiken voor het genereren van de certificaten voor apparaten in de volgende stap.
    - **Handtekeningen voor gedeelde toegang:** Kopieer de primaire sleutel (deze sleutel is de groep gedeelde handtekening toegangssleutel voor deze Azure IoT Central-toepassing) en deze gebruiken voor het genereren van de toegangssleutel handtekening apparaat dat wordt gedeeld in de volgende stap.

       ![Verbindingsinstellingen voor handtekeningen voor gedeelde toegang](media/concepts-connectivity/connection-settings-sas.png)

1. **Referenties van het apparaat genereren:**

    - **X.509-certificaten:** Genereren de leaf-certificaten voor uw apparaten met behulp van de basis- of tussencertificaat die u hebt toegevoegd aan deze app. Zorg ervoor dat u de apparaat-ID gebruiken als een CNAME in de leaf-certificaten en zorgt ervoor dat deze kleine letters. Hier volgt een [opdrachtregelprogramma](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md) voor het genereren van certificaten voor leaf/apparaten voor het testen.
    - **Handtekeningen voor gedeelde toegang:** Gebruik dit voor het genereren van toegangssleutels voor handtekening apparaat dat wordt gedeeld, [opdrachtregelprogramma](https://www.npmjs.com/package/dps-keygen). Gebruik de handtekeningsleutel van primaire gedeelde toegang (shared access signature groepssleutel) uit de vorige stap. Zorg ervoor dat het apparaat-ID in kleine letters is.

        Gebruik de volgende opdracht om op te halen van de verbindingsreeks van het apparaat: 

        ```
        npm i -g dps-keygen
        ```
    
        Gebruik de volgende opdracht om een apparaat shared access signature-sleutel te genereren:
                        
        ```
        dps-keygen <Primary_Key(GroupSAS)> <device_id>
        ```

1. **Instellen van de apparaten:** Elk apparaat voorzien van Flash de **bereik-ID**, **apparaat-ID**, en **certificaat/SAS-sleutel voor apparaat**, en klik vervolgens op het apparaat verbinding maakt met de Azure IoT Central-app in te schakelen.

1. **Apparaten verbinden met de Azure IoT Central:** Nadat de apparaten zijn overgeschakeld op, verbinden ze met Device Provisioning Service/Azure IoT Central voor registratie.

1. **Koppel de apparaten naar een sjabloon:** De verbonden apparaten worden weergegeven onder **niet verbonden apparaten** in **Device Explorer**. Hun apparaat Inrichtingsstatus **geregistreerde**. **Koppelen** de apparaten die aan de sjabloon van het betreffende apparaat en de apparaten verbinding maken met de Azure IoT Central-app goed te keuren. De apparaten de verbindingsdetails voor de Azure IoT Central-app krijgen, en vervolgens verbinding maken en beginnen met het verzenden van gegevens. Apparaatinrichting is nu voltooid, en de **Inrichtingsstatus** verandert voor de apparaten in **ingerichte**.

## <a name="device-provisioning-status"></a>Status van inrichting

Wanneer een echt apparaat is verbonden met Azure IoT Central, gebeuren de volgende stappen uit:

1. **Geregistreerd**: Het apparaat is eerst geregistreerd, wat betekent dat het apparaat in Azure IoT Central wordt gemaakt en heeft de apparaat-ID voor het apparaat. Een apparaat is geregistreerd bij:

    * Een nieuwe echt apparaat wordt toegevoegd aan **Device Explorer**
    * Een set met apparaten wordt toegevoegd met behulp van **importeren** in **Device Explorer**
    * Een apparaat dat is niet geregistreerd verbonden met geldige referenties en is zichtbaar onder **niet verbonden apparaten**

    In de bovenstaande gevallen, het **Inrichtingsstatus** is **geregistreerde**.

1. **Ingericht**: Wanneer het apparaat verbinding met geldige referenties maakt, Azure IoT Central is voltooid de inrichting stap (door het maken van het apparaat in IoT Hub). Azure IoT Central retourneert vervolgens de verbindingsreeks naar het apparaat zodat het kan verbinding maken en beginnen met het verzenden van gegevens. Van het apparaat **Inrichtingsstatus** verandert van **geregistreerde** naar **ingerichte**.

1. **Geblokkeerd**: De operator kan een apparaat blokkeren. Nadat een apparaat wordt geblokkeerd, kan deze gegevens naar Azure IoT Central verzenden en er opnieuw moet worden ingesteld. Apparaten die zijn geblokkeerd, hebben de **Inrichtingsstatus** van **geblokkeerd**. De operator kan ook blokkering van het apparaat. Nadat deze is gedeblokkeerd, van het apparaat **Inrichtingsstatus** geretourneerd naar de vorige status (**geregistreerde** of **ingerichte**). 

## <a name="get-the-device-connection-string"></a>De verbindingsreeks ophalen

U kunt de Iot Hub-verbindingsreeks voor Azure IoT Hub met behulp van de volgende stappen uit:

1. Details van de verbinding van het apparaat, ophalen, zoals de **bereik-ID**, **apparaat-ID**, en **primaire sleutel** uit de **apparaatverbinding** pagina. Als u deze informatie, gaat u naar de **apparaat** pagina en selecteer **Connect**. 

    ![Verbindingsdetails](media/concepts-connectivity/device-connect.png)

1. Haal de verbindingsreeks van het apparaat met behulp van de dps-keygen-opdrachtregelprogramma. Gebruik de volgende opdracht om op te halen van de verbindingsreeks van het apparaat:  

    ```cmd/sh
    npm i -g dps-keygen
    ```

    Voor het maken van een verbindingsreeks, zoek het binaire bestand onder de *bin /* map:

    ```cmd/sh
    dps_cstr <scope_id> <device_id> <Primary Key(for device)>
    ```

    [Meer informatie over de dps-keygen-hulpprogramma](https://www.npmjs.com/package/dps-keygen).

## <a name="sdk-support"></a>SDK-ondersteuning

De Azure IoT SDK's bieden de eenvoudigste manier voor het gebruik van de code op uw apparaten die verbinding met uw Azure IoT Central-toepassing maken. De volgende SDK's zijn beschikbaar:

- [Azure IoT-SDK voor C](https://github.com/azure/azure-iot-sdk-c)
- [Azure IoT-SDK voor Python](https://github.com/azure/azure-iot-sdk-python)
- [Azure IoT-SDK voor Node.js](https://github.com/azure/azure-iot-sdk-node)
- [Azure IoT-SDK voor Java](https://github.com/azure/azure-iot-sdk-java)
- [Azure IoT-SDK voor .NET](https://github.com/azure/azure-iot-sdk-csharp)

Elk apparaat verbinding maakt met behulp van een unieke verbindingsreeks waarmee het apparaat. Een apparaat verbinding kan maken alleen naar de IoT-hub wanneer deze geregistreerd. Wanneer u een echt apparaat in uw Azure IoT Central-toepassing maakt, genereert de toepassing een verbindingsreeks te gebruiken.

## <a name="sdk-features-and-iot-hub-connectivity"></a>SDK-functies en IoT Hub-connectiviteit

Alle communicatie van het apparaat met IoT Hub maakt gebruik van de volgende opties voor netwerkconnectiviteit van IoT-Hub:

- [Apparaat-naar-cloud-berichten](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-messages-d2c)
- [Apparaatdubbels](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-device-twins)

De volgende tabel geeft een overzicht van hoe Azure IoT Central apparaatfuncties toegewezen aan IoT Hub-functies:

| Azure IoT Central | Azure IoT Hub |
| ----------- | ------- |
| Meting: Telemetrie | Apparaat-naar-cloud-berichten |
| Apparaateigenschappen | Apparaatdubbel-gerapporteerde eigenschappen |
| Instellingen | Apparaatdubbel gewenst en gerapporteerde eigenschappen |

Zie voor meer informatie over het gebruik van de Azure IoT SDK's, de volgende artikelen voor voorbeeldcode:

- [Een generieke Node.js-client verbinden met uw Azure IoT Central-toepassing](howto-connect-nodejs-experimental.md)
- [Een Raspberry Pi-apparaat verbinden met uw Azure IoT Central-toepassing](howto-connect-raspberry-pi-python.md)
- [Een apparaat MXChip IoT DevKit verbinden met uw Azure IoT Central-toepassing](howto-connect-devkit-experimental.md)


## <a name="protocols"></a>Protocollen

De Azure IoT SDK's ondersteunen de volgende netwerkprotocollen om verbinding te maken naar een IoT-hub:

- MQTT
- AMQP
- HTTPS

Zie voor meer informatie over deze protocollen en richtlijnen over het kiezen van een [een communicatieprotocol kiezen](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-protocols).

Als uw apparaat geen van de ondersteunde protocollen gebruiken kan, kunt u Azure IoT Edge kunt gebruiken om u te conversie-protocol. IoT Edge biedt ondersteuning voor andere intelligence op de edge-scenario's voor verwerking naar de rand van de toepassing voor Azure IoT Central-offload.

## <a name="security"></a>Beveiliging

Alle gegevens die worden uitgewisseld tussen apparaten en uw app in Azure IoT Central is versleuteld. IoT Hub wordt geverifieerd voor elke aanvraag van een apparaat dat verbinding met een van de IoT Hub apparaat gerichte eindpunten maakt. Om te voorkomen dat het uitwisselen van referenties via de kabel, een apparaat ondertekende tokens gebruikt om te verifiëren. Zie voor meer informatie [Toegang tot IoT Hub regelen](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-security).

## <a name="next-steps"></a>Volgende stappen

- [Voorbereiden en verbinding maken met een MXChip IoT DevKit-apparaat](howto-connect-devkit-experimental.md)
- [Voorbereiden en verbinding maken met een Raspberry Pi-apparaat](howto-connect-raspberry-pi-python.md)
- [Een generieke Node.js-client verbinden met uw Azure IoT Central-toepassing](howto-connect-nodejs-experimental.md)
