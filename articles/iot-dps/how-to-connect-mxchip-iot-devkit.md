---
title: Het gebruik van automatische inrichting van Azure IoT Hub Device Provisioning Service voor het registreren van de MXChip IoT DevKit met IoT Hub | Microsoft Docs
description: Het gebruik van automatische inrichting van Azure IoT Hub Device Provisioning Service voor het registreren van de MXChip IoT DevKit met IoT Hub.
author: liydu
ms.author: liydu
ms.date: 12/18/2018
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: jeffya
ms.openlocfilehash: 513d4e51ced798f5fe49e2e1e59fcc8ec02d9c2c
ms.sourcegitcommit: a65b424bdfa019a42f36f1ce7eee9844e493f293
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/04/2019
ms.locfileid: "55699190"
---
# <a name="use-azure-iot-hub-device-provisioning-service-auto-provisioning-to-register-the-mxchip-iot-devkit-with-iot-hub"></a>Gebruik automatische inrichting van Azure IoT Hub Device Provisioning Service voor het registreren van de MXChip IoT DevKit met IoT Hub

In dit artikel wordt beschreven hoe u Azure IoT Hub Device Provisioning Service [automatische inrichting](concepts-auto-provisioning.md), de MXChip IoT DevKit registreren bij Azure IoT Hub. In deze zelfstudie leert u het volgende:

* De globale-eindpunt van de Device Provisioning-service configureren op een apparaat.
* Gebruik een unieke apparaat-geheim (ud's) voor het genereren van een X.509-certificaat.
* Een afzonderlijk apparaat inschrijven.
* Controleer of dat het apparaat is geregistreerd.

De [MXChip IoT DevKit](https://aka.ms/iot-devkit) is een alles-in-een Arduino-compatibel bord met uitgebreide randapparatuur en sensoren. U kunt ontwikkelen voor met behulp van [Azure IoT-apparaat Workbench](https://aka.ms/iot-workbench) of [hulpprogramma's voor Azure IoT](https://aka.ms/azure-iot-tools) -uitbreidingspakket in Visual Studio Code. De DevKit wordt geleverd met een groeiend [projecten catalogus](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/) om u te helpen uw prototype Internet of Things (IoT)-oplossingen die van Azure-services profiteren.

## <a name="before-you-begin"></a>Voordat u begint

Als u wilt de stappen in deze zelfstudie hebt voltooid, moet u eerst de volgende taken uitvoeren:

* Voorbereiden van uw DevKit met de volgende stappen in [IoT DevKit AZ3166 verbinding maken met Azure IoT Hub in de cloud](/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started.md).
* Een upgrade uitvoeren naar de meest recente firmware (1.3.0 of hoger) met de [firmware-Update DevKit](https://microsoft.github.io/azure-iot-developer-kit/docs/firmware-upgrading/) zelfstudie.
* Maken en koppelen van een IoT-Hub met een exemplaar van Device Provisioning service met de volgende stappen in [instellen van de IoT Hub Device Provisioning Service met de Azure-portal](/azure/iot-dps/quick-setup-auto-provision).

## <a name="open-sample-project"></a>Open voorbeeldproject

1. Zorg ervoor dat uw IoT DevKit is **niet verbonden** op uw computer. VS Code eerst start, en vervolgens de DevKit verbinden met uw computer.

1. Klik op `F1` om te openen de command palette, typ en selecteer **Azure IoT-apparaat Workbench: Voorbeelden openen...** . Selecteer vervolgens **IoT DevKit** als bord.

1. Zoek in de pagina IoT Workbench voorbeelden **Device Registration service met DPS** en klikt u op **Open voorbeeld**. Vervolgens selecteert het standaardpad voor het downloaden van de voorbeeldcode.
    ![Open voorbeeld](media/how-to-connect-mxchip-iot-devkit/open-sample.png)

## <a name="save-a-unique-device-secret-on-device-security-storage"></a>Een unieke apparaat-geheim opslaan op opslagruimte op apparaten van beveiliging

Automatische inrichting kan worden geconfigureerd op een apparaat, gebaseerd op van het apparaat [attestation-mechanisme](concepts-security.md#attestation-mechanism). Maakt gebruik van de MXChip IoT DevKit de [apparaat Identity Composition Engine](https://trustedcomputinggroup.org/wp-content/uploads/Foundational-Trust-for-IOT-and-Resource-Constrained-Devices.pdf) uit de [Trusted Computing Group](https://trustedcomputinggroup.org). Een **unieke apparaat-geheim** (ud's) die zijn opgeslagen in een STSAFE security-chip ([STSAFE A100](https://microsoft.github.io/azure-iot-developer-kit/docs/understand-security-chip/)) op de DevKit wordt gebruikt voor het genereren van het apparaat met de unieke [X.509-certificaat](concepts-security.md#x509-certificates). Het certificaat wordt later gebruikt voor het registratieproces in de Device Provisioning service en tijdens de registratie tijdens runtime.

Een typische ud's is een 64-tekenreeks, zoals te zien is in het volgende voorbeeld:

```
19e25a259d0c2be03a02d416c05c48ccd0cc7d1743458aae1cb488b074993eae
```

Om op te slaan een ud's op de DevKit:

1. Klik op de statusbalk om te selecteren van de COM-poort voor de DevKit in VS Code.
  ![Selecteer COM-poort](media/how-to-connect-mxchip-iot-devkit/select-com.png)

1. Op DevKit, houdt u **een knop**, push en laat de **opnieuw** knop en loslaten **een knop**. Uw DevKit krijgt de configuratiemodus.

1. Klik op `F1` om te openen de command palette, typ en selecteer **Azure IoT-apparaat Workbench: Configureren van apparaatinstellingen... > Config unieke apparaat-tekenreeks (ud's)**.
  ![Ud's configureren](media/how-to-connect-mxchip-iot-devkit/config-uds.png)

1. Noteer de gegenereerde ud's-tekenreeks. U hebt deze voor het genereren van het X.509-certificaat. Druk op `Enter`.
  ![Ud's kopiëren](media/how-to-connect-mxchip-iot-devkit/copy-uds.png)

1. Controleer of van de melding dat ud's is geconfigureerd op de STSAFE.
  ![Succes ud's configureren](media/how-to-connect-mxchip-iot-devkit/config-uds-success.png)

> [!NOTE]
> U kunt ook ud's via de seriële poort configureren met behulp van hulpprogramma's, zoals Putty. Ga als volgt [gebruik configuratiemodus](https://microsoft.github.io/azure-iot-developer-kit/docs/use-configuration-mode/) om dit te doen.

## <a name="update-the-global-device-endpoint-and-id-scope"></a>De Global Device Endpoint en de ID-bereik bijwerken

In de apparaatcode, moet u opgeven de [eindpunt voor Apparaatinrichting](/azure/iot-dps/concepts-service#device-provisioning-endpoint) en ID-bereik zodat de isolatie van tenants.

1. Selecteer in de Azure portal, de **overzicht** deelvenster van de Device Provisioning service en noteert u de **Global device endpoint** en **ID-bereik** waarden.
  ![Device Provisioning Service Global Endpoint en ID-bereik](media/how-to-connect-mxchip-iot-devkit/dps-global-endpoint.png)

1. Open **DeKitDPS.ino**. Zoeken en vervangen `[Global Device Endpoint]` en `[ID Scope]` door de waarden die u zojuist hebt genoteerd.
  ![Device Provisioning Service-eindpunt](media/how-to-connect-mxchip-iot-devkit/endpoint.png)

1. Vul de `registrationId` variabele in de code. Alleen alfanumerieke tekens, kleine letters, en het afbreekstreepje combinatie met een maximum van 128 tekens is toegestaan. Ook de waarde genoteerd.
  ![Registratie-ID](media/how-to-connect-mxchip-iot-devkit/registration-id.png)

1. Klik op `F1`, typt en selecteer **Azure IoT-apparaat Workbench: Uploaden apparaatcode**. Het begint compileren en de code uploaden naar DevKit.
  ![Apparaat uploaden](media/how-to-connect-mxchip-iot-devkit/device-upload.png)

## <a name="generate-x509-certificate"></a>X.509-certificaat genereren

De [attestation-mechanisme](/azure/iot-dps/concepts-device#attestation-mechanism) die worden gebruikt door dit voorbeeld wordt het x.509-certificaat. U moet een hulpprogramma gebruiken voor het genereren.

> [!NOTE]
> De X.509-certificaatgenerator ondersteunt alleen Windows nu.

1. In VS Code, klikt u op `F1`, typt en selecteer **nieuwe Terminal openen** om terminal-venster te openen.

1. Voer `dps_cert_gen.exe` in `tool` map.

1. Geef de locatie van het gecompileerde binaire bestand als `..\.build\DevKitDPS`. Plak de **ud's** en **registratie-id** u net hebt genoteerd. 
  ![X.509 genereren](media/how-to-connect-mxchip-iot-devkit/gen-x509.png)

1. Een `.pem` X.509-certificaat wordt gegenereerd in dezelfde map.
  ![X.509-bestand](media/how-to-connect-mxchip-iot-devkit/pem-file.png)

## <a name="create-a-device-enrollment-entry"></a>Een vermelding voor apparaatinschrijving maken

1. Open de Service van uw apparaat inrichten in de Azure-portal, gaat u voor het beheren van inschrijvingen sectie en op **afzonderlijke registratie toevoegen**.
  ![Afzonderlijke registratie toevoegen](media/how-to-connect-mxchip-iot-devkit/add-enrollment.png)

1. Klik op het pictogram naast **primaire PEM- of cer-certificaatbestand** het uploaden van de `.pem` bestand dat is gegenereerd.
  ![Upload .pem](media/how-to-connect-mxchip-iot-devkit/upload-pem.png)

## <a name="verify-the-devkit-is-registered-with-azure-iot-hub"></a>Controleer of dat de DevKit is geregistreerd bij Azure IoT Hub

Druk op de **opnieuw** knop op uw DevKit. U ziet **DPS verbonden!** op het scherm DevKit. Nadat het apparaat opnieuw is opgestart, worden de volgende acties uitgevoerd:

1. Vanaf het apparaat wordt een registratie-aanvraag verzonden naar Device Provisioning Service.
1. De Device Provisioning service teruggestuurd een registratie-uitdaging waarop het apparaat reageert.
1. Registratie is gelukt verzendt de Device Provisioning-service de IoT Hub-URI, apparaat-ID en de versleutelde sleutel terug naar het apparaat.
1. De IoT Hub-clienttoepassing op het apparaat verbinding maakt met uw hub.
1. U ziet het apparaat worden weergegeven in de IoT Hub Device Explorer op geslaagde verbinding met de hub.
  ![Apparaat is geregistreerd](./media/how-to-connect-mxchip-iot-devkit/device-registered.png)

## <a name="problems-and-feedback"></a>Problemen en feedback

Als u problemen ondervindt, raadpleegt u de Iot DevKit [Veelgestelde vragen over](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/), of contact opnemen met de volgende kanalen voor ondersteuning:

* [Gitter.im](http://gitter.im/Microsoft/azure-iot-developer-kit)
* [Stack Overflow](https://stackoverflow.com/questions/tagged/iot-devkit)

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd om een apparaat veilig met de Device Provisioning Service met behulp van de apparaat Identity Composition Engine, schrijven, zodat het apparaat automatisch met Azure IoT Hub registreren kan. 

Kortom, hebt u geleerd hoe u:

> [!div class="checklist"]
> * De globale-eindpunt van de Device Provisioning-service configureren op een apparaat.
> * Gebruik een unieke apparaat-geheim voor het genereren van een X.509-certificaat.
> * Een afzonderlijk apparaat inschrijven.
> * Controleer of dat het apparaat is geregistreerd.

Meer informatie over het [een gesimuleerd apparaat maken en inrichten](./quick-create-simulated-device.md).

