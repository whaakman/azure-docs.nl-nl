---
title: Azure SizeSelector UI-element | Microsoft Docs
description: Beschrijft het Microsoft.Compute.SizeSelector UI-element voor Azure-portal.
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
ms.openlocfilehash: 9009d29e281ace179ad1dd2021c7cf35e3dc611a
ms.sourcegitcommit: d1eefa436e434a541e02d938d9cb9fcef4e62604
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2018
ms.locfileid: "37084802"
---
# <a name="microsoftcomputesizeselector-ui-element"></a>Microsoft.Compute.SizeSelector UI-element
Een besturingselement voor het selecteren van een grootte voor een of meer exemplaren van de virtuele machine.

## <a name="ui-sample"></a>Voorbeeld van de gebruikersinterface

De gebruiker ziet een selector met de standaardwaarden van de elementdefinitie.

![Microsoft.Compute.SizeSelector](./media/managed-application-elements/microsoft.compute.sizeselector.png)

Na het selecteren van het besturingselement, ziet de gebruiker een uitgebreide weergave van de beschikbare grootten.

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
- `recommendedSizes` moet ten minste één grootte hebben. De eerste aanbevolen grootte wordt als standaardwaarde gebruikt. De lijst met beschikbare grootten niet is gesorteerd op de status van de aanbevolen. De gebruiker kunt selecteren die kolom te sorteren op aanbevolen status.
- Als een aanbevolen grootte is niet beschikbaar in de geselecteerde locatie, wordt de grootte automatisch overgeslagen. In plaats daarvan wordt de volgende aanbevolen grootte gebruikt.
- `constraints.allowedSizes` en `constraints.excludedSizes` zijn beide optioneel, maar niet gelijktijdig worden gebruikt. Kan de lijst met beschikbare grootten worden bepaald door het aanroepen van [lijst met beschikbare virtuele machine grootten voor een abonnement](/rest/api/compute/virtualmachines/virtualmachines-list-sizes-region). Elke grootte niet is opgegeven in de `constraints.allowedSizes` is verborgen, en elke grootte niet is opgegeven in `constraints.excludedSizes` wordt weergegeven.
- `osPlatform` moet worden opgegeven en kan **Windows** of **Linux**. Wordt gebruikt om de hardwarekosten van de virtuele machines te bepalen.
- `imageReference` wordt weggelaten voor directe installatiekopieën, maar de opgegeven voor de installatiekopieën van derden. Wordt gebruikt om de softwarekosten van de virtuele machines te bepalen.
- `count` wordt gebruikt om de juiste vermenigvuldiger voor het element. Een statische waarde, zoals ondersteunt **2**, of een dynamische waarde van een ander element, zoals `[steps('step1').vmCount]`. De standaardwaarde is **1**.
- De `numAvailabilityZonesRequired` 1, 2 of 3.
- Standaard `hideDiskTypeFilter` is **false**. Het type schijf filter kan de gebruiker om te zien alle schijftypen of alleen SSD.

## <a name="sample-output"></a>Voorbeelduitvoer
```json
"Standard_D1"
```

## <a name="next-steps"></a>Volgende stappen
* Zie voor een inleiding tot het maken van de definities van de gebruikersinterface, [aan de slag met CreateUiDefinition](create-uidefinition-overview.md).
* Zie voor een beschrijving van de algemene eigenschappen in de UI-elementen, [CreateUiDefinition elementen](create-uidefinition-elements.md).
