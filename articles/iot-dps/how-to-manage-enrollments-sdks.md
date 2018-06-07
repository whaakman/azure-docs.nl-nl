---
title: Beheert de apparaatinschrijvingen met behulp van Azure apparaat inrichten Service-SDK's | Microsoft Docs
description: Apparaatinschrijvingen in de IoT Hub apparaat inrichten met behulp van de Service-SDK's beheren
author: yzhong94
ms.author: yizhon
ms.date: 04/04/18
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: arjmands
ms.openlocfilehash: 9a68d928c70e1e233f6de7df13441a1f688f456a
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34629844"
---
# <a name="how-to-manage-device-enrollments-with-azure-device-provisioning-service-sdks"></a>Apparaatinschrijvingen met Azure apparaat inrichten Service-SDK's beheren
Een *apparaatinschrijving* maakt een record van een enkel apparaat of een groep apparaten die mogelijk op een bepaald moment geregistreerd bij de Service voor het inrichten van apparaten. De record van inschrijving bevat de eerste gewenste configuratie voor de apparaten als onderdeel van dat de inschrijving, met inbegrip van de gewenste IoT-hub. In dit artikel laat zien hoe apparaatinschrijvingen voor uw provisioning service programmatisch met behulp van de Azure IoT inrichting Service SDK's beheren.  De SDK's zijn beschikbaar op GitHub in dezelfde opslagplaats als Azure IoT SDK's.

## <a name="prerequisites"></a>Vereisten
* De verbindingsreeks ophalen met uw apparaat inrichtingsservice-exemplaar.
* Verkrijgen van het apparaat artefacten beveiliging voor de [attestation mechanisme](concepts-security.md#attestation-mechanism) gebruikt:
    * [**Trusted Platform Module (TPM)**](/azure/iot-dps/concepts-security#trusted-platform-module):
        * Afzonderlijke inschrijving: registratie-ID en TPM-goedkeuringssleutel vanaf een fysiek apparaat of van TPM-Simulator.
        * Inschrijvingsgroep geldt niet voor attestation TPM.
    * [**X.509**](/azure/iot-dps/concepts-security):
        * Afzonderlijke inschrijving: de [Leaf certificaat](/azure/iot-dps/concepts-security#leaf-certificate) van fysieke apparaat of van de SDK [OPDELEN](https://azure.microsoft.com/blog/azure-iot-supports-new-security-hardware-to-strengthen-iot-security/) Emulator.
        * Inschrijvingsgroep: de [basis-CA-certificaat](/azure/iot-dps/concepts-security#root-certificate) of de [tussenliggende certificaat](/azure/iot-dps/concepts-security#intermediate-certificate)die worden gebruikt voor het produceren van certificaat voor apparaten op een fysiek apparaat.  Het kan ook worden gegenereerd van de emulator SDK OPDELEN.
* Exacte API-aanroepen kunnen afwijken als gevolg van verschillende talen. Lees de voorbeelden op GitHub voor meer informatie:
   * [Voorbeelden van Java-inrichting Service-Client](https://github.com/Azure/azure-iot-sdk-java/tree/master/provisioning/provisioning-samples)
   * [Voorbeelden van node.js-Client-Service-inrichting](https://github.com/Azure/azure-iot-sdk-node/tree/master/provisioning/service/samples)
   * [Voorbeelden van de Client-Service van .NET-inrichting](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/provisioning/service/samples)

## <a name="create-a-device-enrollment"></a>Maken van een inschrijving van apparaten
Er zijn twee manieren waarop u uw apparaten met de inrichting service kunt inschrijven:

* Een **inschrijvingsgroep** is een vermelding voor een groep apparaten die een gemeenschappelijke attestation-mechanisme van X.509-certificaten delen, ondertekend door de [basiscertificaat](https://docs.microsoft.com/azure/iot-dps/concepts-security#root-certificate) of de [tussenliggende certificaat ](https://docs.microsoft.com/azure/iot-dps/concepts-security#intermediate-certificate). Aangeraden wordt een inschrijvingsgroep voor voor een groot aantal apparaten die een gewenste initiële configuratie delen of voor apparaten alle gaat dezelfde tenant. Houd er rekening mee dat u alleen de apparaten die gebruikmaken van het mechanisme voor X.509 attestation als kan inschrijven *inschrijving groepen*. 

    U kunt een registratie-groep maken met de SDK's die deze werkstroom te volgen:

    1. Het mechanisme voor attestation maakt gebruik van X.509-basiscertificaat voor inschrijvingsgroep.  Service-SDK-API aanroepen ```X509Attestation.createFromRootCertificate``` met basiscertificaat voor het maken van de Attestation-bewerking voor de inschrijving.  Basis-X.509-certificaat is opgegeven in een PEM-bestand of een tekenreeks.
    1. Maak een nieuwe ```EnrollmentGroup``` met behulp van variabele de ```attestation``` gemaakt en een unieke ```enrollmentGroupId```.  U kunt eventueel parameters zoals instellen ```Device ID```, ```IoTHubHostName```, ```ProvisioningStatus```.
    2. Service-SDK-API aanroepen ```createOrUpdateEnrollmentGroup``` in uw back-end-toepassing met ```EnrollmentGroup``` om een registratie-groep te maken.

* Een **afzonderlijke inschrijving** is een vermelding voor één apparaat kan registreren. Afzonderlijke inschrijvingen mag x.509-certificaten of SAS-tokens (van een fysieke of virtuele TPM) gebruiken zoals attestation mechanismen. U wordt aangeraden met behulp van afzonderlijke inschrijvingen voor apparaten die unieke initiële configuraties vereisen of voor apparaten die u kunnen alleen SAS-tokens via TPM of virtuele TPM gebruiken als het mechanisme voor attestation. Afzonderlijke inschrijvingen hebben mogelijk de gewenste apparaat-id voor IoT Hub die is opgegeven.

    U kunt een afzonderlijke inschrijving maken met de SDK's die deze werkstroom te volgen:
    
    1. Kies uw ```attestation``` mechanisme, kan dit TPM of X.509.
        1. **TPM**: de goedkeuringssleutel vanaf een fysiek apparaat of van TPM Simulator gebruikt als invoer, kunt u de SDK-API-Service aanroepen ```TpmAttestation``` attestation voor inschrijving te maken. 
        2. **X.509**: het clientcertificaat als invoer gebruikt, kunt u Service SDK-API aanroepen ```X509Attestation.createFromClientCertificate``` attestation voor inschrijving te maken.
    2. Maak een nieuwe ```IndividualEnrollment``` variabele met behulp van de ```attestation``` gemaakt en een unieke ```registrationId``` als invoer, die op uw apparaat of gegenereerd op basis van de TPM-Simulator.  U kunt eventueel parameters zoals instellen ```Device ID```, ```IoTHubHostName```, ```ProvisioningStatus```.
    3. Service-SDK-API aanroepen ```createOrUpdateIndividualEnrollment``` in uw back-end-toepassing met ```IndividualEnrollment``` voor het maken van een afzonderlijke inschrijving.

Nadat u een inschrijving hebt gemaakt, retourneert de inrichtingsservice van het apparaat een resultaat van de inschrijving. Deze werkstroom wordt geïllustreerd in de voorbeelden [eerder vermeld](#prerequisites).

## <a name="update-an-enrollment-entry"></a>Bijwerken van een vermelding voor inschrijving

Nadat u een registratie-vermelding hebt gemaakt, kunt u de registratie bijwerken.  Mogelijke scenario's omvatten bijwerken van de gewenste eigenschap, het bijwerken van de attestation-methode of het Apparaattoegang intrekken.  Er zijn verschillende API's voor afzonderlijke inschrijving en groepsinschrijving, maar er wordt geen onderscheid voor attestation-mechanisme.

U kunt een vermelding in het volgen van deze werkstroom registratie bijwerken:
* **Afzonderlijke inschrijving**:
    1. De meest recente inschrijving ophalen van de inrichting service eerst met de SDK-API-Service ```getIndividualEnrollment```.
    2. De parameter van de meest recente inschrijving zo nodig wijzigen. 
    3. Met behulp van de meest recente inschrijving Service SDK-API niet aanroepen ```createOrUpdateIndividualEnrollment``` bij te werken uw registratie.
* **Registratie van de groep**:
    1. De meest recente inschrijving ophalen van de inrichting service eerst met de SDK-API-Service ```getEnrollmentGroup```.
    2. De parameter van de meest recente inschrijving zo nodig wijzigen.
    3. Met behulp van de meest recente inschrijving Service SDK-API niet aanroepen ```createOrUpdateEnrollmentGroup``` bij te werken uw registratie.

Deze werkstroom wordt geïllustreerd in de voorbeelden [eerder vermeld](#prerequisites).

## <a name="remove-an-enrollment-entry"></a>Verwijder de vermelding voor een inschrijving

* **Afzonderlijke inschrijving** kan worden verwijderd door het aanroepen van de SDK-API-Service ```deleteIndividualEnrollment``` met ```registrationId```.
* **Registratie van de groep** kan worden verwijderd door het aanroepen van de SDK-API-Service ```deleteEnrollmentGroup``` met ```enrollmentGroupId```.

Deze werkstroom wordt geïllustreerd in de voorbeelden [eerder vermeld](#prerequisites).

## <a name="bulk-operation-on-individual-enrollments"></a>Bulkbewerking op afzonderlijke inschrijvingen

U kunt bulkbewerking te maken, bijwerken of verwijderen van meerdere afzonderlijke inschrijvingen na deze werkstroom uitvoeren:

1. Maak een variabele met meerdere bevat ```IndividualEnrollment```.  Implementatie van deze variabele is verschillend voor elke taal.  Bekijk de bulksgewijze bewerking voorbeeld op GitHub voor meer informatie.
2. Service-SDK-API aanroepen ```runBulkOperation``` met een ```BulkOperationMode``` voor bewerking en de variabele voor afzonderlijke inschrijvingen. Vier modi worden ondersteund: maken, bijwerken, updateIfMatchEtag, en verwijderen.

Nadat u hebt een bewerking is uitgevoerd, zou de Service voor het inrichten van apparaten bulksgewijs bewerkingsresultaat geretourneerd.

Deze werkstroom wordt geïllustreerd in de voorbeelden [eerder vermeld](#prerequisites).
