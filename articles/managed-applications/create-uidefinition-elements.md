---
title: Azure maakt het rapportdefinitie-element UI | Microsoft Docs
description: Beschrijving van de elementen moeten worden gebruikt bij het samenstellen van UI-definities voor de Azure-portal.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/30/2018
ms.author: tomfitz
ms.openlocfilehash: cbfc9c02cbde8bcf9a253144ff41497676c98f13
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="createuidefinition-elements"></a>CreateUiDefinition elementen
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
| naam | Ja | Een interne id om te verwijzen naar een specifiek exemplaar van een element. Het meest voorkomende gebruik van de naam van het element is in `outputs`, waarbij de uitvoerwaarden van de opgegeven elementen zijn toegewezen aan de parameters van de sjabloon. U kunt het ook gebruiken binden van de uitvoerwaarde van een element op de `defaultValue` van een ander element. |
| type | Ja | De UI-besturingselement om weer te geven voor het element. Zie voor een lijst van ondersteunde typen [elementen](#elements). |
| Label | Ja | De weergavetekst van het element. Sommige elementtypen bevatten meerdere labels, zodat de waarde kan een object met meerdere tekenreeksen zijn. |
| defaultValue | Nee | De standaardwaarde van het element. Sommige elementtypen ondersteuning voor complexe standaardwaarden, zodat de waarde kan een object zijn. |
| Knopinfo | Nee | De tekst die moet worden weergegeven in de knopinfo van het element. Net als bij `label`, bepaalde onderdelen van de ondersteuning van meerdere hulpprogramma tip tekenreeksen. Inline koppelingen kunnen worden ingesloten met Markdown-syntaxis.
| Beperkingen | Nee | Een of meer eigenschappen die worden gebruikt voor het validatiegedrag van het element aanpassen. De ondersteunde eigenschappen voor beperkingen verschillen per elementtype. Sommige elementtypen doen geen ondersteuning voor aanpassing van het validatiegedrag en hebben dus geen beperkingen-eigenschap. |
| opties | Nee | Aanvullende eigenschappen die het gedrag van het element aanpassen. Net als bij `constraints`, de ondersteunde eigenschappen is afhankelijk van elementtype. |
| Zichtbaar | Nee | Hiermee wordt aangegeven of het element wordt weergegeven. Als `true`, het element en de betreffende onderliggende elementen worden weergegeven. De standaardwaarde is `true`. Gebruik [logische functies](create-uidefinition-functions.md#logical-functions) waarde van de eigenschap dynamisch kunt bepalen.

## <a name="elements"></a>Elementen

De documentatie voor elk element bevat een voorbeeld van een gebruikersinterface, schema, opmerkingen van het gedrag van het element (meestal met betrekking tot validatie en ondersteunde aanpassing) en een voorbeeld van uitvoer.

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
- [Microsoft.Network.AvailabilityZoneDropDown](microsoft-network-availabilityzonedropdown.md)
- [Microsoft.Network.PublicIpAddressCombo](microsoft-network-publicipaddresscombo.md)
- [Microsoft.Network.VirtualNetworkCombo](microsoft-network-virtualnetworkcombo.md)
- [Microsoft.Storage.MultiStorageAccountCombo](microsoft-storage-multistorageaccountcombo.md)
- [Microsoft.Storage.StorageAccountSelector](microsoft-storage-storageaccountselector.md)

## <a name="next-steps"></a>Volgende stappen
Zie voor een inleiding tot het maken van de definities van de gebruikersinterface, [aan de slag met CreateUiDefinition](create-uidefinition-overview.md).
