---
title: Aanbevelingen voor beveiliging voor Azure IoT | Microsoft Docs
description: In dit artikel bevat een overzicht van aanvullende stappen uit om te zorgen voor beveiliging in uw Azure IoT Hub-oplossing.
author: dsk-2015
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 06/26/2019
ms.author: dkshir
ms.custom: security-recommendations
ms.openlocfilehash: 4416f3149c33a0c9a437b2fbd6a48729a5a7f044
ms.sourcegitcommit: dad277fbcfe0ed532b555298c9d6bc01fcaa94e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2019
ms.locfileid: "67722868"
---
# <a name="security-recommendations-for-azure-internet-of-things-iot-deployment"></a>Aanbevelingen voor beveiliging voor de implementatie van Azure Internet of Things (IoT)

In dit artikel bevat aanbevelingen voor beveiliging voor de Azure IoT-services. Implementatie van deze aanbevelingen verbeteren help u te voldoen aan uw verplichtingen security als een klant van Azure IoT, en zal de algehele beveiliging voor uw IoT-oplossingen. Lees voor meer informatie over de ingebouwde beveiligingsfuncties die worden geleverd door Azure IoT [IoT-beveiliging vanaf de grond](iot-security-ground-up.md).

## <a name="general"></a>Algemeen

| Aanbeveling | Opmerkingen |
|-|-|
| Blijf op de hoogte | Gebruik de nieuwste versies van ondersteunde platforms, programmeertalen, protocollen en frameworks. |
| Verificatiesleutels veilig te houden | Beveilig de apparaat-id's en hun verificatiesleutels fysiek na de implementatie. Dit zal voorkomen dat een kwaadwillende apparaat maskerade als een geregistreerd apparaat. |
| Gebruik apparaat-SDK's indien mogelijk | Apparaat-SDK's implementeren diverse beveiligingsfuncties, zoals versleuteling, verificatie, enzovoort, om u te helpen bij het ontwikkelen van een robuuste en veilige device-toepassing. Zie [begrijpen en gebruiken Azure IoT Hub SDK's](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-sdks) voor meer informatie. |


## <a name="identity-and-access-management"></a>Identiteits- en toegangsbeheer

| Aanbeveling | Opmerkingen |
|-|-|
| Toegangsbeheer voor de hub definiëren | [Begrijpen en definiëren van het type toegang](iot-security-deployment.md#securing-the-cloud) elk onderdeel hebben in uw IoT Hub-oplossing op basis van de functionaliteit. De toegestane machtigingen zijn *lezen van het register*, *RegistryReadWrite*, *ServiceConnect*, en *DeviceConnect*. Standaard [gedeeld toegangsbeleid in uw IoT-hub](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-security#access-control-and-permissions) kunt ook de machtigingen voor elk onderdeel op basis van de rol definiëren. |
| Toegangsbeheer voor back-endservices definiëren | Gegevens die zijn opgenomen door de oplossing van uw IoT-Hub kan worden gebruikt door andere Azure-services zoals [Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/), [Stream Analytics](https://docs.microsoft.com/azure/stream-analytics/), [App Service](https://docs.microsoft.com/azure/app-service/), [Logic Apps](https://docs.microsoft.com/azure/logic-apps/), en [Blob storage](https://docs.microsoft.com/azure/storage/blobs/storage-blobs-introduction). Zorg ervoor dat u begrijpt en toestaan dat de juiste toegangsmachtigingen zoals beschreven voor deze services. |


## <a name="data-protection"></a>Gegevensbescherming

| Aanbeveling | Opmerkingen |
|-|-|
| Verificatie van apparaten beveiligen | Zorg ervoor dat veilige communicatie tussen uw apparaten en uw IoT-hub met behulp van [een unieke id-sleutel of security token](iot-security-deployment.md#iot-hub-security-tokens), of [een X.509-certificaat op het apparaat](iot-security-deployment.md#x509-certificate-based-device-authentication) voor elk apparaat. Gebruik de juiste methode te [beveiligingstokens op basis van de gekozen protocol (MQTT-, AMQP- of HTTPS) gebruiken](https://docs.microsoft.com/azure/iot-hub/iot-hub-devguide-security). |
| Beveiligde apparaatcommunicatie | IoT Hub voor beveiliging van de verbinding met de apparaten met behulp van Transport Layer Security (TLS) standard, ondersteunen versies 1.2, 1.2 en 1.0. Gebruik [TLS 1.2](https://tools.ietf.org/html/rfc5246) om maximale beveiliging te waarborgen. |
| Beveiligde servicecommunicatie | IoT Hub biedt eindpunten verbinding maken met back-endservices zoals [Azure Storage](/azure/storage/) of [Event Hubs](/azure/event-hubs) met behulp van alleen het TLS-protocol en geen eindpunt wordt weergegeven op een niet-versleuteld kanaal. Zodra deze gegevens deze back endservices voor opslag of analyse bereikt, zorg ervoor dat u de juiste beveiliging en versleuteling methoden voor de service gebruiken en beveiligen van vertrouwelijke gegevens op de back-end. |


## <a name="networking"></a>Netwerken

| Aanbeveling | Opmerkingen |
|-|-|
| Toegang tot uw apparaten beveiligen | Houd hardware poorten in uw apparaten tot een absoluut minimum beperkt om te voorkomen dat ongewenste toegang. Bovendien bouwen mechanismen om te voorkomen of fysieke knoeien met het apparaat detecteren. Lezen [best practices voor IoT-beveiliging](iot-security-best-practices.md) voor meer informatie. |
| Bouw veilige hardware | Gebruikmaken van beveiligingsfuncties, zoals versleutelde opslag of Trusted Platform Module (TPM), op apparaten en -infrastructuur beter te beveiligen. Houd de apparaat-besturingssysteem en stuurprogramma's die zijn bijgewerkt naar de meest recente versies en als ruimte toestaat, antivirus- en mogelijkheden te installeren. Lezen [IoT-beveiligingsarchitectuur](iot-security-architecture.md) om te begrijpen hoe dit kan helpen verschillende security bedreigingen te verhelpen. |


## <a name="monitoring"></a>Bewaking

| Aanbeveling | Opmerkingen |
|-|-|
| Niet-geautoriseerde toegang tot uw apparaten controleren |  Functie voor de logboekregistratie van het apparaat besturingssysteem gebruiken voor het bewaken van eventuele schendingen van de beveiliging of fysieke knoeien met het apparaat of de poorten. |
| Bewaken van uw IoT-oplossing vanuit de cloud | Controleren van de algemene status van uw IoT Hub-oplossing met de [metrische gegevens in Azure Monitor](https://docs.microsoft.com/azure/iot-hub/iot-hub-metrics). |
| Diagnose instellen | Uw bedrijfsactiviteiten nauwlettend volgen door logboekregistratie van gebeurtenissen in uw oplossing en vervolgens de diagnostische logboeken te verzenden naar Azure Monitor om inzicht te krijgen in de prestaties. Lezen [bewaken en diagnosticeren van problemen in uw IoT-hub](https://docs.microsoft.com/azure/iot-hub/iot-hub-monitor-resource-health) voor meer informatie. |

## <a name="next-steps"></a>Volgende stappen

Voor geavanceerde scenario's met betrekking tot Azure IoT, moet u mogelijk rekening houden met vereisten voor extra beveiliging. Zie [IoT-beveiligingsarchitectuur](iot-security-architecture.md) voor meer informatie.

