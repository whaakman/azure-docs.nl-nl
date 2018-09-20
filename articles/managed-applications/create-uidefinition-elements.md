---
title: Azure maakt het rapportdefinitie-element UI | Microsoft Docs
description: Beschrijving van de elementen te gebruiken bij het maken van definities van de gebruikersinterface voor Azure-portal.
services: managed-applications
documentationcenter: na
author: tfitzmac
ms.service: managed-applications
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/19/2018
ms.author: tomfitz
ms.openlocfilehash: 895b5cfb121347f9400d1aa315fef05d9088c55c
ms.sourcegitcommit: ce526d13cd826b6f3e2d80558ea2e289d034d48f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/19/2018
ms.locfileid: "46366468"
---
# <a name="createuidefinition-elements"></a>CreateUiDefinition-elementen
Dit artikel wordt beschreven voor het schema en de eigenschappen voor alle ondersteunde elementen van een CreateUiDefinition. 

## <a name="schema"></a>Schema

Het schema voor de meeste elementen is als volgt:

```json
{
  "name": "element1",
  "type": "Microsoft.Common.TextBox",
  "label": "Some text box",
  "defaultValue": "my value",
  "toolTip": "Provide a descriptive name.",
  "constraints": {},
  "options": {},
  "visible": true
}
```

| Eigenschap | Vereist | Beschrijving |
| -------- | -------- | ----------- |
| naam | Ja | Een interne id om te verwijzen naar een specifiek exemplaar van een element. De meest voorkomende gebruik van de naam van het element is in `outputs`, waar de uitvoerwaarden van de opgegeven elementen zijn toegewezen aan de parameters van de sjabloon. U kunt deze ook gebruiken om te koppelen van de uitvoerwaarde van een element op de `defaultValue` van een ander element. |
| type | Ja | De UI-besturingselement om weer te geven voor het element. Zie voor een lijst van ondersteunde typen [elementen](#elements). |
| label | Ja | De weergegeven tekst van het element. Sommige elementtypen bevatten meerdere labels, zodat de waarde kan een object met meerdere tekenreeksen zijn. |
| defaultValue | Nee | De standaardwaarde van het element. Sommige elementtypen ondersteuning voor complexe standaardwaarden, zodat de waarde kan een object zijn. |
| Knopinfo | Nee | De tekst die moet worden weergegeven in de knopinfo van het element. Vergelijkbaar met `label`, bepaalde onderdelen van de ondersteuning van meerdere hulpprogramma tip tekenreeksen. Inlinelinks kan worden ingesloten Markdown-syntaxis gebruiken.
| Beperkingen | Nee | Een of meer eigenschappen die worden gebruikt voor het aanpassen van de werking van de validatie van het element. De ondersteunde eigenschappen voor beperkingen variëren per elementtype. Sommige elementtypen geen ondersteuning voor aanpassing van het validatiegedrag, en dus er is geen eigenschap beperkingen hebben. |
| opties | Nee | Extra eigenschappen die het gedrag van het element aanpassen. Vergelijkbaar met `constraints`, de ondersteunde eigenschappen variëren per elementtype. |
| Zichtbaar | Nee | Geeft aan of het element wordt weergegeven. Als `true`, het element en de betreffende onderliggende elementen worden weergegeven. De standaardwaarde is `true`. Gebruik [logische functies](create-uidefinition-functions.md#logical-functions) dynamisch beheren de waarde van deze eigenschap.

## <a name="elements"></a>Elementen

De documentatie voor elk element bevat een voorbeeld van een gebruikersinterface, schema en opmerkingen van het gedrag van het element (meestal met betrekking tot validatie en ondersteunde aanpassing) en voorbeeld van uitvoer.

- [Microsoft.Common.DropDown](microsoft-common-dropdown.md)
- [Microsoft.Common.FileUpload](microsoft-common-fileupload.md)
- [Microsoft.Common.InfoBox](microsoft-common-infobox.md)
- [Microsoft.Common.OptionsGroup](microsoft-common-optionsgroup.md)
- [Microsoft.Common.PasswordBox](microsoft-common-passwordbox.md)
- [Microsoft.Common.Section](microsoft-common-section.md)
- [Microsoft.Common.TextBlock](microsoft-common-textblock.md)
- [Microsoft.Common.TextBox](microsoft-common-textbox.md)
- [Microsoft.Compute.CredentialsCombo](microsoft-compute-credentialscombo.md)
- [Microsoft.Compute.SizeSelector](microsoft-compute-sizeselector.md)
- [Microsoft.Compute.UserNameTextBox](microsoft-compute-usernametextbox.md)
- [Microsoft.Network.PublicIpAddressCombo](microsoft-network-publicipaddresscombo.md)
- [Microsoft.Network.VirtualNetworkCombo](microsoft-network-virtualnetworkcombo.md)
- [Microsoft.Storage.MultiStorageAccountCombo](microsoft-storage-multistorageaccountcombo.md)
- [Microsoft.Storage.StorageAccountSelector](microsoft-storage-storageaccountselector.md)

## <a name="next-steps"></a>Volgende stappen
Zie voor een inleiding tot het maken van definities van de gebruikersinterface, [aan de slag met CreateUiDefinition](create-uidefinition-overview.md).
