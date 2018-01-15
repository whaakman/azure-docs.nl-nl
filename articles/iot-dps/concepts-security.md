---
title: Beveiligingsconcepten in Azure IoT Hub apparaat-inrichtingsservice | Microsoft Docs
description: Beschrijft security inrichting concepten die specifiek zijn voor apparaten met inrichtingsservice apparaat en IoT-Hub
services: iot-dps
keywords: 
author: nberdy
ms.author: nberdy
ms.date: 09/05/2017
ms.topic: article
ms.service: iot-dps
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: ab2bfff571af659552eef8117de041ca6367ce56
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2018
---
# <a name="iot-hub-device-provisioning-service-security-concepts"></a>IoT Hub apparaat-inrichtingsservice veiligheidsconcepten 

IoT Hub apparaat-inrichtingsservice is een helper-service voor IoT-Hub die u gebruikt om zonder tussenkomst apparaat inrichten met een opgegeven IoT-hub te configureren. U kunt in een veilige en schaalbare manier miljoenen apparaten inrichten met de Service voor het inrichten van apparaten. In dit artikel biedt een overzicht van de *beveiliging* concepten betrokken bij de mobiele apparaten inrichten. Dit artikel is relevant zijn voor alle Persona's die zijn betrokken bij het voorbereiden van een apparaat voor implementatie.

## <a name="attestation-mechanism"></a>Attestation-mechanisme

Het mechanisme voor attestation is de methode die wordt gebruikt voor het bevestigen van een apparaat-id. Het mechanisme voor attestation is ook relevant zijn voor de registratie-lijst, die de inrichting service vertelt welke methode van de Attestation-bewerking moet worden gebruikt met een bepaald apparaat.

> [!NOTE]
> IoT-Hub maakt gebruik van 'verificatieschema' voor een vergelijkbaar concept in die service.

Apparaat inrichtingsservice ondersteunt twee soorten attestation:
* **X.509-certificaten** op basis van de standaard authenticatiestroom voor x.509-certificaat.
* **SAS-tokens** op basis van een nonce uitdaging met de TPM-standaard voor sleutels. Er is geen fysieke TPM op het apparaat vereist, maar wordt verwacht dat de service met behulp van de goedkeuringssleutel per verklaren de [TPM-specificatie](https://trustedcomputinggroup.org/work-groups/trusted-platform-module/).

## <a name="hardware-security-module"></a>Hardware security module

De hardware security module of een HSM, wordt gebruikt voor veilige, op hardware gebaseerde opslag van apparaat geheimen en is de veiligste manier van geheime opslag. X.509-certificaten en SAS-tokens kunnen worden opgeslagen in de HSM. HSM's kunnen worden gebruikt met beide mechanismen attestation de inrichting worden ondersteund.

> [!TIP]
> Wij raden het gebruik van een HSM met apparaten geheimen veilig opslaan op uw apparaten.

Apparaat geheimen kunnen ook worden opgeslagen in de software (geheugen), maar het is een minder veilige vorm van opslag dan een HSM.

## <a name="trusted-platform-module-tpm"></a>Trusted Platform Module (TPM)

TPM kan verwijzen naar een standaard voor het veilig opslaan van sleutels die worden gebruikt voor het verifiëren van het platform of het kan verwijzen naar de i/o-interface gebruikt om te communiceren met de implementatie van de standaard-modules. TPM's kunnen bestaan als discrete hardware, geïntegreerde hardware, firmware gebaseerde of op basis van software. Meer informatie over [TPM's en TPM attestation](/windows-server/identity/ad-ds/manage/component-updates/tpm-key-attestation). Apparaat inrichtingsservice biedt alleen ondersteuning voor TPM 2.0.

### <a name="endorsement-key"></a>Goedkeuringssleutel

De goedkeuringssleutel is een asymmetrische sleutel die is opgenomen in de TPM die intern is gegenereerd of geïnjecteerd productie-tijd en is uniek voor elke TPM. De goedkeuringssleutel kan niet worden gewijzigd of verwijderd. Het persoonlijke gedeelte van de goedkeuringssleutel is nooit buiten de TPM vrijgegeven terwijl het openbare deel van de goedkeuringssleutel wordt gebruikt voor het herkennen van een legitieme TPM. Meer informatie over de [goedkeuringssleutel](https://technet.microsoft.com/library/cc770443(v=ws.11).aspx).

### <a name="storage-root-key"></a>De opslaghoofdsleutel

De opslaghoofdsleutel wordt opgeslagen in de TPM en wordt gebruikt voor het beveiligen van TPM-sleutels die zijn gemaakt door toepassingen, zodat deze sleutels niet kunnen worden gebruikt zonder de TPM. De opslaghoofdsleutel wordt gegenereerd wanneer u eigenaar bent van de TPM; Wanneer u de TPM wist zodat een nieuwe gebruiker kunt eigenaar, wordt een nieuwe hoofdsleutel voor de opslag gegenereerd. Meer informatie over de [opslaghoofdsleutel](https://technet.microsoft.com/library/cc753560(v=ws.11).aspx).

## <a name="x509-certificates"></a>X.509-certificaten

X.509-certificaten gebruiken als een mechanisme voor Attestation-bewerking is een uitstekende manier om te schalen productie en vereenvoudigen apparaten inrichten. X.509-certificaten worden doorgaans gerangschikt in een certificaatvertrouwensketen waarin elk certificaat in de keten is ondertekend door de persoonlijke sleutel van de volgende hogere certificaat, enzovoort, wordt beëindigd in een zelfondertekend basiscertificaat. Hiermee stelt u een gedelegeerde vertrouwensketen van het basiscertificaat dat is gegenereerd door een vertrouwde certificeringsinstantie (CA) omlaag via elke tussenliggende CA aan het einde-entiteit certificaat geïnstalleerd op een apparaat. Zie voor meer informatie, [verificatie van apparaten met behulp van de CA-certificaten X.509](https://docs.microsoft.com/azure/iot-hub/iot-hub-x509ca-overview). 

De certificaatketen vertegenwoordigt vaak een aantal logische of fysieke hiërarchie gekoppeld met apparaten. Bijvoorbeeld een fabrikant mogelijk een zelfondertekend basiscertificaat uitgeven, dat certificaat gebruiken bij het genereren van een unieke tussenliggende CA-certificaat voor elke factory, elke factory certificaat gebruiken voor het genereren van een unieke tussenliggende CA-certificaat voor elke productie regel in de installatie en gebruik ten slotte het certificaat van de productie-regel voor het genereren van een certificaat unieke apparaat (Eindentiteit) voor elk apparaat op de regel geproduceerd. Zie voor meer informatie, [conceptuele kennis van het x.509-CA-certificaten in de branche IoT](https://docs.microsoft.com/azure/iot-hub/iot-hub-x509ca-concept). 

### <a name="root-certificate"></a>Basiscertificaat

Een basiscertificaat is een zelf-ondertekend X.509-certificaat voor een certificeringsinstantie (CA). Het is de terminus of vertrouwensanker van de certificaatketen. Basiscertificaten worden automatisch uitgegeven door een organisatie of aangeschaft via een basiscertificeringsinstantie. Zie voor meer informatie, [X.509-certificaten ophalen](https://docs.microsoft.com/azure/iot-hub/iot-hub-security-x509-get-started#get-x509-ca-certificates). Het basiscertificaat kan ook worden aangeduid als een CA-basiscertificaat.

### <a name="intermediate-certificate"></a>Tussenliggende certificaat

Er is een X.509-certificaat dat is ondertekend door het basiscertificaat (of door een andere tussenliggende certificaat met het basiscertificaat in de keten) voor een tussenliggende certificaat. De laatste tussenliggende certificaten in een keten van wordt gebruikt voor het leaf-certificaat ondertekenen. Een tussenliggende certificaat kan ook worden aangeduid als een tussenliggende CA-certificaat.

### <a name="leaf-certificate"></a>Leaf-certificaat

Het certificaat of eindentiteitscertificaat, identificeert de certificaathouder. Het basiscertificaat in de certificaatketen en nul of meer tussenliggende certificaten heeft. Het certificaat wordt niet gebruikt om alle andere certificaten te ondertekenen. Unieke wijze identificeert het apparaat aan de inrichting service en wordt soms aangeduid als het certificaat voor apparaten. Het apparaat wordt de persoonlijke sleutel die is gekoppeld aan dit certificaat tijdens verificatie wordt gebruikt om te reageren op een bewijs van bezit uitdaging van de service. Zie voor meer informatie, [verificatie van apparaten die zijn ondertekend met x.509-CA-certificaten](https://docs.microsoft.com/azure/iot-hub/iot-hub-x509ca-overview#authenticating-devices-signed-with-x509-ca-certificates).

## <a name="controlling-device-access-to-the-provisioning-service-with-x509-certificates"></a>Apparaattoegang tot de inrichting service met X.509-certificaten beheren

De inrichting service bevat twee soorten inschrijving vermelding die u gebruiken kunt voor het toegangsbeheer voor apparaten die gebruikmaken van het X.509-mechanisme voor attestation:  

- [Afzonderlijke inschrijving](./concepts-service.md#individual-enrollment) vermeldingen zijn geconfigureerd met het certificaat voor apparaten die zijn gekoppeld aan een specifiek apparaat. Deze vermeldingen beheren inschrijving voor specifieke apparaten.
- [Inschrijvingsgroep](./concepts-service.md#enrollment-group) vermeldingen zijn gekoppeld aan een specifieke tussenliggende of CA-basiscertificaat. Deze vermeldingen beheren inschrijving voor alle apparaten waarop die tussenliggende of het certificaat in de certificaatketen hoofdknooppunt. 

Wanneer een apparaat verbinding met de inrichting service maakt, de service bepaalt de volgorde van meer specifieke vermeldingen voor inschrijving via minder specifieke vermeldingen voor inschrijving. Dat wil zeggen, als een afzonderlijke registratie voor het apparaat bestaat, geldt de inrichting service die vermelding. Als er geen afzonderlijke inschrijving voor het apparaat en een inschrijvingsgroep voor het eerste tussenliggende certificaat in de certificaatketen van het apparaat bestaat, geldt de service die vermelding in de keten naar de hoofdmap van enzovoort. De service van toepassing is de eerste toepasselijke vermelding die wordt gevonden, zodat:

- Als de eerste inschrijving vermelding gevonden is ingeschakeld, is de service voorziet het apparaat.
- Als de eerste inschrijving vermelding gevonden is uitgeschakeld, wordt in de service het apparaat niet inrichten.  
- Als er geen vermelding voor inschrijving voor een van de certificaten in de certificaatketen van het apparaat wordt gevonden, wordt in de service het apparaat niet inrichten. 

Dit mechanisme en de hiërarchische structuur van certificaatketens biedt krachtige flexibiliteit in hoe u toegang voor afzonderlijke apparaten ook als voor groepen van apparaten kunt beheren. Denk bijvoorbeeld aan vijf apparaten met de volgende certificaatketens: 

- *Apparaat 1*: basiscertificaat-certificaat een certificaat voor apparaten 1 -> >
- *Apparaat 2*: basiscertificaat-certificaat een certificaat voor apparaten 2 -> >
- *Apparaat 3*: basiscertificaat-certificaat een certificaat voor apparaten 3 -> >
- *Apparaat 4*: basiscertificaat-certificaat B-certificaat voor apparaten 4 > >
- *Apparaat 5*: basiscertificaat-certificaat B-certificaat voor apparaten 5 > >

In eerste instantie, kunt u een vermelding van de registratie één ingeschakelde groep voor het basiscertificaat toegang inschakelen voor alle vijf apparaten. Als het certificaat B later wordt geknoeid, kunt u een vermelding van de groep uitgeschakelde inschrijving voor certificaat B om te voorkomen dat *apparaat 4* en *apparaat 5* niet worden ingeschreven. Indien nog steeds later *apparaat 3* wordt geknoeid, kunt u een vermelding uitgeschakelde afzonderlijke inschrijving voor het certificaat. Dit trekt u toegang voor *apparaat 3*, maar u kunt nog steeds *apparaat 1* en *apparaat 2* om in te schrijven.