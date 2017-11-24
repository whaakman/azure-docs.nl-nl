---
title: De Azure portal gebruiken voor het maken van een IoT Hub | Microsoft Docs
description: "Informatie over het maken, beheren en verwijderen van Azure IoT hubs via de Azure-portal. Bevat informatie over Prijscategorieën, schaalbaarheid, beveiliging en configuratie-berichten."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 0909cd2b-4c1e-49e0-b68a-75532caf0a6a
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/26/2017
ms.author: dobett
ms.openlocfilehash: ac1a52355ffa5354bebe3b98fdb75783bcd57697
ms.sourcegitcommit: 933af6219266cc685d0c9009f533ca1be03aa5e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/18/2017
---
# <a name="create-an-iot-hub-using-the-azure-portal"></a>Een iothub met de Azure portal maken

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

Dit artikel wordt beschreven:

* Hoe de IoT Hub-service niet vinden in de Azure portal.
* Het maken en beheren van IoT hubs.

## <a name="where-to-find-the-iot-hub-service"></a>Waar vind ik de IoT Hub-service

U vindt de IoT Hub-service in de volgende locaties in de portal:

* Kies **+ nieuw**, en kies vervolgens **Internet der dingen**.
* Kies in de Marketplace **Internet der dingen**.

## <a name="create-an-iot-hub"></a>Een IoT Hub maken

U kunt een iothub met behulp van de volgende manieren maken:

* De **+ nieuw** optie opent u de blade die wordt weergegeven in de volgende schermopname. De stappen voor het maken van de IoT-hub via deze methode en via de marketplace zijn identiek.
* Kies in de Marketplace **maken** om de blade wordt weergegeven in de volgende schermopname te openen.

De volgende secties worden de verschillende stappen voor het maken van een IoT-hub:

### <a name="choose-the-name-of-the-iot-hub"></a>Kies de naam van de IoT-hub

U moet de IoT-hub naam voor het maken van een IoT-hub. Deze naam moet uniek zijn in alle IoT hubs.

[!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]

### <a name="choose-the-pricing-tier"></a>Kies de prijscategorie

U kunt kiezen uit vier lagen: **vrije**, **Standard 1** en **standaard 2**, en **Standard-S3**. De gratis categorie kan alleen 500 apparaten zijn verbonden met de iothub en maximaal 8000 berichten per dag.

**Standard-S1**: Gebruik de editie S1 voor IoT-oplossingen met een groot aantal apparaten dat elke kleine hoeveelheden gegevens genereren. Met elke eenheid van de S1-versie kunt u in totaal maximaal 400.000 berichten per dag verzenden voor alle verbonden apparaten.

**Standard S2**: S2 versie gebruiken voor de IoT-oplossingen waarin apparaten grote hoeveelheden gegevens worden gegenereerd. Elke eenheid van de editie S2 kunt 6 miljoen berichten per dag tussen alle verbonden apparaten.

**Standard-S3**: S3 versie gebruiken voor de IoT-oplossingen die grote hoeveelheden gegevens genereren. Elke eenheid van de editie S3 kunt 300 miljoen berichten per dag tussen alle verbonden apparaten.

![][4]

> [!NOTE]
> IoT Hub staat slechts één gratis hub per Azure-abonnement.

### <a name="iot-hub-units"></a>IoT hub-eenheden

Het aantal berichten dat is toegestaan per eenheid per dag, is afhankelijk van uw hub prijscategorie. Als u iothub ter ondersteuning van inkomende berichten 700.000, kiest u bijvoorbeeld twee S1 laag eenheden.

### <a name="device-to-cloud-partitions-and-resource-group"></a>Apparaat cloud partities en resourcegroep

U kunt het aantal partities voor een IoT-hub kunt wijzigen. Het aantal partities is 4, u kunt een ander nummer kiezen uit de vervolgkeuzelijst.

U hoeft niet expliciet een lege resourcegroep te maken. Wanneer u een resource maakt, kunt u kiest u een nieuwe maken of een bestaande resourcegroep gebruiken.

![][5]

### <a name="choose-subscription"></a>Abonnement kiezen

Azure IoT Hub worden automatisch de Azure-abonnementen die aan het gebruikersaccount is gekoppeld. U kunt de Azure-abonnement om te koppelen van de iothub.

### <a name="choose-the-location"></a>Kies de locatie

De optie locatie bevat een lijst van de regio's waar IoT Hub beschikbaar is.

### <a name="create-the-iot-hub"></a>De IoT-hub maken

Wanneer alle voorgaande stappen voltooid zijn, kunt u de IoT-hub kunt maken. Klik op **maken** starten van de back-end-proces voor het maken en implementeren van de iothub met de opties die u hebt gekozen.

Het kan even duren voor het maken van de IoT-hub, omdat het duurt voor de back-end-implementatie uit te voeren op de juiste locatie-servers.

## <a name="change-the-settings-of-the-iot-hub"></a>Wijzig de instellingen van de IoT-hub

U kunt de instellingen van een bestaande IoT-hub wijzigen nadat deze is gemaakt op de blade IoT Hub.

![][8]

**Gedeeld toegangsbeleid**: dit beleid bepaalt de machtigingen voor apparaten en services om te verbinden met IoT Hub. U kunt dit beleid openen door te klikken op **gedeeld toegangsbeleid** onder **algemene**. In deze blade kunt u bestaande beleidsregels wijzigen of toevoegen van een nieuw beleid.

### <a name="create-a-policy"></a>Een beleid maken

* Klik op **toevoegen** om een blade te openen. Hier kunt u de naam van het nieuwe beleid en de machtigingen die u koppelen aan dit beleid, wilt zoals wordt weergegeven in de volgende afbeelding:

    Er zijn enkele machtigingen die gekoppeld aan deze gedeelde beleidsregels worden kunnen. De **register gelezen** en **register schrijven** beleid verlenen van rechten voor lezen en schrijven in het identiteitsregister. Automatisch kiezen van de optie schrijven, kiest de optie voor lezen.

    De **Service verbinding** beleid verleent service-eindpunten, zoals toegang tot **ontvangen van apparaat-naar-cloud**. De **apparaat verbinding kan maken** beleid verleent machtigingen voor het verzenden en ontvangen van berichten met behulp van de IoT Hub apparaat-eindpunten.

* Klik op **maken** deze nieuwe beleid aan de bestaande lijst toevoegen.

![][10]

## <a name="endpoints"></a>Eindpunten

Klik op **eindpunten** om weer te geven een lijst met eindpunten voor de IoT-hub die u wilt wijzigen. Er zijn twee soorten eindpunten: eindpunten die zijn ingebouwd in de IoT-hub en eindpunten die u toevoegt aan de IoT-hub na het maken ervan.

![][11]

### <a name="built-in-endpoints"></a>Ingebouwde eindpunten

Er zijn twee ingebouwde eindpunten: **Cloud naar apparaat feedback** en **gebeurtenissen**.

* **Cloud naar apparaat feedback** instellingen: deze instelling heeft twee subsettings: **Cloud naar apparaat TTL** (time-to-live) en **bewaartijd** (in uren) voor de berichten. Wanneer uw eerste een iothub maakt, zijn deze instellingen voor beide de standaardwaarde van één uur. Deze instellingen aanpassen, gebruikt u de schuifregelaars of typ de waarden.
* **Gebeurtenissen** instellingen: deze instelling heeft verschillende subsettings, waarvan sommige alleen-lezen zijn. De volgende lijst beschrijft deze instellingen:

  * **Partities**: een standaardwaarde is ingesteld wanneer de IoT-hub is gemaakt. U kunt het aantal partities via deze instelling wijzigen.

  * **Event Hub-compatibele naam en een eindpunt**: wanneer de IoT-hub is gemaakt, wordt een Event Hub gemaakt intern mogelijk moet u toegang tot onder bepaalde omstandigheden. De waarden van de Event Hub-compatibele eindpunt en kan niet worden aangepast, maar u kunt deze kopiëren door te klikken op **kopie**.

  * **Bewaartijd**: standaard ingesteld op één dag, maar u kunt wijzigen met behulp van de vervolgkeuzelijst. Deze waarde is in dagen voor de apparaat-naar-cloud-instelling.

  * **Consumergroepen**: consumergroepen inschakelen meerdere lezers om berichten te lezen onafhankelijk van de IoT-hub. Elke iothub is gemaakt met een standaard consumergroep. U kunt echter toevoegen of verwijderen van consumergroepen naar uw IoT-hubs met deze instelling.

  > [!NOTE]
  > De standaardgroep consumer kan niet worden bewerkt of verwijderd.

### <a name="custom-endpoints"></a>Aangepaste eindpunten

U kunt aangepaste eindpunten toevoegen op uw IoT-hub met behulp van de portal. Van de **eindpunten** blade, klikt u op **toevoegen** boven openen de **eindpunt toevoegen** blade. Voer de vereiste gegevens en klik vervolgens op **OK**. Uw aangepaste eindpunt wordt nu weergegeven in het hoofdvenster **eindpunten** blade.

![][13]

Meer informatie over aangepaste eindpunten in [referentie - IoT-hubeindpunten][lnk-devguide-endpoints].

## <a name="routes"></a>Routes

Klik op **Routes** beheren hoe IoT Hub verzendt uw apparaat-naar-cloud-berichten.

![][14]

U kunt routes toevoegen aan uw IoT-hub door te klikken op **toevoegen** boven aan de **Routes*** blade, voert u de vereiste gegevens en op **OK**. De route wordt vervolgens weergegeven in het hoofdvenster **Routes** blade. U kunt een route bewerken door erop te klikken in de lijst met routes. Om een route, klikt u in de lijst met routes op en stel de **ingeschakeld** in-of uitschakelen op **uit**. Klik op om de wijziging, **OK** onderaan de blade.

![][15]

## <a name="pricing-and-scale"></a>Prijs- en schaalniveau

De prijs van een iothub kan worden gewijzigd via de **prijzen** instellingen met de volgende uitzonderingen:

* In de huidige implementatie een iothub met een gratis SKU lagen niet wijzigen in een van de betaalde SKU's, of vice versa.
* Er kan alleen worden één laag gratis IoT-hub in de Azure-abonnement.

![][12]

U kunt verplaatsen van een hogere naar lagere lagen alleen wanneer het aantal berichten die dag het quotum voor de onderste laag overschrijdt. Bijvoorbeeld, als het aantal berichten per dag 400.000 overschrijdt, kan klikt u vervolgens de laag voor de iothub worden gewijzigd. Echter, als u naar de laag S1 wijzigt vervolgens de IoT-hub is beperkt voor die dag.

## <a name="delete-the-iot-hub"></a>Verwijderen van de IoT-hub

U kunt bladeren naar de IoT-hub die u verwijderen wilt door te klikken op **Bladeren**, en vervolgens de juiste hub verwijderen te kiezen. Als u wilt verwijderen van de IoT-hub, klikt u op de **verwijderen** knop onder de naam van de IoT-hub.

## <a name="next-steps"></a>Volgende stappen

Volg deze koppelingen voor meer informatie over het beheren van Azure IoT Hub:

* [Bulksgewijs IoT-apparaten beheren][lnk-bulk]
* [IoT Hub metrische gegevens][lnk-metrics]
* [Bewerkingen controleren][lnk-monitor]

Als u wilt de mogelijkheden van IoT Hub verder verkennen, Zie:

* [Ontwikkelaarshandleiding voor IoT Hub][lnk-devguide]
* [AI implementeren op de edge-apparaten met Azure IoT rand][lnk-iotedge]
* [Beveiligen van uw IoT-oplossing bouwen up][lnk-securing]

[4]: ./media/iot-hub-create-through-portal/create-iothub.png
[5]: ./media/iot-hub-create-through-portal/location1.png
[8]: ./media/iot-hub-create-through-portal/portal-settings.png
[10]: ./media/iot-hub-create-through-portal/shared-access-policies.png
[11]: ./media/iot-hub-create-through-portal/messaging-settings.png
[12]: ./media/iot-hub-create-through-portal/pricing-error.png
[13]: ./media/iot-hub-create-through-portal/endpoint-creation.png
[14]: ./media/iot-hub-create-through-portal/routes-list.png
[15]: ./media/iot-hub-create-through-portal/route-edit.png

[lnk-bulk]: iot-hub-bulk-identity-mgmt.md
[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: ../iot-edge/tutorial-simulate-device-linux.md
[lnk-securing]: iot-hub-security-ground-up.md
[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
