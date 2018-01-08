---
title: Een gesimuleerd TPM-apparaat met C# inrichten voor Azure IoT Hub | Microsoft Docs
description: 'Azure-quickstart: een gesimuleerd TPM-apparaat met de SDK voor C# maken en inrichten voor Azure IoT Hub Device Provisioning Service'
services: iot-dps
keywords: 
author: JimacoMS2
ms.author: v-jamebr
ms.date: 12/21/2017
ms.topic: hero-article
ms.service: iot-dps
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 73c0411d93dae242a735caeb8d9b990720903044
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2018
---
# <a name="create-and-provision-a-simulated-tpm-device-using-c-device-sdk-for-iot-hub-device-provisioning-service"></a>Een gesimuleerd TPM-apparaat met de SDK voor C# maken en inrichten voor IoT Hub Device Provisioning Service
> [!div class="op_single_selector"]
> * [C](quick-create-simulated-device.md)
> * [Java](quick-create-simulated-device-tpm-java.md)
> * [C#](quick-create-simulated-device-tpm-csharp.md)
> * [Python](quick-create-simulated-device-tpm-python.md)

In deze stappen ziet u hoe u met de SDK voor C# voor Azure IoT Hub een gesimuleerd TPM-apparaat maakt op een ontwikkelcomputer met Windows OS en het codevoorbeeld gebruikt om dit gesimuleerde apparaat te verbinden met de Device Provisioning Service en uw IoT-hub. De voorbeeldcode gebruikt de Windows TPM-simulator als de [HSM (Hardware Security Module)](https://azure.microsoft.com/blog/azure-iot-supports-new-security-hardware-to-strengthen-iot-security/) van het apparaat. 

Voltooi de stappen in [IoT Hub Device Provisioning Service instellen](./quick-setup-auto-provision.md) voordat u verdergaat.

<a id="setupdevbox"></a>
## <a name="prepare-the-development-environment"></a>De ontwikkelomgeving voorbereiden 

1. Zorg ervoor dat de [core-SDK voor .NET](https://www.microsoft.com/net/download/windows) op uw computer geïnstalleerd. 

1. Zorg ervoor dat `git` op de computer wordt geïnstalleerd en toegevoegd aan de omgevingsvariabelen die voor het opdrachtvenster toegankelijk zijn. Zie [Software Freedom Conservancy's Git client tools](https://git-scm.com/download/) (Git-clienthulpprogramma's van Software Freedom Conservancy) om de nieuwste versie van `git`-hulpprogramma's te installeren, waaronder **Git Bash**, de opdrachtregel-app die u kunt gebruiken voor interactie met de lokale Git-opslagplaats. 

4. Open een opdrachtprompt of Git Bash. Kloon de GitHub-opslagplaats Azure IoT SDK voor C#:
    
    ```cmd
    git clone --recursive https://github.com/Azure/azure-iot-sdk-csharp.git
    ```

## <a name="provision-the-simulated-device"></a>Het gesimuleerde apparaat inrichten


1. Meld u aan bij Azure Portal. Klik in het linkermenu op de knop **All resources** en open uw Device Provisioning-service. Noteer op de blade **Overzicht** de waarde voor **_Id-bereik_**.

    ![Bereik-id van provisioning-service kopiëren van portal-blade](./media/quick-create-simulated-device-tpm-csharp/copy-scope.png) 


2. Ga op een opdrachtprompt naar de projectmap met de voorbeeldcode voor het inrichten van het de TPM-apparaat.

    ```cmd
    cd .\azure-iot-sdk-csharp\provisioning\device\samples\ProvisioningDeviceClientTpm
    ```

2. Typ de volgende opdracht om de voorbeeldcode voor het TPM-apparaat te bouwen en uit te voeren. Vervang de waarde voor `<IDScope>` door het id-bereik voor uw provisioning-service. 

    ```cmd
    dotnet run <IDScope>
    ```

1. In het opdrachtvenster ziet u waarden voor **_Endorsement Key_**, **_Registration ID_** en een voorgestelde **_Device ID_**. Deze waarden hebt u nodig voor registratie van het apparaat. Noteer deze waarden. 
   > [!NOTE]
   > Verwar het venster met opdrachtuitvoer niet met het venster dat uitvoer van de TPM-simulator bevat. U moet mogelijk in het opdrachtvenster klikken om dit naar de voorgrond te halen.

    ![Uitvoer van het opdrachtvenster](./media/quick-create-simulated-device-tpm-csharp/output1.png) 


4. Selecteer in Azure Portal **Inschrijvingen beheren** in de overzichtsblade van Device Provisioning Service. Selecteer het tabblad **Afzonderlijke registraties** en klik bovenaan op de knop **Toevoegen**. 

5. Voer bij **Vermelding aan registratielijst toevoegen** de volgende gegevens in:
    - Selecteer **TPM** als *mechanisme* voor identiteitscontrole.
    - Voer de *registratie-id* en *goedkeuringssleutel* voor het TPM-apparaat in. 
    - Selecteer eventueel een IoT-hub die is gekoppeld aan uw inrichtingsservice.
    - Voer een unieke apparaat-id in. U kunt de apparaat-id invoeren die wordt voorgesteld in de voorbeelduitvoer of u kunt uw eigen id invoeren. In het laatste geval moet u geen gevoelige gegevens gebruiken bij het benoemen van uw apparaat. 
    - Werk de **initiële status van de apparaatdubbel** bij met de gewenste beginconfiguratie voor het apparaat.
    - Klik op de knop **Save** als u klaar bent. 

    ![Gegevens van apparaatinschrijving invoeren in de portalblade](./media/quick-create-simulated-device-tpm-csharp/enter-device-enrollment.png)  

   Als het apparaat is ingeschreven, wordt de *Registration ID* ervan weergegeven in de lijst onder het tabblad *Individual Enrollments*. 

6. Klik op Enter om het gesimuleerde apparaat te registreren. De gegevens van uw IoT-hub leest u in de berichten die het opstarten van het apparaat en het verbinding maken met Device Provisioning Service simuleren. 

1. Controleer of het apparaat is ingericht. Als het gesimuleerde apparaat is ingericht met de IoT-hub die is gekoppeld met de provisioning-service, wordt de apparaat-id weergegeven op de blade **IoT-apparaten** van de hub. 

    ![Apparaat wordt geregistreerd voor de IoT-hub](./media/quick-create-simulated-device-tpm-csharp/hub-registration.png) 

    Als u de standaardwaarde van de *initiële status van de apparaatdubbel* hebt gewijzigd in de inschrijvingsvermelding voor uw apparaat, kan de gewenste status van de dubbel uit de hub worden gehaald en er dienovereenkomstig naar worden gehandeld. Zie [Understand and use device twins in IoT Hub](../iot-hub/iot-hub-devguide-device-twins.md) (Apparaatdubbelen begrijpen en gebruiken in IoT Hub) voor meer informatie


## <a name="clean-up-resources"></a>Resources opschonen

Als u wilt blijven doorwerken met het voorbeeld van de apparaatclient en deze beter wilt leren kennen, wis de resources die in deze quickstart zijn gemaakt dan niet. Als u niet wilt doorgaan, gebruikt u de volgende stappen om alle resources die via deze quickstart zijn gemaakt, te verwijderen.

1. Sluit het uitvoervenster van het voorbeeld van de apparaatclient op de computer.
1. Sluit het TPM-simulatorvenster op de computer.
1. Klik in het linkermenu in Azure Portal op **All resources** en selecteer uw Device Provisioning-service. Klik bovenaan de blade **Alle resources** op **Verwijderen**.  
1. Klik in het linkermenu in de Azure Portal op **Alle resources** en selecteer vervolgens uw IoT-hub. Klik bovenaan de blade **Alle resources** op **Verwijderen**.  

## <a name="next-steps"></a>Volgende stappen

In deze quickstart hebt u een gesimuleerd TPM-apparaat op de computer gemaakt en dit ingericht voor uw IoT-hub met IoT Hub Device Provisioning Service. Als u wilt weten hoe u uw TPM-apparaat programmatisch kunt registreren, gaat u verder met de quickstart voor programmatische registratie van een TPM-apparaat. 

> [!div class="nextstepaction"]
> [Azure-quickstart: TPM-apparaat inschrijven bij Azure IoT Hub Device Provisioning Service](quick-enroll-device-tpm-node.md)
