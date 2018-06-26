---
title: Het gebruik van Azure IoT Hub apparaat-inrichtingsservice automatische inrichting de MXChip IoT DevKit registreren met IoT Hub | Microsoft Docs
description: Het registreren van de IoT MXChip DevKit met IoT Hub met automatisch inrichten van Azure IoT Hub apparaat-inrichtingsservice.
author: liydu
ms.author: liydu
ms.date: 04/04/2018
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: jeffya
ms.openlocfilehash: a5030c7363d02fe9600183e058f2ff3cc23e9475
ms.sourcegitcommit: 6eb14a2c7ffb1afa4d502f5162f7283d4aceb9e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/25/2018
ms.locfileid: "36750363"
---
# <a name="use-azure-iot-hub-device-provisioning-service-auto-provisioning-to-register-the-mxchip-iot-devkit-with-iot-hub"></a>Gebruik Azure IoT Hub apparaat-inrichtingsservice automatische inrichting voor het registreren van de IoT MXChip DevKit met IoT Hub

In dit artikel wordt beschreven hoe u Azure IoT Hub apparaat-inrichtingsservice [automatische inrichting](concepts-auto-provisioning.md), de MXChip IoT DevKit registreren met Azure IoT Hub. In deze zelfstudie leert u het volgende:

* Configureer het globale eindpunt van het apparaat op een apparaat-service inricht.
* Gebruik een unieke apparaat-geheim (UDS) voor het genereren van een X.509-certificaat.
* Een afzonderlijk apparaat inschrijven.
* Controleer of het apparaat is geregistreerd.

De [MXChip IoT DevKit](https://aka.ms/iot-devkit) een mededelingenbord alles in één Arduino compatibel met de randapparaten uitgebreide en sensoren is. U kunt voor het ontwikkelen met behulp van de [Visual Studio Code-uitbreiding voor Arduino](https://aka.ms/arduino). De DevKit wordt geleverd met een groeiende [projecten catalogus](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/) om te helpen uw prototype Internet der dingen (IoT)-oplossingen die van Azure-services gebruikmaken.

## <a name="before-you-begin"></a>Voordat u begint

Als u wilt de stappen in deze zelfstudie hebt voltooid, moet u eerst de volgende taken uitvoeren:

* Uw DevKit voorbereiden door de stappen in [IoT DevKit AZ3166 verbinding maken met Azure IoT Hub in de cloud](/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started).
* Upgrade naar de meest recente firmware (1.3.0 of hoger) met de [Update DevKit firmware](https://microsoft.github.io/azure-iot-developer-kit/docs/firmware-upgrading/) zelfstudie.
* Maken en koppelen van een IoT Hub met een service-exemplaar wordt ingericht door de stappen in apparaat [instellen van de IoT Hub apparaat inrichtingsservice met de Azure-portal](/azure/iot-dps/quick-setup-auto-provision).

## <a name="build-and-deploy-auto-provisioning-registration-software-to-the-device"></a>Maken en inrichten van automatische inschrijving software implementeren op het apparaat

Sluit de DevKit op het apparaat het inrichten van de service-exemplaar dat u hebt gemaakt:

1. Selecteer in de Azure-portal de **overzicht** deelvenster van het apparaat inrichten service en opmerking op de **globale apparaat eindpunt** en **ID-bereik** waarden.
  ![DP's globale eindpunt en ID-bereik](./media/how-to-connect-mxchip-iot-devkit/dps-global-endpoint.png)

2. Zorg ervoor dat u hebt `git` op deze computer geïnstalleerd en dat deze wordt toegevoegd aan de omgevingsvariabelen die toegankelijk is voor het opdrachtvenster. Zie [Software vrijheid Conservancy Git clienthulpprogramma's](https://git-scm.com/download/) naar de meest recente versie hebt geïnstalleerd.

3. Open een opdrachtprompt. Kloon de GitHub-opslagplaats voor het apparaat inrichten voorbeeldcode service:
  ```bash
  git clone https://github.com/DevKitExamples/DevKitDPS.git
  ```

4. Open Visual Studio Code, de DevKit aansluiten op uw computer en open vervolgens de map waarin de code die u gekloond.

5. Open **DevKitDPS.ino**. Zoeken en vervangen `[Global Device Endpoint]` en `[ID Scope]` met de waarden die u zojuist hebt genoteerd omlaag.
  ![Eindpunt van de DP's](./media/how-to-connect-mxchip-iot-devkit/endpoint.png) kunt u de **registrationId** leeg. De toepassing genereert voor u op basis van de MAC-adres en firmware-versie. Als u aanpassen van de registratie-ID wilt, moet u gebruik alleen alfanumerieke of kleine letters en het koppelteken combinaties met een maximum van 128 tekens. Zie voor meer informatie [apparaatinschrijvingen met Azure portal beheren](https://docs.microsoft.com/azure/iot-dps/how-to-manage-enrollments).

6. Gebruik snelle openen in VS-Code (Windows: `Ctrl+P`, Mac OS: `Cmd+P`) en type *uploaden van het apparaat van de taak* bouwen en de code uploaden naar de DevKit.

7. Het uitvoervenster geeft aan of de taak voltooid is.

## <a name="save-a-unique-device-secret-on-an-stsafe-security-chip"></a>Een unieke apparaat-geheim opslaan op een STSAFE security-chip

Automatische inrichting kan worden geconfigureerd op een apparaat op basis van het apparaat [attestation mechanisme](concepts-security.md#attestation-mechanism). Maakt gebruik van de IoT MXChip DevKit de [apparaat identiteit compositie-Engine](https://trustedcomputinggroup.org/wp-content/uploads/Foundational-Trust-for-IOT-and-Resource-Constrained-Devices.pdf) van de [Trusted Computing Group](https://trustedcomputinggroup.org). Een *unieke apparaat-geheim* (UDS) opgeslagen in een beveiligingsgroep STSAFE chip op de DevKit wordt gebruikt voor het genereren van het apparaat met de unieke [X.509-certificaat](concepts-security.md#x509-certificates). Het certificaat wordt later gebruikt voor het inschrijvingsproces in de apparaat-service inricht en tijdens de registratie tijdens runtime.

Een typische unieke apparaat-geheim is een 64-tekenreeks, zoals in het volgende voorbeeld:

```
19e25a259d0c2be03a02d416c05c48ccd0cc7d1743458aae1cb488b074993eae
```

Elk van de twee tekens wordt gebruikt als de hexadecimale waarde in de berekening van de beveiliging. The preceding sample UDS is resolved to: `0x19`, `0xe2`, `0x5a`, `0x25`, `0x9d`, `0x0c`, `0x2b`, `0xe0`, `0x3a`, `0x02`, `0xd4`, `0x16`, `0xc0`, `0x5c`, `0x48`, `0xcc`, `0xd0`, `0xcc`, `0x7d`, `0x17`, `0x43`, `0x45`, `0x8a`, `0xae`, `0x1c`, `0xb4`, `0x88`, `0xb0`, `0x74`, `0x99`, `0x3e`, `0xae`.

Een unieke apparaat-geheim opslaan op de DevKit:

1. Open de seriële monitor met een hulpprogramma zoals Putty. Zie [gebruik configuratiemodus](https://microsoft.github.io/azure-iot-developer-kit/docs/use-configuration-mode/) voor meer informatie.

2. Met de DevKit aangesloten op de computer, houdt u de **A** knop, en druk vervolgens op en laat de **opnieuw** om in te voeren configuratiemodus. Het scherm toont de DevKit-ID en de configuratie.

3. Het voorbeeld UDS tekenreeks en wijzigt u een of meer tekens in andere waarden tussen nemen `0` en `f` voor uw eigen UDS.

4. Typ in het venster seriële monitor *set_dps_uds [your_own_uds_value]* en selecteer Enter.
  > [!NOTE]
  > Bijvoorbeeld, als u uw eigen UDS ingesteld door het wijzigen van de laatste twee tekens naar `f`, moet u de opdracht als volgt opgeven: `set_dps_uds 19e25a259d0c2be03a02d416c05c48ccd0cc7d1743458aae1cb488b074993eff`.

5. De seriële zonder monitorvenster te sluiten, drukt u op de **opnieuw** knop op de DevKit.

6. Noteer de **DevKit MAC-adres** en **DevKit firmwareversie** waarden.
  ![Firmwareversie](./media/how-to-connect-mxchip-iot-devkit/firmware-version.png)

## <a name="generate-an-x509-certificate"></a>Genereren van een X.509-certificaat

### <a name="windows"></a>Windows

1. Open Windows Verkenner en Ga naar de map waarin het apparaat inrichten service voorbeeldcode die u eerder hebt gekloond. In de **.build** map, zoeken en kopiëren **DPS.ino.bin** en **DPS.ino.map** in de map waarin de code.
  ![Gegenereerde bestanden](./media/how-to-connect-mxchip-iot-devkit/generated-files.png)
  > [!NOTE]
  > Als u hebt gewijzigd de `built.path` configuratie voor Arduino naar een andere map, moet u deze bestanden niet vinden in de map die u hebt geconfigureerd.

2. Plak deze twee bestanden in de **extra** map op hetzelfde niveau met de **.build** map.

3. Run **dps_cert_gen.exe**. Volg de aanwijzingen in te voeren uw **UDS**, wordt de **MAC-adres** voor de DevKit en de **firmwareversie** voor het genereren van het X.509-certificaat.
  ![DP's-cert-gen.exe uitvoeren](./media/how-to-connect-mxchip-iot-devkit/dps-cert-gen.png)

4. Nadat het X.509-certificaat wordt gegenereerd, een **.pem** certificaat wordt opgeslagen in dezelfde map.

## <a name="create-a-device-enrollment-entry-in-the-device-provisioning-service"></a>Een device enrollment-vermelding in het service-inrichting apparaat maken

1. In de Azure portal, gaat u naar uw apparaat inrichtingsservice-exemplaar. Selecteer **inschrijvingen beheren**, en selecteer vervolgens de **afzonderlijke inschrijvingen** tabblad. ![Afzonderlijke inschrijvingen](./media/how-to-connect-mxchip-iot-devkit/individual-enrollments.png)

2. Selecteer **Toevoegen**.

3. In het deelvenster 'Inschrijving toevoegen':
   - Selecteer **X.509** onder **mechanisme**
   - Klik op 'Een bestand selecteren' onder **primaire certificaat voor .pem of .cer-bestand**
   - in het dialoogvenster bestand openen, gaat u naar en upload de **.pem** zojuist gegenereerde certificaat
   - Laat de overige als standaard en klik op **opslaan**

   ![Certificaat uploaden](./media/how-to-connect-mxchip-iot-devkit/upload-cert.png)

  > [!NOTE]
  > Als er een fout opgetreden bij dit bericht:
  >
  > `{"message":"BadRequest:{\r\n \"errorCode\": 400004,\r\n \"trackingId\": \"1b82d826-ccb4-4e54-91d3-0b25daee8974\",\r\n \"message\": \"The certificate is not a valid base64 string value\",\r\n \"timestampUtc\": \"2018-05-09T13:52:42.7122256Z\"\r\n}"}`
  >
  > Open het certificaatbestand **.pem** als tekst (open met Kladblok of een teksteditor), de regels en verwijderen:
  >
  > `"-----BEGIN CERTIFICATE-----"` en `"-----END CERTIFICATE-----"`.
  >


## <a name="start-the-devkit"></a>Start de DevKit

1. Open tegenover Code en de seriële monitor.

2. Druk op de **opnieuw** knop op uw DevKit.

Ziet u de DevKit start de registratie met uw service voor het inrichten van apparaat.

![VS Code-uitvoer](./media/how-to-connect-mxchip-iot-devkit/vscode-output.png)

## <a name="verify-that-the-devkit-is-registered-with-azure-iot-hub"></a>Controleer of de DevKit is geregistreerd met Azure IoT Hub

Nadat het apparaat opnieuw wordt opgestart, worden de volgende acties uitgevoerd:

1. Het apparaat verzendt een aanvraag voor functieregistratie op uw apparaat-service inricht.
2. De apparaat-service inricht vraagt terug registratie waarop het apparaat reageert.
3. Op de registratie is voltooid stuurt de apparaat-service inricht de URI van de IoT Hub, apparaat-ID en de versleutelde sleutel terug naar het apparaat.
4. De IoT Hub-clienttoepassing op het apparaat verbinding maakt met uw hub.
5. In de verbinding is gemaakt met de hub ziet u het apparaat worden weergegeven in de IoT Hub apparaat Explorer.
  ![Apparaat is geregistreerd](./media/how-to-connect-mxchip-iot-devkit/device-registered.png)

## <a name="change-the-device-id"></a>Wijzig de apparaat-ID

De geregistreerd bij Azure IoT Hub apparaat-ID is *AZ3166*. Als u wijzigen van de ID wilt, volg de instructies in [aanpassen van apparaat-ID](https://microsoft.github.io/azure-iot-developer-kit/docs/customize-device-id/).

## <a name="problems-and-feedback"></a>Problemen en feedback

Als u problemen ondervindt, raadpleegt u de Iot DevKit [Veelgestelde vragen over](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/), of met de volgende kanalen voor ondersteuning:

* [Gitter.IM](http://gitter.im/Microsoft/azure-iot-developer-kit)
* [StackOverflow](https://stackoverflow.com/questions/tagged/iot-devkit)

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd om een apparaat veilig naar de service inrichten met behulp van de apparaat-id samenstelling Engine, zodat het apparaat kan automatisch wordt geregistreerd bij Azure IoT Hub apparaat te registreren. 

In de samenvatting, hebt u geleerd hoe:

> [!div class="checklist"]
> * Configureer het globale eindpunt van het apparaat op een apparaat-service inricht.
> * Gebruik een unieke apparaat-geheim voor het genereren van een X.509-certificaat.
> * Een afzonderlijk apparaat inschrijven.
> * Controleer of het apparaat is geregistreerd.

Meer informatie over hoe [maken en een gesimuleerd apparaat in te richten](./quick-create-simulated-device.md).

