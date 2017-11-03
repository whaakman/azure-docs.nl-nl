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
ms.openlocfilehash: 3ccbaaf55d2bdfedffcdb5ca069798328e2d75fd
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
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

## <a name="endorsement-key"></a>Goedkeuringssleutel

De goedkeuringssleutel is een asymmetrische sleutel die is opgenomen in de TPM die productie-tijd is ingevoegd en is uniek voor elke TPM. De goedkeuringssleutel kan niet worden gewijzigd of verwijderd. Het persoonlijke gedeelte van de goedkeuringssleutel is nooit buiten de TPM vrijgegeven terwijl het openbare deel van de goedkeuringssleutel wordt gebruikt voor het herkennen van een legitieme TPM. Meer informatie over de [goedkeuringssleutel](https://technet.microsoft.com/library/cc770443(v=ws.11).aspx).

## <a name="storage-root-key"></a>De opslaghoofdsleutel

De opslaghoofdsleutel wordt opgeslagen in de TPM en wordt gebruikt voor het beveiligen van TPM-sleutels die zijn gemaakt door toepassingen, zodat deze sleutels niet kunnen worden gebruikt zonder de TPM. De opslaghoofdsleutel wordt gegenereerd wanneer u eigenaar bent van de TPM; Wanneer u de TPM wist zodat een nieuwe gebruiker kunt eigenaar, wordt een nieuwe hoofdsleutel voor de opslag gegenereerd. Meer informatie over de [opslaghoofdsleutel](https://technet.microsoft.com/library/cc753560(v=ws.11).aspx).

## <a name="root-certificate"></a>Basiscertificaat

Een basiscertificaat is een type van een certificeringsinstantie die het X.509-certificaat en is zelfondertekend. De terminus van de certificaatketen is.

## <a name="intermediate-certificate"></a>Tussenliggende certificaat

Er is een X.509-certificaat dat is ondertekend door het basiscertificaat (of door een ander certificaat met het basiscertificaat in de keten) en dat wordt gebruikt voor het leaf-certificaat ondertekenen een tussenliggende certificaat.

## <a name="leaf-certificate"></a>Leaf-certificaat

Een certificaat of een eindentiteitscertificaat, wordt gebruikt voor het identificeren van de certificaathouder en heeft het basiscertificaat in de certificaatketen. Het certificaat wordt niet gebruikt om alle andere certificaten te ondertekenen.
