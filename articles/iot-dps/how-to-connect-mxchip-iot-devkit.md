---
title: Het gebruik van MXChip IoT DevKit verbinding maken met Azure IoT Hub apparaat-inrichtingsservice | Microsoft Docs
description: Het gebruik van MXChip IoT DevKit verbinding maken met Azure IoT Hub apparaat inrichten van Service
services: iot-dps
keywords: 
author: liydu
ms.author: liydu
ms.date: 02/20/2018
ms.topic: article
ms.service: iot-dps
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 77c8a6a5e384d27dca2582cc0fedc2bd23c0592e
ms.sourcegitcommit: 83ea7c4e12fc47b83978a1e9391f8bb808b41f97
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/28/2018
---
# <a name="connect-mxchip-iot-devkit-to-azure-iot-hub-device-provisioning-service"></a>Verbinding maken met Azure IoT Hub apparaat-Service inricht met MXChip IoT DevKit

In dit artikel wordt beschreven hoe DevKit configureren zodat automatisch wordt geregistreerd met IoT Hub met behulp van de Service voor het inrichten van apparaten. In deze zelfstudie leert u het volgende:

* Globale eindpunt van de DP's configureren op apparaat
* Unieke apparaat geheim Regulatie gebruikt voor het genereren van het X.509-certificaat
* Afzonderlijke apparaat inschrijven
* Controleer of dat het apparaat is geregistreerd

De [MXChip IoT DevKit](https://aka.ms/iot-devkit) is een Arduino alles in één mededelingenbord compatibel is met de randapparaten uitgebreide en sensoren. U kunt ontwikkelen via [Visual Studio Code-uitbreiding voor Arduino](https://aka.ms/arduino). En beschikt u over een groeiende [projecten catalogus](https://microsoft.github.io/azure-iot-developer-kit/docs/projects/) om u te begeleiden prototype Internet der dingen (IoT) oplossingen die van Microsoft Azure-services gebruikmaken.

## <a name="before-you-begin"></a>Voordat u begint

Als u wilt de stappen in deze zelfstudie hebt voltooid, moet u het volgende:

* Voorbereiden van uw DevKit met [Getting Started Guide](https://docs.microsoft.com/azure/iot-hub/iot-hub-arduino-iot-devkit-az3166-get-started).
* Upgrade naar de meest recente firmware (> = 1.3.0) met [Firmware bijwerken](https://microsoft.github.io/azure-iot-developer-kit/docs/firmware-upgrading/) zelfstudie.
* Maken en koppelen van IoT Hub met apparaat inrichtingsservice-exemplaar met [instellen van automatische inrichting](https://docs.microsoft.com/en-us/azure/iot-dps/quick-setup-auto-provision).

## <a name="set-up-the-device-provisioning-service-configuration-on-the-device"></a>De configuratie van de inrichtingsservice apparaat op het apparaat instellen

Inschakelen van de DevKit verbinding maken met de Device inrichtingsservice-exemplaar dat u hebt gemaakt:

1. Selecteer in de Azure-portal, de **overzicht** van uw apparaat inrichtingsservice en noteer de **globale apparaat eindpunt** en **ID-bereik** waarde.
  ![DP's globale eindpunt en ID-bereik](./media/how-to-connect-mxchip-iot-devkit/dps-global-endpoint.png)

2. Zorg ervoor dat `git` op de computer wordt geïnstalleerd en toegevoegd aan de omgevingsvariabelen die voor het opdrachtvenster toegankelijk zijn. Zie [Software vrijheid Conservancy Git clienthulpprogramma's](https://git-scm.com/download/) naar de meest recente versie hebt geïnstalleerd.

3. Open een opdrachtprompt. Kloon de GitHub-opslagplaats voor voorbeeldcode DP's:
  ```bash
  git clone https://github.com/DevKitExamples/DevKitDPS.git
  ```

4. VS Code starten en DevKit verbinding te maken met computer en open de map bevat de code die u gekloond.

5. Open **DevKitDPS.ino**, zoeken en vervangen `[Global Device Endpoint]` en `[ID Scope]` met de waarden die u zojuist hebt noteren.
  ![Eindpunt van de DP's](./media/how-to-connect-mxchip-iot-devkit/endpoint.png) kunt u de **registrationId** als leeg, de toepassing genereert een op basis van de MAC-adres en firmware-versie. Als u aanpassen wilt, wordt de registratie-ID gebruiken alfanumerieke, kleine letters en koppelteken combinaties alleen met maximaal 128 tekens lang is. Zie voor meer informatie [apparaatinschrijvingen met Azure portal beheren](https://docs.microsoft.com/en-us/azure/iot-dps/how-to-manage-enrollments).

6. Gebruik **snelle Open** in VS-Code (Windows: `Ctrl+P`, Mac OS: `Cmd+P`) en type **uploaden van het apparaat van de taak** bouwen en de code uploaden naar de DevKit.

7. Houd rekening met het succes van de taak in het uitvoervenster.

## <a name="save-unique-device-secret-on-stsafe-security-chip"></a>Unieke apparaat-geheim op STSAFE security chip opslaan

Inrichtingsservice apparaat kan worden geconfigureerd op het apparaat op basis van de [Hardware Security Module (HSM)](https://azure.microsoft.com/en-us/blog/azure-iot-supports-new-security-hardware-to-strengthen-iot-security/). Maakt gebruik van DevKit [apparaat identiteit samenstelling Engine (OPDELEN)](https://trustedcomputinggroup.org/wp-content/uploads/Foundational-Trust-for-IOT-and-Resource-Constrained-Devices.pdf) van de [Trusted Computing Group (TCG)](https://trustedcomputinggroup.org). Een **unieke apparaat geheim Regulatie** opgeslagen in de STSAFE beveiliging chip op de DevKit wordt gebruikt voor het genereren van het apparaat uniek [X.509](https://docs.microsoft.com/en-us/azure/iot-dps/tutorial-set-up-device#select-a-hardware-security-module) certificaat. Het certificaat kan later worden gebruikt voor het inschrijvingsproces in de Service voor het inrichten van apparaten.

Een typische **unieke apparaat geheim Regulatie** string 64 tekens lang is. Een voorbeeld van een UDS als is hieronder:

```
19e25a259d0c2be03a02d416c05c48ccd0cc7d1743458aae1cb488b074993eae
```

Elk van de twee tekens wordt gebruikt als hexadecimale waarde in de berekening van de beveiliging. Het bovenstaande voorbeeld UDS is omgezet naar: '`0x19`, `0xe2`, `0x5a`, `0x25`, `0x9d`, `0x0c`, `0x2b`, `0xe0`, `0x3a`, `0x02`, `0xd4`, `0x16` , `0xc0`, `0x5c`, `0x48`, `0xcc`, `0xd0`, `0xcc`, `0x7d`, `0x17`, `0x43`, `0x45`, `0x8a`, `0xae`, `0x1c`, `0xb4`, `0x88`, `0xb0`, `0x74`, `0x99`, `0x3e`, `0xae`".

Unieke apparaat-geheim opslaan op de DevKit:

1. Seriële monitor geopend via een hulpprogramma zoals Putty, Zie [gebruik configuratiemodus](https://microsoft.github.io/azure-iot-developer-kit/docs/use-configuration-mode/) voor meer informatie.

2. Met de DevKit verbonden met de computer, houdt u A, en vervolgens push en release van de knop opnieuw instellen om configuratiemodus. De DevKit-ID moet worden weergegeven in het scherm en **'Configuration'**.

3. De bovenstaande lang voorbeeld UDS tekenreeks nemen en te wijzigen in een of meer tekens andere waarden tussen `0` en `f`. Dit wordt gebruikt als uw eigen UDS.

4. Typ in het venster van de seriële monitor **set_dps_uds [your_own_uds_value]** en druk op ENTER op te slaan.
  > [!NOTE]
  > Bijvoorbeeld, als u uw eigen UDS ingesteld door het wijzigen van de laatste twee tekens naar `f`, moet u de opdracht zoals invoeren **set_dps_uds 19e25a259d0c2be03a02d416c05c48ccd0cc7d1743458aae1cb488b074993eff**.

5. De seriële zonder monitorvenster te sluiten, opnieuw druk op knop op de DevKit.

6. Noteer **DevKit MAC-adres** en **DevKit firmwareversie** waarde.
  ![Firmwareversie](./media/how-to-connect-mxchip-iot-devkit/firmware-version.png)

## <a name="generate-x509-certificate"></a>X.509-certificaat genereren

### <a name="windows"></a>Windows

1. Open Windows Verkenner en Ga naar de map bevatten de DSP voorbeeldcode gekloond, wordt er een **.build** map, zoeken en kopiëren **DPS.ino.bin** en **DPS.ino.map** erin.
  ![Gegenereerde bestanden](./media/how-to-connect-mxchip-iot-devkit/generated-files.png)
  > [!NOTE]
  > Als u hebt gewijzigd de `built.path` configuratie voor Arduino naar andere map. U moet deze bestanden niet vinden in de map die u hebt geconfigureerd.

2. Plak deze twee bestanden in **extra** map op hetzelfde niveau met **.build** map.

3. Voer **dps_cert_gen.exe**, volg de aanwijzingen in te voeren uw **UDS**, **MAC-adres** voor de DevKit en de **firmwareversie** genereren het X.509-certificaat.
  ![Run dps-cert-gen.exe](./media/how-to-connect-mxchip-iot-devkit/dps-cert-gen.png)

4. Houd rekening met het succes van het genereren, een **.pem** certificaat wordt opgeslagen in dezelfde map.

## <a name="create-a-device-enrollment-entry-in-the-device-provisioning-service"></a>Een vermelding voor apparaatinschrijving maken in Device Provisioning Service

1. Ga in de Azure-portal naar uw provisioning-service. Klik op **Inschrijvingen beheren** en selecteer het tabblad **Afzonderlijke inschrijvingen**. ![Afzonderlijke inschrijvingen](./media/how-to-connect-mxchip-iot-devkit/individual-enrollments.png)

2. Klik op **Add**.

3. In **mechanisme**, kies **X.509**.
  ![Certificaat uploaden](./media/how-to-connect-mxchip-iot-devkit/upload-cert.png)

4. In **.pem of .cer certificaatbestand**, upload het **.pem** certificaat dat u zojuist hebt.

5. Laat de overige als standaard en klik op **opslaan**.

## <a name="start-the-devkit"></a>Start de DevKit

1. Start tegenover Code en seriële monitor openen.

2. Druk op de **opnieuw** knop op uw DevKit.

U ziet de DevKit starten van de registratie met uw Service voor het inrichten van apparaat.

![VS Code-uitvoer](./media/how-to-connect-mxchip-iot-devkit/vscode-output.png)

## <a name="verify-the-devkit-is-registered-on-iot-hub"></a>Controleer of dat de DevKit is geregistreerd in IoT Hub

Eenmaal uw apparaat opnieuw is opgestart, de volgende acties moeten worden uitgevoerd:

1. Het apparaat verzendt een aanvraag voor functieregistratie naar uw apparaat van de inrichtingsservice.
2. De Service voor het inrichten van apparaten vraagt terug registratie waarop het apparaat reageert.
3. Op de registratie is voltooid verzendt de Service voor het inrichten van apparaten de URI van de IoT-hub, apparaat-ID en de versleutelde sleutel terug naar het apparaat.
4. De IoT Hub-clienttoepassing op het apparaat vervolgens verbinding maakt met uw hub.
5. Geslaagde verbinding met de hub ziet u het apparaat weergegeven in de iothub apparaat Verkenner.
  ![Apparaat is geregistreerd](./media/how-to-connect-mxchip-iot-devkit/device-registered.png)

## <a name="change-device-id"></a>Apparaat-ID wijzigen

De geregistreerd in Azure IoT Hub apparaat-ID is **AZ3166**. Als u wijzigen wilt, voert u de [hier instructies](https://microsoft.github.io/azure-iot-developer-kit/docs/customize-device-id/).

## <a name="problems-and-feedback"></a>Problemen en feedback

Als u problemen ondervindt, raadpleegt u [Veelgestelde vragen over](https://microsoft.github.io/azure-iot-developer-kit/docs/faq/) of bereiken aan ons van de volgende kanalen:

* [Gitter.im](http://gitter.im/Microsoft/azure-iot-developer-kit)
* [StackOverflow](https://stackoverflow.com/questions/tagged/iot-devkit)

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd om voor te bereiden de DevKit registreren van een apparaat veilig naar DP's met behulp van OPDELEN, zodat u kunt automatisch registreren met IoT Hub met zonder tussenkomst. In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Globale eindpunt van de DP's configureren op apparaat
> * Unieke apparaat geheim Regulatie gebruikt voor het genereren van het X.509-certificaat
> * Afzonderlijke apparaat inschrijven
> * Controleer of dat het apparaat is geregistreerd

Ga naar de andere zelfstudies voor meer informatie:

> [!div class="nextstepaction"]
> [Maken en inrichten van een gesimuleerd apparaat](./quick-create-simulated-device.md)

