---
title: Functies voor aanvraag klant
author: dominicbetts
ms.author: dobett
manager: timlt
ms.date: 05/16/2018
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
ms.openlocfilehash: 5dd027c886e8102e77ddefe93817daee0e1ec29b
ms.sourcegitcommit: 56d20d444e814800407a955d318a58917e87fe94
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/29/2018
ms.locfileid: "52584143"
---
# <a name="summary-of-customer-data-request-features"></a>Overzicht van de klant functies voor aanvraag

De Azure IoT Hub Device Provisioning Service is een op basis van REST-API-cloudservice die gericht is op enterprise-klanten waarmee naadloze, geautomatiseerde zero-touch inrichting van Azure IoT Hub-apparaten met beveiliging die op het apparaat begint en eindigt met de cloud.

[!INCLUDE [gdpr-intro-sentence](../../includes/gdpr-intro-sentence.md)]

Afzonderlijke apparaten zijn een registratie- en apparaat-ID door een tenantbeheerder zijn toegewezen. Gegevens uit en informatie over deze apparaten is gebaseerd op deze id. Microsoft geen gegevens worden bijgehouden en heeft geen toegang tot gegevens die wordt bevraagd correlatie van deze apparaten aan een persoon.

Veel van de apparaten die worden beheerd in Device Provisioning Service zijn niet persoonlijke apparaten, bijvoorbeeld een office-thermostaat of factory robot. Klanten kunnen echter rekening houden met bepaalde apparaten aan zijn persoonlijke en bij naar eigen goeddunken hun eigen asset of inventaris bijhouden methoden voor het verbinden van apparaten naar personen kunnen onderhouden. Device Provisioning Service beheert en alle gegevens die zijn gekoppeld aan apparaten als persoonlijke gegevens worden opgeslagen.

Tenantbeheerders kunnen de Azure-portal of REST-API's van de service gebruiken om te voldoen aan aanvragen door te exporteren of verwijderen van gegevens die zijn gekoppeld aan een apparaat-ID of de registratie-ID.

> [!NOTE]
> Apparaten die zijn ingericht in Azure IoT Hub via Device Provisioning Service hebben extra gegevens die zijn opgeslagen in de Azure IoT Hub-service. Zie de [naslagdocumentatie voor Azure IoT Hub](../iot-hub/iot-hub-customer-data-requests.md) om te kunnen voltooien van een volledige aanvraag voor een bepaald apparaat.

## <a name="deleting-customer-data"></a>Verwijderen van klantgegevens

Device Provisioning Service slaat inschrijvingen en de registratie van records. Inschrijvingen bevatten informatie over apparaten die kunnen worden ingericht en registratie records laten zien welke apparaten al het inrichtingsproces hebben doorlopen.

Tenantbeheerders kunnen inschrijvingen verwijderen uit de Azure-portal en Hiermee verwijdert u ook alle bijbehorende registratie-records.

Zie voor meer informatie, [het apparaatregistraties beheren](how-to-manage-enrollments.md).

Het is ook mogelijk delete-bewerkingen voor inschrijvingen en registratie-records met behulp van REST-API's uit te voeren:

* Als u wilt verwijderen van registratie-informatie voor een enkel apparaat, kunt u [Apparaatinschrijving - verwijderen](/rest/api/iot-dps/deleteindividualenrollment/deleteindividualenrollment).
* Als u wilt verwijderen van registratie-informatie voor een groep apparaten, kunt u [inschrijving apparaatgroep - verwijderen](/rest/api/iot-dps/deleteenrollmentgroup/deleteenrollmentgroup).
* Als u wilt verwijderen van informatie over apparaten die zijn ingericht, kunt u [registratiestatus - status van de apparaatregistratie verwijderen](/rest/api/iot-dps/deletedeviceregistrationstate/deletedeviceregistrationstate).

## <a name="exporting-customer-data"></a>Exporteren van gegevens van de klant

Device Provisioning Service slaat inschrijvingen en de registratie van records. Inschrijvingen bevatten informatie over apparaten die kunnen worden ingericht en registratie records laten zien welke apparaten al het inrichtingsproces hebben doorlopen.

Tenantbeheerders kunnen inschrijvingen en records van de registratie via de Azure-portal weergeven en exporteren met behulp van kopiëren en plakken.

Zie voor meer informatie over het beheren van inschrijvingen [het apparaatregistraties beheren](how-to-manage-enrollments.md).

Het is ook mogelijk uitvoer-bewerkingen voor inschrijvingen en registratie-records met behulp van REST-API's uit te voeren:

* Als u wilt exporteren inschrijvingsgegevens voor één apparaat, kunt u [Apparaatinschrijving - Get](/rest/api/iot-dps/getindividualenrollment/getindividualenrollment).
* Als u wilt exporteren inschrijvingsgegevens voor een groep apparaten, kunt u [inschrijving apparaatgroep - Get](/rest/api/iot-dps/getenrollmentgroup/getenrollmentgroup).
* Als u wilt exporteren informatie over apparaten die al zijn ingericht, kunt u [registratiestatus - Get-registratiestatus](/rest/api/iot-dps/getdeviceregistrationstate/getdeviceregistrationstate).

> [!NOTE]
> Wanneer u van Microsoft enterprise-services gebruikt, genereert Microsoft bepaalde informatie bekend als het systeem gegenereerde logboeken. Sommige Device Provisioning Service systeem gegenereerde logboeken zijn niet toegankelijk of door tenantbeheerders kan worden geëxporteerd. Deze logboeken vormen feitelijke acties die zijn uitgevoerd binnen de service en diagnostische gegevens met betrekking tot afzonderlijke apparaten.

## <a name="links-to-additional-documentation"></a>Koppelingen naar aanvullende documentatie

Volledige documentatie voor Device Provisioning Service-API's bevindt zich in [ https://docs.microsoft.com/rest/api/iot-dps ](https://docs.microsoft.com/rest/api/iot-dps).

Azure IoT Hub [klantgegevens functies aanvragen](../iot-hub/iot-hub-customer-data-requests.md).