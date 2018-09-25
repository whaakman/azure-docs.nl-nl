---
title: Beveiligingsconcepten in Azure IoT Hub Device Provisioning Service | Microsoft Docs
description: Beschrijving van het inrichten van de concepten die specifiek zijn voor apparaten met Device Provisioning Service en IoT-Hub beveiligen
author: nberdy
ms.author: nberdy
ms.date: 03/30/2018
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: briz
ms.openlocfilehash: 92a30f0754decc3052bf53a64da13325ddc4f954
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46946558"
---
# <a name="iot-hub-device-provisioning-service-security-concepts"></a>Concepten van IoT Hub Device Provisioning Service-beveiliging 

IoT Hub Device Provisioning Service is een helper-service voor IoT Hub die u met zero-touch-apparaatinrichting voor een opgegeven IoT hub configureren. Met de Device Provisioning Service, kunt u [automatisch inrichten](concepts-auto-provisioning.md) miljoenen apparaten op een veilige en schaalbare manier. In dit artikel biedt een overzicht van de *security* concepten betrokken bij het apparaat wordt ingericht. Dit artikel is relevant zijn voor alle personen die betrokken zijn bij het ophalen van een apparaat gereed voor implementatie.

## <a name="attestation-mechanism"></a>Attestation-mechanisme

De attestation-mechanisme is de methode die wordt gebruikt voor het bevestigen van een apparaat-id. De attestation-mechanisme is ook relevant zijn voor de lijst inschrijving, dit geeft aan de inrichtingsservice welke methode voor attestation gebruiken met een bepaald apparaat.

> [!NOTE]
> IoT Hub maakt gebruik van 'verificatieschema' voor een vergelijkbare concept in die service.

Device Provisioning Service ondersteunt de volgende vormen van attestation:
* **X.509-certificaten** op basis van de standaard x.509-certificaat-verificatiestroom.
* **Trusted Platform Module (TPM)** op basis van een nonce uitdaging, met behulp van de TPM-standaard voor sleutels om weer te geven van een ondertekende Shared Access Signature (SAS)-token. Deze vorm van de attestation is niet vereist voor een fysieke TPM op het apparaat, maar de service om te bevestigen met behulp van de goedkeuringssleutel per wordt verwacht dat de [TPM-specificatie](https://trustedcomputinggroup.org/work-groups/trusted-platform-module/).
* **Symmetrische sleutel** op basis van de handtekening voor gedeelde toegang (SAS) [beveiligingstokens](../iot-hub/iot-hub-devguide-security.md#security-tokens), waaronder een hash-handtekening en een ingesloten verlopen. Zie voor meer informatie, [symmetrische sleutel attestation](concepts-symmetric-key-attestation.md).


## <a name="hardware-security-module"></a>Hardware security module

De HSM, of een aangepaste HSM wordt gebruikt voor beveiligde, op hardware gebaseerde opslag van apparaat geheimen en is de veiligste manier van geheime opslag. X.509-certificaten en SAS-tokens kunnen worden opgeslagen in de HSM. HSM's kunnen worden gebruikt in combinatie met beide attestation-mechanismen de provisioning ondersteunt.

> [!TIP]
> Het is raadzaam met behulp van een HSM aan geheimen veilig opslaan op uw apparaten met apparaten.

Apparaat geheimen kunnen ook worden opgeslagen in de software (geheugen), maar er is een minder veilige vorm van opslag dan een HSM.

## <a name="trusted-platform-module"></a>Trusted Platform Module

TPM kan verwijzen naar een standaard voor het veilig opslaan van sleutels die worden gebruikt voor verificatie van het platform of het kan verwijzen naar de i/o-interface gebruikt om te communiceren met de implementatie van de standaard-modules. TPM's kunnen bestaan als discrete hardware, geïntegreerde hardware, op basis van firmware of op basis van software. Meer informatie over [TPM's en TPM attestation](/windows-server/identity/ad-ds/manage/component-updates/tpm-key-attestation). Device Provisioning Service ondersteunt alleen TPM 2.0.

TPM-attestation is gebaseerd op een nonce uitdaging, dat gebruikmaakt van de hoofdsleutels endorsement en opslag om weer te geven van een ondertekende Shared Access Signature (SAS)-token.

### <a name="endorsement-key"></a>Goedkeuringssleutel

De goedkeuringssleutel is een asymmetrische sleutel die is opgenomen in de TPM, die intern gegenereerd of geïnjecteerd productie-tijd en is uniek voor elke TPM. De goedkeuringssleutel kan niet worden gewijzigd of verwijderd. Het persoonlijke gedeelte van de goedkeuringssleutel vrijgegeven nooit buiten de TPM, terwijl het openbare gedeelte van de goedkeuringssleutel wordt gebruikt voor het herkennen van een legitieme TPM. Meer informatie over de [goedkeuringssleutel](https://technet.microsoft.com/library/cc770443(v=ws.11).aspx).

### <a name="storage-root-key"></a>De opslaghoofdsleutel

De opslaghoofdsleutel is opgeslagen in de TPM en wordt gebruikt voor het beveiligen van TPM-sleutels die zijn gemaakt door toepassingen, zodat deze sleutels niet kunnen worden gebruikt zonder de TPM. De opslaghoofdsleutel wordt gegenereerd wanneer u eigenaar worden van de TPM; Wanneer u de TPM wist, zodat een nieuwe gebruiker van eigenaar, wordt een nieuwe hoofdsleutel voor de opslag gegenereerd. Meer informatie over de [opslaghoofdsleutel](https://technet.microsoft.com/library/cc753560(v=ws.11).aspx).

## <a name="x509-certificates"></a>X.509-certificaten

Met behulp van X.509-certificaten als een attestation-mechanisme is een uitstekende manier om te schalen productie en vereenvoudigen apparaatinrichting. X.509-certificaten zijn doorgaans gerangschikt in een certificaatvertrouwensketen waarin elk certificaat in de keten is ondertekend door de persoonlijke sleutel van de volgende hogere certificaat, enzovoort, wordt beëindigd in een zelfondertekend basiscertificaat. In deze rangschikking bepaalt een gedelegeerde vertrouwensketen van het basiscertificaat dat is gegenereerd door een vertrouwde basiscertificeringsinstantie (CA) omlaag via elke tussenliggende CA aan het einde-entiteit "leaf" certificaat geïnstalleerd op een apparaat. Zie voor meer informatie, [Apparaatverificatie met behulp van x.509-CA-certificaten](/azure/iot-hub/iot-hub-x509ca-overview). 

De certificaatketen vertegenwoordigt vaak enkele logische of fysieke-hiërarchie die is gekoppeld aan apparaten. Bijvoorbeeld, een fabrikant mogelijk:
- een zelf-ondertekend basis-CA-certificaat
- het basiscertificaat voor het genereren van een unieke tussenliggende CA-certificaat voor elke factory gebruiken
- van elke factory certificaat gebruiken voor het genereren van een unieke tussenliggende CA-certificaat voor elke regel van de productie in de fabriek
- en ten slotte de productielijn-certificaat gebruiken voor het genereren van een certificaat voor unieke apparaten (einde-entiteit) voor elk apparaat op de regel wordt geproduceerd. 

Zie voor meer informatie, [conceptuele kennis van x.509-CA-certificaten in de branche IoT](/azure/iot-hub/iot-hub-x509ca-concept). 

### <a name="root-certificate"></a>Basiscertificaat

Een basiscertificaat is een zelf-ondertekend X.509-certificaat voor een certificeringsinstantie (CA). Dit is de terminus of vertrouwensanker van de certificaatketen. Basiscertificaten kunnen zelf zijn uitgegeven door een organisatie of hebt aangeschaft via een basiscertificeringsinstantie. Zie voor meer informatie, [ophalen x.509-CA-certificaten](/azure/iot-hub/iot-hub-security-x509-get-started#get-x509-ca-certificates). Het basiscertificaat kan ook worden aangeduid als een CA-basiscertificaat.

### <a name="intermediate-certificate"></a>Tussenliggende certificaat

Een tussenliggende certificaat is een X.509-certificaat, dat is ondertekend door het basiscertificaat (of door een andere tussenliggende certificaat met het basiscertificaat in de keten). De laatste tussenliggende certificaten in een keten wordt gebruikt om de handtekening van het leafcertificaat. Een tussenliggende certificaat kan ook worden aangeduid als een tussenliggende CA-certificaat.

### <a name="end-entity-leaf-certificate"></a>Einde-entiteit 'leaf'-certificaat

De leaf-certificaat of eindentiteitscertificaat, identificeert de certificaathouder. Het basiscertificaat in de certificaatketen en nul of meer tussenliggende certificaten heeft. Het leaf-certificaat wordt niet gebruikt om alle andere certificaten te ondertekenen. Unieke wijze identificeert het apparaat aan de provisioning-service en wordt soms aangeduid als het certificaat voor apparaten. Tijdens verificatie wordt het apparaat de persoonlijke sleutel die is gekoppeld aan dit certificaat gebruikt om te reageren op een bewijs van bezit uitdaging van de service. Zie voor meer informatie, [verificatie van apparaten die zijn ondertekend met x.509-CA-certificaten](/azure/iot-hub/iot-hub-x509ca-overview#authenticating-devices-signed-with-x509-ca-certificates).

## <a name="controlling-device-access-to-the-provisioning-service-with-x509-certificates"></a>Apparaattoegang tot de inrichtingsservice met X.509-certificaten

Twee typen inschrijvingsvermelding die u gebruiken kunt voor het beheren van toegang voor apparaten die gebruikmaken van het x.509-attestation-mechanisme wordt aangegeven dat de provisioning-service:  

- [Afzonderlijke inschrijving](./concepts-service.md#individual-enrollment) vermeldingen zijn geconfigureerd met het certificaat voor apparaten die zijn gekoppeld aan een specifiek apparaat. Deze vermeldingen beheren inschrijvingen voor specifieke apparaten.
- [Registratiegroep](./concepts-service.md#enrollment-group) vermeldingen zijn gekoppeld aan een specifieke tussenliggend of CA-basiscertificaat. Deze vermeldingen beheren inschrijvingen voor alle apparaten waarop die tussenliggende of het certificaat in hun certificaatketen hoofdknooppunt. 

Wanneer een apparaat verbinding met de provisioning-service maakt, de service bepaalt de volgorde van specifiekere inschrijving vermeldingen op minder specifieke vermeldingen van inschrijving. Dat wil zeggen, als een afzonderlijke inschrijving voor het apparaat bestaat, geldt de provisioning-service die vermelding. Als er geen afzonderlijke inschrijving voor het apparaat en een registratiegroep voor de eerste tussenliggende certificaten in de certificaatketen van het apparaat bestaat, geldt de service die post, enzovoort, van de keten voor de hoofdmap. De service van toepassing is het eerste toepasselijke item dat wordt gevonden, zodat:

- Als de eerste vermelding voor apparaatinschrijving gevonden is ingeschakeld, richt de service het apparaat.
- Als de eerste vermelding voor apparaatinschrijving gevonden is uitgeschakeld, wordt in de service het apparaat niet inrichten.  
- Als er geen vermelding voor apparaatinschrijving voor het gebruik van de certificaten in de certificaatketen van het apparaat wordt gevonden, wordt in de service het apparaat niet inrichten. 

Dit mechanisme en de hiërarchische structuur van certificaatketens biedt krachtige flexibiliteit in hoe u toegang tot afzonderlijke apparaten ook als voor groepen apparaten kunt beheren. Voorbeeld van uitgaan dat vijf apparaten met de volgende certificaat is gekoppeld: 

- *Apparaat 1*: basiscertificaat-certificaat een certificaat voor apparaten 1 -> >
- *Apparaat 2*: basiscertificaat-certificaat een certificaat voor apparaten 2 -> >
- *Apparaat 3*: basiscertificaat-certificaat een certificaat voor apparaten 3 -> >
- *Apparaat 4*: basiscertificaat -> certificaat B -> apparaatcertificaat 4
- *Apparaat 5*: basiscertificaat -> certificaat B -> apparaatcertificaat 5

In eerste instantie, kunt u een vermelding voor apparaatinschrijving van één ingeschakelde groep voor het basiscertificaat toegang wilt inschakelen voor alle vijf apparaten. Als het certificaat B later wordt aangetast, kunt u een uitgeschakelde registatiegroepvermelding voor certificaat B om te voorkomen dat *apparaat 4* en *apparaat 5* geregistreerd. Als deze nog steeds hoger *apparaat 3* wordt aangetast, kunt u een vermelding voor uitgeschakelde afzonderlijke registratie voor het certificaat maken. Deze toegang ingetrokken voor *apparaat 3*, maar is nog steeds mogelijk *apparaat 1* en *apparaat 2* om in te schrijven.