---
title: Het beheren van apparaatinschrijvingen voor Azure IoT Hub | Microsoft Docs
description: Het beheren van apparaatinschrijvingen voor uw service DP's in de Azure Portal
services: iot-dps
keywords: 
author: dsk-2015
ms.author: dkshir
ms.date: 09/05/2017
ms.topic: article
ms.service: iot-dps
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: c230e73f83d8acd0f142e037f70a80c9e0e4107e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-manage-device-enrollments-in-the-iot-hub-device-provisioning-service"></a>Het beheren van inschrijvingen van apparaten in de IoT Hub apparaat inrichten van Service

Een *apparaatinschrijving* maakt een record van een enkel apparaat of een groep apparaten die mogelijk op een bepaald moment geregistreerd bij de Azure IoT Hub apparaat inrichtingsservice. De record van inschrijving bevat de eerste gewenste configuratie voor de apparaten als onderdeel van dat de inschrijving, met inbegrip van de gewenste IoT-hub. In dit artikel leest u hoe voor het beheren van apparaatinschrijvingen voor uw provisioning-service.


## <a name="create-a-device-enrollment"></a>Maken van een inschrijving van apparaten

Er zijn twee manieren waarop u uw apparaten met de inrichting service kunt inschrijven:

1. Een **inschrijvingsgroep** is een vermelding voor een groep apparaten die een gemeenschappelijke attestation-mechanisme van X.509-certificaten delen, ondertekend door de dezelfde basis-CA. Aangeraden wordt een inschrijvingsgroep voor voor een groot aantal apparaten met een gewenste initiële configuratie delen of voor apparaten alle gaat dezelfde tenant. Houd er rekening mee dat u alleen de apparaten die gebruikmaken van het mechanisme voor X.509 attestation als kan inschrijven *inschrijving groepen*. 

    U kunt een registratie-groep maken in de portal voor een groep van apparaten met behulp van de volgende stappen uit.

    1. Aanmelden bij de Azure-portal en klikt u op **alle resources** in het menu links.
    2. Klik op de apparaten inrichten service die u wilt inschrijven van uw apparaat uit de lijst met resources.
    3. In uw provisioning-service, klikt u op **inschrijvingen beheren**, selecteer daarna **inschrijving groepen** tabblad.
    4. Klik op de **toevoegen** aan de bovenkant en voert u de vereiste informatie voor de vermelding in de lijst inschrijving. Upload het basiscertificaat voor de groep van apparaten. 
    5. Klik op **Opslaan**. Op uw registratie-groep is gemaakt, ziet u de naam van de groep worden weergegeven onder de **inschrijving groepen** tabblad. 

        ![Registratie-groep in de portal](./media/how-to-manage-enrollments/group-enrollment.png)

    
2. Een **afzonderlijke inschrijving** is een vermelding voor één apparaat kan registreren. Afzonderlijke inschrijvingen mag gebruiken beide x509 certificaten of SAS-tokens (in een TPM echt of virtueel) als de attestation-mechanismen. U wordt aangeraden met behulp van afzonderlijke inschrijvingen voor apparaten die unieke initiële configuraties vereisen of voor apparaten die u kunnen alleen SAS-tokens via TPM of virtuele TPM gebruiken als het mechanisme voor attestation. Afzonderlijke inschrijvingen wellicht de gewenste IoT hub apparaat-ID opgegeven.

    U kunt een afzonderlijke inschrijving maken in de portal met de volgende stappen uit. 

    1. Aanmelden bij de Azure-portal en klikt u op **alle resources** in het menu links.
    2. Klik op de apparaten inrichten service die u wilt inschrijven van uw apparaat uit de lijst met resources.
    3. In uw provisioning-service, klikt u op **inschrijvingen beheren**, selecteer daarna **afzonderlijke inschrijvingen** tabblad.
    4. Klik op de **toevoegen** bovenaan op de knop. 
    5. Selecteer het beveiligingsmechanisme voor het apparaat en voer de vereiste informatie voor de vermelding in de lijst inschrijving. Upload een ondertekend certificaat als het apparaat X.509 implementeert. 
    6. Klik op **Opslaan**. Op uw registratie-groep is gemaakt, ziet u het apparaat worden weergegeven onder de **afzonderlijke inschrijvingen** tabblad. 

        ![Afzonderlijke inschrijving in de portal](./media/how-to-manage-enrollments/individual-enrollment.png)


## <a name="update-an-enrollment-entry"></a>Bijwerken van een vermelding voor inschrijving
U kunt een bestaande vermelding van de inschrijving in de portal met de volgende stappen kunt bijwerken.

1. Open uw service apparaten inrichten in de Azure portal en klik op **inschrijvingen beheren**. 
2. Navigeer naar de inschrijving vermelding die u wilt wijzigen. Klik op het item, een samenvattingsinformatie over de inschrijving van uw apparaten te openen. 
3. Op deze pagina kunt u items dan het beveiligingstype en referenties, zoals iothub het apparaat moeten zijn gekoppeld aan, evenals de apparaat-ID. U kunt ook de status van het eerste apparaat twin wijzigen. 
4. Wanneer het voltooid, klikt u op **opslaan** bijwerken van de inschrijving van uw apparaten. 

    ![Inschrijving in de portal bijwerken](./media/how-to-manage-enrollments/update-enrollment.png)


## <a name="remove-a-device-enrollment"></a>De apparaatinschrijving van een verwijderen
In gevallen waarin uw apparaten hoeft te worden ingericht met een IoT-hub, kunt u de gerelateerde inschrijving vermelding in de portal met de volgende stappen uit.

1. Open uw service apparaten inrichten in de Azure portal en klik op **inschrijvingen beheren**. 
2. Navigeer naar en selecteer de inschrijving invoer die u wilt verwijderen. 
3. Klik op de **verwijderen** knop aan de bovenkant en selecteer vervolgens **Ja** om te bevestigen. 
5. Zodra de actie is voltooid, ziet u de vermelding is verwijderd uit de lijst met apparaatinschrijvingen. 
 
    ![Registratie verwijderen in de portal](./media/how-to-manage-enrollments/remove-enrollment.png)



