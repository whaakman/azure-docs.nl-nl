---
title: Inrichten van een apparaat met Azure IoT Hub apparaat-inrichtingsservice | Microsoft Docs
description: Uw apparaat met een enkele iothub met behulp van de Azure IoT Hub apparaat inrichtingsservice inrichten
services: iot-dps
keywords: 
author: dsk-2015
ms.author: dkshir
ms.date: 09/05/2017
ms.topic: tutorial
ms.service: iot-dps
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: bf50699d2dc67294d554ba15713254a8b88d8ade
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="provision-the-device-to-an-iot-hub-using-the-azure-iot-hub-device-provisioning-service"></a>Het apparaat naar een iothub met behulp van de Azure IoT Hub apparaat inrichtingsservice inrichten

In de vorige zelfstudie hebt u geleerd hoe u een apparaat instellen verbinding maken met uw service apparaten inrichten. In deze zelfstudie leert u hoe u deze service gebruikt voor het inrichten van uw apparaat met een enkele IoT-hub met  **_inschrijving lijsten_**. In deze handleiding ontdekt u hoe u:

> [!div class="checklist"]
> * Het apparaat inschrijven
> * Start het apparaat
> * Controleer of dat het apparaat is geregistreerd

## <a name="prerequisites"></a>Vereisten

Voordat u doorgaat, zorg ervoor dat uw apparaat configureren en de bijbehorende *Hardware Security Module* zoals beschreven in de zelfstudie [instellen van een apparaat om in te richten met behulp van Azure IoT Hub apparaat-inrichtingsservice](./tutorial-set-up-device.md).


<a id="enrolldevice"></a>
## <a name="enroll-the-device"></a>Het apparaat inschrijven

Deze stap omvat het unieke beveiligings-artefacten van het apparaat toe te voegen aan de Service voor het inrichten van apparaten. Deze beveiliging artefacten zijn als volgt:

- Voor op basis van TPM-apparaten:
    - De *goedkeuringssleutel* die uniek is voor elke TPM-chip of de simulatie. Lees de [TPM-goedkeuringssleutel begrijpen](https://technet.microsoft.com/library/cc770443.aspx) voor meer informatie.
    - De *registratie-ID* die wordt gebruikt om een apparaat in de naamruimte/scope uniek te identificeren. Of deze mogelijk niet hetzelfde zijn als de apparaat-ID. De ID is verplicht voor elk apparaat. Voor apparaten op basis van TPM kan de registratie-ID van de TPM zelf, bijvoorbeeld een SHA-256-hash van de TPM-goedkeuringssleutel afkomstig zijn.

    ![Registratie-informatie voor de TPM in de portal](./media/tutorial-provision-device-to-hub/tpm-device-enrollment.png)

- Voor X.509 op basis van apparaten:
    - De [certificaat uitgegeven aan de X.509](https://msdn.microsoft.com/library/windows/desktop/bb540819.aspx) chip of simulatie in de vorm van een *.pem* of een *.cer* bestand. Voor afzonderlijke inschrijving die u wilt gebruiken, de *ondertekenaar certificaat* voor uw systeem X.509 terwijl voor de inschrijving van groepen, moet u gebruiken de *basiscertificaat*.

    ![Registratie-informatie voor X.509 in de portal](./media/tutorial-provision-device-to-hub/x509-device-enrollment.png)


Er zijn twee manieren het apparaat naar de Service voor het inrichten van apparaten te registreren:

- **Inschrijving groepen** dit vertegenwoordigt een groep apparaten die een specifieke attestation-mechanisme delen. Aangeraden wordt een inschrijvingsgroep voor voor een groot aantal apparaten dat een gewenste initiële configuratie delen, of voor apparaten alle gaat dezelfde tenant.

    ![Inschrijving groepen voor X.509 in de portal](./media/tutorial-provision-device-to-hub/x509-enrollment-groups.png)

- **Afzonderlijke inschrijvingen** Hiermee wordt een vermelding voor één apparaat bij de Service voor het inrichten van apparaten registreren kan. Afzonderlijke inschrijvingen mag gebruiken beide x509 certificaten of SAS-tokens (in een TPM echt of virtueel) als de attestation-mechanismen. U wordt aangeraden met behulp van afzonderlijke inschrijvingen voor apparaten die unieke initiële configuraties vereisen of voor apparaten die u kunnen alleen SAS-tokens via TPM of virtuele TPM gebruiken als het mechanisme voor attestation. Afzonderlijke inschrijvingen wellicht de gewenste IoT hub apparaat-ID opgegeven.

Hier volgen de stappen voor het registreren van het apparaat in de portal:

1. Noteer de artefacten beveiliging voor de HSM op uw apparaat. Moet u mogelijk gebruikmaken van de API's die worden vermeld in het gedeelte [beveiliging artefacten extraheren](./tutorial-set-up-device.md#extractsecurity) van de vorige zelfstudie, in een ontwikkelomgeving.

1. Meld u aan bij Azure Portal, klik in het linkermenu op de knop **All resources** en open uw Device Provisioning-service.

1. Selecteer **Manage enrollments** in de overzichtsblade van Device Provisioning Service. Selecteer een **afzonderlijke inschrijvingen** tabblad of de **inschrijving groepen** tabblad aan de hand van uw apparaat instellen. Klik op de **toevoegen** bovenaan op de knop. Selecteer **TPM** of **X.509** als de identiteit attestation *mechanisme*, en voer de juiste beveiligingsrechten artefacten, zoals eerder besproken. U kunt een nieuwe invoeren **IoT Hub apparaat-ID**. Klik op de knop **Save** als u klaar bent. 

1. Wanneer het apparaat is ingeschreven, ziet u deze in de portal als volgt weergegeven:

    ![Geslaagde TPM-inschrijving in de portal](./media/tutorial-provision-device-to-hub/tpm-enrollment-success.png)


## <a name="start-the-device"></a>Start het apparaat

De volgende instellingen zijn nu gereed voor apparaatregistratie:

1. Uw apparaat of een groep apparaten zijn geregistreerd met uw service apparaten inrichten en 
2. Uw apparaat is klaar met de HSM-chip geconfigureerd en toegankelijk zijn via de toepassing die gebruikmaakt van de inrichtingsservice apparaat-client-SDK.

Start het apparaat zodat uw clienttoepassing starten van de registratie met uw service apparaten inrichten.  


## <a name="verify-the-device-is-registered"></a>Controleer of dat het apparaat is geregistreerd

Eenmaal uw apparaat opnieuw is opgestart, de volgende acties moeten worden uitgevoerd. Zie de voorbeeldtoepassing voor TPM-simulator [dps_client_sample](https://github.com/Azure/azure-iot-device-auth/blob/master/dps_client/samples/dps_client_sample/dps_client_sample.c) voor meer informatie. 

1. Het apparaat verzendt een aanvraag voor functieregistratie met uw service apparaten inrichten.
2. Voor de TPM-apparaten vraagt de Service voor het inrichten van apparaten terug registratie waarop het apparaat reageert. 
3. Op de registratie is voltooid verzendt de Service voor het inrichten van apparaten de URI van de IoT-hub, apparaat-ID en de versleutelde sleutel terug naar het apparaat. 
4. De IoT Hub-clienttoepassing op het apparaat vervolgens verbinding maakt met uw hub. 
5. Geslaagde verbinding met de hub, ziet u het apparaat worden weergegeven in de IoT-hub **apparaat Explorer**. 

    ![Geslaagde verbinding met de hub in de portal](./media/tutorial-provision-device-to-hub/hub-connect-success.png)

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Het apparaat inschrijven
> * Start het apparaat
> * Controleer of dat het apparaat is geregistreerd

Ga naar de volgende zelfstudie voor informatie over het inrichten van meerdere apparaten via hubs taakverdeling. 

> [!div class="nextstepaction"]
> [Inrichten van apparaten via IoT hubs Netwerktaakverdeling](./tutorial-provision-multiple-hubs.md)
