---
title: Service-concepten in Azure IoT Hub apparaat-inrichtingsservice | Microsoft Docs
description: Beschrijft service inrichting concepten specifiek zijn voor apparaten met DP's en IoT-Hub
services: iot-dps
keywords: 
author: nberdy
ms.author: nberdy
ms.date: 10/03/2017
ms.topic: article
ms.service: iot-dps
documentationcenter: 
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 96c63e5d0379150ea619dbbe912a21e373f808af
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="iot-hub-device-provisioning-service-concepts"></a>Inrichting-Service van IoT Hub apparaat-concepten

IoT Hub apparaat-inrichtingsservice is een helper-service voor IoT-Hub die u gebruikt om zonder tussenkomst apparaat inrichten met een opgegeven IoT-hub te configureren. U kunt met de Service apparaat inrichten miljoenen apparaten inrichten op een veilige en schaalbare manier.

Mobiele apparaten inrichten bestaat uit twee fasen. Het eerste deel is tot stand brengen van de eerste verbinding tussen het apparaat en IoT-oplossing door *registreren* het apparaat. Het tweede gedeelte toepast de juiste *configuratie* op het apparaat op basis van de specifieke vereisten van de oplossing. Wanneer beide stappen hebt voltooid, je het apparaat volledig is *ingericht*. Apparaat inrichtingsservice automatiseert beide stappen om een naadloze ervaring voor de inrichting van het apparaat.

In dit artikel biedt een overzicht van de inrichting concepten meest van toepassing op het beheren van de *service*. Dit artikel is het meest relevant voor Persona's die zijn betrokken bij de [cloud instellingsstap](about-iot-dps.md#cloud-setup-step) van een apparaat voorbereiden voor implementatie.

## <a name="service-operations-endpoint"></a>Operations-service-eindpunt

Het service-eindpunt voor bewerkingen is het eindpunt voor de service-instellingen beheren en onderhouden van de lijst van de inschrijving. Dit eindpunt wordt alleen gebruikt door de servicebeheerder; Dit wordt niet gebruikt door apparaten.

## <a name="device-provisioning-endpoint"></a>Inrichting endpoint van apparaat

Het apparaat eindpunt inrichting is het centrale eindpunt dat alle apparaten met voor het inrichten communiceren. De URL is hetzelfde voor alle inrichting services die overbodig voor apparaten met nieuwe verbindingsgegevens reflash in levering keten scenario's. De [ID-bereik](#id-scope) zorgt ervoor dat de isolatie van tenants.

## <a name="linked-iot-hubs"></a>Gekoppelde IoT-hubs

Apparaat inrichtingsservice kunt IoT-hubs die zijn gekoppeld aan deze apparaten inrichten. Koppelen van een IoT-hub aan inrichtingsservice apparaat, krijgt de machtigingen voor het lezen/schrijven van service aan het apparaatregister van de IoT-hub; met de koppeling inrichtingsservice apparaat registreren van een apparaat-ID en de eerste configuratie ingesteld in de apparaat-twin. Gekoppelde IoT hubs kunnen zich in een Azure-regio. U kunt hubs in andere abonnementen koppelingen naar uw provisioning-service.

## <a name="allocation-policy"></a>Toewijzingsbeleid voor

Het serviceniveau-instelling die bepaalt hoe apparaat inrichtingsservice apparaten wijst naar een iothub. Er zijn drie ondersteunde toewijzingsbeleid:
* **Gelijkmatig gewogen distributie**: gekoppelde IoT hubs zijn waarschijnlijk gelijkmatig ingericht op deze apparaten hebben. De standaardinstelling. Als u apparaten met slechts één IoT-hub inricht, kunt u deze instelling bewaren.
* **Laagste latentie**: apparaten die zijn ingericht naar een iothub met de laagste latentie voor het apparaat. Als meerdere gekoppelde IoT hubs kan dezelfde laagste latentie opgeven wilt, hash de inrichting service-apparaten via deze hubs
* **Statische configuratie via de lijst inschrijving**: specificatie van de gewenste IoT-hub in de lijst inschrijving heeft voorrang boven het serviceniveau toewijzingsbeleid voor.

## <a name="enrollment"></a>Inschrijving

Een van de inschrijving is de registratie van apparaten of groepen van apparaten die op sommige registreren serviceverbindingspunt mogelijk. De registratie-record bevat informatie over het apparaat of een groep apparaten, met inbegrip van de attestation-methode voor de apparaten en eventueel eerste gewenste configuratie IoT gewenst hub en gewenste apparaat-id. Er zijn twee soorten inschrijvingen ondersteund door de Service voor apparaten inrichten.

### <a name="enrollment-group"></a>Inschrijvingsgroep

Een registratie-groep is een groep apparaten die een specifieke attestation-mechanisme delen. Alle apparaten in de inschrijvingsgroep X.509-certificaten die zijn ondertekend door de dezelfde basis-CA aanwezig. Inschrijving groepen kunnen alleen het X.509-mechanisme voor attestation gebruikt. De groepsnaam van de inschrijving en de naam van het certificaat moeten alfanumerieke, kleine letters en kunnen de afbreekstreepjes bevatten.

> [!TIP]
> Aangeraden wordt een inschrijvingsgroep voor voor een groot aantal apparaten die een gewenste initiële configuratie delen of voor apparaten alle gaat dezelfde tenant.

### <a name="individual-enrollment"></a>Afzonderlijke inschrijving

De inschrijving van een afzonderlijke is een vermelding voor één apparaat kan registreren. Afzonderlijke inschrijvingen mag x.509-certificaten of SAS-tokens (in een echt of virtueel TPM) gebruiken zoals attestation mechanismen. De registratie-ID in een afzonderlijke inschrijving is alfanumerieke, kleine letters en kan de afbreekstreepjes bevatten. Afzonderlijke inschrijvingen wellicht de gewenste IoT hub apparaat-ID opgegeven.

> [!TIP]
> U wordt aangeraden met afzonderlijke inschrijvingen voor apparaten die zijn uniek initiële configuraties vereist of voor apparaten die alleen via TPM of virtuele TPM SAS-tokens als het mechanisme voor attestation kunnen gebruiken.

## <a name="registration"></a>Registratie

Een registratie is de record van een apparaat is geregistreerd/inrichting naar een IoT-Hub via apparaat inrichtingsservice. Registratie-records worden automatisch gemaakt. ze kunnen worden verwijderd, maar kan niet worden bijgewerkt.

## <a name="operations"></a>Bewerkingen

Bewerkingen zijn de facturering eenheid van het apparaat inrichtingsservice. Een bewerking is voltooid met één instructie met de service. Dit omvat de registratie en herregistratie van apparaten en wijzigingen aan de serverkant, zoals het toevoegen en bijwerken van inschrijvingslijsten.
