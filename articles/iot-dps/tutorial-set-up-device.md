---
title: Apparaat instellen voor Azure IoT Hub Device Provisioning Service
description: Apparaat instellen om in te richten via IoT Hub Device Provisioning Service tijdens het fabriceren van het apparaat
services: iot-dps
keywords: ''
author: dsk-2015
ms.author: dkshir
ms.date: 04/02/2018
ms.topic: tutorial
ms.service: iot-dps
documentationcenter: ''
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: c885e4d5d747d913eaf0b7137b240950e920e7ff
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2018
---
# <a name="set-up-a-device-to-provision-using-the-azure-iot-hub-device-provisioning-service"></a>Een apparaat instellen om in te richten met behulp van IoT Hub Device Provisioning Service

In de vorige zelfstudie hebt u geleerd hoe u Azure IoT Hub Device Provisioning Service kunt instellen om apparaten automatisch in te richten in uw IoT-hub. In deze zelfstudie ziet u hoe u uw apparaat kunt instellen tijdens het fabriceren van het apparaat, en ervoor zorgt dat het automatisch kan worden ingericht met IoT Hub. Uw apparaat wordt ingericht op basis van het bijbehorende [Attestation-mechanisme](concepts-device.md#attestation-mechanism) na de eerste keer opstarten en verbinden met de inrichtingsservice. In deze zelfstudie worden de volgende processen besproken:

> [!div class="checklist"]
> * Platformspecifieke SDK van de Device Provisioning Service-client bouwen
> * De beveiligingsartefacten extraheren
> * De software voor apparaatregistratie maken

## <a name="prerequisites"></a>Vereisten

Voordat u doorgaat, maakt u een Device Provisioning Service-instantie en een IoT-hub, met behulp van de instructies in de vorige zelfstudie [1 - Cloudresources instellen](./tutorial-set-up-cloud.md).

In deze zelfstudie wordt gebruikgemaakt van de [Azure IoT SDKs and libraries for C](https://github.com/Azure/azure-iot-sdk-c)-opslagplaats (Azure IoT SDK’s en bibliotheken voor C), die de SDK van de Device Provisioning Service-client voor C bevat. De SDK biedt momenteel ondersteuning voor TPM en X.509 voor apparaten die worden uitgevoerd in Windows- of Ubuntu-implementaties. Deze zelfstudie is gebaseerd op het gebruik van een Windows-ontwikkelclient. Er wordt vanuit gegaan dat u de basisprincipes van het werken met Visual Studio 2017 onder de knie hebt. 

Als u niet bekend bent met het proces van automatische inrichting, bekijkt u de [Concepten voor automatische inrichting](concepts-auto-provisioning.md) voordat u verdergaat. 

## <a name="build-a-platform-specific-version-of-the-sdk"></a>Een platformspecifieke versie van de SDK bouwen

De SDK van de Device Provisioning Service-client helpt u bij het implementeren van de software voor apparaatregistratie. Maar voordat u deze kunt gebruiken, moet u een versie van de SDK bouwen die specifiek is voor uw ontwikkelclientplatform en attestation- mechanisme. In deze zelfstudie bouwt u een SDK die gebruikmaakt van Visual Studio 2017 op een Windows-ontwikkelplatform, voor een ondersteund type attestation:

1. Installeer de vereiste hulpprogramma’s en kloon de GitHub-opslagplaats die de Client SDK voor C voor de inrichtingsservice bevat:

   a. Op uw computer moet Visual Studio 2015 of [Visual Studio 2017](https://www.visualstudio.com/vs/) zijn geïnstalleerd. De workload ['Desktop development with C++'](https://www.visualstudio.com/vs/support/selecting-workloads-visual-studio-2017/) (Bureaubladontwikkeling met C++) moet zijn ingeschakeld voor uw installatie van Visual Studio.

   b. Download en installeer het [CMake-bouwsysteem](https://cmake.org/download/). Het is belangrijk dat Visual Studio met 'Desktop development with C++'-werkbelasting (Bureaubladontwikkeling met C++) op de computer wordt geïnstalleerd **vóór** de CMake-installatie.

   c. Zorg ervoor dat `git` op de computer wordt geïnstalleerd en toegevoegd aan de omgevingsvariabelen die voor het opdrachtvenster toegankelijk zijn. Zie [Hulpprogramma’s voor de Git-client van Software Freedom Conservancy](https://git-scm.com/download/) voor de nieuwste hulpprogramma’s voor `git`, inclusief **Git Bash**, een opdrachtregel Bash-shell voor interactie met de lokale Git-opslagplaats. 

   d. Open Git Bash en kloon de ‘Azure IoT SDKs and libraries for C’-opslagplaats (Azure IoT SDK’s en bibliotheken voor C). Het kan enige tijd duren voordat de kloonopdracht is voltooid, omdat ook verschillende afhankelijke submodules worden gedownload:
    
   ```cmd/sh
   git clone https://github.com/Azure/azure-iot-sdk-c.git --recursive
   ```

   e. Een nieuwe `cmake`-submap maken in de submap van de nieuwe opslagplaats:

   ```cmd/sh
   mkdir azure-iot-sdk-c/cmake
   ``` 

2. Ga vanuit de Git Bash-opdrachtprompt naar de `cmake`-submap van de azure-iot-sdk-c-opslagplaats:

   ```cmd/sh
   cd azure-iot-sdk-c/cmake
   ```

3. Bouw de SDK voor het ontwikkelplatform en een van de ondersteunde attestation-mechanismen met behulp van een van de volgende opdrachten (let op: eindigt op twee puntjes). Na het voltooien wordt de `/cmake`-submap met CMake uitgebreid met inhoud die specifiek is voor uw apparaat:
    - Voor apparaten die gebruikmaken van een fysieke TPM/HSM, of een gesimuleerd X.509-certificaat voor attestation:
        ```cmd/sh
        cmake -Duse_prov_client:BOOL=ON ..
        ```

    - Voor apparaten die gebruikmaken van de TPM-simulator voor attestation:
        ```cmd/sh
        cmake -Duse_prov_client:BOOL=ON -Duse_tpm_simulator:BOOL=ON ..
        ```

Nu bent u klaar om de SDK te gebruiken om de registratiecode voor het apparaat te bouwen. 
 
<a id="extractsecurity"></a> 

## <a name="extract-the-security-artifacts"></a>De beveiligingsartefacten extraheren 

De volgende stap is het extraheren van de beveiligingsartefacten voor het attestation-mechanisme dat wordt gebruikt op het apparaat. 

### <a name="physical-device"></a>Fysiek apparaat 

Als u de SDK hebt gebouwd om attestation te gebruiken vanaf een fysieke TPM/HSM :

- Voor een TPM-apparaat moet u bij de TPM-chipfabrikant de bijbehorende **Goedkeuringssleutel** achterhalen. U kunt een unieke **Registratie-id** voor het TPM-apparaat afleiden door de goedkeuringssleutel te hashen.  

- Voor een X.509-apparaat moet u de certificaten verkrijgen die zijn verleend aan uw apparaat/apparaten: eindentiteitcertificaten voor afzonderlijke apparaatinschrijvingen, en hoofdcertificaten voor groepsinschrijvingen van apparaten. 

### <a name="simulated-device"></a>Gesimuleerd apparaat

Als u de SDK hebt gebouwd om attestation te gebruiken vanaf een gesimuleerd TPM-certificaat of een gesimuleerd X.509-certificaat:

- Voor een gesimuleerd TPM-apparaat:
   1. Ga in een afzonderlijke/nieuwe opdrachtprompt naar de `azure-iot-sdk-c`-submap en voer de TPM-simulator uit. Deze luistert via een socket op poorten 2321 en 2322. Sluit dit opdrachtvenster niet. De simulator moet actief blijven tot het einde van de volgende snelstart. 

      Voer vanuit de `azure-iot-sdk-c`-submap de volgende opdracht uit om de simulator te starten:

      ```cmd/sh
      .\provisioning_client\deps\utpm\tools\tpm_simulator\Simulator.exe
      ```

   2. Open in Visual Studio de oplossing die is gegenereerd in de *cmake*-map met de naam `azure_iot_sdks.sln`, en bouw deze met behulp van de opdracht Build solution in het menu Build.

   3. In het deelvenster *Solution Explorer* van Visual Studio gaat u naar de map **Inrichten\_Extra**. Klik met de rechtermuisknop op het **tpm_device_provision**-project en selecteer **Set as Startup Project**. 

   4. Voer de oplossing uit met behulp van een van de twee opdrachten Start in het menu Debug. In het uitvoervenster worden de voor apparaatinschrijving en -registratie vereiste **_Registratie-id_** en **_Goedkeuringssleutel_** van de TPM-simulator weergegeven. Kopieer deze waarden voor later gebruik. U kunt dit venster (met de registratie-id en goedkeuringssleutel) sluiten, maar laat het venster voor de TPM-simulator uit stap 1 geopend.

- Voor een gesimuleerd X.509-apparaat:
  1. Open in Visual Studio de oplossing die is gegenereerd in de *cmake*-map met de naam `azure_iot_sdks.sln`, en bouw deze met behulp van de opdracht Build solution in het menu Build.

  2. In het deelvenster *Solution Explorer* van Visual Studio gaat u naar de map **Inrichten\_Extra**. Klik met de rechtermuisknop op het**dice\_device\_enrollment**-project en selecteer **Set as Startup Project**. 
  
  3. Voer de oplossing uit met behulp van een van de twee opdrachten Start in het menu Debug. Voer in het uitvoervenster **i** in voor individuele registratie wanneer hierom wordt gevraagd. In het uitvoervenster wordt een lokaal gegenereerd X.509-certificaat weergegeven voor uw gesimuleerde apparaat. Kopieer de uitvoer naar Klembord vanaf *-----BEGIN CERTIFICATE-----* tot en met de eerste *-----END CERTIFICATE-----*, en zorg ervoor dat deze beide regels ook zijn opgenomen. U hebt alleen het eerste certificaat uit het uitvoervenster nodig.
 
  4. Maak een bestand met de naam **_X509testcert.pem_**, open het in een teksteditor naar keuze en kopieer de inhoud van het Klembord naar dit bestand. Sla het bestand op. U gebruikt dit bestand later voor apparaatinschrijving. Wanneer de registratiesoftware wordt uitgevoerd, maakt deze gebruik van hetzelfde certificaat tijdens de automatische inrichting.    

Deze beveiligingsartefacten zijn vereist tijdens de inschrijving van het apparaat voor Device Provisioning Service. De inrichtingsservice wacht tot het apparaat op enig moment later is opgestart en verbonden met de service. Als het apparaat voor de eerste keer wordt opgestart, communiceert de Client SDK-logica met de chip (of simulator) om de beveiligingsartefacten te extraheren uit het apparaat, en wordt de registratie bij Device Provisioning Service geverifieerd. 

## <a name="create-the-device-registration-software"></a>De software voor apparaatregistratie maken

De laatste stap is het schrijven van een registratietoepassing die gebruikmaakt van de SDK van de Device Provisioning Service-client om het apparaat te registreren bij de IoT Hub-service. 

> [!NOTE]
> Voor deze stap wordt ervan uitgegaan dat u een gesimuleerd apparaat gebruikt, gerealiseerd door het uitvoeren van een SDK-voorbeeldtoepassing voor registratie vanuit uw werkstation. Dezelfde concepten zijn echter van toepassing als u een registratietoepassing bouwt die moet worden geïmplementeerd op een fysiek apparaat. 

1. Selecteer in Azure Portal de blade **Overzicht** voor Device Provisioning Service en kopieer de waarde bij **_Id-bereik_**. Het *Id-bereik* wordt gegenereerd met de service en is gegarandeerd uniek. Het is onveranderbaar en wordt gebruikt als een unieke identificatie van de registratie-id's.

    ![DPS-eindpuntgegevens uit de portalblade extraheren](./media/tutorial-set-up-device/extract-dps-endpoints.png) 

2. In *Solution Explorer* van Visual Studio op uw computer gaat u naar de map **Inrichten\_Voorbeelden**. Selecteer het voorbeeldproject met de naam **prov\_dev\_client\_sample** en open het bronbestand **prov\_dev\_client\_sample.c**.

3. Wijs de waarde _Id-bereik_ die u hebt verkregen in stap 1, toe aan de variabele `id_scope` (verwijder de haakjes links/`[` en rechts/`]`): 

    ```c
    static const char* global_prov_uri = "global.azure-devices-provisioning.net";
    static const char* id_scope = "[ID Scope]";
    ```

    Ter informatie, de variabele `global_prov_uri`, waarmee de IoT Hub API voor clientregistratie `IoTHubClient_LL_CreateFromDeviceAuth` verbinding kan maken met de toegewezen Device Provisioning Service-instantie.

4. In de functie **main()** in hetzelfde bestand, voegt u een opmerking toe aan of verwijdert u een opmerking bij de variabele `hsm_type` die overeenkomt met het attestation-mechanisme dat wordt gebruikt voor de registratiesoftware van het apparaat (TPM of X.509): 

    ```c
    hsm_type = SECURE_DEVICE_TYPE_TPM;
    //hsm_type = SECURE_DEVICE_TYPE_X509;
    ```

5. Sla de wijzigingen op en bouw het voorbeeld **prov\_dev\_client\_sample** opnieuw door Build solution te selecteren in het menu Build. 

6. Klik met de rechtermuisknop op het project **prov\_dev\_client\_sample** project onder de map **Provision\_Samples**, en selecteer **Set as Startup Project**. Voer de voorbeeldtoepassing nog NIET uit.

> [!IMPORTANT]
> Start het apparaat nog niet! U moet het proces voltooien door het apparaat eerst in te schrijven bij Device Provisioning Service, voordat u het apparaat start. In de sectie Volgende stappen hieronder vindt u het volgende artikel.

### <a name="sdk-apis-used-during-registration-for-reference-only"></a>SDK API's die worden gebruikt tijdens de registratie (alleen ter informatie)

Ter informatie, de SDK biedt de volgende API's die de toepassing kan gebruiken tijdens de registratie. Deze API’s helpen om het apparaat te verbinden met en te registreren bij Device Provisioning Service wanneer het wordt gestart. Het apparaat ontvangt vervolgens de informatie die is vereist om een verbinding tot stand te brengen met de IoT Hub-instantie:

```C
// Creates a Provisioning Client for communications with the Device Provisioning Client Service.  
PROV_DEVICE_LL_HANDLE Prov_Device_LL_Create(const char* uri, const char* scope_id, PROV_DEVICE_TRANSPORT_PROVIDER_FUNCTION protocol)

// Disposes of resources allocated by the provisioning Client.
void Prov_Device_LL_Destroy(PROV_DEVICE_LL_HANDLE handle)

// Asynchronous call initiates the registration of a device.
PROV_DEVICE_RESULT Prov_Device_LL_Register_Device(PROV_DEVICE_LL_HANDLE handle, PROV_DEVICE_CLIENT_REGISTER_DEVICE_CALLBACK register_callback, void* user_context, PROV_DEVICE_CLIENT_REGISTER_STATUS_CALLBACK reg_status_cb, void* status_user_ctext)

// Api to be called by user when work (registering device) can be done
void Prov_Device_LL_DoWork(PROV_DEVICE_LL_HANDLE handle)

// API sets a runtime option identified by parameter optionName to a value pointed to by value
PROV_DEVICE_RESULT Prov_Device_LL_SetOption(PROV_DEVICE_LL_HANDLE handle, const char* optionName, const void* value)
```

Mogelijk moet u ook de Device Provisioning Service-clientoepassing voor registratie verfijnen, aanvankelijk met behulp van een gesimuleerd apparaat, en een testservice-installatie. Als de toepassing werkt in de testomgeving, kunt u het bouwen voor uw specifieke apparaat en het uitvoerbare bestand kopiëren naar de installatiekopie van het apparaat. 

## <a name="clean-up-resources"></a>Resources opschonen

Op dit punt worden Device Provisioning Service en IoT Hub mogelijk uitgevoerd in de portal. Als u de installatie van apparaatinrichting wilt afbreken, en/of de afronding van deze reeks zelfstudies wilt uitstellen, raden we u aan ze uit te schakelen om onnodige kosten te vermijden.

1. Klik in het linkermenu in de Azure Portal op **Alle resources** en selecteer uw Device Provisioning Service. Klik bovenaan de blade **Alle resources** op **Verwijderen**.  
2. Klik in het linkermenu in de Azure Portal op **Alle resources** en selecteer vervolgens uw IoT-hub. Klik bovenaan de blade **Alle resources** op **Verwijderen**.  

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Platformspecifieke SDK van de Device Provisioning Service-client bouwen
> * De beveiligingsartefacten extraheren
> * De software voor apparaatregistratie maken

Ga verder met de volgende zelfstudie voor informatie over het inrichten van het apparaat in de IoT-hub door het te registreren bij Azure IoT Hub Device Provisioning Service voor automatische inrichting.

> [!div class="nextstepaction"]
> [Het apparaat inrichten in de IoT-hub](tutorial-provision-device-to-hub.md)

