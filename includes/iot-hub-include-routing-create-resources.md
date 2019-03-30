---
title: bestand opnemen
description: bestand opnemen
author: robinsh
ms.service: iot-hub
services: iot-hub
ms.topic: include
ms.date: 03/25/2019
ms.author: robinsh
ms.custom: include file
ms.openlocfilehash: 710bb8cba7fbbe4bc9b9fdc52b0767c96f97fe72
ms.sourcegitcommit: 956749f17569a55bcafba95aef9abcbb345eb929
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58631088"
---
## <a name="create-base-resources"></a>Basis-resources maken

Voordat u om de routering van berichten configureren kunt, moet u een IoT-hub, een storage-account en een Service Bus-wachtrij maken. Deze resources kunnen worden gemaakt met een van de vier artikelen die beschikbaar zijn voor deze zelfstudie, deel 1: de Azure CLI, Azure PowerShell, Azure portal of een Azure Resource Manager-sjabloon.

Gebruik dezelfde resourcegroep en -locatie voor alle resources. Klik aan het einde, kunt u alle resources in één stap door de resourcegroep te verwijderen.

De volgende secties beschrijven de stappen om te worden uitgevoerd.

1. Maak een [resourcegroep](../articles/azure-resource-manager/resource-group-overview.md).

2. Maak een IoT Hub in de S1-laag. Voeg een consumentengroep toe aan uw IoT Hub. De consumentengroep wordt gebruikt door de Azure Stream Analytics bij het ophalen van gegevens.

   > [!NOTE]
   > U moet een IoT Hub in een betaalde laag gebruiken om deze zelfstudie te voltooien. Met de gratis laag kunt u slechts één eindpunt instellen, terwijl voor deze zelfstudie meerdere eindpunten zijn vereist.
   > 

3. Maak een standaard V1-opslagaccount met Standard_LRS replicatie.

4. Maak een Service Bus-naamruimte en -wachtrij.

5. Maak een apparaat-id voor het gesimuleerde apparaat dat berichten naar uw hub verzendt. Sla de sleutel op voor de testfase. (Als u een Resource Manager-sjabloon maakt, wordt dit gedaan na de implementatie van de sjabloon.)