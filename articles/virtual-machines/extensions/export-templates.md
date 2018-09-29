---
title: Azure-resourcegroepen met VM-extensies exporteren | Microsoft Docs
description: Exporteren van Resource Manager-sjablonen die extensies voor virtuele machines bevatten.
services: virtual-machines-windows
documentationcenter: ''
author: roiyz-msft
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 7f4e2ca6-f1c7-4f59-a2cc-8f63132de279
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 12/05/2016
ms.author: roiyz
ms.openlocfilehash: f56cfeeede393dbdb9632ea4120d3a81e89f3f7c
ms.sourcegitcommit: f31bfb398430ed7d66a85c7ca1f1cc9943656678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/28/2018
ms.locfileid: "47451966"
---
# <a name="exporting-resource-groups-that-contain-vm-extensions"></a>Resourcegroepen met VM-extensies exporteren

Azure-resourcegroepen kunnen worden geëxporteerd naar een nieuwe Resource Manager-sjabloon die vervolgens kan worden geïmplementeerd. Het exportproces interpreteert bestaande resources en Resource Manager-sjabloon maakt die bij de implementatie resulteert in een vergelijkbaar resourcegroep. Wanneer u de optie voor het exporteren van resourcegroep op basis van een resourcegroep met de extensies voor virtuele machines, kunnen meerdere items moeten worden overwogen, zoals compatibiliteit van de extensie en beveiligde instellingen.

Deze document staat informatie over de werking van het exportproces resourcegroep met betrekking tot de VM-extensies, waaronder een lijst van ondersteunde extensies en meer informatie over de verwerking van beveiligde gegevens.

## <a name="supported-virtual-machine-extensions"></a>Ondersteunde VM-extensies

Veel extensies voor virtuele machines zijn beschikbaar. Niet alle uitbreidingen kunnen worden geëxporteerd naar een Resource Manager-sjabloon met behulp van de functie 'Automatiseringsscript'. Als een VM-extensie wordt niet ondersteund, moet deze handmatig terug in de geëxporteerde sjabloon worden geplaatst.

De volgende extensies kunnen worden geëxporteerd met de functie voor automation-script.

| Toestelnummer ||||
|---|---|---|---|
| Acronis back-up | Datadog Windows-Agent | OS-patches voor Linux | VM-momentopname Linux
| Back-up Acronis Linux | Docker-extensie | Puppet Agent |
| BG-Info | DSC-extensie | Site 24 x 7 Apm inzicht |
| BMC-CMT Agent Linux | Dynatrace-Linux | 24 x 7 Linux siteserver |
| BMC-CMT Agent-Windows | Dynatrace Windows | 24 x 7 Windows siteserver |
| Chef-Client | HPE Security toepassing Defender | Trend Micro DSA |
| Aangepast script | IaaS-Antimalware | Trend Micro DSA Linux |
| Aangepaste scriptextensie | Diagnostische gegevens over IaaS | Voor Linux-VM-toegang |
| Aangepast Script voor Linux | Chef-Client voor Linux | Voor Linux-VM-toegang |
| Datadog Linux-Agent | De diagnostische Linux | VM-momentopname |

## <a name="export-the-resource-group"></a>De resourcegroep exporteren

Als u wilt een resourcegroep in een herbruikbare sjabloon exporteren, voert u de volgende stappen uit:

1. Aanmelden bij Azure Portal
2. Klik in het Menu Hub op resourcegroepen
3. Selecteer de doelresourcegroep in de lijst
4. Klik in de resourcegroep-blade op automatiseringsscript

![Sjabloon exporteren](./media/export-templates/template-export.png)

Het Azure Resource Manager-script voor automatische produceert een Resource Manager-sjabloon, een parameterbestand en enkele voorbeelden van implementatiescripts, zoals PowerShell en Azure CLI. De geëxporteerde sjabloon kan op dit moment worden gedownload met behulp van de downloadknop als een nieuwe sjabloon toegevoegd aan de bibliotheek voor sjablonen of gedistribueerd met behulp van de knop implementeren.

## <a name="configure-protected-settings"></a>Beveiligde instellingen configureren

Veel extensies voor virtuele machine van Azure bevatten een configuratie met beveiligde instellingen die gevoelige gegevens zoals referenties en van configuratietekenreeksen worden gecodeerd. Beveiligde instellingen worden niet geëxporteerd met het automatiseringsscript. Indien nodig, beveiligde instellingen moeten opnieuw worden geplaatst in de geëxporteerde sjablonen.

### <a name="step-1---remove-template-parameter"></a>Stap 1: sjabloonparameter verwijderen

Tijdens het exporteren van de resourcegroep, wordt een enkele sjabloonparameter gemaakt om u te bieden een waarde voor de geëxporteerde beveiligde instellingen. Deze parameter kan worden verwijderd. Verwijder de parameter, bekijk de lijst met parameters en verwijderen van de parameter die op dit JSON-voorbeeld lijkt.

```json
"extensions_extensionname_protectedSettings": {
    "defaultValue": null,
    "type": "SecureObject"
}
```

### <a name="step-2---get-protected-settings-properties"></a>Stap 2: Get beveiligde eigenschappen

Omdat elk beveiligd instellen van een reeks vereiste eigenschappen heeft, moet een lijst van deze eigenschappen worden verzameld. Alle parameters van de configuratie van de beveiligde instellingen kunt u vinden in de [Azure Resource Manager-schema op GitHub](https://raw.githubusercontent.com/Azure/azure-resource-manager-schemas/master/schemas/2015-08-01/Microsoft.Compute.json). Dit schema bevat alleen de parametersets voor de uitbreidingen die worden vermeld in de sectie overzicht van dit document. 

Uit binnen de opslagplaats schema zoeken naar de gewenste extensie, in dit voorbeeld `IaaSDiagnostics`. Zodra de extensies `protectedSettings` object is gevonden, Let op elke parameter. In het voorbeeld van de `IaasDiagnostic` extensie, de parameters zijn vereist `storageAccountName`, `storageAccountKey`, en `storageAccountEndPoint`.

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

### <a name="step-3---re-create-the-protected-configuration"></a>Stap 3: de configuratie van de beveiligde opnieuw te maken

Zoek op de geëxporteerde sjabloon, `protectedSettings` en het instellingsobject geëxporteerde beveiligde vervangen door een nieuw exemplaar met de vereiste extensieparameters en een waarde voor elk criterium.

In het voorbeeld van de `IaasDiagnostic` extensie, de nieuwe voor beveiligde instellingsconfiguratie eruit als in het volgende voorbeeld:

```json
"protectedSettings": {
    "storageAccountName": "[parameters('storageAccountName')]",
    "storageAccountKey": "[parameters('storageAccountKey')]",
    "storageAccountEndPoint": "https://core.windows.net"
}
```

De laatste extensie resource lijkt op het volgende JSON-voorbeeld:

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

Als u Sjabloonparameters eigenschapswaarden op te geven, moeten deze worden gemaakt. Bij het maken van Sjabloonparameters voor beveiligd waarden in te stellen, zorg ervoor dat u de `SecureString` parametertype zodat gevoelige waarden zijn beveiligd. Zie voor meer informatie over het gebruik van parameters [Authoring Azure Resource Manager-sjablonen](../../resource-group-authoring-templates.md).

In het voorbeeld van de `IaasDiagnostic` extensie, de volgende parameters zouden worden gemaakt in de parametersectie van de Resource Manager-sjabloon.

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
