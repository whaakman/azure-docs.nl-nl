---
title: Beheert de apparaatinschrijvingen met Azure portal | Microsoft Docs
description: Het beheren van apparaatinschrijvingen voor uw service DP's in de Azure Portal
author: dsk-2015
ms.author: dkshir
ms.date: 04/05/2018
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: timlt
ms.openlocfilehash: b13f74e0c3df5090d1b1b2e0c48e3dc612821250
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34628433"
---
# <a name="how-to-manage-device-enrollments-with-azure-portal"></a>Het beheren van apparaatinschrijvingen met Azure-Portal

Een *apparaatinschrijving* maakt een record van een enkel apparaat of een groep apparaten die mogelijk op een bepaald moment geregistreerd bij de Azure IoT Hub apparaat inrichtingsservice. De record van inschrijving bevat de eerste gewenste configuratie voor de apparaten als onderdeel van dat de inschrijving, met inbegrip van de gewenste IoT-hub. In dit artikel leest u hoe voor het beheren van apparaatinschrijvingen voor uw provisioning-service.


## <a name="create-a-device-enrollment"></a>Maken van een inschrijving van apparaten

Er zijn twee manieren waarop u uw apparaten met de inrichting service kunt inschrijven:

* Een **inschrijvingsgroep** is een vermelding voor een groep apparaten die delen een gemeenschappelijke attestation-mechanisme van X.509-certificaten, ondertekend door de dezelfde handtekeningcertificaat, wat kan de [basiscertificaat](https://docs.microsoft.com/azure/iot-dps/concepts-security#root-certificate) of de [tussenliggende certificaat](https://docs.microsoft.com/azure/iot-dps/concepts-security#intermediate-certificate)die worden gebruikt voor het produceren van certificaat voor apparaten op het fysieke apparaat. Aangeraden wordt een inschrijvingsgroep voor voor een groot aantal apparaten met een gewenste initiële configuratie delen of voor apparaten alle gaat dezelfde tenant. Houd er rekening mee dat u alleen de apparaten die gebruikmaken van het mechanisme voor X.509 attestation als kan inschrijven *inschrijving groepen*. 

    U kunt een registratie-groep maken in de portal voor een groep van apparaten met behulp van de volgende stappen uit:

    1. Aanmelden bij de Azure-portal en klikt u op **alle resources** uit in het menu links.  
    2. Klik op de apparaten inrichten service die u wilt inschrijven van uw apparaat uit de lijst met resources.  
    3. In uw provisioning-service:  
       a. Klik op **inschrijvingen beheren**, selecteer vervolgens de **inschrijving groepen** tabblad.  
       b. Klik bovenaan op de knop **Toevoegen**.  
       c. Wanneer het paneel 'Inschrijving groep toevoegen' wordt weergegeven, voer de gegevens voor de vermelding van de lijst met inschrijving.  **Groepsnaam** is vereist. Selecteer ook 'CA of tussenliggende' voor **certificaattype**, en de hoofdmap uploaden **primaire certificaat voor** voor de groep van apparaten.  
       d. Klik op **Opslaan**. Op uw registratie-groep is gemaakt, ziet u de naam van de groep worden weergegeven onder de **inschrijving groepen** tabblad.  

       [![Registratie-groep in de portal](./media/how-to-manage-enrollments/group-enrollment.png)] (. / media/how-to-manage-enrollments/group-enrollment.png#lightbox)
    

* Een **afzonderlijke inschrijving** is een vermelding voor één apparaat kan registreren. Afzonderlijke inschrijvingen mag gebruiken beide x509 certificaten of SAS-tokens (van een fysieke of virtuele TPM) als de attestation-mechanismen. U wordt aangeraden met behulp van afzonderlijke inschrijvingen voor apparaten die unieke initiële configuraties vereisen of voor apparaten die u kunnen alleen SAS-tokens via TPM of virtuele TPM gebruiken als het mechanisme voor attestation. Afzonderlijke inschrijvingen hebben mogelijk de gewenste apparaat-id voor IoT Hub die is opgegeven.

    U kunt een afzonderlijke inschrijving maken in de portal met de volgende stappen uit:

    1. Aanmelden bij de Azure-portal en klikt u op **alle resources** uit in het menu links.
    2. Klik op de apparaten inrichten service die u wilt inschrijven van uw apparaat uit de lijst met resources.
    3. In uw provisioning-service:  
       a. Klik op **inschrijvingen beheren**, selecteer vervolgens de **afzonderlijke inschrijvingen** tabblad.  
       b. Klik bovenaan op de knop **Toevoegen**.   
       c. Wanneer het paneel 'Inschrijving toevoegen' wordt weergegeven, voer de gegevens voor de vermelding van de lijst met inschrijving. Selecteer eerst de attestation **mechanisme** voor het apparaat (X.509 of TPM). X.509-verklaring vereist dat u voor het uploaden van de leaf **primaire certificaat voor** voor het apparaat. TPM vereist, geeft u de **Attestation-sleutel** en **registratie-ID** voor het apparaat.  
       d. Klik op **Opslaan**. Op uw registratie-groep is gemaakt, ziet u het apparaat worden weergegeven onder de **afzonderlijke inschrijvingen** tabblad.  

       [![Afzonderlijke inschrijving in de portal](./media/how-to-manage-enrollments/individual-enrollment.png)](./media/how-to-manage-enrollments/individual-enrollment.png#lightbox)

## <a name="update-an-enrollment-entry"></a>Bijwerken van een vermelding voor inschrijving
U kunt een bestaande vermelding van de inschrijving in de portal met de volgende stappen bijwerken:

1. Open uw service apparaten inrichten in de Azure portal en klik op **inschrijvingen beheren**. 
2. Navigeer naar de inschrijving vermelding die u wilt wijzigen. Klik op het item, een samenvattingsinformatie over de inschrijving van uw apparaten te openen. 
3. Op deze pagina kunt u items dan het beveiligingstype en referenties, zoals iothub het apparaat moeten zijn gekoppeld aan, evenals de apparaat-ID. U kunt ook de status van het eerste apparaat twin wijzigen. 
4. Wanneer het voltooid, klikt u op **opslaan** bijwerken van de inschrijving van uw apparaten. 

    ![Inschrijving in de portal bijwerken](./media/how-to-manage-enrollments/update-enrollment.png)

## <a name="remove-a-device-enrollment"></a>De apparaatinschrijving van een verwijderen
In gevallen waarin uw apparaten hoeft te worden ingericht met een IoT-hub, kunt u de gerelateerde inschrijving vermelding in de portal met de volgende stappen uit:

1. Open uw service apparaten inrichten in de Azure portal en klik op **inschrijvingen beheren**. 
2. Navigeer naar en selecteer de inschrijving invoer die u wilt verwijderen. 
3. Klik op de **verwijderen** knop aan de bovenkant en selecteer vervolgens **Ja** om te bevestigen. 
5. Zodra de actie is voltooid, ziet u de vermelding is verwijderd uit de lijst met apparaatinschrijvingen. 
 
    ![Registratie verwijderen in de portal](./media/how-to-manage-enrollments/remove-enrollment.png)


