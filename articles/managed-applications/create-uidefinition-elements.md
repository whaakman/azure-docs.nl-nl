---
title: Azure-beheerde toepassing UI definitie functies maken | Microsoft Docs
description: Beschrijft de functies voor gebruik bij het samenstellen van UI-definities voor beheerde Azure-toepassingen
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
ms.date: 10/12/2017
ms.author: tomfitz
ms.openlocfilehash: 23e407bf93bc51116ca45339bffcb801d69290f0
ms.sourcegitcommit: 3ab5ea589751d068d3e52db828742ce8ebed4761
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/27/2017
---
# <a name="createuidefinition-elements"></a>CreateUiDefinition elementen
Dit artikel wordt beschreven voor het schema en de eigenschappen voor alle ondersteunde elementen van een CreateUiDefinition. Gebruik van deze elementen wanneer [maken van een Azure-toepassing beheerd](publish-service-catalog-app.md). Het schema voor de meeste elementen is als volgt:

```json
{
  "name": "element1",
  "type": "Microsoft.Common.TextBox",
  "label": "Some text box",
  "defaultValue": "foobar",
  "toolTip": "Keep calm and visit the [Azure Portal](portal.azure.com).",
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
| Standaardwaarde | Nee | De standaardwaarde van het element. Sommige elementtypen ondersteuning voor complexe standaardwaarden, zodat de waarde kan een object zijn. |
| Knopinfo | Nee | De tekst die moet worden weergegeven in de knopinfo van het element. Net als bij `label`, bepaalde onderdelen van de ondersteuning van meerdere hulpprogramma tip tekenreeksen. Inline koppelingen kunnen worden ingesloten met Markdown-syntaxis.
| Beperkingen | Nee | Een of meer eigenschappen die worden gebruikt voor het validatiegedrag van het element aanpassen. De ondersteunde eigenschappen voor beperkingen verschillen per elementtype. Sommige elementtypen doen geen ondersteuning voor aanpassing van het validatiegedrag en hebben dus geen beperkingen-eigenschap. |
| Opties | Nee | Aanvullende eigenschappen die het gedrag van het element aanpassen. Net als bij `constraints`, de ondersteunde eigenschappen is afhankelijk van elementtype. |
| Zichtbaar | Nee | Hiermee wordt aangegeven of het element wordt weergegeven. Als `true`, het element en de betreffende onderliggende elementen worden weergegeven. De standaardwaarde is `true`. Gebruik [logische functies](create-uidefinition-functions.md#logical-functions) waarde van de eigenschap dynamisch kunt bepalen.

## <a name="elements"></a>Elementen

De documentatie voor elk element bevat een voorbeeld van een gebruikersinterface, schema, opmerkingen van het gedrag van het element (meestal met betrekking tot validatie en ondersteunde aanpassing) en een voorbeeld van uitvoer.

- [Microsoft.Common.DropDown](microsoft-common-dropdown.md)
- [Microsoft.Common.FileUpload](microsoft-common-fileupload.md)
- [Microsoft.Common.OptionsGroup](microsoft-common-optionsgroup.md)
- [Microsoft.Common.PasswordBox](microsoft-common-passwordbox.md)
- [Microsoft.Common.Section](microsoft-common-section.md)
- [Microsoft.Common.TextBox](microsoft-common-textbox.md)
- [Microsoft.Compute.CredentialsCombo](microsoft-compute-credentialscombo.md)
- [Microsoft.Compute.SizeSelector](microsoft-compute-sizeselector.md)
- [Microsoft.Compute.UserNameTextBox](microsoft-compute-usernametextbox.md)
- [Microsoft.Network.PublicIpAddressCombo](microsoft-network-publicipaddresscombo.md)
- [Microsoft.Network.VirtualNetworkCombo](microsoft-network-virtualnetworkcombo.md)
- [Microsoft.Storage.MultiStorageAccountCombo](microsoft-storage-multistorageaccountcombo.md)
- [Microsoft.Storage.StorageAccountSelector](microsoft-storage-storageaccountselector.md)

## <a name="next-steps"></a>Volgende stappen
* Zie voor een inleiding tot beheerde toepassingen, [overzicht van Azure Managed toepassing](overview.md).
* Zie voor een inleiding tot het maken van de definities van de gebruikersinterface, [aan de slag met CreateUiDefinition](create-uidefinition-overview.md).
