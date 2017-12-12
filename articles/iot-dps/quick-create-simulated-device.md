---
title: Een gesimuleerd apparaat inrichten voor Azure IoT Hub | Microsoft Docs
description: 'QuickStart van Azure: een gesimuleerd apparaat maken en inrichten met Azure IoT Hub Device Provisioning Service'
services: iot-dps
keywords: 
author: dsk-2015
ms.author: dkshir
ms.date: 09/18/2017
ms.topic: hero-article
ms.service: iot-dps
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 6e486b8fb5924c8a021a781742478e9082ca668c
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/05/2017
---
# <a name="create-and-provision-a-simulated-device-using-iot-hub-device-provisioning-services"></a>Een gesimuleerd apparaat maken en inrichten met IoT Hub Device Provisioning Service
> [!div class="op_single_selector"]
> * [TPM](quick-create-simulated-device.md)
> * [X.509](quick-create-simulated-device-x509.md)

In deze stappen wordt getoond hoe u een gesimuleerd apparaat maakt op een ontwikkelcomputer met Windows OS, de Windows TPM-simulator uitvoert als de [HSM (Hardware Security Module)](https://azure.microsoft.com/blog/azure-iot-supports-new-security-hardware-to-strengthen-iot-security/) van het apparaat en het codevoorbeeld gebruikt om dit gesimuleerde apparaat te verbinden met Device Provisioning Service en uw IoT-hub. 

Voltooi de stappen in [IoT Hub Device Provisioning Service instellen](./quick-setup-auto-provision.md) voordat u verdergaat.

<a id="setupdevbox"></a>
## <a name="prepare-the-development-environment"></a>De ontwikkelomgeving voorbereiden 

1. Op uw computer moet Visual Studio 2015 of [Visual Studio 2017](https://www.visualstudio.com/vs/) zijn geïnstalleerd. De workload 'Desktop development with C++' (Bureaubladontwikkeling met C++) moet zijn ingeschakeld voor uw installatie van Visual Studio.

2. Download en installeer het [CMake-bouwsysteem](https://cmake.org/download/).

3. Zorg ervoor dat `git` op de computer wordt geïnstalleerd en toegevoegd aan de omgevingsvariabelen die voor het opdrachtvenster toegankelijk zijn. Zie [Software Freedom Conservancy's Git client tools](https://git-scm.com/download/) (Git-clienthulpprogramma's van Software Freedom Conservancy) om de nieuwste versie van `git`-hulpprogramma's te installeren, waaronder **Git Bash**, de opdrachtregel-app die u kunt gebruiken voor interactie met de lokale Git-opslagplaats. 

4. Open een opdrachtprompt of Git Bash. Kloon het codevoorbeeld voor de GitHub-opslagplaats voor apparaatsimulatie:
    
    ```cmd/sh
    git clone https://github.com/Azure/azure-iot-sdk-c.git --recursive
    ```

5. Maak een map in de lokale kopie van deze GitHub-opslagplaats voor het CMake-bouwproces. 

    ```cmd/sh
    cd azure-iot-sdk-c
    mkdir cmake
    cd cmake
    ```

6. In het codevoorbeeld wordt een Windows TPM-simulator gebruikt. Voer de volgende opdracht uit om SAS-tokenverificatie in te schakelen. Er wordt ook een Visual Studio-oplossing voor het gesimuleerde apparaat gegenereerd.

    ```cmd/sh
    cmake -Ddps_auth_type=tpm_simulator ..
    ```

7. Ga door middel van een aparte opdrachtprompt naar de hoofdmap van GitHub en voer de [TPM](https://docs.microsoft.com/windows/device-security/tpm/trusted-platform-module-overview)-simulator uit. Deze luistert via een socket op poorten 2321 en 2322. Sluit dit opdrachtvenster niet; de simulator moet actief blijven tot u deze QuickStart hebt voltooid. 

    ```cmd/sh
    .\azure-iot-sdk-c\dps_client\deps\utpm\tools\tpm_simulator\Simulator.exe
    ```

## <a name="create-a-device-enrollment-entry-in-the-device-provisioning-service"></a>Een vermelding voor apparaatinschrijving maken in Device Provisioning Service

1. Open de in de map *cmake* gemaakt oplossing met de naam `azure_iot_sdks.sln` en bouw deze in Visual Studio.

2. Klik met de rechtermuisknop op het **tpm_device_provision**-project en selecteer **Set as Startup Project**. Voer de oplossing uit. In het uitvoervenster worden de voor apparaatinschrijving vereiste **_goedkeuringssleutel_** en **_registratie-id_** weergegeven. Noteer deze waarden. 

3. Meld u aan bij Azure Portal, klik in het linkermenu op de knop **All resources** en open uw Device Provisioning-service.

4. Selecteer **Manage enrollments** in de overzichtsblade van Device Provisioning Service. Selecteer het tabblad **Individual Enrollments** en klik bovenaan op de knop **Add**. 

5. Voer onder bij **Registratielijstitem toevoegen** de volgende gegevens in:
    - Selecteer **TPM** als *mechanisme* voor identiteitscontrole.
    - Voer de *registratie-id* en *goedkeuringssleutel* voor het TPM-apparaat in. 
    - Selecteer een IoT-hub die is gekoppeld aan uw inrichtingsservice.
    - Voer een unieke apparaat-id in. Vermijd gevoelige gegevens bij het benoemen van uw apparaat.
    - Werk de **initiële status van de apparaatdubbel** bij met de gewenste beginconfiguratie voor het apparaat.
    - Klik op de knop **Save** als u klaar bent. 

    ![Gegevens van apparaatinschrijving invoeren in de portalblade](./media/quick-create-simulated-device/enter-device-enrollment.png)  

   Als het apparaat is ingeschreven, wordt de *Registration ID* ervan weergegeven in de lijst onder het tabblad *Individual Enrollments*. 


<a id="firstbootsequence"></a>
## <a name="simulate-first-boot-sequence-for-the-device"></a>Eerste opstartvolgorde voor het apparaat simuleren

1. In Azure Portal selecteert u de blade **Overview** voor uw Device Provisioning-service en noteert u de waarden **_Global device endpoint_** en **_ID Scope_**.

    ![DPS-eindpuntgegevens uit de portalblade extraheren](./media/quick-create-simulated-device/extract-dps-endpoints.png) 

2. Selecteer in Visual Studio op de computer het voorbeeldproject met de naam **dps_client_sample** en open het bestand **dps_client_sample.c**.

3. Wijs de waarde _ID Scope_ aan de `dps_scope_id`-variabele toe. De `dps_uri`-variabele heeft dezelfde waarde als _Global device endpoint_. 

    ```c
    static const char* dps_uri = "global.azure-devices-provisioning.net";
    static const char* dps_scope_id = "[DPS Id Scope]";
    ```

4. Klik met de rechtermuisknop op het **dps_client_sample**-project en selecteer **Set as Startup Project**. Voet het voorbeeld uit. De gegevens van uw IoT-hub leest u in de berichten die het opstarten van het apparaat en het verbinding maken met Device Provisioning Service simuleren. Als het gesimuleerde apparaat is ingericht met de IoT-hub die is gekoppeld met de provisioningservice, wordt de apparaat-id weergegeven op de blade **Device Explorer** van de hub. 

    ![Apparaat wordt geregistreerd voor de IoT-hub](./media/quick-create-simulated-device/hub-registration.png) 

    Als u de standaardwaarde van de *initiële status van de apparaatdubbel* hebt gewijzigd in de inschrijvingsvermelding voor uw apparaat, kan de gewenste status van de dubbel uit de hub worden gehaald en er dienovereenkomstig naar worden gehandeld. Zie [Understand and use device twins in IoT Hub](../iot-hub/iot-hub-devguide-device-twins.md) (Apparaatdubbelen begrijpen en gebruiken in IoT Hub) voor meer informatie


## <a name="clean-up-resources"></a>Resources opschonen

Als u wilt blijven doorwerken met het voorbeeld van de apparaatclient en deze beter wilt leren kennen, wis de resources die in deze QuickStart zijn gemaakt dan niet. Als u niet wilt doorgaan, gebruikt u de volgende stappen om alle resources die via deze QuickStart zijn gemaakt, te verwijderen.

1. Sluit het uitvoervenster van het voorbeeld van de apparaatclient op de computer.
1. Sluit het TPM-simulatorvenster op de computer.
1. Klik in het linkermenu in Azure Portal op **All resources** en selecteer uw Device Provisioning-service. Klik bovenaan de blade **Alle resources** op **Verwijderen**.  
1. Klik in het linkermenu in de Azure Portal op **Alle resources** en selecteer vervolgens uw IoT-hub. Klik bovenaan de blade **Alle resources** op **Verwijderen**.  

## <a name="next-steps"></a>Volgende stappen

In deze QuickStart hebt u een gesimuleerd TPM-apparaat op de computer gemaakt en het ingericht voor uw IoT-hub met Azure IoT Hub Device Provisioning Service. Voor meer informatie over device provisioning, gaat u verder met de zelfstudie voor het instellen van Device Provisioning Service in Azure Portal. 

> [!div class="nextstepaction"]
> [Zelfstudies over Azure IoT Hub Device Provisioning Service](./tutorial-set-up-cloud.md)
