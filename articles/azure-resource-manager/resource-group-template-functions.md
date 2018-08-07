---
title: Resource Manager-sjabloonfuncties | Microsoft Docs
description: Beschrijft de functies te gebruiken in een Azure Resource Manager-sjabloon voor het ophalen van waarden, werken met tekenreeksen en numerieke waarden en ophalen van informatie over de implementatie.
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 0644abe1-abaa-443d-820d-1966d7d26bfd
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/03/2018
ms.author: tomfitz
ms.openlocfilehash: a4a86576b8f9f842c54cfa195305a3e0d0ff4724
ms.sourcegitcommit: 9819e9782be4a943534829d5b77cf60dea4290a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/06/2018
ms.locfileid: "39527616"
---
# <a name="azure-resource-manager-template-functions"></a>Functies van Azure Resource Manager-sjablonen
Dit artikel beschrijft alle functies die u in een Azure Resource Manager-sjabloon gebruiken kunt.

U functies in uw sjablonen toevoegen door deze tekst tussen vierkante haken: `[` en `]`, respectievelijk. De expressie wordt geëvalueerd tijdens de implementatie. Terwijl geschreven als een letterlijke tekenreeks, kan het resultaat van evaluatie van de expressie van een ander JSON-type, zoals een matrix, een object of een geheel getal zijn. Net zoals in JavaScript-functieaanroepen die zijn opgemaakt als `functionName(arg1,arg2,arg3)`. U verwijzen naar eigenschappen met behulp van de operators stip en [index].

Een sjabloonexpressie voor een niet langer zijn dan 24.576 tekens.

Sjabloonfuncties en de bijbehorende parameters zijn niet hoofdlettergevoelig. Bijvoorbeeld: Resource Manager wordt omgezet **variables('var1')** en **VARIABLES('VAR1')** als hetzelfde. Wanneer geëvalueerd, tenzij de functie wijzigt uitdrukkelijk geval (zoals toUpper of toLower), de functie blijft behouden voor het geval is. Bepaalde resourcetypen mogelijk case-vereisten, ongeacht hoe de functies worden geëvalueerd.

Zie voor het maken van uw eigen functies [door gebruiker gedefinieerde functies](resource-group-authoring-templates.md#functions).

<a id="array" />
<a id="coalesce" />
<a id="concatarray" />
<a id="contains" />
<a id="createarray" />
<a id="empty" />
<a id="first" />
<a id="intersection" />
<a id="json" />
<a id="last" />
<a id="length" />
<a id="min" />
<a id="max" />
<a id="range" />
<a id="skip" />
<a id="take" />
<a id="union" />

## <a name="array-and-object-functions"></a>Matrix- en objectfuncties
Resource Manager biedt verschillende functies voor het werken met matrices en objecten.

* [Matrix](resource-group-template-functions-array.md#array)
* [samenvoegen](resource-group-template-functions-array.md#coalesce)
* [concat](resource-group-template-functions-array.md#concat)
* [bevat](resource-group-template-functions-array.md#contains)
* [createArray](resource-group-template-functions-array.md#createarray)
* [leeg zijn](resource-group-template-functions-array.md#empty)
* [eerste](resource-group-template-functions-array.md#first)
* [snijpunt](resource-group-template-functions-array.md#intersection)
* [json](resource-group-template-functions-array.md#json)
* [laatste](resource-group-template-functions-array.md#last)
* [Lengte](resource-group-template-functions-array.md#length)
* [min](resource-group-template-functions-array.md#min)
* [max](resource-group-template-functions-array.md#max)
* [Bereik](resource-group-template-functions-array.md#range)
* [skip](resource-group-template-functions-array.md#skip)
* [toets maken](resource-group-template-functions-array.md#take)
* [Union](resource-group-template-functions-array.md#union)

<a id="equals" />
<a id="less" />
<a id="lessorequals" />
<a id="greater" />
<a id="greaterorequals" />

## <a name="comparison-functions"></a>Vergelijkingsfuncties
Resource Manager biedt verschillende functies voor het maken van vergelijkingen in uw sjablonen.

* [is gelijk aan](resource-group-template-functions-comparison.md#equals)
* [minder](resource-group-template-functions-comparison.md#less)
* [lessOrEquals](resource-group-template-functions-comparison.md#lessorequals)
* [meer](resource-group-template-functions-comparison.md#greater)
* [greaterOrEquals](resource-group-template-functions-comparison.md#greaterorequals)

<a id="deployment" />
<a id="parameters" />
<a id="variables" />

## <a name="deployment-value-functions"></a>Implementatiefuncties waarde
Resource Manager biedt de volgende functies voor het ophalen van waarden uit de secties van de sjabloon en de waarden die betrekking hebben op de implementatie:

* [Implementatie](resource-group-template-functions-deployment.md#deployment)
* [parameters](resource-group-template-functions-deployment.md#parameters)
* [Variabelen](resource-group-template-functions-deployment.md#variables)

<a id="and" />
<a id="bool" />
<a id="if" />
<a id="not" />
<a id="or" />

## <a name="logical-functions"></a>Logische functies
Resource Manager biedt de volgende functies voor het werken met logische voorwaarden:

* [en](resource-group-template-functions-logical.md#and)
* [bool](resource-group-template-functions-logical.md#bool)
* [if](resource-group-template-functions-logical.md#if)
* [niet](resource-group-template-functions-logical.md#not)
* [or](resource-group-template-functions-logical.md#or)

<a id="add" />
<a id="copyindex" />
<a id="div" />
<a id="float" />
<a id="int" />
<a id="minint" />
<a id="maxint" />
<a id="mod" />
<a id="mul" />
<a id="sub" />

## <a name="numeric-functions"></a>Numerieke functies
Resource Manager biedt de volgende functies voor het werken met gehele getallen:

* [add](resource-group-template-functions-numeric.md#add)
* [copyIndex](resource-group-template-functions-numeric.md#copyindex)
* [div](resource-group-template-functions-numeric.md#div)
* [drijvende komma](resource-group-template-functions-numeric.md#float)
* [int](resource-group-template-functions-numeric.md#int)
* [min](resource-group-template-functions-numeric.md#min)
* [max](resource-group-template-functions-numeric.md#max)
* [Mod](resource-group-template-functions-numeric.md#mod)
* [mul](resource-group-template-functions-numeric.md#mul)
* [sub](resource-group-template-functions-numeric.md#sub)

<a id="listkeys" />
<a id="list" />
<a id="providers" />
<a id="reference" />
<a id="resourcegroup" />
<a id="resourceid" />
<a id="subscription" />

## <a name="resource-functions"></a>Resourcefuncties
Resource Manager biedt de volgende functies voor het ophalen van waarden van resources:

* [listAccountSas](resource-group-template-functions-resource.md#list)
* [listKeys](resource-group-template-functions-resource.md#listkeys)
* [listSecrets](resource-group-template-functions-resource.md#list)
* [lijst met *](resource-group-template-functions-resource.md#list)
* [Providers](resource-group-template-functions-resource.md#providers)
* [Verwijzing](resource-group-template-functions-resource.md#reference)
* [resourceGroup](resource-group-template-functions-resource.md#resourcegroup)
* [ResourceId](resource-group-template-functions-resource.md#resourceid)
* [abonnement](resource-group-template-functions-resource.md#subscription)

<a id="base64" />
<a id="base64tojson" />
<a id="base64tostring" />
<a id="concat" />
<a id="containsstring" />
<a id="datauri" />
<a id="datauritostring" />
<a id="emptystring" />
<a id="endswith" />
<a id="firststring" />
<a id="guid" />
<a id="indexof" />
<a id="laststring" />
<a id="lastindexof" />
<a id="lengthstring" />
<a id="padleft" />
<a id="replace" />
<a id="skipstring" />
<a id="split" />
<a id="startswith" />
<a id="string" />
<a id="substring" />
<a id="takestring" />
<a id="tolower" />
<a id="toupper" />
<a id="trim" />
<a id="uniquestring" />
<a id="uri" />
<a id="uricomponent" />
<a id="uricomponenttostring" />

## <a name="string-functions"></a>Tekenreeksfuncties
Resource Manager biedt de volgende functies voor het werken met tekenreeksen:

* [base64](resource-group-template-functions-string.md#base64)
* [base64ToJson](resource-group-template-functions-string.md#base64tojson)
* [base64ToString](resource-group-template-functions-string.md#base64tostring)
* [concat](resource-group-template-functions-string.md#concat)
* [bevat](resource-group-template-functions-string.md#contains)
* [dataUri](resource-group-template-functions-string.md#datauri)
* [dataUriToString](resource-group-template-functions-string.md#datauritostring)
* [leeg zijn](resource-group-template-functions-string.md#empty)
* [endsWith](resource-group-template-functions-string.md#endswith)
* [eerste](resource-group-template-functions-string.md#first)
* [GUID](resource-group-template-functions-string.md#guid)
* [indexOf](resource-group-template-functions-string.md#indexof)
* [laatste](resource-group-template-functions-string.md#last)
* [lastIndexOf](resource-group-template-functions-string.md#lastindexof)
* [Lengte](resource-group-template-functions-string.md#length)
* [padLeft](resource-group-template-functions-string.md#padleft)
* [vervangen](resource-group-template-functions-string.md#replace)
* [skip](resource-group-template-functions-string.md#skip)
* [split](resource-group-template-functions-string.md#split)
* [startsWith](resource-group-template-functions-string.md#startswith)
* [Tekenreeks](resource-group-template-functions-string.md#string)
* [de subtekenreeks](resource-group-template-functions-string.md#substring)
* [toets maken](resource-group-template-functions-string.md#take)
* [toLower](resource-group-template-functions-string.md#tolower)
* [toUpper](resource-group-template-functions-string.md#toupper)
* [trim](resource-group-template-functions-string.md#trim)
* [uniqueString](resource-group-template-functions-string.md#uniquestring)
* [URI](resource-group-template-functions-string.md#uri)
* [uriComponent](resource-group-template-functions-string.md#uricomponent)
* [uriComponentToString](resource-group-template-functions-string.md#uricomponenttostring)

## <a name="next-steps"></a>Volgende stappen
* Zie voor een beschrijving van de secties in een Azure Resource Manager-sjabloon, [Authoring Azure Resource Manager-sjablonen](resource-group-authoring-templates.md)
* U kunt meerdere sjablonen samenvoegen, Zie [gekoppelde sjablonen gebruiken met Azure Resource Manager](resource-group-linked-templates.md)
* Op een opgegeven aantal keren herhalen bij het maken van een type resource, Zie [meerdere exemplaren van resources maken in Azure Resource Manager](resource-group-create-multiple.md)
* Zie voor meer informatie over het implementeren van de sjabloon die u hebt gemaakt, [een toepassing implementeren met Azure Resource Manager-sjabloon](resource-group-template-deploy.md)
