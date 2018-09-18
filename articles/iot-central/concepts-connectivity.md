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
ms.openlocfilehash: d2ed07be829e48cc4fc0538c08fd498dea99e71e
ms.sourcegitcommit: 776b450b73db66469cb63130c6cf9696f9152b6a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/18/2018
ms.locfileid: "45985172"
---
# <a name="device-connectivity-in-azure-iot-central"></a>Connectiviteit van apparaten in Azure IoT Central

In dit artikel bevat belangrijke concepten met betrekking tot connectiviteit van apparaten in Microsoft Azure IoT Central.

Maakt gebruik van Azure IoT Central [Azure IoT Hub Device Provisioning service (DPS)](https://docs.microsoft.com/azure/iot-dps/about-iot-dps), IoT Central ter ondersteuning van onboarding inschakelen en verbinding te maken van apparaten op schaal.

-   Klanten kunnen nu apparaatreferenties genereren en configureert u de apparaten offline zonder eerst de registratie van apparaten in IoT Central
-   IoT Central ondersteunt apparaatverbinding met de aanbevolen X509 industrie-certificaat op basis van connectiviteit, maar blijft ondersteunen en de Shared Access Signatures (SAS)-connectiviteit te verbeteren
-   IoT Central klanten kunnen nu meebrengen hun eigen apparaat-id's voor het registreren van apparaten in IoT Central, doordat eenvoudige integratie met bestaande back-officesystemen uitvoeren
-   Er is een consistente manier apparaten verbinden met IoT Central 

>[!NOTE]
>IoT Central gebruikmaakt van Azure IoT Device Provisioning-service (DPS) onder voor alle device Registration service en -verbinding, [meer informatie over DPS](https://docs.microsoft.com/azure/iot-dps/about-iot-dps).

Op basis van uw gebruik case-Volg de instructies voor het verbinden van apparaten bij IoT Central
1. [Snel verbinding maken met een enkel apparaat (met behulp van handtekeningen voor gedeelde toegang)](#connect-a-single-device)
1. [Verbinding maken met apparaten op schaal met behulp van de Shared Access Signatures (SAS)](#connect-devices-at-scale-using-shared-access-signatures)
1. [Verbinding maken met apparaten op schaal met behulp van X509 certificaten](#connect-devices-using-x509-certificates) **aanbevolen voor productieworkloads**
1. [Verbinding maken zonder eerst registreren apparaten](#connect-without-first-registering-devices) 


>[!NOTE]
>Dit is de globaal eindpunt voor de apparaten verbinding maken en inrichten **global.azure-apparaten-provisioning.net**.

## <a name="connect-a-single-device"></a>Verbinding maken met een enkel apparaat
Een enkel apparaat verbinding maakt met IoT Central met behulp van SAS is eenvoudig en duurt slechts enkele stappen 
1. Voeg een **echt apparaat** van Device Explorer, klikt u op **+ Nieuw > echte** om toe te voegen een echt apparaat.
    * Voer de apparaat-Id **<span style="color:Red">(moet zich bevinden in kleine letters)</span>** of gebruikt u de voorgestelde apparaat-id.
    * Voer de naam van het apparaat of de voorgestelde naam gebruiken   
    ![Apparaat toevoegen](media\concepts-connectivity\add-device.png)
1. Zoals Verbindingsdetails ophalen **bereik-ID, apparaat-ID en primaire sleutel** voor het apparaat door te klikken op **Connect** op de Apparaatpagina.
    * **[Scope-ID](https://docs.microsoft.com/azure/iot-dps/concepts-device#id-scope)**  per IoT Central-App en wordt gegenereerd door DP's, die wordt gebruikt om te controleren of de unieke apparaat-ID in een App.
    * **Apparaat-Id** is de unieke apparaat-ID per App, het apparaat moet voor het verzenden van apparaat-Id als onderdeel van de registratie-aanroep.   
    * **Primaire sleutel** een SAS-token is gegenereerd door IoT Central voor dit specifieke apparaat. 
    ![Verbindingsdetails](media\concepts-connectivity\device-connect.PNG)
1. Details voor deze verbinding gebruiken **apparaat-id, de apparaatnaam en de primaire sleutel voor het apparaat** in uw apparaatcode inrichten en verbinding maken met uw apparaat en weer te geven van de gegevens direct worden doorgevoerd. Als u van het volgen van het apparaat MxChip gebruikmaakt [de stapsgewijze instructies hier](howto-connect-devkit.md#add-a-real-device), start u vanuit de sectie **bereid het apparaat DevKit**.   

    Hieronder vindt u de referenties voor andere talen die u wilt gebruiken.

    *   **Taal C:** als u met behulp van C, voert u de [deze client voor C voorbeeld apparaten](https://github.com/Azure/azure-iot-sdk-c/blob/dps_symm_key/provisioning_client/devdoc/using_provisioning_client.md) om een voorbeeld-apparaat te verbinden. Gebruik de volgende instellingen in het voorbeeld.   

         ```
         hsm_type = SECURE_DEVICE_TYPE_SYMMETRIC_KEY;
         
         static const char* const SYMMETRIC_KEY_VALUE = "Enter Primary Symmetric key here";

         static const char* const REGISTRATION_NAME = "Enter Device Id here";
        ```

    *   **Node.js:** als u wilt gebruiken van Node.js [hier gebruikt u de stapsgewijze instructies](tutorial-add-device.md#prepare-the-client-code), start u vanuit de sectie **voorbereiden van de clientcode**.



## <a name="connect-devices-at-scale-using-shared-access-signatures"></a>Verbinding maken met apparaten op schaal met behulp van handtekeningen voor gedeelde toegang

Als u wilt verbinding maken met apparaten op schaal met IoT Central met behulp van SAS, zijn er twee stappen die betrokken zijn 
1. **Apparaten registreren** door deze te importeren in IoT Central via een CSV-bestand en exporteren van apparaten met apparaat verbindingsgegevens te gebruiken om uw apparaten te verbinden
1. **Configuratie van het apparaat** het apparaat is geprogrammeerd met details van de verbinding ( **bereik-ID, apparaat-ID en primaire sleutel**), waardoor het aanroepen van de provisioning-service om op te halen van de verbinding info/IoT Central-app-toewijzing wanneer deze is ingeschakeld.

>[!NOTE]
>Een geavanceerde optie is ook beschikbaar wanneer u apparaten verbinding maken kunt zonder eerst om apparaten te registreren in IoT Central, [hier voor meer informatie](https://docs.microsoft.com/azure/iot-dps/about-iot-dps).

**Apparaten registreren**

Als u wilt verbinden met uw toepassing, Azure IoT Central groot aantal apparaten Bulksgewijs aanbiedingen importeren apparaten via een CSV-bestand. 

Vereisten voor CSV-bestand: de CSV-bestand moet de volgende kolommen (en headers)
1.  IOTC_DeviceID  **<span style="color:Red">(moet zich bevinden in kleine letters)</span>**
1.  IOTC_DeviceName (optioneel)



Importeren van apparaten in uw toepassing registreren
1.  Kies **Explorer** in het navigatiemenu links.
1.  Kies de apparaat-sjabloon die u bulksgewijs wilt maken de apparaten in het linkerdeelvenster. 
1.  Klik op **importeren**, selecteert u het CSV-bestand met de lijst van apparaat-id's moeten worden geïmporteerd.
Het CSV-bestand moet de volgende kolommen (en headers)
    *   IOTC_DeviceID  **<span style="color:Red">(moet zich bevinden in kleine letters)</span>**
    *   IOTC_DeviceName (optioneel)
1.  Nadat het importeren is voltooid, wordt een bericht wordt weergegeven op het raster apparaat.

Apparaten om op te halen van de gegevens van de verbinding, exporteren Export wordt een CSV-bestand gemaakt met de apparaat-Id, de apparaatnaam en de sleutel voor het apparaat. Aan de hand van deze gegevens naar het apparaat verbinden met IoT Central.
Bulksgewijs export-apparaten van uw toepassing:
1.  Kies **Explorer** in het navigatiemenu links.
1.  Selecteer de apparaten die u wilt exporteren en klik vervolgens op de **exporteren** actie.
1.  Nadat de export is voltooid, wordt een bericht weergegeven samen met een koppeling voor het downloaden van het gegenereerde bestand.
1.  Klik op het bericht weergegeven voor het downloaden van het bestand naar een lokale map op de schijf.
1.  Het geëxporteerde CSV-bestand heeft de volgende informatie in de kolommen: **thumbrpints voor apparaat-Id, de naam van apparaat, apparaat Priamary/ecundaire sleutels en primair/secundair certificaat**
    *   IOTC_DEVICEID
    *   IOTC_DEVICENAME
    *   IOTC_SASKEY_PRIMARY
    *   IOTC_SASKEY_SECONDARY
    *   IOTC_X509THUMBPRINT_PRIMARY 
    *   IOTC_X509THUMBPRINT_SECONDARY


**De installatie van apparaat**

De details voor deze verbinding gebruiken **apparaat-id (IOTC_DEVICEID), apparaat primaire sleutel (IOTC_SASKEY_PRIMARY) en Scope-ID** in uw apparaatcode inrichten en uw apparaat te verbinden. Als u nog niet gedaan hebt, krijgen de **bereik-Id** van uw IoT Central-App **beheer > apparaatverbinding > bereik-ID**.
Als u de **MxChip** apparaat verbinding maakt volgen [de stapsgewijze instructies hier](howto-connect-devkit.md#add-a-real-device), starten vanaf de sectie **bereid het apparaat DevKit**.   

Hieronder vindt u de referenties voor andere talen die u wilt gebruiken.

   *   **Taal C:** als u van C volgen gebruikmaakt [deze client voor C voorbeeld apparaten](https://github.com/Azure/azure-iot-sdk-c/blob/dps_symm_key/provisioning_client/devdoc/using_provisioning_client.md) om een voorbeeld-apparaat te verbinden. Gebruik de volgende instellingen in het voorbeeld.   
         ```
         hsm_type = SECURE_DEVICE_TYPE_SYMMETRIC_KEY;

         static const char* const SYMMETRIC_KEY_VALUE = "Enter Primary Symmetric key here";
         static const char* const REGISTRATION_NAME = "Enter Device Id here";
        ```
    * **Node.js:** als u wilt gebruiken van Node.js [hier gebruikt u de stapsgewijze instructies](tutorial-add-device.md#prepare-the-client-code), start u vanuit de sectie **voorbereiden van de clientcode**.


## <a name="connect-devices-using-x509-certificates"></a>Verbind apparaten met behulp van X509 certificaten

Als een attestation-mechanisme is een uitstekende manier om te schalen met behulp van X.509-certificaten **productie** en apparaatinrichting te vereenvoudigen. X.509-certificaten zijn doorgaans gerangschikt in een certificaatvertrouwensketen waarin elk certificaat in de keten is ondertekend door de persoonlijke sleutel van de volgende hogere certificaat, enzovoort, wordt beëindigd in een zelfondertekend basiscertificaat. Deze manier maakt u een gedelegeerde vertrouwensketen van het basiscertificaat dat is gegenereerd door een vertrouwde basiscertificeringsinstantie (CA) omlaag via elke tussenliggende CA aan het einde-entiteit "leaf" certificaat geïnstalleerd op een apparaat. Zie voor meer informatie, [Apparaatverificatie met behulp van x.509-CA-certificaten](https://docs.microsoft.com/azure/iot-hub/iot-hub-x509ca-overview). 

Apparaten kunt verbinden met IoT Central X509 met certificaten, er zijn drie belangrijke stappen die betrokken zijn 
1. **Configureren van de verbindingsinstellingen** in IoT Central-app door de X509 root-/ tussencertificaat gebruikt voor het genereren van de certificaten voor apparaten toe te voegen/te verifiëren.  Er zijn twee stappen voor het configureren van de verbindingsinstellingen voor X509 certificaten.  

    *   **Toevoegen van X509 basis- of tussencertificaat** u gebruikt voor het genereren van de certificaten voor de leaf-apparaten. Ga naar Beheer > apparaatverbinding > certificaten. 
    
        ![Verbindingsinstellingen](media\concepts-connectivity\connection-settings.PNG)
    *   **Verificatie van het certificaat:** certificaat eigendom verifiëren zorgt ervoor dat de uploader van het certificaat in het bezit is van de persoonlijke sleutel van het certificaat. Om te controleren of het certificaat
        *  Verificatiecode genereren, klikt u op de knop naast het veld voor de verificatie voor het genereren van de verificatiecode. 
        *  Maak een X.509-certificaat voor verificatie met de verificatiecode, sla het certificaat als een cer-bestand. 
        *  Upload het ondertekende verificatiecertificaat en klikt u op verifiëren.

        ![Verbindingsinstellingen](media\concepts-connectivity\verify-cert.png)
    *   **Secundair certificaat:** tijdens de levenscyclus van uw IoT-oplossing, moet u certificaten implementeren. Twee van de belangrijkste redenen voor het verlengen van certificaten is een schending van de beveiliging en certificaten verlopen. Secundaire certificaten worden gebruikt om uitvaltijd voor de poging om in te richten als u bij het bijwerken van het primaire certificaat voor apparaten te verlagen.

    **TESTEN ALLEEN VOOR TESTDOELEINDEN** 
    
    Hieronder vindt u enkele hulpprogramma opdrachtregel-hulpprogramma's die kunt u voor het genereren van CA-certificaten en certificaten voor het apparaat.

    * Als u MxChip hier is een [-opdrachtregelprogramma](http://aka.ms/iotcentral-docs-dicetool) voor het genereren van CA-certificaten toe te voegen aan uw IoT Central-app en controleer of de certificaten. 

    *   Gebruik deze [-opdrachtregelprogramma](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md ) naar
        * Maak de certificaatketen (Volg stap 2 in de GitHub-documenten). 
         De certificaten als cer-bestanden opslaan en uploadt naar IoT Central (primair).   
        * Ophalen van de verificatiecode uit de IoT Central-App, het certificaat (Volg stap 3 in de GitHub-documenten) te genereren en uploaden om te controleren. 
        * Leaf-certificaten maken met uw apparaat-Id als een parameter voor het hulpprogramma (Volg stap 4). Sla het certificaat en op uw apparaat te gebruiken.     

1. **Apparaten registreren** door deze via een CSV-bestand importeert in IoT Central.

1. **Configuratie van het apparaat** : genereren van het leaf-certificaten met behulp van de geüploade basiscertificaat. Zorg ervoor dat u de **apparaat-ID** als de CNAME in de leaf-certificaten en bevindt zich in **kleine letters**. Hier volgt een [-opdrachtregelprogramma](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md ) voor het genereren van certificaten voor leaf/apparaat **alleen testen**.

    Het apparaat met provisioning service informatie voor het inschakelen van het ophalen van de verbindingsgegevens en IoT Central, app-toewijzing wanneer overgeschakeld op programmeren.    

    **Verdere referene** 
    *   Voorbeeldimplementatie voor [RaspberryPi.](http://aka.ms/iotcentral-docs-Raspi-releases)  

    *   [Voorbeeld van apparaatclient in C.](https://github.com/Azure/azure-iot-sdk-c/blob/dps_symm_key/provisioning_client/devdoc/using_provisioning_client.md)

>[!NOTE]
>Gebruik de **apparaat-ID** als een cname bij het genereren van het leaf-certificaten voor apparaten.

>[!NOTE]
>De **apparaat-ID** moeten kleine letters 
 
## <a name="connect-without-first-registering-devices"></a>Verbinding maken zonder eerst registreren apparaten
Een van de belangrijkste scenario's met die IOT Central kunnen is voor OEM's voor massaopslag vervaardiging apparaten genereren van referenties en deze in de fabriek kunt configureren zonder ze eerst registreren in IoT Central. Zodra de apparaten zijn ingeschakeld en verbinding met IoT Central maken keurt de operator voor het apparaat verbinding maakt met de IoT Central-app.

Hieronder ziet u de stroom om apparaten te verbinden met deze functie

![Verbindingsinstellingen](media\concepts-connectivity\device-connection-flow.PNG)


Volg de stappen op basis van uw keuze van apparaat-verificatieschema (X509/SAS)

1. **Verbindingsinstellingen** 
    * **X509 certificaten:** [toevoegen en controleer of de basis-/ tussencertificaat](#connect-devices-using-x509-certificates) en deze gebruiken voor het genereren van de certificaten voor apparaten in de volgende stap.
    * **SAS:** Kopieer de primaire sleutel (deze sleutel is de groep SAS-sleutel voor deze IoT Central-toepassing) en deze gebruiken voor het genereren van de SAS-sleutels van het apparaat in de volgende stap. 
![Verbindingsinstellingen SAS](media\concepts-connectivity\connection-settings-sas.png)

1. **Apparaatreferenties genereren** 
    *   **Certificaten X509:** genereren van het leaf-certificaten voor uw apparaten met behulp van de root/tussencertificaat die u hebt toegevoegd aan deze app. Zorg ervoor dat u de **apparaat-ID** als een cname in de leaf-certificaten en  **<span style="color:Red">(moet zich bevinden in kleine letters)</span>**. Hier volgt een [-opdrachtregelprogramma](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md ) voor het genereren van certificaten voor het testen van leaf/apparaat.
    *   **SAS** apparaat SAS-sleutels kunnen worden gegenereerd met behulp van dit [opdrachtregelprogramma](https://www.npmjs.com/package/dps-keygen). Gebruik de primaire SAS-sleutel (SAS-sleutel, groep) uit de vorige stap. Zorg ervoor dat de apparaat-ID  **<span style="color:Red">in kleine letters is</span>**.

        Gebruik de onderstaande instructies voor het genereren van SAS-sleutel voor apparaat           

        ```
        npm i -g dps-keygen
        ```
    
        **Gebruik**
                        
        ```
        dps-keygen <Primary_Key(GroupSAS)> <device_id>
        ```

1. **De installatie van apparaat** 
    
     Het apparaat met Flash **bereik-ID, apparaat-ID apparaat certificaat/SAS-sleutel** en schakelt u het apparaat verbinding maakt met IoT Central-app.

1. **Apparaat verbinden met IoT Central:** eenmaal is overgeschakeld op de apparaten verbinding maken met DPS/IoT Central voor registratie.

1. **Apparaat koppelen aan een sjabloon:** het verbonden apparaat wordt weergegeven onder **niet-gekoppelde apparaten** in **Device Explorer**. Het apparaat Inrichtingsstatus is **geregistreerde**. **Koppelen** het apparaat aan de sjabloon van het betreffende apparaat en het apparaat verbinding maakt met de IoT Central-app goed te keuren. Het apparaat haalt de details van de verbinding voor de app IoT Central, verbinding maakt en begint met het verzenden van gegevens. Apparaat provioning is nu voltooid en de *Inrichtingsstatus* verandert in **ingerichte**.

## <a name="device-provisioning-status"></a>De status van apparaat inrichten
Er zijn een reeks stappen die betrokken zijn bij een echt apparaat is verbonden met Azure IoT Central 
1. **Geregistreerde**: het apparaat is het eerste **geregistreerde**, wat betekent dat het apparaat in IoT Central wordt gemaakt en heeft de apparaat-ID voor het apparaat.
Apparaat is Registeretd wanneer  
    *   Een nieuwe echt apparaat wordt toegevoegd aan **Explorer**
    *   Een set van apparaat is toegevoegd met behulp van **importeren** op **Explorer**
    *   Een apparaat dat niet is geregistreerd, maar maakt verbinding met geldige referenties en is zichtbaar onder **niet gekoppeld** apparaten. 

    In alle gevallen de *Inrichtingsstatus* is **geregistreerd**

1. **Ingericht**: de volgende stap is wanneer het apparaat verbinding met geldige referenties maakt IoT Central is de inrichting stap voltooid (door het maken van het apparaat in IoT Hub). Deze retourneert vervolgens de verbindingsreeks naar het apparaat verbinding maakt en beginnen met het verzenden van gegevens. Het apparaat *Inrichtingsstatus* nu verandert van **geregistreerde** naar **ingerichte**.

1.  **Geblokkeerd**: de operator een apparaat, kunt blokkeren wanneer een apparaat is geblokkeerd. Er kan geen gegevens verzenden naar IoT Central en moet opnieuw worden ingesteld. Apparaten die zijn geblokkeerd, hebben de *Inrichtingsstatus* van **geblokkeerd**. De operator kan ook blokkering van het apparaat. Zodra het apparaat gedeblokkeerd *Inrichtingsstatus* Ga terug naar de vorige *Inrichtingsstatus* (geregistreerd of ingericht). 

## <a name="getting-device-connection-string"></a>Apparaat-verbindingsreeks ophalen
U kunt Iot hub apparaat-verbindingsreeks voor Azure IoT Hub ophalen met behulp van de volgende stappen 
1. Zoals Verbindingsdetails ophalen **bereik-ID, apparaat-ID apparaat primaire sleutel** van de Apparaatpagina (hebt u naar de Apparaatpagina > Klik op verbinding maken) 

    ![Verbindingsdetails](media\concepts-connectivity\device-connect.PNG)

1. De verbindingsreeks van het apparaat met behulp van het opdrachtregelprogramma commnd ophalen.
    Gebruik de onderstaande instructies voor het ophalen van de verbindingsreeks van apparaat  

    ```cmd/sh
    npm i -g dps-keygen
    ```
    **Gebruik**

    Zoeken om te kunnen maken van een verbindingsreeks, het binaire bestand onder bin / map
    ```cmd/sh
    dps_cstr <scope_id> <device_id> <Primary Key(for device)>
    ```
    Meer informatie over de [hier dps-keygen-hulpprogramma.](https://www.npmjs.com/package/dps-keygen)

## <a name="sdk-support"></a>SDK-ondersteuning

De apparaat-SDK's van Azure-aanbieding de eenvoudigste manier voor u de code implementeert op uw apparaten die verbinding maakt met uw Azure IoT Central-toepassing. Het volgende apparaat-SDK's zijn beschikbaar:

- [Azure IoT-SDK voor C](https://github.com/azure/azure-iot-sdk-c)
- [Azure IoT-SDK voor Python](https://github.com/azure/azure-iot-sdk-python)
- [Azure IoT-SDK voor Node.js](https://github.com/azure/azure-iot-sdk-node)
- [Azure IoT-SDK voor Java](https://github.com/azure/azure-iot-sdk-java)
- [Azure IoT-SDK voor .NET](https://github.com/azure/azure-iot-sdk-csharp)

Elk apparaat verbinding maakt met behulp van een unieke verbindingsreeks waarmee het apparaat. Een apparaat kan alleen verbinding maken met de IoT-hub wanneer deze is geregistreerd. Wanneer u een echt apparaat in uw Azure IoT Central-toepassing maakt, genereert de toepassing een verbindingsreeks te gebruiken.

## <a name="sdk-features-and-iot-hub-connectivity"></a>SDK-functies en IoT Hub-connectiviteit

Alle communicatie van het apparaat met IoT Hub maakt gebruik van de volgende opties voor netwerkconnectiviteit van IoT-Hub:

- [Apparaat-naar-cloud-berichten](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-messages-d2c)
- [Apparaatdubbels](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-device-twins)

De volgende tabel geeft een overzicht van hoe Azure IoT Central apparaatfuncties toegewezen bij IoT Hub-functies:

| Azure IoT Central | Azure IoT Hub |
| ----------- | ------- |
| Meting: telemetrie | Apparaat-naar-cloud-berichten |
| Apparaateigenschappen | Apparaatdubbel-gerapporteerde eigenschappen |
| Instellingen | Apparaatdubbel gewenst en gerapporteerde eigenschappen |

Zie voor meer informatie over het gebruik van de apparaat-SDK's, een van de volgende artikelen bijvoorbeeld code:

- [Een generieke Node.js-client verbinden met uw Azure IoT Central-toepassing](howto-connect-nodejs.md)
- [Een Raspberry Pi-apparaat verbinden met uw Azure IoT Central-toepassing](howto-connect-raspberry-pi-python.md)
- [Een apparaat van de kit DevDiv verbinden met uw Azure IoT Central application](howto-connect-devkit.md).


## <a name="protocols"></a>Protocollen

De apparaat-SDK's ondersteunen de volgende netwerkprotocollen om verbinding te maken naar een IoT-hub:

- MQTT
- AMQP
- HTTPS

Zie voor meer informatie over deze verschil protocollen en richtlijnen over het kiezen van een [een communicatieprotocol kiezen](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-protocols).

Als uw apparaat geen van de ondersteunde protocollen gebruiken kan, kunt u Azure IoT Edge kunt gebruiken om u te conversie-protocol. IoT Edge biedt ondersteuning voor andere intelligence op de edge-scenario's voor verwerking naar de rand van de toepassing voor Azure IoT Central-offload.

## <a name="security"></a>Beveiliging

Alle gegevens die worden uitgewisseld tussen apparaten en uw Azure IoT Central is versleuteld. IoT Hub wordt geverifieerd voor elke aanvraag van een apparaat dat verbinding met een van de IoT Hub apparaat gerichte eindpunten maakt. Om te voorkomen dat het uitwisselen van referenties via de kabel, een apparaat ondertekende tokens gebruikt om te verifiëren. Voor meer informatie ziet, [beheren van toegang tot IoT Hub](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-security).

> [!NOTE]
> Apparaten die verbinding met Azure IoT Central maken moeten op dit moment SAS-tokens gebruiken. X.509-certificaten worden niet ondersteund voor apparaten die verbinding met Azure IoT Central maken.

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd over verbindingsmogelijkheden voor apparaten in Azure IoT Central, vindt hier u de voorgestelde volgende stappen:

- [Voorbereiden en een apparaat DevKit verbinden](howto-connect-devkit.md)
- [Raspberry Pi voorbereiden en verbinden](howto-connect-raspberry-pi-python.md)
- [Een generieke Node.js-client verbinden met uw Azure IoT Central-toepassing](howto-connect-nodejs.md)
