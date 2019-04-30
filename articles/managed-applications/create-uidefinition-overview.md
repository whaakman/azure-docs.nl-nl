---
title: Informatie over het maken van de gebruikersinterface van de definitie voor Azure beheerde toepassingen | Microsoft Docs
description: Beschrijft het maken van definities van de gebruikersinterface voor Azure Managed Applications
services: managed-applications
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: managed-applications
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/15/2017
ms.author: tomfitz
ms.openlocfilehash: ab777b487159b009bf2cac6086bb09cc71714b0d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60587747"
---
# <a name="create-azure-portal-user-interface-for-your-managed-application"></a>Azure portal-gebruikersinterface voor uw beheerde toepassing maken
Dit document beschrijft de belangrijkste concepten van het bestand createUiDefinition.json. Dit bestand in de Azure-portal wordt gebruikt voor het genereren van de gebruikersinterface voor het maken van een beheerde toepassing.

```json
{
   "$schema": "https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json#",
   "handler": "Microsoft.Compute.MultiVm",
   "version": "0.1.2-preview",
   "parameters": {
      "basics": [ ],
      "steps": [ ],
      "outputs": { }
   }
}
```

Een CreateUiDefinition bevat altijd drie eigenschappen: 

* Handler
* versie
* parameters

Voor beheerde toepassingen-handler altijd moet worden `Microsoft.Compute.MultiVm`, en de nieuwste ondersteunde versie is `0.1.2-preview`.

Het schema van de parameters-eigenschap is afhankelijk van de combinatie van de opgegeven handler en versie. Voor beheerde toepassingen, de ondersteunde eigenschappen zijn `basics`, `steps`, en `outputs`. De eigenschappen van de basisbeginselen en stappen bevatten de _elementen_ - zoals tekstvakken en vervolgkeuzelijsten - moet worden weergegeven in de Azure-portal. De uitvoer-eigenschap wordt gebruikt om de uitvoerwaarden van de opgegeven elementen worden toegewezen aan de parameters van de sjabloon van Azure Resource Manager-implementatie.

Inclusief `$schema` wordt aanbevolen, maar optionele. Als de waarde voor opgegeven `version` moet overeenkomen met de versie in de `$schema` URI.

## <a name="basics"></a>Basics
De grondbeginselen van stap is altijd de eerste stap van de wizard die wordt gegenereerd wanneer het bestand in de Azure-portal wordt geparseerd. Naast het weergeven van de elementen die zijn opgegeven in `basics`, de portal injects-elementen voor gebruikers een voor het abonnement, resourcegroep en locatie voor de implementatie. Elementen die op te voor distributie-parameters, zoals de naam van de referenties van een cluster of de beheerder vragen, moeten over het algemeen gaat u in deze stap.

Als het gedrag van een element afhankelijk is van de gebruiker-abonnement, resourcegroep of locatie, worden niet dat element gebruikt in de basisprincipes. Bijvoorbeeld, **Microsoft.Compute.SizeSelector** is afhankelijk van het abonnement en de locatie om te bepalen van de lijst met beschikbare grootten van de gebruiker. Daarom **Microsoft.Compute.SizeSelector** kan alleen worden gebruikt in stappen. Over het algemeen alleen elementen in de **Microsoft.Common** naamruimte kan worden gebruikt in de basisprincipes. Hoewel sommige elementen in andere naamruimten (zoals **Microsoft.Compute.Credentials**) die niet afhankelijk zijn van de context van de gebruiker, nog steeds zijn toegestaan.

## <a name="steps"></a>Stappen
De eigenschap stappen kan nul of meer extra stappen om weer te geven na de basisbeginselen, die elk een of meer elementen bevat bevatten. U kunt toevoegen van stappen per rol of laag van de toepassing wordt geïmplementeerd. Bijvoorbeeld, een stap voor de invoer voor de hoofdknooppunten en een stap voor de worker-knooppunten in een cluster toevoegen.

## <a name="outputs"></a>Uitvoer
De Azure portal maakt gebruik van de `outputs` eigenschap toe te wijzen van elementen uit `basics` en `steps` aan de parameters van de sjabloon van Azure Resource Manager-implementatie. De sleutels van de woordenlijst zijn de namen van de parameters van de sjabloon en de waarden zijn eigenschappen van de objecten van de uitvoer van de elementen waarnaar wordt verwezen.

Als u wilt de naam van de beheerde toepassing-resource instellen, moet u een waarde met de naam opnemen `applicationResourceName` in de uitvoer-eigenschap. Als deze waarde niet is ingesteld, wordt een GUID voor de naam van de toegewezen door de toepassing. U kunt een tekstvak opnemen in de gebruikersinterface die door een naam op van de gebruiker worden aangevraagd.

```json
"outputs": {
    "vmName": "[steps('appSettings').vmName]",
    "trialOrProduction": "[steps('appSettings').trialOrProd]",
    "userName": "[steps('vmCredentials').adminUsername]",
    "pwd": "[steps('vmCredentials').vmPwd.password]",
    "applicationResourceName": "[steps('appSettings').vmName]"
}
```

## <a name="functions"></a>Functions
Net als bij functies van sjablonen in Azure Resource Manager (zowel in de syntaxis en functionaliteit), CreateUiDefinition biedt functies voor het werken met elementen invoer en uitvoer, evenals functies zoals voorwaardelijke instructies.

## <a name="next-steps"></a>Volgende stappen
Het bestand createUiDefinition.json zelf is een eenvoudig schema. De echte diepte van deze zijn afkomstig van de ondersteunde elementen en functies. Deze items worden beschreven in meer detail op:

- [Elementen](create-uidefinition-elements.md)
- [Functies](create-uidefinition-functions.md)

Een huidige JSON-schema voor createUiDefinition is hier beschikbaar: https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json.

Zie voor een voorbeeld van de interface gebruikersbestand [createUiDefinition.json](https://github.com/Azure/azure-managedapp-samples/blob/master/samples/201-managed-app-using-existing-vnet/createUiDefinition.json).
