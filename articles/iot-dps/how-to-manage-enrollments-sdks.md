---
title: Met behulp van Azure Device Provisioning Service-SDK's apparaatregistraties beheren | Microsoft Docs
description: Het apparaatregistraties in de IoT Hub Device Provisioning Service met behulp van de Service-SDK's beheren
author: yzhong94
ms.author: yizhon
ms.date: 04/04/2018
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: arjmands
ms.openlocfilehash: c73a40e46d86632732454ae16ea4f83e3ffa0281
ms.sourcegitcommit: 8ca6cbe08fa1ea3e5cdcd46c217cfdf17f7ca5a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/22/2019
ms.locfileid: "56674749"
---
# <a name="how-to-manage-device-enrollments-with-azure-device-provisioning-service-sdks"></a>Het apparaatregistraties beheren met Azure Device Provisioning Service-SDK 's
Een *apparaatinschrijving* maakt een record van een enkel apparaat of een groep apparaten dat kan op een bepaald moment worden geregistreerd met de Device Provisioning Service. De inschrijvingsrecord bevat de gewenste beginconfiguratie voor de apparaten als onderdeel van dat de inschrijving, met inbegrip van de gewenste IoT-hub. Dit artikel laat u het apparaatregistraties voor uw provisioning-service programmatisch met behulp van de Azure IoT Provisioning Service-SDK's beheren.  De SDK's zijn beschikbaar op GitHub in dezelfde opslagplaats bevindt als Azure IoT SDK's.

## <a name="prerequisites"></a>Vereisten
* De verbindingsreeks ophalen uit uw Device Provisioning Service-exemplaar.
* Verkrijgen van het apparaat beveiligingsartefacten voor de [attestation-mechanisme](concepts-security.md#attestation-mechanism) gebruikt:
    * [**Trusted Platform Module (TPM)**](/azure/iot-dps/concepts-security#trusted-platform-module):
        * Afzonderlijke inschrijving: Registratie-ID en TPM-goedkeuringssleutel vanaf een fysiek apparaat of van TPM-Simulator.
        * Registratiegroep geldt niet voor TPM-attestation.
    * [**X.509**](/azure/iot-dps/concepts-security):
        * Afzonderlijke inschrijving: De [Leaf-certificaat](/azure/iot-dps/concepts-security) van fysiek apparaat of van de SDK [DICE](https://azure.microsoft.com/blog/azure-iot-supports-new-security-hardware-to-strengthen-iot-security/) Emulator.
        * Registratiegroep: De [basis-CA-certificaat](/azure/iot-dps/concepts-security#root-certificate) of de [tussencertificaat](/azure/iot-dps/concepts-security#intermediate-certificate), die wordt gebruikt voor het produceren van certificaat voor apparaten op een fysiek apparaat.  Het kan ook worden gegenereerd van de SDK DICE-emulator.
* Exacte API-aanroepen kunnen afwijken als gevolg van taalverschillen. Raadpleeg de voorbeelden op GitHub voor meer informatie:
   * [Provisioning-serviceclient van Java-voorbeelden](https://github.com/Azure/azure-iot-sdk-java/tree/master/provisioning/provisioning-samples)
   * [Voorbeelden van node.js-Client voor inrichting](https://github.com/Azure/azure-iot-sdk-node/tree/master/provisioning/service/samples)
   * [Voorbeelden van Service-Client voor .NET-inrichting](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/provisioning/service/samples)

## <a name="create-a-device-enrollment"></a>Maken van een apparaatinschrijving
Er zijn twee manieren waarop u uw apparaten met de provisioning-service kunt inschrijven:

* Een **registratiegroep** een vermelding voor een groep van apparaten die een gemeenschappelijke attestation-mechanisme van X.509-certificaten delen, ondertekend door de [basiscertificaat](https://docs.microsoft.com/azure/iot-dps/concepts-security#root-certificate) of de [tussencertificaat ](https://docs.microsoft.com/azure/iot-dps/concepts-security#intermediate-certificate). Wordt u aangeraden een registratiegroep voor een groot aantal apparaten die een gewenste initiële configuratie delen, of voor apparaten die allemaal zijn verbonden aan dezelfde tenant. Houd er rekening mee dat u alleen de apparaten die gebruikmaken van het x.509-attestation-mechanisme als kunt inschrijven *Registratiegroepen*. 

    U kunt een registratiegroep maken met de SDK's voor deze werkstroom te volgen:

    1. Voor een registratiegroep zit gebruikt de attestation-mechanisme X.509-basiscertificaat.  Service-SDK-API aanroepen ```X509Attestation.createFromRootCertificate``` met basiscertificaat te maken van de attestation voor inschrijving.  X.509-basiscertificaat is opgegeven in een PEM-bestand of als een tekenreeks.
    1. Maak een nieuwe ```EnrollmentGroup``` variabele gebruiken de ```attestation``` gemaakt en een unieke ```enrollmentGroupId```.  U kunt eventueel parameters, zoals instellen ```Device ID```, ```IoTHubHostName```, ```ProvisioningStatus```.
    2. Service-SDK-API aanroepen ```createOrUpdateEnrollmentGroup``` in uw back-endtoepassing met ```EnrollmentGroup``` te maken van een registratiegroep zit.

* Een **afzonderlijke inschrijving** een vermelding voor een enkel apparaat dat kan worden geregistreerd. Afzonderlijke inschrijvingen kunnen X.509-certificaten of SAS-tokens (van een fysiek of virtueel TPM) gebruiken als attestation-mechanismen. Het is raadzaam om afzonderlijke inschrijvingen te gebruiken voor apparaten die unieke initiële configuraties vereisen, of voor apparaten die u kunnen alleen SAS-tokens via TPM of virtuele TPM als attestation-mechanisme. Afzonderlijke inschrijvingen hebben mogelijk de gewenste apparaat-id voor IoT Hub die is opgegeven.

    U kunt een afzonderlijke inschrijving maken met de SDK's voor deze werkstroom te volgen:
    
    1. Kies uw ```attestation``` mechanisme, dit kan TPM of X.509.
        1. **TPM**: Met behulp van de goedkeuringssleutel vanaf een fysiek apparaat of van TPM-Simulator als invoer, kunt u Service-SDK-API aanroepen ```TpmAttestation``` attestation voor inschrijving te maken. 
        2. **X.509**: Het clientcertificaat gebruikt als invoer, kunt u Service-SDK-API aanroepen ```X509Attestation.createFromClientCertificate``` attestation voor inschrijving te maken.
    2. Maak een nieuwe ```IndividualEnrollment``` variabele bij het gebruik van de ```attestation``` gemaakt en een unieke ```registrationId``` als invoer, die op uw apparaat is of gegenereerd op basis van de TPM-Simulator.  U kunt eventueel parameters, zoals instellen ```Device ID```, ```IoTHubHostName```, ```ProvisioningStatus```.
    3. Service-SDK-API aanroepen ```createOrUpdateIndividualEnrollment``` in uw back-endtoepassing met ```IndividualEnrollment``` te maken van een afzonderlijke inschrijving.

Nadat u een inschrijving hebt gemaakt, retourneert de Device Provisioning Service een registratie-resultaat. Deze werkstroom wordt geïllustreerd in de voorbeelden [eerder is vermeld](#prerequisites).

## <a name="update-an-enrollment-entry"></a>Bijwerken van een vermelding voor apparaatinschrijving

Nadat u een vermelding voor apparaatinschrijving hebt gemaakt, kunt u de inschrijving bijwerken.  Mogelijke scenario's omvatten bijwerken van de gewenste eigenschap, de attestation-methode bijwerken of intrekken van toegang tot het apparaat.  Er zijn verschillende API's voor afzonderlijke inschrijving en groepsregistratie, maar er wordt geen onderscheid voor attestation-mechanisme.

U kunt een vermelding voor apparaatinschrijving volgen van deze werkstroom bijwerken:
* **Afzonderlijke inschrijving**:
    1. De meest recente inschrijving ophalen uit de inrichtingsservice eerste met Service-SDK-API ```getIndividualEnrollment```.
    2. De parameter van de meest recente inschrijving zo nodig wijzigen. 
    3. Met behulp van de meest recente inschrijving Service SDK-API aanroepen ```createOrUpdateIndividualEnrollment``` om bij te werken van de vermelding voor apparaatinschrijving.
* **Groepsregistratie**:
    1. De meest recente inschrijving ophalen uit de inrichtingsservice eerste met Service-SDK-API ```getEnrollmentGroup```.
    2. De parameter van de meest recente inschrijving zo nodig wijzigen.
    3. Met behulp van de meest recente inschrijving Service SDK-API aanroepen ```createOrUpdateEnrollmentGroup``` om bij te werken van de vermelding voor apparaatinschrijving.

Deze werkstroom wordt geïllustreerd in de voorbeelden [eerder is vermeld](#prerequisites).

## <a name="remove-an-enrollment-entry"></a>Verwijderen van een vermelding voor apparaatinschrijving

* **Afzonderlijke inschrijving** kan worden verwijderd door het aanroepen van API-Service SDK ```deleteIndividualEnrollment``` met behulp van ```registrationId```.
* **Groepsregistratie** kan worden verwijderd door het aanroepen van API-Service SDK ```deleteEnrollmentGroup``` met behulp van ```enrollmentGroupId```.

Deze werkstroom wordt geïllustreerd in de voorbeelden [eerder is vermeld](#prerequisites).

## <a name="bulk-operation-on-individual-enrollments"></a>Bulkbewerking op afzonderlijke inschrijvingen

U kunt bulkbewerking als u wilt maken, bijwerken of verwijderen van meerdere afzonderlijke registraties volgen van deze werkstroom kunt uitvoeren:

1. Maak een variabele met meerdere bevat ```IndividualEnrollment```.  Implementatie van deze variabele is verschillend voor elke taal.  Bekijk het voorbeeld van de bulk-bewerking op GitHub voor meer informatie.
2. Service-SDK-API aanroepen ```runBulkOperation``` met een ```BulkOperationMode``` voor de gewenste bewerking en de variabele voor afzonderlijke inschrijvingen. Vier modi worden ondersteund: maken, bijwerken, updateIfMatchEtag, en verwijderen.

Nadat u hebt een bewerking is uitgevoerd, zou de Device Provisioning Service een bulksgewijze bewerkingsresultaat retourneren.

Deze werkstroom wordt geïllustreerd in de voorbeelden [eerder is vermeld](#prerequisites).
