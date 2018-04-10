---
title: Een apparaat inrichten met behulp van Azure IoT Hub Device Provisioning Service | Microsoft Docs
description: Uw apparaat inrichten in een enkele IoT-hub met behulp van IoT Hub Device Provisioning Service
services: iot-dps
keywords: ''
author: dsk-2015
ms.author: dkshir
ms.date: 03/28/2018
ms.topic: tutorial
ms.service: iot-dps
documentationcenter: ''
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 4d98ce103bed7f9d14eb45422b70ceca1328afaa
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/30/2018
---
# <a name="provision-the-device-to-an-iot-hub-using-the-azure-iot-hub-device-provisioning-service"></a>Het apparaat inrichten in een IoT-hub met behulp van IoT Hub Device Provisioning Service

In de vorige zelfstudie hebt u geleerd hoe u een apparaat kunt instellen om verbinding te maken met Device Provisioning Service. In deze zelfstudie leert u hoe u deze service gebruikt om een apparaat in te richten in een enkele IoT-hub, met behulp van **_inschrijvingslijsten_**. In deze handleiding ontdekt u hoe u:

> [!div class="checklist"]
> * Het apparaat inschrijven
> * Het apparaat starten
> * Controleren of het apparaat is geregistreerd

## <a name="prerequisites"></a>Vereisten

Voordat u doorgaat, moet uw apparaat zijn geconfigureerd zoals is besproken in de zelfstudie [Een apparaat instellen voor inrichten met behulp van Azure IoT Hub Device Provisioning Service](./tutorial-set-up-device.md).

Als u niet bekend bent met het proces van automatische inrichting, bekijkt u de [Concepten voor automatische inrichting](concepts-auto-provisioning.md) voordat u verdergaat.

<a id="enrolldevice"></a>
## <a name="enroll-the-device"></a>Het apparaat inschrijven

In deze stap worden de unieke beveiligingsartefacten van het apparaat toegevoegd aan Device Provisioning Service. Deze beveiligingsartefacten zijn als volgt gebaseerd op het [Attestation-mechanisme](concepts-device.md#attestation-mechanism) van het apparaat:

- Voor TPM-apparaten hebt u het volgende nodig:
    - De *Goedkeuringssleutel* die uniek is voor elke TPM-chip of -simulatie en kan worden verkregen bij de TPM-chipfabrikant.  Lees [TPM-goedkeuringssleutel begrijpen](https://technet.microsoft.com/library/cc770443.aspx) voor meer informatie.
    - De *Registratie-id* die wordt gebruikt om een apparaat op unieke wijze te identificeren in de naamruimte of het bereik. Deze id is al dan niet dezelfde id als van het apparaat. De id is verplicht voor elk apparaat. Voor TPM-apparaten kan de registratie-id worden afgeleid van de TPM zelf, bijvoorbeeld een SHA-256-hash van de TPM-goedkeuringssleutel.

    ![Registratiegegevens voor TPM in de portal](./media/tutorial-provision-device-to-hub/tpm-device-enrollment.png)

- Voor X.509-apparaten hebt u het volgende nodig:
    - Het [certificaat dat is verleend aan de X.509](https://msdn.microsoft.com/library/windows/desktop/bb540819.aspx)-chip of -simulatie, in de vorm van een *.pem*- of *.cer*-bestand. Voor een afzonderlijke inschrijving moet u het *ondertekenaarcertificaat* per apparaat voor het X.509-systeem gebruiken, maar voor het registreren van groepen, moet u het *hoofdcertificaat* gebruiken. 

    ![Inschrijvingsgegevens voor X.509 in de portal](./media/tutorial-provision-device-to-hub/x509-device-enrollment.png)

Er zijn twee manieren om het apparaat te registreren bij Device Provisioning Service:

- **Inschrijvingsgroepen**: dit is een groep apparaten die een specifiek attestation-mechanisme delen. We raden u aan om een inschrijvingsgroep te gebruiken bij een groot aantal apparaten die een gewenste initiële configuratie delen, of voor apparaten die allemaal zijn verbonden met dezelfde tenant.

    ![Inschrijvingsgroepen voor X.509 in de portal](./media/tutorial-provision-device-to-hub/x509-enrollment-groups.png)

- **Afzonderlijke inschrijvingen**: dit is een vermelding voor een enkel apparaat dat kan worden geregistreerd met Device Provisioning Service. Afzonderlijke inschrijvingen kunnen X509-certificaten of SAS-tokens (in een echt of virtueel TPM) gebruiken als attestation-mechanismen. We raden u aan om afzonderlijke inschrijvingen te gebruiken bij apparaten die unieke initiële configuraties vereisen, en voor apparaten die alleen SAS-tokens via TPM of virtuele TPM kunnen gebruiken als het attestation-mechanisme. Afzonderlijke inschrijvingen hebben mogelijk de gewenste apparaat-id voor IoT Hub die is opgegeven.

Nu schrijft u het apparaat in met uw Device Provisioning Service-instantie, met behulp van de vereiste beveiligingsartefacten op basis van het attestation-mechanisme van het apparaat: 

1. Meld u aan bij Azure Portal, klik in het linkermenu op de knop **Alle bronnen** en open Device Provisioning Service.

2. Selecteer **Manage enrollments** in de overzichtsblade van Device Provisioning Service. Selecteer het tabblad **Afzonderlijke inschrijvingen** of het tabblad **Inschrijvingsgroepen** afhankelijk van de gekozen installatie voor het apparaat. Klik bovenaan op de knop **Toevoegen**. Selecteer **TPM** of **X.509** als het attestation-*mechanisme*, en voer de juiste beveiligingsartefacten toe, zoals eerder besproken. U kunt een nieuwe **apparaat-id voor IoT Hub** invoeren. Klik op de knop **Save** als u klaar bent. 

3. Wanneer het apparaat is ingeschreven, ziet u dat deze in de portal als volgt wordt weergegeven:

    ![Geslaagde TPM-inschrijving in de portal](./media/tutorial-provision-device-to-hub/tpm-enrollment-success.png)

Na de inschrijving wacht de inrichtingsservice tot het apparaat op enig moment later is opgestart en verbonden met de service. Als het apparaat voor de eerste keer wordt opgestart, communiceert de SDK-clientbibliotheek met uw chip om de beveiligingsartefacten te extraheren uit het apparaat, en wordt de registratie bij Device Provisioning Service geverifieerd. 

## <a name="start-the-device"></a>Het apparaat starten

Op dit punt is de volgende installatie gereed voor apparaatregistratie:

1. Uw apparaat of groep met apparaten zijn geregistreerd bij Device Provisioning Service, en 
2. Het attestation-mechanisme is geconfigureerd op het apparaat en het apparaat is toegankelijk via de toepassing met behulp van de SDK van de Device Provisioning Service-client.

Start het apparaat zodat de registratie via de clienttoepassing met Device Provisioning service kan worden gestart.  

## <a name="verify-the-device-is-registered"></a>Controleren of het apparaat is geregistreerd

Zodra het apparaat is opgestart, worden de volgende acties uitgevoerd. Zie de voorbeeldtoepassing van TPM-simulator [dps_client_sample](https://github.com/Azure/azure-iot-device-auth/blob/master/dps_client/samples/dps_client_sample/dps_client_sample.c) voor meer informatie. 

1. Vanaf het apparaat wordt een registratie-aanvraag verzonden naar Device Provisioning Service.
2. Voor TPM-apparaten wordt vanuit Device Provisioning Service een registratie-uitdaging teruggestuurd waarop het apparaat reageert. 
3. Als de registratie is gelukt, worden Device Provisioning Service de IoT Hub-URI, apparaat-id en versleutelingssleutel via Device Provisioning Service teruggestuurd naar het apparaat. 
4. De IoT Hub-clienttoepassing op het apparaat maakt vervolgens verbinding met de hub. 
5. Als de verbinding met de hub is gemaakt, wordt het apparaat weergegeven in **Device Explorer** van de IoT-hub. 

    ![Geslaagde verbinding met de hub in de portal](./media/tutorial-provision-device-to-hub/hub-connect-success.png)

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Het apparaat inschrijven
> * Het apparaat starten
> * Controleren of het apparaat is geregistreerd

Ga naar de volgende zelfstudie voor informatie over het inrichten van meerdere apparaten via hubs met gelijke taakverdeling. 

> [!div class="nextstepaction"]
> [Apparaten inrichten in meerdere IoT-hubs met gelijke taakverdeling](./tutorial-provision-multiple-hubs.md)
