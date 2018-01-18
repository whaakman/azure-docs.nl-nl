---
title: Beheert de apparaatinschrijvingen met Azure apparaat inrichten Service-SDK's | Microsoft Docs
description: Apparaatinschrijvingen in de IoT Hub apparaat inrichtingsservice met de Service-SDK's beheren
services: iot-dps
keywords: 
author: yzhong94
ms.author: yizhon
ms.date: 12/01/2017
ms.topic: article
ms.service: iot-dps
documentationcenter: 
manager: arjmands
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 14e353af82342bc7a580e1a0a02b8b4e29514fb9
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2018
---
# <a name="how-to-manage-device-enrollments-with-azure-device-provisioning-service-sdks"></a>Apparaatinschrijvingen met Azure apparaat inrichten Service-SDK's beheren
Een *apparaatinschrijving* maakt een record van een enkel apparaat of een groep apparaten die mogelijk op een bepaald moment geregistreerd bij de Service voor het inrichten van apparaten. De record van inschrijving bevat de eerste gewenste configuratie voor de apparaten als onderdeel van dat de inschrijving, met inbegrip van de gewenste IoT-hub. In dit artikel laat zien hoe apparaatinschrijvingen voor uw provisioning service programmatisch met behulp van de Azure IoT inrichting Service SDK's beheren.  De SDK's zijn beschikbaar op GitHub in dezelfde opslagplaats als Azure IoT SDK's.

## <a name="samples"></a>Voorbeelden
In dit artikel controleert de concepten van hoog niveau voor het beheren van apparaatinschrijvingen voor uw provisioning service programmatisch met behulp van de Azure IoT inrichting Service SDK's.  Exacte API-aanroepen kunnen afwijken als gevolg van verschillende talen.  Lees de controles die we op GitHub voor meer informatie bieden:
* [Voorbeelden van Java-inrichting Service-Client](https://github.com/Azure/azure-iot-sdk-java/tree/master/provisioning/provisioning-samples)
* [Voorbeelden van node.js-Client-Service-inrichting](https://github.com/Azure/azure-iot-sdk-node/tree/master/provisioning/service/samples)
* [Voorbeelden van de Client-Service van .NET-inrichting](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/provisioning/service/samples)

## <a name="prerequisites"></a>Vereisten
* Verbindingsreeks uit een apparaat inrichtingsservice-exemplaar
* Apparaat beveiliging artefacten:
    * [**TPM**](https://docs.microsoft.com/azure/iot-dps/concepts-security):
        * Afzonderlijke inschrijving: registratie-ID en TPM-goedkeuringssleutel vanaf een fysiek apparaat of van TPM-Simulator.
        * Inschrijvingsgroep geldt niet voor attestation TPM.
    * [**X.509**](https://docs.microsoft.com/azure/iot-dps/concepts-security):
        * Afzonderlijke inschrijving: de [Leaf certificaat](https://docs.microsoft.com/azure/iot-dps/concepts-security#leaf-certificate) van fysieke apparaat of Emulator OPDELEN.
        * Inschrijvingsgroep: de [basiscertificaat](https://docs.microsoft.com/azure/iot-dps/concepts-security#root-certificate) of de [tussenliggende certificaat](https://docs.microsoft.com/azure/iot-dps/concepts-security#intermediate-certificate)die worden gebruikt voor het produceren van certificaat voor apparaten op een fysiek apparaat.  Het kan ook worden gegenereerd van de Emulator OPDELEN.

## <a name="create-a-device-enrollment"></a>Maken van een inschrijving van apparaten

Er zijn twee manieren waarop u uw apparaten met de inrichting service kunt inschrijven:

* Een **inschrijvingsgroep** is een vermelding voor een groep apparaten die een gemeenschappelijke attestation-mechanisme van X.509-certificaten delen, ondertekend door de [basiscertificaat](https://docs.microsoft.com/azure/iot-dps/concepts-security#root-certificate) of de [tussenliggende certificaat ](https://docs.microsoft.com/azure/iot-dps/concepts-security#intermediate-certificate). Aangeraden wordt een inschrijvingsgroep voor voor een groot aantal apparaten met een gewenste initiële configuratie delen of voor apparaten alle gaat dezelfde tenant. Houd er rekening mee dat u alleen de apparaten die gebruikmaken van het mechanisme voor X.509 attestation als kan inschrijven *inschrijving groepen*. 

    U kunt een registratie-groep maken met de SDK's die deze werkstroom te volgen:

    1. Het mechanisme voor attestation maakt gebruik van X.509-basiscertificaat voor inschrijvingsgroep.  Service-SDK-API aanroepen ```X509Attestation.createFromRootCertificate``` met basiscertificaat voor het maken van de Attestation-bewerking voor de inschrijving.  Basis-X.509-certificaat is opgegeven in een PEM-bestand of een tekenreeks.
    1. Maak een nieuwe ```EnrollmentGroup``` met behulp van variabele de ```attestation``` gemaakt en een unieke ```enrollmentGroupId```.  U kunt eventueel parameters zoals instellen ```Device ID```, ```IoTHubHostName```, ```ProvisioningStatus```.
    2. Service-SDK-API aanroepen ```createOrUpdateEnrollmentGroup``` in uw back-end-toepassing met ```EnrollmentGroup``` om een registratie-groep te maken.

* Een **afzonderlijke inschrijving** is een vermelding voor één apparaat kan registreren. Afzonderlijke inschrijvingen mag x.509-certificaten of SAS-tokens (in een echt of virtueel TPM) gebruiken zoals attestation mechanismen. U wordt aangeraden met behulp van afzonderlijke inschrijvingen voor apparaten die unieke initiële configuraties vereisen of voor apparaten die u kunnen alleen SAS-tokens via TPM of virtuele TPM gebruiken als het mechanisme voor attestation. Afzonderlijke inschrijvingen wellicht de gewenste IoT hub apparaat-ID opgegeven.

    U kunt een afzonderlijke inschrijving maken met de SDK's die deze werkstroom te volgen:
    
    1. Kies uw ```attestation``` mechanisme, kan dit TPM of X.509.
        1. **TPM**: de goedkeuringssleutel vanaf een fysiek apparaat of van TPM Simulator gebruikt als invoer, kunt u de SDK-API-Service aanroepen ```TpmAttestation``` attestation voor inschrijving te maken. 
        2. **X.509**: het clientcertificaat als invoer gebruikt, kunt u Service SDK-API aanroepen ```X509Attestation.createFromClientCertificate``` attestation voor inschrijving te maken.
    2. Maak een nieuwe ```IndividualEnrollment``` variabele met behulp van de ```attestation``` gemaakt en een unieke ```registrationId``` als invoer, die op uw apparaat of gegenereerd op basis van de TPM-Simulator.  U kunt eventueel parameters zoals instellen ```Device ID```, ```IoTHubHostName```, ```ProvisioningStatus```.
    3. Service-SDK-API aanroepen ```createOrUpdateIndividualEnrollment``` in uw back-end-toepassing met ```IndividualEnrollment``` voor het maken van een afzonderlijke inschrijving.

Nadat u een inschrijving hebt gemaakt, zou de inrichtingsservice van het apparaat een registratie-resultaat retourneren.

Deze werkstroom wordt geïllustreerd in de [voorbeelden](#samples).

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

Deze werkstroom wordt geïllustreerd in de [voorbeelden](#samples).

## <a name="remove-an-enrollment-entry"></a>Verwijder de vermelding voor een inschrijving

* **Afzonderlijke inschrijving** kan worden verwijderd door het aanroepen van de SDK-API-Service ```deleteIndividualEnrollment``` met ```registrationId```.
* **Registratie van de groep** kan worden verwijderd door het aanroepen van de SDK-API-Service ```deleteEnrollmentGroup``` met ```enrollmentGroupId```.

Deze werkstroom wordt geïllustreerd in de [voorbeelden](#samples).

## <a name="bulk-operation-on-individual-enrollments"></a>Bulkbewerking op afzonderlijke inschrijvingen

U kunt bulkbewerking te maken, bijwerken of verwijderen van meerdere afzonderlijke inschrijvingen na deze werkstroom uitvoeren:

1. Maak een variabele met meerdere bevat ```IndividualEnrollment```.  Implementatie van deze variabele is verschillend voor elke taal.  Bekijk de bulksgewijze bewerking voorbeeld op GitHub voor meer informatie.
2. Service-SDK-API aanroepen ```runBulkOperation``` met een ```BulkOperationMode``` voor bewerking en de variabele voor afzonderlijke inschrijvingen. Vier modi worden ondersteund: maken, bijwerken, updateIfMatchEtag, en verwijderen.

Nadat u hebt een bewerking is uitgevoerd, zou de Service voor het inrichten van apparaten bulksgewijs bewerkingsresultaat geretourneerd.

Deze werkstroom wordt geïllustreerd in de [voorbeelden](#samples).
