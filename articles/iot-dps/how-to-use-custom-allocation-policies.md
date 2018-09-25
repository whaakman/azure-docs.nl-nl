---
title: Aangepaste toewijzingsbeleid voor gebruiken met de Azure IoT Hub Device Provisioning Service | Microsoft Docs
description: Aangepaste toewijzingsbeleid voor gebruiken met de Azure IoT Hub Device Provisioning Service
author: wesmc7777
ms.author: wesmc
ms.date: 08/15/2018
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: timlt
ms.openlocfilehash: 503a8026fe11d1cdb3d0fc0c2680d8d545a1c992
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46955236"
---
# <a name="how-to-use-custom-allocation-policies"></a>Het gebruik van aangepaste toewijzingsbeleid


Een aangepaste toewijzingsbeleid hebt u meer controle over hoe apparaten worden toegewezen aan een IoT-hub. Dit wordt gerealiseerd met behulp van aangepaste code in een [Azure Function](../azure-functions/functions-overview.md) apparaten toewijzen aan een IoT-hub. De service voor apparaatinrichting roept uw Azure-functie code voor het leveren van de IoT hub-groep. Uw functiecode retourneert de IoT hub-informatie voor het inrichten van het apparaat.

Met behulp van aangepaste toewijzingsbeleid definieert u uw eigen toewijzingsbeleid wanneer het beleid dat is geleverd door de Device Provisioning Service niet voldoen aan de vereisten van uw scenario.

Bijvoorbeeld, misschien u wilt bekijken van het certificaat dat een apparaat wordt gebruikt tijdens het inrichten en toewijzen van het apparaat naar een IoT-hub op basis van de certificaateigenschap van een. U kunt mogelijk die zijn opgeslagen in een database voor uw apparaten hebt en wilt zoeken in de database om te bepalen welke IoT-hub een apparaat moet worden toegewezen aan.


In dit artikel ziet u een aangepaste toewijzingsbeleid met behulp van een Azure-functie die is geschreven in C#. Twee nieuwe IoT-hubs worden gemaakt voor een *Contoso broodroosters deling* en een *Contoso Heatmap pompen deling*. Apparaten inrichten aanvragen moeten een registratie-ID met een van de volgende achtervoegsels moeten worden geaccepteerd voor het inrichten van hebben:

- **-contoso-tstrsd-007**: Contoso broodroosters deling
- **-contoso-hpsd-088**: Contoso Heatmap pompen deling

De apparaten worden ingericht op basis van een van deze vereiste achtervoegsels op de registratie-ID. Deze apparaten worden gesimuleerd met behulp van een inrichting voorbeeld opgenomen in de [Azure IoT C SDK](https://github.com/Azure/azure-iot-sdk-c). 

U wordt de volgende stappen uitvoeren in dit artikel:

* De Azure CLI gebruiken om twee Contoso-deling IoT hubs te maken (**Contoso broodroosters deling** en **Contoso Heatmap pompen deling**)
* Maken van een nieuwe groepsinschrijving met behulp van een Azure-functie voor het aangepaste toewijzingsbeleid
* Apparaatsleutels voor twee apparaat simulaties maken.
* De ontwikkelomgeving instellen voor de Azure IoT C SDK
* De apparaten om te zien dat deze zijn ingericht op basis van de voorbeeldcode van de aangepaste toewijzingsbeleid simuleren


[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Vereisten

* Voltooiing van de [IoT Hub Device Provisioning Service instellen met de Azure-portal](./quick-setup-auto-provision.md) Quick Start.
* Visual Studio 2015 of [Visual Studio 2017](https://www.visualstudio.com/vs/) met de workload ['Desktopontwikkeling met C++'](https://www.visualstudio.com/vs/support/selecting-workloads-visual-studio-2017/) ingeschakeld.
* Meest recente versie van [Git](https://git-scm.com/download/) geïnstalleerd.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="create-two-divisional-iot-hubs"></a>Twee divisie IoT-hubs maken

In deze sectie gebruikt u de Azure Cloud Shell te maken van twee nieuwe IoT-hubs die de **Contoso broodroosters deling** en de **Contoso Heatmap pompen deling**.

1. De Azure Cloud Shell gebruiken om te maken van een resourcegroep met de [az-groep maken](/cli/azure/group#az-group-create) opdracht. Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. 

    Het volgende voorbeeld wordt een resourcegroep met de naam *contoso-us-resource-group* in de *eastus* regio. Het verdient aanbeveling gebruik te maken van deze groep voor alle resources die in dit artikel is gemaakt. Deze aanpak wordt eenvoudiger opschonen nadat u klaar bent.

    ```azurecli-interactive 
    az group create --name contoso-us-resource-group --location eastus
    ```

2. De Azure Cloud Shell gebruiken om te maken de **Contoso broodroosters deling** IoT-hub met de [az iot hub maken](/cli/azure/iot/hub#az-iot-hub-create) opdracht. De IoT-hub worden toegevoegd aan *contoso-us-resource-group*.

    Het volgende voorbeeld wordt een IoT-hub met de naam *contoso-broodroosters-hub-1098* in de *eastus* locatie. U moet de hubnaam van uw eigen unieke. Uw eigen-achtervoegsel in de naam van de hub in plaats van gezamenlijk **1098**. De voorbeeldcode voor het aangepaste toewijzingsbeleid vereist `-toasters-` in naam van de hub.

    ```azurecli-interactive 
    az iot hub create --name contoso-toasters-hub-1098 --resource-group contoso-us-resource-group --location eastus --sku S1
    ```
    
    Met deze opdracht kan enkele minuten duren.

3. De Azure Cloud Shell gebruiken om te maken de **Contoso Heatmap pompen deling** IoT-hub met de [az iot hub maken](/cli/azure/iot/hub#az-iot-hub-create) opdracht. Deze IoT hub wordt ook toegevoegd aan *contoso-us-resource-group*.

    Het volgende voorbeeld wordt een IoT-hub met de naam *contoso-heatpumps-hub-1098* in de *eastus* locatie. U moet de hubnaam van uw eigen unieke. Uw eigen-achtervoegsel in de naam van de hub in plaats van gezamenlijk **1098**. De voorbeeldcode voor het aangepaste toewijzingsbeleid vereist `-heatpumps-` in naam van de hub.

    ```azurecli-interactive 
    az iot hub create --name contoso-heatpumps-hub-1098 --resource-group contoso-us-resource-group --location eastus --sku S1
    ```
    
    Met deze opdracht kan ook een paar minuten duren.




## <a name="create-the-enrollment"></a>Maken van de inschrijving

In deze sectie maakt u een nieuwe registratiegroep die gebruikmaakt van de aangepaste toewijzingsbeleid. Voor het gemak in dit artikel wordt [symmetrische sleutel attestation](concepts-symmetric-key-attestation.md) bij de inschrijving. Overweeg het gebruik van een veiligere oplossing [x.509-certificaat attestation](concepts-security.md#x509-certificates) met een keten van vertrouwen.

1. Aanmelden bij de [Azure-portal](http://portal.azure.com), en open uw Device Provisioning Service-exemplaar.

2. Selecteer de **registraties beheren** tabblad en klik vervolgens op de **toevoegen registratiegroep** knop aan de bovenkant van de pagina. 

3. Op **Registratiegroep toevoegen**, voer de volgende informatie en klik op de **opslaan** knop.

    **Naam van groep**: Voer **contoso-aangepaste-toegewezen-apparaten**.

    **Type Attestation**: Selecteer **symmetrische sleutel**.

    **Automatisch sleutels genereren**: dit selectievakje al moet worden gecontroleerd.

    **Selecteer de gewenste apparaten toewijzen aan hubs**: Selecteer **aangepast (gebruik Azure-functie)**.

    ![Aangepaste toewijzing registratiegroep voor attestation-symmetrische sleutel toevoegen](./media/how-to-use-custom-allocation-policies/create-custom-allocation-enrollment.png)


4. Op **Registratiegroep toevoegen**, klikt u op **een nieuwe IoT-hub koppelen** te koppelen van uw nieuwe divisie IoT-hubs.

    **Abonnement**: als u meerdere abonnementen hebt, kiest u het abonnement waarin u de divisie IoT-hubs hebt gemaakt.

    **IoT-hub**: Selecteer een van de divisie hubs die u hebt gemaakt.

    **Het toegangsbeleid**: kies **iothubowner**.

    ![Koppeling van de divisie IoT-hubs met de provisioning-service](./media/how-to-use-custom-allocation-policies/link-divisional-hubs.png)


5. Op **Registratiegroep toevoegen**, nadat de beide divisie IoT-hubs zijn gekoppeld, moet u deze selecteren als de IoT Hub-groep voor de registratiegroep zoals hieronder wordt weergegeven:

    ![Maken van de divisie hub-groep voor de inschrijving](./media/how-to-use-custom-allocation-policies/enrollment-divisional-hub-group.png)


6. Op **Registratiegroep toevoegen**, schuif omlaag naar de **Selecteer Azure Function** sectie en klikt u op **maken van een nieuwe functie-app**.

7. Op **functie-App** maken dat wordt geopend en voer de volgende instellingen voor uw nieuwe functie en klik op pagina **maken**:

    **App-naam**: Voer de naam van een unieke functie-app. **Contoso-functie-app-1098** wordt weergegeven als een voorbeeld.

    **Resourcegroep**: Selecteer **Use existing** en de **contoso-us-resource-group** te houden van alle resources die in dit artikel is gemaakt, samen.

    **Application Insights**: voor deze oefening kunt u dit uitschakelen.

    ![De functie-app maken](./media/how-to-use-custom-allocation-policies/function-app-create.png)


8. Terug op uw **Registratiegroep toevoegen** pagina, zorg ervoor dat uw nieuwe functie-app is geselecteerd. U moet mogelijk opnieuw Selecteer het abonnement om de functie-app-lijst te vernieuwen.

    Nadat de nieuwe functie-app is geselecteerd, klikt u op **maken van een nieuwe functie**.

    ![De functie-app maken](./media/how-to-use-custom-allocation-policies/click-create-new-function.png)

    uw nieuwe functie-app wordt geopend.

9. Klik op uw functie-app op om een nieuwe functie te maken

    ![De functie-app maken](./media/how-to-use-custom-allocation-policies/new-function.png)

    Voor de nieuwe functie kunt u de standaardinstellingen gebruiken om u te maken van een nieuwe **Webhook + API** met behulp van de **CSharp** taal. Klik op **maken van deze functie**.

    Hiermee maakt u een nieuwe C#-functie met de naam **HttpTriggerCSharp1**.

10. Vervang de code voor de nieuwe C#-functie met de volgende code en klik op **opslaan**:    

    ```C#
    #r "Newtonsoft.Json"
    using System.Net;
    using System.Text;
    using Newtonsoft.Json;

    public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
    {
        // Just some diagnostic logging
        log.Info("C# HTTP trigger function processed a request.");
        log.Info("Request.Content:...");    
        log.Info(req.Content.ReadAsStringAsync().Result);

        // Get request body
        dynamic data = await req.Content.ReadAsAsync<object>();

        // Get registration ID of the device
        string regId = data?.deviceRuntimeContext?.registrationId;

        string message = "Uncaught error";
        bool fail = false;
        ResponseObj obj = new ResponseObj();

        if (regId == null)
        {
            message = "Registration ID not provided for the device.";
            log.Info("Registration ID : NULL");
            fail = true;
        }
        else
        {
            string[] hubs = data?.linkedHubs.ToObject<string[]>();

            // Must have hubs selected on the enrollment
            if (hubs == null)
            {
                message = "No hub group defined for the enrollment.";
                log.Info("linkedHubs : NULL");
                fail = true;
            }
            else
            {
                // This is a Contoso Toaster Model 007
                if (regId.Contains("-contoso-tstrsd-007"))
                {
                    //Find the "-toasters-" IoT hub configured on the enrollment
                    foreach(string hubString in hubs)
                    {
                        if (hubString.Contains("-toasters-"))
                            obj.iotHubHostName = hubString;
                    }

                    if (obj.iotHubHostName == null)
                    {
                        message = "No toasters hub found for the enrollment.";
                        log.Info(message);
                        fail = true;
                    }
                }
                // This is a Contoso Heat pump Model 008
                else if (regId.Contains("-contoso-hpsd-088"))
                {
                    //Find the "-heatpumps-" IoT hub configured on the enrollment
                    foreach(string hubString in hubs)
                    {
                        if (hubString.Contains("-heatpumps-"))
                            obj.iotHubHostName = hubString;
                    }

                    if (obj.iotHubHostName == null)
                    {
                        message = "No heat pumps hub found for the enrollment.";
                        log.Info(message);
                        fail = true;
                    }
                }
                // Unrecognized device.
                else
                {
                    fail = true;
                    message = "Unrecognized device registration.";
                    log.Info("Unknown device registration");
                }
            }
        }

        return (fail)
            ? req.CreateResponse(HttpStatusCode.BadRequest, message)
            : new HttpResponseMessage(HttpStatusCode.OK)
            {
                Content = new StringContent(JsonConvert.SerializeObject(obj, Formatting.Indented), Encoding.UTF8, "application/json")
            };
    }    

    public class DeviceTwinObj
    {
        public string deviceId {get; set;}
    }

    public class ResponseObj
    {
        public string iotHubHostName {get; set;}
        public string IoTHub {get; set;}
        public DeviceTwinObj initialTwin {get; set;}
        public string[] linkedHubs {get; set;}
        public string enrollment {get; set;}
    }
    ```


11. Ga terug naar uw **Registratiegroep toevoegen** pagina, en zorg ervoor dat de nieuwe functie is geselecteerd. U moet mogelijk opnieuw selecteert u de functie-app om de functielijst te vernieuwen.

    Nadat de nieuwe functie is geselecteerd, klikt u op **opslaan** om op te slaan van de registratiegroep.

    ![Ten slotte de registratiegroep opslaan](./media/how-to-use-custom-allocation-policies/save-enrollment.png)


12. Na het opslaan van de inschrijving, opent u het opnieuw en neem notitie van de **primaire sleutel**. U moet eerst om de sleutels die zijn gegenereerd van de inschrijving opslaan. Deze sleutel wordt gebruikt voor het genereren van unieke apparaat-sleutels voor de gesimuleerde apparaten later opnieuw.


## <a name="derive-unique-device-keys"></a>Unieke apparaat-sleutels worden afgeleid

In deze sectie maakt u twee unieke apparaat-sleutels. Een sleutel wordt gebruikt voor een gesimuleerde toaster-apparaat. De andere sleutel wordt gebruikt voor een gesimuleerde heatmap pomp-apparaat.

Gebruiken voor het genereren van de sleutel van het apparaat, de **primaire sleutel** u eerder hebt genoteerd voor het berekenen van de [HMAC-SHA256](https://wikipedia.org/wiki/HMAC) van het apparaat registratie-ID voor elk apparaat en het resultaat in Base 64-indeling converteren.

Gebruik de volgende twee apparaatregistratie-id's en compute van een apparaatsleutel voor beide apparaten. Een geldige achtervoegsel om te werken met de voorbeeldcode voor het aangepaste toewijzingsbeleid voor beide registratie-id's hebben:

- **breakroom499-contoso-tstrsd-007**
- **mainbuilding167-contoso-hpsd-088**

#### <a name="linux-workstations"></a>Linux-werkstations

Als u een Linux-werkstation gebruikt, kunt u openssl gebruiken voor het genereren van uw apparaatsleutels afgeleide zoals wordt weergegeven in het volgende voorbeeld.

Vervang de waarde van **sleutel** met de **primaire sleutel** u eerder hebt genoteerd.

```bash
KEY=oiK77Oy7rBw8YB6IS6ukRChAw+Yq6GC61RMrPLSTiOOtdI+XDu0LmLuNm11p+qv2I+adqGUdZHm46zXAQdZoOA==

REG_ID1=breakroom499-contoso-tstrsd-007
REG_ID2=mainbuilding167-contoso-hpsd-088

keybytes=$(echo $KEY | base64 --decode | xxd -p -u -c 1000)
devkey1=$(echo -n $REG_ID1 | openssl sha256 -mac HMAC -macopt hexkey:$keybytes -binary | base64)
devkey2=$(echo -n $REG_ID2 | openssl sha256 -mac HMAC -macopt hexkey:$keybytes -binary | base64)

echo -e $"\n\n$REG_ID1 : $devkey1\n$REG_ID2 : $devkey2\n\n"
```

```bash
breakroom499-contoso-tstrsd-007 : JC8F96eayuQwwz+PkE7IzjH2lIAjCUnAa61tDigBnSs=
mainbuilding167-contoso-hpsd-088 : 6uejA9PfkQgmYylj8Zerp3kcbeVrGZ172YLa7VSnJzg=
```


#### <a name="windows-based-workstations"></a>Windows-werkstations

Als u een Windows-werkstation gebruikt, kunt u PowerShell gebruiken voor het genereren van de afgeleide apparaatsleutel zoals wordt weergegeven in het volgende voorbeeld.

Vervang de waarde van **sleutel** met de **primaire sleutel** u eerder hebt genoteerd.

```PowerShell
$KEY='oiK77Oy7rBw8YB6IS6ukRChAw+Yq6GC61RMrPLSTiOOtdI+XDu0LmLuNm11p+qv2I+adqGUdZHm46zXAQdZoOA=='

$REG_ID1='breakroom499-contoso-tstrsd-007'
$REG_ID2='mainbuilding167-contoso-hpsd-088'

$hmacsha256 = New-Object System.Security.Cryptography.HMACSHA256
$hmacsha256.key = [Convert]::FromBase64String($key)
$sig1 = $hmacsha256.ComputeHash([Text.Encoding]::ASCII.GetBytes($REG_ID1))
$sig2 = $hmacsha256.ComputeHash([Text.Encoding]::ASCII.GetBytes($REG_ID2))
$derivedkey1 = [Convert]::ToBase64String($sig1)
$derivedkey2 = [Convert]::ToBase64String($sig2)

echo "`n`n$REG_ID1 : $derivedkey1`n$REG_ID2 : $derivedkey2`n`n"
```

```PowerShell
breakroom499-contoso-tstrsd-007 : JC8F96eayuQwwz+PkE7IzjH2lIAjCUnAa61tDigBnSs=
mainbuilding167-contoso-hpsd-088 : 6uejA9PfkQgmYylj8Zerp3kcbeVrGZ172YLa7VSnJzg=
```


De gesimuleerde apparaten worden de apparaatsleutels afgeleide gebruiken met elke registratie-ID om uit te voeren van de symmetrische sleutel attestation.




## <a name="prepare-an-azure-iot-c-sdk-development-environment"></a>Een Azure IoT C SDK-ontwikkelomgeving voorbereiden

In deze sectie maakt u een ontwikkelomgeving die is gebruikt voor het bouwen bereidt de [Azure IoT C SDK](https://github.com/Azure/azure-iot-sdk-c). De SDK bevat de voorbeeldcode voor het gesimuleerde apparaat. Dit gesimuleerde apparaat probeert tijdens de opstartvolgorde van het apparaat wordt ingericht.

Deze sectie is gericht op een Windows-werkstation. Zie voor een voorbeeld van een Linux, de configuratie van de virtuele machines in [inrichten voor multitenancy](how-to-provision-multitenant.md).



1. Download de versie 3.11.4 van de [CMake-bouwsysteem](https://cmake.org/download/). Controleer het gedownloade binaire bestand met behulp van de bijbehorende cryptografische hash-waarde. In het volgende voorbeeld is Windows PowerShell gebruikt om de cryptografische hash te controleren voor versie 3.11.4 van de x64 MSI-distributie:

    ```PowerShell
    PS C:\Downloads> $hash = get-filehash .\cmake-3.11.4-win64-x64.msi
    PS C:\Downloads> $hash.Hash -eq "56e3605b8e49cd446f3487da88fcc38cb9c3e9e99a20f5d4bd63e54b7a35f869"
    True
    ```
    
    De volgende hashwaarden voor versie 3.11.4 zijn vermeld op de CMake-site op het moment van schrijven:

    ```
    6dab016a6b82082b8bcd0f4d1e53418d6372015dd983d29367b9153f1a376435  cmake-3.11.4-Linux-x86_64.tar.gz
    72b3b82b6d2c2f3a375c0d2799c01819df8669dc55694c8b8daaf6232e873725  cmake-3.11.4-win32-x86.msi
    56e3605b8e49cd446f3487da88fcc38cb9c3e9e99a20f5d4bd63e54b7a35f869  cmake-3.11.4-win64-x64.msi
    ```

    Het is belangrijk dat de vereisten voor Visual Studio met (Visual Studio en de workload Desktopontwikkeling met C++) op uw computer zijn geïnstalleerd **voordat** de `CMake`-installatie wordt gestart. Zodra aan de vereisten is voldaan en de download is geverifieerd, installeert u het CMake-bouwsysteem.

2. Open een opdrachtprompt of Git Bash-shell. Voer de volgende opdracht uit om de Azure IoT C SDK GitHub-opslagplaats te klonen:
    
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

4. Voer de volgende opdracht uit, bouwt een versie van de SDK die specifiek zijn voor uw clientplatform voor ontwikkeling. Er wordt een Visual Studio-oplossing voor het gesimuleerde apparaat gegenereerd in de map `cmake`. 

    ```cmd
    cmake -Duse_prov_client:BOOL=ON ..
    ```
    
    Als `cmake` uw C++-compiler niet kan vinden, kunnen er fouten in de build optreden tijdens het uitvoeren van de bovenstaande opdracht. Als dit gebeurt, voert u deze opdracht uit bij de [Visual Studio-opdrachtprompt](https://docs.microsoft.com/dotnet/framework/tools/developer-command-prompt-for-vs). 

    Zodra het bouwen is voltooid, zijn de laatste paar uitvoerregels vergelijkbaar met de volgende uitvoer:

    ```cmd/sh
    $ cmake -Duse_prov_client:BOOL=ON ..
    -- Building for: Visual Studio 15 2017
    -- Selecting Windows SDK version 10.0.16299.0 to target Windows 10.0.17134.
    -- The C compiler identification is MSVC 19.12.25835.0
    -- The CXX compiler identification is MSVC 19.12.25835.0

    ...

    -- Configuring done
    -- Generating done
    -- Build files have been written to: E:/IoT Testing/azure-iot-sdk-c/cmake
    ```




## <a name="simulate-the-devices"></a>De apparaten simuleren

In deze sectie maakt u een voorbeeld van een inrichten met de naam bijwerken **prov\_dev\_client\_voorbeeld** zich in de Azure IoT C SDK u eerder hebt ingesteld. 

Deze voorbeeldcode simuleert de opstartvolgorde van een apparaat waarmee de aanvraag voor inrichting worden verzonden naar uw Device Provisioning Service-exemplaar. De opstartvolgorde zorgt ervoor dat het apparaat toaster worden herkend en toegewezen aan de IoT-hub met behulp van de aangepaste toewijzingsbeleid.

1. Selecteer in Azure Portal het tabblad **Overzicht** voor uw Device Provisioning-service en noteer de waarde van het **_Id-bereik_**.

    ![Device Provisioning Service-eindpuntgegevens uit de portalblade extraheren](./media/quick-create-simulated-device-x509/extract-dps-endpoints.png) 

2. Open in Visual Studio, de **azure_iot_sdks.sln** oplossingsbestand dat is gegenereerd door eerdere CMake uitgevoerd. Het oplossingsbestand moet in de volgende locatie:

    ```
    \azure-iot-sdk-c\cmake\azure_iot_sdks.sln
    ```

3. Navigeer in het deelvenster *Solution Explorer* van Visual Studio naar de map **Provision\_Samples**. Vouw het voorbeeldproject met de naam **prov\_dev\_client\_sample** uit. Vouw **Source Files** uit en open **prov\_dev\_client\_sample.c**.

4. Zoek de constante `id_scope` op en vervang de waarde door uw **Id-bereik**-waarde die u eerder hebt gekopieerd. 

    ```c
    static const char* id_scope = "0ne00002193";
    ```

5. Zoek de definitie voor de functie `main()` op in hetzelfde bestand. Zorg ervoor dat de `hsm_type` variabele is ingesteld op `SECURE_DEVICE_TYPE_SYMMETRIC_KEY` zoals hieronder wordt weergegeven:

    ```c
    SECURE_DEVICE_TYPE hsm_type;
    //hsm_type = SECURE_DEVICE_TYPE_TPM;
    //hsm_type = SECURE_DEVICE_TYPE_X509;
    hsm_type = SECURE_DEVICE_TYPE_SYMMETRIC_KEY;
    ```

6. Klik met de rechtermuisknop op het **prov\_dev\_client\_sample**-project en selecteer **Set as Startup Project**. 

#### <a name="simulate-the-contoso-toaster-device"></a>Het Contoso toaster-apparaat simuleren

1. In Visual Studio *Solution Explorer* venster, gaat u naar de **hsm\_security\_client** project en vouw dit uit. Vouw **bronbestanden**, en open **hsm\_client\_key.c**. 

    Zoek de declaratie van de `REGISTRATION_NAME` en `SYMMETRIC_KEY_VALUE` constanten. De volgende wijzigingen aanbrengen aan het bestand en sla het bestand.

    Werk de waarde van de `REGISTRATION_NAME` constante met de registratie-ID voor het apparaat toaster **breakroom499-contoso-tstrsd-007**.
    
    Werk de waarde van de `SYMMETRIC_KEY_VALUE` constante met de apparaatsleutel die u hebt gegenereerd voor het apparaat toaster. De waarde **JC8F96eayuQwwz + PkE7IzjH2lIAjCUnAa61tDigBnSs =** wordt alleen weergegeven als een voorbeeld.

    ```c
    static const char* const REGISTRATION_NAME = "breakroom499-contoso-tstrsd-007";
    static const char* const SYMMETRIC_KEY_VALUE = "JC8F96eayuQwwz+PkE7IzjH2lIAjCUnAa61tDigBnSs=";
    ```

2. Selecteer in het menu van Visual Studio de optie **Debug** > **Start without debugging** om de oplossing uit te voeren. Klik in de prompt om het project opnieuw te bouwen op **Yes** om het project opnieuw te bouwen voordat het wordt uitgevoerd.

    De volgende uitvoer is een voorbeeld van het gesimuleerde toaster-apparaat is opgestart en verbinding maken met de provisioning Service-exemplaar moet worden toegewezen aan de broodroosters IoT-hub door het toewijzingsbeleid voor aangepaste:

    ```cmd
    Provisioning API Version: 1.2.9

    Registering Device

    Provisioning Status: PROV_DEVICE_REG_STATUS_CONNECTED
    Provisioning Status: PROV_DEVICE_REG_STATUS_ASSIGNING
    Provisioning Status: PROV_DEVICE_REG_STATUS_ASSIGNING

    Registration Information received from service: contoso-toasters-hub-1098.azure-devices.net, deviceId: breakroom499-contoso-tstrsd-007

    Press enter key to exit:
    ```


#### <a name="simulate-the-contoso-heat-pump-device"></a>Het Contoso heatmap pomp-apparaat simuleren

1. Terug in Visual Studio *Solution Explorer* venster, gaat u naar de **hsm\_security\_client** project en vouw dit uit. Vouw **bronbestanden**, en open **hsm\_client\_key.c**. 

    Zoek de declaratie van de `REGISTRATION_NAME` en `SYMMETRIC_KEY_VALUE` constanten. De volgende wijzigingen aanbrengen aan het bestand en sla het bestand.

    Werk de waarde van de `REGISTRATION_NAME` constante met de registratie-ID voor het apparaat van de heatmap pomp **mainbuilding167-contoso-hpsd-088**.
    
    Werk de waarde van de `SYMMETRIC_KEY_VALUE` constante met de apparaatsleutel die u hebt gegenereerd voor het apparaat toaster. De waarde **6uejA9PfkQgmYylj8Zerp3kcbeVrGZ172YLa7VSnJzg =** wordt alleen weergegeven als een voorbeeld.

    ```c
    static const char* const REGISTRATION_NAME = "mainbuilding167-contoso-hpsd-088";
    static const char* const SYMMETRIC_KEY_VALUE = "6uejA9PfkQgmYylj8Zerp3kcbeVrGZ172YLa7VSnJzg=";
    ```

7. Selecteer in het menu van Visual Studio de optie **Debug** > **Start without debugging** om de oplossing uit te voeren. Klik in de prompt om het project opnieuw te bouwen op **Yes** om het project opnieuw te bouwen voordat het wordt uitgevoerd.

    De volgende uitvoer is een voorbeeld van het apparaat gesimuleerde heatmap pomp noodzakelijk is opgestart en verbinding maken met de provisioning Service-exemplaar moet worden toegewezen aan de Contoso heatmap pompen IoT-hub door het toewijzingsbeleid voor aangepaste:

    ```cmd
    Provisioning API Version: 1.2.9

    Registering Device

    Provisioning Status: PROV_DEVICE_REG_STATUS_CONNECTED
    Provisioning Status: PROV_DEVICE_REG_STATUS_ASSIGNING
    Provisioning Status: PROV_DEVICE_REG_STATUS_ASSIGNING

    Registration Information received from service: contoso-heatpumps-hub-1098.azure-devices.net, deviceId: mainbuilding167-contoso-hpsd-088

    Press enter key to exit:
    ```




## <a name="troubleshooting-custom-allocation-policies"></a>Aangepaste van toewijzingsbeleid voor problemen oplossen

De volgende tabel ziet u verwachte scenario's en de resultaten foutcodes die optreden. Deze tabel gebruiken om op te lossen aangepaste toewijzingsfouten beleid met uw Azure-functies.


| Scenario | Registratieresultaat van Provisioning Service | Inrichting van de resultaten van de SDK |
| -------- | --------------------------------------------- | ------------------------ |
| De webhook geeft als resultaat 200 OK met iotHubHostName ingesteld op een geldige hostnaam van de IoT-hub | Resultaat van de status: toegewezen  | SDK retourneert PROV_DEVICE_RESULT_OK samen met informatie van de hub |
| De webhook geeft als resultaat 200 OK met 'iotHubHostName' die aanwezig zijn in het antwoord, maar ingesteld op een lege tekenreeks of null zijn | Resultaat van de status: mislukt<br><br> Foutcode: CustomAllocationIotHubNotSpecified (400208) | SDK retourneert PROV_DEVICE_RESULT_HUB_NOT_SPECIFIED |
| De webhook retourneert een 401-niet gemachtigd | Resultaat van de status: mislukt<br><br>Foutcode: CustomAllocationUnauthorizedAccess (400209) | SDK retourneert PROV_DEVICE_RESULT_UNAUTHORIZED |
| Een afzonderlijke inschrijving is gemaakt om het apparaat uitschakelen | Resultaat van de status: uitgeschakeld | SDK retourneert PROV_DEVICE_RESULT_DISABLED |
| De webhook retourneert foutcode > = 429 | De DPS-indeling wordt opnieuw geprobeerd een aantal keer. Het beleid voor opnieuw proberen is momenteel:<br><br>&nbsp;&nbsp;-Aantal nieuwe pogingen: 10<br>&nbsp;&nbsp;-Eerste interval: 1s<br>&nbsp;&nbsp;-Verhoog: 9 's | SDK wordt fout negeren en een andere get-statusbericht indienen in de opgegeven tijd |
| De webhook wordt elke andere statuscode geretourneerd | Resultaat van de status: mislukt<br><br>Foutcode: CustomAllocationFailed (400207) | SDK retourneert PROV_DEVICE_RESULT_DEV_AUTH_ERROR |


## <a name="clean-up-resources"></a>Resources opschonen

Als u van plan bent om door te gaan werken met resources in dit artikel hebt gemaakt, kunt u ze kunt laten. Als u niet van plan bent om door te gaan met behulp van de resource, gebruikt u de volgende stappen uit te verwijderen van alle resources die zijn gemaakt door in dit artikel om onnodige kosten te voorkomen.

De stappen die hier wordt ervan uitgegaan dat u alle resources in dit artikel gemaakt volgens de instructies in dezelfde resourcegroep met de naam **contoso-us-resource-group**.

> [!IMPORTANT]
> Het verwijderen van een resourcegroep kan niet ongedaan worden gemaakt. De resourcegroep en alle resources daarin worden permanent verwijderd. Zorg ervoor dat u niet per ongeluk de verkeerde resourcegroep of resources verwijdert. Als u de IoT Hub in een bestaande resourcegroep hebt gemaakt met resources die u wilt behouden, moet u alleen de IoT Hub-resource zelf verwijderen in plaats van de resourcegroep te verwijderen.
>

Verwijderen van de resourcegroep met de naam:

1. Meld u aan bij [Azure Portal](https://portal.azure.com) en klik op **Resourcegroepen**.

2. In de **filteren op naam...**  tekstvak, type de naam van de resource van de groep met uw resources **contoso-us-resource-group**. 

3. Klik rechts van de resourcegroep in de lijst met resultaten op **...** en vervolgens op **Resourcegroep verwijderen**.

4. U wordt gevraagd om het verwijderen van de resourcegroep te bevestigen. Typ de naam van de resourcegroep nogmaals om te bevestigen en klik op **Verwijderen**. Na enkele ogenblikken worden de resourcegroep en alle resources in de groep verwijderd.

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer meer Reprovisioning [reprovisoning concepten van IoT Hub-apparaat](concepts-device-reprovision.md) 
- Zie voor meer meer opheffen van inrichting [hoe u de inrichting van apparaten die zijn eerder automatisch ingericht ](how-to-unprovision-devices.md) 











