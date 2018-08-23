---
title: De Azure portal gebruiken voor het maken van een IoT-Hub | Microsoft Docs
description: Het maken, beheren en verwijderen van Azure IoT hubs via de Azure-portal. Bevat informatie over Prijscategorieën, schaling, beveiliging en configuratie van berichten.
author: dominicbetts
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/01/2018
ms.author: dobett
ms.openlocfilehash: 0b03ae434e93dbab45235fe67c499497e1257064
ms.sourcegitcommit: 1aedb52f221fb2a6e7ad0b0930b4c74db354a569
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/17/2018
ms.locfileid: "42055109"
---
# <a name="create-an-iot-hub-using-the-azure-portal"></a>Een IoT-hub met behulp van de Azure portal maken

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

Dit artikel wordt beschreven:

* Over het vinden van de IoT Hub-service in Azure portal.
* Over het maken en beheren van IoT-hubs.

## <a name="where-to-find-the-iot-hub-service"></a>Waar vind ik de IoT Hub-service

U vindt de IoT Hub-service in de volgende locaties in de portal:

* Kies **+ nieuw**, en kies vervolgens **Internet of Things**.
* Kies in de Marketplace **Internet of Things**.

## <a name="create-an-iot-hub"></a>Een IoT Hub maken

U kunt een IoT-hub met behulp van de volgende manieren maken:

* De **+ nieuw** optie opent u de blade die wordt weergegeven in de volgende schermopname. De stappen voor het maken van de IoT-hub via deze methode en via de marketplace zijn identiek.

* Kies in de Marketplace **maken** om de blade die wordt weergegeven in de volgende schermopname te openen.

De volgende secties worden de verschillende stappen voor het maken van een IoT-hub.

### <a name="choose-the-name-of-the-iot-hub"></a>Kies de naam van de IoT-hub

Voor het maken van een IoT-hub, moet u de naam de IoT-hub. Deze naam moet uniek zijn voor alle IoT-hubs.

[!INCLUDE [iot-hub-pii-note-naming-hub](../../includes/iot-hub-pii-note-naming-hub.md)]

### <a name="choose-the-pricing-tier"></a>Kies de prijscategorie

U kunt kiezen uit verschillende categorieën, afhankelijk van hoeveel functies die u wilt en het aantal berichten die u via uw oplossing per dag verzenden. De gratis laag is bedoeld voor test- en evaluatiedoeleinden. Hierdoor kan 500 apparaten zijn verbonden met de IoT-hub en tot wel 8000 berichten per dag. Elk Azure-abonnement kunt maken van een IoT-Hub in de gratis laag. 

Zie voor meer informatie over de andere opties voor de laag [kiezen van de juiste IoT Hub-laag](iot-hub-scaling.md).

### <a name="iot-hub-units"></a>IoT hub-eenheden

Het aantal berichten dat is toegestaan per eenheid per dag, is afhankelijk van de prijscategorie van uw hub. Als u wilt dat de IoT-hub voor invoer van 700.000 berichten, kiest u bijvoorbeeld twee eenheden van S1-laag.

### <a name="device-to-cloud-partitions-and-resource-group"></a>Apparaat naar cloud-partities en de resourcegroep

U kunt het aantal partities voor een IoT-hub kunt wijzigen. Het aantal partities is 4; u kunt een ander aantal kiezen uit de vervolgkeuzelijst.

U hoeft niet expliciet een lege resourcegroep maken. Wanneer u een resource maakt, kunt u kiezen uit om een nieuwe resourcegroep te maken of een bestaande resourcegroep gebruiken.

![Schermopname van het maken van een hub in Azure portal](./media/iot-hub-create-through-portal/location1.png)

### <a name="choose-subscription"></a>Abonnement kiezen

Azure IoT Hub worden automatisch de Azure-abonnementen die aan het gebruikersaccount dat is gekoppeld. U kunt het Azure-abonnement om te koppelen van de IoT-hub te kiezen.

### <a name="choose-the-location"></a>Kies de locatie

De optie locatie bevat een lijst van de regio's waar de IoT Hub beschikbaar is.

### <a name="create-the-iot-hub"></a>De IoT-hub maken

Wanneer alle vorige stappen voltooid zijn, kunt u de IoT-hub maken. Klik op **maken** om de back-end-proces voor het maken en implementeren van de IoT-hub met de opties die u hebt gekozen te starten.

Het kan maken van de IoT-hub, aangezien het enige tijd voor de implementatie van de back-end om uit te voeren op de juiste locatie-servers in een paar minuten duren.

## <a name="change-the-settings-of-the-iot-hub"></a>Wijzig de instellingen van de IoT-hub
<!--robinsh these screenshots are out of date -->

U kunt de instellingen van een bestaande IoT-hub wijzigen nadat deze is gemaakt op de blade IoT Hub.

![Schermopname van de instellingen voor de IoT-hub](./media/iot-hub-create-through-portal/portal-settings.png)

**Gedeeld toegangsbeleid**: deze beleidsregels definiëren de machtigingen voor apparaten en services om te verbinden met IoT Hub. U kunt deze beleidsregels openen door te klikken **beleid voor gedeelde toegang** onder **algemene**. In deze blade kunt u bestaande beleidsregels wijzigen of een nieuw beleid toevoegen.

### <a name="create-a-policy"></a>Een beleid maken

* Klik op **toevoegen** om een blade te openen. Hier kunt u de naam van het nieuwe beleid en de machtigingen die u koppelen aan dit beleid, wilt zoals wordt weergegeven in de volgende afbeelding:

    Er zijn enkele machtigingen die gekoppeld aan deze gedeelde beleidsregels worden kunnen. De **register lezen** en **register schrijven** beleid lezen en schrijven rechten toekennen voor het id-register. Automatisch kiezen van de optie voor het schrijven, kiest de optie voor lezen.

    De **Service verbinding maken met** beleid een machtiging verleend voor toegang tot service-eindpunten, zoals **ontvangen van apparaat-naar-cloud**. De **apparaat verbinding maken met** beleid verleent machtigingen voor het verzenden en ontvangen van berichten met behulp van de IoT Hub apparaat-side-eindpunten.

* Klik op **maken** om toe te voegen zojuist gemaakte beleid aan de bestaande lijst.

   ![Schermafbeelding van een beleid voor gedeelde toegang toevoegen](./media/iot-hub-create-through-portal/shared-access-policies.png)

## <a name="endpoints"></a>Eindpunten

Klik op **eindpunten** om weer te geven van een lijst met eindpunten voor de IoT-hub die u wilt wijzigen. Er zijn twee soorten eindpunten: eindpunten die zijn ingebouwd in de IoT-hub en eindpunten die u toevoegt aan de IoT-hub nadat het is gemaakt.

![Schermafbeelding van een eindpunt toevoegen](./media/iot-hub-create-through-portal/messaging-settings.png)

### <a name="built-in-endpoints"></a>Ingebouwde eindpunten

Er zijn twee ingebouwde eindpunten: **Cloud naar apparaat feedback** en **gebeurtenissen**.

* **Cloud naar apparaat feedback** instellingen: deze instelling heeft twee subsettings: **Cloud naar apparaat TTL** (time-to-live) en **bewaartijd** (in uren) voor de berichten. Wanneer uw eerste een IoT-hub maakt, hebben beide deze instellingen de standaardwaarde van één uur. Deze instellingen aanpassen, gebruikt u de schuifregelaars of typt u de waarden.

* **Gebeurtenissen** instellingen: deze instelling heeft verschillende subsettings, waarvan sommige alleen-lezen zijn. De volgende lijst beschrijft deze instellingen:

  * **Partities**: een standaardwaarde is ingesteld wanneer de IoT-hub wordt gemaakt. U kunt het aantal partities via deze instelling wijzigen.

  * **Event Hub-compatibele naam en het eindpunt**: wanneer de IoT hub is gemaakt, wordt een Event Hub gemaakt intern mogelijk moet u toegang tot onder bepaalde omstandigheden. U kunt de waarden voor de Event Hub-compatibele naam en -eindpunt niet aanpassen, maar u kunt deze kopiëren door te klikken op **kopie**.

  * **Bewaartijd**: standaard ingesteld op één dag, maar u kunt wijzigen met behulp van de vervolgkeuzelijst. Deze waarde is in dagen voor de instelling van de apparaat-naar-cloud.

  * **Consumentengroepen**: groepen consumenten inschakelen meerdere lezers om berichten te lezen onafhankelijk van de IoT-hub. Elke IoT hub wordt gemaakt met een standaard-consumergroep. U kunt echter toevoegen of verwijderen van consumentengroepen naar uw IoT-hubs met behulp van deze instelling.

  > [!NOTE]
  > De standaardgroep van de consument kan niet worden bewerkt of verwijderd.

### <a name="custom-endpoints"></a>Aangepaste eindpunten

U kunt aangepaste eindpunten toevoegen op uw IoT-hub met behulp van de portal. Uit de **eindpunten** blade, klikt u op **toevoegen** aan de bovenkant om te openen de **eindpunt toevoegen** blade. Voer de vereiste gegevens en klik vervolgens op **OK**. Uw aangepast eindpunt is nu opgenomen in het hoofdvenster **eindpunten** blade.

![Schermopname van het maken van een aangepast eindpunt](./media/iot-hub-create-through-portal/endpoint-creation.png)

Meer informatie over aangepaste eindpunten in [referentie - eindpunten van IoT-hub]( iot-hub-devguide-endpoints.md).

## <a name="routes"></a>Routes

Klik op **Routes** om te beheren hoe IoT Hub apparaat-naar-cloud-berichten verzendt.

![Schermafbeelding van een nieuwe route toevoegen](./media/iot-hub-create-through-portal/routes-list.png)

U kunt routes toevoegen aan uw IoT-hub door te klikken op **toevoegen** aan de bovenkant van de **Routes*** blade, de vereiste gegevens op te geven en te klikken op **OK**. Uw route wordt vervolgens weergegeven in het hoofdvenster **Routes** blade. U kunt een route bewerken door erop te klikken in de lijst van routes. Als u wilt een route inschakelen, klikt u in de lijst met routes op en stel de **ingeschakeld** overzet naar **uit**. Om de wijziging hebt opgeslagen, klikt u op **OK** aan de onderkant van de blade.

![Schermopname van het bewerken van een nieuwe regel voor doorsturen](./media/iot-hub-create-through-portal/route-edit.png)

## <a name="delete-the-iot-hub"></a>De IoT-hub verwijderen

U kunt bladeren naar de IoT-hub die u verwijderen wilt door te klikken op **Bladeren**, en kies de juiste hub te verwijderen. Als u wilt verwijderen van de IoT-hub, klikt u op de **verwijderen** knop onder de naam van de IoT-hub.

## <a name="next-steps"></a>Volgende stappen

Volg deze koppelingen voor meer informatie over het beheren van Azure IoT Hub:

* [IoT-apparaten bulksgewijs beheren](iot-hub-bulk-identity-mgmt.md)
* [Metrische gegevens van IoT Hub](iot-hub-metrics.md)
* [Controle van bewerkingen](iot-hub-operations-monitoring.md)

Als u wilt de mogelijkheden van IoT Hub verder verkennen, Zie:

* [Ontwikkelaarshandleiding voor IoT Hub](iot-hub-devguide.md)
* [AI implementeren op edge-apparaten met Azure IoT Edge](../iot-edge/tutorial-simulate-device-linux.md)
* [Beveiligen van uw IoT-oplossing vanaf het begin van](../iot-fundamentals/iot-security-ground-up.md)
