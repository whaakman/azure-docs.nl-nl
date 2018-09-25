---
title: Apparaatconcepten het inrichten van apparaten in Azure | Microsoft Docs
description: Beschrijft de concepten die specifiek zijn voor apparaten met Device Provisioning Service en IoT Hub voor apparaatinrichting
author: nberdy
ms.author: nberdy
ms.date: 09/05/2017
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: briz
ms.openlocfilehash: 354ef48f7935536864cde9dc0d9a130fa5aeb865
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46972858"
---
# <a name="iot-hub-device-provisioning-service-device-concepts"></a>Concepten van IoT Hub Device Provisioning Service-apparaat

IoT Hub Device Provisioning Service is een helper-service voor IoT Hub die u met zero-touch-apparaatinrichting voor een opgegeven IoT hub configureren. Met de Device Provisioning Service kunt u miljoenen apparaten inrichten op een veilige en schaalbare manier.

In dit artikel biedt een overzicht van de *apparaat* concepten betrokken bij het apparaat wordt ingericht. In dit artikel is het meest relevant voor personen die betrokken zijn bij de [fabricageproces stap](about-iot-dps.md#manufacturing-step) van een apparaat voorbereiden voor implementatie.

## <a name="attestation-mechanism"></a>Attestation-mechanisme

De attestation-mechanisme is de methode die wordt gebruikt voor het bevestigen van een apparaat-id. De attestation-mechanisme is ook relevant zijn voor de lijst inschrijving, dit geeft aan de inrichtingsservice welke methode voor attestation gebruiken met een bepaald apparaat.

> [!NOTE]
> IoT Hub maakt gebruik van 'verificatieschema' voor een vergelijkbare concept in die service.

De Device Provisioning Service ondersteunt de volgende vormen van attestation:
* **X.509-certificaten** op basis van de standaard x.509-certificaat-verificatiestroom.
* **Trusted Platform Module (TPM)** op basis van een nonce uitdaging, met behulp van de TPM-standaard voor sleutels om weer te geven van een ondertekende Shared Access Signature (SAS)-token. Dit is een fysieke TPM op het apparaat niet vereist, maar de service om te bevestigen met behulp van de goedkeuringssleutel per wordt verwacht dat de [TPM-specificatie](https://trustedcomputinggroup.org/work-groups/trusted-platform-module/).
* **Symmetrische sleutel** op basis van de handtekening voor gedeelde toegang (SAS) [beveiligingstokens](../iot-hub/iot-hub-devguide-security.md#security-tokens), waaronder een hash-handtekening en een ingesloten verlopen. Zie voor meer informatie, [symmetrische sleutel attestation](concepts-symmetric-key-attestation.md).

## <a name="hardware-security-module"></a>Hardware security module

De HSM, of een aangepaste HSM wordt gebruikt voor beveiligde, op hardware gebaseerde opslag van apparaat geheimen en is de veiligste manier van geheime opslag. X.509-certificaten en SAS-tokens kunnen worden opgeslagen in de HSM. HSM's kunnen worden gebruikt met beide attestation-mechanismen de provisioning service ondersteunt.

> [!TIP]
> Het is raadzaam met behulp van een HSM aan geheimen veilig opslaan op uw apparaten met apparaten.

Apparaat geheimen kunnen ook worden opgeslagen in de software (geheugen), maar er is een minder veilige vorm van opslag dan een HSM.

## <a name="registration-id"></a>Registratie-id

De registratie-ID wordt gebruikt voor het aanduiden van een apparaat in de Device Provisioning Service. De apparaat-ID moet uniek zijn in de inrichtingsservice [ID-bereik](#id-scope). Elk apparaat moet een registratie-id hebben. De registratie-ID is alfanumerieke tekens, kleine letters en afbreekstreepjes mag bevatten.

* In het geval van een TPM, wordt de registratie-ID verstrekt door de TPM zelf.
* De registratie-ID is opgegeven in het geval van op basis van X.509-attestation, als de onderwerpnaam van het certificaat.

## <a name="device-id"></a>Apparaat-id

De apparaat-ID is de ID zoals deze wordt weergegeven in de IoT Hub. De gewenste apparaat-ID kan worden ingesteld in de vermelding voor apparaatinschrijving, maar het is niet vereist om te worden ingesteld. Als geen gewenste apparaat-ID in de lijst inschrijving is opgegeven, wordt de registratie-ID wordt gebruikt als de apparaat-ID bij het registreren van het apparaat. Meer informatie over [apparaat-id's van IoT-Hub](../iot-hub/iot-hub-devguide-identity-registry.md).

## <a name="id-scope"></a>ID-bereik

De ID-bereik is toegewezen aan een Device Provisioning Service wanneer deze wordt gemaakt door de gebruiker en wordt gebruikt voor het aanduiden van de specifieke provisioning-service die het apparaat wordt geregistreerd via. De ID-bereik wordt gegenereerd door de service en is onveranderbaar, die gegarandeerd uniek.

> [!NOTE]
> Uniekheid is belangrijk voor langlopende implementatiebewerkingen en fusie of scenario's.
