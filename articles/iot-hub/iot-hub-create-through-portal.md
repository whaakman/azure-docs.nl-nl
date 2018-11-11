---
title: De Azure portal gebruiken voor het maken van een IoT-Hub | Microsoft Docs
description: Het maken, beheren en verwijderen van Azure IoT hubs via de Azure-portal. Bevat informatie over Prijscategorieën, schaling, beveiliging en configuratie van berichten.
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 09/06/2018
ms.author: robinsh
ms.openlocfilehash: 21758d112cd7eaf45b8b279cf2a6f6060c44c9c1
ms.sourcegitcommit: 5a1d601f01444be7d9f405df18c57be0316a1c79
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/10/2018
ms.locfileid: "51515842"
---
# <a name="create-an-iot-hub-using-the-azure-portal"></a>Een IoT-hub met behulp van de Azure portal maken

[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

In dit artikel wordt beschreven hoe u maken en beheren van IoT-hubs met behulp van de [Azure-portal](https://portal.azure.com).

Voor het gebruik van de stappen in deze zelfstudie, moet u een Azure-abonnement. Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="create-an-iot-hub"></a>Een IoT Hub maken

[!INCLUDE [iot-hub-include-create-hub](../../includes/iot-hub-include-create-hub.md)]

## <a name="change-the-settings-of-the-iot-hub"></a>Wijzig de instellingen van de IoT-hub

U kunt de instellingen van een bestaande IoT-hub wijzigen nadat deze gemaakt in het deelvenster IoT-Hub.

![Schermopname van de instellingen voor de IoT-hub](./media/iot-hub-create-through-portal/iot-hub-settings-panel.png)

Hier volgen enkele van de eigenschappen die u voor een IoT-hub instellen kunt:

**Prijs- en schaalniveau**: U kunt deze eigenschap gebruiken om te migreren naar een andere laag of het aantal IoT Hub-eenheden ingesteld. 

**Bewerkingen controleren**: Schakel de verschillende categorieën van de bewaking in of uit, zoals logboekregistratie voor gebeurtenissen met betrekking tot apparaat-naar-cloud-berichten of cloud-naar-apparaat-berichten.

**IP-Filter**: Geef een bereik van IP-adressen die worden geaccepteerd of geweigerd door de IoT-hub.

**Eigenschappen**: bevat de lijst met eigenschappen die u kunt kopiëren en ergens anders, zoals de resource-ID, resourcegroep, locatie, enzovoort te gebruiken.

### <a name="shared-access-policies"></a>Gedeeld toegangsbeleid

U kunt ook weergeven of wijzigen van de lijst met beleid voor gedeelde toegang door te klikken op **beleid voor gedeelde toegang** in de **instellingen** sectie. Deze beleidsregels definiëren de machtigingen voor apparaten en services om te verbinden met IoT Hub. 

Klik op **toevoegen** openen de **toevoegen van een beleid voor gedeelde toegang** blade.  U kunt de naam van het nieuwe beleid en de machtigingen die u koppelen aan dit beleid, wilt zoals wordt weergegeven in de volgende afbeelding:

![Schermafbeelding van een beleid voor gedeelde toegang toevoegen](./media/iot-hub-create-through-portal/iot-hub-add-shared-access-policy.png)

* De **register lezen** en **register schrijven** beleid lezen en schrijven rechten toekennen voor het id-register. Automatisch kiezen van de optie voor het schrijven, kiest de optie voor lezen.

* De **Service verbinding maken met** beleid een machtiging verleend voor toegang tot service-eindpunten, zoals **ontvangen van apparaat-naar-cloud**. 

* De **apparaat verbinding maken met** beleid verleent machtigingen voor het verzenden en ontvangen van berichten met behulp van de IoT Hub apparaat-side-eindpunten.

Klik op **maken** om toe te voegen zojuist gemaakte beleid aan de bestaande lijst.

## <a name="message-routing-for-an-iot-hub"></a>Bericht routering voor een IoT-hub

Klik op **berichtroutering** onder **Messaging** om te zien van de routering van berichten in het deelvenster waar u routes en aangepaste eindpunten voor de hub definieert. [Berichtroutering](iot-hub-devguide-messages-d2c.md) kunt u beheren hoe gegevens van uw apparaten worden verzonden naar uw eindpunten. De eerste stap is het toevoegen van een nieuwe route. U kunt een bestaand eindpunt toevoegen aan de route, of een nieuwe maken van de typen worden ondersteund, zoals blob-opslag. 

![Routering berichtvenster](./media/iot-hub-create-through-portal/iot-hub-message-routing.png)

### <a name="routes"></a>Routes

Routes is het eerste tabblad in het deelvenster routering van berichten. Als u wilt een nieuwe route toevoegen, klikt u op +**toevoegen**. U ziet het volgende scherm. 

![Schermafbeelding van een nieuwe route toevoegen](./media/iot-hub-create-through-portal/iot-hub-add-route-storage-endpoint.png)

Naam van uw hub. De naam moet uniek zijn binnen de lijst met routes voor deze hub. 

Voor **eindpunt**, u kunt een in de vervolgkeuzelijst selecteren of toevoegen van een nieuwe account. In dit voorbeeld wordt zijn een opslagaccount en container al beschikbaar. Als u wilt toevoegen als een eindpunt, klikt u op +**toevoegen** naast de eindpunt-vervolgkeuzelijst en selecteer **Blob-opslag**. Het volgende scherm ziet u waar het opslagaccount en container zijn opgegeven.

![Schermopname van het toevoegen van een storage-eindpunt voor de regel voor doorsturen](./media/iot-hub-create-through-portal/iot-hub-routing-add-storage-endpoint.png)

Klik op **een container kiezen** om de storage-account en container te selecteren. Wanneer u deze velden hebt geselecteerd, wordt het deelvenster eindpunt. Gebruik de standaardinstellingen voor de rest van de velden en **maken** naar het eindpunt voor de storage-account maken en toe te voegen aan de regels voor doorsturen.

Voor **gegevensbron**, selecteert u Telemetrieberichten van apparaten. 

Voeg vervolgens een query routering. In dit voorbeeld wordt de berichten waarvoor de toepassingseigenschap van een met de naam `level` met een waarde die gelijk is aan `critical` worden doorgestuurd naar het opslagaccount.

![Schermopname van het opslaan van een nieuwe regel voor doorsturen](./media/iot-hub-create-through-portal/iot-hub-add-route.png)

Klik op **opslaan** om op te slaan van de regel voor doorsturen. U terugkeert naar het deelvenster berichtroutering en de nieuwe regel voor doorsturen wordt weergegeven.

### <a name="custom-endpoints"></a>Aangepaste eindpunten

Klik op de **aangepaste eindpunten** tabblad. Ziet u geen aangepaste eindpunten al gemaakt. Hier kunt u nieuwe eindpunten toevoegen of verwijderen van bestaande eindpunten. 

> [!NOTE]
> Als u een route verwijdert, wordt de eindpunten die zijn toegewezen aan deze route niet verwijderd. Als u wilt verwijderen van een eindpunt, klikt u op het tabblad met aangepaste eindpunten, selecteert u het eindpunt dat u wilt verwijderen en klik op verwijderen.
>

Meer informatie over aangepaste eindpunten in [referentie - eindpunten van IoT-hub](iot-hub-devguide-endpoints.md).

U kunt maximaal 10 aangepaste eindpunten voor een IoT-hub definiëren. 

Zie voor een compleet voorbeeld van hoe u aangepaste eindpunten met routering [berichtroutering met IoT Hub](tutorial-routing.md).

## <a name="find-a-specific-iot-hub"></a>Een specifieke IoT-hub vinden

Hier volgen twee manieren om een specifieke IoT-hub niet vinden in uw abonnement:

1. Als u weet dat de resource groep waartoe de IoT-hub behoort, klikt u op **resourcegroepen**, selecteer vervolgens de resourcegroep in de lijst. De resource-group-scherm ziet u alle resources in die groep, met inbegrip van de IoT-hubs. Klik op de hub die u zoekt.

2. Klik op **alle resources**. Op de **alle resources** deelvenster, er wordt een vervolgkeuzelijst die standaard `All types`. Klik op de vervolgkeuzelijst, schakel het selectievakje `Select all`. Zoek `IoT Hub` en controleren. Klik op de vervolgkeuzelijst om deze te sluiten en de vermeldingen worden gefilterd, met slechts uw IoT-hubs.

## <a name="delete-the-iot-hub"></a>De IoT-hub verwijderen

Als u wilt verwijderen van een Iot-hub, vinden de IoT-hub die u wilt verwijderen en klik vervolgens op de **verwijderen** knop onder de naam van de IoT-hub.

## <a name="next-steps"></a>Volgende stappen

Volg deze koppelingen voor meer informatie over het beheren van Azure IoT Hub:

* [Met IoT Hub-berichtroutering](tutorial-routing.md)
* [Metrische gegevens van IoT Hub](iot-hub-metrics.md)
* [Controle van bewerkingen](iot-hub-operations-monitoring.md)