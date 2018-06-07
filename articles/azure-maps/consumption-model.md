---
title: Model van Azure Maps verbruik | Microsoft Docs
description: Meer informatie over het verbruik model van Azure Maps
author: subbarayudukamma
ms.author: skamma
ms.date: 05/08/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.openlocfilehash: cf20c7dbfbf7cd3f09579b03b835148c1c295137
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34600626"
---
# <a name="consumption-model"></a>Verbruiksmodel

Online Routering biedt een set parameters voor een gedetailleerde beschrijving van vehicle-specifieke verbruik Model.
Afhankelijk van de waarde van **vehicleEngineType**, twee principal verbruik modellen worden ondersteund: _verbranding_ en _elektrische_. Het opgeven van de parameters die deel uitmaken van verschillende modellen in dezelfde aanvraag is een fout.
Verbruik Model kan niet worden gebruikt met **travelMode** waarden _fiets_ en _personen_.

## <a name="parameter-constraints-for-consumption-model"></a>Beperkingen van de typeparameters voor verbruik model

In beide modellen verbruik sommige parameters expliciet op te geven wilt, moet u andere ook. Deze afhankelijkheden zijn:

* Alle parameters vereisen **constantSpeedConsumption** wordt opgegeven door de gebruiker. Er treedt een fout aan geen andere verbruik model parameter opgeven, met uitzondering van **vehicleWeight**als **constantSpeedConsumption*** is niet opgegeven.
* **accelerationEfficiency** en **decelerationEfficiency** moeten altijd worden opgegeven als een paar (dat wil zeggen beide of geen).
* Als **accelerationEfficiency** en **decelerationEfficiency** zijn opgegeven product van hun waarden mag niet groter zijn dan 1 (om te voorkomen Eeuwige beweging) zijn.
* **uphillEfficiency** en **downhillEfficiency** moeten altijd worden opgegeven als een paar (dat wil zeggen beide of geen).
* Als **uphillEfficiency** en **downhillEfficiency** zijn opgegeven product van hun waarden mag niet groter zijn dan 1 (om te voorkomen Eeuwige beweging) zijn.
* Als \* **efficiëntie** parameters zijn opgegeven door de gebruiker vervolgens **vehicleWeight** moet ook worden opgegeven. Wanneer **vehicleEngineType** is _verbranding_, **fuelEnergyDensityInMJoulesPerLiter** moet ook worden opgegeven.
* **maxChargeInkWh** en **currentChargeInkWh** moeten altijd worden opgegeven als een paar (dat wil zeggen beide of geen).

> [!NOTE]
> Als er slechts **constantSpeedConsumption** is opgegeven, wordt er geen andere verbruik aspecten zoals hellingen en vehicle versnelling voor verbruik berekeningen rekening wordt gehouden.

## <a name="combustion-consumption-model"></a>Verbranding verbruik model

Het Model van de consumptie verbranding wordt gebruikt wanneer **vehicleEngineType** is ingesteld op _verbranding_.
De lijst met parameters die deel uitmaken van dit model zijn hieronder. Raadpleeg het gedeelte Parameters voor een gedetailleerde beschrijving.

* constantSpeedConsumptionInLitersPerHundredkm
* VehicleWeight
* currentFuelInLiters
* auxiliaryPowerInLitersPerHour
* fuelEnergyDensityInMJoulesPerLiter
* accelerationEfficiency
* decelerationEfficiency
* uphillEfficiency
* downhillEfficiency

## <a name="electric-consumption-model"></a>Elektrische verbruik model

Het elektrische verbruik Model wordt gebruikt wanneer **vehicleEngineType** is ingesteld op _elektrische_.
De lijst met parameters die deel uitmaken van dit model zijn hieronder. Raadpleeg het gedeelte Parameters voor een gedetailleerde beschrijving.

* constantSpeedConsumptionInkWhPerHundredkm
* VehicleWeight
* currentChargeInkWh
* maxChargeInkWh
* auxiliaryPowerInkW
* accelerationEfficiency
* decelerationEfficiency
* uphillEfficiency
* downhillEfficiency

## <a name="sensible-values-of-consumption-parameters"></a>Functionele waarden van verbruik parameters

Parameters voor een bepaalde set kan worden afgewezen, hoewel deze mogelijk te voldoen aan de expliciete vereisten die hierboven is opgegeven. Dit gebeurt wanneer de waarde van een specifieke parameter of een combinatie van waarden van verschillende parameters, kan leiden tot onredelijk magnitude van verbruik waarden wordt geacht. Als dat gebeurt, geeft deze waarschijnlijk een invoerfout zoals juiste nauwkeurig worden voor alle logisch waarden van verbruik parameters. Als een bepaalde set parameters voor verbruik is afgewezen, wordt het bijbehorende foutbericht een tekstuele uitleg van de reden(en) bevatten.
De gedetailleerde beschrijvingen van de parameters bevatten voorbeelden van praktische waarden voor beide modellen.
