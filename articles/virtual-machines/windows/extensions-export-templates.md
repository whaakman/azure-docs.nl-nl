---
title: Azure-resourcegroepen die VM-extensies bevatten exporteren | Microsoft Docs
description: Resource Manager-sjablonen met uitbreidingen van de virtuele machine exporteren.
services: virtual-machines-windows
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 7f4e2ca6-f1c7-4f59-a2cc-8f63132de279
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 12/05/2016
ms.author: nepeters
ms.openlocfilehash: cc3c705f1c9123de75ced016a5b39eb1a86b0f73
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="exporting-resource-groups-that-contain-vm-extensions"></a>Exporteren van resourcegroepen met VM-extensies

Azure-resourcegroepen kunnen worden geëxporteerd naar een nieuwe Resource Manager-sjabloon die vervolgens kan worden geïmplementeerd. Het exportproces interpreteert bestaande resources en Resource Manager-sjabloon maakt die bij de implementatie resulteert in een vergelijkbare resourcegroep. Wanneer u de optie voor het exporteren van resourcegroep tegen een bronnengroep met uitbreidingen van de virtuele Machine, worden meerdere items moeten worden overwogen zoals extensie compatibiliteit en instellingen beveiligd.

De Documentdetails van dit de werking van het exportproces resourcegroep met betrekking tot de virtuele machine extensies, met inbegrip van een lijst met extensies ondersteund en informatie over de verwerking van beveiligde gegevens.

## <a name="supported-virtual-machine-extensions"></a>Ondersteunde virtuele machines-extensies

Er zijn veel uitbreidingen van de virtuele Machine beschikbaar. Niet alle uitbreidingen kunnen worden geëxporteerd naar een Resource Manager-sjabloon met de functie 'Automatiseringsscript'. Als de extensie van een virtuele machine niet wordt ondersteund, moet deze handmatig terug in de geëxporteerde sjabloon worden geplaatst.

De volgende extensies kunnen met de functie voor automatisering script worden geëxporteerd.

| Toestelnummer ||||
|---|---|---|---|
| Acronis back-up | Datadog Windows-Agent | Patches voor Linux OS | VM-momentopname Linux
| Back-up Acronis Linux | Docker-uitbreiding | Puppet-Agent |
| BG Info | DSC-extensie | Site 24 x 7 Apm inzicht |
| BMC CTM Agent Linux | Dynatrace Linux | 24 x 7 Linux siteserver |
| BMC CTM Agent Windows | Dynatrace Windows | 24 x 7 Windows siteserver |
| Chef-Client | HPE beveiliging toepassing Defender | Trend Micro DSA |
| Aangepast Script | IaaS Antimalware | Trend Micro DSA Linux |
| Aangepaste scriptextensie | IaaS diagnostische gegevens | Toegang voor Linux VM |
| Aangepast Script voor Linux | Linux Chef-Client | Toegang voor Linux VM |
| Datadog Linux-Agent | Diagnose van Linux | VM-momentopname |

## <a name="export-the-resource-group"></a>De resourcegroep exporteren

Als u wilt een resourcegroep exporteren naar een herbruikbare sjabloon, moet u de volgende stappen uitvoeren:

1. Aanmelden bij Azure Portal
2. Klik in het Menu Hub op resourcegroepen
3. Selecteer de doelresourcegroep in de lijst
4. Klik op de blade resourcegroep automatiseringsscript

![Sjabloon exporteren](./media/extensions-export-templates/template-export.png)

Het Azure Resource Manager-script voor automatische produceert een Resource Manager-sjabloon, een parameterbestand en implementatie van verschillende voorbeeldscripts zoals PowerShell en Azure CLI. De geëxporteerde sjabloon kan op dit moment worden gedownload met behulp van de downloadknop als een nieuwe sjabloon wordt toegevoegd aan de sjabloonbibliotheek of geïmplementeerd met behulp van de knop implementeren.

## <a name="configure-protected-settings"></a>Beveiligde instellingen configureren

Veel virtuele machine van Azure-extensies bevatten een beveiligde instellingen configuratie, waarbij gevoelige gegevens zoals referenties en configuratie tekenreeksen worden gecodeerd. Beveiligde instellingen worden niet geëxporteerd met een automatiseringsscript. Indien nodig, beveiligde instellingen moeten opnieuw worden ingevoegd in de geëxporteerde sjablonen.

### <a name="step-1---remove-template-parameter"></a>Stap 1 - sjabloonparameter verwijderen

Als de resourcegroep wordt geëxporteerd, wordt een enkele sjabloonparameter gemaakt voor een waarde opgeven voor de geëxporteerde beveiligde instellingen. Deze parameter kan worden verwijderd. Verwijder de parameter, bekijk de parameterlijst en verwijderen van de parameter dat op dit JSON-voorbeeld lijkt.

```json
"extensions_extensionname_protectedSettings": {
    "defaultValue": null,
    "type": "SecureObject"
}
```

### <a name="step-2---get-protected-settings-properties"></a>Stap 2: Get beveiligde eigenschappen

Omdat elk beveiligd instellen van een reeks vereiste eigenschappen heeft, moet een lijst van deze eigenschappen worden verzameld. Alle parameters van de configuratie van de beveiligde vindt u in de [Azure Resource Manager-schema op GitHub](https://raw.githubusercontent.com/Azure/azure-resource-manager-schemas/master/schemas/2015-08-01/Microsoft.Compute.json). Dit schema bevat alleen de parametersets voor de uitbreidingen die worden vermeld in de sectie overzicht van dit document. 

Uit binnen de schema-opslagplaats, zoekt u de gewenste extensie voor dit voorbeeld `IaaSDiagnostics`. Eenmaal de extensies `protectedSettings` object is gevonden, dient u elke parameter. In het voorbeeld van de `IaasDiagnostic` uitbreiding, de parameters zijn vereist `storageAccountName`, `storageAccountKey`, en `storageAccountEndPoint`.

```json
"protectedSettings": {
    "type": "object",
    "properties": {
        "storageAccountName": {
            "type": "string"
        },
        "storageAccountKey": {
            "type": "string"
        },
        "storageAccountEndPoint": {
            "type": "string"
        }
    },
    "required": [
        "storageAccountName",
        "storageAccountKey",
        "storageAccountEndPoint"
    ]
}
```

### <a name="step-3---re-create-the-protected-configuration"></a>Stap 3: de configuratie van de beveiligde opnieuw maken

Op de geëxporteerde sjabloon en zoek naar `protectedSettings` en het geëxporteerde beveiligde instellingsobject vervangen door een nieuwe met de vereiste extensie-parameters en een waarde voor elk criterium.

In het voorbeeld van de `IaasDiagnostic` uitbreiding, de nieuwe configuratie van de instelling voor beveiligde eruit als in het volgende voorbeeld:

```json
"protectedSettings": {
    "storageAccountName": "[parameters('storageAccountName')]",
    "storageAccountKey": "[parameters('storageAccountKey')]",
    "storageAccountEndPoint": "https://core.windows.net"
}
```

De resource eindextensie lijkt op het volgende JSON-voorbeeld:

```json
{
    "name": "Microsoft.Insights.VMDiagnosticsSettings",
    "type": "extensions",
    "location": "[resourceGroup().location]",
    "apiVersion": "[variables('apiVersion')]",
    "dependsOn": [
        "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'))]"
    ],
    "tags": {
        "displayName": "AzureDiagnostics"
    },
    "properties": {
        "publisher": "Microsoft.Azure.Diagnostics",
        "type": "IaaSDiagnostics",
        "typeHandlerVersion": "1.5",
        "autoUpgradeMinorVersion": true,
        "settings": {
            "xmlCfg": "[base64(concat(variables('wadcfgxstart'), variables('wadmetricsresourceid'), variables('vmName'), variables('wadcfgxend')))]",
            "storageAccount": "[parameters('existingdiagnosticsStorageAccountName')]"
        },
        "protectedSettings": {
            "storageAccountName": "[parameters('storageAccountName')]",
            "storageAccountKey": "[parameters('storageAccountKey')]",
            "storageAccountEndPoint": "https://core.windows.net"
        }
    }
}
```

Als u Sjabloonparameters eigenschapswaarden opgeven, moeten deze worden gemaakt. Bij het maken van Sjabloonparameters voor beveiligd waarden in te stellen, moet u de `SecureString` parametertype zodat gevoelige waarden zijn beveiligd. Zie voor meer informatie over het gebruik van parameters [Azure Resource Manager-sjablonen samenstellen](../../resource-group-authoring-templates.md).

In het voorbeeld van de `IaasDiagnostic` uitbreiding mag de volgende parameters moeten worden gemaakt in de sectie parameters van de Resource Manager-sjabloon.

```json
"storageAccountName": {
    "defaultValue": null,
    "type": "SecureString"
},
"storageAccountKey": {
    "defaultValue": null,
    "type": "SecureString"
}
```

De sjabloon kan op dit moment worden geïmplementeerd met behulp van de implementatiemethode van een sjabloon.
