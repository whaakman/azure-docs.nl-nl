---
title: Aanvraag van de klant gegevensfuncties
author: dominicbetts
ms.author: dobett
ms.date: 05/16/2018
ms.topic: conceptual
ms.service: iot-dps
ms.openlocfilehash: 28cadac33c4e73e6158f878b3c79ff09b4765fff
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
---
# <a name="summary-of-customer-data-request-features"></a>Overzicht van de klant aanvraag gegevensfuncties

De Azure IoT Hub apparaat inrichten-Service is een op basis van REST-API-cloudservice die gericht is op de enterprise-klanten die naadloze, geautomatiseerde inrichting zonder tussenkomst van apparaten met Azure IoT Hub met beveiliging die op het apparaat begint en eindigt op de cloud.

[!INCLUDE [gdpr-intro-sentence](../../includes/gdpr-intro-sentence.md)]

Afzonderlijke apparaten zijn een registratie-ID en apparaat-ID door een tenantbeheerder zijn toegewezen. Gegevens van en over deze apparaten zijn gebaseerd op deze id. Microsoft houdt geen informatie en heeft geen toegang tot gegevens waarmee correlatie van deze apparaten aan een persoon.

Veel van de apparaten die worden beheerd in inrichtingsservice apparaat zijn niet persoonlijke apparaten, bijvoorbeeld een office-thermostaat of factory robot. Klanten kunnen echter rekening houden met bepaalde apparaten aan zijn persoonlijke en hun goeddunken hun eigen asset of Voorraadbeheer methoden voor het koppelen van apparaten naar personen kunnen onderhouden. Inrichtingsservice apparaat beheert, en alle gegevens die zijn gekoppeld aan apparaten als persoonlijke gegevens worden opgeslagen.

Tenantbeheerders om te voldoen aan aanvragen door het exporteren van de Azure portal of REST-API's van de service kunnen gebruiken of verwijderen van gegevens die zijn gekoppeld aan een apparaat-ID of een registratie-ID.

> [!NOTE]
> Apparaten die zijn ingericht in Azure IoT Hub via apparaat inrichtingsservice zijn extra gegevens die zijn opgeslagen in de service Azure IoT Hub. Zie de [Azure IoT Hub-naslagdocumentatie](../iot-hub/iot-hub-customer-data-requests.md) om te kunnen voltooien van een volledige aanvraag voor een bepaald apparaat.

## <a name="deleting-customer-data"></a>Verwijderen van klantgegevens

Apparaat inrichtingsservice opslaat inschrijvingen en registratie records. Inschrijvingen bevatten informatie over apparaten die kunnen worden ingericht en registratie records weergeven die apparaten al het inrichtingsproces hebben doorlopen.

Tenantbeheerders kunnen inschrijvingen verwijderen uit de Azure portal en Hiermee verwijdert u eventuele bijbehorende registratie-records.

Zie voor meer informatie [apparaatinschrijvingen beheren](how-to-manage-enrollments.md).

Het is ook mogelijk om uit te voeren delete-bewerkingen voor inschrijvingen en registratie-records met REST API's:

* U kunt gebruiken voor het verwijderen van registratie-informatie voor één apparaat [Apparaatinschrijving - Delete](https://docs.microsoft.com/rest/api/iot-dps/deviceenrollment/delete).
* U kunt gebruiken voor het verwijderen van registratie-informatie voor een groep apparaten [inschrijving apparaatgroep - Delete](https://docs.microsoft.com/rest/api/iot-dps/deviceenrollmentgroup/delete).
* Als u wilt verwijderen van informatie over apparaten die zijn ingericht, kunt u [registratiestatus - status van de registratie verwijderen](https://docs.microsoft.com/rest/api/iot-dps/registrationstate/deleteregistrationstate).

## <a name="exporting-customer-data"></a>Exporteren van klantgegevens

Apparaat inrichtingsservice opslaat inschrijvingen en registratie records. Inschrijvingen bevatten informatie over apparaten die kunnen worden ingericht en registratie records weergeven die apparaten al het inrichtingsproces hebben doorlopen.

Tenantbeheerders kunnen weergeven inschrijvingen en registratie records via de Azure portal en exporteert u ze met behulp van kopiëren en plakken.

Zie voor meer informatie over het beheren van inschrijvingen [apparaatinschrijvingen beheren](how-to-manage-enrollments.md).

Het is ook mogelijk exportbewerkingen voor inschrijvingen en registratie-records met REST API's uit te voeren:

* Als u wilt exporteren registratie-informatie voor één apparaat, kunt u [Apparaatinschrijving - Get](https://docs.microsoft.com/rest/api/iot-dps/deviceenrollment/get).
* Als u wilt exporteren registratie-informatie voor een groep apparaten, kunt u [inschrijving apparaatgroep - Get](https://docs.microsoft.com/rest/api/iot-dps/deviceenrollmentgroup/get).
* Als u wilt exporteren informatie over apparaten die al zijn ingericht, kunt u [registratiestatus - Get-registratiestatus](https://docs.microsoft.com/rest/api/iot-dps/registrationstate/getregistrationstate).

> [!NOTE]
> Wanneer u van Microsoft enterprise-services gebruikmaakt, genereert Microsoft bepaalde informatie, bekend als het systeem gegenereerde logboeken. Sommige apparaat inrichtingsservice systeem gegenereerde logboeken zijn niet toegankelijk of door tenantbeheerders exporteerbaar. Deze logboeken vormen feitelijke acties die deel uitmaken van de service en diagnostische gegevens met betrekking tot afzonderlijke apparaten.

## <a name="links-to-additional-documentation"></a>Koppelingen naar aanvullende documentatie

Volledige documentatie voor apparaat inrichten Service API's bevindt zich op [ https://docs.microsoft.com/rest/api/iot-dps ](https://docs.microsoft.com/rest/api/iot-dps).

Azure IoT Hub [klantgegevens aanvragen functies](../iot-hub/iot-hub-customer-data-requests.md).