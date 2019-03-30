---
title: bestand opnemen
description: bestand opnemen
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: include
ms.date: 03/05/2019
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: 400f12237ae8b8cbaf6d66bda1663ecb680136f3
ms.sourcegitcommit: 956749f17569a55bcafba95aef9abcbb345eb929
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58630966"
---
[Berichtroutering](../articles/iot-hub/iot-hub-devguide-messages-d2c.md) kunnen verzenden van telemetriegegevens van uw IoT-apparaten aan ingebouwde Event Hub-compatibele eindpunten of aangepaste eindpunten, zoals blob storage, Service Bus-wachtrijen, Service Bus-onderwerpen en Event Hubs. Voor het configureren van aangepaste berichtroutering, maakt u [routering query's](../articles/iot-hub/iot-hub-devguide-routing-query-syntax.md) om aan te passen van de route die overeenkomt met een bepaalde voorwaarde. Zodra u deze hebt ingesteld, worden de inkomende gegevens automatisch door de IoT Hub doorgestuurd naar de eindpunten. Als een bericht niet overeenkomt met een van de gedefinieerde routering query's, wordt deze doorgestuurd naar het standaardeindpunt.

In deze zelfstudie 2-onderdeel leert u hoe u kunt instellen en gebruiken van deze aangepaste routering query's met IoT Hub. Routeren van berichten van een IoT-apparaat op een van meerdere eindpunten, met inbegrip van de blob-opslag en een Service Bus-wachtrij. Berichten naar de Service Bus-wachtrij worden opgehaald door een logische App en via e-mail verzonden. Berichten waarop geen aangepaste berichtroutering gedefinieerd zijn verzonden naar het standaardeindpunt worden opgehaald door Azure Stream Analytics en weergegeven in een Power BI-visualisatie.

 Volledige delen 1 en 2 van deze zelfstudie, moet u de volgende taken uitvoeren:

**Deel i Resources maken en instellen op Routering van berichten**
> [!div class="checklist"]
> * De resources--een IoT-hub, een storage-account, een Service Bus-wachtrij en een gesimuleerd apparaat maken. Dit kan worden gedaan met behulp van de portal, de Azure CLI, Azure PowerShell of een Azure Resource Manager-sjabloon.
> * De eindpunten en berichtroutes in IoT Hub configureren voor de storage-account en Service Bus-wachtrij.

**Deel II: Berichten verzenden naar de hub, gerouteerde resultaten weergeven**
> [!div class="checklist"]
> * Maak een logische app die wordt geactiveerd en een e-mailbericht verzendt wanneer een bericht wordt toegevoegd aan de Service Bus-wachtrij.
> * Download een app die een IoT-apparaat aanzet tot het verzenden van berichten naar de hub voor de verschillende routeringsopties en voer deze uit.
> * Maak een Power BI-visualisatie voor gegevens die naar het standaardeindpunt worden verzonden.
> * Bekijk de resultaten...
> * .. .in de Service Bus-wachtrij en e-mailberichten.
> * ...in het opslagaccount.
> * ...in de Power BI-visualisatie.

## <a name="prerequisites"></a>Vereisten

* Deel 1 van deze zelfstudie:
  - U hebt een abonnement op Azure nodig. Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

* Deel 2 van deze zelfstudie:
  - U moet voltooide deel 1 van deze zelfstudie hebt en de resources zijn nog steeds beschikbaar.
  - Installeer [Visual Studio](https://www.visualstudio.com/).
  - Een Power BI-account voor het analyseren van de Stream Analytics van het standaardeindpunt. ([Probeer Power BI gratis uit](https://app.powerbi.com/signupredirect?pbi_source=web).)
  - Een Office 365-account voor het verzenden van melding via e-mail.

[!INCLUDE [cloud-shell-try-it.md](cloud-shell-try-it.md)]
