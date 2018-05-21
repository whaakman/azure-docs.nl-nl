---
title: Aanvraag van de klant gegevensfuncties
author: dominicbetts
ms.author: dobett
ms.date: 05/16/2018
ms.topic: conceptual
ms.service: iot-hub
ms.openlocfilehash: 3af76fe22e93b6c5d502733196994bda61b9a93e
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2018
---
# <a name="summary-of-customer-data-request-features"></a>Overzicht van de klant aanvraag gegevensfuncties

De Azure IoT Hub is een op basis van REST-API-cloudservice die gericht is op de enterprise-klanten die beveiligde, bidirectionele communicatie tussen miljoenen apparaten en een gepartitioneerde Azure service.

[!INCLUDE [gdpr-intro-sentence](../../includes/gdpr-intro-sentence.md)]

Afzonderlijke apparaten zijn een apparaat-id (apparaat-ID) door een tenantbeheerder zijn toegewezen. Apparaat is gebaseerd op de toegewezen apparaat-ID. Microsoft houdt geen informatie en heeft geen toegang tot gegevens die apparaat-ID voor de gebruiker correlatie wilt toestaan.

Veel van de apparaten die worden beheerd in Azure IoT Hub zijn niet persoonlijke apparaten, bijvoorbeeld een office-thermostaat of factory robot. Klanten kunnen echter rekening houden met bepaalde apparaten aan zijn persoonlijke en hun goeddunken hun eigen asset of Voorraadbeheer methoden voor het koppelen van apparaten naar personen kunnen onderhouden. Azure IoT Hub beheert en alle gegevens die zijn gekoppeld aan apparaten als persoonlijke gegevens worden opgeslagen.

Tenantbeheerders kunnen de Azure-portal of REST-API's van de service gebruiken om te voldoen aan aanvragen door te exporteren of verwijderen van gegevens die zijn gekoppeld aan een apparaat-ID.

Als u de functie voor de routering van de service Azure IoT Hub gebruiken voor het doorsturen van apparaat-berichten bij andere services, moeten klikt u vervolgens gegevensaanvragen worden uitgevoerd door de tenantbeheerder voor elk eindpunt dat routering om te kunnen voltooien van een volledige aanvraag voor een bepaald apparaat. Zie elk eindpunt-naslagdocumentatie voor meer informatie. Zie voor meer informatie over ondersteunde eindpunten [referentie - IoT-hubeindpunten](iot-hub-devguide-endpoints.md).

Als u de functie van de integratie met Azure Event raster van de service Azure IoT Hub gebruikt, moeten vervolgens gegevensaanvragen worden uitgevoerd door de tenantbeheerder voor elke abonnee van deze gebeurtenissen. Zie voor meer informatie [reageren op gebeurtenissen van de IoT Hub met behulp van de gebeurtenis raster](iot-hub-event-grid.md).

Als u de functie van de integratie met Azure-Monitor van de service Azure IoT Hub maken diagnostische logboeken gebruikt, moeten gegevensaanvragen worden uitgevoerd door de tenantbeheerder met de opgeslagen logboeken. Zie voor meer informatie [de status van Azure IoT Hub](iot-hub-monitor-resource-health.md).

## <a name="deleting-customer-data"></a>Verwijderen van klantgegevens

Tenantbeheerders kunnen de blade IoT-apparaten van de extensie Azure IoT Hub in de Azure portal gebruiken om te verwijderen van een apparaat, de gegevens die zijn gekoppeld aan dat het apparaat te verwijderen.

Het is ook mogelijk om uit te voeren delete-bewerkingen voor apparaten met behulp van REST-API's. Zie voor meer informatie [apparaat-Api - apparaat verwijderen](https://docs.microsoft.com/rest/api/iothub/deviceapi/deletedevice).

## <a name="exporting-customer-data"></a>Exporteren van klantgegevens

Tenantbeheerders kunnen gebruikmaken van kopiëren en plakken binnen de blade IoT-apparaten van de extensie Azure IoT Hub in de Azure portal om gegevens die zijn gekoppeld aan een apparaat te exporteren.

Het is ook mogelijk om te exporteren voor bewerkingen voor apparaten met behulp van REST-API's. Zie voor meer informatie [apparaat-Api - apparaat ophalen](https://docs.microsoft.com/rest/api/iothub/deviceapi/getdevice).

> [!NOTE]
> Wanneer u van Microsoft enterprise-services gebruikmaakt, genereert Microsoft bepaalde informatie, bekend als het systeem gegenereerde logboeken. Sommige Azure IoT Hub systeem gegenereerde logboeken zijn niet toegankelijk of door tenantbeheerders exporteerbaar. Deze logboeken vormen feitelijke acties die deel uitmaken van de service en diagnostische gegevens met betrekking tot afzonderlijke apparaten.

## <a name="links-to-additional-documentation"></a>Koppelingen naar aanvullende documentatie

Volledige documentatie voor Azure IoT Hub apparaat-API's bevindt zich op [ https://docs.microsoft.com/rest/api/iothub/deviceapi ](https://docs.microsoft.com/rest/api/iothub/deviceapi).