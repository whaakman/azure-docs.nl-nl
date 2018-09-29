---
title: Functies voor aanvraag klant
description: Overzicht van de klant functies voor aanvraag
author: dominicbetts
ms.author: dobett
manager: timlt
ms.date: 05/16/2018
ms.topic: conceptual
ms.service: iot-hub
services: iot-hub
ms.openlocfilehash: 862ed2b38521cac3a4c83a9dc90db78f9f030ff1
ms.sourcegitcommit: f31bfb398430ed7d66a85c7ca1f1cc9943656678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/28/2018
ms.locfileid: "47451813"
---
# <a name="summary-of-customer-data-request-features"></a>Overzicht van de klant functies voor aanvraag

De Azure IoT Hub is een op basis van REST-API-cloudservice die gericht is op enterprise-klanten waarmee veilige bidirectionele communicatie tussen miljoenen apparaten en een gepartitioneerde Azure-service.

[!INCLUDE [gdpr-related guidance](../../includes/gdpr-intro-sentence.md)]

Afzonderlijke apparaten zijn een apparaat-id (apparaat-ID) door een tenantbeheerder zijn toegewezen. Apparaatgegevens is gebaseerd op het toegewezen apparaat-ID. Microsoft geen gegevens worden bijgehouden en heeft geen toegang tot gegevens waarmee apparaat-ID van gebruiker correlatie.

Veel van de apparaten die worden beheerd in Azure IoT Hub zijn niet persoonlijke apparaten, bijvoorbeeld een office-thermostaat of factory robot. Klanten kunnen echter rekening houden met bepaalde apparaten aan zijn persoonlijke en bij naar eigen goeddunken hun eigen asset of inventaris bijhouden methoden voor het verbinden van apparaten naar personen kunnen onderhouden. Azure IoT Hub beheert en alle gegevens die zijn gekoppeld aan apparaten als persoonlijke gegevens worden opgeslagen.

Tenantbeheerders kunnen de Azure-portal of REST-API's van de service gebruiken om te voldoen aan aanvragen door te exporteren of verwijderen van gegevens die zijn gekoppeld aan een apparaat-ID.

Als u de routering-functie van de service Azure IoT Hub gebruikt voor het doorsturen van apparaat-berichten naar andere services, moeten klikt u vervolgens gegevensaanvragen worden uitgevoerd door de tenantbeheerder voor elk eindpunt routering om te kunnen voltooien van een volledige aanvraag voor een bepaald apparaat. Zie voor meer informatie de documentatie voor elk eindpunt. Zie voor meer informatie over ondersteunde eindpunten, [referentie - eindpunten van IoT-Hub](iot-hub-devguide-endpoints.md).

Als u de functie voor Azure Event Grid-integratie van de Azure IoT Hub-service gebruikt, moeten klikt u vervolgens gegevensaanvragen worden uitgevoerd door de tenantbeheerder voor abonnees van deze gebeurtenissen. Zie voor meer informatie, [reageren op gebeurtenissen van IoT-Hub met behulp van Event Grid](iot-hub-event-grid.md).

Als u de functie van de integratie met Azure Monitor van de service Azure IoT Hub te maken diagnostische logboeken gebruikt, moeten gegevensaanvragen worden uitgevoerd door de tenantbeheerder op basis van de opgeslagen logboeken. Zie voor meer informatie, [bewaak de status van Azure IoT Hub](iot-hub-monitor-resource-health.md).

## <a name="deleting-customer-data"></a>Verwijderen van klantgegevens

Tenantbeheerders kunnen de blade voor IoT-apparaten van de Azure IoT Hub-extensie gebruiken in Azure portal om te verwijderen van een apparaat waardoor de gegevens die zijn gekoppeld aan dat het apparaat worden verwijderd.

Het is ook mogelijk delete-bewerkingen voor apparaten met behulp van REST-API's uit te voeren. Zie voor meer informatie, [Service - apparaat verwijderen](/rest/api/iothub/service/deletedevice).

## <a name="exporting-customer-data"></a>Exporteren van gegevens van de klant

Tenantbeheerders kunnen gebruikmaken van kopiëren en plakken in de blade voor IoT-apparaten van de Azure IoT Hub-extensie in de Azure-portal voor het exporteren van gegevens die zijn gekoppeld aan een apparaat.

Het is ook mogelijk om uit te voeren exportbewerkingen voor apparaten met behulp van REST-API's. Zie voor meer informatie, [Service - apparaat opvragen](/rest/api/iothub/service/getdevice).

> [!NOTE]
> Wanneer u van Microsoft enterprise-services gebruikt, genereert Microsoft bepaalde informatie bekend als het systeem gegenereerde logboeken. Sommige Azure IoT Hub het systeem gegenereerde logboeken zijn niet toegankelijk of door tenantbeheerders kan worden geëxporteerd. Deze logboeken vormen feitelijke acties die zijn uitgevoerd binnen de service en diagnostische gegevens met betrekking tot afzonderlijke apparaten.

## <a name="links-to-additional-documentation"></a>Koppelingen naar aanvullende documentatie

Volledige documentatie voor Azure IoT Hub-Service-API's bevindt zich in [IoT Hub-Service-API's](https://docs.microsoft.com/rest/api/iothub/service).
