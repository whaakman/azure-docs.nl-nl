---
title: Service-concepten in Azure IoT Hub Device Provisioning Service | Microsoft Docs
description: Beschrijving van service inrichten concepten specifiek zijn voor apparaten met de Device Provisioning Service en IoT-Hub
author: nberdy
ms.author: nberdy
ms.date: 03/30/2018
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: briz
ms.openlocfilehash: ca2ea3c000e811223ded3022021c2516f547ae66
ms.sourcegitcommit: f057c10ae4f26a768e97f2cb3f3faca9ed23ff1b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/17/2018
ms.locfileid: "42059443"
---
# <a name="iot-hub-device-provisioning-service-concepts"></a>Concepten van IoT Hub Device Provisioning Service

IoT Hub Device Provisioning Service is een helper-service voor IoT Hub die u met zero-touch-apparaatinrichting voor een opgegeven IoT hub configureren. Met de Device Provisioning Service, kunt u [automatisch inrichten](concepts-auto-provisioning.md) miljoenen apparaten op een veilige en schaalbare manier.

Apparaatinrichting bestaat proces uit twee delen. Het eerste deel is tot stand brengen van de eerste verbinding tussen het apparaat en de IoT-oplossing door *registreren* het apparaat. Het tweede gedeelte is de juiste toepassing *configuratie* op het apparaat op basis van de specifieke vereisten van de oplossing. Nadat de beide stappen zijn voltooid, het apparaat volledig is *ingericht*. De Device Provisioning Service automatiseert beide stappen en biedt zo een naadloze ervaring voor de inrichting van het apparaat.

In dit artikel biedt een overzicht van de inrichting concepten die het meest van toepassing voor het beheren van de *service*. In dit artikel is het meest relevant voor personen die betrokken zijn bij de [cloud instellingsstap](about-iot-dps.md#cloud-setup-step) van een apparaat voorbereiden voor implementatie.

## <a name="service-operations-endpoint"></a>Operations-service-eindpunt

Het service-eindpunt voor bewerkingen is het eindpunt voor de service-instellingen beheren en onderhouden van de lijst inschrijving. Dit eindpunt wordt alleen gebruikt door de servicebeheerder. het wordt niet gebruikt door apparaten.

## <a name="device-provisioning-endpoint"></a>Inrichting apparaateindpunt

Het eindpunt voor het inrichten van apparaat is het één eindpunt dat alle apparaten voor automatische inrichting gebruiken. De URL is hetzelfde voor alle provisioning service-exemplaren, elimineert de noodzaak voor apparaten met de nieuwe verbindingsinformatie reflash in supply chain scenario's. De ID-bereik zorgt ervoor dat de isolatie van tenants.

## <a name="linked-iot-hubs"></a>Gekoppelde IoT-hubs

De Device Provisioning Service kan alleen apparaten met IoT-hubs die zijn gekoppeld aan het inrichten. Een IoT-hub koppelen aan een exemplaar van de Device Provisioning service, krijgt de machtigingen van de service voor lezen/schrijven van de IoT hub-apparaatregister; met de koppeling een Device Provisioning service registreren van een apparaat-ID en de eerste configuratie ingesteld op het dubbele apparaat. Gekoppelde IoT-hubs mogelijk zijn in elke Azure-regio. U kunt hubs in andere abonnementen koppelen aan uw provisioning-service.

## <a name="allocation-policy"></a>Toewijzingsbeleid

Het serviceniveau-instelling waarmee wordt bepaald hoe Device Provisioning Service apparaten toegewezen aan een IoT-hub. Er worden drie soorten toewijzingsbeleid ondersteund:

* **Gelijk gewogen distributie**: gekoppelde IoT-hubs hebben evenveel kans dat apparaten die voor hen ingericht. De standaardinstelling. Als u apparaten voor slechts één IoT-hub inricht, kunt u deze instelling bewaren.

* **Laagste latentie**: apparaten worden ingericht voor een IoT-hub met de laagste latentie voor het apparaat. Als meerdere gekoppelde IoT-hubs dezelfde laagste latentie bieden, hashes de inrichtingsservice apparaten via deze hubs

* **Statische configuratie via de registratielijst**: specificatie van de gewenste IoT-hub in de registratielijst heeft voorrang boven het toewijzingsbeleid serviceniveau.

## <a name="enrollment"></a>Inschrijving

Een inschrijving is de registratie van apparaten of groepen van apparaten dat kunnen worden geregistreerd via automatische inrichting. De inschrijvingsrecord bevat informatie over het apparaat of een groep apparaten, waaronder:
- de [attestation-mechanisme](concepts-security.md#attestation-mechanism) wordt gebruikt door het apparaat
- de optionele gewenste beginconfiguratie
- gewenste IoT-hub
- de gewenste apparaat-ID

Er zijn twee soorten inschrijvingen die worden ondersteund door de Device Provisioning Service:

### <a name="enrollment-group"></a>Registratiegroep

Een registratiegroep zit is een groep van apparaten die een specifiek attestation-mechanisme delen. Alle apparaten in de registratiegroep aanwezig X.509-certificaten die zijn ondertekend door de dezelfde basis of tussenliggende certificeringsinstantie (CA). Registreren van groepen kunnen alleen gebruiken voor het x.509-attestation-mechanisme. De naam van de registratiegroep en de naam van het certificaat moeten alfanumeriek, kleine letters en afbreekstreepjes mag bevatten.

> [!TIP]
> Wordt u aangeraden een registratiegroep voor een groot aantal apparaten die een gewenste initiële configuratie delen, of voor apparaten die allemaal zijn verbonden aan dezelfde tenant.

### <a name="individual-enrollment"></a>Afzonderlijke inschrijving

Een afzonderlijke inschrijving wordt een post voor één apparaat dat kan worden geregistreerd. Afzonderlijke inschrijvingen kunnen X.509 leaf-certificaten of SAS-tokens (van een fysiek of virtueel TPM) gebruiken als attestation-mechanismen. De registratie-ID in een afzonderlijke inschrijving is alfanumerieke tekens, kleine letters en afbreekstreepjes mag bevatten. Afzonderlijke inschrijvingen hebben mogelijk de gewenste apparaat-id voor IoT Hub die is opgegeven.

> [!TIP]
> Het is raadzaam om afzonderlijke inschrijvingen te gebruiken voor apparaten die unieke initiële configuraties vereisen, of voor apparaten die alleen kunnen verifiëren met behulp van SAS-tokens via TPM-attestation.

## <a name="registration"></a>Registratie

Een registratie is de record van een apparaat is geregistreerd/inrichting naar een IoT-Hub via de Device Provisioning Service. Registratie-records worden automatisch gemaakt. ze kunnen worden verwijderd, maar ze kunnen niet worden bijgewerkt.

## <a name="operations"></a>Bewerkingen

Bewerkingen zijn de Factureringseenheid van de Device Provisioning Service. Een bewerking is de voltooiing van een instructie met de service. Dit omvat de registratie en herregistratie van apparaten en wijzigingen aan de serverkant, zoals het toevoegen en bijwerken van inschrijvingslijsten.
