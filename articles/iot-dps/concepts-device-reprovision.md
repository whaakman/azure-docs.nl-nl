---
title: Apparaatconcepten voor beëindiging van de Azure IoT Hub Device Provisioning Service | Microsoft Docs
description: Beschrijving apparaat beëindiging van concepten voor de Azure IoT Hub Device Provisioning Service
author: wesmc7777
ms.author: wesmc
ms.date: 11/14/2018
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: timlt
ms.openlocfilehash: f52e2a1095c329aabf44a846a644cc05548d4df3
ms.sourcegitcommit: a4e4e0236197544569a0a7e34c1c20d071774dd6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2018
ms.locfileid: "51712276"
---
# <a name="iot-hub-device-reprovisioning-concepts"></a>Concepten van IoT Hub Device beëindiging

Tijdens de levenscyclus van een IoT-oplossing is het gebruikelijk dat apparaten verplaatsen tussen IoT-hubs. De redenen voor deze overstap, kunnen de volgende scenario's omvatten:

* **Geolocatie / GeoLatency**: als een apparaat worden verplaatst tussen locaties, netwerklatentie is verbeterd doordat het apparaat gemigreerd naar een dichter bij IoT-hub.

* **Multitenancy**: een apparaat kan worden gebruikt binnen de dezelfde IoT-oplossing en opnieuw toegewezen aan een nieuwe klant of de klant. Deze nieuwe klanten kan worden verwerkt met behulp van een andere IoT-hub.

* **Wijzigen van de oplossing**: een apparaat kan worden verplaatst naar een nieuwe of bijgewerkte IoT-oplossing. Het opnieuw toewijzen is mogelijk het apparaat om te communiceren met een nieuwe IoT-hub die verbonden met andere back-end-onderdelen.

* **In quarantaine plaatsen**: die vergelijkbaar is met het wijzigen van een oplossing. Een apparaat dat functioneert, aangetast of verouderde mogelijk opnieuw worden toegewezen aan een IoT-hub die alleen kan worden bijgewerkt en weer toegang te krijgen in acht genomen. Zodra het apparaat goed werkt, is deze vervolgens terug naar de belangrijkste hub gemigreerd.

Deze vereisten voor ondersteuning van de Device Provisioning Service-adressen voor ingebruikname beëindiging. Apparaten kunnen automatisch worden toegewezen aan nieuwe IoT-hubs op basis van de reprovisioning beleid dat geconfigureerd op de vermelding van de inschrijving van het apparaat.

## <a name="device-state-data"></a>Statusgegevens van apparaat

Statusgegevens van apparaat bestaat uit de [apparaatdubbel](../iot-hub/iot-hub-devguide-device-twins.md) en mogelijkheden van apparaten. Deze gegevens worden opgeslagen in de Device Provisioning Service-exemplaar en de IoT-hub die een apparaat is toegewezen aan.

![Met de Device Provisioning Service inrichten](./media/concepts-device-reprovisioning/dps-provisioning.png)

Wanneer een apparaat in eerste instantie met een Device Provisioning Service-exemplaar is ingericht, worden de volgende stappen worden uitgevoerd:

1. Het apparaat verzendt een aanvraag voor inrichting naar een Device Provisioning Service-exemplaar. Het service-exemplaar verifieert de identiteit van het apparaat op basis van een vermelding voor apparaatinschrijving en de eerste configuratie van de statusgegevens van de apparaat maakt. Het service-exemplaar wordt het apparaat toegewezen aan een IoT-hub op basis van de inschrijvingsconfiguratie en retourneert die IoT hub-toewijzing naar het apparaat.

2. Een kopie van de gegevens van een initiële status van geeft het provisioning service-exemplaar tot de toegewezen IoT-hub. Het apparaat verbinding maakt met de toegewezen IoT-hub en bewerkingen begint.

Na verloop van tijd, de statusgegevens van de apparaten op de IoT-hub kan worden bijgewerkt door [apparaatbewerkingen](../iot-hub/iot-hub-devguide-device-twins.md#device-operations) en [back-end-bewerkingen](../iot-hub/iot-hub-devguide-device-twins.md#back-end-operations). Het eerste apparaat informatie over de status opgeslagen in de Device Provisioning Service-exemplaar blijft ongewijzigd. Deze ongewijzigd status apparaatgegevens is de initiële configuratie.

![Met de Device Provisioning Service inrichten](./media/concepts-device-reprovisioning/dps-provisioning-2.png)

Afhankelijk van het scenario, als een apparaat worden verplaatst tussen IoT-hubs, kan het ook nodig zijn voor het migreren van de status van het apparaat bijgewerkt op de vorige IoT-hub over naar de nieuwe IoT-hub. Deze migratie wordt ondersteund door het beleid in de Device Provisioning Service beëindiging.

## <a name="reprovisioning-policies"></a>Beleid voor beëindiging

Afhankelijk van het scenario verzendt een apparaat meestal een aanvraag naar een provisioning service-exemplaar bij opnieuw opstarten. Het ondersteunt ook een methode voor het handmatig activeren inrichting op aanvraag. Het reprovisioning beleid op een vermelding voor apparaatinschrijving bepaalt hoe de device provisioning service-exemplaar worden verwerkt door deze aanvragen wordt ingericht. Het beleid bepaalt ook of de statusgegevens van het apparaat tijdens de beëindiging moeten worden gemigreerd. Dezelfde beleidsregels zijn beschikbaar voor afzonderlijke inschrijvingen en Registratiegroepen:

* **Opnieuw inrichten en migreren van gegevens**: dit beleid is de standaardinstelling voor nieuwe inschrijving vermeldingen. Dit beleid onderneemt actie wanneer de apparaten die zijn gekoppeld aan de vermelding voor apparaatinschrijving een nieuwe aanvraag (1) te verzenden. Afhankelijk van de configuratie van de vermelding voor inschrijving het apparaat wordt mogelijk opnieuw toegewezen aan een andere IoT-hub. Als het apparaat IoT-hubs wijzigt is, wordt de apparaatregistratie bij de eerste IoT-hub worden verwijderd. De informatie over de status van bijgewerkte apparaten van die eerste IoT-hub worden gemigreerd naar de nieuwe IoT hub (2). Tijdens de migratie van het apparaat de status wordt gerapporteerd als **toewijzen**.

    ![Met de Device Provisioning Service inrichten](./media/concepts-device-reprovisioning/dps-reprovisioning-migrate.png)

* **Opnieuw inrichten en opnieuw ingesteld op initiële configuratie**: dit beleid actie onderneemt wanneer apparaten die zijn gekoppeld aan de vermelding voor apparaatinschrijving een nieuwe aanvraag voor inrichting (1 verzendt). Afhankelijk van de configuratie van de vermelding voor inschrijving het apparaat wordt mogelijk opnieuw toegewezen aan een andere IoT-hub. Als het apparaat IoT-hubs wijzigt is, wordt de apparaatregistratie bij de eerste IoT-hub worden verwijderd. De oorspronkelijke configuratiegegevens die het provisioning service-exemplaar dat is ontvangen tijdens het inrichten van het apparaat is opgegeven voor de nieuwe IoT hub (2). Tijdens de migratie van het apparaat de status wordt gerapporteerd als **toewijzen**.

    Dit beleid wordt vaak gebruikt voor een Fabrieksinstellingen terugzetten zonder te hoeven wijzigen van IoT-hubs.

    ![Met de Device Provisioning Service inrichten](./media/concepts-device-reprovisioning/dps-reprovisioning-reset.png)

* **Nooit herprovisioning**: het apparaat is nooit opnieuw worden toegewezen aan een andere hub. Dit beleid is opgegeven voor het beheren van achterwaartse compatibiliteit.

### <a name="managing-backwards-compatibility"></a>Beheren van achterwaartse compatibiliteit

Toewijzingen van apparaten naar IoT-hubs had vóór September 2018, een sticky gedrag. Wanneer een apparaat is verkeerd terug via het inrichtingsproces, zou deze alleen worden toegewezen terug naar de dezelfde IoT-hub.

Voor oplossingen die een afhankelijkheid op dit gedrag hebt uitgevoerd, bevat de inrichtingsservice achterwaartse compatibiliteit. Dit gedrag wordt momenteel beheerd voor apparaten op basis van de volgende criteria:

1. De apparaten verbinding maken met een API-versie voordat u de beschikbaarheid van systeemeigen ondersteuning voor beëindiging in de Device Provisioning Service. Raadpleeg de volgende API-tabel.

2. De vermelding voor apparaatinschrijving voor de apparaten beschikt niet over een reprovisioning beleid dat is ingesteld op deze.

Deze compatibiliteit zorgt ervoor dat die eerder door u geïmplementeerde apparaten ervaring hetzelfde gedrag dat aanwezig is tijdens de eerste test. Als u wilt behouden in het vorige gedrag, niet een reprovisioning beleid opslaan op deze registraties. Als een reprovisioning beleid is ingesteld, heeft het reprovisioning beleid voorrang op het gedrag. Doordat het reprovisioning beleid om voorrang te kunnen klanten gedrag van het apparaat bijwerken zonder te hoeven de installatiekopie van het apparaat terugzetten.

Het volgende stroomdiagram helpt om weer te geven wanneer het gedrag aanwezig is:

![achterwaartse compatibiliteit stroomdiagram](./media/concepts-device-reprovisioning/reprovisioning-compatibility-flow.png)

De volgende tabel ziet u de API-versies voor de beschikbaarheid van systeemeigen ondersteuning voor beëindiging in de Device Provisioning Service:

| REST-API | C-SDK | Python-SDK |  Node SDK | Java-SDK | .NET SDK |
| -------- | ----- | ---------- | --------- | -------- | -------- |
| [2018-04-01 en oudere versies](/rest/api/iot-dps/createorupdateindividualenrollment/createorupdateindividualenrollment#uri-parameters) | [1.2.8 en lager](https://github.com/Azure/azure-iot-sdk-c/blob/master/version.txt) | [1.4.2 en lager](https://github.com/Azure/azure-iot-sdk-python/blob/0a549f21f7f4fc24bc036c1d2d5614e9544a9667/device/iothub_client_python/src/iothub_client_python.cpp#L53) | [1.7.3 of eerder](https://github.com/Azure/azure-iot-sdk-node/blob/074c1ac135aebb520d401b942acfad2d58fdc07f/common/core/package.json#L3) | [1.13.0 of eerder](https://github.com/Azure/azure-iot-sdk-java/blob/794c128000358b8ed1c4cecfbf21734dd6824de9/device/iot-device-client/pom.xml#L7) | [1.1.0 of eerder](https://github.com/Azure/azure-iot-sdk-csharp/blob/9f7269f4f61cff3536708cf3dc412a7316ed6236/provisioning/device/src/Microsoft.Azure.Devices.Provisioning.Client.csproj#L20)

> [!NOTE]
> Deze waarden en koppelingen zijn waarschijnlijk zal veranderen. Dit is slechts een tijdelijke aanduiding poging om te bepalen waar de versies door een klant kunnen worden bepaald en wat de verwachte versies zal zijn.

## <a name="next-steps"></a>Volgende stappen

* [Het opnieuw inrichten van apparaten](how-to-reprovision.md)
