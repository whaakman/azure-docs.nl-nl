---
title: Service-concepten in Azure IoT Hub apparaat-inrichtingsservice | Microsoft Docs
description: Beschrijft service inrichting concepten specifiek zijn voor apparaten met DP's en IoT-Hub
services: iot-dps
keywords: ''
author: nberdy
ms.author: nberdy
ms.date: 03/30/2018
ms.topic: article
ms.service: iot-dps
documentationcenter: ''
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: d2bc58514ea716954ec3ac96151549168fedc2ed
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
---
# <a name="iot-hub-device-provisioning-service-concepts"></a>Inrichting-Service van IoT Hub apparaat-concepten

IoT Hub apparaat-inrichtingsservice is een helper-service voor IoT-Hub die u gebruikt om zonder tussenkomst apparaat inrichten met een opgegeven IoT-hub te configureren. Met de Service apparaat inrichting, kunt u [automatisch inrichten](concepts-auto-provisioning.md) miljoenen apparaten op een veilige en schaalbare manier.

Mobiele apparaten inrichten bestaat uit twee fasen. Het eerste deel is tot stand brengen van de eerste verbinding tussen het apparaat en IoT-oplossing door *registreren* het apparaat. Het tweede gedeelte toepast de juiste *configuratie* op het apparaat op basis van de specifieke vereisten van de oplossing. Wanneer beide stappen hebt voltooid, het apparaat volledig is *ingericht*. De Device Provisioning Service automatiseert beide stappen en biedt zo een naadloze ervaring voor de inrichting van het apparaat.

In dit artikel biedt een overzicht van de inrichting concepten meest van toepassing op het beheren van de *service*. Dit artikel is het meest relevant voor Persona's die zijn betrokken bij de [cloud instellingsstap](about-iot-dps.md#cloud-setup-step) van een apparaat voorbereiden voor implementatie.

## <a name="service-operations-endpoint"></a>Operations-service-eindpunt

Het service-eindpunt voor bewerkingen is het eindpunt voor de service-instellingen beheren en onderhouden van de lijst van de inschrijving. Dit eindpunt wordt alleen gebruikt door de servicebeheerder; Dit wordt niet gebruikt door apparaten.

## <a name="device-provisioning-endpoint"></a>Inrichting endpoint van apparaat

Het eindpunt van apparaat-inrichting is het één eindpunt dat alle apparaten voor het automatisch inrichten gebruiken. De URL is hetzelfde voor alle inrichting service-exemplaren, de noodzaak voor apparaten met nieuwe verbindingsgegevens reflash in levering keten scenario's elimineren. De [ID-bereik](#id-scope) zorgt ervoor dat de isolatie van tenants.

## <a name="linked-iot-hubs"></a>Gekoppelde IoT-hubs

Apparaat inrichtingsservice kunt IoT-hubs die zijn gekoppeld aan deze apparaten inrichten. Koppelen van een IoT-hub aan inrichtingsservice apparaat, krijgt de machtigingen voor het lezen/schrijven van service aan het apparaatregister van de IoT-hub; met de koppeling inrichtingsservice apparaat registreren van een apparaat-ID en de eerste configuratie ingesteld in de apparaat-twin. Gekoppelde IoT hubs kunnen zich in een Azure-regio. U kunt hubs in andere abonnementen koppelingen naar uw provisioning-service.

## <a name="allocation-policy"></a>Toewijzingsbeleid voor

Het serviceniveau-instelling die bepaalt hoe apparaat inrichtingsservice apparaten wijst naar een iothub. Er worden drie soorten toewijzingsbeleid ondersteund:
* **Gelijkmatig gewogen distributie**: gekoppelde IoT hubs zijn waarschijnlijk gelijkmatig ingericht op deze apparaten hebben. De standaardinstelling. Als u apparaten voor slechts één IoT-hub inricht, kunt u deze instelling bewaren.
* **Laagste latentie**: apparaten die zijn ingericht naar een iothub met de laagste latentie voor het apparaat. Als meerdere gekoppelde IoT hubs kan dezelfde laagste latentie opgeven wilt, hash de inrichting service-apparaten via deze hubs
* **Statische configuratie via de lijst inschrijving**: specificatie van de gewenste IoT-hub in de lijst inschrijving heeft voorrang boven het serviceniveau toewijzingsbeleid voor.

## <a name="enrollment"></a>Inschrijving

Een van de inschrijving is de registratie van apparaten of groepen van apparaten die via automatische inrichting kunnen registreren. De registratie-record bevat informatie over het apparaat of de groep van apparaten, waaronder:
- de [attestation mechanisme](concepts-security.md#attestation-mechanism) gebruikt door het apparaat
- de optionele eerste gewenste configuratie
- gewenste IoT-hub
- de gewenste apparaat-ID

Er zijn twee soorten inschrijvingen ondersteund door apparaten inrichtingsservice:

### <a name="enrollment-group"></a>Inschrijvingsgroep

Een registratie-groep is een groep apparaten die een specifieke attestation-mechanisme delen. Alle apparaten in de inschrijving groep aanwezig X.509-certificaten die zijn ondertekend door de dezelfde hoofd- of tussenliggende CA. Inschrijving groepen kunnen alleen het X.509-mechanisme voor attestation gebruikt. De groepsnaam van de inschrijving en de naam van het certificaat moeten alfanumerieke, kleine letters en kunnen de afbreekstreepjes bevatten.

> [!TIP]
> Aangeraden wordt een inschrijvingsgroep voor voor een groot aantal apparaten die een gewenste initiële configuratie delen of voor apparaten alle gaat dezelfde tenant.

### <a name="individual-enrollment"></a>Afzonderlijke inschrijving

De inschrijving van een afzonderlijke is een vermelding voor één apparaat kan registreren. Afzonderlijke inschrijvingen mag leaf-X.509-certificaten of SAS-tokens (van een fysieke of virtuele TPM) gebruiken zoals attestation mechanismen. De registratie-ID in een afzonderlijke inschrijving is alfanumerieke, kleine letters en kan de afbreekstreepjes bevatten. Afzonderlijke inschrijvingen hebben mogelijk de gewenste apparaat-id voor IoT Hub die is opgegeven.

> [!TIP]
> U wordt aangeraden met afzonderlijke inschrijvingen voor apparaten die zijn uniek initiële configuraties vereist of voor apparaten die alleen kunnen verifiëren met behulp van SAS-tokens via TPM attestation.

## <a name="registration"></a>Registratie

Een registratie is de record van een apparaat is geregistreerd/inrichting naar een IoT-Hub via apparaat inrichtingsservice. Registratie-records worden automatisch gemaakt. ze kunnen worden verwijderd, maar kan niet worden bijgewerkt.

## <a name="operations"></a>Bewerkingen

Bewerkingen zijn de facturering eenheid van het apparaat inrichtingsservice. Een bewerking is voltooid met één instructie met de service. Dit omvat de registratie en herregistratie van apparaten en wijzigingen aan de serverkant, zoals het toevoegen en bijwerken van inschrijvingslijsten.
