---
title: Apparaat-concepten in Azure apparaten inrichten | Microsoft Docs
description: Beschrijft het inrichten van de concepten die specifiek zijn voor apparaten met inrichtingsservice apparaat en IoT-Hub
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
ms.openlocfilehash: 5297bc57729d9e983d63244c71eb21995cf73f0e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="iot-hub-device-provisioning-service-device-concepts"></a>IoT Hub apparaat-inrichtingsservice apparaat concepten

IoT Hub apparaat-inrichtingsservice is een helper-service voor IoT-Hub die u gebruikt om zonder tussenkomst apparaat inrichten met een opgegeven IoT-hub te configureren. U kunt in een veilige en schaalbare manier miljoenen apparaten inrichten met de Service voor het inrichten van apparaten.

In dit artikel biedt een overzicht van de *apparaat* concepten betrokken bij de mobiele apparaten inrichten. Dit artikel is het meest relevant voor Persona's die zijn betrokken bij de [productie stap](about-iot-dps.md#manufacturing-step) van een apparaat voorbereiden voor implementatie.

## <a name="attestation-mechanism"></a>Attestation-mechanisme

Het mechanisme voor attestation is de methode die wordt gebruikt voor het bevestigen van een apparaat-id. Het mechanisme voor attestation is ook relevant zijn voor de registratie-lijst, die de inrichting service vertelt welke methode van de Attestation-bewerking moet worden gebruikt met een bepaald apparaat.

> [!NOTE]
> IoT-Hub maakt gebruik van 'verificatieschema' voor een vergelijkbaar concept in die service.

De Service voor het inrichten van apparaten ondersteunt twee soorten attestation:
* **X.509-certificaten** op basis van de standaard authenticatiestroom voor x.509-certificaat.
* **SAS-tokens** op basis van een nonce uitdaging met de TPM-standaard voor sleutels. Er is geen fysieke TPM op het apparaat vereist, maar wordt verwacht dat de service met behulp van de goedkeuringssleutel per verklaren de [TPM-specificatie](https://trustedcomputinggroup.org/work-groups/trusted-platform-module/).

## <a name="hardware-security-module"></a>Hardware security module

De hardware security module of een HSM, wordt gebruikt voor veilige, op hardware gebaseerde opslag van apparaat geheimen en is de veiligste manier van geheime opslag. X.509-certificaten en SAS-tokens kunnen worden opgeslagen in de HSM. HSM's kunnen worden gebruikt met beide mechanismen attestation de inrichting service ondersteunt.

> [!TIP]
> Wij raden het gebruik van een HSM met apparaten geheimen veilig opslaan op uw apparaten.

Apparaat geheimen kunnen ook worden opgeslagen in de software (geheugen), maar het is een minder veilige vorm van opslag dan een HSM.

## <a name="registration-id"></a>Registratie-ID

De registratie-ID wordt gebruikt voor het aanduiden van een apparaat in de Service voor het inrichten van apparaten. De apparaat-ID moet uniek zijn in de inrichting service [ID-bereik](#id-scope). Elk apparaat moet een registratie-id hebben. De registratie-ID is alfanumerieke, kleine letters en kan de afbreekstreepjes bevatten.

* In het geval van een TPM krijgt u de registratie-ID van de TPM zelf.
* In het geval van op basis van een X.509 attestation, wordt de registratie-ID opgegeven als de onderwerpnaam van het certificaat.

## <a name="device-id"></a>Apparaat-id

De apparaat-ID is de ID zoals deze wordt weergegeven in IoT-Hub. De gewenste apparaat-ID kan worden ingesteld in de vermelding voor inschrijving, maar dit is niet vereist om te worden ingesteld. Als geen gewenste apparaat-ID is opgegeven in de lijst van de inschrijving, wordt de registratie-ID wordt gebruikt als de apparaat-ID bij het registreren van het apparaat. Meer informatie over [apparaat-id's van IoT-Hub](../iot-hub/iot-hub-devguide-identity-registry.md).

## <a name="id-scope"></a>ID-bereik

Het ID-bereik is toegewezen aan een Service voor het inrichten van apparaat wanneer dit wordt gemaakt door de gebruiker en wordt gebruikt om de specifieke inrichting service die het apparaat wordt geregistreerd via uniek te identificeren. Het ID-bereik is gegenereerd door de service en is onveranderbaar, die wordt gegarandeerd dat uniekheid.

> [!NOTE]
> Uniekheid is belangrijk voor langlopende implementatiebewerkingen en fusies en overnames scenario's.
