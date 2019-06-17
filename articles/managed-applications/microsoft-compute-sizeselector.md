---
title: Azure SizeSelector UI-element | Microsoft Docs
description: Beschrijft de Microsoft.Compute.SizeSelector UI-element voor Azure-portal.
services: managed-applications
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: managed-applications
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/27/2018
ms.author: tomfitz
ms.openlocfilehash: e5be5635964ebeedc7be4d1d1f5403e4d281b55c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64722346"
---
# <a name="microsoftcomputesizeselector-ui-element"></a>Microsoft.Compute.SizeSelector UI-element
Een besturingselement voor het selecteren van een grootte voor een of meer exemplaren van de virtuele machine.

## <a name="ui-sample"></a>Voorbeeld van de gebruikersinterface

De gebruiker ziet een selector met de standaardwaarden van de definitie van het element.

![Microsoft.Compute.SizeSelector](./media/managed-application-elements/microsoft.compute.sizeselector.png)

Na het selecteren van het besturingselement, ziet de gebruiker een uitgebreid overzicht van de beschikbare grootten.

![Microsoft.Compute.SizeSelector uitgevouwen](./media/managed-application-elements/microsoft.compute.sizeselector-expanded.png)

## <a name="schema"></a>Schema
```json
{
  "name": "element1",
  "type": "Microsoft.Compute.SizeSelector",
  "label": "Size",
  "toolTip": "",
  "recommendedSizes": [
    "Standard_D1",
    "Standard_D2",
    "Standard_D3"
  ],
  "constraints": {
    "allowedSizes": [],
    "excludedSizes": [],
    "numAvailabilityZonesRequired": 3,
    "zone": "3"
  },
  "options": {
    "hideDiskTypeFilter": false
  },
  "osPlatform": "Windows",
  "imageReference": {
    "publisher": "MicrosoftWindowsServer",
    "offer": "WindowsServer",
    "sku": "2012-R2-Datacenter"
  },
  "count": 2,
  "visible": true
}
```

## <a name="remarks"></a>Opmerkingen
- `recommendedSizes` moet ten minste één grootte hebben. De aanbevolen grootte van de eerste wordt als standaardwaarde gebruikt. De lijst met beschikbare grootten niet is gesorteerd op de status van de aanbevolen. De gebruiker kan deze kolom te sorteren op aanbevolen status selecteren.
- Als een aanbevolen grootte is niet beschikbaar in de geselecteerde locatie, wordt de grootte automatisch overgeslagen. In plaats daarvan wordt de volgende aanbevolen grootte gebruikt.
- `constraints.allowedSizes` en `constraints.excludedSizes` zijn beide optioneel, maar kunnen niet gelijktijdig worden gebruikt. Kan de lijst met beschikbare grootten worden bepaald door het aanroepen van [lijst van beschikbare VM-grootten voor een abonnement](/rest/api/compute/virtualmachines/virtualmachines-list-sizes-region). Elke grootte niet is opgegeven in de `constraints.allowedSizes` verborgen is, en elke grootte niet is opgegeven in `constraints.excludedSizes` wordt weergegeven.
- `osPlatform` Er moet worden opgegeven en kan **Windows** of **Linux**. Wordt gebruikt om te bepalen van de kosten voor hardware van de virtuele machines.
- `imageReference` voor installatiekopieën met eigen, maar opgegeven voor externe afbeeldingen wordt weggelaten. Wordt gebruikt om te bepalen van de softwarekosten voor van de virtuele machines.
- `count` wordt gebruikt voor het instellen van de juiste vermenigvuldiger voor het element. Azureondersteunt een statische waarde, zoals **2**, of een dynamische waarde van een ander element, zoals `[steps('step1').vmCount]`. De standaardwaarde is **1**.
- De `numAvailabilityZonesRequired` kan 1, 2 of 3 zijn.
- Standaard `hideDiskTypeFilter` is **false**. Het type schijf filter kan de gebruiker om te zien van alle schijftypen of alleen SSD.

## <a name="sample-output"></a>Voorbeelduitvoer
```json
"Standard_D1"
```

## <a name="next-steps"></a>Volgende stappen
* Zie voor een inleiding tot het maken van definities van de gebruikersinterface, [aan de slag met CreateUiDefinition](create-uidefinition-overview.md).
* Zie voor een beschrijving van de algemene eigenschappen in de UI-elementen, [CreateUiDefinition elementen](create-uidefinition-elements.md).
