---
title: Gebruik-Model van Azure Maps | Microsoft Docs
description: Meer informatie over verbruik model van Azure Maps
author: subbarayudukamma
ms.author: skamma
ms.date: 05/08/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.openlocfilehash: 5f75f656312c11a4668ca9ef9fe7b2a61a7d13e8
ms.sourcegitcommit: f863ed1ba25ef3ec32bd188c28153044124cacbc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/15/2019
ms.locfileid: "56301136"
---
# <a name="consumption-model"></a>Verbruiksmodel

Online Routing biedt een set parameters voor een gedetailleerde beschrijving van het voertuig-specifieke verbruik Model.
Afhankelijk van de waarde van **vehicleEngineType**, twee modellen met principal verbruik worden ondersteund: _Verbranding_ en _Electric_. Parameters die deel uitmaken van verschillende modellen in dezelfde aanvraag op te geven, is een fout.
Verbruik Model kan niet worden gebruikt met **travelMode** waarden _bicycle_ en _pedestrian_.

## <a name="parameter-constraints-for-consumption-model"></a>Parameter-beperkingen voor verbruik model

In beide modellen verbruik sommige parameters expliciet op te geven is vereist op andere ook te geven. Deze afhankelijkheden zijn:

* Alle parameters vereist **constantSpeedConsumption** worden opgegeven door de gebruiker. Het is een fout die naar een andere verbruik model parameter opgeeft, met uitzondering van **vehicleWeight**als **constantSpeedConsumption** is niet opgegeven.
* **accelerationEfficiency** en **decelerationEfficiency** moeten altijd worden opgegeven als een paar (dat wil zeggen beide of geen).
* Als **accelerationEfficiency** en **decelerationEfficiency** zijn opgegeven, product van de waarden van mag niet groter zijn dan 1 (om te voorkomen dat Eeuwige beweging) zijn.
* **uphillEfficiency** en **downhillEfficiency** moeten altijd worden opgegeven als een paar (dat wil zeggen beide of geen).
* Als **uphillEfficiency** en **downhillEfficiency** zijn opgegeven, product van de waarden van mag niet groter zijn dan 1 (om te voorkomen dat Eeuwige beweging) zijn.
* Als de \* __efficiëntie__ parameters zijn opgegeven door de gebruiker vervolgens **vehicleWeight** moeten ook worden opgegeven. Wanneer **vehicleEngineType** is _verbranding_, **fuelEnergyDensityInMJoulesPerLiter** moet ook worden opgegeven.
* **maxChargeInkWh** en **currentChargeInkWh** moeten altijd worden opgegeven als een paar (dat wil zeggen beide of geen).

> [!NOTE]
> Als er slechts **constantSpeedConsumption** is opgegeven, wordt er geen andere verbruik aspecten zoals hellingen en versnelling voor een voertuig in aanmerking voor verbruik-berekeningen worden genomen.

## <a name="combustion-consumption-model"></a>Verbranding verbruik model

Het Model van de consumptie verbranding wordt gebruikt wanneer **vehicleEngineType** is ingesteld op _combustion_.
De lijst met parameters die deel uitmaken van dit model staan hieronder. Raadpleeg de sectie Parameters voor een gedetailleerde beschrijving.

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

Het elektrische verbruik Model wordt gebruikt wanneer **vehicleEngineType** is ingesteld op _electric_.
De lijst met parameters die deel uitmaken van dit model staan hieronder. Raadpleeg de sectie Parameters voor een gedetailleerde beschrijving.

* constantSpeedConsumptionInkWhPerHundredkm
* VehicleWeight
* currentChargeInkWh
* maxChargeInkWh
* auxiliaryPowerInkW
* accelerationEfficiency
* decelerationEfficiency
* uphillEfficiency
* downhillEfficiency

## <a name="sensible-values-of-consumption-parameters"></a>Functionele waarden van parameters voor verbruik

Een bepaalde set verbruik parameters kan worden afgewezen, hoewel het mogelijk te voldoen aan de expliciete vereisten die hierboven is opgegeven. Dit gebeurt wanneer de waarde van een specifieke parameter of een combinatie van waarden van verschillende parameters, wordt geacht leiden tot een onredelijke magnitude van consumptie waarden. Als dit gebeurt, wordt waarschijnlijk aangegeven die een invoerfoutpakketten juiste zorg voor alle zijn waarden van parameters voor gebruik is genomen. In het geval een bepaalde set parameters voor verbruik is geweigerd, wordt het bijbehorende foutbericht een tekstuele uitleg van de reden(en) bevatten.
De gedetailleerde beschrijvingen van de parameters bevatten voorbeelden van functionele waarden voor beide modellen.
