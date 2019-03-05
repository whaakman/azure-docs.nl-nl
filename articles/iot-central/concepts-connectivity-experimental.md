---
title: Connectiviteit van apparaten in Azure IoT Central | Microsoft Docs
description: In dit artikel bevat belangrijke concepten met betrekking tot connectiviteit van apparaten in Azure IoT Central
author: dominicbetts
ms.author: dobett
ms.date: 02/28/2019
ms.topic: conceptual
ms.service: iot-central
services: iot-central
manager: timlt
ms.openlocfilehash: 47fde9099ed8172b83f9f12085c5aee219c4716c
ms.sourcegitcommit: 3f4ffc7477cff56a078c9640043836768f212a06
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/04/2019
ms.locfileid: "57318016"
---
# <a name="device-connectivity-in-azure-iot-central"></a>Connectiviteit van apparaten in Azure IoT Central

In dit artikel bevat belangrijke concepten met betrekking tot connectiviteit van apparaten in Microsoft Azure IoT Central.

Maakt gebruik van Azure IoT Central het [Azure IoT Hub Device Provisioning service (DPS)](https://docs.microsoft.com/azure/iot-dps/about-iot-dps) voor het beheren van alle device Registration service en -verbinding.

Met DPS kunnen:

- IoT Central om onboarding en verbindende apparaten op schaal te ondersteunen.
- U voor het genereren van apparaat-referenties en de apparaten offline configureren zonder de apparaten via IoT Central gebruikersinterface registreren.
- Apparaten verbinding maken met behulp van shared access signatures (SAS).
- Apparaten verbinding maken met behulp van industriestandaard x.509-certificaten.
- U kunt uw eigen apparaat-id's gebruiken voor het registreren van apparaten in IoT Central. Met behulp van uw eigen apparaat-id's vereenvoudigt de integratie met bestaande back-officesystemen uitvoeren.
- Een enkele, consistente manier apparaten verbinden met IoT Central.

In dit artikel beschrijft de volgende vier gebruiksvoorbeelden:

1. [Snel verbinding maken met een enkel apparaat met behulp van SAS](#connect-a-single-device)
1. [Verbinding maken met apparaten op schaal met behulp van SAS](#connect-devices-at-scale-using-shared-access-signatures)
1. [Verbinding maken met apparaten op schaal met behulp van X.509-certificaten](#connect-devices-using-x509-certificates) dit is de aanbevolen aanpak voor productieomgevingen.
1. [Verbinding maken zonder eerst registreren apparaten](#connect-without-first-registering-devices) 

## <a name="connect-a-single-device"></a>Verbinding maken met een enkel apparaat

Deze methode is nuttig wanneer u experimenteren met IoT Central of testen van apparaten.

Voor een enkel apparaat verbinding met IoT Central met behulp van SAS, de volgende stappen uit:

1. Als u wilt toevoegen echt apparaat, gaat u naar de **Device Explorer**, kies een apparaat-sjabloon en selecteer **+ Nieuw > echte**:
    - Voer uw eigen (kleine letters) **apparaat-ID** of gebruikt u de voorgestelde-ID.
    - Voer een **apparaatnaam** of de voorgestelde naam gebruiken.

      ![Apparaat toevoegen](media/concepts-connectivity-experimental/add-device.png)

1. Als u de verbindingsgegevens van apparaat, selecteert u **Connect** op de Apparaatpagina. U moet de **bereik-ID**, **apparaat-ID**, en **primaire sleutel** waarden:
    - Elke IoT Central-toepassing heeft een unieke [bereik-ID](../iot-dps/concepts-device.md#id-scope) die door de DPS wordt gegenereerd.
    - [Apparaat-ID](../iot-dps/concepts-device.md#device-id) is de unieke apparaat-ID. De apparaat-ID opgeslagen in de [id-register](../iot-hub/iot-hub-devguide-identity-registry.md).
    - **Primaire sleutel** een SAS-token is gegenereerd door IoT Central voor het apparaat.

      ![Verbindingsdetails](media/concepts-connectivity-experimental/device-connect.png)

Gebruik de verbindingsgegevens in de apparaatcode van uw zodat het apparaat verbinding maakt en gegevens verzenden naar IoT voor uw IoT Central-toepassing. Zie voor meer informatie over het verbinden van apparaten, [Vervolgstappen](#next-steps).

## <a name="connect-devices-at-scale-using-sas"></a>Verbinding maken met apparaten op schaal met behulp van SAS

Voor apparaten verbinding met IoT Central op schaal met behulp van SAS, moet u registreren en stel vervolgens de apparaten:

### <a name="register-devices-in-bulk"></a>Apparaten bulksgewijs registreren

Voor het registreren van een groot aantal apparaten met uw IoT Central-toepassing, gebruikt u een CSV-bestand [importeren van apparaat-id's en apparaatnamen](howto-manage-devices-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json#import-devices).

Om op te halen van de verbindingsgegevens voor de geïmporteerde apparaten [exporteren van een CSV-bestand van uw toepassing IoT Central](howto-manage-devices-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json#export-devices).

> [!NOTE]
> Zie voor meer informatie hoe u apparaten kunt verbinden zonder het eerst in IoT Central registreren, [verbinden zonder eerst registreren apparaten](#connect-without-first-registering-devices).

### <a name="set-up-your-devices"></a>Instellen van uw apparaten

Gebruik de verbindingsgegevens uit het bestand exporteren in de apparaatcode van uw wilt dat uw apparaten verbinding maken met en gegevens verzenden naar IoT voor uw IoT Central-toepassing. Zie voor meer informatie over het verbinden van apparaten, [Vervolgstappen](#next-steps).

## <a name="connect-devices-using-x509-certificates"></a>Verbind apparaten met behulp van X.509-certificaten

In een productieomgeving is met behulp van X.509-certificaten het aanbevolen apparaat verificatiemechanisme voor IoT Central. Zie voor meer informatie, [Apparaatverificatie met behulp van x.509-CA-certificaten](../iot-hub/iot-hub-x509ca-overview.md).

De volgende stappen wordt beschreven hoe u apparaten kunt verbinden met IoT Central met behulp van X.509-certificaten:

1. In uw toepassing IoT Central _toevoegen en controleer of de tussenliggende of x.509-basiscertificaat_ u voor het genereren van certificaten voor apparaten:

    - Navigeer naar **beheer > apparaatverbinding > certificaten (X.509)** en x.509-basis- of tussenliggende certificaat u voor het genereren van de certificaten voor de leaf-apparaten toe te voegen.

      ![Verbindingsinstellingen](media/concepts-connectivity-experimental/connection-settings.png)

      Als er een schending van de beveiliging of het primaire certificaat is ingesteld op verlopen, moet u het secundaire certificaat gebruiken om uitvaltijd te verlagen. U kunt blijven voor het inrichten van apparaten met behulp van het secundaire certificaat tijdens het bijwerken van het primaire certificaat.

    - Eigendom van het certificaat te verifiëren, zorgt u ervoor dat de uploader van het certificaat een persoonlijke sleutel van het certificaat heeft. Om te controleren of het certificaat:
        - Selecteer de knop naast **verificatiecode** om een code te genereren.
        - Maak een X.509-certificaat voor verificatie met de verificatiecode in die u hebt gegenereerd in de vorige stap. Sla het certificaat als een cer-bestand.
        - Upload het ondertekende verificatiecertificaat en selecteer **controleren**.

          ![Verbindingsinstellingen](media/concepts-connectivity-experimental/verify-cert.png)

1. Een CSV-bestand te gebruiken _importeren en het registreren van apparaten_ in uw IoT Central-toepassing.

1. _Instellen van uw apparaten._ De leaf-certificaten met behulp van de geüploade basiscertificaat genereren. Gebruik de **apparaat-ID** als de CNAME-waarde in de leaf-certificaten. De apparaat-ID moet alleen kleine letters. Het programma vervolgens uw apparaten met het service-informatie voor het inrichten. Wanneer een apparaat voor het eerst wordt ingeschakeld, worden de verbindingsgegevens voor uw IoT Central-toepassing van DPS opgehaald.

### <a name="further-reference"></a>Aanvullende referentie

- Voorbeeldimplementatie voor [RaspberryPi.](https://aka.ms/iotcentral-docs-Raspi-releases)

- [Voorbeeld van apparaatclient in C.](https://github.com/Azure/azure-iot-sdk-c/blob/dps_symm_key/provisioning_client/devdoc/using_provisioning_client.md)

### <a name="for-testing-purposes-only"></a>Voor het testen van uitsluitend bedoeld

Alleen voor testdoeleinden, kunt u deze hulpprogramma's voor het genereren van CA-certificaten en certificaten voor apparaten.

- Als u een apparaat DevKit dit [opdrachtregelprogramma](https://aka.ms/iotcentral-docs-dicetool) genereert een CA-certificaat dat u kunt toevoegen aan uw IoT Central-toepassing om te controleren of de certificaten.

- Gebruik deze [opdrachtregelprogramma](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md ) aan:
  - Maak een certificaatketen. Volg stap 2 in het GitHub-artikel.
  - Sla de certificaten als cer-bestanden te uploaden naar uw IoT Central-toepassing.
  - De verificatiecode uit de IoT Central-toepassing gebruiken voor het genereren van het verificatiecertificaat. Voer stap 3 in de GitHub-artikel.
  - Leaf-certificaten voor uw apparaten met behulp van uw apparaat-id's als een parameter voor het hulpprogramma maken. Volg stap 4 in de GitHub-artikel.

## <a name="connect-without-registering-devices"></a>Verbinding maken zonder het registreren van apparaten

Een belangrijke scenario IoT Central kunnen is bedoeld voor OEM's voor de massa vervaardiging van apparaten die verbinding met een IoT Central-toepassing zonder dat eerst maken kunnen wordt geregistreerd. Een fabrikant geschikt referenties genereren, en configureert u de apparaten in de fabriek. Wanneer een apparaat voor de eerste keer inschakelt, deze verbinding maakt automatisch aan een IoT Central-toepassing. Een operator IoT Central kunnen het apparaat moet goedkeuren voordat het kan worden stat verzenden van gegevens.

Het volgende diagram geeft een overzicht van deze stroom:

![Verbindingsinstellingen](media/concepts-connectivity-experimental/device-connection-flow.png)

De volgende stappen wordt dit proces in meer detail beschreven. De stappen verschillen enigszins, afhankelijk van of u van SAS- of x.509-certificaten voor verificatie van apparaten gebruikmaakt:

1. Configureer instellingen voor de verbinding:

    - **X.509-certificaten:** [Toevoegen en controleer of de basis-/ tussencertificaat](#connect-devices-using-x509-certificates) en deze gebruiken voor het genereren van de certificaten voor apparaten in de volgende stap.
    - **SAS:** Kopieer de primaire sleutel. Deze sleutel is de groep SAS-sleutel voor de IoT Central-toepassing. Gebruik de sleutel voor het genereren van de SAS-sleutels van het apparaat in de volgende stap.
    ![Verbindingsinstellingen SAS](media/concepts-connectivity-experimental/connection-settings-sas.png)

1. De referenties van uw apparaat te genereren
    - **X.509-certificaten:** Genereer de leaf-certificaten voor uw apparaten met behulp van de basis- of tussencertificaat die u hebt toegevoegd aan uw IoT Central-toepassing. Zorg ervoor dat u de kleine **apparaat-ID** als de CNAME in de leaf-certificaten. Voor voor testdoeleinden Gebruik dit alleen [opdrachtregelprogramma](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md ) voor het genereren van certificaten voor apparaten.
    - **SAS:** Gebruik deze [opdrachtregelprogramma](https://www.npmjs.com/package/dps-keygen) voor het genereren van SAS-sleutels van apparaat. Gebruik de groep **primaire sleutel** uit de vorige stap. De apparaat-ID moet kleine letters.

      Voor het installeren van de [sleutel generator-hulpprogramma](https://github.com/Azure/dps-keygen), voer de volgende opdracht uit:

      ```cmd/sh
      npm i -g dps-keygen
      ```

      Voor het genereren van een apparaatsleutel van de primaire groep SAS-sleutel, moet u de volgende opdracht uitvoeren:

      ```cmd/sh
      dps-keygen -mk:<Primary_Key(GroupSAS)> -di:<device_id>
      ```

1. Als u uw apparaten instelt, flash elk apparaat voorzien van de **bereik-ID**, **apparaat-ID**, en **X.509-certificaat voor apparaten** of **SAS-sleutel**.

1. Schakel op het apparaat voor het verbinding maken met uw IoT Central-toepassing. Als u op een apparaat, het eerst verbinding maakt met DPS worden de registratiegegevens IoT Central opgehaald.

1. Het verbonden apparaat wordt in eerste instantie weergegeven als een **niet-gekoppelde apparaat** op de **Device Explorer** pagina. Het apparaat Inrichtingsstatus is **geregistreerde**. **Koppelen** het apparaat aan de sjabloon van het betreffende apparaat en het apparaat verbinding maakt met uw IoT Central-toepassing goedkeuren. Het apparaat kan vervolgens een verbindingsreeks ophaalt uit IoT Hub en beginnen met het verzenden van gegevens. Apparaten inrichten is voltooid en de status van de inrichting is nu **ingerichte**.

## <a name="provisioning-status"></a>Inrichtingsstatus

Wanneer een echt apparaat verbinding maakt met uw IoT Central-toepassing, de inrichting verandert de status als volgt:

1. Het apparaat Inrichtingsstatus is het eerste **geregistreerde**. Deze status betekent dat het apparaat in IoT Central wordt gemaakt en heeft een apparaat-ID. Een apparaat is geregistreerd bij:
    - Een nieuwe echt apparaat wordt toegevoegd aan de **Device Explorer** pagina.
    - Een set met apparaten wordt toegevoegd met behulp van **importeren** op de **Device Explorer** pagina.
    - Een apparaat niet is geregistreerd, handmatig in op de **Device Explorer** pagina, maar verbonden zijn met geldige referenties en wordt weergegeven als een **Unassociated** apparaat op de **Device Explorer**pagina.

1. De status voor apparaatinrichting wordt gewijzigd in **ingerichte** wanneer de inrichting stap door het apparaat dat met uw IoT Central-toepassing met geldige referenties verbonden is voltooid. In deze stap wordt het apparaat een verbindingsreeks opgehaald uit IoT Hub. Het apparaat kan nu verbinding maken met IoT Hub en beginnen met het verzenden van gegevens.

1. Een operator kan een apparaat blokkeren. Wanneer een apparaat is geblokkeerd, kan deze gegevens naar uw IoT Central-toepassing verzenden. Geblokkeerde apparaten hebben een status van de inrichting van **geblokkeerd**. Een operator moet het apparaat opnieuw instellen voordat u deze kunt doorgaan met het verzenden van gegevens. Wanneer een apparaat de status van de inrichting wordt geretourneerd naar de vorige waarde door een operator beter zicht **geregistreerde** of **ingerichte**.

## <a name="get-a-connection-string"></a>Een verbindingsreeks ophalen

De volgende stappen wordt beschreven hoe u een verbindingsreeks voor een apparaat kan krijgen:

1. Selecteer **Connect** op de **Device Explorer** pagina voor de verbindingsdetails: **Scope-ID**, **apparaat-ID**, en **apparaat primaire sleutel**:

    ![Verbindingsdetails](media/concepts-connectivity-experimental/device-connect.png)

1. Gebruik de `dps-keygen` opdrachtregel-hulpprogramma voor het genereren van een verbindingsreeks:  Voor het installeren van de [sleutel generator-hulpprogramma](https://github.com/Azure/dps-keygen), voer de volgende opdracht uit:

    ```cmd/sh
    npm i -g dps-keygen
    ```

    Voor het genereren van een verbindingsreeks, moet u de volgende opdracht uitvoeren:

    ```cmd/sh
    dps-keygen -di:<device_id> -dk:<device_key> -si:<scope_id>
    ```

## <a name="sdk-support"></a>SDK-ondersteuning

De apparaat-SDK's van Azure-aanbieding de eenvoudigste manier voor u de apparaatcode van uw implementeert. Het volgende apparaat-SDK's zijn beschikbaar:

- [Azure IoT-SDK voor C](https://github.com/azure/azure-iot-sdk-c)
- [Azure IoT-SDK voor Python](https://github.com/azure/azure-iot-sdk-python)
- [Azure IoT-SDK voor Node.js](https://github.com/azure/azure-iot-sdk-node)
- [Azure IoT-SDK voor Java](https://github.com/azure/azure-iot-sdk-java)
- [Azure IoT-SDK voor .NET](https://github.com/azure/azure-iot-sdk-csharp)

Elk apparaat verbinding maakt met behulp van een unieke verbindingsreeks waarmee het apparaat. Een apparaat kan alleen verbinding maken met de IoT-hub wanneer deze geregistreerd. Wanneer u een echt apparaat in uw Azure IoT Central-toepassing maakt, de toepassing genereert de informatie die u nodig hebt om te maken van een verbindingsreeks op met `dps-keygen`.

### <a name="sdk-features-and-iot-hub-connectivity"></a>SDK-functies en IoT Hub-connectiviteit

Alle communicatie van het apparaat met IoT Hub maakt gebruik van de volgende opties voor netwerkconnectiviteit van IoT-Hub:

- [Apparaat-naar-cloud-berichten](../iot-hub/iot-hub-devguide-messages-d2c.md)
- [Apparaatdubbels](../iot-hub/iot-hub-devguide-device-twins.md)

De volgende tabel geeft een overzicht van hoe Azure IoT Central apparaatfuncties toegewezen bij IoT Hub-functies:

| Azure IoT Central | Azure IoT Hub |
| ----------- | ------- |
| Meting: Telemetrie | Apparaat-naar-cloud-berichten |
| Apparaateigenschappen | Apparaatdubbel-gerapporteerde eigenschappen |
| Instellingen | Apparaatdubbel gewenst en gerapporteerde eigenschappen |

Zie voor meer informatie over het gebruik van de apparaat-SDK's, een van de volgende artikelen bijvoorbeeld code:

- [Een generieke Node.js-client verbinden met uw Azure IoT Central-toepassing](howto-connect-nodejs-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json)
- [Een Raspberry Pi-apparaat verbinden met uw Azure IoT Central-toepassing](howto-connect-raspberry-pi-python.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json)
- [Een apparaat van de kit DevDiv verbinden met uw Azure IoT Central application](howto-connect-devkit-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json).

### <a name="protocols"></a>Protocollen

De apparaat-SDK's ondersteunen de volgende netwerkprotocollen om verbinding te maken naar een IoT-hub:

- MQTT
- AMQP
- HTTPS

Zie voor meer informatie over deze verschil protocollen en richtlijnen over het kiezen van een [een communicatieprotocol kiezen](../iot-hub/iot-hub-devguide-protocols.md).

Als uw apparaat geen van de ondersteunde protocollen gebruiken kan, kunt u Azure IoT Edge kunt gebruiken om u te conversie-protocol. IoT Edge biedt ondersteuning voor andere intelligence op de edge-scenario's voor verwerking naar de rand van de toepassing voor Azure IoT Central-offload.

## <a name="security"></a>Beveiliging

Alle gegevens die worden uitgewisseld tussen apparaten en uw Azure IoT Central is versleuteld. IoT Hub wordt geverifieerd voor elke aanvraag van een apparaat dat verbinding met een van de IoT Hub apparaat gerichte eindpunten maakt. Om te voorkomen dat het uitwisselen van referenties via de kabel, een apparaat ondertekende tokens gebruikt om te verifiëren. Voor meer informatie ziet, [beheren van toegang tot IoT Hub](../iot-hub/iot-hub-devguide-security.md).

## <a name="next-steps"></a>Volgende stappen

Nu dat u hebt geleerd over verbindingsmogelijkheden voor apparaten in Azure IoT Central, vindt hier u de voorgestelde volgende stappen:

- [Voorbereiden en een apparaat DevKit verbinden](howto-connect-devkit-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json)
- [Raspberry Pi voorbereiden en verbinden](howto-connect-raspberry-pi-python.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json)
- [Een generieke Node.js-client verbinden met uw Azure IoT Central-toepassing](howto-connect-nodejs-experimental.md?toc=/azure/iot-central-experimental/toc.json&bc=/azure/iot-central-experimental/breadcrumb/toc.json)
- [C-SDK: Provisioning Device SDK-Client](https://github.com/Azure/azure-iot-sdk-c/blob/master/provisioning_client/devdoc/using_provisioning_client.md)
