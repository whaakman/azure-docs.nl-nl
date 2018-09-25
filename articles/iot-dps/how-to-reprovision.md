---
title: Het opnieuw inrichten van apparaten in de Azure IoT Hub Device Provisioning Service | Microsoft Docs
description: Hoe u apparaten met uw device provisioning service-exemplaar opnieuw inrichten
author: wesmc7777
ms.author: wesmc
ms.date: 08/15/2018
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: timlt
ms.openlocfilehash: 36f919d1c22a88dfaf13079f09e6a43980a22828
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46981777"
---
# <a name="how-to-reprovision-devices"></a>Het opnieuw inrichten van apparaten

Tijdens de levenscyclus van een IoT-oplossing is het gebruikelijk dat apparaten verplaatsen tussen IoT-hubs. De redenen voor deze overstap, kunnen de volgende scenario's omvatten:

* **Geolocatie**: als een apparaat worden verplaatst tussen locaties, netwerklatentie is verbeterd doordat het apparaat gemigreerd naar een IoT hub dichter naar elke locatie.

* **Multitenancy**: een apparaat kan worden gebruikt binnen de dezelfde IoT-oplossing, maar, toegewezen of in lease gegeven naar een nieuwe klant of een klantsite. Deze nieuwe klanten kan worden verwerkt met behulp van een andere IoT-hub.

* **Wijzigen van de oplossing**: een apparaat kan worden verplaatst naar een nieuwe of bijgewerkte IoT-oplossing. Het opnieuw toewijzen is mogelijk dat het apparaat met een nieuwe IoT-hub die is verbonden met andere back-end-onderdelen communiceren. 

* **In quarantaine plaatsen**: die vergelijkbaar is met het wijzigen van een oplossing. Een apparaat dat is niet goed functioneert, waarmee is geknoeid of verouderde mogelijk opnieuw worden toegewezen aan een IoT-hub waar mee kunt doen, is bijgewerkt en weer toegang te krijgen in naleving. Zodra het apparaat goed werkt, wordt deze vervolgens terug naar de belangrijkste hub gemigreerd.

Zie voor meer een meer gedetailleerd overzicht van beëindiging, [concepten van IoT Hub Device reprovisoning](concepts-device-reprovision.md).


## <a name="configure-the-enrollment-allocation-policy"></a>Het toewijzingsbeleid inschrijving configureren

Het toewijzingsbeleid bepaalt hoe de apparaten die zijn gekoppeld aan de inschrijving wordt toegewezen, of toegewezen, naar een IoT-hub eenmaal is ingericht.

De volgende stappen uit configureren het toewijzingsbeleid voor inschrijving van een apparaat:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) en navigeer naar uw Device Provisioning Service-exemplaar.

2. Klik op **registraties beheren**, en klik op de registratiegroep of afzonderlijke inschrijving die u configureren wilt voor beëindiging. 

3. Onder **selecteert u hoe u apparaten toewijzen aan hubs wilt**, selecteer een van de volgende toewijzingsbeleid voor:

    * **Laagste latentie**: dit beleid apparaten toegewezen aan de gekoppelde IoT-Hub die in de laagste latentie communicatie tussen apparaat en IoT-Hub resulteren. Deze optie kunt het apparaat om te communiceren met de dichtstbijzijnde IoT-hub op basis van locatie. 
    
    * **Gelijk gewogen distributie**: dit beleid worden apparaten verdeeld voor de gekoppelde IoT-Hubs op basis van de toewijzingsgewicht toegewezen aan elke gekoppelde IoT-hub. Dit beleid kunt u saldo apparaten laden voor een groep van gekoppelde hubs op basis van de toewijzing van waarden instellen voor deze hubs. Als u apparaten voor slechts één IoT-Hub inricht, wordt aangeraden deze instelling. Dit is de standaardinstelling. 
    
    * **Statische configuratie**: dit beleid vereist een gewenste IoT-Hub worden vermeld in de inschrijvingsvermelding voor een apparaat in te richten. Dit beleid kunt u om één specifieke IoT-hub die u wilt dat apparaten toewijzen aan toe te wijzen.

4. Onder **selecteert u deze groep kan worden toegewezen aan de IoT-hubs**, selecteert u de gekoppelde IoT-hubs die u wilt opnemen met uw toewijzingsbeleid. (Optioneel) Voeg een nieuwe gekoppelde Iot-hub met behulp de **een nieuwe IoT-Hub koppelen** knop.

    Met de **laagste latentie** toewijzingsbeleid, de hubs die u selecteert worden opgenomen in de evaluatie latentie om te bepalen van de dichtstbijzijnde hub voor apparaattoewijzing.

    Met de **gelijk gewogen distributie** toewijzingsbeleid, apparaten, worden verdeeld over de hubs die u selecteert op basis van hun gewichten geconfigureerde toewijzing en hun huidige apparaat laden.

    Met de **statische configuratie** toewijzingsbeleid, selecteert u de IoT-hub die u wilt dat apparaten die zijn toegewezen aan.

4. Klik op **opslaan**, of gaat u verder met de volgende sectie om in te stellen het reprovisioning beleid.

    ![Toewijzingsbeleid voor inschrijving selecteren](./media/how-to-reprovision/enrollment-allocation-policy.png)



## <a name="set-the-reprovisioning-policy"></a>Het reprovisioning beleid instellen

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) en navigeer naar uw Device Provisioning Service-exemplaar.

2. Klik op **registraties beheren**, en klik op de registratiegroep of afzonderlijke inschrijving die u configureren wilt voor beëindiging.

3. Onder **selecteert u hoe u gegevens van het apparaat moet worden verwerkt op herinrichting voor verschillende IoT-hub wilt**, kies een van de volgende reprovisioning beleidsregels:

    * **Opnieuw inrichten en migreren van gegevens**: dit beleid actie onderneemt wanneer apparaten die zijn gekoppeld aan de vermelding voor apparaatinschrijving een nieuwe aanvraag voor inrichting verzendt. Afhankelijk van de configuratie van de vermelding voor inschrijving het apparaat wordt mogelijk opnieuw toegewezen aan een andere IoT-hub. Als het apparaat IoT-hubs wijzigt is, wordt de apparaatregistratie bij de eerste IoT-hub worden verwijderd. Alle informatie over de apparaatstatus in die eerste IoT-hub worden gemigreerd naar de nieuwe IoT-hub. Tijdens de migratie van het apparaat de status wordt gerapporteerd als **toewijzen**

    * **Opnieuw inrichten en opnieuw ingesteld op initiële configuratie**: dit beleid actie onderneemt wanneer apparaten die zijn gekoppeld aan de vermelding voor apparaatinschrijving een nieuwe aanvraag voor inrichting verzendt. Afhankelijk van de configuratie van de vermelding voor inschrijving het apparaat wordt mogelijk opnieuw toegewezen aan een andere IoT-hub. Als het apparaat IoT-hubs wijzigt is, wordt de apparaatregistratie bij de eerste IoT-hub worden verwijderd. De oorspronkelijke configuratiegegevens die het provisioning service-exemplaar dat is ontvangen tijdens het inrichten van het apparaat is opgegeven voor de nieuwe IoT hub. Tijdens de migratie van het apparaat de status wordt gerapporteerd als **toewijzen**.

4. Klik op **opslaan** om in te schakelen de beëindiging van het apparaat op basis van uw wijzigingen.

    ![Toewijzingsbeleid voor inschrijving selecteren](./media/how-to-reprovision/reprovisioning-policy.png)



## <a name="send-a-provisioning-request-from-the-device"></a>Een aanvraag voor inrichting van het apparaat verzenden

In de volgorde voor apparaten om te worden ingericht op basis van de configuratiewijzigingen in de voorgaande secties, deze apparaten moeten aanvragen beëindiging. 

Hoe vaak een apparaat een aanvraag voor inrichting verzendt, is afhankelijk van het scenario. Echter, is het raadzaam om uw apparaten voor het verzenden van een aanvraag voor inrichting naar een provisioning service-exemplaar opnieuw opstarten en ondersteuning te programmeren een [methode](../iot-hub/iot-hub-devguide-direct-methods.md) handmatig activeren inrichting op aanvraag. Inrichting kan ook worden geactiveerd door in te stellen een [gewenste eigenschap](../iot-hub/iot-hub-devguide-device-twins.md#desired-property-example). 

Het reprovisioning beleid op een vermelding voor apparaatinschrijving bepaalt hoe deze inrichting aanvragen worden verwerkt door de device provisioning service-exemplaar, en als de statusgegevens van het apparaat tijdens de beëindiging moeten worden gemigreerd. Dezelfde beleidsregels zijn beschikbaar voor afzonderlijke inschrijvingen en Registratiegroepen:

Bijvoorbeeld code van het verzenden van aanvragen van een apparaat inrichten tijdens een boot-volgorde, Zie [een gesimuleerd apparaat automatische inrichting](quick-create-simulated-device.md).


## <a name="next-steps"></a>Volgende stappen

- Zie voor meer meer Reprovisioning [reprovisoning concepten van IoT Hub-apparaat](concepts-device-reprovision.md) 
- Zie voor meer meer opheffen van inrichting [hoe u de inrichting van apparaten die zijn eerder automatisch ingericht ](how-to-unprovision-devices.md) 











