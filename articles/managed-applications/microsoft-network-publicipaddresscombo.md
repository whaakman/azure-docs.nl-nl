---
title: Azure PublicIpAddressCombo UI-element | Microsoft Docs
description: Beschrijft het Microsoft.Network.PublicIpAddressCombo UI-element voor Azure-portal.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/30/2018
ms.author: tomfitz
ms.openlocfilehash: bced85abaf24beb0997ec906a68e5947f0e67cca
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2018
---
# <a name="microsoftnetworkpublicipaddresscombo-ui-element"></a>Microsoft.Network.PublicIpAddressCombo UI-element
Een groep besturingselementen voor het selecteren van een nieuw of bestaand openbaar IP-adres.

## <a name="ui-sample"></a>Voorbeeld van de gebruikersinterface
![Microsoft.Network.PublicIpAddressCombo](./media/managed-application-elements/microsoft.network.publicipaddresscombo.png)

- Als de gebruiker 'None' voor openbare IP-adres selecteert, wordt het tekstvak domain name label verborgen.
- Als de gebruiker selecteert een bestaand openbaar IP-adres, is het tekstvak label domein uitgeschakeld. De waarde is het domeinnaamlabel van het geselecteerde IP-adres.
- De domain name achtervoegsel (bijvoorbeeld westus.cloudapp.azure.com)-updates automatisch op basis van de geselecteerde locatie.

## <a name="schema"></a>Schema
```json
{
  "name": "element1",
  "type": "Microsoft.Network.PublicIpAddressCombo",
  "label": {
    "publicIpAddress": "Public IP address",
    "domainNameLabel": "Domain name label"
  },
  "toolTip": {
    "publicIpAddress": "",
    "domainNameLabel": ""
  },
  "defaultValue": {
    "publicIpAddressName": "ip01",
    "domainNameLabel": "mydomain"
  },
  "constraints": {
    "required": {
      "domainNameLabel": true
    }
  },
  "options": {
    "hideNone": false,
    "hideDomainNameLabel": false,
    "hideExisting": false
  },
  "visible": true
}
```

## <a name="remarks"></a>Opmerkingen
- Als `constraints.required.domainNameLabel` is ingesteld op **true**, de gebruiker moet een domeinnaamlabel opgeven bij het maken van een nieuw openbaar IP-adres. Bestaande openbare IP-adressen zonder een label zijn niet beschikbaar voor selectie.
- Als `options.hideNone` is ingesteld op **true**, klikt u vervolgens de optie te selecteren **geen** voor het openbare IP-adres wordt verborgen. De standaardwaarde is **false**.
- Als `options.hideDomainNameLabel` is ingesteld op **true**, en vervolgens in het tekstvak voor domeinnaamlabel wordt verborgen. De standaardwaarde is **false**.
- Als `options.hideExisting` is ingesteld op true, wordt de gebruiker is geen kunnen kiezen een bestaand openbaar IP-adres. De standaardwaarde is **false**.

## <a name="sample-output"></a>Voorbeelduitvoer
Als de gebruiker heeft geen openbare IP-adres geselecteerd, worden de volgende uitvoer wordt verwacht.
```json
{
  "newOrExistingOrNone": "none"
}
```

Als de gebruiker een nieuwe of bestaande IP-adres selecteert, wordt de volgende uitvoer verwacht:
```json
{
  "name": "ip01",
  "resourceGroup": "rg01",
  "domainNameLabel": "mydomain",
  "publicIPAllocationMethod": "Dynamic",
  "newOrExistingOrNone": "new"
}
```
- Wanneer `options.hideNone` is opgegeven als **true**, `newOrExistingOrNone` heeft alleen een waarde van **nieuwe** of **bestaande**.
- Wanneer `options.hideDomainNameLabel` is opgegeven als **true**, `domainNameLabel` is niet gedeclareerd.

## <a name="next-steps"></a>Volgende stappen
* Zie voor een inleiding tot het maken van de definities van de gebruikersinterface, [aan de slag met CreateUiDefinition](create-uidefinition-overview.md).
* Zie voor een beschrijving van de algemene eigenschappen in de UI-elementen, [CreateUiDefinition elementen](create-uidefinition-elements.md).
