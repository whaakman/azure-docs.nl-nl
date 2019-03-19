---
title: Apparaatregistraties beheren met Azure portal | Microsoft Docs
description: Het apparaatregistraties voor uw Device Provisioning Service in de Azure-Portal beheren
author: wesmc7777
ms.author: wesmc
ms.date: 04/05/2018
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: timlt
ms.openlocfilehash: 51b072bfd0827528a5504133dff8c1cdd7a7ca86
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58089383"
---
# <a name="how-to-manage-device-enrollments-with-azure-portal"></a>Het apparaatregistraties beheren met Azure Portal

Een *apparaatinschrijving* maakt een record van een enkel apparaat of een groep apparaten dat kan op een bepaald moment worden geregistreerd met de Azure IoT Hub Device Provisioning Service. De inschrijvingsrecord bevat de gewenste beginconfiguratie voor de apparaten als onderdeel van dat de inschrijving, met inbegrip van de gewenste IoT-hub. Dit artikel laat u het apparaatregistraties voor de inrichtingsservice beheren.


## <a name="create-a-device-enrollment"></a>Maken van een apparaatinschrijving

Er zijn twee manieren waarop u uw apparaten met de provisioning-service kunt inschrijven:

* Een **registratiegroep** een vermelding voor een groep van apparaten die delen een gemeenschappelijke attestation-mechanisme van X.509-certificaten, ondertekend door de dezelfde handtekeningcertificaat, dit kan de [basiscertificaat](https://docs.microsoft.com/azure/iot-dps/concepts-security#root-certificate) of de [tussencertificaat](https://docs.microsoft.com/azure/iot-dps/concepts-security#intermediate-certificate), die wordt gebruikt voor het produceren van certificaat voor apparaten op het fysieke apparaat. Wordt u aangeraden een registratiegroep voor een groot aantal apparaten die een gewenste initiële configuratie delen, of voor apparaten die allemaal zijn verbonden aan dezelfde tenant. Houd er rekening mee dat u alleen de apparaten die gebruikmaken van het x.509-attestation-mechanisme als kunt inschrijven *Registratiegroepen*. 

    U kunt een registratiegroep maken in de portal voor een groep van apparaten met behulp van de volgende stappen uit:

  1. Meld u aan bij de Azure-portal en klikt u op **alle resources** in het menu links.  
  1. Klik op de Device Provisioning service die u wilt uw apparaat inschrijven bij in de lijst met resources.  
  1. In de provisioning-service:  
     a. Klik op **registraties beheren**en selecteer vervolgens de **Registratiegroepen** tabblad.  
     b. Klik bovenaan op de knop **Toevoegen**.  
     c. Wanneer het paneel 'Registratiegroep toevoegen' wordt weergegeven, voert u de gegevens voor de inschrijving van de lijst.  **Naam van groep** is vereist. Ook selecteren 'CA of tussenliggende' voor **certificaattype**, en upload de hoofdmap **primair certificaat** voor de groep van apparaten.  
     d. Klik op **Opslaan**. Op de registratiegroep is gemaakt, ziet u de naam van de groep worden weergegeven onder de **Registratiegroepen** tabblad.  

     [![Registratiegroep zit in de portal](./media/how-to-manage-enrollments/group-enrollment.png)](./media/how-to-manage-enrollments/group-enrollment.png#lightbox)
    

* Een **afzonderlijke inschrijving** een vermelding voor een enkel apparaat dat kan worden geregistreerd. Afzonderlijke inschrijvingen kunnen beide x509 gebruiken certificaten of SAS-tokens (van een fysiek of virtueel TPM) als attestation-mechanismen. Het is raadzaam om afzonderlijke inschrijvingen te gebruiken voor apparaten die unieke initiële configuraties vereisen, of voor apparaten die u kunnen alleen SAS-tokens via TPM of virtuele TPM als attestation-mechanisme. Afzonderlijke inschrijvingen hebben mogelijk de gewenste apparaat-id voor IoT Hub die is opgegeven.

    U kunt een afzonderlijke inschrijving maken in de portal met de volgende stappen uit:

    1. Meld u aan bij de Azure-portal en klikt u op **alle resources** in het menu links.
    1. Klik op de Device Provisioning service die u wilt uw apparaat inschrijven bij in de lijst met resources.
    1. In de provisioning-service:  
       a. Klik op **registraties beheren**en selecteer vervolgens de **afzonderlijke inschrijvingen** tabblad.  
       b. Klik bovenaan op de knop **Toevoegen**.   
       c. Wanneer het paneel 'Inschrijving toevoegen' wordt weergegeven, voert u de gegevens voor de inschrijving van de lijst. Selecteer eerst de attestation **mechanisme** voor het apparaat (X.509 of TPM). X.509-attestation, moet u voor het uploaden van het leaf **primair certificaat** voor het apparaat. TPM, moet u opgeven de **Attestation-sleutel** en **registratie-ID** voor het apparaat.  
       d. Klik op **Opslaan**. Op de registratiegroep is gemaakt, ziet u het apparaat worden weergegeven onder de **afzonderlijke inschrijvingen** tabblad.  

       [![Afzonderlijke inschrijving in de portal](./media/how-to-manage-enrollments/individual-enrollment.png)](./media/how-to-manage-enrollments/individual-enrollment.png#lightbox)

## <a name="update-an-enrollment-entry"></a>Bijwerken van een vermelding voor apparaatinschrijving
U kunt een bestaande vermelding voor apparaatinschrijving in de portal met de volgende stappen uit bijwerken:

1. Open uw Device Provisioning service in Azure portal en klik op **inschrijvingen beheren**. 
1. Navigeer naar de inschrijvingsvermelding die u wilt wijzigen. Klik op de vermelding, een samenvattende informatie over de registratie van uw apparaat te openen. 
1. Op deze pagina kunt u items dan het beveiligingstype en referenties, zoals de IoT-hub het apparaat moeten zijn gekoppeld aan, evenals de apparaat-ID. U kunt ook de oorspronkelijke apparaatdubbelstatus wijzigen. 
1. Wanneer voltooid, klikt u op **opslaan** registratie van uw apparaat bijwerken. 

    ![Update-inschrijving in de portal](./media/how-to-manage-enrollments/update-enrollment.png)

## <a name="remove-a-device-enrollment"></a>De apparaatregistratie van een verwijderen
In gevallen waarin uw apparaten hoeft te worden ingericht op een IoT-hub, verwijdert u de gerelateerde inschrijvingsvermelding in de portal met de volgende stappen uit:

1. Open uw Device Provisioning service in Azure portal en klik op **inschrijvingen beheren**. 
1. Navigeer naar en selecteer de inschrijvingsvermelding die u wilt verwijderen. 
1. Klik op de **verwijderen** knop bovenaan en selecteer vervolgens **Ja** wanneer hierom wordt gevraagd om te bevestigen. 
1. Zodra de actie is voltooid, ziet u de vermelding is verwijderd uit de lijst met het apparaatregistraties. 
 
    ![Registratie verwijderen in de portal](./media/how-to-manage-enrollments/remove-enrollment.png)


