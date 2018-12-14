---
title: Het gebruik van symmetrische sleutels voor het inrichten van verouderde apparaten met de Azure IoT Hub Device Provisioning Service | Microsoft Docs
description: Het gebruik van symmetrische sleutels voor het inrichten van verouderde apparaten met uw device provisioning service-exemplaar
author: wesmc7777
ms.author: wesmc
ms.date: 08/31/2018
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: timlt
ms.openlocfilehash: 9d82ff29b988925f244fc33d7124fe43487895b8
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2018
ms.locfileid: "53341232"
---
# <a name="how-to-provision-legacy-devices-using-symmetric-keys"></a>Over het inrichten van verouderde apparaten met behulp van symmetrische sleutels


Een veelvoorkomend probleem met veel verouderde apparaten is dat ze vaak een identiteit die is samengesteld uit een los stukje van gegevens hebben. Deze gegevens van identiteit is meestal een MAC-adres of een serienummer. Verouderde apparaten mogelijk niet een certificaat, TPM of andere beveiligingsfunctie die kan worden gebruikt om het apparaat veilig kan identificeren. De Device Provisioning Service voor IoT-hub bevat een symmetrische sleutel attest. Attestation-symmetrische sleutel kan worden gebruikt om een apparaat op basis van gegevens, zoals het MAC-adres of een serienummer te identificeren.

Als u eenvoudig kunt installeren een [hardware security module (HSM)](concepts-security.md#hardware-security-module) en een certificaat en vervolgens die mogelijk een betere benadering voor het identificeren en inrichten van uw apparaten. Aangezien deze aanpak kunt u als volgt te omzeilen voor het bijwerken van de code is geïmplementeerd naar al uw apparaten, en u geen een geheime sleutel die is ingesloten in uw installatiekopie van het apparaat.

In dit artikel wordt ervan uitgegaan dat geen van beide een HSM, of een certificaat een beschikbare optie. Echter, wordt ervan uitgegaan dat u een methode hebt voor het bijwerken van apparaatcode voor het gebruik van de Device Provisioning Service voor het inrichten van deze apparaten. 

In dit artikel wordt ook van uitgegaan dat de update vindt plaats in een beveiligde omgeving om te voorkomen dat onbevoegde toegang tot de groepssleutel van de master-of de afgeleide apparaat.

Dit artikel is gericht op een Windows-gebaseerd werkstation. U kunt de procedures echter ook uitvoeren op Linux. Zie [Inrichten voor multitenancy](how-to-provision-multitenant.md) voor een Linux-voorbeeld.


## <a name="overview"></a>Overzicht

Een unieke registratie-ID worden gedefinieerd voor elk apparaat dat is gebaseerd op informatie die wordt aangegeven dat het apparaat. Bijvoorbeeld, het MAC-adres of een serienummer.

Een registratiegroep zit die gebruikmaakt van [symmetrische sleutel attestation](concepts-symmetric-key-attestation.md) wordt gemaakt met de Device Provisioning Service. De registratiegroep bevat een hoofdsleutel voor de groep. Deze master-sleutel wordt gebruikt voor hashing van elke unieke registratie-ID voor het produceren van een unieke apparaat-sleutel voor elk apparaat. Het apparaat gebruikt deze apparaatsleutel afgeleide met de unieke registratie-ID om te bevestigen met de Device Provisioning Service en worden toegewezen aan een IoT-hub.

De apparaatcode gedemonstreerd in dit artikel volgen hetzelfde patroon als het [Quick Start: Een gesimuleerd apparaat inrichten met symmetrische sleutels](quick-create-simulated-device-symm-key.md). De code met een voorbeeld van een apparaat simuleert de [Azure IoT C SDK](https://github.com/Azure/azure-iot-sdk-c). Het gesimuleerde apparaat wordt bevestigen met een registratiegroep in plaats van een afzonderlijke inschrijving, zoals in de Quick Start wordt gedemonstreerd.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]


## <a name="prerequisites"></a>Vereisten

* Voltooiing van de [IoT Hub Device Provisioning Service instellen met de Azure-portal](./quick-setup-auto-provision.md) Quick Start.
* Visual Studio 2015 of [Visual Studio 2017](https://www.visualstudio.com/vs/) met de workload ['Desktopontwikkeling met C++'](https://www.visualstudio.com/vs/support/selecting-workloads-visual-studio-2017/) ingeschakeld.
* Meest recente versie van [Git](https://git-scm.com/download/) geïnstalleerd.


## <a name="prepare-an-azure-iot-c-sdk-development-environment"></a>Een ontwikkelomgeving voorbereiden voor de Azure IoT C-SDK

In deze sectie bereidt u een ontwikkelomgeving voor die wordt gebruikt om de [Azure IoT C-SDK](https://github.com/Azure/azure-iot-sdk-c) te bouwen. 

De SDK bevat de voorbeeldcode voor het gesimuleerde apparaat. Dit gesimuleerde apparaat probeert de inrichting uit te voeren tijdens de opstartprocedure van het apparaat.

1. Download versie 3.11.4 van het [CMake-buildsysteem](https://cmake.org/download/). Controleer het gedownloade binaire bestand met behulp van de bijbehorende cryptografische hash-waarde. In het volgende voorbeeld is Windows PowerShell gebruikt om de cryptografische hash te controleren voor versie 3.11.4 van de x64 MSI-distributie:

    ```PowerShell
    PS C:\Downloads> $hash = get-filehash .\cmake-3.11.4-win64-x64.msi
    PS C:\Downloads> $hash.Hash -eq "56e3605b8e49cd446f3487da88fcc38cb9c3e9e99a20f5d4bd63e54b7a35f869"
    True
    ```
    
    De volgende hash-waarden voor versie 3.11.4 werden vermeld op de CMake-site ten tijde van dit schrijven:

    ```
    6dab016a6b82082b8bcd0f4d1e53418d6372015dd983d29367b9153f1a376435  cmake-3.11.4-Linux-x86_64.tar.gz
    72b3b82b6d2c2f3a375c0d2799c01819df8669dc55694c8b8daaf6232e873725  cmake-3.11.4-win32-x86.msi
    56e3605b8e49cd446f3487da88fcc38cb9c3e9e99a20f5d4bd63e54b7a35f869  cmake-3.11.4-win64-x64.msi
    ```

    Het is belangrijk dat de vereisten voor Visual Studio met (Visual Studio en de workload Desktopontwikkeling met C++) op uw computer zijn geïnstalleerd **voordat** de `CMake`-installatie wordt gestart. Zodra aan de vereisten is voldaan en de download is geverifieerd, installeert u het CMake-bouwsysteem.

2. Open een opdrachtprompt of Git Bash-shell. Voer de volgende opdracht uit voor het klonen van de GitHub-opslagplaats voor de Azure IoT C-SDK:
    
    ```cmd/sh
    git clone https://github.com/Azure/azure-iot-sdk-c.git --recursive
    ```
    De grootte van deze opslagplaats is momenteel ongeveer 220 MB. Deze bewerking kan enkele minuten in beslag nemen.


3. Maak de submap `cmake` in de hoofdmap van de Git-opslagplaats en navigeer naar die map. 

    ```cmd/sh
    cd azure-iot-sdk-c
    mkdir cmake
    cd cmake
    ```

4. Voer de volgende opdracht uit om een versie van de SDK te bouwen die specifiek is voor uw clientplatform voor ontwikkeling. Er wordt een Visual Studio-oplossing voor het gesimuleerde apparaat gegenereerd in de map `cmake`. 

    ```cmd
    cmake -Dhsm_type_symm_key:BOOL=ON -Duse_prov_client:BOOL=ON  ..
    ```
    
    Als `cmake` uw C++-compiler niet kan vinden, kunnen er fouten in de build optreden tijdens het uitvoeren van de bovenstaande opdracht. Als dit gebeurt, voert u deze opdracht uit bij de [Visual Studio-opdrachtprompt](https://docs.microsoft.com/dotnet/framework/tools/developer-command-prompt-for-vs). 

    Zodra het bouwen is voltooid, zijn de laatste paar uitvoerregels vergelijkbaar met de volgende uitvoer:

    ```cmd/sh
    $ cmake -Dhsm_type_symm_key:BOOL=ON -Duse_prov_client:BOOL=ON  ..
    -- Building for: Visual Studio 15 2017
    -- Selecting Windows SDK version 10.0.16299.0 to target Windows 10.0.17134.
    -- The C compiler identification is MSVC 19.12.25835.0
    -- The CXX compiler identification is MSVC 19.12.25835.0

    ...

    -- Configuring done
    -- Generating done
    -- Build files have been written to: E:/IoT Testing/azure-iot-sdk-c/cmake
    ```


## <a name="create-a-symmetric-key-enrollment-group"></a>Een symmetrische sleutel registratiegroep maken

1. Aanmelden bij de [Azure-portal](http://portal.azure.com), en open uw Device Provisioning Service-exemplaar.

2. Selecteer de **registraties beheren** tabblad en klik vervolgens op de **toevoegen registratiegroep** knop aan de bovenkant van de pagina. 

3. Op **Registratiegroep toevoegen**, voer de volgende informatie en klik op de **opslaan** knop.

    - **Naam van groep**: Voer **mylegacydevices**.

    - **Type Attestation**: Selecteer **symmetrische sleutel**.

    - **Automatisch sleutels genereren**: Schakel dit selectievakje in.

    - **Selecteer de gewenste apparaten toewijzen aan hubs**: Selecteer **statische configuratie** , zodat u aan een specifieke hub toewijzen kunt.

    - **Selecteer de IoT-hubs deze groep kan worden toegewezen aan**: Selecteer een van uw hubs.

    ![Registratiegroep voor attestation-symmetrische sleutel toevoegen](./media/how-to-legacy-device-symm-key/symm-key-enrollment-group.png)

4. Zodra u uw inschrijving hebt opgeslagen, worden de **primaire sleutel** en **secundaire sleutel** gegenereerd en aan de inschrijvingsvermelding toegevoegd. De symmetrische sleutel inschrijvingsgroep wordt weergegeven als **mylegacydevices** onder de *groepsnaam* kolom in de *Registratiegroepen* tabblad. 

    Open de inschrijving en kopieer de waarde van uw gegenereerde **primaire sleutel**. Deze sleutel is de groepssleutel van uw hoofd.


## <a name="choose-a-unique-registration-id-for-the-device"></a>Kies een unieke registratie-ID voor het apparaat

Een unieke registratie-ID moet worden gedefinieerd voor het identificeren van elk apparaat. U kunt het MAC-adres, serienummer of unieke gegevens van het apparaat. 

In dit voorbeeld gebruiken we een combinatie van een MAC-adres en het serienummer van de volgende tekenreeks vormt voor een registratie-ID.

```
sn-007-888-abc-mac-a1-b2-c3-d4-e5-f6
```

Maak een unieke registratie-ID voor uw apparaat. Geldige tekens zijn alfanumerieke kleine letters en streepjes ('-').


## <a name="derive-a-device-key"></a>De apparaatsleutel van een afleiden 

Voor het genereren van de sleutel van het apparaat, gebruikt u de hoofdsleutel van de groep voor het berekenen van een [HMAC-SHA256](https://wikipedia.org/wiki/HMAC) van de unieke registratie-ID voor het apparaat en het resultaat in Base 64-indeling converteren.

Voegt de hoofdsleutel van de groep toe aan de apparaatcode van uw.


#### <a name="linux-workstations"></a>Linux-werkstations

Als u een Linux-werkstation gebruikt, kunt u openssl gebruiken voor het genereren van de afgeleide apparaatsleutel zoals wordt weergegeven in het volgende voorbeeld.

Vervang de waarde van **sleutel** met de **primaire sleutel** u eerder hebt genoteerd.

Vervang de waarde van **REG_ID** met uw registratie-ID.

```bash
KEY=8isrFI1sGsIlvvFSSFRiMfCNzv21fjbE/+ah/lSh3lF8e2YG1Te7w1KpZhJFFXJrqYKi9yegxkqIChbqOS9Egw==
REG_ID=sn-007-888-abc-mac-a1-b2-c3-d4-e5-f6

keybytes=$(echo $KEY | base64 --decode | xxd -p -u -c 1000)
echo -n $REG_ID | openssl sha256 -mac HMAC -macopt hexkey:$keybytes -binary | base64
```

```bash
Jsm0lyGpjaVYVP2g3FnmnmG9dI/9qU24wNoykUmermc=
```


#### <a name="windows-based-workstations"></a>Windows-werkstations

Als u een Windows-werkstation gebruikt, kunt u PowerShell gebruiken voor het genereren van de afgeleide apparaatsleutel zoals wordt weergegeven in het volgende voorbeeld.

Vervang de waarde van **sleutel** met de **primaire sleutel** u eerder hebt genoteerd.

Vervang de waarde van **REG_ID** met uw registratie-ID.

```PowerShell
$KEY='8isrFI1sGsIlvvFSSFRiMfCNzv21fjbE/+ah/lSh3lF8e2YG1Te7w1KpZhJFFXJrqYKi9yegxkqIChbqOS9Egw=='
$REG_ID='sn-007-888-abc-mac-a1-b2-c3-d4-e5-f6'

$hmacsha256 = New-Object System.Security.Cryptography.HMACSHA256
$hmacsha256.key = [Convert]::FromBase64String($KEY)
$sig = $hmacsha256.ComputeHash([Text.Encoding]::ASCII.GetBytes($REG_ID))
$derivedkey = [Convert]::ToBase64String($sig)
echo "`n$derivedkey`n"
```

```PowerShell
Jsm0lyGpjaVYVP2g3FnmnmG9dI/9qU24wNoykUmermc=
```


Uw apparaat met uw unieke registratie-ID om uit te voeren van de attestation-symmetrische sleutel met de registratiegroep tijdens het inrichten van de sleutel van de afgeleide apparaat gebruikt.



## <a name="create-a-device-image-to-provision"></a>Maken van een installatiekopie van het apparaat inrichten

In deze sectie maakt u een voorbeeld van een inrichten met de naam bijwerken **prov\_dev\_client\_voorbeeld** zich in de Azure IoT C SDK u eerder hebt ingesteld. 

Deze voorbeeldcode simuleert de opstartvolgorde van een apparaat waarmee de aanvraag voor inrichting worden verzonden naar uw Device Provisioning Service-exemplaar. De opstartvolgorde zorgt ervoor dat het apparaat kan worden herkend en toegewezen aan de IoT-hub die u hebt geconfigureerd op de registratiegroep.

1. Selecteer in Azure Portal het tabblad **Overzicht** voor uw Device Provisioning-service en noteer de waarde van het **_Id-bereik_**.

    ![Device Provisioning Service-eindpuntgegevens uit de portalblade extraheren](./media/quick-create-simulated-device-x509/extract-dps-endpoints.png) 

2. Open in Visual Studio, de **azure_iot_sdks.sln** oplossingsbestand dat is gegenereerd door eerdere CMake uitgevoerd. Het oplossingsbestand bevindt zich als het goed is op de volgende locatie:

    ```
    \azure-iot-sdk-c\cmake\azure_iot_sdks.sln
    ```

3. Navigeer in het deelvenster *Solution Explorer* van Visual Studio naar de map **Provision\_Samples**. Vouw het voorbeeldproject met de naam **prov\_dev\_client\_sample** uit. Vouw **Source Files** uit en open **prov\_dev\_client\_sample.c**.

4. Zoek de constante `id_scope` op en vervang de waarde door uw **Id-bereik**-waarde die u eerder hebt gekopieerd. 

    ```c
    static const char* id_scope = "0ne00002193";
    ```

5. Zoek de definitie voor de functie `main()` op in hetzelfde bestand. Zorg ervoor dat de variabele `hsm_type` is ingesteld op `SECURE_DEVICE_TYPE_SYMMETRIC_KEY`, zoals hieronder wordt weergegeven:

    ```c
    SECURE_DEVICE_TYPE hsm_type;
    //hsm_type = SECURE_DEVICE_TYPE_TPM;
    //hsm_type = SECURE_DEVICE_TYPE_X509;
    hsm_type = SECURE_DEVICE_TYPE_SYMMETRIC_KEY;
    ```

6. De aanroep naar `prov_dev_set_symmetric_key_info()` in **prov\_dev\_client\_sample.c** die is opgenomen als opmerking.

    ```c
    // Set the symmetric key if using they auth type
    //prov_dev_set_symmetric_key_info("<symm_registration_id>", "<symmetric_Key>");
    ```

    Verwijder opmerkingen bij de aanroep van de functie en vervang de tijdelijke aanduiding voor waarden (inclusief de punthaken) door de unieke registratie-ID voor uw apparaat en de afgeleide apparaatsleutel die u hebt gegenereerd.

    ```c
    // Set the symmetric key if using they auth type
    prov_dev_set_symmetric_key_info("sn-007-888-abc-mac-a1-b2-c3-d4-e5-f6", "Jsm0lyGpjaVYVP2g3FnmnmG9dI/9qU24wNoykUmermc=");
    ```
   
    Sla het bestand op.

7. Klik met de rechtermuisknop op het **prov\_dev\_client\_sample**-project en selecteer **Set as Startup Project**. 

8. Selecteer in het menu van Visual Studio de optie **Debug** > **Start without debugging** om de oplossing uit te voeren. Klik in de prompt om het project opnieuw te bouwen op **Yes** om het project opnieuw te bouwen voordat het wordt uitgevoerd.

    De volgende output is een voorbeeld waarbij het gesimuleerde apparaat met succes opstart en verbinding maakt met het inrichtingsservice-exemplaar voor toewijzing aan een IoT-hub:

    ```cmd
    Provisioning API Version: 1.2.8

    Registering Device

    Provisioning Status: PROV_DEVICE_REG_STATUS_CONNECTED
    Provisioning Status: PROV_DEVICE_REG_STATUS_ASSIGNING
    Provisioning Status: PROV_DEVICE_REG_STATUS_ASSIGNING

    Registration Information received from service: 
    test-docs-hub.azure-devices.net, deviceId: sn-007-888-abc-mac-a1-b2-c3-d4-e5-f6

    Press enter key to exit:
    ```

9. Navigeer in de portal naar de IoT-hub waaraan uw gesimuleerd apparaat is toegewezen, en klik op het tabblad **IoT-apparaten**. Wanneer de inrichting van het gesimuleerde apparaat voor de hub is geslaagd, wordt de betreffende apparaat-ID weergegeven op de blade **IoT-apparaten** met de *STATUS* op **ingeschakeld**. Mogelijk moet u bovenaan op de knop **Vernieuwen** klikken. 

    ![Apparaat wordt geregistreerd voor de IoT-hub](./media/how-to-legacy-device-symm-key/hub-registration.png) 



## <a name="security-concerns"></a>Zorgen over de beveiliging

Let erop dat dit de afgeleide apparaatsleutel opgenomen als onderdeel van de installatiekopie, die niet een aanbevolen beveiligingsprocedure verlaat. Dit is een van de redenen waarom nadelen beveiligings- en eenvoudig te gebruiken zijn. 





## <a name="next-steps"></a>Volgende stappen

* Zie voor meer meer Reprovisioning [reprovisoning concepten van IoT Hub-apparaat](concepts-device-reprovision.md) 
* [Snelstartgids: Een gesimuleerd apparaat inrichten met symmetrische sleutels](quick-create-simulated-device-symm-key.md)
* Zie voor meer meer opheffen van inrichting [hoe u de inrichting van apparaten die zijn eerder automatisch ingericht ](how-to-unprovision-devices.md) 











