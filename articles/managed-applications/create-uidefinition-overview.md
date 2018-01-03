---
title: Begrijpen gebruikersinterface maken definitie voor Azure beheerde toepassingen | Microsoft Docs
description: Hierin wordt beschreven hoe u de definities van de gebruikersinterface voor beheerde Azure-toepassingen maken
services: managed-applications
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.service: managed-applications
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/15/2017
ms.author: tomfitz
ms.openlocfilehash: bdbde834695040df4e333bef42fab7d29614ab75
ms.sourcegitcommit: 68aec76e471d677fd9a6333dc60ed098d1072cfc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2017
---
# <a name="create-azure-portal-user-interface-for-your-managed-application"></a>Azure portal-gebruikersinterface voor de beheerde toepassing maken
Dit document worden de belangrijkste concepten van het bestand createUiDefinition.json. Dit bestand wordt gebruikt voor het genereren van de gebruikersinterface voor het maken van een beheerde toepassing van de Azure-portal.

```json
{
   "$schema": "https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json",
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

* handler
* versie
* parameters

Voor beheerde toepassingen handler altijd moet `Microsoft.Compute.MultiVm`, en de laatste ondersteunde versie is `0.1.2-preview`.

Het schema van de eigenschap parameters is afhankelijk van de combinatie van de opgegeven handler en versie. Voor beheerde toepassingen, de ondersteunde eigenschappen zijn `basics`, `steps`, en `outputs`. De eigenschappen van de basisprincipes en stappen bevatten de _elementen_ - zoals tekstvakken en vervolgkeuzelijsten - moet worden weergegeven in de Azure portal. De uitvoer-eigenschap wordt gebruikt om de uitvoerwaarden van de opgegeven elementen toewijzen aan de parameters van de Azure Resource Manager-implementatiesjabloon.

Inclusief `$schema` wordt aanbevolen, maar is optioneel. Als u opgeeft, wordt de waarde voor `version` moet overeenkomen met de versie in de `$schema` URI.

## <a name="basics"></a>Basisbeginselen
De grondbeginselen van stap is altijd de eerste stap van de wizard gegenereerd wanneer het bestand wordt geparseerd in de Azure-portal. Naast het weergeven van de elementen die zijn opgegeven in `basics`, de portal injects-elementen voor gebruikers om te kiezen voor het abonnement, resourcegroep en locatie voor de implementatie. In het algemeen moeten elementen waarmee een query voor distributie-parameters, zoals de naam van een cluster of de administrator-referenties uitvoeren, gaat u in deze stap.

Als het gedrag van een element, is afhankelijk van abonnement, resourcegroep of locatie van de gebruiker, kan niet in basisbeginselen van dat element gebruikt. Bijvoorbeeld: **Microsoft.Compute.SizeSelector** is afhankelijk van het abonnement en de locatie voor het bepalen van de lijst met beschikbare grootten van de gebruiker. Daarom **Microsoft.Compute.SizeSelector** kan alleen worden gebruikt in stappen. In het algemeen alleen elementen in de **Microsoft.Common** naamruimte in basisbeginselen kan worden gebruikt. Hoewel bepaalde elementen in andere naamruimten (zoals **Microsoft.Compute.Credentials**) dat niet afhankelijk zijn van de context van de gebruiker, nog steeds zijn toegestaan.

## <a name="steps"></a>Stappen
De eigenschap stappen kan nul of meer extra stappen om weer te geven nadat de basisbeginselen, die elk een of meer elementen bevat bevatten. Overweeg stappen per rol of laag van de toepassing wordt geïmplementeerd. Voeg bijvoorbeeld een stap voor de invoer voor de hoofdknooppunten, en een voor worker-knooppunten in een cluster.

## <a name="outputs"></a>Uitvoer
De Azure portal maakt gebruik van de `outputs` eigenschap toewijzen van elementen uit `basics` en `steps` aan de parameters van de Azure Resource Manager-implementatiesjabloon. De sleutels van deze woordenlijst zijn de namen van de sjabloonparameters en de waarden worden de eigenschappen van de uitvoer-objecten van de elementen waarnaar wordt verwezen.

De naam van de beheerde toepassing resource stelt u een waarde met de naam moet opnemen `applicationResourceName` in de uitvoer-eigenschap. Als u deze waarde niet instelt, wordt een GUID voor de naam van de toegewezen door de toepassing. U kunt een tekstvak opnemen in de gebruikersinterface die aanvragen van een naam op van de gebruiker.

```json
"outputs": {
    "vmName": "[steps('appSettings').vmName]",
    "trialOrProduction": "[steps('appSettings').trialOrProd]",
    "userName": "[steps('vmCredentials').adminUsername]",
    "pwd": "[steps('vmCredentials').vmPwd.password]",
    "applicationResourceName": "[steps('appSettings').vmName]"
}
```

## <a name="functions"></a>Functies
Net als bij sjabloonfuncties in Azure Resource Manager (zowel in de syntaxis en functionaliteit), CreateUiDefinition biedt functies voor het werken met elementen invoer en uitvoer, evenals functies zoals voorwaardelijke.

## <a name="next-steps"></a>Volgende stappen
Het bestand createUiDefinition.json zelf is een eenvoudig schema. De echte diepte van het afkomstig van de ondersteunde elementen en functies. Deze items worden beschreven in meer detail op:

- [Elementen](create-uidefinition-elements.md)
- [Functies](create-uidefinition-functions.md)

Een huidige JSON-schema voor createUiDefinition is hier beschikbaar: https://schema.management.azure.com/schemas/0.1.2-preview/CreateUIDefinition.MultiVm.json.

Zie voor een voorbeeld van de interface gebruikersbestand [createUiDefinition.json](https://github.com/Azure/azure-managedapp-samples/blob/master/samples/201-managed-app-using-existing-vnet/createUiDefinition.json).