---
title: Over het inrichten van apparaten voor multitenancy in de Azure IoT Hub Device Provisioning Service | Microsoft Docs
description: Over het inrichten van apparaten voor multitenancy met uw device provisioning service-exemplaar
author: wesmc7777
ms.author: wesmc
ms.date: 08/15/2018
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: timlt
ms.openlocfilehash: 54804867cfaf38965b3dbf5ceb51e08a731d4dd8
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46966536"
---
# <a name="how-to-provision-for-multitenancy"></a>Inrichten voor multitenancy 

Het toewijzingsbeleid gedefinieerd door de inrichtingsservice ondersteuning van tal van scenario's voor toewijzing. Er zijn twee gangbare scenario's:

* **Geolocatie / GeoLatency**: als een apparaat worden verplaatst tussen locaties, netwerklatentie is verbeterd doordat het apparaat is ingericht voor de IoT-hub die het dichtst bij elke locatie. In dit scenario wordt een groep van IoT-hubs in regio's omvatten, worden geselecteerd voor inschrijvingen. De **laagste latentie** toewijzingsbeleid is geselecteerd voor deze registraties. Dit beleid zorgt ervoor dat de Device Provisioning Service om te evalueren apparaat latentie en bepalen de kast IoT-hub uit de groep van IoT-hubs. 

* **Multitenancy**: apparaten die worden gebruikt binnen een IoT-oplossing mogelijk moeten worden toegewezen aan een specifieke IoT-hub of de groep van IoT-hubs. De oplossing moeten alle apparaten voor een bepaalde tenant om te communiceren met een specifieke groep van IoT-hubs. In sommige gevallen kan een tenant eigenaar bent van IoT-hubs en vereisen dat apparaten worden toegewezen aan hun IoT-hubs.

Het is gebruikelijk om te combineren van deze twee scenario's. Een multitenant IoT-oplossing wordt bijvoorbeeld vaak toewijzen voor tenant-apparaten met behulp van een groep van IoT-hubs die worden verspreid over verschillende regio's. Deze tenant-apparaten kunnen worden toegewezen aan de IoT-hub in de groep met de laagste latentie op basis van geografische locatie.

In dit artikel wordt een voorbeeld van het gesimuleerde apparaat van de [Azure IoT C SDK](https://github.com/Azure/azure-iot-sdk-c) te laten zien hoe u apparaten in een scenario voor meerdere tenants inrichten in regio's. U wordt de volgende stappen uitvoeren in dit artikel:

* De Azure CLI gebruiken voor het maken van de twee regionale IoT-hubs (**VS-West** en **VS-Oost**)
* Maken van een multitenant-inschrijving
* De Azure CLI gebruiken voor het maken van de twee regionale virtuele Linux-machines om te fungeren als apparaten in dezelfde regio's (**VS-West** en **VS-Oost**)
* De ontwikkelomgeving instellen voor de Azure IoT C SDK op beide virtuele Linux-machines
* De apparaten om te zien dat deze zijn ingericht voor de dezelfde tenant in de dichtstbijzijnde regio simuleren.


[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]


## <a name="prerequisites"></a>Vereisten

* Voltooiing van de [IoT Hub Device Provisioning Service instellen met de Azure-portal](./quick-setup-auto-provision.md) Quick Start.


[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]


## <a name="create-two-regional-iot-hubs"></a>Twee regionale IoT-hubs maken

In deze sectie gebruikt u de Azure Cloud Shell te maken van twee nieuwe regionale IoT-hubs in de **VS-West** en **VS-Oost** regio's voor een tenant.


1. De Azure Cloud Shell gebruiken om te maken van een resourcegroep met de [az-groep maken](/cli/azure/group#az-group-create) opdracht. Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. 

    Het volgende voorbeeld wordt een resourcegroep met de naam *contoso-us-resource-group* in de *eastus* regio. Het verdient aanbeveling gebruik te maken van deze groep voor alle resources die in dit artikel is gemaakt. Dit maakt opschonen gemakkelijker nadat u klaar bent.

    ```azurecli-interactive 
    az group create --name contoso-us-resource-group --location eastus
    ```

2. De Azure Cloud Shell gebruiken om te maken van een IoT-hub in de **eastus** regio met de [az iot hub maken](/cli/azure/iot/hub#az-iot-hub-create) opdracht. De IoT-hub worden toegevoegd aan de *contoso-us-resource-group*.

    Het volgende voorbeeld wordt een IoT-hub met de naam *contoso-Oost-hub* in de *eastus* locatie. Moet u de naam van uw eigen unieke hub in plaats van **contoso-Oost-hub**.

    ```azurecli-interactive 
    az iot hub create --name contoso-east-hub --resource-group contoso-us-resource-group --location eastus --sku S1
    ```
    
    Met deze opdracht kan enkele minuten duren.

3. De Azure Cloud Shell gebruiken om te maken van een IoT-hub in de **westus** regio met de [az iot hub maken](/cli/azure/iot/hub#az-iot-hub-create) opdracht. Deze IoT hub wordt ook toegevoegd aan de *contoso-us-resource-group*.

    Het volgende voorbeeld wordt een IoT-hub met de naam *contoso-west-hub* in de *westus* locatie. Moet u de naam van uw eigen unieke hub in plaats van **contoso-west-hub**.

    ```azurecli-interactive 
    az iot hub create --name contoso-west-hub --resource-group contoso-us-resource-group --location westus --sku S1
    ```

    Met deze opdracht kan enkele minuten duren.



## <a name="create-the-multitenant-enrollment"></a>De multitenant inschrijving maken

In deze sectie maakt u een nieuwe registratiegroep voor de tenant-apparaten.  

Voor het gemak in dit artikel wordt [symmetrische sleutel attestation](concepts-symmetric-key-attestation.md) bij de inschrijving. Overweeg het gebruik van een veiligere oplossing [x.509-certificaat attestation](concepts-security.md#x509-certificates) met een keten van vertrouwen.

1. Aanmelden bij de [Azure-portal](http://portal.azure.com), en open uw Device Provisioning Service-exemplaar.

2. Selecteer de **registraties beheren** tabblad en klik vervolgens op de **toevoegen registratiegroep** knop aan de bovenkant van de pagina. 

3. Op **Registratiegroep toevoegen**, voer de volgende informatie en klik op de **opslaan** knop.

    **Naam van groep**: Voer **contoso-us-apparaten**.

    **Type Attestation**: Selecteer **symmetrische sleutel**.

    **Automatisch sleutels genereren**: dit selectievakje al moet worden gecontroleerd.

    **Selecteer de gewenste apparaten toewijzen aan hubs**: Selecteer **laagste latentie**.

    ![Multitenant registratiegroep voor attestation-symmetrische sleutel toevoegen](./media/how-to-provision-multitenant/create-multitenant-enrollment.png)


4. Op **Registratiegroep toevoegen**, klikt u op **een nieuwe IoT-hub koppelen** te koppelen van de regionale hubs.

    **Abonnement**: als u meerdere abonnementen hebt, kiest u het abonnement waarin u de regionale IoT-hubs hebt gemaakt.

    **IoT-hub**: Selecteer een van de regionale hubs die u hebt gemaakt.

    **Het toegangsbeleid**: kies **iothubowner**.

    ![De regionale IoT-hubs met de provisioning-service koppelen](./media/how-to-provision-multitenant/link-regional-hubs.png)


5. Nadat de beide regionale IoT-hubs zijn gekoppeld, moet u deze selecteren voor de registratiegroep en klikt u op **opslaan** te maken van de regionale IoT hub-groep voor de inschrijving.

    ![De regionale hub-groep voor de inschrijving maken](./media/how-to-provision-multitenant/enrollment-regional-hub-group.png)


6. Na het opslaan van de inschrijving, opent u het opnieuw en neem notitie van de **primaire sleutel**. U moet eerst om de sleutels die zijn gegenereerd van de inschrijving opslaan. Deze sleutel wordt gebruikt voor het genereren van unieke apparaat-sleutels voor beide gesimuleerde apparaten later opnieuw.


## <a name="create-regional-linux-vms"></a>Regionale Linux-VM's maken

In deze sectie maakt u twee regionale virtuele Linux-machines (VM's). Deze virtuele machines wordt een voorbeeld van de simulatie apparaat uitvoeren uit elke regio ter illustratie van apparaat inrichten voor tenant-apparaten in beide regio's.

Om het opschonen gemakkelijker, deze VM's wordt toegevoegd aan dezelfde resourcegroep met de IoT-hubs die zijn gemaakt, *contoso-us-resource-group*. De virtuele machines wordt echter uitgevoerd in verschillende regio's (**VS-West** en **VS-Oost**).

1. In de Azure Cloud Shell, dan de volgende opdracht maakt een **VS-Oost** regio VM nadat u hebt de volgende Parameterwijzigingen aangebracht in de opdracht:

    **--naam**: Voer een unieke naam voor uw **VS-Oost** regionale apparaat VM. 

    **-admin-username**: uw eigen naam gebruiken.

    **---beheerderswachtwoord**: uw eigen beheerderswachtwoord gebruiken.

    ```azurecli-interactive
    az vm create \
    --resource-group contoso-us-resource-group \
    --name ContosoSimDeviceEest \
    --location eastus \
    --image Canonical:UbuntuServer:18.04-LTS:18.04.201809110 \
    --admin-username contosoadmin \
    --admin-password myContosoPassword2018 \
    --authentication-type password
    ```

    Met deze opdracht duurt enkele minuten om te voltooien. Als de opdracht is voltooid, maak een notitie van de **publicIpAddress** waarde voor de regio VS-Oost-VM.

1. In de Azure Cloud Shell, geeft u de opdracht maakt een **VS-West** regio VM nadat u hebt de volgende Parameterwijzigingen aangebracht in de opdracht:

    **--naam**: Voer een unieke naam voor uw **VS-West** regionale apparaat VM. 

    **-admin-username**: uw eigen naam gebruiken.

    **---beheerderswachtwoord**: uw eigen beheerderswachtwoord gebruiken.

    ```azurecli-interactive
    az vm create \
    --resource-group contoso-us-resource-group \
    --name ContosoSimDeviceWest \
    --location westus \
    --image Canonical:UbuntuServer:18.04-LTS:18.04.201809110 \
    --admin-username contosoadmin \
    --admin-password myContosoPassword2018 \
    --authentication-type password
    ```

    Met deze opdracht duurt enkele minuten om te voltooien. Als de opdracht is voltooid, maak een notitie van de **publicIpAddress** waarde voor de regio VS-West VM.

1. Open twee opdrachtregel shells. Verbinding maken met een van de regionale virtuele machines in elke shell met behulp van SSH. 

    De gebruikersnaam van uw beheerder en het openbare IP-adres dat u hebt genoteerd voor de virtuele machine als parameters SSH doorgeven. Voer het beheerderswachtwoord wanneer hierom wordt gevraagd.

    ```bash
    ssh contosoadmin@1.2.3.4

    contosoadmin@ContosoSimDeviceEast:~$
    ```

    ```bash
    ssh contosoadmin@5.6.7.8

    contosoadmin@ContosoSimDeviceWest:~$
    ```



## <a name="prepare-the-azure-iot-c-sdk-development-environment"></a>Bereid de ontwikkelomgeving voor Azure IoT C SDK

In deze sectie wordt u de Azure IoT C SDK op elke virtuele machine klonen. De SDK bevat een voorbeeld waarin het simuleren van een tenant voor apparaatinrichting uit elke regio.


1. Voor elke virtuele machine, installeert u **Cmake**, **g ++**, **gcc**, en [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) met de volgende opdrachten:

    ```bash
    sudo apt-get update
    sudo apt-get install cmake build-essential libssl-dev libcurl4-openssl-dev uuid-dev git-all
    ```


1. Kloon de [Azure IoT C SDK](https://github.com/Azure/azure-iot-sdk-c) op beide VM's.

    ```bash
    cd ~/
    git clone https://github.com/Azure/azure-iot-sdk-c.git --recursive
    ```

    De grootte van deze opslagplaats is momenteel ongeveer 220 MB. Deze bewerking kan enkele minuten in beslag nemen.

1. Voor beide VM's, maakt u een nieuwe **cmake** map in de opslagplaats en de wijzigingen in die map.

    ```bash
    mkdir ~/azure-iot-sdk-c/cmake
    cd ~/azure-iot-sdk-c/cmake
    ```

1. Voor beide VM's, voert u de volgende opdracht maakt die een versie van de SDK die specifiek zijn voor uw clientplatform voor ontwikkeling. 

    ```bash
    cmake -Duse_prov_client:BOOL=ON ..
    ```

    Zodra het bouwen is voltooid, zijn de laatste paar uitvoerregels vergelijkbaar met de volgende uitvoer:

    ```bash
    -- IoT Client SDK Version = 1.2.9
    -- Provisioning client ON
    -- Provisioning SDK Version = 1.2.9
    -- target architecture: x86_64
    -- Checking for module 'libcurl'
    --   Found libcurl, version 7.58.0
    -- Found CURL: curl
    -- target architecture: x86_64
    -- target architecture: x86_64
    -- target architecture: x86_64
    -- target architecture: x86_64
    -- iothub architecture: x86_64
    -- target architecture: x86_64
    -- Configuring done
    -- Generating done
    -- Build files have been written to: /home/contosoadmin/azure-iot-sdk-c/cmake
    ```    


## <a name="derive-unique-device-keys"></a>Unieke apparaat-sleutels worden afgeleid

Als u de symmetrische sleutel attestation voor groepsinschrijvingen, gebruikt u niet de sleutels van de groep inschrijving rechtstreeks. In plaats daarvan u maakt een unieke afgeleid voor elk apparaat en de vermelde in [Groepsinschrijvingen met symmetrische sleutels](concepts-symmetric-key-attestation.md#group-enrollments).

Voor het genereren van de sleutel van het apparaat, gebruikt u de hoofdsleutel van de groep voor het berekenen van een [HMAC-SHA256](https://wikipedia.org/wiki/HMAC) van de unieke registratie-ID voor het apparaat en het resultaat in Base 64-indeling converteren.

Voegt de hoofdsleutel van de groep toe aan de apparaatcode van uw.

Het voorbeeld van Bash-shell gebruiken om te maken van een afgeleide apparaatsleutel voor het gebruik van elk apparaat **openssl**.

- Vervang de waarde voor **sleutel** met de **primaire sleutel** u eerder hebt genoteerd voor uw inschrijving.

- Vervang de waarde voor **REG_ID** met uw eigen unieke registratie-ID voor elk apparaat. Gebruik kleine alfanumeriek en streepje ('-') tekens dat moet worden beide-id's definiëren.

Voorbeeld van de apparaat-sleutelgeneratie voor *contoso-simdevice-Oost*:

```bash
KEY=rLuyBPpIJ+hOre2SFIP9Ajvdty3j0EwSP/WvTVH9eZAw5HpDuEmf13nziHy5RRXmuTy84FCLpOnhhBPASSbHYg==
REG_ID=contoso-simdevice-east

keybytes=$(echo $KEY | base64 --decode | xxd -p -u -c 1000)
echo -n $REG_ID | openssl sha256 -mac HMAC -macopt hexkey:$keybytes -binary | base64
```

```bash
p3w2DQr9WqEGBLUSlFi1jPQ7UWQL4siAGy75HFTFbf8=
```

Voorbeeld van de apparaat-sleutelgeneratie voor *contoso-simdevice-west*:

```bash
KEY=rLuyBPpIJ+hOre2SFIP9Ajvdty3j0EwSP/WvTVH9eZAw5HpDuEmf13nziHy5RRXmuTy84FCLpOnhhBPASSbHYg==
REG_ID=contoso-simdevice-west

keybytes=$(echo $KEY | base64 --decode | xxd -p -u -c 1000)
echo -n $REG_ID | openssl sha256 -mac HMAC -macopt hexkey:$keybytes -binary | base64
```

```bash
J5n4NY2GiBYy7Mp4lDDa5CbEe6zDU/c62rhjCuFWxnc=
```


De tenant-apparaten wordt elk gebruik van hun afgeleide apparaatsleutel en unieke registratie-ID om uit te voeren van de attestation-symmetrische sleutel met de registratiegroep tijdens het inrichten voor de tenant IoT hubs.




## <a name="simulate-the-devices-from-each-region"></a>De apparaten uit elke regio simuleren


In deze sectie wordt een inrichting voorbeeld in de Azure IoT C SDK bijgewerkt voor zowel van de regionale virtuele machines. 

De voorbeeldcode simuleert de opstartvolgorde van een apparaat waarmee de aanvraag voor inrichting worden verzonden naar uw Device Provisioning Service-exemplaar. De opstartvolgorde zorgt ervoor dat het apparaat kan worden herkend en toegewezen aan de IoT-hub die het dichtst is gebaseerd op de latentie.

1. Selecteer in Azure Portal het tabblad **Overzicht** voor uw Device Provisioning-service en noteer de waarde van het **_Id-bereik_**.

    ![Device Provisioning Service-eindpuntgegevens uit de portalblade extraheren](./media/quick-create-simulated-device-x509/extract-dps-endpoints.png) 

1. Open **~/azure-iot-sdk-c/provisioning\_client/samples/prov\_dev\_client\_voorbeeld/prov\_dev\_client\_sample.c**voor het bewerken van op beide VM's.

    ```bash
    vi ~/azure-iot-sdk-c/provisioning_client/samples/prov_dev_client_sample/prov_dev_client_sample.c
    ```

1. Zoek de constante `id_scope` op en vervang de waarde door uw **Id-bereik**-waarde die u eerder hebt gekopieerd. 

    ```c
    static const char* id_scope = "0ne00002193";
    ```

1. Zoek de definitie voor de functie `main()` op in hetzelfde bestand. Zorg ervoor dat de `hsm_type` variabele is ingesteld op `SECURE_DEVICE_TYPE_SYMMETRIC_KEY` zoals hieronder wordt weergegeven zodat deze overeenkomen met de inschrijvingsmethode voor de attestation van groep. 

    Sla uw wijzigingen in de bestanden op beide VM's.

    ```c
    SECURE_DEVICE_TYPE hsm_type;
    //hsm_type = SECURE_DEVICE_TYPE_TPM;
    //hsm_type = SECURE_DEVICE_TYPE_X509;
    hsm_type = SECURE_DEVICE_TYPE_SYMMETRIC_KEY;
    ```


1. Open **~/azure-iot-sdk-c/provisioning\_adapters-client/hsm\_client\_key.c** op beide VM's. 

    ```bash
     vi ~/azure-iot-sdk-c/provisioning_client/adapters/hsm_client_key.c
    ```

1. Zoek de declaratie van de `REGISTRATION_NAME` en `SYMMETRIC_KEY_VALUE` constanten. De volgende wijzigingen aanbrengen in de bestanden op beide regionale VM's en sla de bestanden.

    Werk de waarde van de `REGISTRATION_NAME` constante met de **unieke registratie-ID voor uw apparaat**.
    
    Werk de waarde van de `SYMMETRIC_KEY_VALUE` constante met uw **apparaatsleutel afgeleid**.

    ```c
    static const char* const REGISTRATION_NAME = "contoso-simdevice-east";
    static const char* const SYMMETRIC_KEY_VALUE = "p3w2DQr9WqEGBLUSlFi1jPQ7UWQL4siAGy75HFTFbf8=";
    ```

    ```c
    static const char* const REGISTRATION_NAME = "contoso-simdevice-west";
    static const char* const SYMMETRIC_KEY_VALUE = "J5n4NY2GiBYy7Mp4lDDa5CbEe6zDU/c62rhjCuFWxnc=";
    ```

1. Navigeer naar de voorbeeldmap hieronder wordt weergegeven op beide VM's, en bouw het voorbeeld.

    ```bash
    cd ~/azure-iot-sdk-c/cmake/provisioning_client/samples/prov_dev_client_sample/
    cmake --build . --target prov_dev_client_sample --config Debug
    ```

1. Zodra de opbouwbewerking uitvoeren **prov\_dev\_client\_sample.exe** op beide VM's voor het simuleren van een tenant-apparaat uit elke regio. U ziet dat elk apparaat IoT-hub die het dichtst bij de regio's van het gesimuleerde apparaat is toegewezen aan de tenant.

    ```bash
    contosoadmin@ContosoSimDeviceEast:~/azure-iot-sdk-c/cmake/provisioning_client/samples/prov_dev_client_sample$ ./prov_dev_client_sample
    Provisioning API Version: 1.2.9

    Registering Device

    Provisioning Status: PROV_DEVICE_REG_STATUS_CONNECTED
    Provisioning Status: PROV_DEVICE_REG_STATUS_ASSIGNING
    Provisioning Status: PROV_DEVICE_REG_STATUS_ASSIGNING

    Registration Information received from service: contoso-east-hub.azure-devices.net, deviceId: contoso-simdevice-east
    Press enter key to exit:

    ```

    ```bash
    contosoadmin@ContosoSimDeviceWest:~/azure-iot-sdk-c/cmake/provisioning_client/samples/prov_dev_client_sample$ ./prov_dev_client_sample
    Provisioning API Version: 1.2.9

    Registering Device

    Provisioning Status: PROV_DEVICE_REG_STATUS_CONNECTED
    Provisioning Status: PROV_DEVICE_REG_STATUS_ASSIGNING
    Provisioning Status: PROV_DEVICE_REG_STATUS_ASSIGNING

    Registration Information received from service: contoso-west-hub.azure-devices.net, deviceId: contoso-simdevice-west
    Press enter key to exit:
    ```



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











